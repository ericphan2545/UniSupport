# M5. Lõi hệ thống & Nền tảng

Khi M1 tạo ticket, M5 lấy danh mục/SLA snapshot và sinh mã; khi M2/M1 thao tác, M5 kiểm tra status/version, cập nhật hạn/pause và history. Tác vụ quá hạn đổi NEW/IN_PROGRESS thành OVERDUE. M3 đọc số liệu từ timestamp và trường đã chốt; M4 bảo đảm scope/audit. VN/EN và responsive áp dụng cho cả ba cổng.

## Nội dung module

- [PRD chi tiết của module](prd.md): 9 chức năng, mỗi chức năng có mô tả, actor, điều kiện, luồng, Input/Output, business rules, lỗi, acceptance criteria và edge case.
- Phần đầu PRD mô tả luồng, đầu vào/đầu ra module, phụ thuộc và những giả định cần xác nhận.
- Actor là người sử dụng/hệ thống. Người lập trình và task thuộc kế hoạch của nhóm, không được suy từ Actor.

## Cách đọc

Đọc luồng và quy ước đầu PRD trước, sau đó xem từng FR. Các quy tắc nghiệp vụ liên module nằm trong [M5](../M05-core-platform/prd.md); phiên, quyền, bảo mật, audit và mã lỗi nằm trong [M4](../M04-authentication-security/prd.md). Các thông tin chưa được trường xác nhận không được coi là đã phê duyệt chỉ vì được viết trong tài liệu.

## Danh sách chức năng

| Mã | Chức năng |
|---|---|
| FR-CORE-01 | Sinh mã hồ sơ |
| FR-CORE-02 | Trạng thái hồ sơ và quy tắc chuyển trạng thái |
| FR-CORE-03 | Tính hạn xử lý (SLA) |
| FR-CORE-04 | Mức độ ưu tiên |
| FR-CORE-05 | Danh mục loại yêu cầu và phòng ban |
| FR-CORE-06 | Lịch sử thay đổi của hồ sơ |
| FR-CORE-07 | Giới hạn tệp đính kèm |
| FR-CORE-08 | Giao diện song ngữ VN/EN |
| FR-CORE-09 | Giao diện responsive cho mobile và desktop |

## Module liên quan

- [M1 — Cổng Sinh viên](../M01-student-portal/prd.md)
- [M2 — Cổng Nhân viên](../M02-staff-portal/prd.md)
- [M3 — Cổng Quản lý](../M03-management-portal/prd.md)
- [M4 — Xác thực & Bảo mật](../M04-authentication-security/prd.md)

## Quy ước sử dụng tài liệu

Ví dụ tên người/phòng/loại chỉ là dữ liệu kiểm thử. Nếu phát hiện hai module trái nhau, nhóm chốt cách xử lý rồi cập nhật cả FR/AC liên quan trước khi code. Mọi thay đổi phạm vi phải được xác nhận và cập nhật thống nhất trong các module chịu ảnh hưởng.
