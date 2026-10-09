# M3. Cổng Quản lý

Manager đăng nhập M4 → dashboard số lượng hiện tại/trung bình CLOSED → danh sách tóm tắt nếu chọn chỉ số → báo cáo loại/khối lượng/hài lòng từ dữ liệu M1/M2 → cấp quyền tài khoản và thiết lập SLA. Manager không xử lý ticket hoặc xem chi tiết/tệp sinh viên.

## Nội dung module

- [PRD chi tiết của module](prd.md): 7 chức năng, mỗi chức năng có mô tả, actor, điều kiện, luồng, Input/Output, business rules, lỗi, acceptance criteria và edge case.
- Phần đầu PRD mô tả luồng, đầu vào/đầu ra module, phụ thuộc và những giả định cần xác nhận.
- Actor là người sử dụng/hệ thống. Người lập trình và task thuộc kế hoạch của nhóm, không được suy từ Actor.

## Cách đọc

Đọc luồng và quy ước đầu PRD trước, sau đó xem từng FR. Các quy tắc nghiệp vụ liên module nằm trong [M5](../M05-core-platform/prd.md); phiên, quyền, bảo mật, audit và mã lỗi nằm trong [M4](../M04-authentication-security/prd.md). Các thông tin chưa được trường xác nhận không được coi là đã phê duyệt chỉ vì được viết trong tài liệu.

## Danh sách chức năng

| Mã | Chức năng |
|---|---|
| FR-MGT-01 | Dashboard: số hồ sơ mới, đang xử lý, quá hạn |
| FR-MGT-02 | Dashboard: thời gian xử lý trung bình |
| FR-MGT-03 | Báo cáo loại yêu cầu phổ biến |
| FR-MGT-04 | Báo cáo khối lượng công việc theo phòng ban |
| FR-MGT-05 | Báo cáo mức độ hài lòng của sinh viên |
| FR-MGT-06 | Quản lý phân quyền |
| FR-MGT-07 | Thiết lập SLA cho từng loại yêu cầu |

## Module liên quan

- [M1 — Cổng Sinh viên](../M01-student-portal/prd.md)
- [M2 — Cổng Nhân viên](../M02-staff-portal/prd.md)
- [M4 — Xác thực & Bảo mật](../M04-authentication-security/prd.md)
- [M5 — Lõi hệ thống & Nền tảng](../M05-core-platform/prd.md)

## Quy ước sử dụng tài liệu

Ví dụ tên người/phòng/loại chỉ là dữ liệu kiểm thử. Nếu phát hiện hai module trái nhau, nhóm chốt cách xử lý rồi cập nhật cả FR/AC liên quan trước khi code. Mọi thay đổi phạm vi phải được xác nhận và cập nhật thống nhất trong các module chịu ảnh hưởng.
