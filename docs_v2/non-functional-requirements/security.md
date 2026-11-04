# Bảo mật

Proposal không yêu cầu tuân thủ chuẩn bảo mật chuyên sâu, chỉ bảo vệ dữ liệu sinh viên ở mức cơ bản: đăng nhập bằng tài khoản trường, giới hạn phạm vi truy cập dữ liệu sinh viên, bảo vệ tệp đính kèm, ghi nhật ký hệ thống.

Cách thực hiện từng biện pháp nằm ở `architecture/security-design.md`. Tài liệu này ghi cách kiểm chứng và ngưỡng rate limit.

## Kiểm chứng

| Yêu cầu | Kiểm chứng |
|---|---|
| Chỉ đăng nhập qua SSO của trường; SSO giả lập không chạy ở production | E2E-01; khởi động bản production với cấu hình bật SSO giả lập → ứng dụng từ chối khởi động |
| Phiên trong cookie HttpOnly, Secure, SameSite=Lax; 8 giờ; thu hồi khi đăng xuất hoặc tài khoản bị vô hiệu hóa | E2E-01, X-06 |
| Vai trò và phạm vi dữ liệu kiểm tra ở backend; ngoài phạm vi trả 404, không có quyền ghi trả 403 | E2E-01, X-01 |
| Request ghi kiểm tra CSRF token và Origin | Gửi request ghi thiếu CSRF token hoặc từ Origin khác → 403 |
| Nội dung người dùng nhập không thực thi được | Nhập thẻ HTML và script vào mô tả, ghi chú, kết quả, lý do, nhận xét, tên tệp → hiển thị nguyên văn |
| Tệp chỉ tải được qua API có kiểm tra quyền; định dạng kiểm tra bằng magic bytes | X-01, X-04 |
| Trang và tệp có dữ liệu cá nhân không bị lưu cache | X-06; header `Cache-Control: private, no-store` |
| Lỗi không lộ thông tin hệ thống | Response lỗi của mọi test case không có stack trace, câu SQL, đường dẫn kho, token |
| Nhật ký hệ thống chỉ thêm mới; log không chứa cookie, token, nội dung nhạy cảm | X-05; rà log sau khi chạy các kịch bản E2E |
| Chỉ phục vụ HTTPS | Gọi bằng HTTP → chuyển sang HTTPS |
| Bí mật không nằm trong repo | Rà repo và `.env.example` trước bàn giao |
| Rate limit theo bảng dưới | X-10 |

## Rate limit

Vượt giới hạn trả 429 kèm `Retry-After`. Chức năng không có trong bảng chưa có ngưỡng riêng.

| Chức năng | Giới hạn | Tính theo |
|---|---|---|
| Callback đăng nhập SSO | 10 lần/phút | IP |
| Tạo yêu cầu hỗ trợ | 10 hồ sơ/giờ | Sinh viên |
| Danh mục loại yêu cầu và phòng tiếp nhận | 60 request/phút | Người dùng |
| Xem danh sách yêu cầu của mình | 60 request/phút | Sinh viên |
| Xem trạng thái và tiến độ của từng yêu cầu | 60 request/phút | Sinh viên |
| Hàng đợi hồ sơ của phòng ban | 120 request/phút | Nhân viên |
| Xem thông tin sinh viên và nội dung yêu cầu | 120 request/phút | Nhân viên |
| Xem và tải tệp đính kèm | 30 lần tải/phút | Nhân viên |
| Ghi nhật ký xử lý | 30 ghi chú/phút | Nhân viên |
| Dashboard: số hồ sơ mới, đang xử lý, quá hạn | 60 request/phút | Người dùng |
| Dashboard: thời gian xử lý trung bình | 60 request/phút | Người dùng |
| Báo cáo loại yêu cầu phổ biến | 30 request/phút | Người dùng |
| Báo cáo khối lượng công việc theo phòng ban | 30 request/phút | Người dùng |
| Báo cáo mức độ hài lòng của sinh viên | 30 request/phút | Người dùng |
| Quản lý phân quyền | 30 thao tác ghi/phút | Người dùng |
| Thiết lập SLA | 30 thao tác ghi/phút | Người dùng |

- Ngưỡng callback đăng nhập cần kiểm chứng nếu mạng trường dùng chung một IP (NAT).
- Gửi lại thao tác tạo hồ sơ đã thành công với cùng `Idempotency-Key` không tính thêm vào giới hạn 10 hồ sơ/giờ.
