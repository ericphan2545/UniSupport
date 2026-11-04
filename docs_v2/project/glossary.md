# Thuật ngữ

## Nghiệp vụ

| Thuật ngữ | Nghĩa |
|---|---|
| Hồ sơ / ticket | Một yêu cầu hỗ trợ sinh viên gửi lên, có mã `USP-YYYY-NNNNN` |
| Loại yêu cầu | Phân loại hồ sơ; mỗi loại do đúng một phòng ban tiếp nhận và có SLA riêng |
| Phòng ban tiếp nhận | Phòng ban đang giữ và xử lý hồ sơ |
| Người phụ trách | Nhân viên được phân công xử lý hồ sơ; chỉ người này được ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ |
| SLA / hạn xử lý | Số giờ được phép để xử lý một loại yêu cầu / thời điểm hồ sơ phải xong (`due_at`) |
| Yêu cầu bổ sung | Nhân viên đề nghị sinh viên gửi thêm thông tin hoặc tệp; trong lúc chờ, SLA tạm dừng |
| Escalate | Tăng mức ưu tiên của hồ sơ kèm lý do |
| Chuyển phòng ban | Đưa hồ sơ sang phòng khác; phòng cũ mất quyền ngay |
| Kết quả xử lý | Nội dung người phụ trách nhập khi đóng hồ sơ, sinh viên xem được |
| Đánh giá | Sinh viên chấm 1–5 sao và nhận xét một lần sau khi hồ sơ đóng |
| Lịch sử hồ sơ | Dòng thời gian nghiệp vụ; mốc công khai cho sinh viên, mốc nội bộ cho nhân viên |
| Nhật ký hệ thống (audit) | Bản ghi truy vết bảo mật, chỉ thêm mới, không có màn hình xem |

## Trạng thái và mức ưu tiên

| Giá trị | Nghĩa |
|---|---|
| `NEW` | Mới tiếp nhận, chưa phân công |
| `IN_PROGRESS` | Đang xử lý |
| `PENDING_INFO` | Chờ sinh viên bổ sung; SLA tạm dừng |
| `OVERDUE` | Quá hạn xử lý |
| `CLOSED` | Đã đóng; trạng thái cuối, không mở lại |
| `LOW` / `MEDIUM` / `HIGH` | Mức ưu tiên Thấp / Trung bình (mặc định) / Cao; chỉ dùng để sắp xếp, không đổi SLA |

## Vai trò

| Vai trò | Nghĩa |
|---|---|
| `STUDENT` | Sinh viên, dùng Cổng Sinh viên |
| `STAFF` | Nhân viên phòng ban, dùng Cổng Nhân viên |
| `ADMIN` | Quản trị phía trường, dùng Cổng Admin |

## Kỹ thuật

| Thuật ngữ | Nghĩa |
|---|---|
| SSO / Userinfo | Dịch vụ đăng nhập của trường / dữ liệu tài khoản dịch vụ đó trả về |
| Phiên (JWT, cookie HttpOnly) | Chuỗi phiên có chữ ký lưu trong cookie mà JavaScript không đọc được; server vẫn kiểm tra thu hồi và quyền |
| Idempotency-Key | Mã gắn với một thao tác để thử lại không thực hiện lần hai |
| Version / optimistic locking | Số phiên bản của dữ liệu; chỉ lưu nếu dữ liệu chưa bị đổi kể từ lúc người dùng đọc |
| Transaction | Nhóm thay đổi CSDL được lưu cùng nhau hoặc không lưu gì |
| Snapshot | Bản lưu dữ liệu tại một thời điểm, không cập nhật theo nguồn |
| Polling | Trình duyệt hỏi dữ liệu mới theo chu kỳ |
| CSRF / IDOR | Request ghi giả mạo từ trang khác / truy cập đối tượng của người khác bằng cách đổi ID |
| Magic bytes | Các byte đầu tệp dùng để xác định định dạng thật |
| p95 | 95% request có thời gian phản hồi không vượt giá trị này |

## Quản lý dự án

| Thuật ngữ | Nghĩa |
|---|---|
| Milestone | Cột mốc giao một luồng chạy trọn vẹn qua các vai trò, theo dõi trên GitHub |
| Hotfix | Milestone riêng cho lỗi phát hiện sau khi milestone đã đóng |
| Staging | Môi trường test trước khi triển khai chính thức |
| Feature freeze | Sau milestone Thống kê báo cáo cho Admin thì không thêm chức năng |
| Go-live | Đưa hệ thống vào sử dụng thật trên server của trường |
