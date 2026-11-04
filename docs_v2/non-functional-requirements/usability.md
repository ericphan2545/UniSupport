# Khả năng sử dụng

Nhiều sinh viên chủ yếu dùng điện thoại; có sinh viên và nhân viên không nói tiếng Việt.

## Song ngữ VN/EN

- Mặc định tiếng Việt. Trước khi đăng nhập dùng lựa chọn của phiên trình duyệt; sau khi đăng nhập dùng lựa chọn lưu trên tài khoản.
- Được dịch: nhãn giao diện, tên loại yêu cầu, phòng ban, trạng thái, mức ưu tiên, thông báo trong app, thông báo lỗi. Thông báo cũ hiển thị theo ngôn ngữ đang chọn.
- Không dịch nội dung người dùng nhập (mô tả, ghi chú, nhận xét, kết quả xử lý).
- Thiếu bản dịch tiếng Anh thì hiện bản tiếng Việt, không hiện mã khóa thô.
- Ngày giờ dạng `dd/MM/yyyy HH:mm` theo giờ Việt Nam ở cả hai ngôn ngữ.
- Đổi ngôn ngữ không làm mất nội dung đang nhập và tệp đã chọn.

## Responsive

- Hỗ trợ từ độ rộng 360px. Bố cục: điện thoại dưới 768px, máy tính bảng 768–1279px, máy tính từ 1280px.
- Ở 360px không có thanh cuộn ngang, không cắt chữ hoặc nút; bảng chuyển sang dạng thẻ.
- Vùng bấm tối thiểu 44×44px.
- Mọi chức năng của các vai trò dùng được trên điện thoại; ưu tiên tối ưu bố cục Cổng Sinh viên.
- Trên điện thoại, đính kèm được ảnh từ thư viện hoặc chụp bằng camera.
- Xoay màn hình khi đang điền form không làm mất dữ liệu.

## Trình duyệt

- 2 phiên bản mới nhất của Chrome, Safari, Edge và Firefox; ghi lại phiên bản đã test.
- Trình duyệt ngoài danh sách vẫn dùng được, có thông báo khuyến nghị đổi trình duyệt.

## Thông báo lỗi

- Thông báo lỗi theo ngôn ngữ đang chọn; lỗi nhập liệu hiện theo từng trường.
- Hành vi khi lỗi đọc hoặc gửi: xem [reliability.md](reliability.md).

## Usability test

- Người tham gia thực hiện các tác vụ chính của từng vai trò, theo phạm vi 3 bộ hướng dẫn sử dụng trong Proposal 6.1:
  - Sinh viên: tạo yêu cầu, đính kèm minh chứng, theo dõi tiến độ, đánh giá.
  - Nhân viên: nhận hàng việc, phân loại, chuyển tiếp, cập nhật trạng thái.
  - Admin: quản lý phân quyền, thiết lập SLA, xem báo cáo.
- Mục tiêu nội bộ (QMP): ≥ 80% người tham gia hoàn thành các tác vụ chính, hoặc theo Usability Test Report.
- Kết quả ghi trong Usability Test Report, hạng mục bàn giao theo Proposal 6.1. Số người tham gia và kịch bản chi tiết do người phụ trách usability test chốt trong báo cáo.
- Dùng dữ liệu giả, không dùng dữ liệu sinh viên thật.
