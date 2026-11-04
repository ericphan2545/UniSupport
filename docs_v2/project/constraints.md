# Ràng buộc

Những điều cố định mà dự án phải tuân theo.

## Hợp đồng (Proposal)

| # | Ràng buộc |
|---|---|
| C1 | Giá trị hợp đồng 332.316.000 VNĐ (đã gồm VAT), thanh toán 3 đợt |
| C2 | Thời gian dự kiến 15 tuần, không tính thời gian chờ phản hồi của trường |
| C3 | Chỉ làm web responsive cho điện thoại và máy tính, có VN/EN; không làm ứng dụng di động riêng |
| C4 | Không tích hợp SIS/LMS, thanh toán; không tự động ra quyết định học vụ |
| C5 | Sửa nhỏ tối đa 2–5 lần trong phạm vi đã thống nhất; thay đổi lớn phải báo giá và được khách xác nhận bằng văn bản |
| C6 | Hạng mục bàn giao theo Proposal 6.1: mã nguồn, Docker Compose và `.env.example`, triển khai lên server trường, 3 bộ hướng dẫn sử dụng, OpenAPI và mã lỗi, bộ 200 ticket mẫu, báo cáo kiểm thử và Usability Test Report |
| C7 | Trường chi trả hạ tầng (server, lưu trữ, tên miền, SSL) và tự vận hành sau bàn giao |
| C8 | Bảo hành sửa lỗi 1 tháng; yêu cầu tính năng mới không thuộc bảo hành |

## Nội bộ nhóm (Team Charter)

| # | Ràng buộc |
|---|---|
| C9 | Tổng 870 giờ công |
| C10 | Ít nhất 95% test case đạt, không còn lỗi Critical, không có lỗi chặn khi chạy 100–200 người dùng đồng thời trên hạ tầng trường |
| C11 | Mỗi người tối đa 2 task cùng lúc; code chỉ merge sau khi có người khác review |

## Kỹ thuật

| # | Ràng buộc |
|---|---|
| C12 | Chỉ 3 vai trò: Sinh viên, Nhân viên, Admin; mỗi tài khoản đúng một vai trò |
| C13 | Admin không xem chi tiết hồ sơ, thông tin cá nhân sinh viên hay tệp |
| C14 | Thông báo chỉ trong ứng dụng; không email, SMS, push |
| C15 | Chỉ phục vụ HTTPS; SSO giả lập không được bật ở production |
| C16 | Thời gian lưu UTC, hiển thị giờ Việt Nam (UTC+7) |
