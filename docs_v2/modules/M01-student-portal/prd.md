# M1. Cổng Sinh viên – Functional Requirement Specification

> Cập nhật: 09/10/2026. Giữ nguyên 42 chức năng của 5 module trong repo; bổ sung và thống nhất đặc tả trực tiếp tại module. Các giả định chưa được trường xác nhận được ghi tại phần đầu module.

**Mục đích module:** Thông tin tài khoản trường, yêu cầu, tệp, phản hồi bổ sung và đánh giá.

**Actor chính:** STUDENT. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M4 xác thực/phạm vi; M5 danh mục, mã, trạng thái, SLA và tệp.

## Luồng sử dụng và liên kết module

Sinh viên đăng nhập qua M4 → xem thông tin → chọn loại yêu cầu để biết phòng nhận → nhập mô tả/tệp → gửi và nhận mã NEW → theo dõi tiến độ. Staff M2 phân công và xử lý; khi cần bổ sung, sinh viên nhận thông báo và trả lời. Staff đóng với kết quả; sinh viên xem và đánh giá một lần. M3 tổng hợp báo cáo; M5 bảo đảm mã/trạng thái/SLA/tệp/lịch sử thống nhất.

**Input module:** phiên STUDENT, loại yêu cầu, mô tả/tệp, nội dung bổ sung và đánh giá.
**Output module:** mã/hồ sơ thuộc sinh viên, trạng thái và lịch sử công khai, tệp có quyền, thông báo, kết quả xử lý và đánh giá đã lưu. Không trả dữ liệu xử lý nội bộ.

**Phạm vi:** chọn loại rồi hiển thị phòng nhận; không thêm AI/từ khóa. Giữ luồng GitHub làm phương án làm việc, cần xác nhận cách hướng dẫn phân loại với nhóm/khách. Không có mở lại/hủy ticket, email/push hoặc nút refresh SSO riêng.

**Cần xác nhận:** trường SSO thực tế và quy tắc hai trường bắt buộc; danh mục/mapping; các ngưỡng ký tự/rate limit/cache là lựa chọn đặc tả, chưa phải bằng chứng khách đã duyệt.

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Xác thực:** Mọi API yêu cầu phiên ứng dụng hợp lệ theo FR-SEC-01 và vai trò `STUDENT`. Sai hoặc thiếu token trả 401. Sai vai trò trả 403.
- **Định danh sinh viên:** Luôn lấy từ token. Hệ thống bỏ qua mọi `student_id` do client gửi lên.
- **Trạng thái hồ sơ:** `NEW` (Mới tiếp nhận), `IN_PROGRESS` (Đang xử lý), `PENDING_INFO` (Chờ bổ sung), `OVERDUE` (Quá hạn), `CLOSED` (Đã đóng). Quy tắc chuyển trạng thái theo CORE-02.
- **Hồ sơ không thuộc sinh viên:** Trả 404 để không tiết lộ hồ sơ có tồn tại hay không.
- **Vượt rate limit:** Trả 429 kèm header `Retry-After`.
- **Nhật ký hệ thống:** Theo SEC-06 (ghi các thao tác trên hồ sơ và tệp).
- **Ngôn ngữ:** Nhãn, danh mục và thông báo hiển thị theo ngôn ngữ người dùng chọn (VN/EN).

---

### [FR-STU-01] Xem thông tin cá nhân lấy từ SSO

**Mô tả**
Cho phép sinh viên xem thông tin cá nhân (họ tên, MSSV, khoa/lớp, email, SĐT) đồng bộ từ SSO. MSSV và Họ tên phải có; các trường tùy chọn thiếu được chuẩn hóa null và hiển thị --. Thiếu trường bắt buộc trả lỗi nguồn SSO, không hiển thị dữ liệu giả.

**Actor**
Sinh viên đã đăng nhập qua SSO.

**Preconditions**
- Sinh viên có JWT hợp lệ.
- Có cache Userinfo hợp lệ của đúng tài khoản hoặc kết nối được SSO/Userinfo để lấy mới.

**Luồng chính**
1. Sinh viên mở màn hình Thông tin cá nhân.
2. Hệ thống gửi yêu cầu lấy thông tin người dùng từ SSO dựa trên token.
3. Hệ thống nhận dữ liệu, kiểm tra và chuẩn hóa các trường.
4. Hệ thống hiển thị thông tin cá nhân.

**Input**
- **Người dùng/request:** Không có input chỉnh sửa thông tin; thao tác mở trang Thông tin cá nhân.
- **Hệ thống:** Phiên ứng dụng; hồ sơ Userinfo của đúng định danh SSO hoặc cache hợp lệ.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: full_name, student_code, faculty_name, class_name, email, phone_number, fetched_at. Trường tùy chọn thiếu là null.
- **Thay đổi/sự kiện:** Chỉ đọc; cập nhật cache khi hết hạn, không sửa snapshot các hồ sơ đã tạo.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Cache theo định danh SSO/account_id, TTL 15 phút; không dùng chung giữa tài khoản. MSSV/Họ tên chuỗi rỗng sau trim cũng là payload lỗi. Cache chỉ chứa dữ liệu của đúng định danh đã xác thực.
- MSSV và Họ tên là bắt buộc. Thiếu một trong hai trường này thì xem như payload lỗi.
- Các trường tùy chọn (Khoa, Lớp, Email, SĐT): nếu SSO không trả về, trả về null, chuỗi rỗng hoặc chỉ có khoảng trắng thì backend lưu và trả về `null`.
- UI hiển thị `--` cho trường `null`, không để trống ô và không hiện `undefined`.
- Thông tin lấy từ SSO được cache trên Redis trong 15 phút.

**Alternative / Error Flows**
- Nếu token không hợp lệ hoặc hết hạn, hệ thống trả 401 và yêu cầu đăng nhập lại.
- Nếu SSO lỗi kết nối hoặc phản hồi quá 3 giây, hệ thống trả 504 với thông báo "Không thể kết nối đến hệ thống xác thực".
- Nếu SSO thiếu MSSV hoặc Họ tên, hệ thống trả 502 với thông báo "Dữ liệu SSO không hợp lệ".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** SSO trả về đủ 6 trường -> 200, hiển thị đúng họ tên, MSSV, khoa, lớp, email, SĐT.
- **AC-02 [Validation]:** SSO chỉ trả về MSSV, Họ tên, Email -> 200; `phone_number`, `faculty_name`, `class_name` = `null`; UI hiển thị `--`.
- **AC-03 [Boundary]:** SSO trả về SĐT là `""` hoặc `"   "` -> hệ thống trim và chuyển thành `null`.
- **AC-04 [Security]:** Request thiếu cookie phiên ứng dụng, cookie sai chữ ký hoặc phiên hết hạn/đã thu hồi -> 401. Cookie hợp lệ không cần header Authorization.
- **AC-05 [Concurrency/Cache]:** Sinh viên tải lại trang 3 lần trong 1 phút -> lần 2 và lần 3 lấy dữ liệu từ Redis, không gọi lại SSO.
- **AC-06 [Resilience]:** Cache hết hạn và SSO không phản hồi quá 3 giây -> 504, không treo. SSO thiếu MSSV/Họ tên hoặc chuỗi rỗng -> 502. Cookie hợp lệ không có Authorization vẫn đọc được thông tin.

**Ví dụ Edge Case**
SSO không trả về Lớp, sinh viên thấy `--`. Khi cache hết hạn và sinh viên tải lại trang, hệ thống lấy lại Userinfo. Nếu Lớp vẫn thiếu thì giữ null; không có nút cập nhật SSO riêng trong phạm vi này.

**Expected Result:** Hiển thị đầy đủ các thông tin có sẵn. Thông tin thiếu được chuẩn hóa thành `null` và hiển thị `--`, không phát sinh lỗi.

---

### [FR-STU-02] Tạo yêu cầu hỗ trợ

**Mô tả**
Sinh viên tạo yêu cầu hỗ trợ bằng cách chọn loại yêu cầu và nhập mô tả vấn đề, để mọi yêu cầu đi vào một kênh duy nhất và theo dõi được.

**Actor**
Sinh viên đã đăng nhập (vai trò `STUDENT`).

**Preconditions**
- Sinh viên đã đăng nhập.
- Danh mục loại yêu cầu đã được cấu hình (CORE-05).
- Mỗi loại yêu cầu đã có SLA (FR-MGT-07).

**Luồng chính**
1. Sinh viên mở màn hình Tạo yêu cầu.
2. Hệ thống tải danh sách loại yêu cầu.
3. Sinh viên chọn loại yêu cầu, nhập mô tả và đính kèm tệp nếu có (FR-STU-04).
4. Sinh viên bấm Gửi.
5. Hệ thống kiểm tra dữ liệu.
6. Hệ thống tạo hồ sơ ở trạng thái `NEW`, gán phòng ban (FR-STU-03) và sinh mã hồ sơ (FR-STU-05).
7. Hệ thống lưu snapshot thông tin sinh viên lấy từ SSO tại thời điểm gửi (họ tên, MSSV, khoa/lớp, email, SĐT).
8. Hệ thống chốt hạn xử lý `due_at` theo SLA hiện tại của loại yêu cầu (FR-CORE-03).
9. Hệ thống hiển thị màn hình xác nhận (FR-STU-05).

**Input**
- **Người dùng/request:** request_type_id, description, 0–5 files; header Idempotency-Key.
- **Hệ thống:** Sinh viên từ phiên; danh mục/ánh xạ; SLA hiện tại; snapshot thông tin theo STU-01.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: ticket_id, code, request_type, department, status, created_at, file_count, version và liên kết chi tiết. Không trả priority, due_at, assignee hay dữ liệu nội bộ.
- **Thay đổi/sự kiện:** Tạo hồ sơ NEW, snapshot, hạn nội bộ, metadata tệp, lịch sử công khai và audit; không thông báo cho hành động của chính sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- `request_type_id`: bắt buộc, phải thuộc danh mục.
- `description`: bắt buộc, dài 20–2000 ký tự sau khi trim.
- Hồ sơ mới có `status = NEW` và `priority = MEDIUM` (CORE-04).
- Snapshot thông tin sinh viên được dùng cho nhân viên xem ở FR-STF-02 và không bị cập nhật khi dữ liệu SSO thay đổi sau này. Trường SSO không trả về được lưu là `null` (theo quy tắc FR-STU-01).
- `due_at = created_at + sla_hours` của loại yêu cầu tại thời điểm tạo. Đổi SLA về sau không ảnh hưởng tới hồ sơ này.
- Client phải gửi header `Idempotency-Key`. Key được lưu 24 giờ. Request trùng key trả về hồ sơ đã tạo.
- Rate limit: tối đa 10 hồ sơ mỗi giờ cho mỗi sinh viên.
- Tạo hồ sơ, snapshot, hạn xử lý, mã, metadata tệp, lịch sử và audit được commit trong một transaction DB. Tệp vật lý dùng lưu tạm và xóa bù theo quy ước chung; DB transaction không tự rollback kho tệp.

**Alternative / Error Flows**
- Nếu thiếu loại yêu cầu hoặc mô tả sai độ dài, hệ thống trả 400 kèm danh sách trường lỗi.
- Nếu `request_type_id` không tồn tại, hoặc loại yêu cầu chưa có SLA, hệ thống trả 422.
- Nếu thiếu `Idempotency-Key`, hệ thống trả 400.
- Nếu không lấy được snapshot hợp lệ: phiên lỗi trả 401, payload SSO thiếu trường bắt buộc trả 502, Userinfo quá 3 giây trả 504; không tạo hồ sơ. Dùng cache hợp lệ theo FR-STU-01, không bắt buộc gọi SSO mới ở mỗi lần gửi.
- Nếu vượt rate limit, hệ thống trả 429.
- Nếu lỗi cơ sở dữ liệu, hệ thống rollback toàn bộ và trả 500. UI giữ nguyên nội dung form để sinh viên gửi lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Chọn loại yêu cầu có SLA 48 giờ, mô tả 50 ký tự, gửi lúc 08:00 ngày 01/10 -> 201; hồ sơ `NEW`, ưu tiên `MEDIUM`, có mã, có snapshot thông tin sinh viên, `due_at` = 08:00 ngày 03/10.
- **AC-02 [Validation/Boundary]:** Mô tả 19 ký tự, 2001 ký tự hoặc toàn khoảng trắng -> 400, không tạo hồ sơ. Mô tả đúng 20 hoặc 2000 ký tự -> 201.
- **AC-03 [Security]:** Body chứa `student_id`, `priority` hoặc `due_at` -> hệ thống bỏ qua tất cả, dùng giá trị do hệ thống xác định. Token vai trò `STAFF` -> 403.
- **AC-04 [Idempotency]:** Gửi 2 lần cùng `Idempotency-Key` trong 24 giờ -> chỉ tạo 1 hồ sơ, lần thứ hai trả về đúng mã hồ sơ đã tạo.
- **AC-05 [Rate Limit]:** Gửi hồ sơ thứ 11 trong cùng 1 giờ -> 429 kèm `Retry-After`.
- **AC-06 [Resilience & Audit]:** SSO không phản hồi khi lấy snapshot -> 504, không có hồ sơ nào được tạo. Lỗi cơ sở dữ liệu giữa chừng -> 500, không có hồ sơ dở dang; tệp tạm được dọn bù theo quy ước chung. Tạo thành công -> nhật ký có bản ghi `CREATE_TICKET`.

**Ví dụ Edge Case**
Sinh viên bấm Gửi rồi mất mạng, không nhận được phản hồi, sau đó bấm Gửi lại. Client giữ nguyên `Idempotency-Key` cho đến khi nhận được phản hồi, nên hệ thống trả về hồ sơ đã tạo ở lần đầu và không tạo hồ sơ trùng.

**Expected Result:** Mỗi lần gửi hợp lệ tạo đúng một hồ sơ `NEW`, gắn đúng sinh viên trong token, có snapshot thông tin sinh viên và hạn xử lý được chốt.

---

### [FR-STU-03] Hiển thị phòng ban tiếp nhận

**Mô tả**
Khi sinh viên chọn loại yêu cầu, hệ thống hiển thị phòng ban tiếp nhận tương ứng và gán phòng ban đó khi tạo hồ sơ, giúp sinh viên biết yêu cầu của mình sẽ được gửi đến đâu.

**Actor**
Sinh viên đã đăng nhập.

**Preconditions**
- Sinh viên đang ở màn hình Tạo yêu cầu.
- Mỗi loại yêu cầu được ánh xạ tới đúng một phòng ban (CORE-05).

**Luồng chính**
1. Sinh viên chọn loại yêu cầu.
2. Hệ thống hiển thị tên phòng ban tiếp nhận tương ứng.
3. Sinh viên đổi loại yêu cầu thì phòng ban hiển thị cập nhật theo.
4. Khi sinh viên gửi, backend tự tra phòng ban theo loại yêu cầu và ghi vào hồ sơ.

**Input**
- **Người dùng/request:** request_type_id đang chọn; không có input department_id để quyết định nơi nhận.
- **Hệ thống:** Danh mục loại yêu cầu → phòng ban; ngôn ngữ tài khoản.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: danh mục và department_id/name tương ứng; màn hình cập nhật theo lựa chọn cuối.
- **Thay đổi/sự kiện:** Chỉ đọc/hiển thị; gán phòng ban thực tế trong transaction STU-02.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Phạm vi hiện tại dùng chọn loại yêu cầu + hiển thị phòng ban làm cách hướng dẫn phân loại. Không có AI hoặc tìm từ khóa từ mô tả. Đây là phương án làm rõ scope cần xác nhận khi review prototype.
- Phòng ban chỉ để hiển thị, sinh viên không sửa được.
- Backend không nhận `department_id` từ client. Phòng ban luôn được tra theo `request_type_id` tại thời điểm tạo hồ sơ.
- Danh mục loại yêu cầu và phòng ban được cache phía server vì là cấu hình cố định.
- Rate limit API danh mục: 60 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu loại yêu cầu chưa được ánh xạ phòng ban, hệ thống trả 422 "Loại yêu cầu chưa được cấu hình phòng ban", không tạo hồ sơ và ghi log lỗi cấu hình.
- Nếu không tải được danh mục, hệ thống trả 503 và UI khóa nút Gửi.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Chọn loại "Học phí" (ví dụ) -> hiển thị phòng ban ánh xạ (ví dụ "Phòng Tài chính"); hồ sơ tạo ra có đúng `department_id` này.
- **AC-02 [Validation]:** Đổi loại yêu cầu 3 lần rồi gửi -> phòng ban hiển thị và phòng ban được gán đều theo loại cuối cùng.
- **AC-03 [Security]:** Client gửi kèm `department_id` khác -> hệ thống bỏ qua, gán phòng ban theo ánh xạ.
- **AC-04 [Concurrency/Rate Limit]:** Nhiều sinh viên mở form cùng lúc -> danh mục được trả từ cache. Request API danh mục thứ 61 trong 1 phút -> 429.
- **AC-05 [Resilience]:** Loại yêu cầu thiếu ánh xạ phòng ban -> 422, không có hồ sơ nào được tạo mà thiếu phòng ban.
- **AC-06 [Audit]:** Hồ sơ vừa tạo -> bản ghi đầu tiên trong lịch sử hồ sơ (CORE-06) có `department_id` ban đầu.

**Ví dụ Edge Case**
Sinh viên đang điền form thì chuyển ngôn ngữ sang EN. Tên loại yêu cầu và tên phòng ban hiển thị bằng tiếng Anh, lựa chọn và nội dung đã nhập vẫn giữ nguyên.

**Expected Result:** Mọi hồ sơ được gán đúng một phòng ban theo ánh xạ của loại yêu cầu, không phụ thuộc dữ liệu client gửi lên.

---

### [FR-STU-04] Đính kèm tệp minh chứng

**Mô tả**
Cho phép sinh viên đính kèm tệp minh chứng khi tạo yêu cầu, để nhân viên có đủ thông tin xử lý mà không phải hỏi lại.

**Actor**
Sinh viên đã đăng nhập.

**Preconditions**
- Sinh viên đang ở màn hình Tạo yêu cầu (hoặc Bổ sung thông tin, theo FR-STU-09).

**Luồng chính**
1. Sinh viên chọn tệp từ thiết bị.
2. Hệ thống kiểm tra định dạng, dung lượng và số lượng tệp ngay trên UI.
3. Sinh viên gửi yêu cầu. Tệp được gửi cùng request tạo hồ sơ (multipart).
4. Hệ thống kiểm tra lại ở backend, lưu tệp và gắn vào hồ sơ.

**Input**
- **Người dùng/request:** 0–5 files trong multipart STU-02 hoặc STU-09; không phải API tạo hồ sơ riêng.
- **Hệ thống:** Phiên, quyền với thao tác gốc, magic bytes, giới hạn cấu hình.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Kết quả dùng HTTP status của thao tác gốc; metadata gồm file_id, original_name, verified_mime, size_bytes. Không trả đường dẫn kho riêng.
- **Thay đổi/sự kiện:** Lưu tệp riêng và metadata sau khi hợp lệ; lỗi không để hồ sơ/bổ sung thành công một phần.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Định dạng cho phép: PDF, JPG, PNG. Kiểm tra theo nội dung tệp (magic bytes), không chỉ theo đuôi tệp.
- Tối đa 5 tệp mỗi lần gửi; mỗi tệp tối đa 10 MiB (10.485.760 byte) (CORE-07). Không đính kèm tệp nào vẫn hợp lệ.
- Tệp lưu với tên ngẫu nhiên, tên gốc giữ lại để hiển thị.
- Tệp lưu ngoài thư mục web công khai, chỉ tải được qua API có kiểm tra quyền (SEC-05).
- Lỗi ở bất kỳ tệp nào thì toàn bộ request bị từ chối, không tạo hồ sơ.

**Alternative / Error Flows**
- Nếu một tệp vượt 10MB, hệ thống trả 413.
- Nếu tệp sai định dạng, hệ thống trả 415.
- Nếu có hơn 5 tệp, hệ thống trả 400.
- Nếu lỗi kho lưu trữ, hệ thống rollback, xóa các tệp đã lưu và trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Gửi 2 tệp PDF 1MB -> 201; hai tệp gắn vào hồ sơ, hiển thị đúng tên gốc.
- **AC-02 [Boundary]:** Tệp đúng 10MB -> chấp nhận; tệp 10MB + 1 byte -> 413. Gửi 5 tệp -> chấp nhận; gửi 6 tệp -> 400.
- **AC-03 [Validation]:** Tệp `.exe` đổi đuôi thành `.pdf` -> 415.
- **AC-04 [Security]:** Gọi API tải tệp không có token -> 401. Sinh viên khác gọi API tải tệp -> 404.
- **AC-05 [Idempotency]:** Gửi lại request cùng `Idempotency-Key` -> tệp không bị lưu thêm lần nữa.
- **AC-06 [Resilience & Audit]:** Lưu tệp thứ 3 bị lỗi -> rollback, không có hồ sơ, tệp không tham chiếu không tải được và được dọn theo quy ước chung. Lưu thành công -> nhật ký có bản ghi `UPLOAD_FILE` cho từng tệp.

**Ví dụ Edge Case**
Sinh viên chụp minh chứng bằng iPhone và tệp có định dạng HEIC. Hệ thống trả 415 với thông báo "Chỉ chấp nhận PDF, JPG, PNG. Vui lòng chuyển ảnh sang JPG hoặc PNG."

**Expected Result:** Chỉ tệp hợp lệ được lưu, chỉ người có quyền tải được, và hồ sơ không bao giờ ở trạng thái dở dang vì lỗi tệp.

---

### [FR-STU-05] Nhận mã hồ sơ và xác nhận gửi thành công

**Mô tả**
Sau khi gửi thành công, sinh viên nhận mã hồ sơ duy nhất và màn hình xác nhận, để biết chắc yêu cầu đã được tiếp nhận và có mã để tra cứu.

**Actor**
Sinh viên vừa gửi yêu cầu thành công.

**Preconditions**
- Hồ sơ được tạo thành công theo FR-STU-02.

**Luồng chính**
1. Hệ thống sinh mã hồ sơ trong cùng transaction tạo hồ sơ.
2. Hệ thống trả về mã và thông tin hồ sơ.
3. UI hiển thị màn hình xác nhận.

**Input**
- **Người dùng/request:** Không có input mã do sinh viên tự chọn; nhận kết quả tạo hồ sơ.
- **Hệ thống:** Năm Việt Nam tại created_at và sequence của năm.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Mã USP-YYYY-NNNNN, loại yêu cầu, phòng ban, trạng thái, ngày gửi và số tệp trên màn hình xác nhận.
- **Thay đổi/sự kiện:** Mã được sinh trong STU-02; không gọi tạo mã lần nữa khi render/retry.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Định dạng mã: `USP-YYYY-NNNNN`. `YYYY` là năm tạo hồ sơ theo giờ Việt Nam (UTC+7). `NNNNN` là số thứ tự tăng dần, bắt đầu lại từ `00001` mỗi năm.
- Số thứ tự sinh bằng sequence của cơ sở dữ liệu. Cột mã có unique constraint.
- Nếu trong một năm vượt quá 99999 hồ sơ thì phần số tự mở rộng thêm chữ số, không báo lỗi.
- Mã không thay đổi trong suốt vòng đời hồ sơ và không chứa thông tin cá nhân.
- Màn hình xác nhận hiển thị: mã hồ sơ, loại yêu cầu, phòng ban, trạng thái "Mới tiếp nhận", thời gian gửi, số tệp đính kèm.

**Alternative / Error Flows**
- Nếu sinh mã lỗi, hệ thống rollback, không tạo hồ sơ và trả 500.
- Nếu tra cứu một mã không thuộc sinh viên, hệ thống trả 404.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ đầu tiên của năm 2027 -> mã `USP-2027-00001`; màn hình xác nhận hiển thị đủ thông tin.
- **AC-02 [Boundary]:** Hồ sơ tạo lúc 23:59:59 ngày 31/12/2026 (giờ VN) -> mã năm 2026. Hồ sơ tạo lúc 00:00:00 ngày 01/01/2027 -> `USP-2027-00001`.
- **AC-03 [Security]:** Sinh viên A tra mã hồ sơ của sinh viên B -> 404.
- **AC-04 [Concurrency]:** 50 hồ sơ gửi đồng thời -> 50 mã khác nhau, không trùng.
- **AC-05 [Idempotency]:** Gửi lại cùng `Idempotency-Key` -> trả về mã cũ, không sinh mã mới.
- **AC-06 [Resilience & Audit]:** Sinh mã lỗi -> 500, không có hồ sơ thiếu mã. Tạo thành công -> mã hồ sơ được ghi trong bản ghi nhật ký `CREATE_TICKET`.

**Ví dụ Edge Case**
Sinh viên đóng trình duyệt trước khi màn hình xác nhận kịp hiện. Hồ sơ đã được tạo nên vẫn xuất hiện trong danh sách yêu cầu (FR-STU-06), kèm mã hồ sơ.

**Expected Result:** Mỗi hồ sơ có đúng một mã duy nhất, đúng định dạng và đúng năm theo giờ Việt Nam.

---

### [FR-STU-06] Xem danh sách yêu cầu của mình

**Mô tả**
Sinh viên xem toàn bộ yêu cầu mình đã gửi cùng trạng thái hiện tại, không phải liên hệ phòng ban để hỏi tiến độ.

**Actor**
Sinh viên đã đăng nhập.

**Preconditions**
- Sinh viên đã đăng nhập.

**Luồng chính**
1. Sinh viên mở màn hình Yêu cầu của tôi.
2. Hệ thống trả về danh sách hồ sơ của sinh viên, sắp xếp mới nhất trước.
3. Sinh viên lọc theo trạng thái hoặc tìm theo mã hồ sơ.
4. Sinh viên chọn một hồ sơ để xem chi tiết (FR-STU-07).

**Input**
- **Người dùng/request:** page mặc định 1; page_size mặc định 10, 1–50; statuses; code tìm chính xác.
- **Hệ thống:** Sinh viên từ phiên; danh mục trạng thái.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: items, page, page_size, total_items, total_pages; mỗi item có mã, loại, phòng hiện tại, trạng thái, created_at, needs_info.
- **Thay đổi/sự kiện:** Chỉ đọc; tải khi mở trang, đổi bộ lọc, phân trang hoặc bấm Làm mới.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- page/page_size phải là số nguyên dương; page_size không hợp lệ trả 400. Cùng created_at thì sắp ticket_id giảm dần để thứ tự ổn định.
- Chỉ trả về hồ sơ của sinh viên trong token.
- Mặc định 10 hồ sơ mỗi trang; `page_size` tối đa 50.
- Lọc theo một hoặc nhiều trạng thái; tìm theo mã hồ sơ chính xác.
- Mỗi dòng hiển thị: mã hồ sơ, loại yêu cầu, phòng ban hiện tại, trạng thái, ngày gửi, và dấu hiệu "Cần bổ sung" nếu có yêu cầu bổ sung đang mở.
- Rate limit: 60 request mỗi phút cho mỗi sinh viên.

**Alternative / Error Flows**
- Nếu `page < 1`, `page_size > 50` hoặc giá trị trạng thái không hợp lệ, hệ thống trả 400.
- Nếu truy vấn quá 5 giây, hệ thống trả 503 và UI hiển thị nút Thử lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên có 25 hồ sơ -> trang 1 có 10 hồ sơ mới nhất, tổng số trang là 3.
- **AC-02 [Validation]:** Lọc `OVERDUE` -> chỉ trả về hồ sơ quá hạn. Không có kết quả -> 200 với danh sách rỗng và dòng chữ "Chưa có yêu cầu nào".
- **AC-03 [Boundary]:** `page_size = 51` -> 400. `page` lớn hơn tổng số trang -> 200 với danh sách rỗng.
- **AC-04 [Security]:** Truyền `student_id` của người khác trên query -> hệ thống bỏ qua, chỉ trả hồ sơ của người dùng trong token.
- **AC-05 [Rate Limit]:** Request thứ 61 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Cơ sở dữ liệu phản hồi quá 5 giây -> 503 với thông báo "Vui lòng thử lại", trang không bị trắng.

**Ví dụ Edge Case**
Hồ sơ vừa được nhân viên chuyển sang phòng ban khác. Khi tải lại danh sách, cột phòng ban hiển thị phòng ban mới.

**Expected Result:** Sinh viên chỉ thấy hồ sơ của mình, với trạng thái và phòng ban đúng tại thời điểm xem.

---

### [FR-STU-07] Xem trạng thái và tiến độ của từng yêu cầu

**Mô tả**
Sinh viên xem chi tiết một hồ sơ, gồm trạng thái hiện tại, các mốc cập nhật và kết quả xử lý, để biết yêu cầu đang ở đâu và khi nào cần bổ sung thông tin.

**Actor**
Sinh viên chủ hồ sơ.

**Preconditions**
- Hồ sơ thuộc sinh viên đang đăng nhập.

**Luồng chính**
1. Sinh viên chọn một hồ sơ từ danh sách hoặc nhập mã hồ sơ.
2. Hệ thống trả về thông tin hồ sơ và dòng thời gian cập nhật.
3. Nếu hồ sơ có yêu cầu bổ sung đang mở, UI hiển thị thông báo nổi bật "Cần bổ sung thông tin" kèm nút Bổ sung (FR-STU-09).
4. Nếu hồ sơ đã `CLOSED`, UI hiển thị kết quả xử lý và form đánh giá (FR-STU-10).
5. Trang tự cập nhật định kỳ trong lúc đang mở.

**Input**
- **Người dùng/request:** ticket_id hoặc code hợp lệ của hồ sơ.
- **Hệ thống:** Sinh viên từ phiên; hồ sơ, yêu cầu bổ sung, timeline công khai, đánh giá.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: hồ sơ, version, tệp, yêu cầu bổ sung công khai, public_history; CLOSED có resolution/closed_at và đánh giá đã có hoặc form đánh giá.
- **Thay đổi/sự kiện:** Chỉ đọc, polling 30 giây; API và timeline cùng loại bỏ dữ liệu nội bộ.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Hồ sơ đã đánh giá hiển thị đánh giá chỉ đọc, không hiện form gửi lần nữa. Kiểm tra cả public_history và response chính để không trả due_at, priority, assignee hoặc lý do nội bộ.
- Thông tin hiển thị: mã hồ sơ, loại yêu cầu, phòng ban hiện tại, trạng thái, ngày gửi, mô tả, tệp đính kèm.
- Khi hồ sơ `CLOSED`: hiển thị thêm kết quả xử lý (`resolution`, nhập ở FR-STF-06) và thời điểm đóng.
- Dòng thời gian chỉ gồm các mốc công khai theo FR-CORE-06.
- Không hiển thị: nhật ký xử lý nội bộ, tên nhân viên phụ trách, mức ưu tiên, hạn xử lý, lý do chuyển phòng ban.
- Trang tự cập nhật bằng polling mỗi 30 giây.
- Thông báo "Cần bổ sung thông tin" hiển thị theo yêu cầu bổ sung đang mở, kể cả khi hồ sơ ở trạng thái `OVERDUE`.
- Rate limit: 60 request mỗi phút cho mỗi sinh viên.

**Alternative / Error Flows**
- Nếu mã hồ sơ sai định dạng, hệ thống trả 400.
- Nếu hồ sơ không tồn tại hoặc không thuộc sinh viên, hệ thống trả 404.
- Nếu không tải được dòng thời gian, hệ thống vẫn hiển thị thông tin chính và báo "Chưa tải được lịch sử cập nhật".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ CLOSED chưa đánh giá -> trạng thái/kết quả/closed_at/timeline/form rating; đã đánh giá -> rating chỉ đọc. Public JSON/timeline không có due_at hoặc trường nội bộ.
- **AC-02 [Validation]:** Nhập mã `USP-26-1` -> 400 "Mã hồ sơ không đúng định dạng".
- **AC-03 [Security]:** Mở hồ sơ của sinh viên khác -> 404. Response của hồ sơ hợp lệ không chứa ghi chú nội bộ, thông tin nhân viên, mức ưu tiên, `due_at` hay lý do chuyển phòng ban.
- **AC-04 [Concurrency]:** Trang đang mở và nhân viên đổi trạng thái -> trạng thái mới hiển thị trong vòng 30 giây mà không cần tải lại trang.
- **AC-05 [Boundary]:** Hồ sơ `OVERDUE` có yêu cầu bổ sung đang mở -> trạng thái hiển thị "Quá hạn", đồng thời có thông báo "Cần bổ sung thông tin" và nút Bổ sung.
- **AC-06 [Resilience & Audit]:** API lịch sử lỗi -> thông tin chính vẫn hiển thị, không trắng trang. Sinh viên tải tệp từ trang chi tiết -> nhật ký có bản ghi `DOWNLOAD_FILE`.

**Ví dụ Edge Case**
Sinh viên đang mở trang chi tiết thì nhân viên đóng hồ sơ. Trong vòng 30 giây, trang tự cập nhật: trạng thái đổi thành "Đã đóng", kết quả xử lý và form đánh giá xuất hiện mà sinh viên không cần tải lại.

**Expected Result:** Sinh viên luôn thấy trạng thái đúng, biết rõ khi nào cần làm gì, nhận được kết quả xử lý khi hồ sơ đóng, và không thấy thông tin nội bộ.

---

### [FR-STU-08] Nhận thông báo trong app

**Mô tả**
Hệ thống thông báo trong app cho sinh viên khi trạng thái hồ sơ thay đổi hoặc khi có yêu cầu bổ sung, để sinh viên không phải tự vào kiểm tra.

**Actor**
Sinh viên đã đăng nhập (người nhận). Thông báo được tạo tự động bởi hệ thống.

**Preconditions**
- Sinh viên có ít nhất một hồ sơ.

**Luồng chính**
1. Một sự kiện xảy ra trên hồ sơ: trạng thái đổi, hoặc nhân viên yêu cầu bổ sung.
2. Hệ thống tạo thông báo cho sinh viên chủ hồ sơ.
3. Biểu tượng chuông hiển thị số thông báo chưa đọc.
4. Sinh viên mở danh sách thông báo và chọn một thông báo.
5. Hệ thống đánh dấu thông báo đã đọc và chuyển tới chi tiết hồ sơ (FR-STU-07).

**Input**
- **Người dùng/request:** Đọc danh sách: page/page_size; đánh dấu một: notification_id; đánh dấu tất cả: read_before là thời điểm chụp danh sách do server cấp.
- **Hệ thống:** Sinh viên từ phiên; notification event_type và tham số.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: items, total_items, page, total_pages, unread_count, snapshot_at; đánh dấu đọc trả unread_count và thời điểm xử lý.
- **Thay đổi/sự kiện:** Cập nhật read_at thuộc người dùng; đánh dấu tất cả chỉ áp dụng thông báo tạo đến read_before, không nuốt thông báo vừa đến.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- page_size mặc định 20, tối đa 100; page ≥1. Đánh dấu tất cả dùng read_before do server cấp và chỉ đổi thông báo của chính sinh viên tạo trước/đúng mốc đó. Một lần REQUEST_INFO đồng thời đổi trạng thái chỉ sinh một thông báo, cùng event_id.
- Sự kiện tạo thông báo: hồ sơ đổi sang `IN_PROGRESS`, `PENDING_INFO`, `OVERDUE` hoặc `CLOSED`; nhân viên tạo yêu cầu bổ sung (kể cả khi hồ sơ đang `OVERDUE` nên trạng thái không đổi).
- Không thông báo cho sinh viên về hành động do chính sinh viên thực hiện (ví dụ `PENDING_INFO -> IN_PROGRESS` sau khi sinh viên bổ sung xong).
- Thông báo được tạo trong cùng transaction với sự kiện. Mỗi sự kiện tạo tối đa một thông báo (khóa duy nhất theo `event_id`).
- Nội dung lưu dạng `event_type` kèm tham số, và được hiển thị theo ngôn ngữ người dùng.
- Số chưa đọc cập nhật bằng polling mỗi 30 giây. Lớn hơn 99 thì hiển thị "99+".
- Danh sách thông báo: 20 thông báo mỗi trang, mới nhất trước. Có thể đánh dấu đã đọc từng thông báo hoặc tất cả.

**Alternative / Error Flows**
- Nếu sinh viên đánh dấu một thông báo không thuộc mình, hệ thống trả 404.
- Nếu sự kiện bị rollback, hệ thống không tạo thông báo.
- Nếu API số chưa đọc lỗi, UI giữ số cũ và thử lại ở lần polling sau.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Nhân viên chuyển hồ sơ sang `PENDING_INFO` -> sinh viên có 1 thông báo chưa đọc, số trên chuông tăng 1 trong vòng 30 giây.
- **AC-02 [Validation]:** Sinh viên bổ sung xong và hồ sơ về `IN_PROGRESS` -> không có thông báo mới cho sinh viên.
- **AC-03 [Boundary]:** Có 120 thông báo chưa đọc -> chuông hiển thị "99+".
- **AC-04 [Security]:** Sinh viên A đánh dấu đọc thông báo của sinh viên B -> 404.
- **AC-05 [Idempotency]:** Đánh dấu đã đọc cùng một thông báo 2 lần -> cả hai lần đều 200, kết quả như nhau. Một event REQUEST_INFO đồng thời đổi PENDING_INFO, hoặc xử lý lại event -> một thông báo; read-all tại snapshot không đánh dấu thông báo tạo sau snapshot.
- **AC-06 [Resilience]:** Thao tác đổi trạng thái bị rollback -> không có thông báo nào được tạo.

**Ví dụ Edge Case**
Hồ sơ đang ở `OVERDUE` và nhân viên yêu cầu bổ sung. Trạng thái vẫn là "Quá hạn" (CORE-02), nhưng sinh viên vẫn nhận thông báo "Hồ sơ USP-2026-00123 cần bổ sung thông tin".

**Expected Result:** Mỗi sự kiện cần báo tạo đúng một thông báo cho đúng sinh viên, không mất và không trùng.

---

### [FR-STU-09] Bổ sung thông tin khi được yêu cầu

**Mô tả**
Sinh viên gửi nội dung và tệp bổ sung khi nhân viên yêu cầu, để hồ sơ tiếp tục được xử lý mà không phải tạo yêu cầu mới.

**Actor**
Sinh viên chủ hồ sơ.

**Preconditions**
- Hồ sơ thuộc sinh viên đang đăng nhập.
- Hồ sơ có một yêu cầu bổ sung đang mở (hồ sơ ở `PENDING_INFO`, hoặc ở `OVERDUE` nhưng có yêu cầu bổ sung chưa trả lời).

**Luồng chính**
1. Sinh viên mở hồ sơ và xem nội dung nhân viên yêu cầu bổ sung.
2. Sinh viên nhập nội dung bổ sung và đính kèm tệp nếu cần.
3. Sinh viên bấm Gửi.
4. Hệ thống lưu nội dung và tệp, rồi đóng yêu cầu bổ sung.
5. Nếu hồ sơ đang `PENDING_INFO`, hệ thống chuyển sang `IN_PROGRESS` và tiếp tục tính SLA. Nếu hồ sơ đang `OVERDUE`, trạng thái giữ nguyên.
6. Hệ thống ghi mốc bổ sung vào lịch sử hồ sơ.

**Input**
- **Người dùng/request:** ticket_id, info_request_id, version, content, 0–5 files; header Idempotency-Key.
- **Hệ thống:** Chủ hồ sơ từ phiên; yêu cầu bổ sung mở; trạng thái và khoảng pause hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: supplement_id, ticket_id/code, info_request_id, created_at, file metadata, status, version mới; không trả due_at.
- **Thay đổi/sự kiện:** Đóng yêu cầu bổ sung; PENDING_INFO → IN_PROGRESS và cộng bù hạn, hoặc giữ OVERDUE; ghi timeline/audit; không thông báo cho hành động của chính sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Bắt buộc ticket_id, info_request_id và version từ trang đang xem. Cập nhật ticket và info request theo kiểm tra nguyên tử; version tăng một lần khi bổ sung thành công. Hai thao tác bổ sung/đóng với cùng version cũ chỉ một bên thành công; thao tác sau khi tải version mới vẫn có thể hợp lệ.
- Retry cùng Idempotency-Key hợp lệ trả kết quả cũ sau kiểm tra quyền, trước kiểm tra version/trạng thái cũ; payload khác trả 409. Info request được nhận phải đúng yêu cầu đang mở.
- `content`: bắt buộc, dài 10–2000 ký tự sau khi trim.
- Tệp: theo quy tắc của FR-STU-04, tối đa 5 tệp cho mỗi lần bổ sung.
- Mỗi yêu cầu bổ sung chỉ được trả lời một lần.
- Không cho bổ sung khi hồ sơ đã `CLOSED`.
- Bắt buộc có header `Idempotency-Key`.

**Alternative / Error Flows**
- Nếu thiếu/sai kiểu version hoặc info_request_id, trả 400; version cũ hoặc info_request_id không trùng yêu cầu mở, trả 409 sau kiểm tra scope và retry. Nếu không có yêu cầu mở, trả 409.
- Nếu hồ sơ đã `CLOSED`, hệ thống trả 409 "Hồ sơ đã đóng".
- Nếu nội dung sai độ dài, hệ thống trả 400. Tệp lỗi trả 413 hoặc 415 như FR-STU-04.
- Nếu lưu DB lỗi, rollback nghiệp vụ, trạng thái không đổi và trả 500; lỗi storage trả 503 theo STU-04, tệp tạm được dọn bù.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `PENDING_INFO`, gửi nội dung hợp lệ kèm 1 tệp -> 201; hồ sơ chuyển `IN_PROGRESS`; yêu cầu bổ sung được đóng; SLA tiếp tục tính.
- **AC-02 [Boundary]:** Hồ sơ `OVERDUE` có yêu cầu bổ sung đang mở, gửi bổ sung -> 201; trạng thái vẫn `OVERDUE`; yêu cầu bổ sung được đóng.
- **AC-03 [Validation]:** Nội dung 9 ký tự -> 400. Gửi 6 tệp -> 400.
- **AC-04 [Security]:** Bổ sung cho hồ sơ của sinh viên khác -> 404.
- **AC-05 [Concurrency/Idempotency]:** Sinh viên bổ sung và nhân viên đóng dùng cùng version đã đọc -> một thao tác commit, bên kia 409; nếu hồ sơ đóng trước thì không lưu bổ sung. Bên đóng tải version mới sau bổ sung vẫn có thể đóng. Bấm Gửi 2 lần cùng `Idempotency-Key` -> chỉ lưu 1 lần bổ sung.
- **AC-06 [Resilience & Audit]:** Lưu tệp lỗi -> rollback, trạng thái và yêu cầu bổ sung giữ nguyên. Thành công -> nhật ký có bản ghi `SUPPLEMENT_SUBMITTED`, lịch sử hồ sơ có mốc bổ sung.

**Ví dụ Edge Case**
Sinh viên mở form bổ sung trên hai tab trình duyệt và gửi ở cả hai. Hai tab cùng snapshot nhưng khác key: tab commit trước thành công; tab sau 409 do version cũ/yêu cầu đã trả lời, không lưu thêm. Nếu cùng key/cùng payload thì tab sau nhận kết quả đã lưu theo quy ước retry.

**Expected Result:** Mỗi yêu cầu bổ sung nhận đúng một lần trả lời, và trạng thái hồ sơ chuyển đúng theo CORE-02.

---

### [FR-STU-10] Đánh giá chất lượng hỗ trợ

**Mô tả**
Sau khi hồ sơ đóng, sinh viên chấm 1–5 sao kèm nhận xét, cung cấp dữ liệu hài lòng cho báo cáo của Ban quản lý (MGT-05).

**Actor**
Sinh viên chủ hồ sơ.

**Preconditions**
- Hồ sơ thuộc sinh viên đang đăng nhập.
- Hồ sơ ở trạng thái `CLOSED`.
- Hồ sơ chưa được đánh giá.

**Luồng chính**
1. Sinh viên mở hồ sơ đã đóng.
2. UI hiển thị form đánh giá gồm chọn số sao và ô nhận xét.
3. Sinh viên chọn số sao, nhập nhận xét và bấm Gửi.
4. Hệ thống lưu đánh giá.
5. UI hiển thị đánh giá ở chế độ chỉ đọc.

**Input**
- **Người dùng/request:** ticket_id, rating số nguyên 1–5, comment tùy chọn tối đa 1000 ký tự.
- **Hệ thống:** Sinh viên từ phiên; trạng thái CLOSED; unique đánh giá theo ticket_id.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 201: rating_id, ticket_id, rating, comment, created_at; lần đã có trả 409 và UI tải lại đánh giá.
- **Thay đổi/sự kiện:** Tạo một đánh giá, không sửa ticket hoặc tăng version hồ sơ; ghi RATE_TICKET; báo cáo tính vào kỳ closed_at.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Không yêu cầu Idempotency-Key riêng: unique ticket_id ngăn đánh giá trùng. Nếu mất phản hồi rồi gửi lại nhận 409, UI tải lại đánh giá để xác nhận thay vì báo thất bại chung. Tạo rating không phải sửa ticket CLOSED.
- `rating`: bắt buộc, là số nguyên từ 1 đến 5.
- `comment`: tùy chọn, tối đa 1000 ký tự.
- Mỗi hồ sơ chỉ được đánh giá một lần (unique theo `ticket_id`). Không được sửa sau khi gửi.
- Không giới hạn thời gian đánh giá sau khi hồ sơ đóng.
- Nhận xét được lưu dạng văn bản thuần và escape khi hiển thị.
- Không có chức năng mở lại hồ sơ. Nếu vấn đề chưa được giải quyết, UI gợi ý sinh viên tạo yêu cầu mới.

**Alternative / Error Flows**
- Nếu hồ sơ chưa `CLOSED`, hệ thống trả 409 "Chỉ đánh giá được hồ sơ đã đóng".
- Nếu hồ sơ đã có đánh giá, hệ thống trả 409 "Hồ sơ đã được đánh giá".
- Nếu `rating` hoặc `comment` không hợp lệ, hệ thống trả 400.
- Nếu lưu dữ liệu lỗi, hệ thống trả 500 và sinh viên có thể gửi lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ `CLOSED`, chọn 4 sao và nhập nhận xét -> 201; đánh giá hiển thị ở chế độ chỉ đọc và được tính vào MGT-05.
- **AC-02 [Validation/Boundary]:** `rating` = 0, 6 hoặc 3.5 -> 400. Nhận xét 1000 ký tự -> 201; 1001 ký tự -> 400. Không nhập nhận xét -> 201.
- **AC-03 [Security]:** Đánh giá hồ sơ của sinh viên khác -> 404. Nhận xét chứa `<script>` -> lưu dạng văn bản, khi hiển thị không thực thi.
- **AC-04 [Business Logic]:** Đánh giá hồ sơ đang `IN_PROGRESS` -> 409.
- **AC-05 [Concurrency/Idempotency]:** Bấm Gửi 2 lần liên tiếp -> chỉ lưu 1 đánh giá, lần thứ hai trả 409.
- **AC-06 [Resilience & Audit]:** Lưu đánh giá lỗi -> 500, không có đánh giá dở dang, sinh viên gửi lại được. Lưu thành công -> nhật ký có bản ghi `RATE_TICKET`.

**Ví dụ Edge Case**
Sinh viên chấm 1 sao vì vấn đề chưa được giải quyết và muốn mở lại hồ sơ. Hệ thống không có chức năng mở lại. Sau khi lưu đánh giá, UI hiển thị gợi ý "Nếu vấn đề chưa được giải quyết, bạn có thể tạo yêu cầu mới" kèm nút Tạo yêu cầu.

**Expected Result:** Mỗi hồ sơ đã đóng có tối đa một đánh giá hợp lệ, không sửa được, và được dùng cho báo cáo hài lòng.
