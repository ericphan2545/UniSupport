# M2. Cổng Nhân viên

M1 tạo ticket NEW vào phòng → staff mở hàng đợi/chi tiết → phân công người nhận active cùng phòng → IN_PROGRESS → người phụ trách xem tệp/ghi note/xử lý → yêu cầu bổ sung nếu thiếu → student trả lời → xử lý tiếp → nhập resolution và đóng CLOSED. Sau đóng, student đánh giá và M3 tính báo cáo.

## Nội dung module

- [PRD chi tiết của module](prd.md): 10 chức năng, mỗi chức năng có mô tả, actor, điều kiện, luồng, Input/Output, business rules, lỗi, acceptance criteria và edge case.
- Phần đầu PRD mô tả luồng, đầu vào/đầu ra module, phụ thuộc và những giả định cần xác nhận.
- Actor là người sử dụng/hệ thống. Người lập trình và task thuộc kế hoạch của nhóm, không được suy từ Actor.

## Cách đọc

Đọc luồng và quy ước đầu PRD trước, sau đó xem từng FR. Các quy tắc nghiệp vụ liên module nằm trong [M5](../M05-core-platform/prd.md); phiên, quyền, bảo mật, audit và mã lỗi nằm trong [M4](../M04-authentication-security/prd.md). Các thông tin chưa được trường xác nhận không được coi là đã phê duyệt chỉ vì được viết trong tài liệu.

## Danh sách chức năng

| Mã | Chức năng |
|---|---|
| FR-STF-01 | Hàng đợi hồ sơ của phòng ban |
| FR-STF-02 | Xem thông tin sinh viên và nội dung yêu cầu |
| FR-STF-03 | Xem và tải tệp đính kèm |
| FR-STF-04 | Ghi nhật ký xử lý |
| FR-STF-05 | Yêu cầu sinh viên bổ sung thông tin |
| FR-STF-06 | Đóng hồ sơ |
| FR-STF-07 | Phân công nhân viên phụ trách |
| FR-STF-08 | Đặt mức ưu tiên |
| FR-STF-09 | Chuyển hồ sơ sang phòng ban khác |
| FR-STF-10 | Tăng mức ưu tiên (escalate) |

## Module liên quan

- [M1 — Cổng Sinh viên](../M01-student-portal/prd.md)
- [M3 — Cổng Quản lý](../M03-management-portal/prd.md)
- [M4 — Xác thực & Bảo mật](../M04-authentication-security/prd.md)
- [M5 — Lõi hệ thống & Nền tảng](../M05-core-platform/prd.md)

## Quy ước sử dụng tài liệu

Ví dụ tên người/phòng/loại chỉ là dữ liệu kiểm thử. Nếu phát hiện hai module trái nhau, nhóm chốt cách xử lý rồi cập nhật cả FR/AC liên quan trước khi code. Mọi thay đổi phạm vi phải được xác nhận và cập nhật thống nhất trong các module chịu ảnh hưởng.
