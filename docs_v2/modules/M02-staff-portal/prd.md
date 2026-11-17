# M02. Cổng Nhân viên – Functional Requirement Specification

> Đặc tả thuộc bộ 42 chức năng của 5 module, chốt cùng khách ở `MS-01` (`A-13`); sau khi chốt, thay đổi đi qua change request ([03-quan-tri-giam-sat.md](../../management/03-quan-tri-giam-sat.md) mục 5). Lịch sử sửa xem trong các PR. Điểm còn chờ trường xác nhận có một nơi gốc là [assumptions.md](../../project/assumptions.md) (mã `A-xx`, cột Trạng thái); phần đầu module chỉ dẫn lại mã.

**Mục đích module:** Hàng đợi và xử lý/điều phối hồ sơ trong phòng ban hiện tại.

**Actor chính:** STAFF. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M04 xác thực/phạm vi; M05 trạng thái, SLA và lịch sử; M01 phản hồi sinh viên.

## Luồng xử lý và liên kết module

M01 tạo ticket NEW vào phòng → staff mở hàng đợi/chi tiết → phân công người nhận active cùng phòng → IN_PROGRESS → người phụ trách xem tệp/ghi note/xử lý → yêu cầu bổ sung nếu thiếu → student trả lời → xử lý tiếp → nhập resolution và đóng CLOSED. Sau đóng, student đánh giá và M03 tính báo cáo.

Nhánh chuyển phòng: mọi staff active cùng phòng chọn đích/reason → xóa assignee, giữ loại/status/SLA/info request → phòng cũ mất quyền, phòng mới phân công. Nhánh quá hạn do M05 chuyển OVERDUE; ticket vẫn xử lý/bổ sung được và giữ OVERDUE đến đóng.

**Input module:** phiên STAFF/phòng hiện tại, ticket/version, content/note/resolution, assignee, priority, đích chuyển và reason.
**Output module:** hàng đợi/chi tiết trong phạm vi, tệp, hồ sơ cập nhật, lịch sử nội bộ/công khai và sự kiện audit/thông báo theo quyền.

**Quyền:** mọi staff cùng phòng được điều phối; chỉ assignee xử lý. Không thêm bước admin duyệt chuyển hoặc chỉ cho tự claim. Nếu chưa phụ trách thì có thể chuyển với reason mà không cần ghi note trước.

**Điểm chờ xác nhận:** danh sách nhân viên và phòng ban thực tế (`A-10`); SLA tính theo giờ lịch và tạm dừng khi chờ bổ sung (`A-08`). Quy tắc OVERDUE không tạm dừng và chặn tạm dừng muộn trước tác vụ quá hạn ghi ở FR-CORE-03, chốt cùng bộ đặc tả ở `MS-01` (`A-13`). Trạng thái theo dõi ở [assumptions.md](../../project/assumptions.md).

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

**Priority**
`Must`: Hàng đợi chung của phòng giải quyết vấn đề thất lạc hồ sơ (G-01).

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: items, total_items, total_pages, page, page_size, updated_at; item có code, loại, trạng thái, ưu tiên, người nhận, ngày gửi, due_at, sla_paused, remaining_seconds, needs_info.
- **Thay đổi/sự kiện:** Chỉ đọc và polling 30 giây; hồ sơ vừa chuyển đi không được trả nữa.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** remaining_seconds được tính theo CORE-03; PENDING_INFO hiện Chờ sinh viên/SLA tạm dừng, không hiển thị số âm do dùng due_at cũ. Mọi tham số phân trang phải là số nguyên dương. Cùng ưu tiên/ngày gửi thì ticket_id tăng dần.
- **BR-02:** Chỉ trả về hồ sơ có `department_id` trùng với phòng ban của nhân viên.
- **BR-03:** Bộ lọc:
  - Trạng thái (chọn nhiều).
  - Mức ưu tiên.
  - Người phụ trách: "Của tôi", "Chưa phân công", "Tất cả".
- **BR-04:** Sắp xếp mặc định: ưu tiên Cao → Thấp, sau đó hồ sơ gửi sớm hơn đứng trước.
- **BR-05:** Mỗi dòng hiển thị: mã hồ sơ, loại yêu cầu, trạng thái, mức ưu tiên, người phụ trách (hoặc "Chưa phân công"), ngày gửi, hạn xử lý theo SLA, dấu hiệu "Chờ sinh viên" nếu có yêu cầu bổ sung đang mở.
- **BR-06:** 20 hồ sơ mỗi trang; `page_size` tối đa 100.
- **BR-07:** Hàng đợi tự cập nhật bằng polling mỗi 30 giây.
- **BR-08:** Rate limit: 120 request mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- **E-01:** Nếu tham số lọc hoặc phân trang không hợp lệ, hệ thống trả 400.
- **E-02:** Nếu nhân viên chưa được gán phòng ban, hệ thống trả 403.
- **E-03:** Nếu truy vấn quá 5 giây, hệ thống trả 503 và UI hiển thị nút Thử lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Phòng ban có 45 hồ sơ -> trang 1 có 20 hồ sơ, sắp xếp đúng theo mức ưu tiên rồi đến ngày gửi. _(Kiểm cho: luồng chính, BR-04, BR-06)_
- **AC-02 [Validation]:** Lọc "Chưa phân công" -> chỉ trả về hồ sơ chưa có người phụ trách. Lọc trạng thái `ABC` -> 400. _(Kiểm cho: BR-03, E-01)_
- **AC-03 [Boundary]:** `page_size = 100` -> 200. `page_size = 101` -> 400. _(Kiểm cho: BR-06, E-01)_
- **AC-04 [Security]:** Truyền `department_id` của phòng ban khác trên query -> hệ thống bỏ qua, chỉ trả hồ sơ của phòng ban nhân viên. _(Kiểm cho: BR-02)_
- **AC-05 [Concurrency/Rate Limit]:** Hồ sơ mới gửi vào phòng ban trong lúc hàng đợi đang mở -> xuất hiện trong vòng 30 giây. Request thứ 121 trong 1 phút -> 429. _(Kiểm cho: BR-07, BR-08)_
- **AC-06 [Resilience]:** Cơ sở dữ liệu phản hồi quá 5 giây -> 503 với thông báo "Vui lòng thử lại", trang không bị trắng. _(Kiểm cho: E-03)_
- **AC-07 [Validation]:** Lọc trạng thái `NEW` và `OVERDUE` cùng mức ưu tiên `HIGH` -> chỉ trả hồ sơ thỏa cả hai điều kiện. Lọc "Của tôi" -> chỉ trả hồ sơ do nhân viên đang đăng nhập phụ trách. `page = 0` -> 400. _(Kiểm cho: BR-01, BR-03, E-01)_
- **AC-08 [Business Logic]:** Hai hồ sơ cùng mức ưu tiên và cùng ngày gửi -> hồ sơ có `ticket_id` nhỏ hơn đứng trước. Mỗi dòng có mã, loại yêu cầu, trạng thái, mức ưu tiên, người phụ trách hoặc "Chưa phân công", ngày gửi, hạn xử lý. Hồ sơ `PENDING_INFO` hiển thị "Chờ sinh viên" và SLA tạm dừng, không hiển thị số giờ còn lại âm. _(Kiểm cho: BR-01, BR-05)_
- **AC-09 [Security]:** Tài khoản `STAFF` chưa được gán phòng ban mở hàng đợi -> 403 "Tài khoản chưa được gán phòng ban". _(Kiểm cho: E-02)_

**Ví dụ Edge Case**
Một hồ sơ vừa được chuyển sang phòng ban khác trong lúc nhân viên đang xem hàng đợi. Ở lần polling tiếp theo, hồ sơ biến mất khỏi hàng đợi của phòng ban cũ. Nếu nhân viên bấm vào dòng cũ trước khi danh sách kịp cập nhật, hệ thống trả 404 "Hồ sơ không còn thuộc phòng ban của bạn".

**Expected Result:** Nhân viên luôn thấy đủ và đúng các hồ sơ của phòng ban mình, không thấy hồ sơ của phòng ban khác.

---

### [FR-STF-02] Xem thông tin sinh viên và nội dung yêu cầu

**Mô tả**
Nhân viên xem đầy đủ thông tin một hồ sơ để xử lý, không cần hỏi lại sinh viên những thông tin đã có.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Must`: Nhân viên phải đọc nội dung yêu cầu mới xử lý được.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên.

**Luồng chính**
1. Nhân viên chọn hồ sơ từ hàng đợi hoặc nhập mã hồ sơ.
2. Hệ thống trả về thông tin sinh viên, nội dung yêu cầu, danh sách tệp, nhật ký xử lý và lịch sử hồ sơ.
3. UI hiển thị các nút thao tác theo quyền: nút xử lý chỉ hiện với nhân viên phụ trách, nút điều phối hiện với mọi nhân viên trong phòng ban.

**Input**
- **Người dùng/request:** ticket_id hoặc code.
- **Hệ thống:** Phiên staff, phòng hiện tại; hồ sơ/snapshot/history/notes/files/info requests.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: đầy đủ dữ liệu hồ sơ trong phạm vi, version, allowed_actions; CLOSED có resolution và closed_at. allowed_actions chỉ để hỗ trợ UI.
- **Thay đổi/sự kiện:** Chỉ đọc; mọi thao tác sau phải kiểm tra lại quyền, không tin allowed_actions cũ.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Thông tin sinh viên hiển thị là bản lưu (snapshot) tại thời điểm tạo hồ sơ: họ tên, MSSV, khoa/lớp, email, SĐT. Trường trống hiển thị `--`.
- **BR-02:** Nội dung hiển thị: mã hồ sơ, loại yêu cầu, mô tả, trạng thái, mức ưu tiên, người phụ trách, ngày gửi, hạn xử lý, các yêu cầu bổ sung và nội dung sinh viên đã bổ sung, nhật ký xử lý nội bộ, lịch sử hồ sơ; hồ sơ CLOSED có kết quả resolution và closed_at.
- **BR-03:** Response trả về trường `version` để dùng cho các thao tác ghi.
- **BR-04:** Rate limit: 120 request mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- **E-01:** Nếu mã hồ sơ sai định dạng, hệ thống trả 400.
- **E-02:** Nếu hồ sơ không tồn tại hoặc không thuộc phòng ban, hệ thống trả 404.
- **E-03:** Nếu không tải được lịch sử, hệ thống vẫn hiển thị thông tin chính và báo "Chưa tải được lịch sử".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Mở hồ sơ hợp lệ của phòng ban -> 200, hiển thị đủ thông tin sinh viên, nội dung, tệp, nhật ký và lịch sử. _(Kiểm cho: luồng chính, BR-01, BR-02)_
- **AC-02 [Validation]:** Snapshot không có SĐT -> UI hiển thị `--`, không lỗi. _(Kiểm cho: BR-01)_
- **AC-03 [Security]:** Mở hồ sơ của phòng ban khác -> 404. Nhân viên không phụ trách mở hồ sơ -> 200, nhưng không thấy các nút xử lý. _(Kiểm cho: E-02)_
- **AC-04 [Concurrency]:** Hai nhân viên cùng mở một hồ sơ -> cả hai nhận cùng giá trị `version`. Sau khi một người cập nhật, người kia thấy `version` mới khi tải lại. _(Kiểm cho: BR-03)_
- **AC-05 [Boundary]:** Hồ sơ `CLOSED` -> hiển thị đầy đủ ở chế độ chỉ đọc, không có nút thao tác. _(Kiểm cho: BR-02)_
- **AC-06 [Resilience]:** API lịch sử lỗi -> thông tin chính vẫn hiển thị, không trắng trang. _(Kiểm cho: E-03)_
- **AC-07 [Validation/Rate Limit]:** Mở hồ sơ với mã sai định dạng (`USP-26-1`) -> 400. Request thứ 121 trong 1 phút -> 429 kèm `Retry-After`. _(Kiểm cho: BR-04, E-01)_

**Ví dụ Edge Case**
Sinh viên đổi số điện thoại trên SSO sau khi đã gửi hồ sơ. Nhân viên vẫn thấy số cũ trong snapshot, vì đó là thông tin tại thời điểm gửi. Hồ sơ không bị cập nhật ngược.

**Expected Result:** Nhân viên của đúng phòng ban thấy đầy đủ thông tin cần để xử lý, và chỉ thấy những thao tác mình được phép làm.

---

### [FR-STF-03] Xem và tải tệp đính kèm

**Mô tả**
Nhân viên xem trực tiếp hoặc tải tệp minh chứng của hồ sơ để kiểm tra khi xử lý.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Must`: Minh chứng đính kèm là đầu vào để xử lý.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: nội dung tệp và header MIME/tên/cache; 404 ngoài phạm vi, 503 storage/audit lỗi.
- **Thay đổi/sự kiện:** Audit khởi tạo tải và kết quả theo SEC-05; không tăng version, không tạo mốc timeline khi đọc tệp.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Tệp chỉ được trả qua API có kiểm tra quyền, không có đường dẫn công khai (SEC-05).
- **BR-02:** Tệp trả về với tên gốc và đúng `Content-Type`.
- **BR-03:** Mỗi lần xem hoặc tải đều ghi nhật ký `DOWNLOAD_FILE` (người thực hiện, thời gian, tệp, hồ sơ).
- **BR-04:** Rate limit: 30 lần tải mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- **E-01:** Nếu tệp không thuộc hồ sơ của phòng ban, hệ thống trả 404.
- **E-02:** Nếu metadata có quyền nhưng object mất, trả 404 chung; audit khởi tạo và FILE_READ_RESULT=STORAGE_FAILED/OBJECT_MISSING theo SEC-05. Log kỹ thuật không có đường dẫn/token.
- **E-03:** Nếu kho lưu trữ không phản hồi, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Mở một tệp PDF -> hiển thị trên trình duyệt với đúng tên gốc. _(Kiểm cho: luồng chính, BR-02)_
- **AC-02 [Validation]:** Gọi API với `file_id` không tồn tại -> 404. _(Kiểm cho: E-01)_
- **AC-03 [Security]:** Nhân viên phòng ban khác gọi trực tiếp API tải tệp -> 404. Gọi API không có token -> 401. _(Kiểm cho: BR-01, E-01)_
- **AC-04 [Rate Limit]:** Lần tải thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-04)_
- **AC-05 [Boundary]:** Tải tệp 10MB -> tải trọn vẹn, đúng dung lượng. _(Kiểm cho: luồng chính)_
- **AC-06 [Resilience & Audit]:** Kho lưu trữ không phản hồi -> 503, ứng dụng không treo. Mỗi lần tải thành công -> có một bản ghi `DOWNLOAD_FILE`. _(Kiểm cho: BR-03, E-03)_
- **AC-07 [Validation]:** Tải tệp PNG và tệp PDF -> header `Content-Type` lần lượt là `image/png` và `application/pdf`; tên tệp tải về là tên gốc. _(Kiểm cho: BR-02)_
- **AC-08 [Resilience & Audit]:** Tệp có metadata trong hồ sơ nhưng object trong kho đã mất -> 404 với thông báo chung; nhật ký có bản ghi khởi tạo tải và `FILE_READ_RESULT` = `OBJECT_MISSING`; log kỹ thuật không chứa đường dẫn kho hay token. _(Kiểm cho: E-02)_

**Ví dụ Edge Case**
Hồ sơ vừa được chuyển sang phòng ban khác trong lúc nhân viên phòng cũ còn mở trang chi tiết và bấm tải tệp. Hệ thống kiểm tra quyền tại thời điểm tải, nên trả 404 và không ghi nhật ký tải.

**Expected Result:** Chỉ nhân viên đang có quyền với hồ sơ mới mở được tệp, và mọi lần mở đều được ghi lại.

---

### [FR-STF-04] Ghi nhật ký xử lý

**Mô tả**
Nhân viên phụ trách ghi lại các bước đã làm với hồ sơ, giúp người khác nắm được lịch sử xử lý khi tiếp nhận hoặc kiểm tra lại.

**Actor**
Nhân viên phụ trách hồ sơ.

**Priority**
`Should`: Ghi nhật ký giúp phối hợp trong phòng; hồ sơ vẫn xử lý được nếu làm muộn hơn.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: note_id, content, actor, created_at, ticket_id, version mới cho staff có quyền.
- **Thay đổi/sự kiện:** Tạo note và timeline nội bộ; trạng thái không đổi; audit chỉ tham chiếu note.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Gửi version và tăng version một lần theo quy ước chung. Staff không phụ trách vẫn được chuyển phòng bằng STF-09 với reason bắt buộc; không cần tạo note trước nếu không có quyền ghi note.
- **BR-02:** `content`: bắt buộc, dài 1–2000 ký tự sau khi trim.
- **BR-03:** Ghi chú chỉ hiển thị cho nhân viên phòng ban đang giữ hồ sơ, không hiển thị cho sinh viên.
- **BR-04:** Ghi chú không được sửa hoặc xóa sau khi lưu.
- **BR-05:** Ghi chú lưu dạng văn bản thuần và escape khi hiển thị.
- **BR-06:** Dùng để ghi lại cả trường hợp hồ sơ bị gửi sai phòng ban.
- **BR-07:** Ghi chú không làm đổi trạng thái hồ sơ.
- **BR-08:** Bắt buộc có `Idempotency-Key`. Rate limit: 30 ghi chú mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- **E-01:** Nếu nội dung rỗng hoặc vượt 2000 ký tự, hệ thống trả 400.
- **E-02:** Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- **E-03:** Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Người phụ trách lưu ghi chú 100 ký tự -> 201; ghi chú hiển thị kèm tên người ghi và thời gian; trạng thái hồ sơ không đổi. _(Kiểm cho: luồng chính, BR-07)_
- **AC-02 [Validation/Boundary]:** Nội dung toàn khoảng trắng -> 400. Đúng 2000 ký tự -> 201. 2001 ký tự -> 400. _(Kiểm cho: BR-02, E-01)_
- **AC-03 [Security]:** Nhân viên cùng phòng ban nhưng không phụ trách -> 403. Sinh viên xem hồ sơ -> không thấy ghi chú nội bộ. _(Kiểm cho: BR-03, E-02)_
- **AC-04 [Idempotency]:** Bấm Lưu 2 lần cùng `Idempotency-Key` -> chỉ có 1 ghi chú. _(Kiểm cho: BR-08)_
- **AC-05 [Business Logic]:** Ghi chú vào hồ sơ `CLOSED` -> 409. _(Kiểm cho: E-03)_
- **AC-06 [Resilience & Audit]:** Lưu ghi chú lỗi -> 500, UI giữ nội dung đã nhập để lưu lại. Lưu thành công -> có bản ghi nhật ký `ADD_NOTE`. _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Security]:** Ghi chú chứa `<script>` -> lưu dạng văn bản, khi hiển thị không thực thi. Ghi chú đã lưu không có nút hoặc API sửa, xóa. _(Kiểm cho: BR-04, BR-05)_
- **AC-08 [Validation/Rate Limit]:** Thiếu header `Idempotency-Key` -> 400. Ghi chú thứ 31 trong 1 phút -> 429. Lưu ghi chú thành công -> `version` của hồ sơ tăng đúng 1. _(Kiểm cho: BR-01, BR-08)_
- **AC-09 [Business Logic]:** Người phụ trách ghi chú "Hồ sơ gửi nhầm phòng" rồi chuyển hồ sơ sang phòng B (FR-STF-09) -> nhân viên phòng B thấy ghi chú này trong nhật ký nội bộ. Nhân viên không phụ trách vẫn chuyển được hồ sơ khi nhập lý do, không cần ghi chú trước. _(Kiểm cho: BR-01, BR-03, BR-06)_

**Ví dụ Edge Case**
Nhân viên phụ trách nhận thấy nội dung hồ sơ không thuộc trách nhiệm phòng mình. Người này ghi chú "Hồ sơ thuộc Phòng Đào tạo" và dùng chức năng chuyển phòng ban (FR-STF-09). Ghi chú vẫn được lưu lại trong hồ sơ để phòng ban mới đọc được lý do.

**Expected Result:** Mỗi ghi chú hợp lệ được lưu đúng một lần, không sửa được, và chỉ nội bộ đọc được.

---

### [FR-STF-05] Yêu cầu sinh viên bổ sung thông tin

**Mô tả**
Nhân viên phụ trách gửi yêu cầu sinh viên bổ sung thông tin khi hồ sơ chưa đủ dữ liệu để xử lý.

**Actor**
Nhân viên phụ trách hồ sơ.

**Priority**
`Must`: Không có yêu cầu bổ sung thì hồ sơ thiếu thông tin không xử lý tiếp được.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: info_request_id, content, created_at, status, version mới, sla_paused và due_at theo quyền staff.
- **Thay đổi/sự kiện:** Tạo yêu cầu mở; pause nếu IN_PROGRESS chuyển PENDING_INFO; một thông báo cho sinh viên; history/audit.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Nếu status còn IN_PROGRESS nhưng now > due_at trước lần cron kế tiếp, không cho chuyển PENDING_INFO để tạm dừng muộn: trả 409 kèm yêu cầu tải lại; job quá hạn sẽ đổi OVERDUE. Sau đó người phụ trách gửi lại yêu cầu, trạng thái giữ OVERDUE. Retry thành công cũ theo quy ước idempotency.
- **BR-02:** `content`: bắt buộc, dài 10–1000 ký tự sau khi trim. Nội dung này sinh viên đọc được.
- **BR-03:** Mỗi hồ sơ chỉ có tối đa một yêu cầu bổ sung đang mở.
- **BR-04:** Yêu cầu bổ sung đóng lại khi sinh viên trả lời (STU-09) hoặc khi hồ sơ được đóng (FR-STF-06).
- **BR-05:** Bắt buộc gửi `version` và `Idempotency-Key`.

**Alternative / Error Flows**
- **E-01:** Nếu nội dung sai độ dài, hệ thống trả 400.
- **E-02:** Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- **E-03:** Nếu hồ sơ đã có yêu cầu bổ sung đang mở hoặc ở `NEW` hay `CLOSED`, hệ thống trả 409.
- **E-04:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `IN_PROGRESS`, gửi yêu cầu hợp lệ -> 201; hồ sơ chuyển `PENDING_INFO`; SLA tạm dừng; sinh viên nhận thông báo. _(Kiểm cho: luồng chính)_
- **AC-02 [Boundary]:** Hồ sơ `OVERDUE`, gửi yêu cầu -> 201; trạng thái vẫn `OVERDUE`; sinh viên vẫn nhận thông báo. _(Kiểm cho: BR-01)_
- **AC-03 [Validation]:** Nội dung 9 ký tự -> 400. Hồ sơ đang `PENDING_INFO` -> 409 "Đã có yêu cầu bổ sung đang mở". _(Kiểm cho: BR-02, BR-03, E-01, E-03)_
- **AC-04 [Security]:** Nhân viên không phụ trách gửi yêu cầu -> 403. _(Kiểm cho: E-02)_
- **AC-05 [Concurrency/Idempotency]:** Bấm Gửi 2 lần cùng `Idempotency-Key` -> 1 yêu cầu, 1 thông báo. Gửi với version cũ khác key -> 409. Retry cùng key đã thành công trả kết quả cũ dù version đổi; cùng key payload khác 409. now > due_at khi còn IN_PROGRESS -> 409, không tạo pause muộn. _(Kiểm cho: BR-01, BR-05, E-04)_
- **AC-06 [Resilience & Audit]:** Tạo thông báo lỗi -> rollback cả yêu cầu bổ sung và việc đổi trạng thái. Thành công -> có bản ghi nhật ký `REQUEST_INFO` và mốc trong lịch sử hồ sơ. _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Validation]:** Hồ sơ `NEW` hoặc `CLOSED` -> 409. Thiếu `version` hoặc `Idempotency-Key` -> 400. Nội dung đúng 1000 ký tự -> 201; 1001 ký tự -> 400. _(Kiểm cho: BR-02, BR-05, E-01, E-03)_
- **AC-08 [Business Logic]:** Sinh viên trả lời yêu cầu bổ sung (FR-STU-09) -> yêu cầu đóng, người phụ trách tạo được yêu cầu mới. Hồ sơ được đóng (FR-STF-06) khi còn yêu cầu mở -> yêu cầu đóng theo. _(Kiểm cho: BR-03, BR-04)_

**Ví dụ Edge Case**
Hồ sơ `IN_PROGRESS` còn 2 giờ là đến hạn SLA thì nhân viên gửi yêu cầu bổ sung. SLA dừng lại, nên hồ sơ không chuyển `OVERDUE` trong thời gian chờ sinh viên. Khi sinh viên bổ sung xong, SLA chạy tiếp với 2 giờ còn lại.

**Expected Result:** Mỗi lần gửi hợp lệ tạo đúng một yêu cầu bổ sung đang mở, trạng thái và SLA thay đổi đúng theo CORE-02 và CORE-03.

---

### [FR-STF-06] Đóng hồ sơ

**Mô tả**
Nhân viên phụ trách đóng hồ sơ khi đã xử lý xong, thông báo kết quả cho sinh viên và kết thúc tính SLA.

**Actor**
Nhân viên phụ trách hồ sơ.

**Priority**
`Must`: Đóng hồ sơ kèm kết quả là đầu ra của luồng chính.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: ticket_id/code, status=CLOSED, resolution, closed_at, version mới.
- **Thay đổi/sự kiện:** Đóng ticket/yêu cầu bổ sung; kết thúc pause cuối nếu có; history/audit, một thông báo sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** `resolution`: bắt buộc, dài 10–2000 ký tự sau khi trim. Sinh viên đọc được.
- **BR-02:** Thời điểm đóng được dùng để tính thời gian xử lý trung bình (MGT-02).
- **BR-03:** Hồ sơ `CLOSED` không thể chuyển sang trạng thái khác (không có chức năng mở lại).
- **BR-04:** Bắt buộc gửi `version` và `Idempotency-Key`.

**Alternative / Error Flows**
- **E-01:** Nếu thiếu kết quả xử lý hoặc sai độ dài, hệ thống trả 400.
- **E-02:** Nếu nhân viên không phải người phụ trách, hệ thống trả 403.
- **E-03:** Nếu hồ sơ đã `CLOSED` hoặc đang `NEW`, hệ thống trả 409.
- **E-04:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `IN_PROGRESS`, nhập kết quả hợp lệ -> 200; hồ sơ chuyển `CLOSED`; sinh viên nhận thông báo và thấy form đánh giá. _(Kiểm cho: luồng chính)_
- **AC-02 [Boundary]:** PENDING_INFO có info request mở -> close hợp lệ, request đóng, cộng bù pause cuối đến closed_at, version tăng một lần; retry cùng key trả kết quả cũ, không thêm history/noti. _(Kiểm cho: BR-04)_
- **AC-03 [Validation]:** Kết quả xử lý 9 ký tự -> 400. Đóng hồ sơ đã `CLOSED` -> 409. _(Kiểm cho: BR-01, BR-03, E-01, E-03)_
- **AC-04 [Security]:** Nhân viên không phụ trách đóng hồ sơ -> 403. _(Kiểm cho: E-02)_
- **AC-05 [Concurrency]:** Nhân viên đóng và sinh viên bổ sung dùng cùng version đã đọc -> chỉ một thao tác commit; bên kia nhận 409. Nếu bên đóng tải version mới sau bổ sung rồi đóng thì hợp lệ. _(Kiểm cho: BR-04, E-04)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, trạng thái giữ nguyên. Thành công -> có bản ghi nhật ký `CLOSE_TICKET` và mốc trong lịch sử hồ sơ. _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Validation]:** Đóng hồ sơ đang `NEW` -> 409. Thiếu `resolution` -> 400; `resolution` đúng 2000 ký tự -> 200, 2001 ký tự -> 400. Thiếu `version` hoặc `Idempotency-Key` -> 400. _(Kiểm cho: BR-01, BR-04, E-01, E-03)_
- **AC-08 [Business Logic]:** Đóng hồ sơ lúc 10:00 -> `closed_at` = 10:00 theo giờ server; hồ sơ được tính vào thời gian xử lý trung bình (FR-MGT-02) theo mốc này. Sau khi đóng, mọi thao tác đổi trạng thái hồ sơ -> 409. _(Kiểm cho: BR-02, BR-03)_

**Ví dụ Edge Case**
Sinh viên không trả lời yêu cầu bổ sung sau một thời gian dài. Nhân viên đóng hồ sơ với kết quả "Không nhận được thông tin bổ sung". Yêu cầu bổ sung được đóng theo, và sinh viên không còn gửi bổ sung được cho hồ sơ này.

**Expected Result:** Hồ sơ đóng đúng một lần, sinh viên nhận được kết quả xử lý, và hồ sơ được tính vào báo cáo.

---

### [FR-STF-07] Phân công nhân viên phụ trách

**Mô tả**
Nhân viên trong phòng ban phân công người phụ trách cho hồ sơ, để mọi hồ sơ đều có người chịu trách nhiệm rõ ràng.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Must`: Rõ người phụ trách là mục tiêu G-01; chỉ người phụ trách được xử lý.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Danh sách 200 gồm id/tên người nhận active; cập nhật 200 có assignee, status và version mới hoặc version hiện tại nếu no-op.
- **Thay đổi/sự kiện:** Gán/đổi người nhận; NEW → IN_PROGRESS; history/audit ASSIGN; chỉ thông báo nếu đổi trạng thái.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Người nhận phải active; API danh sách chỉ trả STAFF active trong phòng, backend kiểm tra lại khi lưu. Kiểm tra phân công và đổi vai trò/phòng/vô hiệu hóa tài khoản phải bảo vệ cùng ràng buộc một cách nguyên tử; không để hồ sơ mở gán người mất quyền. Người nhận inactive trả 422.
- **BR-02:** Khi NEW đã vượt hạn nhưng cron chưa chạy, phân công vẫn hợp lệ và không reset hạn; job chuyển OVERDUE theo due_at. Phân công không che trạng thái quá hạn.
- **BR-03:** Phân công lại: được phép đổi người phụ trách khi hồ sơ ở `IN_PROGRESS`, `PENDING_INFO` hoặc `OVERDUE`. Trạng thái giữ nguyên.
- **BR-04:** Hồ sơ `OVERDUE` chưa từng được phân công khi được phân công thì vẫn giữ `OVERDUE` (CORE-02).
- **BR-05:** Phân công trùng người phụ trách hiện tại trả 200 và không ghi thêm lịch sử.
- **BR-06:** Bắt buộc gửi `version`.

**Alternative / Error Flows**
- **E-01:** Nếu người được chọn không thuộc phòng ban hoặc không phải `STAFF`, hệ thống trả 422.
- **E-02:** Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.
- **E-03:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `NEW`, phân công nhân viên A -> 200; hồ sơ chuyển `IN_PROGRESS`; sinh viên nhận thông báo đổi trạng thái. _(Kiểm cho: luồng chính)_
- **AC-02 [Validation]:** Chọn người phòng khác hoặc inactive -> 422. Phân công cạnh tranh deactivate/đổi phòng: không để ticket mở gán người mất quyền, một thao tác bị chặn nguyên tử. _(Kiểm cho: BR-01, E-01)_
- **AC-03 [Boundary]:** Hồ sơ `OVERDUE` chưa có người phụ trách, phân công A -> 200, trạng thái vẫn `OVERDUE`. _(Kiểm cho: BR-04)_
- **AC-04 [Security]:** Nhân viên phòng ban khác gọi API phân công -> 404. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Concurrency/Idempotency]:** Hai nhân viên phân công cùng lúc với cùng `version` -> người đầu thành công, người sau nhận 409. Phân công lại đúng người hiện tại -> 200, không có mốc lịch sử mới. _(Kiểm cho: BR-05, BR-06, E-03)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `ASSIGN` (người cũ, người mới). _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Business Logic]:** Hồ sơ `PENDING_INFO` đổi người phụ trách từ A sang B -> 200, trạng thái vẫn `PENDING_INFO`. Hồ sơ `NEW` đã qua `due_at` nhưng tác vụ quá hạn chưa chạy, phân công A -> 200, `due_at` không đổi; lần chạy kế tiếp của tác vụ chuyển hồ sơ sang `OVERDUE`. _(Kiểm cho: BR-02, BR-03)_
- **AC-08 [Validation]:** Chọn tài khoản `ADMIN` hoặc `STUDENT` làm người phụ trách -> 422. Thiếu `version` -> 400. Hồ sơ đã `CLOSED` -> 409. _(Kiểm cho: BR-06, E-01, E-02)_

**Ví dụ Edge Case**
Nhân viên A đang phụ trách nghỉ phép dài ngày. Nhân viên B trong phòng ban phân công lại hồ sơ cho C. Trạng thái giữ nguyên, C có quyền xử lý ngay, còn A mất quyền xử lý nhưng vẫn xem được hồ sơ vì cùng phòng ban.

**Expected Result:** Mỗi hồ sơ chưa đóng có tối đa một người phụ trách thuộc đúng phòng ban, và mọi lần đổi đều được ghi lại.

---

### [FR-STF-08] Đặt mức ưu tiên

**Mô tả**
Nhân viên trong phòng ban đặt mức ưu tiên cho hồ sơ để phòng ban xử lý việc gấp trước.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Should`: Giúp sắp xếp thứ tự xử lý; hàng đợi vẫn dùng được theo thời gian gửi.

**Preconditions**
- Hồ sơ thuộc phòng ban của nhân viên và không ở `CLOSED`.

**Luồng chính**
1. Nhân viên chọn mức ưu tiên mới.
2. Nhân viên xác nhận.
3. Hệ thống cập nhật mức ưu tiên.

**Input**
- **Người dùng/request:** ticket_id, version, priority=LOW/MEDIUM/HIGH.
- **Hệ thống:** Staff active cùng phòng, ticket chưa CLOSED.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: priority, status, version mới; no-op trả version hiện tại.
- **Thay đổi/sự kiện:** Đổi ưu tiên, không đổi SLA/status; history nội bộ và CHANGE_PRIORITY nếu dữ liệu đổi.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Giá trị hợp lệ: `LOW`, `MEDIUM`, `HIGH` (Thấp, Trung bình, Cao), theo CORE-04. Được tăng hoặc giảm.
- **BR-02:** Mức ưu tiên dùng để sắp xếp và lọc, không làm thay đổi SLA.
- **BR-03:** Đặt trùng mức hiện tại trả 200 và không ghi thêm lịch sử.
- **BR-04:** Bắt buộc gửi `version`.

**Alternative / Error Flows**
- **E-01:** Nếu giá trị ngoài 3 mức, hệ thống trả 400.
- **E-02:** Nếu hồ sơ đã `CLOSED`, hệ thống trả 409.
- **E-03:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Đổi Trung bình -> Thấp -> 200; hàng đợi sắp xếp lại theo mức mới. _(Kiểm cho: luồng chính, BR-01, BR-02)_
- **AC-02 [Validation]:** Gửi `URGENT` -> 400. _(Kiểm cho: BR-01, E-01)_
- **AC-03 [Boundary]:** Đặt trùng mức hiện tại -> 200, không có mốc lịch sử mới. _(Kiểm cho: BR-03)_
- **AC-04 [Security]:** Nhân viên phòng ban khác -> 404. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Concurrency]:** Hai nhân viên đổi mức ưu tiên cùng lúc -> người sau nhận 409. _(Kiểm cho: BR-04, E-03)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `CHANGE_PRIORITY` (mức cũ, mức mới). _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Business Logic]:** Đổi mức ưu tiên từ `LOW` lên `HIGH` -> `due_at` của hồ sơ không đổi. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400. _(Kiểm cho: BR-01, BR-02, BR-04, E-02)_

**Ví dụ Edge Case**
Hồ sơ đang `OVERDUE` bị hạ mức ưu tiên xuống Thấp. Hồ sơ vẫn giữ `OVERDUE`, vì mức ưu tiên không ảnh hưởng SLA, và vẫn được tính vào số hồ sơ quá hạn trên dashboard.

**Expected Result:** Mức ưu tiên đổi đúng, không ảnh hưởng SLA, và mọi lần đổi đều được ghi lại.

---

### [FR-STF-09] Chuyển hồ sơ sang phòng ban khác

**Mô tả**
Nhân viên chuyển hồ sơ sang đúng phòng ban khi hồ sơ không thuộc trách nhiệm của phòng mình, để sinh viên không phải gửi lại yêu cầu.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Must`: Hồ sơ gửi nhầm loại chỉ đi đúng phòng nhờ chuyển phòng; thiếu thì hồ sơ bị kẹt.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: code, department mới, assignee=null, status và version mới. Sau response này caller phòng cũ không có quyền tải lại chi tiết.
- **Thay đổi/sự kiện:** Chuyển phòng, xóa người nhận; giữ loại/status/hạn/info request; public history chỉ tên phòng mới, reason nội bộ; audit tham chiếu.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Phòng ban đích phải tồn tại và khác phòng ban hiện tại.
- **BR-02:** `reason`: bắt buộc, dài 10–500 ký tự. Chỉ nội bộ đọc được.
- **BR-03:** Sau khi chuyển:
  - Trạng thái hồ sơ giữ nguyên.
  - SLA không bị đặt lại.
  - Yêu cầu bổ sung đang mở (nếu có) vẫn giữ.
  - Loại yêu cầu không đổi.
- **BR-04:** Phòng ban cũ mất quyền truy cập hồ sơ ngay khi chuyển xong.
- **BR-05:** Sinh viên thấy phòng ban mới trong danh sách và chi tiết hồ sơ (STU-06, STU-07). Việc chuyển phòng ban không tạo thông báo vì không phải đổi trạng thái.
- **BR-06:** Bắt buộc gửi `version`.

**Alternative / Error Flows**
- **E-01:** Nếu phòng ban đích trùng phòng ban hiện tại, hệ thống trả 400.
- **E-02:** Nếu phòng ban đích không tồn tại, hệ thống trả 422.
- **E-03:** Nếu thiếu lý do hoặc lý do sai độ dài, hệ thống trả 400.
- **E-04:** Nếu hồ sơ đã `CLOSED`, hoặc lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Chuyển hồ sơ `IN_PROGRESS` sang phòng ban B -> 200; hồ sơ nằm ở hàng đợi B, mục "Chưa phân công"; trạng thái vẫn `IN_PROGRESS`. _(Kiểm cho: luồng chính, BR-03)_
- **AC-02 [Validation]:** Chọn phòng ban đích trùng phòng ban hiện tại -> 400. Lý do 9 ký tự -> 400. _(Kiểm cho: BR-01, BR-02, E-01, E-03)_
- **AC-03 [Security]:** Ngay sau khi chuyển, nhân viên phòng ban cũ mở hồ sơ -> 404. _(Kiểm cho: BR-04)_
- **AC-04 [Concurrency]:** Hai thao tác dùng cùng version cạnh tranh: một cập nhật được commit. Nếu chuyển phòng đã commit, staff phòng cũ gọi tiếp nhận 404 do mất phạm vi; nếu vẫn trong phạm vi nhưng version cũ thì 409. Không bảo đảm thứ tự theo thời điểm bấm Gửi. _(Kiểm cho: BR-06, E-04)_
- **AC-05 [Boundary]:** Hồ sơ `PENDING_INFO` có yêu cầu bổ sung đang mở được chuyển đi -> yêu cầu vẫn mở; khi sinh viên bổ sung, nội dung đến phòng ban mới. _(Kiểm cho: BR-03)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, hồ sơ vẫn ở phòng ban cũ cùng người phụ trách cũ. Thành công -> có audit TRANSFER (phòng cũ/mới, transfer_event_id); lý do lưu trong lịch sử nghiệp vụ nội bộ, không nhúng vào audit. _(Kiểm cho: BR-02)_
- **AC-07 [Validation]:** Phòng ban đích không tồn tại -> 422. Lý do đúng 500 ký tự -> 200; 501 ký tự -> 400. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400. _(Kiểm cho: BR-01, BR-02, BR-06, E-02, E-03, E-04)_
- **AC-08 [Business Logic]:** Sau khi chuyển: `due_at` và loại yêu cầu của hồ sơ giữ nguyên; sinh viên thấy phòng ban mới trong danh sách và chi tiết hồ sơ, không nhận thông báo nào và không thấy lý do chuyển. _(Kiểm cho: BR-02, BR-03, BR-05)_

**Ví dụ Edge Case**
Hồ sơ bị chuyển qua lại giữa hai phòng ban nhiều lần. Mỗi lần chuyển đều được ghi vào lịch sử, kèm lý do. SLA vẫn tính liên tục từ lúc sinh viên gửi, nên việc chuyển qua lại không làm hồ sơ "trẻ lại" trên báo cáo.

**Expected Result:** Hồ sơ chỉ thuộc một phòng ban tại một thời điểm, quyền truy cập đổi ngay theo phòng ban, và SLA không bị đặt lại.

---

### [FR-STF-10] Tăng mức ưu tiên (escalate)

**Mô tả**
Nhân viên tăng mức ưu tiên của hồ sơ kèm lý do khi phát hiện vấn đề gấp hơn dự kiến, để phòng ban xử lý sớm hơn.

**Actor**
Nhân viên thuộc phòng ban đang giữ hồ sơ.

**Priority**
`Could`: Đã có thể đổi mức ưu tiên bằng FR-STF-08; escalate thêm bước có lý do.

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
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: priority tăng đúng một bậc, status và version mới.
- **Thay đổi/sự kiện:** History nội bộ ESCALATE có reason; audit tham chiếu; không đổi SLA/status và không báo sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Mỗi lần escalate tăng đúng một bậc: `LOW` → `MEDIUM` → `HIGH`.
- **BR-02:** `reason`: bắt buộc, dài 10–500 ký tự. Chỉ nội bộ đọc được.
- **BR-03:** Khác với FR-STF-08: escalate chỉ tăng, bắt buộc có lý do và được ghi riêng là `ESCALATE`.
- **BR-04:** Không làm thay đổi SLA.
- **BR-05:** Bắt buộc gửi `version`.

**Alternative / Error Flows**
- **E-01:** Nếu hồ sơ đã ở `HIGH`, hệ thống trả 409 "Hồ sơ đã ở mức ưu tiên cao nhất".
- **E-02:** Nếu thiếu lý do hoặc lý do sai độ dài, hệ thống trả 400.
- **E-03:** Nếu hồ sơ đã `CLOSED`, hoặc lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `MEDIUM`, escalate có lý do hợp lệ -> 200; mức ưu tiên thành `HIGH`; lịch sử ghi một lần escalate. _(Kiểm cho: luồng chính, BR-03)_
- **AC-02 [Validation]:** Escalate không có lý do -> 400. _(Kiểm cho: BR-02, E-02)_
- **AC-03 [Boundary]:** Hồ sơ `LOW` escalate 2 lần -> `HIGH`. Escalate lần thứ 3 -> 409. _(Kiểm cho: BR-01, E-01)_
- **AC-04 [Security]:** Nhân viên phòng ban khác -> 404. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Concurrency]:** Hai nhân viên cùng escalate một hồ sơ `MEDIUM` cùng lúc -> chỉ tăng một bậc lên `HIGH`, người sau nhận 409. _(Kiểm cho: BR-05, E-03)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có audit ESCALATE (mức cũ/mới, escalation_event_id); lý do lưu lịch sử nghiệp vụ nội bộ. _(Kiểm cho: BR-03)_
- **AC-07 [Validation/Business Logic]:** Lý do 9 ký tự hoặc 501 ký tự -> 400. Escalate thành công -> `due_at` không đổi; sinh viên không thấy lý do escalate. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400. _(Kiểm cho: BR-02, BR-04, BR-05, E-02, E-03)_

**Ví dụ Edge Case**
Sinh viên bổ sung thông tin cho thấy hồ sơ liên quan đến hạn chót đăng ký thi vào ngày mai. Nhân viên escalate từ Trung bình lên Cao với lý do "Hạn đăng ký thi ngày mai". Hồ sơ lên đầu hàng đợi của phòng ban.

**Expected Result:** Mỗi lần escalate hợp lệ tăng đúng một bậc, có lý do, và được ghi riêng trong lịch sử.
