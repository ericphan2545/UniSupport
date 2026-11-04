# ADR-004: Thao tác đồng thời trên hồ sơ

**Trạng thái:** Chấp nhận

## Bối cảnh

Nhiều nhân viên cùng phòng có thể cùng phân công, đổi ưu tiên hoặc chuyển một hồ sơ. Sinh viên có thể bổ sung đúng lúc nhân viên đóng hồ sơ. Tác vụ quá hạn chạy mỗi phút cũng đổi trạng thái hồ sơ.

## Quyết định

- Hồ sơ có cột `version`, tăng một lần cho mỗi thao tác thay đổi: phân công, đổi ưu tiên, escalate, chuyển phòng, ghi nhật ký, yêu cầu bổ sung, bổ sung, đóng, tác vụ quá hạn. Thao tác không đổi gì (no-op) không tăng version.
- Mọi API thay đổi hồ sơ gửi `version` đang có. Server chỉ cập nhật khi version còn khớp (optimistic locking); lệch trả 409, giao diện tải lại hồ sơ.
- Tác vụ quá hạn khóa từng hồ sơ theo version, bỏ qua hồ sơ đã bị đổi; chỉ một tiến trình chạy tại một thời điểm.
- Phân công kiểm tra người nhận đang active và cùng phòng trong cùng transaction với việc đổi quyền tài khoản, để không gán hồ sơ cho tài khoản vừa bị vô hiệu hóa hoặc đổi phòng.
- Kiểm tra phạm vi trước version: nhân viên phòng cũ sau khi hồ sơ đã chuyển nhận 404, không nhận 409.

## Hệ quả

- Không cần khóa hồ sơ trong lúc nhân viên đang xem; người thao tác sau nhận 409 và thấy dữ liệu mới.
- Bổ sung và đóng cạnh tranh cùng version: chỉ một bên thành công; hồ sơ đã CLOSED không bị mở lại.
- Cần test race cho: phân công đồng thời, bổ sung/đóng, chuyển phòng và request cũ, tác vụ quá hạn/đóng.
