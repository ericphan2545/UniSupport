# Độ tin cậy

## Toàn vẹn dữ liệu

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-REL-01` | Mỗi thao tác ghi lưu hồ sơ, lịch sử, nhật ký hệ thống, thông báo và kết quả idempotency trong cùng một transaction; lỗi thì không lưu phần nào | `TS-12`, `TS-13` |
| `NFR-REL-02` | Ghi nhật ký hệ thống lỗi thì rollback cả thao tác | `TS-13` |
| `NFR-REL-03` | Tạo hồ sơ, bổ sung thông tin, ghi nhật ký xử lý, yêu cầu bổ sung, đóng hồ sơ bắt buộc `Idempotency-Key`, lưu 24 giờ; gửi lại cùng key không thực hiện lần hai | `TS-11` |
| `NFR-REL-04` | Thao tác sửa hồ sơ, tài khoản, SLA gửi kèm version; version cũ trả 409 | `TS-10` |
| `NFR-REL-05` | Không xác minh được phiên hoặc quyền thì trả 503, không mặc định cho phép | `TS-14` |
| `NFR-REL-06` | Tệp chỉ được tham chiếu sau khi CSDL commit; rollback thì xóa tệp đã chuẩn bị; xóa lỗi thì tác vụ dọn tệp xử lý lại; tệp chưa được tham chiếu không tải được | `TS-12` |

## Tác vụ nền

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-REL-07` | Tác vụ quá hạn: chỉ một tiến trình chạy hiệu lực; khóa từng hồ sơ theo version, bỏ qua hồ sơ đã đổi; bị dừng giữa chừng thì lần chạy sau xử lý tiếp các hồ sơ còn lại | `TS-06` bước 5 |
| `NFR-REL-08` | Tác vụ dọn tệp: dọn tệp tạm không được tham chiếu; không xóa tệp đã được tham chiếu bởi transaction thành công | `TS-12` |

## Khi lỗi

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-REL-09` | Không trắng trang. Lỗi khi đọc: giữ dữ liệu cũ kèm chỉ báo hoặc hiện nút Thử lại. Lỗi khi gửi: giữ nội dung form và tệp đã chọn để thử lại với cùng `Idempotency-Key`; không lưu nội dung nhạy cảm lâu dài trong localStorage. Polling lỗi: giữ giá trị cũ, không dồn request thử lại; hết phiên thì dừng polling và chuyển về đăng nhập | `TS-11`, `FR-CORE-09/AC-06` |

Ngưỡng timeout: xem [performance.md](performance.md) (`NFR-PERF-04` đến `NFR-PERF-07`).

## Lưu trữ dữ liệu

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-REL-10` | Hệ thống không tự xóa dữ liệu. Thời hạn lưu hồ sơ, tệp, lịch sử, nhật ký và lịch sao lưu phải được trường xác nhận trước production (rủi ro `R-25`) | Rà cấu hình và tác vụ nền trước bàn giao: không có tác vụ xóa dữ liệu nghiệp vụ |

Trách nhiệm sao lưu sau bàn giao: xem [deployment.md](deployment.md).
