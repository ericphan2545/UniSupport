# Tổng quan sản phẩm

Phần mở đầu của PRD: vì sao làm UniSupport, làm cho ai, đo thành công bằng gì và không làm gì. Đây là nơi gốc của vấn đề, actor, goals, non-goals và thang ưu tiên FR. Proposal mục 1–2 nêu cùng nội dung cho khách; chức năng chi tiết nằm trong [`modules/`](../modules/), yêu cầu phi chức năng trong [`non-functional-requirements/`](../non-functional-requirements/README.md).

Khách hàng Aurora University là **Mô phỏng**; hiện trạng dưới đây là bối cảnh nhóm giả định để lập dự án.

## Vấn đề

Aurora University có khoảng 3.000 sinh viên. Hiện trạng (Mô phỏng):

- Sinh viên gửi yêu cầu hỗ trợ qua nhiều kênh rời rạc (email, tin nhắn, đến trực tiếp phòng ban). Yêu cầu không có mã hồ sơ, nên sinh viên không tự biết yêu cầu đang ở đâu và phải hỏi lại nhiều lần.
- Phòng ban không có hàng đợi chung: hồ sơ dễ thất lạc, không rõ ai đang phụ trách, không có hạn xử lý nên không biết hồ sơ nào đã trễ. Hồ sơ gửi nhầm phòng phải chuyển tay.
- Ban quản lý (Director of Student Services) không có số liệu về số lượng yêu cầu, thời gian xử lý, khối lượng từng phòng và mức hài lòng của sinh viên, nên khó điều phối nhân sự và cải thiện dịch vụ.

## Actor

| Actor | Là ai | Cần gì từ hệ thống |
|---|---|---|
| Sinh viên (`STUDENT`) | Sinh viên của trường, đăng nhập bằng tài khoản trường; nhiều người dùng chủ yếu điện thoại | Gửi yêu cầu kèm minh chứng đến đúng phòng, có mã hồ sơ, theo dõi tiến độ, bổ sung khi được yêu cầu, nhận kết quả và đánh giá |
| Nhân viên phòng ban (`STAFF`) | Cán bộ tiếp nhận và xử lý của một phòng ban | Hàng đợi của phòng, thông tin sinh viên và tệp, phân công, ghi nhật ký, yêu cầu bổ sung, đặt ưu tiên, chuyển phòng, đóng hồ sơ kèm kết quả |
| Admin (`ADMIN`) | Ban quản lý phía trường, gồm Director of Student Services | Dashboard và báo cáo để điều phối; cấp và thu quyền nhân viên; đặt SLA cho từng loại yêu cầu |
| SSO của trường | Hệ thống đăng nhập tập trung của trường (hệ thống ngoài) | Xác thực người dùng và cung cấp thông tin tài khoản |
| Người vận hành phía trường | Bộ phận kỹ thuật của trường sau bàn giao | Triển khai, sao lưu, tra cứu nhật ký hệ thống bằng truy vấn CSDL |

## Goals

| Mã | Mục tiêu | Cách đo | Ngưỡng đạt |
|---|---|---|---|
| `G-01` | Mọi yêu cầu đi qua một kênh, có mã hồ sơ, đến đúng phòng tiếp nhận và có hạn xử lý ngay khi tạo | Kịch bản `TS-02` và `TS-06` trên Staging | 100% hồ sơ tạo trong kịch bản có mã, phòng tiếp nhận và hạn xử lý; hồ sơ quá hạn được đánh dấu trong tối đa 60 giây |
| `G-02` | Sinh viên tự theo dõi tiến độ, không phải hỏi lại phòng ban | Usability test với sinh viên (`NFR-USE-01`) | ≥ 80% người tham gia tự tìm được trạng thái và kết quả hồ sơ |
| `G-03` | Ban quản lý có số liệu tin được để điều phối | Đối chiếu dashboard và 3 báo cáo với bộ 200 ticket mẫu (`TS-08`) | Khớp 100% bảng kết quả tính sẵn |
| `G-04` | Dùng được trên điện thoại và máy tính, tiếng Việt và tiếng Anh | Usability test và kịch bản `TS-15` | ≥ 80% người tham gia hoàn thành tác vụ chính; không lỗi bố cục ở độ rộng 360px |
| `G-05` | Chạy ổn ở quy mô của trường | Test hiệu năng `TS-19` trên hạ tầng trường | Đạt `NFR-PERF-01` đến `NFR-PERF-03` |

Sau bàn giao, giá trị sử dụng được theo dõi bằng các chỉ số ở [Kế hoạch quản lý sản phẩm](../management/01-quan-ly-san-pham.md) mục 6.

## Non-goals

Dự án **không** làm những việc sau. Proposal mục 2.2 nêu cùng danh sách. Yêu cầu thuộc danh sách này xử lý như change request ([03-quan-tri-giam-sat.md](../management/03-quan-tri-giam-sat.md) mục 5).

| Mã | Không làm |
|---|---|
| `NG-01` | Tích hợp với hệ thống hiện có của trường (SIS, LMS, thanh toán); tự động ra quyết định học vụ |
| `NG-02` | Ứng dụng di động riêng (iOS, Android); chỉ làm web responsive |
| `NG-03` | Gửi thông báo qua email, SMS hoặc push; chỉ có thông báo trong ứng dụng |
| `NG-04` | Mở lại hồ sơ đã đóng hoặc để sinh viên hủy hồ sơ đã gửi |
| `NG-05` | Tự phân loại yêu cầu bằng AI hoặc từ khóa; sinh viên chọn loại yêu cầu và hệ thống hiển thị phòng tiếp nhận |
| `NG-06` | Admin xem chi tiết hồ sơ, thông tin cá nhân, tệp của sinh viên hoặc xử lý hồ sơ |
| `NG-07` | Trích xuất dữ liệu, xuất báo cáo ra tệp |
| `NG-08` | Màn hình thêm, sửa, xóa danh mục loại yêu cầu và phòng ban (đội phát triển nạp dữ liệu khi triển khai) |
| `NG-09` | Màn hình xem nhật ký hệ thống; chức năng xóa dữ liệu |
| `NG-10` | Tuân thủ chuẩn bảo mật chuyên sâu; dự án bảo vệ dữ liệu sinh viên ở mức cơ bản |
| `NG-11` | Vận hành hệ thống sau bàn giao (quản lý server, sao lưu, cấp tài khoản nội bộ) |

## Thang ưu tiên FR

Mỗi FR trong `modules/*/prd.md` có mục **Priority** theo thang dưới đây.

| Mức | Nghĩa là | Khi thiếu giờ hoặc có CR chen vào |
|---|---|---|
| `Must` | Thiếu thì luồng chính gửi → xử lý → trả kết quả không chạy được, hoặc dữ liệu sinh viên không được bảo vệ | Không dời |
| `Should` | Thuộc phạm vi hợp đồng, có giá trị rõ cho người dùng; luồng chính vẫn chạy nếu làm muộn hơn | Có thể đề xuất dời sang milestone sau qua change request, khách duyệt |
| `Could` | Tiện ích bổ sung, đã có cách khác để đạt cùng kết quả | Dời đầu tiên |

BA / Mid đề xuất mức, PM / Senior duyệt, khách xác nhận khi chốt yêu cầu ở `MS-01`. Đổi mức sau khi chốt đi qua change request. Người giữ PRD trong ban quản lý: Ngô Tấn Đức. Thứ tự milestone theo phụ thuộc của luồng nghiệp vụ; cách dùng mức ưu tiên trong kế hoạch: [master-plan.md](../management/master-plan.md).
