UNISUPPORT – PRODUCT REQUIREMENTS DOCUMENT (PRD)

Ngày: 25/09/2026

## 1. Tổng quan

UniSupport là web application (responsive) cho phép sinh viên Aurora University gửi và theo dõi yêu cầu hỗ trợ, nhân viên tiếp nhận và xử lý theo hàng đợi, và ban quản lý đánh giá chất lượng xử lý qua báo cáo. Khách hàng: Director of Student Services.

| Người dùng | Số lượng | Nhu cầu chính |
| --- | --- | --- |
| Sinh viên | ~3.000 đăng ký | Gửi yêu cầu nhanh, đính kèm minh chứng, biết hồ sơ đang ở đâu |
| Nhân viên xử lý | Chờ chốt (Q2) | Một hàng đợi rõ ràng, đủ thông tin để xử lý |
| Ban quản lý | Chờ chốt (Q2) | Số liệu về khối lượng, thời gian xử lý, mức hài lòng |

## 2. Mục tiêu

Mục tiêu sản phẩm:

•     G1 – Sinh viên gửi yêu cầu, đính kèm tài liệu và theo dõi trạng thái trực tuyến.

•     G2 – Nhân viên tiếp nhận, phân công và xử lý yêu cầu trên một hàng đợi chung.

•     G3 – Ban quản lý theo dõi và đánh giá chất lượng xử lý qua dashboard, báo cáo.

•     G4 – Dữ liệu sinh viên và tệp đính kèm chỉ người có quyền mới truy cập được.

Chỉ số thành công:

| Chỉ số | Mục tiêu |
| --- | --- |
| FR mức Must hoàn thành | 100% khi nghiệm thu |
| Chốt scope và tiêu chí chất lượng | Tuần 7 (Intermediate Project Review) |
| Go-live và nghiệm thu tổng thể | Tuần 15 (không tính thời gian chờ phản hồi của trường) |
| Usability, chỉ số nghiệp vụ | Chờ chốt (Q11) |

## 3. Phạm vi

Trong phạm vi:

| Module | Nội dung |
| --- | --- |
| Cổng Sinh viên | Thông tin sinh viên, gửi yêu cầu có gợi ý phòng ban, upload tệp, mã hồ sơ, theo dõi, thông báo, đánh giá |
| Cổng Nhân viên | Hàng đợi theo trạng thái, chi tiết hồ sơ, nhật ký, yêu cầu bổ sung, đóng hồ sơ, phân công, ưu tiên, chuyển phòng ban |
| Cổng Quản lý | Dashboard, phân tích nhóm vấn đề, khối lượng theo phòng ban, điểm hài lòng |
| Định danh & Bảo mật | Đăng nhập tài khoản trường, phân quyền 3 vai trò, giới hạn truy cập, bảo vệ tệp, nhật ký hệ thống |

Ngoài phạm vi:

•     Tích hợp SIS/LMS, thanh toán và hệ thống hiện có khác của trường; tự động ra quyết định học vụ.

•     Ứng dụng di động native (iOS/Android).

•     Vận hành sau bàn giao: server, cấp tài khoản nội bộ, sao lưu, an toàn thông tin nội bộ.

## 4. Luồng chính và trạng thái hồ sơ

•     Sinh viên: đăng nhập → gửi yêu cầu (có gợi ý phòng ban, đính kèm tệp) → nhận mã hồ sơ → theo dõi, bổ sung khi được yêu cầu → đánh giá khi hồ sơ đóng.

•     Nhân viên: xem hàng đợi → mở hồ sơ → phân công, đặt ưu tiên hoặc chuyển phòng ban → xử lý, ghi nhật ký → yêu cầu bổ sung nếu thiếu → đóng hồ sơ.

•     Ban quản lý: xem dashboard → xem báo cáo nhóm vấn đề, khối lượng theo phòng ban, mức hài lòng.

| Trạng thái | Khi nào | Nguồn |
| --- | --- | --- |
| Mới tiếp nhận | Sinh viên vừa gửi, chưa có người phụ trách | Proposal |
| Đang xử lý | Đã có người phụ trách | Proposal |
| Chờ bổ sung | Nhân viên yêu cầu sinh viên bổ sung thông tin | Đề xuất |
| Đã đóng | Nhân viên đóng hồ sơ khi hoàn tất | Đề xuất |
| Quá hạn | Hồ sơ vượt thời hạn xử lý; là cờ hay trạng thái chờ chốt (Q3) | Proposal |

#### 5. Yêu cầu chức năng

##### MOD-STU: Cổng Sinh viên

[FR-STU-01] Hiển thị thông tin sinh viên từ tài khoản trường

- Mô tả: Lấy thông tin định danh của sinh viên từ hệ thống SSO để điền sẵn vào form, không cho phép sửa đổi để đảm bảo tính xác thực.

- Quy tắc kỹ thuật: Các trường (Họ tên, MSSV, Khoa/Viện, Email) phải được map từ Payload của JWT Token (SSO). Giao diện thiết lập readonly (chỉ đọc). API tạo đơn phải phớt lờ các trường này nếu Client cố tình gửi lên, tự động lấy dữ liệu từ Token trên Server để lưu vào DB. SĐT liên hệ bắt buộc validate theo định dạng VN (VD: /^(84|0[3|5|7|8|9])[0-9]{8}$/).

- Tiêu chí nghiệm thu:

  - Happy Case: Form tự động điền đúng tên/MSSV. Nhập SĐT hợp lệ và lưu thành công.

  - Negative Case: Dùng Postman gửi request tạo đơn, cố tình sửa field "mssv": "12345" -> Server vẫn lưu đơn với MSSV thật từ token, bỏ qua số "12345".

[FR-STU-02] Sinh viên gửi yêu cầu bằng form nhập mô tả vấn đề

- Mô tả: Form điền thông tin để tạo Ticket mới.

- Quy tắc kỹ thuật: Bắt buộc áp dụng cơ chế Idempotency (Chống trùng lặp request). Khi user bấm Submit, disable nút để tránh spam click. Phía backend cấu hình Rate Limit (VD: 1 request / 5 giây / 1 user cho API tạo đơn). Validate null/empty ở cả Frontend và Backend.

- Tiêu chí nghiệm thu:

  - Edge Case: User click đúp chuột 5 lần liên tục vào nút "Gửi" -> Database chỉ ghi nhận 1 Ticket duy nhất được tạo ra.

[FR-STU-03] Gợi ý phòng ban tiếp nhận dựa trên nội dung sinh viên nhập

- Mô tả: AI/Thuật toán từ khóa đọc nội dung và tự chọn đích đến.

- Quy tắc kỹ thuật: Sử dụng API Debounce (khoảng 500ms) sau khi sinh viên ngừng gõ chữ. Cần bảng Mapping [Keywords -> Category_ID -> Dept_ID]. Nếu độ tự tin thấp, để trống Dropdown và bắt sinh viên chọn tay.

- Tiêu chí nghiệm thu: Gõ "xin bảo lưu kết quả" -> Hệ thống tự tick chọn danh mục "Bảo lưu" và chuyển đích đến "Phòng Đào tạo".

[FR-STU-04] Tải lên tệp tài liệu, minh chứng kèm yêu cầu

- Mô tả: Đính kèm file minh chứng.

- Quy tắc kỹ thuật: Bắt buộc check Magic Bytes (File Signature) ở Backend (vd: file PDF phải bắt đầu bằng 25 50 44 46). Giới hạn Body Parser max size 10MB. File lưu vào S3/MinIO, đổi tên file thành chuỗi Hash (UUID) chống path traversal.

- Tiêu chí nghiệm thu: Tạo file hack.exe, đổi tên thành minhchung.pdf và upload -> Server từ chối kèm lỗi HTTP 400 "Định dạng file không hợp lệ".

[FR-STU-05] Tự động cấp mã hồ sơ duy nhất

- Mô tả: Sinh mã Ticket không trùng lặp.

- Quy tắc kỹ thuật: Sử dụng Database Sequence (PostgreSQL) hoặc Redis Counter. Tuyệt đối không dùng câu lệnh SELECT COUNT(*) + 1 để sinh mã. Format: #TICK-[YYYY]-[SEQ].

- Tiêu chí nghiệm thu: Dùng JMeter bắn 200 request tạo ticket cùng 1 phần nghìn giây -> Không có bất kỳ ticket nào bị trùng mã.

[FR-STU-06] Theo dõi tiến độ xử lý hồ sơ của mình

- Mô tả: Trục thời gian hiển thị log trạng thái và bình luận công khai.

- Quy tắc kỹ thuật: Truy vấn API bắt buộc phải kèm điều kiện WHERE created_by = {user_id_tu_token}. Bảng Comments phải có field is_internal. Khi query cho sinh viên, kẹp thêm AND is_internal = false.

- Tiêu chí nghiệm thu: Sinh viên A lấy URL chi tiết của Sinh viên B dán vào trình duyệt -> Báo lỗi 403/404. Sinh viên không nhìn thấy các bình luận có cờ Internal.

[FR-STU-07] Nhận thông báo khi hồ sơ được cập nhật

- Mô tả: Gửi Email và Push Notification.

- Quy tắc kỹ thuật: Phải đẩy vào Message Queue (VD: RabbitMQ/Redis BullMQ) chạy ngầm (Asynchronous). Không để logic gửi mail chạy đồng bộ chặn (block) API đổi trạng thái.

- Tiêu chí nghiệm thu: Server SMTP (Email) bị sập -> API đổi trạng thái ticket vẫn trả về 200 OK thành công ngay lập tức, job gửi mail sẽ retry lại sau.

[FR-STU-08] Gửi đánh giá chất lượng hỗ trợ sau khi hồ sơ đóng

- Mô tả: Chấm sao và review. Quá 7 ngày tự động đóng.

- Quy tắc kỹ thuật: Cronjob chạy ngầm mỗi đêm quét: UPDATE tickets SET status = 'ĐÓNG' WHERE status = 'HOÀN_TẤT' AND updated_at < NOW() - 7 days.

- Tiêu chí nghiệm thu: Thử gọi API submit rating cho một ticket đang ở trạng thái ĐANG_XỬ_LÝ -> Server trả lỗi 400 Bad Request.

[FR-STU-09] Bổ sung thông tin khi nhân viên yêu cầu

- Mô tả: Nộp thêm giấy tờ vào ticket cũ.

- Quy tắc kỹ thuật: Nút "Bổ sung" chỉ hiện khi Ticket = CHỜ_BỔ_SUNG. Bổ sung xong, trigger hệ thống update về ĐANG_XỬ_LÝ và báo notification cho assignee (người phụ trách).

- Tiêu chí nghiệm thu: Nộp bù file xong, trạng thái tự động nhảy về Đang xử lý, SLA countdown của nhân viên tiếp tục chạy.

#### MOD-STF: CỔNG NHÂN VIÊN (Xử lý & Điều phối)

[FR-STF-01] Hàng đợi hồ sơ, lọc theo trạng thái

- Mô tả: Màn hình danh sách các hồ sơ được phân về phòng ban của nhân viên, chia thành các tab (Mới, Đang xử lý, Chờ bổ sung, Quá hạn) và tính năng lọc/tìm kiếm. Hiển thị % SLA đã dùng.

- Quy tắc nghiệp vụ:

  - Bảo mật dữ liệu (Data Isolation): Mọi câu truy vấn SELECT danh sách từ Backend bắt buộc phải kẹp thêm điều kiện WHERE department_id = {user_dept_id_tu_token}. Tuyệt đối không nhận department_id từ Frontend gửi lên.

  - Tối ưu hiệu năng: Bắt buộc dùng phân trang ở Database (Limit/Offset hoặc Keyset Pagination). Bắt buộc tạo Composite Index trong Database cho 3 cột: (department_id, status, created_at).

  - Tính SLA trên UI: Trả về target_sla_hours và time_spent_hours. Frontend tự lấy 2 biến này chia cho nhau để ra % SLA và đổi màu (Vàng > 70%, Đỏ > 100%).

- Test Cases (Nghiệm thu):

  - Security: Nhân viên phòng IT dùng Postman gọi API lấy danh sách, cố tình truyền params ?dept_id=2 (phòng Đào tạo) -> API phớt lờ, vẫn trả về danh sách của phòng IT.

  - Performance: Database có 200,000 tickets, API load danh sách trang 1 vẫn phải trả kết quả dưới 300ms.

[FR-STF-02] Xem chi tiết hồ sơ

- Mô tả: Nhân viên mở xem thông tin định danh sinh viên, nội dung yêu cầu, lịch sử các đơn cũ của sinh viên này và tệp đính kèm.

- Quy tắc nghiệp vụ :

  - Lịch sử đơn cũ: Backend truy vấn bảng tickets WHERE mssv = {mssv_cua_don_hien_tai} AND id != {id_hien_tai}. Chỉ lấy các trường cơ bản (ID, Tiêu đề, Trạng thái) để tối ưu payload.

  - Xem tệp đính kèm: Các file PDF/Ảnh không được tải file vật lý qua API này, mà Backend phải gọi lên S3/MinIO để generate một Pre-signed URL (Link có chữ ký) thời hạn 5 phút rồi trả link đó về cho Frontend render vào thẻ <iframe> hoặc <img>.

- Test Cases:

  - Functional: Mở chi tiết đơn của sinh viên A, mục "Lịch sử" hiện đúng 2 đơn cũ sinh viên A từng tạo tháng trước.

  - Security: Copy đường link file ảnh đính kèm, 6 phút sau dán lại vào trình duyệt -> AWS S3 trả về lỗi <Error><Code>AccessDenied</Code>.

[FR-STF-03] Ghi nhật ký xử lý (Audit Trail)

- Mô tả: Nhân viên có thể ghi chú nội bộ (chỉ nhân viên thấy). Hệ thống tự động ghi lại các mốc thời gian (nhận đơn, chuyển phòng, đổi trạng thái).

- Quy tắc nghiệp vụ:

  - Tự động hóa: Dev Backend phải sử dụng tính năng Database Triggers hoặc ORM Hooks (AfterUpdate). Hễ bảng Tickets có thay đổi cột status hoặc assignee_id, hệ thống tự động INSERT một dòng vào bảng Ticket_Logs. Không viết code insert log thủ công rải rác ở từng API.

  - Ẩn danh (Visibility): Bảng Ticket_Logs có cột is_internal (Boolean). Khi Frontend có role Sinh viên gọi API lấy log, Backend phải ngầm thêm WHERE is_internal = false.

- Test Cases:

  - Data Leak: Nhân viên gõ ghi chú và tick chọn "Ghi chú nội bộ". Đăng nhập account Sinh viên xem đơn đó, bật Network tab (F12) lên xem response của API -> Hoàn toàn không có dòng ghi chú đó trong JSON trả về (chặn từ Backend).

[FR-STF-04] Gửi yêu cầu sinh viên bổ sung thông tin

- Mô tả: Khi đơn thiếu giấy tờ, nhân viên soạn nội dung yêu cầu bổ sung, hẹn deadline nộp lại.

- Quy tắc nghiệp vụ:

  - Validation: Trạng thái đơn phải đang là ĐANG_XỬ_LÝ mới được gọi API này. Cột deadline_date Frontend gửi lên phải thỏa mãn điều kiện > NOW() (lớn hơn thời điểm hiện tại ít nhất 1 giờ).

  - Asynchronous (Bất đồng bộ): Khi lưu thành công, Backend bắn một Event sang hệ thống Queue (RabbitMQ / Redis) để tiến trình Worker chạy ngầm gửi Email/Noti cho sinh viên, tránh làm API response bị chậm.

- Test Cases:

  - Validation: Chọn deadline bổ sung là "ngày hôm qua" -> API từ chối, trả lỗi 400 Bad Request.

[FR-STF-05] Cập nhật trạng thái và đóng hồ sơ khi hoàn tất

- Mô tả: Nhân viên cập nhật kết quả xử lý (Hoàn tất hoặc Từ chối). Bắt buộc phải có lý do/căn cứ.

- Quy tắc nghiệp vụ:

  - State Machine (Cỗ máy trạng thái): Backend phải định nghĩa một mảng map: MỚI chỉ được sang ĐANG_XỬ_LÝ; ĐANG_XỬ_LÝ chỉ được sang CHỜ_BỔ_SUNG, HOÀN_TẤT, TỪ_CHỐI. Bất kỳ yêu cầu chuyển trạng thái nào sai luồng phải bị chặn đứng (Throw Exception 422).

  - Validation: Khi chuyển sang HOÀN_TẤT hoặc TỪ_CHỐI, bắt buộc kiểm tra trường resolution_note (Nội dung kết luận) length > 10 ký tự.

- Test Cases:

  - Negative Case: Dùng Postman bắn API đổi trạng thái một đơn MỚI_TIẾP_NHẬN thẳng sang HOÀN_TẤT -> Server trả lỗi 422 Unprocessable Entity (Sai luồng trạng thái).

  - Negative Case: Bấm "Từ chối" nhưng để trống lý do -> Nút Submit bị disable, hoặc API trả lỗi 400 Validation.

[FR-STF-06] Chỉ định nhân viên phụ trách (Claim) - QUAN TRỌNG NHẤT

- Mô tả: Đơn mới đổ về kho chung của phòng, nhân viên tự bấm "Nhận xử lý" để chuyển đơn đó về mình. Chống việc gán việc cho đồng nghiệp.

- Quy tắc nghiệp vụ:

  - Chống Race Condition (Tranh chấp dữ liệu): Bắt buộc viết câu SQL Atomic (nguyên tử) như sau: UPDATE tickets SET assignee_id = {my_user_id}, status = 'ĐANG_XỬ_LÝ' WHERE id = {ticket_id} AND assignee_id IS NULL;

  - Sau khi chạy câu lệnh, Backend kiểm tra affected_rows. Nếu = 0, chứng tỏ đã có người khác nhanh tay nhận trước -> Trả về HTTP Status 409 Conflict (Hồ sơ đã có người nhận).

  - Không dùng: Tuyệt đối cấm code theo logic: SELECT assignee_id -> if (null) -> UPDATE. (Luồng này sẽ chết nếu 2 người click cùng 1 mili-giây).

- Test Cases:

  - Concurrency: Nhân viên A và B cùng mở màn hình kho chung. Cả 2 cùng bấm "Nhận" một hồ sơ chính xác cùng một phần nghìn giây (dùng tool giả lập) -> Một người nhận 200 OK, người còn lại nhận 409 Conflict. Chỉ có 1 người được gán tên vào Ticket.

[FR-STF-07] Thiết lập và nâng mức độ ưu tiên tự động

- Mô tả: Tự động cảnh báo (đổi màu, gửi noti) khi hồ sơ sắp trễ hạn hoặc đã trễ hạn SLA.

- Quy tắc nghiệp vụ:

  - Background Cronjob: Viết một Worker chạy ngầm mỗi 5-10 phút.

  - Công thức SLA: Lấy (Thời gian hiện tại - Thời gian tạo) / Target_SLA. (Lưu ý: Thời gian trôi qua phải viết hàm trừ đi các ngày nghỉ T7, CN và ngoài giờ hành chính).

  - Nếu tỷ lệ > 0.7: Update cột urgency = 'CAO', trigger noti cho Nhân viên.

  - Nếu tỷ lệ >= 1.0: Update cột urgency = 'KHẨN_CẤP', trigger noti cho Quản lý. Cấm UI/Frontend ghi đè tắt tính năng này.

- Test Cases:

  - Time Travel: Sửa created_at của một ticket lùi về 3 ngày trước (quá hạn SLA). Đợi tối đa 5 phút cho Cronjob chạy -> Kiểm tra UI thấy ticket chuyển màu đỏ và có chuông thông báo đẩy về.

[FR-STF-08] Chuyển hồ sơ sang phòng ban khác

- Mô tả: Nhân viên nhận nhầm hồ sơ có thể xin chuyển sang phòng khác, nhưng phải qua Quản lý duyệt.

- Quy tắc nghiệp vụ:

  - Luồng: Sinh ra trạng thái phụ CHỜ_DUYỆT_CHUYỂN. Cột proposed_dept_id lưu ID phòng ban mới.

  - Authorization: API /tickets/{id}/approve-transfer chỉ cho phép JWT Token có Role=MANAGER và dept_id trùng với phòng ban hiện tại của ticket mới được gọi.

  - Thực thi: Khi duyệt -> UPDATE department_id = proposed_dept_id, assignee_id = NULL, status = 'MỚI_TIẾP_NHẬN' (để tính lại SLA cho phòng mới từ số 0).

- Test Cases:

  - Authorization: Nhân viên (Role STAFF) thử gọi API /approve-transfer bằng Postman -> Trả lỗi 403 Forbidden (Không đủ thẩm quyền).

#### 📊 MOD-MGT: CỔNG QUẢN LÝ & BÁO CÁO

[FR-MGT-01] -> [FR-MGT-04] Báo cáo Dashboard, Khối lượng công việc, Điểm đánh giá

- Mô tả: Bảng điều khiển dành cho lãnh đạo xem tổng quan toàn trường, đo lường hiệu suất phòng ban/nhân viên, và tỷ lệ hài lòng (CSAT).

- Quy tắc nghiệp vụ:

  - CQRS / Read Replica: Trên hệ thống lớn (hàng trăm ngàn tickets), việc chạy các lệnh COUNT, GROUP BY, AVG trực tiếp trên bảng chính sẽ gây Deadlock (khóa bảng). Bắt buộc phải tạo Materialized View (PostgreSQL) để cache dữ liệu thống kê mỗi 15 phút, hoặc cấu hình DB để các câu lệnh SELECT báo cáo chỉ chạy trên con Database Slave (Read-only Replica).

  - Timezone (Múi giờ): Trong DB lưu chuẩn UTC (GMT+0). Khi query thống kê theo "Ngày/Tuần/Tháng", Backend bắt buộc phải convert thời gian về múi giờ VN AT TIME ZONE 'Asia/Ho_Chi_Minh' trong câu SQL, nếu không báo cáo sẽ bị lệch ngày.

- Test Cases:

  - Performance: Dùng JMeter giả lập 500 sinh viên đang submit form tạo đơn liên tục. Cùng lúc đó, bật màn hình Dashboard Quản lý -> Cả quá trình tạo đơn và tải báo cáo đều không bị timeout (đảm bảo tác vụ đọc/ghi không chặn nhau).

  - Data Accuracy: Có 2 vé hoàn thành (1 vé mất 2 giờ, 1 vé mất 4 giờ). Biểu đồ Thời gian xử lý trung bình hiển thị chính xác là 3 giờ.

#### 🔐 MOD-SEC: ĐỊNH DANH & BẢO MẬT (Lõi Hệ thống)

[FR-SEC-01] Đăng nhập SSO qua OIDC/OAuth 2.0 & Quản lý phiên

- Mô tả: Sinh viên/Nhân viên đăng nhập một chạm qua hệ thống của trường. Token có thời hạn, tự động làm mới ngầm.

- Quy tắc nghiệp vụ:

  - Không lưu Token bừa bãi: Giao diện Web tuyệt đối không lưu Refresh Token vào localStorage hay sessionStorage (Dễ dính XSS).

  - Cookie Rule: Refresh Token (TTL 8 giờ) phải được Backend set qua Header: Set-Cookie: refresh_token=...; HttpOnly; Secure; SameSite=Strict. Access Token (JWT - TTL 15 phút) lưu trên memory của Browser.

  - Silent Refresh: Khi Access Token hết hạn, API gọi lên sẽ báo 401. Frontend tự động gọi API /refresh ngầm để đổi Token mới và gọi lại API bị lỗi ban đầu mà người dùng không hề hay biết (Axios Interceptors).

- Test Cases:

  - XSS Protection: Đăng nhập thành công. Mở Console (F12) gõ document.cookie -> Không thể nhìn thấy chuỗi Refresh Token (HttpOnly hoạt động).

  - Silent Refresh: Treo tab 16 phút, sau đó bấm xem 1 hồ sơ -> Vẫn lấy được data bình thường không bị văng ra trang Login.

[FR-SEC-02] & [FR-SEC-03] Phân quyền Role (RBAC) & Chống IDOR

- Mô tả: Ngăn chặn tuyệt đối việc người dùng xem/sửa hồ sơ không thuộc thẩm quyền của mình bằng cách mò/đổi ID trên URL.

- Quy tắc nghiệp vụ:

  - Global Middleware: Mọi endpoint /api/* phải có Guard/Middleware kiểm tra Role từ chuỗi JWT.

  - Chống IDOR (Insecure Direct Object Reference): Đừng bao giờ tin tưởng ID. Câu SQL lấy chi tiết hồ sơ phải được viết:

    - Nếu Role = STU: SELECT * FROM tickets WHERE id = {id} AND created_by = {user_id}.

    - Nếu Role = STF: SELECT * FROM tickets WHERE id = {id} AND department_id = {user_dept_id}.

  - Nếu không tìm thấy, trả về HTTP 404 Not Found, không trả về HTTP 403. (Trả 403 sẽ tiết lộ cho hacker biết ID đó thực sự tồn tại).

- Test Cases:

  - IDOR: Sinh viên A (id=1) tạo ticket #100. Sinh viên B (id=2) gọi API GET /api/tickets/100 -> Server trả về 404 Not Found.

[FR-SEC-04] Chỉ người có quyền mới truy cập được tệp đính kèm

- Mô tả: File đính kèm lưu ở kho riêng, không phải ai có link cũng xem được.

- Quy tắc nghiệp vụ:

  - Bucket trên AWS S3 / MinIO phải set Block Public Access.

  - Client gọi API GET /files/{file_id} -> Backend lấy file_id dò ra ticket_id -> Chạy logic check quyền chống IDOR (ở [FR-SEC-03]) -> Nếu pass, gọi AWS SDK sinh ra Pre-signed URL hiệu lực 300s -> Trả link về cho Client redirect hoặc mở iframe.

- Test Cases:

  - Lấy Pre-signed URL hợp lệ, share cho bạn bè không có tài khoản bấm vào xem -> Vẫn xem được trong vòng 5 phút (Tính chất của presigned). Hết 5 phút -> Lỗi <Code>ExpiredToken</Code>.

[FR-SEC-05] Ghi nhật ký hệ thống (Ai, làm gì, lúc nào)

- Mô tả: Audit Log bất biến (Immutable) lưu vết hành động để phục vụ thanh tra.

- Quy tắc nghiệp vụ:

  - Tạo bảng System_Audit_Logs (actor_id, action, target_entity, payload, ip_address, created_at).

  - Database Permission: Cấp quyền cho Database User mà Backend đang kết nối là GRANT INSERT ON System_Audit_Logs. Thu hồi (Revoke) hoàn toàn quyền UPDATE và DELETE.

- Test Cases:

  - Dev cố tình viết 1 API gọi hàm ORM AuditLog.delete() -> Gọi API, Database ném ra Exception "Permission Denied". Dữ liệu không bị xóa.

#### 📱 MOD-GEN: YÊU CẦU CHUNG (UI/UX)

[FR-GEN-01] Giao diện responsive trên web PC & Mobile

- Mô tả: Hiển thị tự động co giãn không bị vỡ bố cục trên điện thoại.

- Quy tắc nghiệp vụ:

  - Bắt buộc dùng kĩ thuật CSS Flexbox hoặc CSS Grid (khuyên dùng thư viện Tailwind CSS).

  - Đối với các bảng (Table) nhiều cột, phải wrap bằng thẻ div có CSS overflow-x-auto để cuộn ngang trên điện thoại, không được bóp nát text.

  - Chống zoom: Bổ sung thẻ <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1"> để khi người dùng iPhone gõ SĐT, màn hình không bị zoom cận cảnh (lỗi rất phổ biến).

- Test Cases:

  - Mở trình duyệt PC, F12 thu nhỏ màn hình về kích thước iPhone 12 (390px) -> Form nhập liệu 2 cột tự động gập xuống thành 1 cột. Bảng danh sách cuộn ngang được mượt mà, không xuất hiện thanh cuộn ngang ở thẻ Body.

[FR-GEN-02] Song ngữ VN/EN

- Mô tả: Đổi ngôn ngữ UI mà không cần tải lại trang. Các dữ liệu cấu hình như Tên phòng ban cũng phải hiển thị đúng ngôn ngữ.

- Quy tắc nghiệp vụ :

  - Text tĩnh (UI Labels): Dùng thư viện i18n (ví dụ: react-i18next). Tạo 2 file vi.json và en.json.

  - Text động (Database): Trong Database, bảng Departments và Categories phải thiết kế thành 2 cột: name_vi và name_en. Backend lấy header Accept-Language từ HTTP Request để tự động map trả về đúng cột name. Text sinh viên tự gõ (Mô tả lỗi) thì giữ nguyên gốc, không gọi API Google Dịch.

- Test Cases:

  - Đổi nút VN sang EN -> Toàn bộ menu, label form chuyển tiếng Anh. Tên phòng ban trên Dropdown chuyển từ "Phòng Đào tạo" thành "Academic Affairs Office". Cấu hình ngôn ngữ được lưu vào LocalStorage để lần sau vào web không phải chọn lại.

## 6. Yêu cầu phi chức năng

| Nhóm | Yêu cầu |
| --- | --- |
| Quy mô | ~3.000 sinh viên đăng ký, 100–200 người dùng đồng thời |
| Hiệu năng | Phụ thuộc hạ tầng trường; nhóm đo và báo cáo trên môi trường Staging |
| Bảo mật | Mức cơ bản: HTTPS, phân quyền, bảo vệ tệp, nhật ký hệ thống |
| Giao diện | Responsive mobile + desktop; VN/EN chờ chốt (Q10) |
| Triển khai | Docker Compose, file cấu hình mẫu .env.example |
| Tài liệu API | OpenAPI/Swagger |
