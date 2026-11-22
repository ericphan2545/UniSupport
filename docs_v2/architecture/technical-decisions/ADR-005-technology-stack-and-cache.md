# ADR-005: Lựa chọn ngăn xếp công nghệ và bộ nhớ đệm phía server

**Trạng thái:** Đề xuất *(chờ chốt kỹ thuật tại WBS `1.04–1.05` và xác nhận tương thích hạ tầng với Aurora University tại WBS `1.11–1.14` trong `MS-01`)*

**Liên quan:** `FR-STU-01/BR-05`, `NFR-PERF-10`, `NFR-PERF-11`, giả định `A-11`, `A-14`, rủi ro `R-27`, WBS `1.04`, `1.05`, `1.11`, `1.14`, `1.35`

## Bối cảnh

Bộ đặc tả nghiệp vụ (`modules/`) và yêu cầu phi chức năng (`non-functional-requirements/`) quy định các yêu cầu bắt buộc ở mức hành vi:
- Web application responsive 3 cổng, song ngữ VN/EN (`FR-CORE-08`, `FR-CORE-09`).
- CSDL quan hệ hỗ trợ transaction nguyên tử, sequence theo năm (`ADR-001`), ràng buộc unique, khóa lạc quan theo version (`ADR-004`) và phân quyền chỉ thêm/đọc trên bảng nhật ký (`FR-SEC-06`).
- Kho tệp lưu ngoài thư mục web, tên ngẫu nhiên, chỉ tải qua API có kiểm tra quyền (`FR-SEC-05`).
- **Bộ nhớ đệm phía server (server-side cache):** lưu thông tin cá nhân lấy từ SSO riêng theo từng tài khoản với TTL 15 phút (`FR-STU-01/BR-05`, `NFR-PERF-10`) và cache danh mục loại yêu cầu/phòng ban (`NFR-PERF-11`).
- Đóng gói chạy bằng Docker Compose trên server của Aurora University (`A-11`, `NFR-DEP-04`).

Trước đây, PRD M01 ghi cứng tên `Redis` trong khi tài liệu kiến trúc ghi chưa chốt công nghệ. ADR này thống nhất ranh giới: **yêu cầu nghiệp vụ là cache phía server TTL 15 phút theo tài khoản**, còn **Redis chỉ là một lựa chọn triển khai đề xuất**.

## Giả định công nghệ dùng để ước lượng baseline (1109 giờ / 424.721.000 VNĐ)

Để lập baseline effort, giá và lịch (`Bang_Effort.md`, `quotation.xlsx`, `Master Plan`), đội thực thi (Mô phỏng) áp dụng giả định **`A-14`**:
- Ngăn xếp web phổ biến, mã nguồn mở, phù hợp năng lực đội thực thi và chạy gọn bằng Docker Compose trên server Linux của trường:
  - **Backend API:** TypeScript (Node.js/NestJS hoặc Express) hoặc Java (Spring Boot) / Python (FastAPI).
  - **Frontend:** React / Next.js hoặc Vue (SPA responsive + i18n VN/EN).
  - **CSDL quan hệ:** PostgreSQL (đáp ứng sẵn transaction, sequence, row lock, phân quyền `INSERT/SELECT` riêng trên bảng nhật ký).
  - **Bộ nhớ đệm phía server (cache 15 phút):** **Redis** là phương án triển khai đề xuất ưu tiên (hỗ trợ sẵn TTL 15 phút, rate limit và phiên chia sẻ nếu chạy nhiều worker); có thể thay bằng cache trong bộ nhớ / bảng cache CSDL nếu hệ thống chỉ chạy một instance trên hạ tầng tối giản của trường.
  - **Kho tệp:** Volume lưu trữ cục bộ gắn ngoài web root hoặc S3-compatible storage (MinIO) chạy qua Docker Compose.

## Vai trò quyết định và mốc chốt

| Bước | Vai trò quyết định | Mốc thời gian & WBS | Đầu ra bắt buộc |
|---|---|---|---|
| 1. Đề xuất & chọn ngăn xếp kỹ thuật (gồm phương án cache Redis hay tương đương) | `BE Dev / Senior` (chủ trì), `FE Dev / Senior`, `DevOps / Mid` | `09/09 – 11/09/2026` — WBS `1.04`, `1.05` (issue `#105`) | Cập nhật `ADR-005` từ `Đề xuất` sang `Chấp nhận` (hoặc ghi phương án chốt) trước khi dựng môi trường dev hoàn chỉnh |
| 2. Xác nhận tương thích hạ tầng với Aurora University | `PM / Senior`, `BE Dev / Senior` làm việc với bộ phận CNTT trường | `14/09 – 17/09/2026` — WBS `1.11`, `1.14` (issue `#113`, `#114`) | Xác nhận bằng văn bản tương thích Docker Compose, tài nguyên RAM/CPU (cho CSDL + cache) và SSO OIDC trước khi khởi động lập trình `1.20–1.23` (`18/09/2026`) |

## Ảnh hưởng nếu thay đổi

1. **Thay đổi trong phạm vi tương đương giả định `A-14`** (ví dụ đổi Redis sang in-memory/DB cache cho triển khai đơn node, hoặc chọn giữa React và Vue): tác động kỹ thuật nhỏ (≤ 4–6 giờ cấu hình Docker/thư viện), xử lý nội bộ trong quỹ dự phòng `Q-01` (dòng WBS `1.35`, rủi ro `R-27`), **không thay đổi giá 424.721.000 VNĐ và không dời mốc Go-live**.
2. **Thay đổi vượt khỏi giả định `A-14` hoặc `A-11` theo yêu cầu bắt buộc của trường** (ví dụ yêu cầu CSDL thương mại bản quyền, máy chủ Windows không chạy Docker Compose, hoặc ngôn ngữ/framework đội thực thi không có sẵn nhân sự): kích hoạt rủi ro `R-27` và điều khoản giả định `A-11`, `A-14`; PM / Senior lập đánh giá tác động effort, lịch và báo giá bổ sung theo quy trình Change Request (`03-quan-tri-giam-sat.md` mục 5) trước khi bắt đầu giai đoạn lập trình `1.20–1.23`.
