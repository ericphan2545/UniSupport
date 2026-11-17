# Giám sát và truy vết

Tra lỗi dựa vào ba nguồn: mã truy vết trong response lỗi, log vận hành và nhật ký hệ thống.

| Mã | Yêu cầu | Cách đo |
|---|---|---|
| `NFR-OBS-01` | Mọi response lỗi có `correlation_id`; khi người dùng báo lỗi, đối chiếu mã này với log vận hành | Kiểm response của các ca lỗi trong test case |
| `NFR-OBS-02` | Log vận hành ghi thời điểm theo UTC; không ghi cookie, token, nội dung nhạy cảm | Rà log sau khi chạy `TS-01` đến `TS-08` |
| `NFR-OBS-03` | Các sự kiện ở danh sách dưới đều được ghi log | Chạy các ca lỗi tương ứng trong `TS-12`, `TS-14`, `TS-16` và kiểm log |
| `NFR-OBS-04` | Nhật ký hệ thống lưu trong CSDL, chỉ thêm mới, thời điểm UTC; khi truy vấn hiển thị giờ Việt Nam. Không có màn hình xem; người vận hành truy vấn CSDL theo quyền vận hành | `TS-13` |

## Sự kiện phải ghi log (`NFR-OBS-03`)

- Thiếu bản dịch tiếng Anh của nhãn hoặc tên danh mục (cảnh báo).
- Sinh viên gửi loại yêu cầu chưa ánh xạ phòng ban (lỗi cấu hình).
- Nạp danh mục thất bại (ghi rõ loại yêu cầu bị lỗi).
- Xóa bù tệp thất bại (ghi mã tham chiếu để tác vụ dọn tệp xử lý lại).
- Tệp không còn trên kho khi tải.
- Ghi kết quả đọc tệp lỗi sau khi đã bắt đầu trả tệp (cảnh báo).
- Lưu thu hồi phiên lỗi khi đăng xuất.
- Không ghi được nhật ký đăng nhập thất bại.
- Vi phạm unique bất thường khi sinh mã hồ sơ.

Các thao tác được ghi vào nhật ký hệ thống: xem `architecture/security-design.md`.
