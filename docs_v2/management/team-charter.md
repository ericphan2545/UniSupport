# UNISUPPORT – TEAM CHARTER

Ngày cập nhật: 17/11/2026

## 1. Mục đích

- Giúp sinh viên Aurora University nhận hỗ trợ nhanh, theo dõi được tiến độ và giúp nhà trường đo chất lượng phục vụ.
- Thống nhất cách làm việc để mỗi người biết mình làm gì, nhận và nộp việc ở đâu, hỏi ai khi gặp vướng mắc.

## 2. Mục tiêu chung

- Bàn giao đúng phạm vi đã chốt trong 15 tuần và 870 giờ công; thời gian chờ khách phản hồi không tính vào 15 tuần.
- Chất lượng bàn giao: đạt điều kiện Go-live ở [QMP](02-qmp-quan-ly-chat-luong.md) mục 2.2, khớp điều kiện nghiệm thu với khách ở Proposal mục 6.2.
- Phạm vi theo PRD ([product/README.md](../product/README.md) và `modules/`). Tiêu chí nghiệm thu chính thức chốt với khách bằng văn bản ở cuối tuần 2 của `MS-01`.

## 3. Vai trò và trách nhiệm

Dự án có hai nhóm người: **ban quản lý dự án** là 4 thành viên của nhóm, và **đội thực thi** là đội giả định (**Mô phỏng**) làm phần phát triển. Khách hàng Aurora University cũng là **Mô phỏng**.

### 3.1 Ban quản lý dự án

Mỗi thành viên chịu trách nhiệm chính một số tài liệu: giữ nội dung đúng, nhất quán với tài liệu khác và duyệt PR sửa tài liệu đó. Lê Hoàng Tùng tham gia mọi mảng.

| Thành viên | Mảng phụ trách | Trách nhiệm chính | Người dự phòng |
|---|---|---|---|
| Lê Hoàng Tùng (trưởng nhóm) | Proposal và báo giá; Quản trị và giám sát; tham gia mọi mảng | `proposal.docx` và `sent/`, `quotation.xlsx`, `Bang_Effort.md`, `03-quan-tri-giam-sat.md`, `project/`; đầu mối với khách (Mô phỏng); duyệt cuối các thay đổi về giá, mốc, phạm vi | Phan Tiến Thịnh |
| Ngô Tấn Đức | PRD | `product/README.md`, `modules/`, `non-functional-requirements/`, `acceptance/`, `architecture/`; giữ thang ưu tiên FR | Đào Tuấn Bảo |
| Phan Tiến Thịnh | Kế hoạch và GitHub | `UniSupport_Masterplan_nhom2.xlsx`, `master-plan.md`, `risk-register.md`; milestone, issue, nhãn và mẫu issue trên GitHub | Lê Hoàng Tùng |
| Đào Tuấn Bảo | Cách làm việc, chất lượng, sản phẩm | `team-charter.md`, `02-qmp-quan-ly-chat-luong.md`, `01-quan-ly-san-pham.md` | Ngô Tấn Đức |

### 3.2 Đội thực thi (Mô phỏng)

Mỗi vai trò 1 người, tổng 10 người. Tên vai trò thống nhất với bảng effort, báo giá, sheet Resources của Master Plan (giờ/tuần của từng người) và nhãn `vai trò: …` trên GitHub. Owner của issue và của dòng WBS là một vai trò trong bảng này.

| Vai trò | Số người | Trách nhiệm chính | Báo cáo cho | Dự phòng |
|---|---|---|---|---|
| PM / Senior | 1 | Lập và theo dõi kế hoạch, giờ công; kiểm soát phạm vi; liên lạc với khách | Ban quản lý (Lê Hoàng Tùng) | BA / Mid |
| BA / Mid | 1 | Làm rõ yêu cầu, viết đặc tả và hướng dẫn sử dụng; phân loại feedback | PM / Senior | PM / Senior |
| UI/UX / Mid | 1 | Thiết kế giao diện, prototype; usability test | PM / Senior | FE Dev / Senior |
| BE Dev / Senior | 1 | Quyết định kiến trúc backend và dữ liệu; review code backend | PM / Senior | BE Dev / Mid |
| BE Dev / Mid | 1 | Lập trình API và nghiệp vụ | BE Dev / Senior | BE Dev / Senior |
| FE Dev / Senior | 1 | Quyết định kiến trúc frontend; review code frontend | PM / Senior | FE Dev / Junior |
| FE Dev / Junior | 1 | Xây dựng giao diện theo thiết kế | FE Dev / Senior | FE Dev / Senior |
| QA / Senior | 1 | Lập kế hoạch kiểm thử, duyệt test case, xác nhận mức lỗi và chất lượng | PM / Senior | Tester / Junior |
| Tester / Junior | 1 | Viết, chạy test case, kiểm tra lại sau sửa lỗi, hỗ trợ khách nghiệm thu | QA / Senior | QA / Senior |
| DevOps / Mid | 1 | Git, CI/CD, môi trường và triển khai | BE Dev / Senior | BE Dev / Senior |

## 4. Quy tắc làm việc

1. Mỗi người làm tối đa 2 task cùng lúc; cập nhật trạng thái issue trên GitHub và ghi giờ thực tế vào Bảng theo dõi giờ ([03](03-quan-tri-giam-sat.md) mục 6.1) mỗi ngày làm việc.
2. Bị chặn quá nửa ngày: báo ngay trên Discord, nêu task, vấn đề và người cần hỗ trợ. Dự kiến vượt 20% giờ kế hoạch: báo PM trước khi làm tiếp.
3. Mọi thay đổi vào nhánh `main` chỉ qua Pull Request đã được review theo mục 9; người kiêm vai trò vẫn không tự review.
4. Yêu cầu từ khách chuyển về PM. Sửa nhỏ theo Proposal mục 4 được hỗ trợ trong giới hạn 2–5 lần; thay đổi lớn đi theo quy trình change request ([03](03-quan-tri-giam-sat.md) mục 5).
5. Sau mỗi milestone, ban quản lý xem lại Team Charter; Đào Tuấn Bảo sửa, Lê Hoàng Tùng duyệt, thay đổi đi qua PR.

## 5. Quy trình nhận và hoàn thành task

1. **Nhận việc:** mở issue trên GitHub đã được giao ở buổi họp đầu milestone ([03](03-quan-tri-giam-sat.md) mục 2.3). Kiểm tra mã FR, milestone `MS-xx`, owner (nhãn `vai trò: …`), hạn, mã WBS và điều kiện hoàn thành. Chưa rõ yêu cầu thì hỏi BA trước khi làm.
2. **Thực hiện:** cập nhật trạng thái issue và giờ thực tế mỗi ngày làm việc. Tạo nhánh riêng theo mục 9. Nếu bị chặn hoặc sắp vượt giờ, xử lý theo mục 4.
3. **Nộp việc:** mở Pull Request theo mẫu, ghi `Closes #n`, phần đã làm và cách kiểm tra. Nhờ người review theo mục 9, sửa các góp ý rồi gửi lại.
4. **Kiểm tra và hoàn tất:** chỉ đóng issue khi đạt Definition of Done ([02-qmp-quan-ly-chat-luong.md](02-qmp-quan-ly-chat-luong.md) mục 5). Với task: đã review và merge, đạt tiêu chí chấp nhận liên quan, Tester đã kiểm thử, không còn lỗi Critical, chạy được trên Staging và đã ghi giờ thực tế.
5. **Báo cáo:** trước 21h30 mỗi ngày làm việc, gửi trên Discord: hôm qua làm gì, hôm nay làm gì, đang vướng gì; kèm link issue.

## 6. Gặp vấn đề thì hỏi ai

- Yêu cầu hoặc đặc tả chưa rõ → BA / Mid.
- API, dữ liệu hoặc backend → BE Dev / Senior.
- Code giao diện hoặc frontend → FE Dev / Senior.
- Thiết kế và prototype → UI/UX / Mid.
- Test case, lỗi hoặc chất lượng → QA / Senior.
- Git, CI/CD, môi trường hoặc triển khai → DevOps / Mid.
- Phạm vi, tiến độ, thay đổi yêu cầu hoặc việc liên quan đến khách → PM / Senior.
- Nội dung một tài liệu dự án → thành viên ban quản lý phụ trách tài liệu đó (mục 3.1).
- Người phụ trách vắng → hỏi người dự phòng ở mục 3; vẫn chưa giải quyết được thì báo PM (đội thực thi) hoặc trưởng nhóm (ban quản lý).

## 7. Giao tiếp và lưu tài liệu

- **GitHub là nơi gốc của task và tiến độ:** Issues, Milestones `MS-xx` và Project #4. Mọi chỉ số tiến độ trong [03](03-quan-tri-giam-sat.md) mục 3.1 lấy từ đây. Tài liệu dự án nằm trong `docs_v2/`, đọc từ [mục lục](../README.md).
- **Bảng theo dõi giờ, Decision Log, biên bản họp, báo cáo:** Google Drive, theo [03](03-quan-tri-giam-sat.md) mục 6.1. Mỗi dòng giờ ghi kèm số issue để PM so với giờ kế hoạch.
- **Discord:** trao đổi hằng ngày và báo cáo trước 21h30. Tin nhắn về công việc ghi rõ task, vấn đề, việc cần người nhận hỗ trợ và gắn link liên quan.
- **Họp:** ban quản lý họp tuần vào thứ Hai, 20h00, qua Discord; họp đầu và cuối mỗi milestone theo [03](03-quan-tri-giam-sat.md) mục 2.3. Biên bản ghi việc cần làm, người phụ trách, thời hạn.
- **Email với khách:** PM gửi chính thức tối thiểu 1 lần/tuần; quyết định và xác nhận của khách phải được lưu bằng văn bản.
- Quyết định trên Discord hoặc trong cuộc họp phải được cập nhật vào issue hoặc tài liệu liên quan ngay khi chốt.

## 8. Ra quyết định và xử lý bất đồng

- Kỹ thuật trong một mảng: Senior của mảng (BE Dev / Senior, FE Dev / Senior) quyết sau khi nghe ý kiến người liên quan.
- Việc ảnh hưởng nhiều mảng: người phụ trách mảng chính quyết sau khi trao đổi với các bên; bất đồng quá 1 ngày thì PM / Senior quyết cuối.
- Việc gấp cần quyết trong 24 giờ khi PM vắng: BE Dev / Senior quyết tạm và ghi lại để PM xem lại.
- Chỉ PM xác nhận với khách, bằng văn bản.
- Trong ban quản lý: nội dung thuộc tài liệu nào thì người phụ trách tài liệu đó quyết. Thay đổi chạm giá, mốc hoặc phạm vi được thống nhất ở họp tuần; không thống nhất thì biểu quyết đa số 4 người, hòa thì trưởng nhóm quyết.

### 8.1 Xử lý xung đột

1. Hai bên trao đổi trực tiếp trong 1 ngày làm việc, dựa trên tài liệu đã chốt (PRD, QMP, kế hoạch).
2. Chưa thống nhất: đưa ra họp tuần hoặc họp gấp; mỗi bên trình bày ngắn kèm bằng chứng, người phụ trách mảng đề xuất phương án.
3. Vẫn chưa thống nhất: PM / Senior quyết với đội thực thi, trưởng nhóm quyết với ban quản lý; ghi vào Decision Log kèm lý do.
4. Xung đột cá nhân ảnh hưởng công việc: trưởng nhóm trao đổi riêng với từng người, sau đó điều chỉnh phân công nếu cần.

### 8.2 Khi thiếu người

- Ban quản lý: người dự phòng ở mục 3.1 nhận mảng và sửa tài liệu thay, ghi rõ trong PR.
- Đội thực thi (Mô phỏng): người dự phòng ở mục 3.2 nhận việc; PM dùng năng lực còn trống ở sheet Resources. Vắng quá 2 ngày làm việc thì xử lý theo rủi ro `R-26` ([risk-register.md](risk-register.md)).

## 9. Quy tắc branch, Pull Request, review, merge

Áp dụng cho tài liệu ngay bây giờ và cho code khi dự án chạy.

- **Branch:** không push trực tiếp lên `main`. Nhánh tài liệu đặt tên `docs/<mô-tả-ngắn>`; nhánh code `feat/<số-issue>-<mô-tả-ngắn>` hoặc `fix/<số-issue>-<mô-tả-ngắn>`. Chữ thường, không dấu, nối bằng gạch ngang. Ví dụ: `docs/r1-feedback`, `feat/1-tao-yeu-cau`.
- **Commit:** bắt đầu bằng loại thay đổi (`docs:`, `feat:`, `fix:`), mô tả ngắn việc đã làm.
- **Pull Request:** dùng mẫu `.github/pull_request_template.md`; ghi `Closes #n` cho từng issue được xử lý; ghi quyết định và lý do; với file Word/Excel ghi rõ sheet hoặc mục đã đổi.
- **Review:** ít nhất 1 thành viên ban quản lý khác tác giả duyệt. PR sửa file Word/Excel phải được người chịu trách nhiệm file đó duyệt; nếu tác giả chính là người đó thì người dự phòng duyệt. Với code: Senior của mảng review; code của Senior do Senior mảng còn lại review.
- **Merge:** tác giả merge sau khi có duyệt và không còn góp ý chưa xử lý; dùng squash merge; xóa nhánh sau khi merge.
- **Ngoại lệ đã ghi nhận:** PR #169 và #170 (sửa theo góp ý vòng 1 và chấm thử) được trưởng nhóm merge khi chưa có review và bằng merge commit, trước khi nhóm thống nhất áp dụng mục này cho PR sửa theo feedback. Từ PR sau, mọi PR áp dụng đúng quy tắc trên, không có ngoại lệ.
- **Góp ý của giảng viên:** mỗi góp ý một issue, giữ tiêu đề và mã `R1-xx`; sửa bằng PR có `Closes #n`, hoặc đóng issue kèm lý do nếu không đồng ý.

## 10. Thuật ngữ cần biết

- **Task:** một issue trên GitHub được giao cho một vai trò, thuộc một milestone `MS-xx`.
- **Milestone:** mốc giao một phần phần mềm chạy được cho khách dùng thử; danh sách ở sheet Milestones của Master Plan.
- **PRD:** tài liệu yêu cầu sản phẩm, gồm tổng quan sản phẩm, đặc tả 42 FR, yêu cầu phi chức năng và kịch bản test.
- **Master Plan:** kế hoạch tổng gồm milestone, WBS, nguồn lực, Gantt và Change log.
- **Prototype:** bản mẫu dữ liệu giả để khách bấm thử và chốt yêu cầu ở đầu `MS-01`.
- **Review / Pull Request:** kiểm tra góp ý / đề nghị đưa thay đổi vào repo; **merge** là gộp thay đổi sau khi được duyệt.
- **Staging:** môi trường thử nghiệm trước khi triển khai chính thức.
- **Definition of Done (DoD):** các điều kiện để một việc được coi là xong; nơi gốc ở QMP mục 5.
- **Critical:** mức lỗi nghiêm trọng nhất theo QMP mục 6.
- **Change Request:** yêu cầu thay đổi sau khi đã chốt phạm vi; **CI/CD** là quy trình tự động kiểm tra, đóng gói và triển khai code.

## 11. Xác nhận của ban quản lý

Tôi đã đọc, hiểu và đồng ý thực hiện Team Charter này.

| STT | Họ tên | Mảng phụ trách | Ngày xác nhận | Xác nhận |
| --- | --- | --- | --- | --- |
| 1 | Lê Hoàng Tùng | Trưởng nhóm; Proposal và báo giá; Quản trị và giám sát |18/11/2026 |Tùng |
| 2 | Ngô Tấn Đức | PRD |18/11/2026 |Đức |
| 3 | Phan Tiến Thịnh | Kế hoạch và GitHub |18/11/2026|Thịnh |
| 4 | Đào Tuấn Bảo | Cách làm việc, chất lượng, sản phẩm |18/11/2026 |Bảo |
