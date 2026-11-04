# Độ tin cậy

## Toàn vẹn dữ liệu

| Yêu cầu | Kiểm chứng |
|---|---|
| Mỗi thao tác ghi lưu hồ sơ, lịch sử, nhật ký hệ thống, thông báo và kết quả idempotency trong cùng một transaction; lỗi thì không lưu phần nào | X-04, X-05 |
| Ghi nhật ký hệ thống lỗi thì rollback cả thao tác | X-05 |
| Tạo hồ sơ, bổ sung thông tin, ghi nhật ký xử lý, yêu cầu bổ sung, đóng hồ sơ bắt buộc `Idempotency-Key`, lưu 24 giờ; gửi lại cùng key không thực hiện lần hai | X-03 |
| Thao tác sửa hồ sơ, tài khoản, SLA gửi kèm version; version cũ trả 409 | X-02 |
| Không xác minh được phiên hoặc quyền thì trả 503, không mặc định cho phép | X-06 |
| Tệp chỉ được tham chiếu sau khi CSDL commit; rollback thì xóa tệp đã chuẩn bị; xóa lỗi thì tác vụ dọn tệp xử lý lại; tệp chưa được tham chiếu không tải được | X-04 |

## Tác vụ nền

- Tác vụ quá hạn: chỉ một tiến trình chạy hiệu lực; khóa từng hồ sơ theo version, bỏ qua hồ sơ đã đổi; bị dừng giữa chừng thì lần chạy sau xử lý tiếp các hồ sơ còn lại (E2E-06).
- Tác vụ dọn tệp: dọn tệp tạm không được tham chiếu; không xóa tệp đã được tham chiếu bởi transaction thành công.

## Khi lỗi

- Không trắng trang. Lỗi khi đọc: giữ dữ liệu cũ kèm chỉ báo hoặc hiện nút Thử lại.
- Lỗi khi gửi: giữ nội dung form và tệp đã chọn trên trình duyệt để thử lại với cùng `Idempotency-Key`; không lưu nội dung nhạy cảm lâu dài trong localStorage.
- Polling lỗi: giữ giá trị cũ kèm chỉ báo, không dồn request thử lại; hết phiên thì dừng polling và chuyển về đăng nhập.
- Ngưỡng timeout: xem [performance.md](performance.md).

## Lưu trữ dữ liệu

- Thời hạn lưu hồ sơ, tệp, lịch sử, nhật ký và lịch sao lưu phải được trường xác nhận trước production.
- Hệ thống không tự xóa dữ liệu.
- Trách nhiệm sao lưu sau bàn giao: xem [deployment.md](deployment.md).
