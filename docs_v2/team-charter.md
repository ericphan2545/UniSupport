# UNISUPPORT – TEAM CHARTER

Ngày cập nhật: 02/10/2026

## 1. Mục đích

- Giúp sinh viên Aurora University nhận hỗ trợ nhanh, theo dõi được tiến độ và giúp nhà trường đo chất lượng phục vụ.
- Thống nhất cách làm việc để thành viên mới biết mình làm gì, nhận và nộp việc ở đâu, hỏi ai khi gặp vướng mắc.

## 2. Mục tiêu chung

- Bàn giao đúng phạm vi đã chốt trong 15 tuần và 870 giờ công; thời gian chờ khách phản hồi không tính vào 15 tuần.
- Chỉ tiêu nội bộ: ít nhất 95% ca kiểm thử đạt, không còn lỗi Critical và không có lỗi chặn khi chạy 100–200 người dùng đồng thời trên hạ tầng trường.
- Phạm vi theo PRD. Tiêu chí nghiệm thu chính thức chốt với khách bằng văn bản tại review tuần 7.

## 3. Vai trò và trách nhiệm

Các mục dưới đây là vai trò công việc, không mặc định là 10 người riêng biệt. PM cần xác nhận người đảm nhiệm và người dự phòng trước khi giao việc; một người có thể kiêm nhiều vai trò.

- **PM (Senior):** lập kế hoạch, theo dõi tiến độ, kiểm soát phạm vi và liên lạc với khách. Dự phòng: BA.
- **BA (Mid):** làm rõ yêu cầu, viết đặc tả và hướng dẫn sử dụng. Dự phòng: PM.
- **UI/UX (Mid):** thiết kế giao diện và prototype. Dự phòng: FE Lead.
- **BE Lead (Senior):** quyết định kiến trúc backend, dữ liệu và review code BE. Dự phòng: BE Dev.
- **BE Dev (Mid):** lập trình API và xử lý nghiệp vụ. Dự phòng: BE Lead.
- **FE Lead (Senior):** quyết định kiến trúc frontend và review code FE. Dự phòng: FE Dev.
- **FE Dev (Junior):** xây dựng giao diện theo thiết kế. Dự phòng: FE Lead.
- **QA Lead (Mid/Senior):** lập kế hoạch kiểm thử, duyệt ca kiểm thử và xác nhận chất lượng. Dự phòng: Tester.
- **Tester (Junior):** viết, chạy ca kiểm thử, kiểm tra lại sau sửa lỗi và hỗ trợ khách nghiệm thu. Dự phòng: QA Lead.
- **DevOps (Mid):** quản lý Git, CI/CD, môi trường và triển khai. Dự phòng: BE Lead.

## 4. Quy tắc làm việc

1. Mỗi người làm tối đa 2 task cùng lúc; cập nhật Trello và ghi giờ thực tế mỗi ngày làm việc.
2. Bị chặn quá nửa ngày: báo ngay trên Discord, nêu task, vấn đề và người cần hỗ trợ. Dự kiến vượt 20% giờ kế hoạch: báo PM trước khi làm tiếp.
3. Code chỉ được merge sau khi có người khác review. Code của Lead do Lead mảng còn lại hoặc Dev cùng mảng review; người kiêm vai trò vẫn không tự review.
4. Yêu cầu từ khách chuyển về PM. Sửa nhỏ theo proposal được hỗ trợ trong giới hạn 2–5 lần; thay đổi lớn chỉ làm sau khi Lead ước giờ, PM báo giá và khách xác nhận bằng văn bản.
5. Sau mỗi buổi nhìn lại sprint, nhóm xem lại Team Charter; PM duyệt thay đổi và ghi phiên bản trong lịch sử tài liệu.

## 5. Quy trình nhận và hoàn thành task

1. **Nhận việc:** mở task trên Trello đã được giao ở buổi lập kế hoạch sprint. Kiểm tra mô tả, người phụ trách, tiêu chí chấp nhận, giờ kế hoạch và hạn hoàn thành. Chưa rõ yêu cầu thì hỏi BA trước khi làm.
2. **Thực hiện:** cập nhật tiến độ và giờ thực tế trên Trello. Với code, tạo nhánh riêng theo quy ước repo. Nếu bị chặn hoặc sắp vượt giờ, xử lý theo mục 4.
3. **Nộp việc:** gắn link Pull Request hoặc tài liệu vào task; ghi phần đã làm và cách kiểm tra. Nhờ Lead liên quan review, sửa các góp ý rồi gửi lại.
4. **Kiểm tra và hoàn tất:** chỉ chuyển task sang Done khi đạt Definition of Done trong Master Plan. Với task code: đã review và merge, đạt tiêu chí chấp nhận, Tester đã kiểm thử, không còn lỗi Critical, chạy được trên Staging và đã ghi giờ thực tế.
5. **Báo cáo:** trước 21h30 mỗi ngày làm việc, gửi trên Discord: hôm qua làm gì, hôm nay làm gì, đang vướng gì; kèm link task để người khác theo dõi.

## 6. Gặp vấn đề thì hỏi ai

- Yêu cầu hoặc đặc tả chưa rõ → BA.
- API, dữ liệu hoặc backend → BE Lead.
- Code giao diện hoặc frontend → FE Lead.
- Thiết kế và prototype → UI/UX.
- Ca kiểm thử, lỗi hoặc chất lượng → QA Lead.
- Git, CI/CD, môi trường hoặc triển khai → DevOps.
- Phạm vi, tiến độ, thay đổi yêu cầu hoặc việc liên quan đến khách → PM.
- Người phụ trách vắng → hỏi người dự phòng ở mục 3; vẫn chưa giải quyết được thì báo PM điều phối.

## 7. Giao tiếp và lưu tài liệu

- **Trello:** nơi giao và theo dõi task; cập nhật mỗi ngày làm việc.
- **Discord:** trao đổi hằng ngày và báo cáo trước 21h30. Tin nhắn về công việc ghi rõ task, vấn đề, việc cần người nhận hỗ trợ và gắn link liên quan.
- **Họp theo sprint 2 tuần:** lập kế hoạch, demo kết quả và nhìn lại cách làm việc; lưu biên bản và người phụ trách việc tiếp theo.
- **Email với khách:** PM gửi chính thức tối thiểu 1 lần/tuần; quyết định và xác nhận của khách phải được lưu bằng văn bản.
- **GitHub:** lưu PRD tại `docs/prd.md` và Team Charter tại `docs/team-charter.md`. Mỗi lần sửa gửi Pull Request, ghi rõ phần thay đổi và nhờ người phụ trách review trước khi merge.
- Quyết định trên Discord hoặc trong cuộc họp phải được cập nhật vào task hoặc tài liệu liên quan ngay khi chốt; tránh để quyết định chỉ nằm trong tin nhắn.

## 8. Ra quyết định và xử lý bất đồng

- Kỹ thuật trong một mảng: Lead của mảng đó quyết sau khi nghe ý kiến người liên quan.
- Việc ảnh hưởng nhiều mảng: người phụ trách mảng chính quyết sau khi trao đổi với các bên; bất đồng quá 1 ngày thì PM quyết cuối.
- Việc gấp cần quyết trong 24 giờ khi PM vắng: BE Lead quyết tạm thời và ghi lại để PM xem lại.
- Chỉ PM xác nhận với khách, bằng văn bản.

## 9. Thuật ngữ cần biết

- **Task:** một công việc được giao và theo dõi trên Trello.
- **Sprint:** chu kỳ làm việc 2 tuần.
- **PRD:** tài liệu yêu cầu sản phẩm, xác định phạm vi chức năng.
- **Master Plan:** kế hoạch tổng, gồm mốc tiến độ và tiêu chí hoàn thành công việc.
- **Prototype:** bản mẫu để khách trải nghiệm và phản hồi trước khi phát triển chính thức.
- **Review / Pull Request:** kiểm tra góp ý / đề nghị đưa thay đổi vào repo; **merge** là gộp thay đổi sau khi được duyệt.
- **Staging:** môi trường thử nghiệm trước khi triển khai chính thức.
- **Definition of Done (DoD):** các điều kiện để task được coi là hoàn thành.
- **Critical:** mức lỗi nghiêm trọng nhất theo phân loại của nhóm; **UAT** là khách kiểm thử để nghiệm thu.
- **Change Request:** yêu cầu thay đổi sau khi đã chốt phạm vi; **CI/CD** là quy trình tự động kiểm tra, đóng gói và triển khai code.

## 10. Xác nhận của thành viên

Tôi đã đọc, hiểu và đồng ý thực hiện Team Charter này. PM điền người đảm nhiệm từng vai trò; người kiêm nhiệm xác nhận tại các dòng tương ứng.

| STT | Họ tên | Vai trò | Ngày xác nhận | Chữ ký |
| --- | --- | --- | --- | --- |
| 1 | | PM | | |
| 2 | | BA | | |
| 3 | | UI/UX | | |
| 4 | | BE Lead | | |
| 5 | | BE Dev | | |
| 6 | | FE Lead | | |
| 7 | | FE Dev | | |
| 8 | | QA Lead | | |
| 9 | | Tester | | |
| 10 | | DevOps | | |
