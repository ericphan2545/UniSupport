# UniSupport – Kế hoạch quản trị và giám sát dự án

Phiên bản 0.5

## Mục đích và phạm vi

Kế hoạch này quy định cách báo cáo tình hình dự án từ người thực hiện lên PM, rồi lên Aurora University (khách hàng). Kế hoạch trả lời bốn câu hỏi: ai báo cáo cho ai, báo cáo khi nào, báo cáo những gì, và khi có vấn đề thì xử lý ra sao. Mục tiêu là để PM luôn biết dự án đang đi đúng hay lệch kế hoạch, từ đó quyết định kịp thời.

Kế hoạch dựa trên vai trò và quy tắc làm việc trong Team Charter, due date của milestone trên GitHub, kế hoạch 870 giờ công (Team Charter) và các cam kết trong Proposal. Nội dung sản phẩm xem [01](01-quan-ly-san-pham.md); tiêu chí chất lượng xem [02](02-qmp-quan-ly-chat-luong.md).

## 1. Cơ cấu quản trị và tuyến báo cáo

Dự án có ba cấp báo cáo. Thành viên báo cáo cho người phụ trách mảng của mình; người phụ trách các mảng báo cáo cho PM; PM tổng hợp và báo cáo cho Aurora University. Vai trò theo Team Charter mục 3, dùng tên Senior / Mid / Junior.

### 1.1 Trách nhiệm báo cáo theo từng cấp

| Cấp | Vai trò | Báo cáo cho | Chịu trách nhiệm báo cáo về |
|---|---|---|---|
| Cấp 1 – Thực hiện | BE Dev / Mid | BE Dev / Senior | Tiến độ từng công việc được giao, số giờ đã làm, vướng mắc kỹ thuật |
| | FE Dev / Junior | FE Dev / Senior | Như trên |
| | DevOps / Mid | BE Dev / Senior | Như trên, kèm tình trạng môi trường (Staging, triển khai) |
| | Tester / Junior | QA / Senior | Kết quả chạy test case, lỗi phát hiện, kết quả retest |
| Cấp 2 – Phụ trách mảng | BE Dev / Senior | PM | Tiến độ phát triển backend, mã đang chờ review, vấn đề và rủi ro kỹ thuật |
| | FE Dev / Senior | PM | Tiến độ phát triển frontend, mã đang chờ review, vấn đề và rủi ro kỹ thuật |
| | BA / Mid | PM | Câu hỏi về yêu cầu chưa được giải đáp, yêu cầu thay đổi |
| | QA / Senior | PM | Kết quả kiểm thử, số lỗi đang mở, kết luận chất lượng của milestone (theo QMP) |
| | UI/UX / Mid | PM | Tiến độ thiết kế và các phản hồi về giao diện |
| Cấp 3 – Quản lý dự án | PM / Senior | Aurora University | Kết quả từng milestone, rủi ro ảnh hưởng đến khách hàng, nội dung cần khách xác nhận |

QA / Senior báo cáo trực tiếp cho PM, không qua BE / FE Senior, để kết quả kiểm thử được đánh giá độc lập với nhóm phát triển. Khi PM vắng mặt, BA / Mid tạm thời nhận báo cáo và điều phối (Team Charter mục 3). Nhiệm vụ chi tiết của từng vai trò nằm trong Team Charter.

Dữ liệu gốc luôn mở cho cấp trên. Báo cáo được tổng hợp theo từng cấp, nhưng số liệu không bị lọc qua từng cấp. Mọi số liệu trong báo cáo đều dẫn về dữ liệu gốc: issue và milestone trên GitHub, bảng theo dõi giờ, các nhật ký ở mục 6.1. PM có quyền xem trực tiếp các dữ liệu này bất cứ lúc nào, không phải chờ đến kỳ báo cáo.

### 1.2 Luồng thông tin từ trên xuống

Báo cáo đi lên để cấp trên nắm tình hình; theo chiều ngược lại, thông tin được truyền xuống như sau:

1. **Tiếp nhận:** PM là đầu mối duy nhất nhận câu hỏi, yêu cầu và quyết định từ Aurora University (Team Charter mục 4 và 8), để các thành viên không nhận những chỉ đạo khác nhau.
2. **Truyền đạt:** PM ghi quyết định vào Decision Log và thông báo cho người phụ trách mảng liên quan; nội dung khẩn qua Discord, nội dung thông thường tại buổi họp tuần.
3. **Thực hiện và báo lại:** người phụ trách chuyển quyết định thành issue và phân công; người được giao xác nhận trong 1 ngày làm việc. PM báo lại kết quả cho khách trong kỳ báo cáo tiếp theo, khép kín vòng thông tin: báo cáo đi lên, quyết định đi xuống, kết quả được báo lại.

## 2. Quy trình báo cáo

Dự án dùng năm loại báo cáo, từ cập nhật hằng ngày đến báo cáo tổng hợp theo milestone.

| Loại báo cáo | Thời điểm | Người lập → người nhận | Hình thức |
|---|---|---|---|
| Cập nhật hằng ngày | Mỗi ngày làm việc, trước 21h30 | Từng thành viên → người phụ trách mảng và PM | Tin nhắn Discord kèm link issue; cập nhật issue trên GitHub |
| Báo cáo tuần | Trước buổi họp nhóm hằng tuần; bản tóm tắt gửi khách tối thiểu 1 lần/tuần | Người phụ trách mảng → PM; PM tổng hợp và gửi khách | Báo cáo viết, trình bày tại buổi họp; email cho khách |
| Báo cáo milestone | Khi đóng mỗi milestone | PM → Aurora University | Báo cáo viết, gửi email |
| Báo cáo cảnh báo | Ngay khi phát hiện vấn đề | Người phát hiện → người phụ trách trực tiếp → PM | Tin nhắn Discord gắn tên người nhận, sau đó ghi vào nhật ký vấn đề |
| Báo cáo tổng kết | Khi bàn giao | PM → nhóm; phần kết quả sản phẩm nằm trong hồ sơ bàn giao cho Aurora University | Báo cáo viết |

### 2.1 Cập nhật hằng ngày

Theo Team Charter (mục 5, bước 5), trước 21h30 mỗi ngày làm việc, mỗi thành viên gửi lên Discord: hôm qua làm gì, hôm nay làm gì, đang vướng gì, kèm link issue. Cùng lúc, thành viên cập nhật trạng thái issue trên GitHub và ghi số giờ thực tế cho từng công việc vào bảng theo dõi giờ (mục 6.1). Số giờ này là nguồn dữ liệu để PM tính giờ công thực tế ở mục 3.

Vướng mắc được PM ghi lại; người phụ trách mảng trao đổi riêng với người liên quan. Vấn đề nghiêm trọng báo theo hình thức cảnh báo ở mục 2.4.

### 2.2 Báo cáo tuần

Mỗi tuần, trước buổi họp nhóm (một ngày làm việc cố định do nhóm chốt), mỗi người phụ trách mảng gửi phần báo cáo của mình theo nội dung ở mục 1.1; riêng QA / Senior báo cáo các chỉ số chất lượng theo QMP mục 7. PM tổng hợp thành Báo cáo tình hình dự án tuần theo mẫu ở mục 6.2.

Tại buổi họp, nhóm rà soát rủi ro theo mục 3.2. Kết luận và việc cần làm ghi vào biên bản họp, có người phụ trách và thời hạn.

Bản tóm tắt gửi Aurora University qua email, tối thiểu 1 lần/tuần (Team Charter mục 7), gồm: tiến độ theo milestone, rủi ro chính, các quyết định cần khách chốt.

### 2.3 Báo cáo milestone

**Họp đầu và cuối milestone.** Khi mở milestone, nhóm họp lập kế hoạch: xác nhận chức năng cần làm, phân công, lịch, test case. Khi đóng milestone, nhóm họp demo trọn luồng, xem kết quả test, đóng milestone và rút kinh nghiệm. PM / Senior chủ trì; QA / Senior cùng chủ trì buổi đóng.

**Báo cáo gửi khách.** Khi đóng mỗi milestone, PM gửi Aurora University báo cáo gồm:

- Kết quả đạt được so với nội dung của milestone.
- Kết quả kiểm tra điều kiện đóng milestone (QMP mục 2.1).
- Tiến độ tổng thể và dự báo thời điểm Go-live.
- Số giờ công đã dùng so với kế hoạch.
- Rủi ro và vấn đề có thể ảnh hưởng đến khách hàng.
- Những nội dung cần Aurora University xác nhận hoặc cung cấp (thông tin nghiệp vụ, điều kiện hạ tầng, SSO, phản hồi).
- Kế hoạch của milestone tiếp theo.

### 2.4 Báo cáo cảnh báo

Các tình huống phải báo ngay, không chờ kỳ báo cáo tuần:

- Bị chặn quá nửa ngày (Team Charter mục 4).
- Dự kiến vượt 20% giờ kế hoạch của một task: báo PM trước khi làm tiếp (Team Charter mục 4).
- Có nguy cơ trễ hạn công việc hoặc milestone.
- Phát hiện lỗi Critical, hoặc Staging không ổn định.
- Xuất hiện rủi ro mới hoặc yêu cầu thay đổi phạm vi.
- Thông tin, phản hồi hoặc hạ tầng từ phía Aurora University bị chậm.

Báo cáo cảnh báo nêu rõ bốn điều: vấn đề là gì, ảnh hưởng đến công việc và milestone nào, đề xuất cách xử lý, cần ai quyết định. Gửi qua Discord; người nhận phản hồi trong 1 ngày làm việc. Nếu vấn đề ảnh hưởng đến phạm vi, mốc Go-live hoặc nghiệm thu đã thống nhất với khách, PM chủ động thông báo cho Aurora University.

### 2.5 Báo cáo tổng kết

Khi bàn giao, PM lập báo cáo tổng kết gồm: kết quả so với kế hoạch về phạm vi, tiến độ và giờ công; kết quả kiểm thử và nghiệm thu nội bộ (QMP mục 2.2); các vấn đề còn tồn đọng, gồm danh sách lỗi còn tồn; bài học kinh nghiệm của nhóm. Phần kết quả sản phẩm nằm trong hồ sơ bàn giao cho Aurora University (Proposal 6.1).

## 3. Chỉ số giám sát và ngưỡng cảnh báo

### 3.1 Ngưỡng cảnh báo

Mỗi báo cáo tuần và báo cáo milestone đánh giá bốn lĩnh vực theo ba mức: **Xanh** là đúng kế hoạch, **Vàng** là có sai lệch nhưng còn kiểm soát được, **Đỏ** là cần PM xử lý ngay. Trạng thái chung của dự án lấy theo lĩnh vực có mức xấu nhất.

| Lĩnh vực | Chỉ số và cách đo | Xanh | Vàng | Đỏ |
|---|---|---|---|---|
| Tiến độ | Tỷ lệ task xong đúng Target date ghi trong issue; dự báo ngày đóng milestone so với due date trên GitHub | Tỷ lệ đúng hạn ≥ 90% và milestone dự kiến đóng đúng due date | Tỷ lệ đúng hạn < 90%, hoặc dự báo trễ không quá 1 ngày làm việc | Dự báo trễ quá 1 ngày làm việc so với due date |
| Giờ công | Dự báo tổng giờ = giờ thực tế đã dùng + giờ kế hoạch của phần việc còn lại, so với 870 giờ (Team Charter) | Dự báo không vượt 870 giờ và không task nào lệch quá 20% | Dự báo không vượt 870 giờ, nhưng có task lệch quá 20% giờ kế hoạch (Team Charter) | Dự báo vượt 870 giờ |
| Chất lượng | Số lỗi Critical và Major đang mở; các chỉ số theo QMP mục 7 | Không có Critical hoặc Major đang mở; chỉ số đạt ngưỡng | Có Critical hoặc Major đang sửa trong hạn, hoặc chỉ số chưa đạt nhưng chưa đến lúc đóng milestone | Còn Critical hoặc Major khi đến lúc đóng milestone, hoặc Critical quá hạn sửa (QMP mục 6) |
| Phạm vi | Số change-request đang chờ xử lý; số lần sửa nhỏ đã dùng (tối đa 5 theo Proposal) | Không có CR đang mở và đã dùng dưới 4 lần sửa nhỏ | Có CR đang đánh giá, hoặc đã dùng 4 lần sửa nhỏ | CR chờ quá 3 ngày làm việc, hoặc thay đổi ảnh hưởng đến Go-live hay phạm vi đã thống nhất, cần khách xác nhận |

Chỉ số giờ công đo bằng giờ, không quy đổi ra tiền.

Hành động theo từng mức:

- **Xanh:** tiếp tục theo kế hoạch.
- **Vàng:** người phụ trách mảng đề xuất hướng khắc phục; PM theo dõi kết quả trong báo cáo tuần tiếp theo.
- **Đỏ:** PM xử lý ngay bằng cách điều chỉnh phân công hoặc thứ tự công việc. Nếu ảnh hưởng đến mốc, phạm vi đã cam kết với khách, PM thông báo Aurora University và xử lý theo quy trình kiểm soát thay đổi ở mục 5.

### 3.2 Giám sát rủi ro

Ngoài các vấn đề đã xảy ra, PM theo dõi những rủi ro có thể xảy ra trong sổ rủi ro (issue gắn label `risk` trên GitHub). Mỗi rủi ro ghi: mô tả, khả năng xảy ra (thấp, trung bình, cao), mức ảnh hưởng (thấp, trung bình, cao), người theo dõi, biện pháp phòng ngừa, dấu hiệu sắp xảy ra, hạn xử lý. Cột "Mức" bên dưới là mức tổng hợp từ khả năng và ảnh hưởng.

Tại buổi họp tuần, nhóm bổ sung rủi ro mới, cập nhật mức độ và đóng các rủi ro không còn khả năng xảy ra. Rủi ro mức Cao chưa có người phụ trách được báo theo mục 2.4. Rủi ro đã xảy ra chuyển sang nhật ký vấn đề và xử lý theo mục 4.

**Sổ rủi ro ban đầu:**

| Rủi ro | Mức | Cách xử lý | Phụ trách |
|---|---|---|---|
| Effort vượt 870 giờ vì mỗi milestone rất ngắn | Cao | Theo dõi giờ hằng tuần; báo cảnh báo khi dự báo vượt | PM / Senior |
| Trường dùng SAML thay vì OIDC | Cao | Xác nhận với trường sớm; nếu đúng là SAML thì tạo change-request | BE Dev / Senior |
| Chưa có danh sách loại yêu cầu và phòng ban thật | Trung bình | Dùng dữ liệu seed; phải có danh sách thật trước khi triển khai | PM / Senior |
| Các yêu cầu đã loại quay lại (Admin xử lý ticket, email/SMS, trích xuất dữ liệu) | Trung bình | Xử lý như change-request | PM / Senior |
| Usability test diễn ra sau feature freeze | Trung bình | Chỉ sửa Critical/Major; phần còn lại ghi vào danh sách lỗi tồn | QA / Senior |
| Hạ tầng của trường khác Staging | Trung bình | Test hiệu năng trên hạ tầng trường trước Go-live | DevOps / Mid |
| Chưa định nghĩa thời gian lưu trữ dữ liệu | Thấp | Trường xác nhận trước khi lên production | PM / Senior |

### 3.3 Chỉ số giá trị đạt được (EVM)

Cuối mỗi milestone, PM tính hai chỉ số theo phương pháp giá trị đạt được (Earned Value Management) để đánh giá cùng lúc tiến độ và giờ công, đơn vị là giờ công. **PV** là số giờ kế hoạch của phần việc lẽ ra phải xong; **EV** là số giờ kế hoạch của phần việc thực tế đã xong đúng DoD (QMP mục 5); **AC** (Actual Cost) là số giờ thực tế đã dùng. Giờ kế hoạch lấy từ masterplan và issue.

| Chỉ số | Công thức | Ý nghĩa |
|---|---|---|
| SPI | EV ÷ PV | Từ 1 trở lên: đúng hoặc nhanh hơn kế hoạch. Dưới 1: chậm so với kế hoạch |
| CPI | EV ÷ AC | Từ 1 trở lên: không vượt số giờ kế hoạch. Dưới 1: tốn nhiều giờ hơn kế hoạch |

Khi SPI hoặc CPI dưới 1, PM nêu nguyên nhân và hướng khắc phục trong báo cáo milestone.

## 4. Chuyển cấp vấn đề và quyền quyết định

Khi gặp vấn đề, thành viên liên hệ người phụ trách trực tiếp của mảng trước. Vấn đề chỉ được chuyển lên cấp tiếp theo khi người phụ trách trực tiếp không xử lý được, hoặc khi vấn đề ảnh hưởng đến tiến độ, phạm vi hay chất lượng. Mỗi cấp phản hồi trong 1 ngày làm việc; bất đồng quá 1 ngày thì PM quyết cuối (Team Charter mục 8). "Gặp vấn đề thì hỏi ai" xem Team Charter mục 6.

| Loại quyết định | Người quyết định |
|---|---|
| Vấn đề nội bộ: cách triển khai, kỹ thuật, phân công, thời hạn | Theo Team Charter mục 8: Senior của mảng (BE Dev / Senior, FE Dev / Senior) quyết kỹ thuật trong mảng; PM quyết khi bất đồng hoặc khẩn cấp |
| Chất lượng, mức độ lỗi | QA / Senior; PM / Senior duyệt trường hợp đặc biệt |
| Thứ tự sửa lỗi | PM / Senior |
| Sửa nhỏ (2–5 lần theo Proposal) | PM / Senior |
| Đóng milestone | PM / Senior, sau khi QA / Senior xác nhận |
| Go-live | PM / Senior, sau khi QA / Senior xác nhận điều kiện chất lượng |
| Thay đổi phạm vi, mốc Go-live hoặc báo giá đã thống nhất với khách | Aurora University (Director of Student Services) xác nhận bằng văn bản; PM đề xuất |
| Việc gấp cần quyết trong 24 giờ khi PM vắng | BE Dev / Senior quyết tạm, ghi lại để PM xem lại (Team Charter mục 8) |

Mọi quyết định quan trọng được ghi vào Decision Log để các thành viên tra cứu khi cần.

## 5. Kiểm soát thay đổi

Mọi thay đổi về phạm vi, tiến độ hoặc báo giá phải được đánh giá và xác nhận trước khi thực hiện, theo cam kết trong Proposal. Thay đổi mục tiêu hoặc ngưỡng chất lượng (QMP mục 8) cũng đi theo quy trình này. Quy trình gồm năm bước:

1. **Ghi nhận.** Ai cũng tạo được issue gắn label `change-request`, nêu nội dung và lý do. Yêu cầu từ khách chuyển về PM (Team Charter mục 4).
2. **Đánh giá ảnh hưởng.** Kiểm tra cổng bắt buộc và chấm tiêu chí theo [01](01-quan-ly-san-pham.md) mục 7. BE Dev / Senior và FE Dev / Senior ước giờ; QA / Senior đánh giá ảnh hưởng đến chất lượng; PM đánh giá ảnh hưởng đến tiến độ và lập báo giá cho khách (Proposal mục 4).
3. **Quyết định.** PM quyết định trong nội bộ. Nếu thay đổi ảnh hưởng đến phạm vi, mốc Go-live hoặc báo giá đã thống nhất với khách, PM gửi Aurora University xác nhận bằng văn bản trước khi thực hiện, hoặc đưa sang phase sau.
4. **Cập nhật kế hoạch.** Khi thay đổi được chấp thuận, BA / Mid cập nhật tài liệu liên quan; PM cập nhật milestone và issue trên GitHub. Quyết định được ghi vào Decision Log.
5. **Theo dõi.** Thay đổi được theo dõi trong báo cáo tuần cho đến khi hoàn thành.

Yêu cầu tính năng mới phát sinh trong thời gian bảo hành không thuộc phạm vi bảo hành (Proposal 6.3) và cũng được xử lý theo quy trình này.

## 6. Nhật ký quản trị và mẫu báo cáo

### 6.1 Nhật ký quản trị

| Nhật ký | Nội dung | Người cập nhật | Nơi lưu |
|---|---|---|---|
| Bảng theo dõi giờ | Ngày, người thực hiện, công việc (kèm link issue), số giờ | Từng thành viên; PM tổng hợp | Google Drive |
| Nhật ký vấn đề | Mô tả, ngày phát hiện, ảnh hưởng, người xử lý, thời hạn, trạng thái | Người phát hiện; PM theo dõi | GitHub, label `bug` hoặc `issue` |
| Sổ rủi ro | Như mục 3.2 | PM; người theo dõi từng rủi ro | GitHub, label `risk` |
| Yêu cầu thay đổi | Nội dung, lý do, đánh giá ảnh hưởng, quyết định | Người đề xuất; PM | GitHub, label `change-request` |
| Decision Log | Ngày, nội dung quyết định, người quyết định, lý do, tài liệu cần cập nhật | PM | Google Drive |
| Biên bản họp | Nội dung thảo luận, việc cần làm, người phụ trách, thời hạn | Thành viên ghi theo lượt | Google Drive |
| Báo cáo tuần, milestone, tổng kết | Theo mục 2 và mẫu ở mục 6.2 | PM | Google Drive |

Quyết định chốt trên Discord hoặc trong họp phải được cập nhật vào issue hoặc tài liệu liên quan ngay khi chốt (Team Charter mục 7).

### 6.2 Mẫu và ví dụ báo cáo tuần

Báo cáo tình hình dự án tuần lập theo mẫu dưới đây. Cột Ví dụ minh họa tuần 09/11–13/11/2026 của milestone Quy trình tiếp nhận và xử lý ticket. **Số liệu trong cột Ví dụ là giả định, không phải số liệu thật.**

| Mục | Nội dung cần điền | Ví dụ |
|---|---|---|
| Kỳ báo cáo | Tuần báo cáo; milestone hiện tại; người lập | Tuần 09/11–13/11/2026; Quy trình tiếp nhận và xử lý ticket; PM lập |
| Trạng thái chung | Xanh, Vàng hoặc Đỏ, lấy theo lĩnh vực xấu nhất ở mục 3.1 | Vàng, do tiến độ ở mức Vàng |
| Tiến độ | Số task hoàn thành; tỷ lệ đúng hạn; task đang chậm; dự báo ngày đóng milestone | Vàng: 5/7 task đúng hạn (71%, dưới ngưỡng 90%); task sinh mã hồ sơ chậm 2 ngày; milestone vẫn dự kiến đóng đúng ngày 16/11 |
| Giờ công | Giờ thực tế trong tuần và lũy kế; dự báo so với 870 giờ; task lệch quá 20% | Xanh: tuần này 52 giờ, lũy kế 214 giờ; dự báo 840 giờ, chưa vượt 870 giờ; không task nào lệch quá 20% |
| Chất lượng | Số lỗi đang mở theo mức; task chờ test; chỉ số theo QMP mục 7 | Xanh: không có Critical hoặc Major; 3 Minor đang sửa; 4 task chờ test |
| Phạm vi | Change-request mới, đang đánh giá hoặc đã quyết định; số lần sửa nhỏ đã dùng | Xanh: không có change-request; đã dùng 1/5 lần sửa nhỏ |
| Rủi ro và vấn đề | Rủi ro mức Cao; vấn đề mới hoặc chưa xử lý xong, người phụ trách, thời hạn | Rủi ro "chưa có danh sách phòng ban thật" (Trung bình): đã gửi câu hỏi cho trường. Chậm sinh mã hồ sơ: BE Dev / Senior hỗ trợ BE Dev / Mid, hạn mới 11/11 |
| Cần quyết định | Nội dung cần PM, nhóm hoặc Aurora University quyết định | Không có |
| Kế hoạch tuần tới | Các việc chính và mục tiêu tuần tiếp theo | Chạy test trọn luồng và đóng milestone ngày 16/11; mở milestone Phân công, xử lý, chuyển phòng ban |
