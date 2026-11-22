# UniSupport – Kế hoạch quản trị và giám sát dự án

Phiên bản 0.5

## Mục đích và phạm vi

Kế hoạch này quy định cách báo cáo tình hình dự án từ người thực hiện lên PM, rồi lên Aurora University (khách hàng). Kế hoạch trả lời bốn câu hỏi: ai báo cáo cho ai, báo cáo khi nào, báo cáo những gì, và khi có vấn đề thì xử lý ra sao. Mục tiêu là để PM luôn biết dự án đang đi đúng hay lệch kế hoạch, từ đó quyết định kịp thời.

Kế hoạch dựa trên vai trò và quy tắc làm việc trong Team Charter, due date của milestone trên GitHub, kế hoạch 870 giờ công (Team Charter) và các cam kết trong Proposal. Nội dung sản phẩm xem [01](01-quan-ly-san-pham.md); tiêu chí chất lượng xem [02](02-qmp-quan-ly-chat-luong.md).

Khách hàng Aurora University, đội thực thi và mọi số liệu ví dụ trong tài liệu này là **Mô phỏng**. Nơi gốc của task và tiến độ là GitHub (Issues, Milestones `MS-xx`, Project #4); giờ thực tế ghi ở Bảng theo dõi giờ (mục 6.1); rủi ro ở [risk-register.md](risk-register.md).

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
| Tiến độ | Tỷ lệ task xong đúng Hạn ghi trong issue (lấy từ dòng WBS); dự báo ngày đóng milestone so với due date trên GitHub | Tỷ lệ đúng hạn ≥ 90% và milestone dự kiến đóng đúng due date | Tỷ lệ đúng hạn < 90%, hoặc dự báo trễ không quá 1 ngày làm việc | Dự báo trễ quá 1 ngày làm việc so với due date |
| Giờ công | Dự báo tổng giờ baseline (`EAC`) = Giờ thực tế lũy kế phạm vi baseline (gồm cả giờ giải ngân từ quỹ dự phòng và giờ lấy thêm từ năng lực dư `Resources` cho phạm vi baseline) + Giờ kế hoạch của phần việc baseline còn lại, so với baseline **870 giờ** ([Bang_Effort.md](Bang_Effort.md) mục 2.0d). Giờ CR đã duyệt theo dõi ngân sách riêng | `EAC ≤ 870 giờ` và không task nào lệch quá 20% | `EAC ≤ 870 giờ`, nhưng có task lệch quá 20% giờ kế hoạch (Team Charter mục 4) | `EAC > 870 giờ` (bên phát triển chịu chi phí vượt nội bộ; báo khách nếu ảnh hưởng mốc, phạm vi `Should` hoặc do giả định/CR) |
| Chất lượng | Số lỗi Critical và Major đang mở; các chỉ số theo QMP mục 7 | Không có Critical hoặc Major đang mở; chỉ số đạt ngưỡng | Có Critical hoặc Major đang sửa trong hạn, hoặc chỉ số chưa đạt nhưng chưa đến lúc đóng milestone | Còn Critical hoặc Major khi đến lúc đóng milestone, hoặc Critical quá hạn sửa (QMP mục 6) |
| Phạm vi | Số change-request đang chờ xử lý; số lần sửa nhỏ đã dùng (tối đa 5 theo Proposal) | Không có CR đang mở và đã dùng dưới 4 lần sửa nhỏ | Có CR đang đánh giá, hoặc đã dùng 4 lần sửa nhỏ | CR chờ quá 3 ngày làm việc, hoặc thay đổi ảnh hưởng đến Go-live hay phạm vi đã thống nhất, cần khách xác nhận |

Chỉ số giờ công đo bằng giờ, không quy đổi ra tiền.

**Lấy số liệu từ GitHub.** Mọi task là issue có milestone `MS-xx`, nhãn `task` và nhãn `vai trò: …`; lỗi có nhãn `bug` và nhãn mức `sev: …`; change request có nhãn `change-request`. PM lấy số liệu cho báo cáo tuần bằng các bộ lọc:

| Chỉ số | Bộ lọc trên GitHub (tab Issues) |
|---|---|
| Task của milestone đã xong / tổng | `is:issue label:task milestone:"<tên MS-xx>" is:closed` so với cùng bộ lọc không có `is:closed` |
| Task quá hạn | Issue `label:task is:open` có Hạn (dòng **Hạn:** trong issue, theo WBS) đã qua; xem theo cột Hạn trên Project #4 |
| Tải theo vai trò | `is:issue is:open label:task label:"vai trò: BE Dev / Mid"` (đổi tên vai trò) |
| Lỗi đang mở theo mức | `is:issue is:open label:bug label:"sev: Critical"` (đổi mức) |
| Change request đang chờ | `is:issue is:open label:change-request` |
| Dự báo ngày đóng milestone | Ngày đến hạn của milestone trên GitHub so với số task còn mở và Hạn của chúng |

Giờ thực tế lấy từ Bảng theo dõi giờ (mục 6.1), cộng theo số issue để so với giờ kế hoạch của dòng WBS tương ứng.

Hành động theo từng mức:

- **Xanh:** tiếp tục theo kế hoạch.
- **Vàng:** người phụ trách mảng đề xuất hướng khắc phục; PM theo dõi kết quả trong báo cáo tuần tiếp theo.
- **Đỏ:** PM xử lý ngay bằng cách điều chỉnh phân công hoặc thứ tự công việc. Nếu ảnh hưởng đến mốc, phạm vi đã cam kết với khách, PM thông báo Aurora University và xử lý theo quy trình kiểm soát thay đổi ở mục 5.

### 3.2 Giám sát rủi ro

Nơi gốc của sổ rủi ro là [risk-register.md](risk-register.md): mỗi rủi ro có mã `R-xx`, mô tả, khả năng, tác động, mức, owner, phòng ngừa, ứng phó, dấu hiệu, trạng thái. Phan Tiến Thịnh giữ sổ.

- Tại buổi họp tuần, nhóm rà toàn bộ sổ: bổ sung rủi ro mới, cập nhật điểm và trạng thái, đóng rủi ro không còn khả năng xảy ra. Thay đổi sổ đi qua PR như tài liệu khác.
- Rủi ro mức **Cao** (điểm từ 12) được nêu trong báo cáo tuần; rủi ro Cao chưa có owner hoặc đang có dấu hiệu xảy ra được báo theo mục 2.4.
- Khi rủi ro xảy ra: chuyển sang nhật ký vấn đề và xử lý theo mục 4; nếu dùng quỹ dự phòng của hạng mục (dòng "Dự phòng Q-xx" trong WBS), PM / Senior ghi số giờ đã dùng vào báo cáo tuần.

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

Mọi thay đổi về phạm vi, tiến độ hoặc báo giá phải được đánh giá và xác nhận trước khi thực hiện, theo cam kết trong Proposal mục 4. Thay đổi mục tiêu hoặc ngưỡng chất lượng (QMP mục 8) cũng đi theo quy trình này. Mỗi change request là một issue mã `CR-xx` tạo bằng mẫu Change request trên GitHub (nhãn `change-request`); CR mô phỏng ghi rõ `Mô phỏng`.

Các đợt rà soát, chuẩn hóa tài liệu nội bộ trước khi hai bên ký chốt phạm vi không phải là change request mà được quản lý qua Pull Request và lịch sử phiên bản. Sau khi phạm vi đã chốt với Aurora University, mọi thay đổi do khách yêu cầu hoặc làm thay đổi phạm vi, mốc, báo giá đều đi qua quy trình dưới đây.

1. **Ghi nhận.** Ai cũng tạo được issue bằng mẫu Change request, nêu nội dung, lý do và feedback gốc (nếu có). Yêu cầu từ khách chuyển về PM (Team Charter mục 4).
2. **Đánh giá tác động.** Kiểm tra cổng bắt buộc và chấm tiêu chí theo [01](01-quan-ly-san-pham.md) mục 7. Điền bảng tác động trong mẫu issue: phạm vi (FR, AC bị ảnh hưởng), effort (BE Dev / Senior và FE Dev / Senior ước giờ), giá (Lê Hoàng Tùng tính theo đơn giá ở `quotation.xlsx`), timeline (milestone bị dời), rủi ro (mã `R-xx` mới hoặc đổi điểm), test (kịch bản `TS-xx` phải viết hoặc chạy lại, QA / Senior đánh giá), dự phòng đã dùng và còn lại.
3. **Quyết định.** Người duyệt theo bảng dưới; kết quả ghi trong issue (Duyệt / Từ chối / Hoãn).
4. **Cập nhật đồng bộ.** Khi được duyệt, cập nhật trong một PR có `Closes #<CR>` và ghi danh sách đã cập nhật vào issue:
   - PRD: FR và AC trong `modules/`, non-goals trong `product/README.md` (Ngô Tấn Đức);
   - báo giá: thêm dòng mới ở sheet Phiên bản của `quotation.xlsx` (Lê Hoàng Tùng);
   - kế hoạch: sheet Milestones, WBS, Change log của Master Plan với giá trị cũ và mới; issue và milestone trên GitHub (Phan Tiến Thịnh);
   - kiểm thử: `acceptance/test-scenarios.md` và ma trận truy vết (Ngô Tấn Đức);
   - sổ rủi ro (Phan Tiến Thịnh);
   - Proposal: nếu đổi phạm vi, giá hoặc mốc, lập phiên bản mới và lưu PDF vào `sent/` (Lê Hoàng Tùng).
5. **Theo dõi.** Thay đổi được theo dõi trong báo cáo tuần cho đến khi hoàn thành; Decision Log ghi quyết định.

**Ai duyệt loại thay đổi nào**

| Loại thay đổi | Người duyệt |
|---|---|
| Sửa nhỏ trong giới hạn 2–5 lần của Proposal (chữ, màu, nhãn VN/EN, bố cục nhẹ) | PM / Senior |
| Đổi thứ tự hoặc phân công việc trong một milestone, không đổi mốc, giá, phạm vi | PM / Senior |
| Dùng quỹ dự phòng của hạng mục khi rủi ro xảy ra | PM / Senior; báo ban quản lý trong báo cáo tuần |
| Đổi mức ưu tiên FR, dời FR `Should`/`Could` sang milestone sau trong cùng mốc Go-live | Ban quản lý thống nhất ở họp tuần; Aurora University xác nhận bằng văn bản |
| Thay đổi phạm vi (thêm, bỏ, đổi FR), mốc đã chốt với khách hoặc giá | Aurora University (Director of Student Services) xác nhận bằng văn bản; PM đề xuất, Lê Hoàng Tùng duyệt báo giá bổ sung |
| Mục tiêu hoặc ngưỡng chất lượng (QMP) | QA / Senior đánh giá; PM / Senior duyệt; báo khách nếu ảnh hưởng điều kiện nghiệm thu |

Yêu cầu tính năng mới phát sinh trong thời gian bảo hành không thuộc phạm vi bảo hành (Proposal 6.3) và cũng được xử lý theo quy trình này.

## 6. Nhật ký quản trị và mẫu báo cáo

### 6.1 Nhật ký quản trị

| Nhật ký | Nội dung | Người cập nhật | Nơi lưu |
|---|---|---|---|
| Bảng theo dõi giờ | Ngày, người thực hiện, công việc (kèm số issue và mã WBS), số giờ | Từng thành viên; PM tổng hợp hằng tuần | Google Drive |
| Nhật ký vấn đề | Mô tả, ngày phát hiện, ảnh hưởng, người xử lý, thời hạn, trạng thái | Người phát hiện; PM theo dõi | GitHub: lỗi dùng mẫu Bug (nhãn `bug`, `sev: …`); vấn đề khác là issue thường gắn milestone |
| Sổ rủi ro | Như mục 3.2 | Phan Tiến Thịnh; owner từng rủi ro | [risk-register.md](risk-register.md) trong repo |
| Yêu cầu thay đổi | Nội dung, lý do, đánh giá tác động, quyết định, tài liệu đã cập nhật | Người đề xuất; PM | GitHub: mẫu Change request, mã `CR-xx`, nhãn `change-request` |
| Decision Log | Ngày, nội dung quyết định, người quyết định, lý do, tài liệu cần cập nhật | PM | Google Drive |
| Biên bản họp | Nội dung thảo luận, việc cần làm, người phụ trách, thời hạn | Thành viên ghi theo lượt | Google Drive |
| Báo cáo tuần, milestone, tổng kết | Theo mục 2 và mẫu ở mục 6.2 | PM | Google Drive |

Quyết định chốt trên Discord hoặc trong họp phải được cập nhật vào issue hoặc tài liệu liên quan ngay khi chốt (Team Charter mục 7).

### 6.2 Mẫu và ví dụ báo cáo tuần

Báo cáo tình hình dự án tuần lập theo mẫu dưới đây. Cột Ví dụ minh họa tuần 12/10–16/10/2026 của `MS-02` Gửi và tiếp nhận yêu cầu. **Cột Ví dụ là Mô phỏng: số liệu giả định, không phải số liệu thật.**

| Mục | Nội dung cần điền | Ví dụ |
|---|---|---|
| Kỳ báo cáo | Tuần báo cáo; milestone hiện tại; người lập | Tuần 12/10–16/10/2026; `MS-02` Gửi và tiếp nhận yêu cầu; PM lập (Mô phỏng) |
| Trạng thái chung | Xanh, Vàng hoặc Đỏ, lấy theo lĩnh vực xấu nhất ở mục 3.1 | Vàng, do tiến độ ở mức Vàng |
| Tiến độ | Số task hoàn thành; tỷ lệ đúng hạn; task đang chậm; dự báo ngày đóng milestone so với kế hoạch đã chốt | Vàng: 5/7 task đúng hạn (71%, dưới ngưỡng 90%); task sinh mã hồ sơ (`FR-CORE-01`) chậm 2 ngày so với Hạn trong WBS; milestone vẫn dự kiến đóng đúng ngày 23/10 |
| Giờ công | Giờ thực tế trong tuần và lũy kế; dự báo so với 870 giờ; task lệch quá 20%; quỹ dự phòng đã dùng | Xanh: tuần này 52 giờ, lũy kế 455 giờ; dự báo 862 giờ, chưa vượt 870 giờ; không task nào lệch quá 20%; chưa dùng dự phòng `Q-03` |
| Chất lượng | Số lỗi đang mở theo mức; task chờ test; chỉ số theo QMP mục 7 | Xanh: không có Critical hoặc Major; 3 Minor đang sửa; 4 task chờ test |
| Phạm vi | Change-request mới, đang đánh giá hoặc đã quyết định; số lần sửa nhỏ đã dùng | Xanh: không có change-request; đã dùng 1/5 lần sửa nhỏ |
| Rủi ro và vấn đề | Rủi ro mức Cao và rủi ro có dấu hiệu xảy ra (mã `R-xx`); vấn đề mới hoặc chưa xử lý xong, người phụ trách, thời hạn | `R-02` (Trung bình): đã nhắc trường gửi danh mục thật trong email tuần. `R-09` (Trung bình): chưa có dấu hiệu. Chậm sinh mã hồ sơ: BE Dev / Senior hỗ trợ BE Dev / Mid, xong trước 20/10 |
| Cần quyết định | Nội dung cần PM, nhóm hoặc Aurora University quyết định | Không có |
| Kế hoạch tuần tới | Các việc chính và mục tiêu tuần tiếp theo | Chạy test trọn luồng và regression (`TS-01` đến `TS-03`), demo cho khách và đóng `MS-02` ngày 23/10; họp đầu `MS-03` |
