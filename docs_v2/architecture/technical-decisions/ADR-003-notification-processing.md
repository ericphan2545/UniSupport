# ADR-003: Xử lý thông báo

**Trạng thái:** Chấp nhận

## Bối cảnh

Sinh viên cần biết khi hồ sơ đổi trạng thái, khi được yêu cầu bổ sung và khi hồ sơ được đóng. Phạm vi hợp đồng chỉ có thông báo trong ứng dụng, không có email, SMS hay push.

## Quyết định

- Thông báo là bản ghi trong CSDL, tạo trong cùng transaction với thay đổi hồ sơ và dùng chung `event_id`. Mỗi sự kiện tối đa một thông báo cho sinh viên.
- Trình duyệt hỏi số thông báo mới mỗi 30 giây, chỉ khi trang đang mở và phiên còn hiệu lực. Hết phiên thì dừng polling và chuyển về trang đăng nhập.
- Lỗi khi hỏi thì giữ số cũ, hiện chỉ báo dữ liệu cũ và thử lại, không dồn retry vô hạn.
- Đánh dấu tất cả đã đọc dùng mốc `read_before` do server cấp, không đánh dấu thông báo đến sau mốc đó.

## Hệ quả

- Không cần hàng đợi tin nhắn hay kết nối giữ mở; đơn giản để triển khai bằng Docker Compose trên server của trường.
- Thông báo có độ trễ tối đa khoảng 30 giây khi trang đang mở.
- Không có thông báo khi sinh viên không mở ứng dụng; sinh viên thấy thông báo chưa đọc ở lần mở sau.
- Đánh dấu đã đọc không thay đổi version của hồ sơ.
