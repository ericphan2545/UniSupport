# Giả định

Đây là nơi gốc của giả định. Proposal (mục 1) và báo giá ([quotation.xlsx](../management/quotation.xlsx) sheet Giả định) chỉ dẫn lại mã `A-xx`. Giả định nào được trường xác nhận khác đi thì xử lý như một thay đổi theo [Kế hoạch quản trị và giám sát](../management/03-quan-tri-giam-sat.md) mục 5: đánh giá lại effort, giá và mốc, rồi báo giá bổ sung nếu cần.

| Mã | Giả định | Nếu sai thì ảnh hưởng tới giá hoặc kế hoạch | Trạng thái |
|---|---|---|---|
| `A-01` | Quy mô nhỏ: khoảng 3.000 sinh viên đăng ký, 100–200 người dùng đồng thời | Quy mô lớn hơn cần tối ưu hiệu năng và test tải ngoài phạm vi; báo giá bổ sung | Từ Proposal |
| `A-02` | Hiệu năng phụ thuộc hạ tầng do trường cung cấp | Hạ tầng không đáp ứng `NFR-PERF-01` đến `NFR-PERF-03` thì trường nâng hạ tầng; công điều chỉnh triển khai ngoài phạm vi báo giá | Từ Proposal |
| `A-03` | Thời gian 15 tuần không tính thời gian chờ phản hồi của trường | Trường phản hồi chậm thì các mốc sau dời tương ứng, không đổi giá | Từ Proposal |
| `A-04` | Không yêu cầu tuân thủ chuẩn bảo mật chuyên sâu; bảo vệ dữ liệu sinh viên ở mức cơ bản | Yêu cầu chuẩn bảo mật chuyên sâu là phạm vi mới, báo giá riêng | Từ Proposal |
| `A-05` | SSO của trường dùng luồng authorization code (OIDC) và trả email trường đã xác thực làm định danh | Phải sửa luồng đăng nhập và tích hợp SSO; đánh giá lại effort đăng nhập và triển khai | Chờ trường xác nhận khi chốt yêu cầu ở `MS-01` |
| `A-06` | SSO trả được họ tên và MSSV; khoa, lớp, email, số điện thoại có thể thiếu | Thiếu họ tên hoặc MSSV thì phải thêm cách lấy thông tin sinh viên khác | Chờ trường xác nhận ở `MS-01` |
| `A-07` | Mỗi loại yêu cầu do đúng một phòng ban tiếp nhận | Phải đổi mô hình danh mục và luồng gán phòng | Chờ trường xác nhận ở `MS-01` |
| `A-08` | SLA tính theo giờ lịch (cả ngoài giờ hành chính và ngày nghỉ); thời gian chờ sinh viên bổ sung không tính vào SLA | Phải đổi cách tính hạn và báo cáo thời gian xử lý | Chờ trường xác nhận ở `MS-01` |
| `A-09` | Giới hạn tệp: PDF/JPG/PNG, tối đa 5 tệp mỗi lần, 10 MiB mỗi tệp | Phải đổi cấu hình tệp và kiểm thử | Chờ trường xác nhận ở `MS-01` |
| `A-10` | Danh mục loại yêu cầu, phòng ban và SLA ban đầu do đội phát triển nạp khi triển khai; không có màn hình sửa danh mục | Phải thêm chức năng quản lý danh mục, là phạm vi mới | Chờ trường xác nhận ở `MS-01` |
| `A-11` | Trường cung cấp server chạy được Docker Compose, kho lưu tệp và chứng chỉ SSL | Phải đổi cách đóng gói và triển khai | Chờ trường xác nhận trước `MS-06` |
| `A-12` | Không cần đọc nhật ký hệ thống qua giao diện; người vận hành truy vấn CSDL | Phải thêm màn hình nhật ký và phân quyền đọc, là phạm vi mới | Chờ trường xác nhận ở `MS-01` |
| `A-13` | Phạm vi chốt ở `MS-01` gồm 42 FR trong `modules/`; sau đó chỉ có các lần sửa nhỏ theo Proposal mục 4 | Thêm hoặc đổi FR sau khi chốt là change request, báo giá và lên lịch riêng | Từ Proposal |
