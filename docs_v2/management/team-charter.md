# UNISUPPORT – TEAM CHARTER

Ngày cập nhật: 17/11/2026

## 1. Mục đích

- Giúp sinh viên Aurora University nhận hỗ trợ nhanh, theo dõi được tiến độ và giúp nhà trường đo chất lượng phục vụ.
- Thống nhất cách làm việc để mỗi người biết mình làm gì, nhận và nộp việc ở đâu, hỏi ai khi gặp vướng mắc.

## 2. Mục tiêu chung

- Bàn giao đúng phạm vi đã chốt trong 15 tuần và định mức baseline **1109 giờ công** (gồm 1021 giờ cơ sở và 88 giờ quỹ dự phòng rủi ro); thời gian chờ khách phản hồi không tính vào 15 tuần. Quy tắc phân biệt giờ baseline, năng lực dư (`Resources`), giờ CR và chịu chi phí vượt giờ theo [Bang_Effort.md](Bang_Effort.md) mục 2.0d.
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
2. Bị chặn quá nửa ngày: báo ngay trên Discord, nêu task, vấn đề và người cần hỗ trợ. Dự kiến vượt 20% giờ kế hoạch của một task: báo PM trước khi làm tiếp. Mọi giờ bổ sung cho phạm vi baseline (kể cả lấy từ năng lực dư ở sheet `Resources`) đều ghi vào Bảng theo dõi giờ và tính vào dự báo so với 1109 giờ; chi phí vượt nội bộ do bên phát triển tự chịu, và PM phải báo Aurora University bằng văn bản nếu việc vượt giờ đe dọa mốc bàn giao, cần điều chỉnh ưu tiên FR `Should` hoặc phát sinh từ thay đổi phạm vi/giả định của khách.
3. Mọi thay đổi về tài liệu dự án, kế hoạch, báo giá, đặc tả, quy trình và mã nguồn đưa vào nhánh `main` đều thực hiện qua Pull Request, có ít nhất 1 người khác tác giả review và gộp bằng **squash merge** theo mục 9; người kiêm vai trò tuyệt đối không tự duyệt PR của mình. Ngoại lệ sửa nhỏ hành chính/chính tả chỉ được phép trong phạm vi hẹp có kiểm soát và phải lưu dấu theo đúng mục 9.
4. Yêu cầu từ khách chuyển về PM. Sửa nhỏ theo Proposal mục 4 được hỗ trợ trong giới hạn 2–5 lần; thay đổi lớn đi theo quy trình change request ([03](03-quan-tri-giam-sat.md) mục 5).
5. Sau mỗi milestone, ban quản lý xem lại Team Charter; Đào Tuấn Bảo sửa, Lê Hoàng Tùng duyệt, thay đổi đi qua PR.

## 5. Quy trình nhận và hoàn thành task

1. **Nhận việc:** mở issue trên GitHub đã được giao ở buổi họp đầu milestone ([03](03-quan-tri-giam-sat.md) mục 2.3). Kiểm tra mã FR, milestone `MS-xx`, nhãn đúng tầng issue (issue thực thi `[BE]`/`[FE]` hoặc task 1 cấp mang nhãn `vai trò: …`; FR parent task mang `task` + `loại: FR` để theo dõi tổng thể theo [03](03-quan-tri-giam-sat.md) mục 3.1), hạn theo WBS, mã WBS và điều kiện hoàn thành. Chưa rõ yêu cầu thì hỏi BA trước khi làm.
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

- **GitHub là nơi gốc của task và tiến độ:** [Issues](https://github.com/ericphan2545/UniSupport/issues), [Milestones `MS-xx`](https://github.com/ericphan2545/UniSupport/milestones). Mọi chỉ số tiến độ trong [03](03-quan-tri-giam-sat.md) mục 3.1 lấy từ đây. Tài liệu dự án nằm trong `docs_v2/`, đọc từ [mục lục](../README.md).
- **Bảng theo dõi giờ, Decision Log, biên bản họp, báo cáo:** danh mục định vị, người quản lý, quyền xem và các mẫu/ví dụ (`Mô phỏng`) đặt tại [records/README.md](records/README.md) ([Bảng theo dõi giờ](records/time-tracking-log.md), [Decision Log](records/decision-log.md), [Biên bản họp](records/meeting-minutes-template.md)); bản vận hành trên Google Drive cập nhật URL theo [03](03-quan-tri-giam-sat.md) mục 6.1. Mỗi dòng giờ ghi kèm số issue và mã WBS để PM đối chiếu giờ kế hoạch.
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

Áp dụng cho toàn bộ tài liệu ngay từ hiện tại và cho mã nguồn khi dự án triển khai.

- **Branch và bảo vệ `main`:** Không push trực tiếp lên `main` (trừ ngoại lệ sửa nhỏ có kiểm soát nêu dưới đây). Khuyến nghị bật Branch Protection trên GitHub cho `main` (chặn force-push, yêu cầu ≥ 1 `Approve` trước khi gộp, bật **Squash and merge**). Tên nhánh dùng chữ thường, không dấu, nối bằng gạch ngang: tài liệu `docs/<mã>-<mô-tả-ngắn>` (ví dụ `docs/cap-nhat-quy-trinh-review`), tính năng `feat/<số-issue>-<mô-tả-ngắn>` (ví dụ `feat/1-tao-yeu-cau`), sửa lỗi `fix/<số-issue>-<mô-tả-ngắn>`.
- **Commit:** dùng tiền tố chuẩn (`docs:`, `feat:`, `fix:`, `chore:`), mô tả ngắn gọn việc đã làm.
- **Pull Request:** bắt buộc điền đầy đủ `.github/pull_request_template.md` (không để nguyên placeholder chưa điền); ghi rõ mã hạng mục/rà soát, `Closes #n` cho từng issue được xử lý, tóm tắt quyết định/lý do và bằng chứng kiểm tra thực tế; với file Word/Excel/PDF phải ghi rõ file, sheet hoặc mục đã đổi và kết quả các ô kiểm tra.
- **Review bắt buộc bởi người khác tác giả (SLA 24 giờ):**
  - Mọi PR phải có **ít nhất 1 thành viên khác tác giả** duyệt (`Approve` trên GitHub). Tác giả không tự review và không tự gộp khi chưa có duyệt.
  - PR sửa tài liệu thuộc mảng nào do người phụ trách mảng đó (mục 3.1) review; nếu người phụ trách là tác giả thì **người dự phòng ở mục 3.1** review. PR chạm nhiều mảng mời các đầu mối của từng mảng. Với code: Senior của mảng review; code của Senior do Senior mảng còn lại review.
  - Thời hạn phản hồi review tối đa **1 ngày làm việc**. Nếu reviewer chính không phản hồi sau 12 giờ, tác giả chuyển thẻ sang người dự phòng (mục 3.1/3.2) để bảo đảm tiến độ.
- **Merge:** chỉ gộp sau khi có ít nhất 1 lượt `Approve` từ người khác tác giả và mọi góp ý đã được xử lý; bắt buộc dùng **Squash and merge**; xóa nhánh sau khi gộp.
- **Ngoại lệ có kiểm soát cho sửa nhỏ:**
  - *Trường hợp được phép:* chỉ áp dụng cho sửa lỗi chính tả, định dạng Markdown hoặc cập nhật thông tin hành chính cá nhân trong `README.md` (ví dụ mã số thành viên, sửa một đường dẫn hiển nhiên) với quy mô **≤ 5 dòng thay đổi trên tối đa 1 file Markdown**. **Tuyệt đối không áp dụng** cho mã nguồn, file Word/Excel/PDF, đặc tả PRD (`FR/AC/BR/E`), NFR, kịch bản test, WBS, báo giá, giả định, rủi ro hay quy trình quản trị.
  - *Người có quyền:* thành viên phụ trách tài liệu đó (mục 3.1) hoặc trưởng nhóm (Lê Hoàng Tùng).
  - *Cách lưu dấu bắt buộc:* dùng tiền tố `docs(typo):` hoặc `docs(admin):` ghi rõ lý do trong thông điệp commit, gửi link commit lên Discord nhóm ngay khi push và phải có ít nhất 1 thành viên khác xác nhận đã xem trong vòng 24 giờ.
- **Áp dụng quy trình:** Mỗi hạng mục rà soát có một nhánh và một PR riêng, liên kết issue liên quan, điền đầy đủ mẫu PR và ghi bằng chứng kiểm tra. Thực hiện review khác tác giả và squash merge theo quy định trên.
- **Tag baseline cho permalink đặc tả:** mỗi lần chốt phạm vi hoặc duyệt CR làm thay đổi PRD, trưởng nhóm hoặc người phụ trách GitHub tạo tag bản chốt (ví dụ `baseline-v2.0`, `baseline-v2.1`) để toàn bộ GitHub Issue dùng permalink ổn định trỏ thẳng tới heading FR theo quy ước ở [master-plan.md](master-plan.md).
- **Hạng mục rà soát chất lượng nội bộ:** mỗi hạng mục rà soát mở thành một issue riêng kèm mã theo dõi; xử lý bằng PR có `Closes #n`, hoặc đóng issue kèm lý do cụ thể nếu giữ nguyên hiện trạng.

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
