# M1. Cổng Sinh viên

Sinh viên đăng nhập qua M4 → xem thông tin → chọn loại yêu cầu để biết phòng nhận → nhập mô tả/tệp → gửi và nhận mã NEW → theo dõi tiến độ. Staff M2 phân công và xử lý; khi cần bổ sung, sinh viên nhận thông báo và trả lời. Staff đóng với kết quả; sinh viên xem và đánh giá một lần. M3 tổng hợp báo cáo; M5 bảo đảm mã/trạng thái/SLA/tệp/lịch sử thống nhất.

## Nội dung module

- [PRD chi tiết của module](prd.md): 10 chức năng, mỗi chức năng có mô tả, actor, điều kiện, luồng, Input/Output, business rules, lỗi, acceptance criteria và edge case.
- Phần đầu PRD mô tả luồng, đầu vào/đầu ra module, phụ thuộc và những giả định cần xác nhận.
- Actor là người sử dụng/hệ thống. Người lập trình và task thuộc kế hoạch của nhóm, không được suy từ Actor.

## Cách đọc

Đọc luồng và quy ước đầu PRD trước, sau đó xem từng FR. Các quy tắc nghiệp vụ liên module nằm trong [M5](../M05-core-platform/prd.md); phiên, quyền, bảo mật, audit và mã lỗi nằm trong [M4](../M04-authentication-security/prd.md). Các thông tin chưa được trường xác nhận không được coi là đã phê duyệt chỉ vì được viết trong tài liệu.

## Danh sách chức năng

| Mã | Chức năng |
|---|---|
| FR-STU-01 | Xem thông tin cá nhân lấy từ SSO |
| FR-STU-02 | Tạo yêu cầu hỗ trợ |
| FR-STU-03 | Hiển thị phòng ban tiếp nhận |
| FR-STU-04 | Đính kèm tệp minh chứng |
| FR-STU-05 | Nhận mã hồ sơ và xác nhận gửi thành công |
| FR-STU-06 | Xem danh sách yêu cầu của mình |
| FR-STU-07 | Xem trạng thái và tiến độ của từng yêu cầu |
| FR-STU-08 | Nhận thông báo trong app |
| FR-STU-09 | Bổ sung thông tin khi được yêu cầu |
| FR-STU-10 | Đánh giá chất lượng hỗ trợ |

## Module liên quan

- [M2 — Cổng Nhân viên](../M02-staff-portal/prd.md)
- [M3 — Cổng Quản lý](../M03-management-portal/prd.md)
- [M4 — Xác thực & Bảo mật](../M04-authentication-security/prd.md)
- [M5 — Lõi hệ thống & Nền tảng](../M05-core-platform/prd.md)

## Quy ước sử dụng tài liệu

Ví dụ tên người/phòng/loại chỉ là dữ liệu kiểm thử. Nếu phát hiện hai module trái nhau, nhóm chốt cách xử lý rồi cập nhật cả FR/AC liên quan trước khi code. Mọi thay đổi phạm vi phải được xác nhận và cập nhật thống nhất trong các module chịu ảnh hưởng.
