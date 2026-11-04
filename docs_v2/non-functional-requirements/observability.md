# Giám sát và truy vết

Tra lỗi dựa vào ba nguồn: mã truy vết trong response lỗi, log vận hành và nhật ký hệ thống.

## Mã truy vết

Mọi response lỗi có `correlation_id`. Khi người dùng báo lỗi, đối chiếu mã này với log vận hành.

## Log vận hành

- Thời điểm ghi theo UTC.
- Không ghi cookie, token, nội dung nhạy cảm.
- Phải ghi log khi:
  - thiếu bản dịch tiếng Anh của nhãn hoặc tên danh mục (cảnh báo);
  - sinh viên gửi loại yêu cầu chưa ánh xạ phòng ban (lỗi cấu hình);
  - nạp danh mục thất bại (ghi rõ loại yêu cầu bị lỗi);
  - xóa bù tệp thất bại (ghi mã tham chiếu để tác vụ dọn tệp xử lý lại);
  - tệp không còn trên kho khi tải;
  - ghi kết quả đọc tệp lỗi sau khi đã bắt đầu trả tệp (cảnh báo);
  - lưu thu hồi phiên lỗi khi đăng xuất;
  - không ghi được nhật ký đăng nhập thất bại;
  - vi phạm unique bất thường khi sinh mã hồ sơ.

## Nhật ký hệ thống

- Lưu trong CSDL, chỉ thêm mới, thời điểm UTC; khi truy vấn hiển thị giờ Việt Nam.
- Các thao tác được ghi: xem `architecture/security-design.md`.
- Không có màn hình xem. Người vận hành truy vấn CSDL theo quyền vận hành.
