# M2. Cổng Nhân viên – Functional Requirement Specification

> Cập nhật: 09/10/2026. Giữ nguyên 42 chức năng của 5 module trong repo; bổ sung và thống nhất đặc tả trực tiếp tại module. Các giả định chưa được trường xác nhận được ghi tại phần đầu module.

**Mục đích module:** Hàng đợi và xử lý/điều phối hồ sơ trong phòng ban hiện tại.

**Actor chính:** STAFF. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M4 xác thực/phạm vi; M5 trạng thái, SLA và lịch sử; M1 phản hồi sinh viên.

## Luồng xử lý và liên kết module

M1 tạo ticket NEW vào phòng → staff mở hàng đợi/chi tiết → phân công người nhận active cùng phòng → IN_PROGRESS → người phụ trách xem tệp/ghi note/xử lý → yêu cầu bổ sung nếu thiếu → student trả lời → xử lý tiếp → nhập resolution và đóng CLOSED. Sau đóng, student đánh giá và M3 tính báo cáo.

Nhánh chuyển phòng: mọi staff active cùng phòng chọn đích/reason → xóa assignee, giữ loại/status/SLA/info request → phòng cũ mất quyền, phòng mới phân công. Nhánh quá hạn do M5 chuyển OVERDUE; ticket vẫn xử lý/bổ sung được và giữ OVERDUE đến đóng.

**Input module:** phiên STAFF/phòng hiện tại, ticket/version, content/note/resolution, assignee, priority, đích chuyển và reason.
**Output module:** hàng đợi/chi tiết trong phạm vi, tệp, hồ sơ cập nhật, lịch sử nội bộ/công khai và sự kiện audit/thông báo theo quyền.

**Quyền:** mọi staff cùng phòng được điều phối; chỉ assignee xử lý. Không thêm bước manager duyệt chuyển hoặc chỉ cho tự claim. Nếu chưa phụ trách thì có thể chuyển với reason mà không cần ghi note trước.

**Cần xác nhận:** staff/phòng thực tế; cách tính SLA giờ lịch và không pause OVERDUE; việc chặn pause muộn trước cron. Nhóm xác nhận các quy tắc này trước khi triển khai nghiệp vụ liên quan.

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Xác thực:** Mọi API yêu cầu phiên ứng dụng hợp lệ theo SEC-01 và vai trò STAFF. Sai hoặc thiếu token trả 401. Sai vai trò trả 403.
- **Phòng ban của nhân viên:** Lấy từ cấu hình phân quyền (MGT-06), không nhận từ client. Tài khoản `STAFF` chưa được gán phòng ban trả 403 "Tài khoản chưa được gán phòng ban".
- **Phạm vi truy cập:**
  - Hồ sơ không thuộc phòng ban của nhân viên trả 404.
  - Thao tác **xử lý** (STF-04, STF-05, STF-06) chỉ dành cho nhân viên phụ trách. Nhân viên khác trong phòng ban gọi thì trả 403. Mã lỗi/phiên tuân theo quy ước chung.
  - Thao tác **điều phối** (STF-07 đến STF-10) thì mọi nhân viên trong phòng ban đều làm được.
- **Trạng thái hồ sơ:** `NEW`, `IN_PROGRESS`, `PENDING_INFO`, `OVERDUE`, `CLOSED`, theo CORE-02. Hồ sơ `CLOSED` chỉ được xem, không được thao tác gì thêm.
- **Cập nhật đồng thời:** Mỗi hồ sơ có trường `version`. Mọi thao tác ghi phải gửi kèm `version` hiện tại. Lệch `version` trả 409 "Hồ sơ đã được người khác cập nhật, vui lòng tải lại".
- **Vượt rate limit:** Trả 429 kèm `Retry-After`.
- **Nhật ký hệ thống:** Theo SEC-06. Mọi thay đổi nghiệp vụ hồ sơ ghi audit và lịch sử theo event ở CORE-06; tải/xem tệp chỉ ghi audit, không sinh mốc timeline hoặc tăng version.

---

### [FR-STF-01] Hàng đợi hồ sơ của phòng ban

**Mô tả**
Hiển thị danh sách hồ sơ thuộc phòng ban của nhân viên, lọc được theo trạng thái, giúp phòng ban không bỏ sót yêu cầu nào.

**Actor**
Nhân viên đã đăng nhập, đã được gán phòng ban.

**Preconditions**
- Nhân viên đã đăng nhập và có phòng ban.

**Luồng chính**
1. Nhân viên mở màn hình Hàng đợi.
2. Hệ thống trả về hồ sơ thuộc phòng ban của nhân viên.
3. Nhân viên lọc theo trạng thái, mức ưu tiên hoặc người phụ trách, hoặc tìm theo mã hồ sơ.
4. Nhân viên chọn một hồ sơ để xem chi tiết (FR-STF-02).

**Input**
- **Người dùng/request:** page=1; page_size=20, 1–100; statuses, priority, assignee_filter=my/unassigned/all; code chính xác.
- **Hệ thống:** STAFF active và phòng ban từ DB.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: items, total_items, total_pages, page, page_size, updated_at; item có code, loại, trạng thái, ưu tiên, người nhận, ngày gửi, due_at, sla_paused, remaining_seconds, needs_info.
- **Thay đổi/sự kiện:** Chỉ đọc và polling 30 giây; hồ sơ vừa chuyển đi không được trả nữa.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- remaining_seconds được tính theo CORE-03; PENDING_INFO hiện Chờ sinh viên/SLA tạm dừng, không hiển thị số âm do dùng due_at cũ. Mọi tham số phân trang phải là số nguyên dương. Cùng ưu tiên/ngày gửi thì ticket_id tăng dần.
- Chỉ trả về hồ sơ có `department_id` trùng với phòng ban của nhân viên.
- Bộ lọc:
  - Trạng thái (chọn nhiều).
  - Mức ưu tiên.
  - Người phụ trách: "Của tôi", "Chưa phân công", "Tất cả".
- Sắp xếp mặc định: ưu tiên Cao → Thấp, sau đó hồ sơ gửi sớm hơn đứng trước.
- Mỗi dòng hiển thị: mã hồ sơ, loại yêu cầu, trạng thái, mức ưu tiên, người phụ trách (hoặc "Chưa phân công"), ngày gửi, hạn xử lý theo SLA, dấu hiệu "Chờ sinh viên" nếu có yêu cầu bổ sung đang mở.
- 20 hồ sơ mỗi trang; `page_size` tối đa 100.
- Hàng đợi tự cập nhật bằng polling mỗi 30 giây.
- Rate limit: 120 request mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- Nếu tham số lọc hoặc phân trang không hợp lệ, hệ thống trả 400.
- Nếu nhân viên chưa được gán phòng ban, hệ thống trả 403.
- Nếu truy vấn quá 5 giây, hệ thống trả 503 và UI hiển thị nút Thử lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Phòng ban có 45 hồ sơ -> trang 1 có 20 hồ sơ, sắp xếp đúng theo mức ưu tiên rồi đến ngày gửi.
- **AC-02 [Validation]:** Lọc "Chưa phân công" -> chỉ trả về hồ sơ chưa có người phụ trách. Lọc trạng thái `ABC` -> 400.
- **AC-03 [Boundary]:** `page_size = 100` -> 200. `page_size = 101` -> 400.
- **AC-04 [Security]:** Truyền `department_id` của phòng ban khác trên query -> hệ thống bỏ qua, chỉ trả hồ sơ của phòng ban nhân viên.
- **AC-05 [Concurrency/Rate Limit]:** Hồ sơ mới gửi vào phòng ban trong lúc hàng đợi đang mở -> xuất hiện trong vòng 30 giây. Request thứ 121 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Cơ sở dữ liệu phản hồi quá 5 giây -> 503 với thông báo "Vui lòng thử lại", trang không bị trắng.

**Ví dụ Edge Case**
Một hồ sơ vừa được chuyển sang phòng ban khác trong lúc nhân viên đang xem hàng đợi. Ở lần polling tiếp theo, hồ sơ biến mất khỏi hàng đợi của phòng ban cũ. Nếu nhân viên bấm vào dòng cũ trước khi danh sách kịp cập nhật, hệ thống trả 404 "Hồ sơ không còn thuộc phòng ban của bạn".

**Expected Result:** Nhân viên luôn thấy đủ và đúng các hồ sơ của phòng ban mình, không thấy hồ sơ của phòng ban khác.

---

### [FR-STF-02] Xem thông tin sinh viên và nội dung yêu cầu

**Mô tả**
Nhân viên xem đầy đủ thông tin một hồ sơ để xử lý, không cần hỏi lại sinh viên những thông tin đã có.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên.

**Luồng chính**
1. Nhân viên chọn hồ sơ từ hàng đợi hoặc nhập mã hồ sơ.
2. Hệ thống trả về thông tin sinh viên, nội dung yêu cầu, danh sách tệp, nhật ký xử lý và lịch sử hồ sơ.
3. UI hiển thị các nút thao tác theo quyền: nút xử lý chỉ hiện với nhân viên phụ trách, nút điều phối hiện với mọi nhân viên trong phòng ban.

**Input**
- **Người dùng/request:** ticket_id hoặc code.
- **Hệ thống:** Phiên staff, phòng hiện tại; hồ sơ/snapshot/history/notes/files/info requests.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: đầy đủ dữ liệu hồ sơ trong phạm vi, version, allowed_actions; CLOSED có resolution và closed_at. allowed_actions chỉ để hỗ trợ UI.
- **Thay đổi/sự kiện:** Chỉ đọc; mọi thao tác sau phải kiểm tra lại quyền, không tin allowed_actions cũ.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Thông tin sinh viên hiển thị là bản lưu (snapshot) tại thời điểm tạo hồ sơ: họ tên, MSSV, khoa/lớp, email, SĐT. Trường trống hiển thị `--`.
- Nội dung hiển thị: mã hồ sơ, loại yêu cầu, mô tả, trạng thái, mức ưu tiên, người phụ trách, ngày gửi, hạn xử lý, các yêu cầu bổ sung và nội dung sinh viên đã bổ sung, nhật ký xử lý nội bộ, lịch sử hồ sơ; hồ sơ CLOSED có kết quả resolution và closed_at.
- Response trả về trường `version` để dùng cho các thao tác ghi.
- Rate limit: 120 request mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- Nếu mã hồ sơ sai định dạng, hệ thống trả 400.
- Nếu hồ sơ không tồn tại hoặc không thuộc phòng ban, hệ thống trả 404.
- Nếu không tải được lịch sử, hệ thống vẫn hiển thị thông tin chính và báo "Chưa tải được lịch sử".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Mở hồ sơ hợp lệ của phòng ban -> 200, hiển thị đủ thông tin sinh viên, nội dung, tệp, nhật ký và lịch sử.
- **AC-02 [Validation]:** Snapshot không có SĐT -> UI hiển thị `--`, không lỗi.
- **AC-03 [Security]:** Mở hồ sơ của phòng ban khác -> 404. Nhân viên không phụ trách mở hồ sơ -> 200, nhưng không thấy các nút xử lý.
- **AC-04 [Concurrency]:** Hai nhân viên cùng mở một hồ sơ -> cả hai nhận cùng giá trị `version`. Sau khi một người cập nhật, người kia thấy `version` mới khi tải lại.
- **AC-05 [Boundary]:** Hồ sơ `CLOSED` -> hiển thị đầy đủ ở chế độ chỉ đọc, không có nút thao tác.
- **AC-06 [Resilience]:** API lịch sử lỗi -> thông tin chính vẫn hiển thị, không trắng trang.

**Ví dụ Edge Case**
Sinh viên đổi số điện thoại trên SSO sau khi đã gửi hồ sơ. Nhân viên vẫn thấy số cũ trong snapshot, vì đó là thông tin tại thời điểm gửi. Hồ sơ không bị cập nhật ngược.

**Expected Result:** Nhân viên của đúng phòng ban thấy đầy đủ thông tin cần để xử lý, và chỉ thấy những thao tác mình được phép làm.

---

### [FR-STF-03] Xem và tải tệp đính kèm

**Mô tả**
Nhân viên xem trực tiếp hoặc tải tệp minh chứng của hồ sơ để kiểm tra khi xử lý.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên.
- Hồ sơ có ít nhất một tệp đính kèm.

**Luồng chính**
1. Nhân viên chọn một tệp trong chi tiết hồ sơ.
2. Hệ thống kiểm tra quyền.
3. Hệ thống trả tệp. PDF và ảnh mở xem ngay trên trình duyệt, đồng thời có nút Tải về.
4. Hệ thống ghi nhật ký.

**Input**
- **Người dùng/request:** file_id; mode=inline hoặc download.
- **Hệ thống:** Phiên staff active; quyền phòng hiện tại với hồ sơ chứa file.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: nội dung tệp và header MIME/tên/cache; 404 ngoài phạm vi, 503 storage/audit lỗi.
- **Thay đổi/sự kiện:** Audit khởi tạo tải và kết quả theo SEC-05; không tăng version, không tạo mốc timeline khi đọc tệp.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Tệp chỉ được trả qua API có kiểm tra quyền, không có đường dẫn công khai (SEC-05).
- Tệp trả về với tên gốc và đúng `Content-Type`.
- Mỗi lần xem hoặc tải đều ghi nhật ký `DOWNLOAD_FILE` (người thực hiện, thời gian, tệp, hồ sơ).
- Rate limit: 30 lần tải mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- Nếu tệp không thuộc hồ sơ của phòng ban, hệ thống trả 404.
- Nếu metadata có quyền nhưng object mất, trả 404 chung; audit khởi tạo và FILE_READ_RESULT=STORAGE_FAILED/OBJECT_MISSING theo SEC-05. Log kỹ thuật không có đường dẫn/token.
- Nếu kho lưu trữ không phản hồi, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Mở một tệp PDF -> hiển thị trên trình duyệt với đúng tên gốc.
- **AC-02 [Validation]:** Gọi API với `file_id` không tồn tại -> 404.
- **AC-03 [Security]:** Nhân viên phòng ban khác gọi trực tiếp API tải tệp -> 404. Gọi API không có token -> 401.
- **AC-04 [Rate Limit]:** Lần tải thứ 31 trong 1 phút -> 429.
- **AC-05 [Boundary]:** Tải tệp 10MB -> tải trọn vẹn, đúng dung lượng.
- **AC-06 [Resilience & Audit]:** Kho lưu trữ không phản hồi -> 503, ứng dụng không treo. Mỗi lần tải thành công -> có một bản ghi `DOWNLOAD_FILE`.

**Ví dụ Edge Case**
Hồ sơ vừa được chuyển sang phòng ban khác trong lúc nhân viên phòng cũ còn mở trang chi tiết và bấm tải tệp. Hệ thống kiểm tra quyền tại thời điểm tải, nên trả 404 và không ghi nhật ký tải.

**Expected Result:** Chỉ nhân viên đang có quyền với hồ sơ mới mở được tệp, và mọi lần mở đều được ghi lại.

---

### [FR-STF-04] Ghi nhật ký xử lý

**Mô tả**
Nhân viên phụ trách ghi lại các bước đã làm với hồ sơ, giúp người khác nắm được lịch sử xử lý khi tiếp nhận hoặc kiểm tra lại.

**Actor**
Nhân viên phụ trách hồ sơ.

**Preconditions**
- Nhân viên là người phụ trách hồ sơ.
- Hồ sơ không ở trạng thái `CLOSED`.

**Luồng chính**
1. Nhân viên nhập nội dung vào ô Nhật ký xử lý.
2. Nhân viên bấm Lưu.
3. Hệ thống lưu ghi chú kèm người ghi và thời gian.
4. Ghi chú xuất hiện trong phần nhật ký của hồ sơ.

**Input**
- **Người dùng/request:** ticket_id, version, content; header Idempotency-Key.
- **Hệ thống:** STAFF active là người phụ trách; hồ sơ chưa CLOSED.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: note_id, content, actor, created_at, ticket_id, version mới cho staff có quyền.
- **Thay đổi/sự kiện:** Tạo note và timeline nội bộ; trạng thái không đổi; audit chỉ tham chiếu note.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Gửi version và tăng version một lần theo quy ước chung. Staff không phụ trách vẫn được chuyển phòng bằng STF-09 với reason bắt buộc; không cần tạo note trước nếu không có quyền ghi note.
- `content`: bắt buộc, dài 1–2000 ký tự sau khi trim.
- Ghi chú chỉ hiển thị cho nhân viên phòng ban đang giữ hồ sơ, không hiển thị cho sinh viên.
- Ghi chú không được sửa hoặc xóa sau khi lưu.
- Ghi chú lưu dạng văn bản thuần và escape khi hiển thị.
- Dùng để ghi lại cả trường hợp hồ sơ bị gửi sai phòng ban.
- Ghi chú không làm đổi trạng thái hồ sơ.
- Bắt buộc có `Idempotency-Key`. Rate limit: 30 ghi chú mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- Nếu nội dung rỗng hoặc vượt 2000 ký tự, hệ thống trả 400.
- Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Người phụ trách lưu ghi chú 100 ký tự -> 201; ghi chú hiển thị kèm tên người ghi và thời gian; trạng thái hồ sơ không đổi.
- **AC-02 [Validation/Boundary]:** Nội dung toàn khoảng trắng -> 400. Đúng 2000 ký tự -> 201. 2001 ký tự -> 400.
- **AC-03 [Security]:** Nhân viên cùng phòng ban nhưng không phụ trách -> 403. Sinh viên xem hồ sơ -> không thấy ghi chú nội bộ.
- **AC-04 [Idempotency]:** Bấm Lưu 2 lần cùng `Idempotency-Key` -> chỉ có 1 ghi chú.
- **AC-05 [Business Logic]:** Ghi chú vào hồ sơ `CLOSED` -> 409.
- **AC-06 [Resilience & Audit]:** Lưu ghi chú lỗi -> 500, UI giữ nội dung đã nhập để lưu lại. Lưu thành công -> có bản ghi nhật ký `ADD_NOTE`.

**Ví dụ Edge Case**
Nhân viên phụ trách nhận thấy nội dung hồ sơ không thuộc trách nhiệm phòng mình. Người này ghi chú "Hồ sơ thuộc Phòng Đào tạo" và dùng chức năng chuyển phòng ban (FR-STF-09). Ghi chú vẫn được lưu lại trong hồ sơ để phòng ban mới đọc được lý do.

**Expected Result:** Mỗi ghi chú hợp lệ được lưu đúng một lần, không sửa được, và chỉ nội bộ đọc được.

---

### [FR-STF-05] Yêu cầu sinh viên bổ sung thông tin

**Mô tả**
Nhân viên phụ trách gửi yêu cầu sinh viên bổ sung thông tin khi hồ sơ chưa đủ dữ liệu để xử lý.

**Actor**
Nhân viên phụ trách hồ sơ.

**Preconditions**
- Nhân viên là người phụ trách hồ sơ.
- Hồ sơ ở `IN_PROGRESS` hoặc `OVERDUE`.
- Hồ sơ chưa có yêu cầu bổ sung nào đang mở.

**Luồng chính**
1. Nhân viên bấm Yêu cầu bổ sung.
2. Nhân viên nhập nội dung cần sinh viên cung cấp.
3. Nhân viên bấm Gửi.
4. Hệ thống tạo một yêu cầu bổ sung ở trạng thái mở.
5. Nếu hồ sơ đang `IN_PROGRESS`, hệ thống chuyển sang `PENDING_INFO` và tạm dừng tính SLA. Nếu hồ sơ đang `OVERDUE`, trạng thái giữ nguyên.
6. Hệ thống tạo thông báo cho sinh viên (STU-08).

**Input**
- **Người dùng/request:** ticket_id, version, content; header Idempotency-Key.
- **Hệ thống:** Người phụ trách, trạng thái IN_PROGRESS/OVERDUE, chưa có info request mở.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: info_request_id, content, created_at, status, version mới, sla_paused và due_at theo quyền staff.
- **Thay đổi/sự kiện:** Tạo yêu cầu mở; pause nếu IN_PROGRESS chuyển PENDING_INFO; một thông báo cho sinh viên; history/audit.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Nếu status còn IN_PROGRESS nhưng now > due_at trước lần cron kế tiếp, không cho chuyển PENDING_INFO để tạm dừng muộn: trả 409 kèm yêu cầu tải lại; job quá hạn sẽ đổi OVERDUE. Sau đó người phụ trách gửi lại yêu cầu, trạng thái giữ OVERDUE. Retry thành công cũ theo quy ước idempotency.
- `content`: bắt buộc, dài 10–1000 ký tự sau khi trim. Nội dung này sinh viên đọc được.
- Mỗi hồ sơ chỉ có tối đa một yêu cầu bổ sung đang mở.
- Yêu cầu bổ sung đóng lại khi sinh viên trả lời (STU-09) hoặc khi hồ sơ được đóng (FR-STF-06).
- Bắt buộc gửi `version` và `Idempotency-Key`.

**Alternative / Error Flows**
- Nếu nội dung sai độ dài, hệ thống trả 400.
- Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- Nếu hồ sơ đã có yêu cầu bổ sung đang mở hoặc ở `NEW` hay `CLOSED`, hệ thống trả 409.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `IN_PROGRESS`, gửi yêu cầu hợp lệ -> 201; hồ sơ chuyển `PENDING_INFO`; SLA tạm dừng; sinh viên nhận thông báo.
- **AC-02 [Boundary]:** Hồ sơ `OVERDUE`, gửi yêu cầu -> 201; trạng thái vẫn `OVERDUE`; sinh viên vẫn nhận thông báo.
- **AC-03 [Validation]:** Nội dung 9 ký tự -> 400. Hồ sơ đang `PENDING_INFO` -> 409 "Đã có yêu cầu bổ sung đang mở".
- **AC-04 [Security]:** Nhân viên không phụ trách gửi yêu cầu -> 403.
- **AC-05 [Concurrency/Idempotency]:** Bấm Gửi 2 lần cùng `Idempotency-Key` -> 1 yêu cầu, 1 thông báo. Gửi với version cũ khác key -> 409. Retry cùng key đã thành công trả kết quả cũ dù version đổi; cùng key payload khác 409. now > due_at khi còn IN_PROGRESS -> 409, không tạo pause muộn.
- **AC-06 [Resilience & Audit]:** Tạo thông báo lỗi -> rollback cả yêu cầu bổ sung và việc đổi trạng thái. Thành công -> có bản ghi nhật ký `REQUEST_INFO` và mốc trong lịch sử hồ sơ.

**Ví dụ Edge Case**
Hồ sơ `IN_PROGRESS` còn 2 giờ là đến hạn SLA thì nhân viên gửi yêu cầu bổ sung. SLA dừng lại, nên hồ sơ không chuyển `OVERDUE` trong thời gian chờ sinh viên. Khi sinh viên bổ sung xong, SLA chạy tiếp với 2 giờ còn lại.

**Expected Result:** Mỗi lần gửi hợp lệ tạo đúng một yêu cầu bổ sung đang mở, trạng thái và SLA thay đổi đúng theo CORE-02 và CORE-03.

---

### [FR-STF-06] Đóng hồ sơ

**Mô tả**
Nhân viên phụ trách đóng hồ sơ khi đã xử lý xong, thông báo kết quả cho sinh viên và kết thúc tính SLA.

**Actor**
Nhân viên phụ trách hồ sơ.

**Preconditions**
- Nhân viên là người phụ trách hồ sơ.
- Hồ sơ ở `IN_PROGRESS`, `PENDING_INFO` hoặc `OVERDUE`.

**Luồng chính**
1. Nhân viên bấm Đóng hồ sơ.
2. Nhân viên nhập kết quả xử lý.
3. Nhân viên xác nhận.
4. Hệ thống chuyển hồ sơ sang `CLOSED`, dừng SLA, đóng yêu cầu bổ sung đang mở (nếu có) và ghi thời điểm đóng.
5. Hệ thống tạo thông báo cho sinh viên (STU-08). Sinh viên có thể đánh giá (STU-10).

**Input**
- **Người dùng/request:** ticket_id, version, resolution; header Idempotency-Key.
- **Hệ thống:** Người phụ trách; trạng thái IN_PROGRESS/PENDING_INFO/OVERDUE.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: ticket_id/code, status=CLOSED, resolution, closed_at, version mới.
- **Thay đổi/sự kiện:** Đóng ticket/yêu cầu bổ sung; kết thúc pause cuối nếu có; history/audit, một thông báo sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- `resolution`: bắt buộc, dài 10–2000 ký tự sau khi trim. Sinh viên đọc được.
- Thời điểm đóng được dùng để tính thời gian xử lý trung bình (MGT-02).
- Hồ sơ `CLOSED` không thể chuyển sang trạng thái khác (không có chức năng mở lại).
- Bắt buộc gửi `version` và `Idempotency-Key`.

**Alternative / Error Flows**
- Nếu thiếu kết quả xử lý hoặc sai độ dài, hệ thống trả 400.
- Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- Nếu hồ sơ đã `CLOSED` hoặc đang `NEW`, hệ thống trả 409.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `IN_PROGRESS`, nhập kết quả hợp lệ -> 200; hồ sơ chuyển `CLOSED`; sinh viên nhận thông báo và thấy form đánh giá.
- **AC-02 [Boundary]:** PENDING_INFO có info request mở -> close hợp lệ, request đóng, cộng bù pause cuối đến closed_at, version tăng một lần; retry cùng key trả kết quả cũ, không thêm history/noti.
- **AC-03 [Validation]:** Kết quả xử lý 9 ký tự -> 400. Đóng hồ sơ đã `CLOSED` -> 409.
- **AC-04 [Security]:** Nhân viên không phụ trách đóng hồ sơ -> 403.
- **AC-05 [Concurrency]:** Nhân viên đóng và sinh viên bổ sung dùng cùng version đã đọc -> chỉ một thao tác commit; bên kia nhận 409. Nếu bên đóng tải version mới sau bổ sung rồi đóng thì hợp lệ.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, trạng thái giữ nguyên. Thành công -> có bản ghi nhật ký `CLOSE_TICKET` và mốc trong lịch sử hồ sơ.

**Ví dụ Edge Case**
Sinh viên không trả lời yêu cầu bổ sung sau một thời gian dài. Nhân viên đóng hồ sơ với kết quả "Không nhận được thông tin bổ sung". Yêu cầu bổ sung được đóng theo, và sinh viên không còn gửi bổ sung được cho hồ sơ này.

**Expected Result:** Hồ sơ đóng đúng một lần, sinh viên nhận được kết quả xử lý, và hồ sơ được tính vào báo cáo.

---

### [FR-STF-07] Phân công nhân viên phụ trách

**Mô tả**
Nhân viên trong phòng ban phân công người phụ trách cho hồ sơ, để mọi hồ sơ đều có người chịu trách nhiệm rõ ràng.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên và không ở `CLOSED`.
- Người được phân công là tài khoản `STAFF` thuộc cùng phòng ban.

**Luồng chính**
1. Nhân viên bấm Phân công.
2. Hệ thống hiển thị danh sách nhân viên của phòng ban.
3. Nhân viên chọn người phụ trách (có thể chọn chính mình) và xác nhận.
4. Hệ thống gán người phụ trách. Nếu hồ sơ đang `NEW`, hệ thống chuyển sang `IN_PROGRESS`.

**Input**
- **Người dùng/request:** Danh sách người nhận: ticket_id; phân công: ticket_id, assignee_id, version.
- **Hệ thống:** STAFF active cùng phòng; người nhận phải STAFF active cùng phòng.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Danh sách 200 gồm id/tên người nhận active; cập nhật 200 có assignee, status và version mới hoặc version hiện tại nếu no-op.
- **Thay đổi/sự kiện:** Gán/đổi người nhận; NEW → IN_PROGRESS; history/audit ASSIGN; chỉ thông báo nếu đổi trạng thái.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Người nhận phải active; API danh sách chỉ trả STAFF active trong phòng, backend kiểm tra lại khi lưu. Kiểm tra phân công và đổi vai trò/phòng/vô hiệu hóa tài khoản phải bảo vệ cùng ràng buộc một cách nguyên tử; không để hồ sơ mở gán người mất quyền. Người nhận inactive trả 422.
- Khi NEW đã vượt hạn nhưng cron chưa chạy, phân công vẫn hợp lệ và không reset hạn; job chuyển OVERDUE theo due_at. Phân công không che trạng thái quá hạn.
- Phân công lại: được phép đổi người phụ trách khi hồ sơ ở `IN_PROGRESS`, `PENDING_INFO` hoặc `OVERDUE`. Trạng thái giữ nguyên.
- Hồ sơ `OVERDUE` chưa từng được phân công khi được phân công thì vẫn giữ `OVERDUE` (CORE-02).
- Phân công trùng người phụ trách hiện tại trả 200 và không ghi thêm lịch sử.
- Bắt buộc gửi `version`.

**Alternative / Error Flows**
- Nếu người được chọn không thuộc phòng ban hoặc không phải `STAFF`, hệ thống trả 422.
- Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `NEW`, phân công nhân viên A -> 200; hồ sơ chuyển `IN_PROGRESS`; sinh viên nhận thông báo đổi trạng thái.
- **AC-02 [Validation]:** Chọn người phòng khác hoặc inactive -> 422. Phân công cạnh tranh deactivate/đổi phòng: không để ticket mở gán người mất quyền, một thao tác bị chặn nguyên tử.
- **AC-03 [Boundary]:** Hồ sơ `OVERDUE` chưa có người phụ trách, phân công A -> 200, trạng thái vẫn `OVERDUE`.
- **AC-04 [Security]:** Nhân viên phòng ban khác gọi API phân công -> 404.
- **AC-05 [Concurrency/Idempotency]:** Hai nhân viên phân công cùng lúc với cùng `version` -> người đầu thành công, người sau nhận 409. Phân công lại đúng người hiện tại -> 200, không có mốc lịch sử mới.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `ASSIGN` (người cũ, người mới).

**Ví dụ Edge Case**
Nhân viên A đang phụ trách nghỉ phép dài ngày. Nhân viên B trong phòng ban phân công lại hồ sơ cho C. Trạng thái giữ nguyên, C có quyền xử lý ngay, còn A mất quyền xử lý nhưng vẫn xem được hồ sơ vì cùng phòng ban.

**Expected Result:** Mỗi hồ sơ chưa đóng có tối đa một người phụ trách thuộc đúng phòng ban, và mọi lần đổi đều được ghi lại.

---

### [FR-STF-08] Đặt mức ưu tiên

**Mô tả**
Nhân viên trong phòng ban đặt mức ưu tiên cho hồ sơ để phòng ban xử lý việc gấp trước.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên và không ở `CLOSED`.

**Luồng chính**
1. Nhân viên chọn mức ưu tiên mới.
2. Nhân viên xác nhận.
3. Hệ thống cập nhật mức ưu tiên.

**Input**
- **Người dùng/request:** ticket_id, version, priority=LOW/MEDIUM/HIGH.
- **Hệ thống:** Staff active cùng phòng, ticket chưa CLOSED.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: priority, status, version mới; no-op trả version hiện tại.
- **Thay đổi/sự kiện:** Đổi ưu tiên, không đổi SLA/status; history nội bộ và CHANGE_PRIORITY nếu dữ liệu đổi.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Giá trị hợp lệ: `LOW`, `MEDIUM`, `HIGH` (Thấp, Trung bình, Cao), theo CORE-04. Được tăng hoặc giảm.
- Mức ưu tiên dùng để sắp xếp và lọc, không làm thay đổi SLA.
- Đặt trùng mức hiện tại trả 200 và không ghi thêm lịch sử.
- Bắt buộc gửi `version`.

**Alternative / Error Flows**
- Nếu giá trị ngoài 3 mức, hệ thống trả 400.
- Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Đổi Trung bình -> Thấp -> 200; hàng đợi sắp xếp lại theo mức mới.
- **AC-02 [Validation]:** Gửi `URGENT` -> 400.
- **AC-03 [Boundary]:** Đặt trùng mức hiện tại -> 200, không có mốc lịch sử mới.
- **AC-04 [Security]:** Nhân viên phòng ban khác -> 404.
- **AC-05 [Concurrency]:** Hai nhân viên đổi mức ưu tiên cùng lúc -> người sau nhận 409.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `CHANGE_PRIORITY` (mức cũ, mức mới).

**Ví dụ Edge Case**
Hồ sơ đang `OVERDUE` bị hạ mức ưu tiên xuống Thấp. Hồ sơ vẫn giữ `OVERDUE`, vì mức ưu tiên không ảnh hưởng SLA, và vẫn được tính vào số hồ sơ quá hạn trên dashboard.

**Expected Result:** Mức ưu tiên đổi đúng, không ảnh hưởng SLA, và mọi lần đổi đều được ghi lại.

---

### [FR-STF-09] Chuyển hồ sơ sang phòng ban khác

**Mô tả**
Nhân viên chuyển hồ sơ sang đúng phòng ban khi hồ sơ không thuộc trách nhiệm của phòng mình, để sinh viên không phải gửi lại yêu cầu.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên và không ở `CLOSED`.

**Luồng chính**
1. Nhân viên bấm Chuyển phòng ban.
2. Nhân viên chọn phòng ban đích và nhập lý do.
3. Nhân viên xác nhận.
4. Hệ thống đổi phòng ban của hồ sơ và bỏ người phụ trách hiện tại.
5. Hồ sơ xuất hiện trong hàng đợi của phòng ban mới ở mục "Chưa phân công".

**Input**
- **Người dùng/request:** ticket_id, version, target_department_id, reason.
- **Hệ thống:** Staff active cùng phòng; phòng đích hợp lệ khác phòng hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: code, department mới, assignee=null, status và version mới. Sau response này caller phòng cũ không có quyền tải lại chi tiết.
- **Thay đổi/sự kiện:** Chuyển phòng, xóa người nhận; giữ loại/status/hạn/info request; public history chỉ tên phòng mới, reason nội bộ; audit tham chiếu.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Phòng ban đích phải tồn tại và khác phòng ban hiện tại.
- `reason`: bắt buộc, dài 10–500 ký tự. Chỉ nội bộ đọc được.
- Sau khi chuyển:
  - Trạng thái hồ sơ giữ nguyên.
  - SLA không bị đặt lại.
  - Yêu cầu bổ sung đang mở (nếu có) vẫn giữ.
  - Loại yêu cầu không đổi.
- Phòng ban cũ mất quyền truy cập hồ sơ ngay khi chuyển xong.
- Sinh viên thấy phòng ban mới trong danh sách và chi tiết hồ sơ (STU-06, STU-07). Việc chuyển phòng ban không tạo thông báo vì không phải đổi trạng thái.
- Bắt buộc gửi `version`.

**Alternative / Error Flows**
- Nếu phòng ban đích trùng phòng ban hiện tại, hệ thống trả 400.
- Nếu phòng ban đích không tồn tại, hệ thống trả 422.
- Nếu thiếu lý do hoặc lý do sai độ dài, hệ thống trả 400.
- Nếu hồ sơ đã `CLOSED`, hoặc lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Chuyển hồ sơ `IN_PROGRESS` sang phòng ban B -> 200; hồ sơ nằm ở hàng đợi B, mục "Chưa phân công"; trạng thái vẫn `IN_PROGRESS`.
- **AC-02 [Validation]:** Chọn phòng ban đích trùng phòng ban hiện tại -> 400. Lý do 9 ký tự -> 400.
- **AC-03 [Security]:** Ngay sau khi chuyển, nhân viên phòng ban cũ mở hồ sơ -> 404.
- **AC-04 [Concurrency]:** Hai thao tác dùng cùng version cạnh tranh: một cập nhật được commit. Nếu chuyển phòng đã commit, staff phòng cũ gọi tiếp nhận 404 do mất phạm vi; nếu vẫn trong phạm vi nhưng version cũ thì 409. Không bảo đảm thứ tự theo thời điểm bấm Gửi.
- **AC-05 [Boundary]:** Hồ sơ `PENDING_INFO` có yêu cầu bổ sung đang mở được chuyển đi -> yêu cầu vẫn mở; khi sinh viên bổ sung, nội dung đến phòng ban mới.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, hồ sơ vẫn ở phòng ban cũ cùng người phụ trách cũ. Thành công -> có audit TRANSFER (phòng cũ/mới, transfer_event_id); lý do lưu trong lịch sử nghiệp vụ nội bộ, không nhúng vào audit.

**Ví dụ Edge Case**
Hồ sơ bị chuyển qua lại giữa hai phòng ban nhiều lần. Mỗi lần chuyển đều được ghi vào lịch sử, kèm lý do. SLA vẫn tính liên tục từ lúc sinh viên gửi, nên việc chuyển qua lại không làm hồ sơ "trẻ lại" trên báo cáo.

**Expected Result:** Hồ sơ chỉ thuộc một phòng ban tại một thời điểm, quyền truy cập đổi ngay theo phòng ban, và SLA không bị đặt lại.

---

### [FR-STF-10] Tăng mức ưu tiên (escalate)

**Mô tả**
Nhân viên tăng mức ưu tiên của hồ sơ kèm lý do khi phát hiện vấn đề gấp hơn dự kiến, để phòng ban xử lý sớm hơn.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên và không ở `CLOSED`.
- Mức ưu tiên hiện tại chưa phải `HIGH`.

**Luồng chính**
1. Nhân viên bấm Escalate.
2. Nhân viên nhập lý do.
3. Nhân viên xác nhận.
4. Hệ thống tăng mức ưu tiên lên một bậc và đánh dấu đây là một lần escalate trong lịch sử.

**Input**
- **Người dùng/request:** ticket_id, version, reason.
- **Hệ thống:** Staff cùng phòng, priority chưa HIGH, ticket chưa CLOSED.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: priority tăng đúng một bậc, status và version mới.
- **Thay đổi/sự kiện:** History nội bộ ESCALATE có reason; audit tham chiếu; không đổi SLA/status và không báo sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Mỗi lần escalate tăng đúng một bậc: `LOW` → `MEDIUM` → `HIGH`.
- `reason`: bắt buộc, dài 10–500 ký tự. Chỉ nội bộ đọc được.
- Khác với FR-STF-08: escalate chỉ tăng, bắt buộc có lý do và được ghi riêng là `ESCALATE`.
- Không làm thay đổi SLA.
- Bắt buộc gửi `version`.

**Alternative / Error Flows**
- Nếu hồ sơ đã ở `HIGH`, hệ thống trả 409 "Hồ sơ đã ở mức ưu tiên cao nhất".
- Nếu thiếu lý do hoặc lý do sai độ dài, hệ thống trả 400.
- Nếu hồ sơ đã `CLOSED`, hoặc lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `MEDIUM`, escalate có lý do hợp lệ -> 200; mức ưu tiên thành `HIGH`; lịch sử ghi một lần escalate.
- **AC-02 [Validation]:** Escalate không có lý do -> 400.
- **AC-03 [Boundary]:** Hồ sơ `LOW` escalate 2 lần -> `HIGH`. Escalate lần thứ 3 -> 409.
- **AC-04 [Security]:** Nhân viên phòng ban khác -> 404.
- **AC-05 [Concurrency]:** Hai nhân viên cùng escalate một hồ sơ `MEDIUM` cùng lúc -> chỉ tăng một bậc lên `HIGH`, người sau nhận 409.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có audit ESCALATE (mức cũ/mới, escalation_event_id); lý do lưu lịch sử nghiệp vụ nội bộ.

**Ví dụ Edge Case**
Sinh viên bổ sung thông tin cho thấy hồ sơ liên quan đến hạn chót đăng ký thi vào ngày mai. Nhân viên escalate từ Trung bình lên Cao với lý do "Hạn đăng ký thi ngày mai". Hồ sơ lên đầu hàng đợi của phòng ban.

**Expected Result:** Mỗi lần escalate hợp lệ tăng đúng một bậc, có lý do, và được ghi riêng trong lịch sử.
