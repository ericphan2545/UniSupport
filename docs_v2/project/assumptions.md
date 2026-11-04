# Giả định

Những điều nhóm đang coi là đúng để thiết kế và lập kế hoạch. Giả định nào được trường xác nhận khác đi thì xử lý như một thay đổi theo Kế hoạch quản trị và giám sát mục 5.

## Từ Proposal

| # | Giả định |
|---|---|
| A1 | Quy mô nhỏ: khoảng 3.000 sinh viên đăng ký, 100–200 người dùng đồng thời |
| A2 | Hiệu năng phụ thuộc vào hạ tầng do trường cung cấp |
| A3 | Thời gian triển khai không tính thời gian chờ phản hồi từ trường |
| A4 | Không yêu cầu tuân thủ chuẩn bảo mật chuyên sâu; bảo vệ dữ liệu sinh viên ở mức cơ bản |

## Thiết kế đang dùng

| # | Giả định | Ảnh hưởng nếu sai |
|---|---|---|
| A5 | SSO của trường dùng luồng authorization code (OIDC) và trả email trường đã xác thực làm định danh | Phải sửa luồng đăng nhập và tích hợp SSO |
| A6 | SSO trả được họ tên và MSSV; khoa, lớp, email, số điện thoại có thể thiếu | Thiếu họ tên hoặc MSSV thì không dùng được thông tin sinh viên |
| A7 | Mỗi loại yêu cầu do đúng một phòng ban tiếp nhận | Phải đổi mô hình danh mục và luồng gán phòng |
| A8 | SLA tính theo giờ lịch (cả ngoài giờ hành chính và ngày nghỉ); thời gian chờ sinh viên bổ sung không tính vào SLA | Phải đổi cách tính hạn và báo cáo thời gian xử lý |
| A9 | Giới hạn tệp: PDF/JPG/PNG, tối đa 5 tệp mỗi lần, 10 MiB mỗi tệp | Phải đổi cấu hình tệp và kiểm thử |
| A10 | Danh mục loại yêu cầu, phòng ban và SLA ban đầu do đội phát triển nạp khi triển khai; không có màn hình sửa danh mục | Phải thêm chức năng quản lý danh mục |
| A11 | Trường cung cấp server chạy được Docker Compose, kho lưu tệp và chứng chỉ SSL | Phải đổi cách đóng gói và triển khai |
| A12 | Không cần đọc nhật ký qua giao diện; người vận hành truy vấn CSDL | Phải thêm màn hình nhật ký và phân quyền đọc |
