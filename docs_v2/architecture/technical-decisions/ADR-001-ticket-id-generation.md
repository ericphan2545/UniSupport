# ADR-001: Sinh mã hồ sơ

**Trạng thái:** Chấp nhận

## Bối cảnh

Sinh viên và nhân viên cần một mã dễ đọc để tra cứu và trao đổi. Mã không được chứa thông tin cá nhân và không được trùng khi nhiều sinh viên gửi cùng lúc.

## Quyết định

- Định dạng `USP-YYYY-NNNNN`: `YYYY` là năm tạo hồ sơ theo giờ Việt Nam; `NNNNN` là số thứ tự 5 chữ số, bắt đầu lại từ `00001` mỗi năm, tự mở rộng khi vượt 99999.
- Số thứ tự lấy từ sequence của CSDL, mỗi năm một sequence, trong transaction tạo hồ sơ. Cột mã có unique constraint.
- Mã không đổi suốt vòng đời hồ sơ.

## Hệ quả

- Không bao giờ trùng mã, kể cả 100 hồ sơ tạo đồng thời.
- Mã có thể nhảy số khi transaction bị rollback; đây là hành vi đúng, không tái sử dụng số đã lấy.
- Mã dễ đoán theo thứ tự nên không được dùng làm cơ chế bảo mật: truy cập vẫn kiểm tra phạm vi và trả 404 cho hồ sơ không thuộc người gọi.
- Hồ sơ tạo lúc 06:59 ngày 01/01 giờ Việt Nam (23:59 ngày 31/12 UTC) mang năm mới.
