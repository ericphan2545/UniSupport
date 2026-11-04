# Thiết kế bảo mật

## Phiên

- Sau khi xác thực SSO, ứng dụng phát phiên riêng (JWT ký bởi ứng dụng) trong cookie `HttpOnly`, `Secure`, `SameSite=Lax`. Token của SSO chỉ lưu phía server.
- Phiên hết hạn sau 8 giờ, không gia hạn. Server giữ danh sách phiên để thu hồi ngay khi đăng xuất hoặc khi tài khoản bị vô hiệu hóa.
- Không đọc được kho phiên hoặc quyền thì trả 503, không mặc định cho phép.
- Tham số `state` của SSO dùng một lần, hết hạn sau 10 phút.

## Phân quyền và phạm vi dữ liệu

| Vai trò | Được dùng | Phạm vi dữ liệu |
|---|---|---|
| `STUDENT` | Cổng Sinh viên | Chỉ hồ sơ, tệp, thông báo của chính mình |
| `STAFF` | Cổng Nhân viên | Hồ sơ của phòng mình. Chỉ người phụ trách được ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ. Mọi nhân viên trong phòng được phân công, đặt ưu tiên, escalate, chuyển phòng |
| `ADMIN` | Cổng Admin | Số liệu tổng hợp, danh sách tóm tắt, phân quyền, SLA. Không xem chi tiết hồ sơ, thông tin cá nhân sinh viên, tệp |

- Vai trò, phòng ban, trạng thái tài khoản đọc từ CSDL ở mỗi request; thay đổi có hiệu lực ngay ở request kế tiếp.
- Phạm vi áp ngay trong câu truy vấn (theo `student_id` hoặc `department_id` lấy từ phiên). Mọi `student_id`, `department_id` do client gửi lên bị bỏ qua khi xác định phạm vi.
- Ngoài phạm vi trả 404 giống hệt đối tượng không tồn tại, chống dò mã hồ sơ. Trong phạm vi nhưng không có quyền ghi trả 403.
- Hồ sơ vừa chuyển phòng thì phòng cũ mất quyền ngay.
- API không khai báo vai trò được phép thì mặc định từ chối. Giao diện ẩn nút không thuộc vai trò, nhưng việc chặn luôn ở backend.

## Chống tấn công phổ biến

| Rủi ro | Biện pháp |
|---|---|
| CSRF | CSRF token gắn phiên và kiểm tra Origin cho mọi request ghi |
| IDOR (đổi mã trên URL) | Kiểm tra phạm vi trong truy vấn, trả 404 thống nhất, rate limit |
| XSS | Nội dung người dùng là văn bản thuần, encode theo ngữ cảnh khi hiển thị |
| Giả mạo tệp | Kiểm tra magic bytes ở backend; tên lưu ngẫu nhiên; header `X-Content-Type-Options: nosniff` |
| Lộ dữ liệu qua cache | `Cache-Control: private, no-store` cho trang và tệp nhạy cảm; xóa cache giao diện khi đăng xuất |
| Lộ thông tin hệ thống | Lỗi chỉ trả `error_code`, thông báo VN/EN và `correlation_id` |

## Bảo vệ tệp

- Tệp lưu ngoài thư mục web, không có đường dẫn trực tiếp, không phát URL có sẵn quyền.
- Chỉ sinh viên chủ hồ sơ và nhân viên phòng đang giữ hồ sơ được xem, tải. Admin nhận 403.
- Mỗi lần tải ghi `DOWNLOAD_FILE` trước khi đọc tệp và `FILE_READ_RESULT` sau khi đọc kho. Ghi nhật ký khởi tạo lỗi thì không trả tệp.

## Nhật ký hệ thống

- Ghi các thao tác: đăng nhập, đăng xuất, tạo hồ sơ, bổ sung, đánh giá, ghi nhật ký xử lý, yêu cầu bổ sung, đóng, phân công, đổi ưu tiên, escalate, chuyển phòng, đổi trạng thái do hệ thống, đổi hạn, tải lên và tải xuống tệp, đổi quyền, đổi SLA.
- Mỗi bản ghi: thời điểm (UTC), người thực hiện, vai trò, mã thao tác, đối tượng, giá trị cũ và mới (chỉ trường được phép), IP, kết quả. Không lưu mô tả, ghi chú, nhận xét, lý do hay nội dung tệp.
- Chỉ thêm mới. Tài khoản CSDL của ứng dụng chỉ có quyền INSERT và SELECT trên bảng nhật ký. Không có màn hình xem; người vận hành truy vấn CSDL.

## Rate limit

Áp theo người dùng và theo chức năng; riêng callback đăng nhập áp theo IP. Vượt giới hạn trả 429 kèm `Retry-After`. Ngưỡng từng chức năng: `non-functional-requirements/security.md`.

## Vận hành

- Chỉ phục vụ HTTPS; chứng chỉ do trường cung cấp.
- SSO giả lập chỉ bật ở dev và test; production bật giả lập thì ứng dụng từ chối khởi động.
- Bí mật nằm trong `.env` trên server, không đưa vào repo. Log vận hành không chứa cookie, token hay nội dung nhạy cảm.
