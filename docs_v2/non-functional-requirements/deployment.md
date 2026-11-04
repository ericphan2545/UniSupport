# Triển khai và vận hành

Đóng gói (Docker Compose, `.env`, `.env.example`), các môi trường và kiểm tra SSO giả lập khi khởi động nằm ở `architecture/architecture-overview.md` và `architecture/security-design.md`. Tài liệu này ghi dữ liệu ban đầu, việc triển khai production và trách nhiệm sau bàn giao.

## Dữ liệu ban đầu

- Danh mục loại yêu cầu, phòng ban và SLA ban đầu nạp từ dữ liệu seed, idempotent theo mã, trong một transaction.
- Nạp lại không ghi đè SLA Admin đã sửa, không xóa loại đã có hồ sơ; nạp lỗi thì giữ nguyên bộ đang dùng.
- Loại yêu cầu không ánh xạ phòng ban hoặc ánh xạ tới phòng không tồn tại: nạp thất bại, ứng dụng không khởi động.
- Không có giao diện thêm, sửa, xóa danh mục. Muốn đổi thì đội phát triển cập nhật dữ liệu nạp và triển khai lại.

## Triển khai production

- Các việc theo dõi trong milestone Bàn giao trên GitHub: Docker Compose production và `.env.example`; triển khai lên server trường, nạp tài khoản Admin đầu tiên, nối SSO thật; dự phòng xử lý sự cố trước Go-live.
- Proposal 6.1: ứng dụng phải chạy thành công trên máy chủ do trường chỉ định.

## Sau bàn giao

Theo Proposal 6.4:

- Trường chịu trách nhiệm vận hành: quản lý server, cấp tài khoản người dùng nội bộ, sao lưu dữ liệu định kỳ, an toàn thông tin nội bộ.
- Chi phí server, kho lưu tệp, tên miền, chứng chỉ SSL do trường chi trả.
- Bên phát triển hỗ trợ cấu hình hạ tầng ban đầu trong lúc triển khai.

Thời hạn lưu dữ liệu: xem [reliability.md](reliability.md).
