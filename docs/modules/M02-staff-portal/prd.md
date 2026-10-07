# M2. Cổng Nhân viên – Functional Requirement Specification

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Xác thực:** Mọi API yêu cầu JWT hợp lệ và vai trò `STAFF`. Sai hoặc thiếu token trả 401. Sai vai trò trả 403.
- **Phòng ban của nhân viên:** Lấy từ cấu hình phân quyền (MGT-06), không nhận từ client. Tài khoản `STAFF` chưa được gán phòng ban trả 403 "Tài khoản chưa được gán phòng ban".
- **Phạm vi truy cập:**
  - Hồ sơ không thuộc phòng ban của nhân viên trả 404.
  - Thao tác **xử lý** (STF-04, STF-05, STF-06) chỉ dành cho nhân viên phụ trách. Nhân viên khác trong phòng ban gọi thì trả 403.
  - Thao tác **điều phối** (STF-07 đến STF-10) thì mọi nhân viên trong phòng ban đều làm được.
- **Trạng thái hồ sơ:** `NEW`, `IN_PROGRESS`, `PENDING_INFO`, `OVERDUE`, `CLOSED`, theo CORE-02. Hồ sơ `CLOSED` chỉ được xem, không được thao tác gì thêm.
- **Cập nhật đồng thời:** Mỗi hồ sơ có trường `version`. Mọi thao tác ghi phải gửi kèm `version` hiện tại. Lệch `version` trả 409 "Hồ sơ đã được người khác cập nhật, vui lòng tải lại".
- **Vượt rate limit:** Trả 429 kèm `Retry-After`.
- **Nhật ký hệ thống:** Theo SEC-06. Mọi thao tác ghi trên hồ sơ và tệp đều sinh bản ghi nhật ký và một mốc trong lịch sử hồ sơ (CORE-06).

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

**Business Rules**
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

**Business Rules**
- Thông tin sinh viên hiển thị là bản lưu (snapshot) tại thời điểm tạo hồ sơ: họ tên, MSSV, khoa/lớp, email, SĐT. Trường trống hiển thị `--`.
- Nội dung hiển thị: mã hồ sơ, loại yêu cầu, mô tả, trạng thái, mức ưu tiên, người phụ trách, ngày gửi, hạn xử lý, các yêu cầu bổ sung và nội dung sinh viên đã bổ sung, nhật ký xử lý nội bộ, lịch sử hồ sơ.
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

**Business Rules**
- Tệp chỉ được trả qua API có kiểm tra quyền, không có đường dẫn công khai (SEC-05).
- Tệp trả về với tên gốc và đúng `Content-Type`.
- Mỗi lần xem hoặc tải đều ghi nhật ký `DOWNLOAD_FILE` (người thực hiện, thời gian, tệp, hồ sơ).
- Rate limit: 30 lần tải mỗi phút cho mỗi nhân viên.

**Alternative / Error Flows**
- Nếu tệp không thuộc hồ sơ của phòng ban, hệ thống trả 404.
- Nếu tệp bị mất trên kho lưu trữ, hệ thống trả 404 "Không tìm thấy tệp" và ghi log lỗi hệ thống.
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

**Business Rules**
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
Nhân viên thấy hồ sơ thuộc phòng ban khác. Nhân viên ghi chú "Hồ sơ thuộc Phòng Đào tạo" và dùng chức năng chuyển phòng ban (FR-STF-09). Ghi chú vẫn được lưu lại trong hồ sơ để phòng ban mới đọc được lý do.

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

**Business Rules**
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
- **AC-05 [Concurrency/Idempotency]:** Bấm Gửi 2 lần cùng `Idempotency-Key` -> 1 yêu cầu, 1 thông báo. Gửi với `version` cũ -> 409.
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
- **AC-02 [Boundary]:** Hồ sơ `PENDING_INFO` có yêu cầu bổ sung đang mở -> đóng thành công, yêu cầu bổ sung chuyển sang đã đóng.
- **AC-03 [Validation]:** Kết quả xử lý 9 ký tự -> 400. Đóng hồ sơ đã `CLOSED` -> 409.
- **AC-04 [Security]:** Nhân viên không phụ trách đóng hồ sơ -> 403.
- **AC-05 [Concurrency]:** Nhân viên đóng hồ sơ đúng lúc sinh viên gửi bổ sung -> chỉ một thao tác thành công, thao tác còn lại nhận 409.
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

**Business Rules**
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
- **AC-02 [Validation]:** Chọn nhân viên của phòng ban khác -> 422.
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
- **AC-04 [Concurrency]:** Một nhân viên chuyển phòng ban trong lúc người khác đang phân công -> chỉ thao tác gửi trước thành công, thao tác sau nhận 409.
- **AC-05 [Boundary]:** Hồ sơ `PENDING_INFO` có yêu cầu bổ sung đang mở được chuyển đi -> yêu cầu vẫn mở; khi sinh viên bổ sung, nội dung đến phòng ban mới.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, hồ sơ vẫn ở phòng ban cũ cùng người phụ trách cũ. Thành công -> có bản ghi nhật ký `TRANSFER` (phòng ban cũ, phòng ban mới, lý do).

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
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `ESCALATE` (mức cũ, mức mới, lý do).

**Ví dụ Edge Case**
Sinh viên bổ sung thông tin cho thấy hồ sơ liên quan đến hạn chót đăng ký thi vào ngày mai. Nhân viên escalate từ Trung bình lên Cao với lý do "Hạn đăng ký thi ngày mai". Hồ sơ lên đầu hàng đợi của phòng ban.

**Expected Result:** Mỗi lần escalate hợp lệ tăng đúng một bậc, có lý do, và được ghi riêng trong lịch sử.
