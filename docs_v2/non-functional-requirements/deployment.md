# Triển khai và vận hành

Đóng gói (Docker Compose, `.env`, `.env.example`), các môi trường và kiểm tra SSO giả lập khi khởi động nằm ở `architecture/architecture-overview.md` và `architecture/security-design.md`. Tài liệu này ghi yêu cầu về dữ liệu ban đầu, triển khai production và trách nhiệm sau bàn giao.

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-DEP-01` | Danh mục loại yêu cầu, phòng ban và SLA ban đầu nạp từ dữ liệu seed, idempotent theo mã, trong một transaction; nạp lại không ghi đè SLA Admin đã sửa, không xóa loại đã có hồ sơ; nạp lỗi thì giữ nguyên bộ đang dùng | `TS-16` |
| `NFR-DEP-02` | Loại yêu cầu không ánh xạ phòng ban, hoặc ánh xạ tới phòng không tồn tại: nạp thất bại, ứng dụng không khởi động | `TS-16`, `FR-CORE-05/AC-02` |
| `NFR-DEP-03` | Bản production từ chối khởi động nếu cấu hình bật SSO giả lập | `NFR-SEC-01` |
| `NFR-DEP-04` | Ứng dụng chạy bằng Docker Compose trên server do trường chỉ định, chỉ qua HTTPS, đăng nhập bằng SSO thật; Admin đầu tiên vào được Cổng Admin | Buổi demo nghiệm thu `MS-06`: chạy lại `TS-01` đến `TS-08` trên server trường |

- Không có giao diện thêm, sửa, xóa danh mục (`NG-08`). Muốn đổi thì đội phát triển cập nhật dữ liệu nạp và triển khai lại.
- Các việc triển khai production theo dõi trong `MS-06` (sheet WBS và GitHub): Docker Compose production và `.env.example`; triển khai lên server trường, nạp tài khoản Admin đầu tiên, nối SSO thật, chạy test hiệu năng `TS-19`; hỗ trợ Go-live và xử lý sự cố triển khai.

## Sau bàn giao

Theo Proposal mục 6.4 (non-goal `NG-11`):

- Trường chịu trách nhiệm vận hành: quản lý server, cấp tài khoản người dùng nội bộ, sao lưu dữ liệu định kỳ, an toàn thông tin nội bộ.
- Chi phí server, kho lưu tệp, tên miền, chứng chỉ SSL do trường chi trả.
- Bên phát triển hỗ trợ cấu hình hạ tầng ban đầu trong lúc triển khai.

Thời hạn lưu dữ liệu: xem [reliability.md](reliability.md) (`NFR-REL-10`).
