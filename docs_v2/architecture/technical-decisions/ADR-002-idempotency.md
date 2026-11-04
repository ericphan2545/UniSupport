# ADR-002: Idempotency cho thao tác tạo và xử lý

**Trạng thái:** Chấp nhận

## Bối cảnh

Sinh viên chủ yếu dùng điện thoại, mạng có thể chập chờn. Nếu mất phản hồi rồi bấm gửi lại, hệ thống không được tạo hai hồ sơ, hai lần bổ sung hay đóng hồ sơ hai lần.

## Quyết định

- Bắt buộc header `Idempotency-Key` cho: tạo hồ sơ, bổ sung thông tin, ghi nhật ký xử lý, yêu cầu bổ sung, đóng hồ sơ.
- Key duy nhất theo tài khoản + thao tác + đối tượng; giữ 24 giờ sau khi hoàn tất.
- Kết quả thành công và key được lưu trong cùng transaction nghiệp vụ.
- Cùng key, cùng payload đã thành công: kiểm tra lại phiên và quyền, rồi trả đúng kết quả đã lưu; không tạo thêm tệp, nhật ký, thông báo.
- Cùng key, khác payload (gồm checksum tệp): 409 `IDEMPOTENCY_PAYLOAD_MISMATCH`.
- Hai request cùng key đang xử lý: một bên chạy, bên kia nhận 409 `REQUEST_IN_PROGRESS` kèm `Retry-After`.
- Đánh giá không dùng key; dùng unique theo hồ sơ và trả 409 nếu đã đánh giá.

## Hệ quả

- Giao diện tạo key một lần cho mỗi thao tác và giữ key khi thử lại.
- Lỗi trước commit không lưu key thành công, nên thử lại cùng key vẫn chạy được.
- Sau 24 giờ không còn bảo đảm; giao diện phải tra cứu kết quả trước khi tạo mới.
- Key không thay cho phân quyền hay rate limit.
