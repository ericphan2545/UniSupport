# M4. Xác thực & Bảo mật

Trường/DevOps xác nhận SSO và nạp MANAGER đầu → manager cấp quyền staff → người dùng login → xác minh danh tính trường → tìm account hoặc tạo STUDENT → kiểm tra active → phát cookie phiên ứng dụng → đưa vào M1/M2/M3. Mỗi request đọc quyền hiện tại, kiểm tra phạm vi rồi mới ghi/đọc dữ liệu. Logout thu hồi phiên hiện tại khi server xác nhận thành công.

## Nội dung module

- [PRD chi tiết của module](prd.md): 6 chức năng, mỗi chức năng có mô tả, actor, điều kiện, luồng, Input/Output, business rules, lỗi, acceptance criteria và edge case.
- Phần đầu PRD mô tả luồng, đầu vào/đầu ra module, phụ thuộc và những giả định cần xác nhận.
- Actor là người sử dụng/hệ thống. Người lập trình và task thuộc kế hoạch của nhóm, không được suy từ Actor.

## Cách đọc

Đọc luồng và quy ước đầu PRD trước, sau đó xem từng FR. Các quy tắc nghiệp vụ liên module nằm trong [M5](../M05-core-platform/prd.md); phiên, quyền, bảo mật, audit và mã lỗi nằm trong [M4](../M04-authentication-security/prd.md). Các thông tin chưa được trường xác nhận không được coi là đã phê duyệt chỉ vì được viết trong tài liệu.

## Danh sách chức năng

| Mã | Chức năng |
|---|---|
| FR-SEC-01 | Đăng nhập bằng SSO tài khoản trường |
| FR-SEC-02 | Đăng xuất |
| FR-SEC-03 | Phân quyền theo 3 vai trò |
| FR-SEC-04 | Giới hạn phạm vi dữ liệu theo vai trò |
| FR-SEC-05 | Bảo vệ tệp đính kèm |
| FR-SEC-06 | Ghi nhật ký hệ thống |

## Module liên quan

- [M1 — Cổng Sinh viên](../M01-student-portal/prd.md)
- [M2 — Cổng Nhân viên](../M02-staff-portal/prd.md)
- [M3 — Cổng Quản lý](../M03-management-portal/prd.md)
- [M5 — Lõi hệ thống & Nền tảng](../M05-core-platform/prd.md)

## Quy ước sử dụng tài liệu

Ví dụ tên người/phòng/loại chỉ là dữ liệu kiểm thử. Nếu phát hiện hai module trái nhau, nhóm chốt cách xử lý rồi cập nhật cả FR/AC liên quan trước khi code. Mọi thay đổi phạm vi phải được xác nhận và cập nhật thống nhất trong các module chịu ảnh hưởng.
