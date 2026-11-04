**Project Scope – UniSupport – Student Support Management System**

_Khách hàng: Aurora University – Director of Student Services \| Quy mô: 3.000 sinh viên_

**1. Mục tiêu**

Xây dựng một web application cho phép:

- **Sinh viên** đăng nhập và gửi yêu cầu, đính kèm tài liệu, theo dõi trạng thái
- **Nhân viên** đăng nhập, tiếp nhận, phân công và xử lý yêu cầu được giao
- **Ban quản lý** đăng nhập, theo dõi và đánh giá chất lượng xử lý qua báo cáo

_(Chi tiết về dữ liệu và luồng sử dụng sẽ được làm rõ và chốt trong giai đoạn prototype)_

**Assumptions:**

- Hệ thống hướng đến quy mô sử dụng nhỏ (khoảng 3.000 sinh viên đăng ký, khoảng 100-200 người dùng đồng thời)
- Hiệu năng phụ thuộc vào hạ tầng do phía trường cung cấp
- Thời gian triển khai không bao gồm thời gian chờ phản hồi từ phía trường
- Không yêu cầu tuân thủ chuẩn bảo mật chuyên sâu, chỉ bảo vệ dữ liệu sinh viên ở mức cơ bản

**2. Phạm vi chức năng**

### **2.1. Phạm vi chức năng triển khai (In Scope)**

#### **2.1.1. Cổng Nhân viên tiếp nhận & Xử lý (Staff Portal)**

- **Danh sách yêu cầu:** Hiển thị hàng đợi hồ sơ sinh viên gửi lên hệ thống (phân loại theo trạng thái: Mới tiếp nhận / Đang xử lý / Quá hạn).
- **Chi tiết hồ sơ:**
  - Xem thông tin sinh viên và xem bộ sưu tập tệp tài liệu, minh chứng đính kèm.
  - Ghi nhận nhật ký xử lý, gửi yêu cầu sinh viên bổ sung thông tin hoặc đóng hồ sơ khi hoàn tất.
  - Cập nhật trạng thái xử lý của hồ sơ.
- **Phân loại & Điều phối:** Chỉ định nhân viên phụ trách, thiết lập mức độ ưu tiên, chuyển tiếp phòng ban hoặc đẩy mức độ ưu tiên của hồ sơ khi cần thiết.

#### **2.1.2. Cổng Sinh viên (Student Portal)**

- **Thông tin sinh viên:** Kế thừa và hiển thị thông tin cơ bản từ tài khoản trường cấp (Họ tên, MSSV, khoa/lớp, email, số điện thoại liên hệ).
- **Gửi yêu cầu hỗ trợ:**
  - Biểu mẫu nhập mô tả vấn đề kèm gợi ý phân loại đúng phòng ban tiếp nhận.
  - Tải lên tệp tài liệu, minh chứng liên quan và tự động cấp mã định danh hồ sơ duy nhất.
- **Theo dõi & Tương tác:** Tra cứu tiến độ xử lý theo thời gian thực, nhận thông báo cập nhật và gửi đánh giá chất lượng hỗ trợ.

#### **2.1.3. Cổng Quản lý & Báo cáo (Management Portal)**

- **Bảng điều khiển (Dashboard):** Theo dõi các chỉ số vận hành theo thời gian thực (số lượng yêu cầu mới, đang làm, quá hạn và thời gian xử lý trung bình).
- **Báo cáo phân tích:**
  - Phân tích các nhóm vấn đề và loại yêu cầu phát sinh phổ biến.
  - Đo lường khối lượng công việc của từng phòng ban phục vụ điều phối nhân sự.
- **Đánh giá chất lượng:** Theo dõi điểm số và mức độ hài lòng của sinh viên đối với dịch vụ hỗ trợ.

#### **2.1.4. Quản lý Định danh & Bảo mật (Authentication & Security)**

- Đăng nhập / Đăng xuất hệ thống bảo mật bằng tài khoản định danh do nhà trường cấp.
- Cơ chế phân quyền phân định rõ 3 vai trò cốt lõi: **Sinh viên**, **Nhân viên xử lý** và **Ban quản lý**.
- Kiểm soát bảo mật: Giới hạn phạm vi truy cập dữ liệu sinh viên, bảo vệ an toàn cho các tệp đính kèm và ghi nhận nhật ký hệ thống.

### **2.2. Phạm vi ngoài chức năng triển khai (Out of Scope)**

| **Các hạng mục sau không nằm trong phạm vi hiện tại:**                                                                                                            |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tích hợp & Dữ liệu:** Không tích hợp với hệ thống hiện có của trường (SIS/LMS, thanh toán); không tự động ra quyết định học vụ.                                 |
| **Không phát triển ứng dụng di động độc lập (Native App trên iOS/Android)**; toàn bộ trải nghiệm tập trung tối ưu trên nền tảng Web đa thiết bị (Responsive Web). |

_Các mục trên có thể đưa vào future roadmap ở phase sau nếu báo giá bổ sung._

**3. Thời gian và quy trình thực hiện**

- Dự kiến: 15 tuần và không bao gồm thời gian chờ feedback
- **Phạm vi công việc:** Bao gồm trọn gói các giai đoạn: Thiết kế -> Phát triển -> Kiểm thử chất lượng -> Triển khai cài đặt -> Đóng gói tài liệu & Bàn giao.

**Dự án triển khai theo 2 giai đoạn chính:**

**Giai đoạn 1: Prototype & xác nhận yêu cầu**

- Xây dựng một bản prototype (UI chạy với dữ liệu mẫu / giả lập)
- Prototype thể hiện:
  - Luồng sử dụng chính (student / staff / manager)
  - Cấu trúc dữ liệu cơ bản
  - Giao diện ở mức cơ bản (mobile + desktop, VN / EN, đơn giản)
- Khách hàng sẽ:
  - Trải nghiệm trực tiếp trên prototype
  - Đưa ra feedback về:
    - Giao diện
    - Luồng sử dụng
    - Dữ liệu cần thu thập
- Kết quả: chốt scope + quality criteria tại Week 7: Intermediate Project Review

**Giai đoạn 2: Phát triển & triển khai**

- Sau khi prototype được xác nhận:
  - Tiến hành phát triển hệ thống chính thức (vẫn dùng dữ liệu giả lập)
  - Hoàn thiện chức năng
  - Test và deploy lên môi trường thực tế

**Lưu ý quan trọng**

- Prototype là bước để xác định và chốt phạm vi công việc cuối cùng
- Các thay đổi nhỏ trong giai đoạn này được hỗ trợ bình thường
- Tuy nhiên, nếu có yêu cầu:
  - Thay đổi lớn về luồng
  - Bổ sung nhiều chức năng mới
  - Hoặc mở rộng phạm vi so với ban đầu, yêu cầu mâu thuẫn từ stakeholder

  _Khi đó Phạm vi công việc sẽ được cập nhật lại và báo giá bổ sung tương ứng để tiếp tục phát triển._

**4. Điều kiện & ghi chú**

- Bao gồm 2–5 lần chỉnh sửa nhỏ trong phạm vi đã thống nhất. Sửa nhỏ là: sửa chữ, màu, label VN/EN, layout nhẹ, không đổi luồng, không thêm màn / tính năng.
- Các yêu cầu phát sinh ngoài scope sẽ được:
  - báo giá bổ sung effort
  - hoặc đưa vào phase tiếp theo
- Mọi thay đổi chốt bằng văn bản. Nhóm 4 người, rõ vai trò, track planned / actual effort từng người.

**5. Chi phí / Effort**

**5.1. Tổng chi phí dự án**

- **Tổng chi phí** : 332.316.000 VNĐ (Ba trăm ba mươi hai triệu ba trăm mười sáu nghìn đồng) (giá đã bao gồm 10% VAT) _(dự kiến, có thể thay đổi theo phạm vi chốt ở Giai đoạn 1)_

**5.2. Tiến độ thanh toán đề xuất _(Bổ sung chi tiết)_**

- **Đợt 1 (30% – tương đương 99.694.800 VNĐ):** Tạm ứng triển khai trong vòng 05 ngày làm việc sau khi hai bên ký kết Hợp đồng dịch vụ và Bên A nhận được đề nghị thanh toán.
- **Đợt 2 (40% – tương đương 132.926.400 VNĐ):** Thanh toán trong vòng 07 ngày làm việc kể từ ngày hoàn tất nghiệm thu Giai đoạn 1 (phê duyệt bộ tài liệu phân tích kỹ thuật, sơ đồ luồng dữ liệu và thiết kế giao diện tương tác UI/UX).
- **Đợt 3 (30% – tương đương 99.694.800 VNĐ):** Quyết toán trong vòng 10 ngày làm việc sau khi hệ thống Go-live thành công trên môi trường thực tế, bàn giao đầy đủ mã nguồn/tài liệu vận hành và hai bên ký kết Biên bản nghiệm thu tổng thể.

**_Lưu ý:_ Mỗi đợt thanh toán đều đi kèm Biên bản nghiệm thu giai đoạn tương ứng và Hóa đơn Giá trị gia tăng (VAT) hợp lệ theo quy định pháp luật.**

# **6. Quy chế bàn giao, nghiệm thu & dịch vụ sau triển khai**

## **6.1. Hạng mục bàn giao (Deliverables)**

Phía phát triển sẽ bàn giao cho Aurora University các hạng mục sau:

- **Bộ mã nguồn hoàn chỉnh của ứng dụng:**
  - Mã nguồn giao diện người dùng (Frontend) và logic máy chủ (Backend).
  - Kịch bản đóng gói và triển khai tự động (Docker Compose, file cấu hình môi trường mẫu .env.example).
- **Triển khai và cài đặt hệ thống:**
  - Hỗ trợ thiết lập và chạy thành công phiên bản ứng dụng lên máy chủ/hạ tầng do Nhà trường chỉ định.
- **Tài liệu hướng dẫn sử dụng cơ bản:**
  - Hướng dẫn dành cho Sinh viên (tạo yêu cầu, đính kèm minh chứng, theo dõi tiến độ, đánh giá).
  - Hướng dẫn dành cho Cán bộ tiếp nhận & Xử lý (nhận hàng việc, phân loại, chuyển tiếp, cập nhật trạng thái).
  - Hướng dẫn dành cho Ban quản lý / Quản trị viên (quản lý phân quyền, thiết lập SLA danh mục, xem báo cáo KPI).
- **Tài liệu kỹ thuật / API:**
  - Danh mục các điểm cuối (Endpoints) chuẩn hóa theo chuẩn OpenAPI/Swagger.
  - Tài liệu hướng dẫn tích hợp và quy định mã lỗi hệ thống.
  - API Key và cấu hình xác thực bảo mật tương ứng (nếu có tích hợp).
- **Dữ liệu mẫu & Hồ sơ kiểm thử:**
  - Bộ dữ liệu giả định chuẩn hóa 200 ticket mẫu) phục vụ nghiệm thu và đào tạo.
  - Biên bản kết quả kiểm thử chức năng và đánh giá tính khả dụng (Usability Test Report).

## **6.2. Nghiệm thu (Acceptance Testing)**

Khách hàng sẽ kiểm tra hệ thống dựa trên các chức năng đã thống nhất và chốt thực tế sau "Giai đoạn 1: Prototype & xác nhận yêu cầu":

- **Thời gian nghiệm thu:** Từ 3 đến 5 ngày làm việc kể từ ngày bàn giao môi trường thử nghiệm.
- **Kết quả:** Nếu hệ thống đáp ứng đúng các tiêu chí chấp thuận (Acceptance Criteria) đã chốt bằng văn bản, đại diện có thẩm quyền của hai bên tiến hành ký Biên bản xác nhận hoàn thành dự án.

## **6.3. Bảo hành & Hỗ trợ (Warranty & Support)**

- **Thời gian bảo hành:** Hỗ trợ sửa lỗi (fix bug) trong vòng 1 tháng sau khi ký Biên bản nghiệm thu chính thức.
- **Phạm vi bảo hành:**
  - Khắc phục toàn bộ các lỗi phát sinh (Bugs), lỗi sai lệch logic nghiệp vụ bắt nguồn từ mã nguồn của hệ thống đã xây dựng theo tài liệu thống nhất.
  - Hướng dẫn xử lý sự cố kỹ thuật qua email hoặc kênh liên lạc chính thức.
- **Phạm vi KHÔNG bao gồm trong bảo hành:**
  - Yêu cầu thay đổi luồng nghiệp vụ hoặc phát triển thêm tính năng mới nằm ngoài Scope ban đầu (sẽ được tách thành Gói Yêu cầu Thay đổi - Change Request riêng).
  - Lỗi phát sinh do tự ý can thiệp, chỉnh sửa trực tiếp vào mã nguồn hoặc cơ sở dữ liệu mà không có sự phê duyệt của phía phát triển.
  - Hỗ trợ các công việc liên quan đến nghiệp vụ vận hành hành chính hoặc nhập liệu hàng ngày của cán bộ nhà trường.

## **6.4. Hạ tầng & Chi phí vận hành (Infrastructure & Operations)**

- **Chi phí hạ tầng:** Chi phí mua và duy trì Server / Cloud VPS, Storage lưu trữ tài liệu đính kèm, Tên miền (Domain), Chứng chỉ bảo mật SSL... do phía Aurora University trực tiếp chi trả cho bên cung cấp dịch vụ.
- **Hỗ trợ kỹ thuật ban đầu:** Bên phát triển hỗ trợ cấu hình hạ tầng ban đầu trong quá trình triển khai hệ thống lên server của trường.
- **Trách nhiệm sau bàn giao:** Phía Nhà trường hoàn toàn chịu trách nhiệm vận hành hệ thống sau khi bàn giao, bao gồm: quản lý server, cấp phát tài khoản người dùng nội bộ, sao lưu dữ liệu định kỳ và bảo đảm an toàn thông tin nội bộ.
- **Giới hạn trách nhiệm:** Bên phát triển không chịu trách nhiệm cho các sự cố hoặc vấn đề phát sinh từ quá trình vận hành nội bộ, lỗi nhà cung cấp hạ tầng/mạng bên thứ ba, hoặc do thao tác sai lệch của người dùng phía Nhà trường.
