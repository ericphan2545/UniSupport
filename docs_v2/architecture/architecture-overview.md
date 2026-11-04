# Tổng quan kiến trúc

## Thành phần

```mermaid
flowchart TB
    B[Trình duyệt<br/>điện thoại / máy tính]
    subgraph SERVER[Server của trường – Docker Compose]
        GW[Reverse proxy<br/>HTTPS, giới hạn body 55 MiB]
        FE[Frontend<br/>3 cổng, VN/EN, responsive]
        API[Backend API]
        JOB[Tác vụ nền<br/>quá hạn mỗi 1 phút, dọn tệp tạm]
        DB[(CSDL quan hệ)]
        FS[(Kho tệp<br/>ngoài thư mục web)]
        CACHE[(Cache<br/>thông tin SSO, danh mục)]
    end
    SSO[[SSO của trường]]

    B -->|HTTPS| GW
    GW --> FE
    GW -->|/api| API
    API --> DB
    API --> FS
    API --> CACHE
    API -->|authorization code, Userinfo| SSO
    JOB --> DB
    JOB --> FS
```

| Thành phần | Trách nhiệm |
|---|---|
| Reverse proxy | Chỉ phục vụ HTTPS (HTTP chuyển hướng sang HTTPS); giới hạn tổng request multipart 55 MiB; chuyển `/api` vào backend |
| Frontend | Giao diện 3 cổng theo vai trò; đổi VN/EN; bố cục điện thoại và máy tính; kiểm tra sớm dữ liệu và tệp; polling thông báo 30 giây khi trang đang mở |
| Backend API | Phiên và phân quyền; nghiệp vụ hồ sơ; SLA; lịch sử và nhật ký; thông báo; báo cáo; nhận và trả tệp |
| Tác vụ nền | Chuyển hồ sơ quá hạn sang `OVERDUE` mỗi 1 phút, chỉ một tiến trình chạy tại một thời điểm; dọn tệp tạm không được tham chiếu |
| CSDL quan hệ | Dữ liệu nghiệp vụ, phiên, idempotency, lịch sử, nhật ký; cần transaction, sequence theo năm, unique constraint và phân quyền chỉ thêm/đọc trên bảng nhật ký |
| Kho tệp | Lưu tệp với tên ngẫu nhiên, không có đường dẫn công khai; chỉ trả qua API có kiểm tra quyền |
| Cache phía server | Thông tin tài khoản lấy từ SSO, 15 phút, riêng từng tài khoản (đặc tả module ghi Redis); danh mục loại yêu cầu và phòng ban |

## Cách backend xử lý một request ghi

Thứ tự kiểm tra cố định cho mọi API:

1. Phiên hợp lệ và chưa bị thu hồi.
2. Vai trò và trạng thái tài khoản (đọc từ CSDL ở mỗi request).
3. Phạm vi dữ liệu: ngoài phạm vi trả 404.
4. Quyền thao tác: trong phạm vi nhưng không có quyền ghi trả 403.
5. Kết quả idempotency đã lưu (nếu thao tác bắt buộc Idempotency-Key).
6. Version, trạng thái, kiểm tra nghiệp vụ.
7. Cập nhật nguyên tử: hồ sơ, lịch sử, nhật ký, thông báo, kết quả idempotency trong cùng một transaction.

## Luồng chính: sinh viên gửi yêu cầu kèm tệp

```mermaid
sequenceDiagram
    actor SV as Sinh viên
    participant FE as Frontend
    participant API as Backend API
    participant FS as Kho tệp
    participant DB as CSDL

    SV->>FE: Chọn loại yêu cầu, nhập mô tả, chọn tệp
    FE->>FE: Kiểm tra sớm (độ dài, số tệp, dung lượng, định dạng)
    FE->>API: POST tạo hồ sơ (multipart, Idempotency-Key)
    API->>API: Phiên → vai trò → idempotency → kiểm tra lại dữ liệu và magic bytes
    API->>FS: Lưu tệp vào vùng tạm
    API->>DB: BEGIN, lấy SLA hiện tại, sinh mã USP-YYYY-NNNNN, tạo hồ sơ NEW,<br/>lưu tham chiếu tệp, lịch sử, nhật ký, kết quả idempotency, COMMIT
    alt Commit thành công
        API-->>FE: 201 + mã hồ sơ
    else Rollback
        API->>FS: Xóa tệp đã chuẩn bị (lỗi thì để job dọn)
        API-->>FE: Lỗi, form và tệp được giữ để thử lại cùng key
    end
```

## Luồng chính: đăng nhập

```mermaid
sequenceDiagram
    actor U as Người dùng
    participant API as Backend API
    participant SSO as SSO trường
    participant DB as CSDL

    U->>API: Bấm "Đăng nhập bằng tài khoản trường"
    API-->>U: Chuyển hướng sang SSO kèm state (một lần, hết hạn 10 phút)
    U->>SSO: Đăng nhập
    SSO-->>API: Callback kèm mã xác thực và state
    API->>SSO: Đổi mã lấy token, xác minh, lấy định danh
    API->>DB: Tìm tài khoản theo email trường, chưa có thì tạo STUDENT, kiểm tra active
    API->>DB: Tạo phiên (8 giờ, không gia hạn), ghi LOGIN_SUCCESS
    API-->>U: Cookie phiên HttpOnly/Secure/SameSite=Lax, vào đúng cổng theo vai trò
```

## Vòng đời hồ sơ

```mermaid
stateDiagram-v2
    [*] --> NEW: Sinh viên gửi
    NEW --> IN_PROGRESS: Phân công lần đầu
    NEW --> OVERDUE: Vượt hạn
    IN_PROGRESS --> PENDING_INFO: Yêu cầu bổ sung
    IN_PROGRESS --> OVERDUE: Vượt hạn
    PENDING_INFO --> IN_PROGRESS: Sinh viên bổ sung
    IN_PROGRESS --> CLOSED: Đóng hồ sơ
    PENDING_INFO --> CLOSED: Đóng hồ sơ
    OVERDUE --> CLOSED: Đóng hồ sơ
    CLOSED --> [*]
```

`PENDING_INFO` tạm dừng SLA nên không chuyển sang `OVERDUE`. `OVERDUE` giữ nguyên khi được phân công, yêu cầu bổ sung hay nhận bổ sung; chỉ đổi khi đóng.

## Môi trường

| Môi trường | Dùng cho | SSO | Dữ liệu |
|---|---|---|---|
| Dev (máy lập trình viên) | Phát triển | Giả lập | Seed |
| Staging | Test trọn luồng từng milestone, nghiệm thu nội bộ | Giả lập | Seed, bộ 200 ticket mẫu |
| Production (server của trường) | Go-live | SSO thật; ứng dụng từ chối khởi động nếu bật giả lập | Danh mục thật do trường xác nhận; Admin đầu tiên nạp khi triển khai |

Đóng gói bằng Docker Compose, cấu hình qua `.env` (mẫu `.env.example` trong hồ sơ bàn giao). Bí mật (khóa SSO, khóa ký phiên, mật khẩu CSDL) không đưa vào repo.

## Công nghệ

Chưa chốt ngôn ngữ backend, framework frontend, hệ quản trị CSDL và kho tệp. Yêu cầu bắt buộc với lựa chọn:

- CSDL hỗ trợ transaction, sequence, unique constraint, khóa dòng và phân quyền theo bảng.
- Kho tệp nằm ngoài thư mục web và không phát URL công khai.
- Chạy được bằng Docker Compose trên hạ tầng của trường.

Prototype hiện tại (thư mục `prototype/`) chỉ là vỏ giao diện chạy luồng, không phải kiến trúc production.
