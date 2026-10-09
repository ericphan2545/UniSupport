# M3. Cổng Quản lý – Functional Requirement Specification

> Cập nhật: 09/10/2026. Giữ nguyên 42 chức năng của 5 module trong repo; bổ sung và thống nhất đặc tả trực tiếp tại module. Các giả định chưa được trường xác nhận được ghi tại phần đầu module.

**Mục đích module:** Báo cáo toàn hệ thống, quản lý quyền và SLA.

**Actor chính:** MANAGER. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M4 xác thực/phạm vi; dữ liệu hồ sơ, đánh giá và danh mục từ M1, M2, M5.

## Luồng quản lý và liên kết module

Manager đăng nhập M4 → dashboard số lượng hiện tại/trung bình CLOSED → danh sách tóm tắt nếu chọn chỉ số → báo cáo loại/khối lượng/hài lòng từ dữ liệu M1/M2 → cấp quyền tài khoản và thiết lập SLA. Manager không xử lý ticket hoặc xem chi tiết/tệp sinh viên.

**Input module:** phiên MANAGER, bộ lọc kỳ/phòng/loại/sao, định danh/quyền/phòng/active và SLA/version cấu hình.
**Output module:** số liệu/tables/charts cùng dữ liệu, danh sách tóm tắt không thông tin cá nhân, quyền và SLA đã cập nhật.

**Kỳ báo cáo:** count dashboard dùng hiện tại; loại phổ biến dùng created_at/phòng hiện tại; trung bình/hài lòng dùng closed_at/phòng lúc đóng. Cột khối lượng gọi đúng “Hồ sơ tạo trong kỳ, hiện thuộc phòng ban”, không gọi lượt tiếp nhận thực tế.

**Cần xác nhận:** chỉ số/nhãn báo cáo với người quản lý khách; danh mục/SLA đầu; email MANAGER bootstrap và định danh SSO. Các giá trị lọc mặc định/ngưỡng là phương án đặc tả chưa có biên bản xác nhận.

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Xác thực:** Mọi API yêu cầu phiên ứng dụng hợp lệ theo SEC-01 và vai trò MANAGER. Sai hoặc thiếu token trả 401. Sai vai trò trả 403.
- **Kiểm tra quyền:** Vai trò và phòng ban được đọc từ cơ sở dữ liệu ở mỗi request, không đọc từ claim trong JWT. Nhờ vậy thay đổi phân quyền (FR-MGT-06) có hiệu lực ngay ở request kế tiếp.
- **Phạm vi dữ liệu:** Ban quản lý xem số liệu của toàn bộ phòng ban.
- **Khoảng thời gian báo cáo:**
  - Tính theo giờ Việt Nam (UTC+7), bao gồm cả ngày bắt đầu và ngày kết thúc.
  - Mặc định 30 ngày gần nhất. Tối đa 365 ngày.
  - `from` không được sau `to`.
- **Bộ lọc dùng chung:** phòng ban, loại yêu cầu, khoảng thời gian (chỉ áp dụng cho FR nào có ghi rõ).
- **Lỗi truy vấn chậm:** Truy vấn báo cáo quá 10 giây trả 503 "Vui lòng thử lại".
- **Vượt rate limit:** Trả 429 kèm `Retry-After`.
- **Nhật ký hệ thống:** Theo SEC-06. Các thay đổi cấu hình ở FR-MGT-06 và FR-MGT-07 cũng được ghi nhật ký.

---

### [FR-MGT-01] Dashboard: số hồ sơ mới, đang xử lý, quá hạn

**Mô tả**
Hiển thị số lượng hồ sơ theo trạng thái tại thời điểm hiện tại, giúp Ban quản lý biết ngay khối lượng đang chờ và số hồ sơ đang trễ hạn.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở Dashboard.
2. Hệ thống đếm số hồ sơ theo từng nhóm trạng thái tại thời điểm hiện tại.
3. Hệ thống hiển thị 3 chỉ số: Mới, Đang xử lý, Quá hạn.
4. Ban quản lý lọc theo phòng ban nếu cần.
5. Dashboard tự cập nhật định kỳ.

**Input**
- **Người dùng/request:** department_id tùy chọn; drill-down thêm group=new/in_progress/overdue, page=1, page_size=20 (1–100).
- **Hệ thống:** MANAGER active; trạng thái hiện tại theo phòng hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: new_count, in_progress_count, overdue_count, updated_at; drill-down có items/total/page và chỉ code, loại, phòng, trạng thái, ngày gửi.
- **Thay đổi/sự kiện:** Chỉ đọc; polling cả số liệu và danh sách mở mỗi 30 giây.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Drill-down có page/page_size, total_items/total_pages, sắp created_at giảm dần rồi ticket_id giảm dần. Bất kỳ filter/giá trị group không hợp lệ trả 400. Danh sách chỉ tóm tắt, không có liên kết thao tác xử lý/chi tiết staff.
- Mới = số hồ sơ `NEW`.
- Đang xử lý = số hồ sơ `IN_PROGRESS` cộng `PENDING_INFO`.
- Quá hạn = số hồ sơ `OVERDUE`.
- Hồ sơ `CLOSED` không được tính.
- Số liệu là ảnh chụp tại thời điểm hiện tại, không phụ thuộc khoảng thời gian.
- Bộ lọc: phòng ban (mặc định là tất cả).
- Tự cập nhật bằng polling mỗi 30 giây. Hiển thị thời điểm cập nhật gần nhất.
- Bấm vào một chỉ số thì mở danh sách các hồ sơ thuộc chỉ số đó (mã, loại, phòng ban, trạng thái, ngày gửi), ở chế độ chỉ đọc.
- Rate limit: 60 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu `department_id` không tồn tại, hệ thống trả 400.
- Nếu truy vấn quá 10 giây, hệ thống trả 503. UI giữ số liệu lần trước kèm cảnh báo "Số liệu chưa được cập nhật".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hệ thống có 5 hồ sơ `NEW`, 8 `IN_PROGRESS`, 2 `PENDING_INFO`, 3 `OVERDUE`, 20 `CLOSED` -> Dashboard hiển thị Mới 5, Đang xử lý 10, Quá hạn 3.
- **AC-02 [Validation]:** Lọc theo một phòng ban -> chỉ đếm hồ sơ hiện đang thuộc phòng ban đó. `department_id` không tồn tại -> 400.
- **AC-03 [Boundary]:** Hệ thống chưa có hồ sơ nào -> cả 3 chỉ số hiển thị 0, không lỗi.
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API dashboard -> 403.
- **AC-05 [Concurrency/Rate Limit]:** Một hồ sơ chuyển sang `OVERDUE` trong lúc dashboard đang mở -> số Quá hạn tăng trong vòng 30 giây. Request thứ 61 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503; UI vẫn hiển thị số liệu lần trước kèm cảnh báo, không trắng trang.

**Ví dụ Edge Case**
Một hồ sơ vừa được chuyển từ phòng ban A sang B. Khi lọc theo phòng ban A, hồ sơ không còn được đếm. Khi lọc theo B, hồ sơ được đếm, vì chỉ số tính theo phòng ban hiện tại của hồ sơ.

**Expected Result:** Ba chỉ số phản ánh đúng số hồ sơ đang mở theo trạng thái tại thời điểm xem.

---

### [FR-MGT-02] Dashboard: thời gian xử lý trung bình

**Mô tả**
Hiển thị thời gian xử lý trung bình của các hồ sơ đã đóng, giúp Ban quản lý đánh giá tốc độ phục vụ của từng phòng ban.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở Dashboard hoặc chọn khoảng thời gian.
2. Hệ thống lấy các hồ sơ có thời điểm đóng nằm trong khoảng thời gian.
3. Hệ thống tính thời gian xử lý của từng hồ sơ rồi lấy trung bình.
4. Hệ thống hiển thị kết quả, kèm số hồ sơ được dùng để tính.

**Input**
- **Người dùng/request:** from/to, department_id, request_type_id tùy chọn.
- **Hệ thống:** MANAGER; ticket CLOSED trong kỳ closed_at, phòng lúc đóng, tổng pause PENDING_INFO.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: average_hours hoặc null, closed_count, updated_at; ≥48 giờ có average_days. Số ngày làm tròn một chữ số.
- **Thay đổi/sự kiện:** Chỉ đọc; polling 30 giây khi nằm trên dashboard đang mở.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Chỉ số trên dashboard polling 30 giây khi trang mở; hiển thị updated_at. Chỉ trừ khoảng PENDING_INFO; yêu cầu bổ sung trên ticket OVERDUE không tạo pause và thời gian chờ đó vẫn tính. Ticket đóng trong khoảng trễ cron vẫn tính thời lượng từ timestamp, không phụ thuộc job đã đổi status hay chưa.
- Thời gian xử lý của một hồ sơ = thời điểm đóng trừ thời điểm gửi, sau đó trừ tổng thời gian hồ sơ ở `PENDING_INFO` (khớp với cách tính SLA ở CORE-03).
- Chỉ tính hồ sơ `CLOSED` có thời điểm đóng nằm trong khoảng thời gian.
- Hiển thị theo giờ, làm tròn 1 chữ số thập phân. Nếu từ 48 giờ trở lên thì hiển thị thêm số ngày tương ứng.
- Bộ lọc: khoảng thời gian, phòng ban (phòng ban tại thời điểm đóng), loại yêu cầu.
- Rate limit: 60 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu `from` sau `to` hoặc khoảng thời gian dài hơn 365 ngày, hệ thống trả 400.
- Nếu không có hồ sơ nào đóng trong khoảng thời gian, hệ thống trả 200 với giá trị `null` và UI hiển thị "Chưa có dữ liệu".
- Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hai hồ sơ đóng trong kỳ, mất 10 giờ và 20 giờ -> hiển thị 15,0 giờ, dựa trên 2 hồ sơ.
- **AC-02 [Business Logic]:** Hồ sơ gửi lúc 08:00, chờ bổ sung từ 10:00 đến 14:00, đóng lúc 18:00 -> thời gian xử lý được tính là 6 giờ.
- **AC-03 [Validation/Boundary]:** Khoảng thời gian 366 ngày -> 400. Đúng 365 ngày -> 200. Không có hồ sơ đóng trong kỳ -> 200, giá trị `null`, UI hiển thị "Chưa có dữ liệu" (không hiển thị 0).
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403.
- **AC-05 [Rate Limit]:** Request thứ 61 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503 với thông báo thử lại, không làm treo dashboard.

**Ví dụ Edge Case**
Một hồ sơ được gửi trong tháng 9 nhưng đến tháng 10 mới đóng. Khi xem báo cáo tháng 10, hồ sơ được tính vào trung bình của tháng 10, vì kỳ báo cáo lọc theo thời điểm đóng.

**Expected Result:** Thời gian xử lý trung bình chỉ tính các hồ sơ đã đóng trong kỳ, và không tính thời gian chờ sinh viên bổ sung.

---

### [FR-MGT-03] Báo cáo loại yêu cầu phổ biến

**Mô tả**
Thống kê số hồ sơ theo từng loại yêu cầu, giúp Ban quản lý biết sinh viên thường gặp vấn đề gì để cải thiện quy trình.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở báo cáo Loại yêu cầu.
2. Ban quản lý chọn khoảng thời gian và phòng ban (nếu cần).
3. Hệ thống đếm số hồ sơ theo từng loại yêu cầu.
4. Hệ thống hiển thị bảng và biểu đồ cột, sắp xếp giảm dần theo số lượng.

**Input**
- **Người dùng/request:** from/to và department_id tùy chọn.
- **Hệ thống:** MANAGER; ticket created_at trong kỳ; phòng hiện tại; toàn bộ loại yêu cầu.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: rows gồm type_id/name, ticket_count, percentage hoặc null; total_count và updated_at.
- **Thay đổi/sự kiện:** Chỉ đọc; bảng và biểu đồ dùng cùng dữ liệu, không tự thêm nhóm vấn đề chưa có trong danh mục.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Phòng ban dùng giá trị hiện tại nên báo cáo kỳ cũ có thể đổi sau chuyển phòng. Cùng số lượng thì sắp mã loại tăng dần. Tỷ lệ làm tròn có thể không cộng đúng 100%; không chỉnh số liệu gốc để ép tổng.
- Đếm các hồ sơ có thời điểm gửi nằm trong khoảng thời gian, ở mọi trạng thái.
- Mỗi dòng gồm: tên loại yêu cầu, số hồ sơ, tỷ lệ % trên tổng (làm tròn 1 chữ số thập phân).
- Hiển thị tất cả loại yêu cầu trong danh mục, kể cả loại có 0 hồ sơ.
- Bộ lọc: khoảng thời gian, phòng ban (phòng ban hiện tại của hồ sơ).
- Tên loại yêu cầu hiển thị theo ngôn ngữ người dùng.
- Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu khoảng thời gian không hợp lệ, hệ thống trả 400.
- Nếu không có hồ sơ trong kỳ, hệ thống trả 200 với tất cả loại bằng 0 và tỷ lệ hiển thị `--`.
- Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Trong kỳ có 60 hồ sơ "Học phí" và 40 hồ sơ "Xác nhận sinh viên" (ví dụ) -> bảng hiển thị Học phí 60 (60,0%) ở trên, Xác nhận sinh viên 40 (40,0%) ở dưới.
- **AC-02 [Validation]:** `from` sau `to` -> 400.
- **AC-03 [Boundary]:** Không có hồ sơ trong kỳ -> mọi loại hiển thị 0, tỷ lệ `--`, không lỗi chia cho 0.
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403.
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.

**Ví dụ Edge Case**
Một hồ sơ loại "Học phí" bị chuyển sang phòng ban khác với phòng ban ánh xạ mặc định. Hồ sơ vẫn được đếm vào loại "Học phí", vì chuyển phòng ban không làm đổi loại yêu cầu (FR-STF-09).

**Expected Result:** Báo cáo cho thấy đúng số lượng và tỷ lệ hồ sơ của từng loại yêu cầu trong kỳ.

---

### [FR-MGT-04] Báo cáo khối lượng công việc theo phòng ban

**Mô tả**
Thống kê khối lượng hồ sơ của từng phòng ban, giúp Ban quản lý phát hiện phòng ban quá tải để điều phối nhân sự.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở báo cáo Khối lượng theo phòng ban.
2. Ban quản lý chọn khoảng thời gian.
3. Hệ thống tính các chỉ số cho từng phòng ban.
4. Hệ thống hiển thị bảng, sắp xếp giảm dần theo số hồ sơ đang mở.

**Input**
- **Người dùng/request:** from/to.
- **Hệ thống:** MANAGER; hồ sơ/phòng hiện tại, closed_at/phòng lúc đóng, staff active.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: rows theo phòng gồm open_count, overdue_count, created_in_period_current_department_count, closed_in_period_count, active_staff_count; updated_at.
- **Thay đổi/sự kiện:** Chỉ đọc; ảnh chụp hiện tại và số liệu theo kỳ được gắn nhãn riêng.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Chọn cách thống kê đơn giản theo created_at và phòng hiện tại; không đo số lượt chuyển đến. UI ghi rõ cột hiện tại và cột trong kỳ. Cùng open_count sắp mã phòng tăng dần.
- Mỗi phòng ban có 5 chỉ số:
  - Đang mở: số hồ sơ chưa `CLOSED` hiện thuộc phòng ban (ảnh chụp tại thời điểm hiện tại).
  - Quá hạn: số hồ sơ `OVERDUE` hiện thuộc phòng ban (ảnh chụp tại thời điểm hiện tại).
  - Hồ sơ tạo trong kỳ, hiện thuộc phòng ban: số hồ sơ có created_at trong kỳ và department_id hiện tại của phòng. Không gọi chỉ số này là lượt tiếp nhận/chuyển đến trong kỳ.
  - Đã đóng trong kỳ: số hồ sơ được đóng trong kỳ, tính cho phòng ban tại thời điểm đóng.
  - Số nhân viên: số tài khoản `STAFF` đang hoạt động của phòng ban.
- Hiển thị tất cả phòng ban, kể cả phòng ban có 0 hồ sơ.
- Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu khoảng thời gian không hợp lệ, hệ thống trả 400.
- Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Phòng A có 12 hồ sơ đang mở, trong đó 3 quá hạn; phòng B có 4 hồ sơ đang mở -> phòng A đứng trên phòng B, với số liệu đúng.
- **AC-02 [Validation]:** Khoảng thời gian dài 366 ngày -> 400.
- **AC-03 [Boundary]:** Phòng ban không có hồ sơ và không có nhân viên -> vẫn hiển thị, các chỉ số bằng 0.
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403.
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.

**Ví dụ Edge Case**
Một hồ sơ được phòng A tiếp nhận rồi chuyển sang phòng B, và phòng B đóng hồ sơ. Hồ sơ được tính vào "Đã đóng trong kỳ" của phòng B. Hồ sơ không bị đếm ở cả hai phòng ban.

**Expected Result:** Mỗi hồ sơ chỉ được tính cho đúng một phòng ban ở mỗi chỉ số, nên tổng các phòng ban luôn khớp với tổng toàn hệ thống.

---

### [FR-MGT-05] Báo cáo mức độ hài lòng của sinh viên

**Mô tả**
Thống kê điểm đánh giá và nhận xét của sinh viên, giúp Ban quản lý đánh giá chất lượng dịch vụ hỗ trợ.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở báo cáo Mức độ hài lòng.
2. Ban quản lý chọn khoảng thời gian, phòng ban, loại yêu cầu (nếu cần).
3. Hệ thống tính các chỉ số đánh giá.
4. Hệ thống hiển thị chỉ số tổng hợp và danh sách nhận xét.

**Input**
- **Người dùng/request:** from/to, department_id, request_type_id; danh sách nhận xét thêm rating 1–5, page=1, page_size=20 (1–100).
- **Hệ thống:** MANAGER; ticket CLOSED trong kỳ closed_at; rating hiện có; phòng lúc đóng.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: average_rating hoặc null, distribution_1_to_5, rating_count, closed_count, rating_percentage hoặc null; comments phân trang không có trường định danh sinh viên.
- **Thay đổi/sự kiện:** Chỉ đọc; lọc rating chỉ áp dụng danh sách nhận xét, không đổi các chỉ số tổng; nhận xét tự nhập không được cam kết ẩn danh tuyệt đối.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Lọc sao chỉ áp dụng danh sách nhận xét, không thay đổi chỉ số tổng. Không có hồ sơ CLOSED trong kỳ: rating_percentage=null hiển thị --; không có rating: average_rating=null, rating_count=0, phân bố sao đều 0.
- API không trả trường định danh sinh viên; nhận xét có thể tự chứa tên/MSSV do người dùng nhập, vì vậy không cam kết ẩn danh nội dung tuyệt đối. Không tự thêm bộ lọc/AI xóa nội dung.
- Chỉ số tổng hợp:
  - Điểm trung bình (làm tròn 2 chữ số thập phân).
  - Phân bố số lượt theo từng mức 1–5 sao.
  - Tổng số lượt đánh giá.
  - Tỷ lệ đánh giá = số hồ sơ được đánh giá / số hồ sơ đóng trong kỳ.
- Tính theo các hồ sơ có thời điểm đóng nằm trong khoảng thời gian. Phòng ban được tính theo phòng ban tại thời điểm đóng.
- Danh sách nhận xét: mã hồ sơ, loại yêu cầu, phòng ban, số sao, nhận xét, ngày đánh giá. Mới nhất trước, 20 dòng mỗi trang. Lọc được theo số sao.
- Không hiển thị tên và MSSV của sinh viên trong báo cáo.
- Nhận xét hiển thị dạng văn bản đã escape.
- Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu khoảng thời gian hoặc bộ lọc số sao không hợp lệ, hệ thống trả 400.
- Nếu không có lượt đánh giá nào, hệ thống trả 200; điểm trung bình là `null` và UI hiển thị "Chưa có đánh giá".
- Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Trong kỳ có 10 hồ sơ đóng, 4 hồ sơ được đánh giá 5, 4, 4, 3 sao -> điểm trung bình 4,00; tổng 4 lượt; tỷ lệ đánh giá 40%.
- **AC-02 [Validation]:** Lọc theo số sao = 6 -> 400.
- **AC-03 [Boundary]:** Không có đánh giá trong kỳ -> điểm trung bình hiển thị "Chưa có đánh giá" (không hiển thị 0), không lỗi chia cho 0.
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. Response không chứa tên hoặc MSSV của sinh viên. Nhận xét chứa `<script>` -> hiển thị dạng văn bản, không thực thi.
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429.
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.

**Ví dụ Edge Case**
Một hồ sơ được đóng ngày 30/9 nhưng đến ngày 05/10 sinh viên mới đánh giá. Đánh giá được tính vào báo cáo tháng 9, vì báo cáo lọc theo thời điểm đóng. Vì vậy báo cáo tháng 9 có thể thay đổi nhẹ khi xem lại sau đó.

**Expected Result:** Báo cáo phản ánh đúng mức độ hài lòng của các hồ sơ đã đóng trong kỳ, và không trả các trường định danh sinh viên trong dữ liệu báo cáo.

---

### [FR-MGT-06] Quản lý phân quyền

**Mô tả**
Ban quản lý gán vai trò và phòng ban cho từng tài khoản, để hệ thống biết ai là nhân viên phòng nào và ai thuộc Ban quản lý.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.

**Luồng chính**
1. Ban quản lý mở màn hình Phân quyền.
2. Hệ thống hiển thị STAFF/MANAGER và ô tìm email trường chính xác để cấp quyền cho tài khoản STUDENT đã có; không liệt kê hàng loạt sinh viên.
3. Ban quản lý thực hiện một trong các thao tác: thêm định danh chưa có, cấp quyền cho tài khoản có sẵn tìm theo email, đổi vai trò/phòng ban, vô hiệu hóa hoặc kích hoạt lại tài khoản.
4. Hệ thống kiểm tra quy tắc rồi lưu.
5. Thay đổi có hiệu lực ngay từ request kế tiếp của tài khoản đó.

**Input**
- **Người dùng/request:** List STAFF/MANAGER; tìm cấp quyền: email trường chính xác; thêm/cấp quyền: email, role, department_id nếu STAFF; sửa/kích hoạt: account_id, version, giá trị mới.
- **Hệ thống:** MANAGER active; tài khoản SSO đã có hoặc danh sách cấp quyền trước; ràng buộc người nhận hồ sơ.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200 list/search: id, email, tên nếu có, role, department, active, version; tìm STUDENT chỉ bằng email chính xác. Thêm mới 201, sửa/cấp quyền tài khoản có sẵn 200 với version mới.
- **Thay đổi/sự kiện:** Cấp/đổi quyền/phòng/active; tác dụng request kế tiếp; audit ROLE_CHANGE kèm subtype. Không trả MSSV/khoa/lớp/SĐT hoặc hồ sơ sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- MANAGER đầu được nạp khi triển khai từ định danh do trường xác nhận (SEC-01); không tạo tài khoản quản trị bằng email ví dụ.
- Tìm chính xác email đã chuẩn hóa có thể trả tài khoản STUDENT với id/email/tên/role/active/version tối thiểu để cấp quyền. Không trả MSSV, khoa/lớp, SĐT hoặc hồ sơ của sinh viên. Tài khoản có sẵn được cập nhật theo account_id/version, không tạo bản sao.
- Chuẩn hóa định danh theo hợp đồng SSO được xác nhận; trim email, so sánh miền không phân biệt hoa/thường, quy tắc phần local theo nhà cung cấp. Không tự gộp hai tài khoản khi chưa xác nhận định danh.
- Việc kiểm tra hồ sơ mở và cập nhật quyền/phòng/active phải nguyên tử với phân công (STF-07). Version tài khoản riêng không thay thế ràng buộc này. ROLE_CHANGE có subtype ACCOUNT_ADD/ROLE_SET/DEPARTMENT_SET/ACTIVATE/DEACTIVATE.
- **Thêm tài khoản:** Ban quản lý nhập định danh SSO (email trường) của người dùng, chọn vai trò `STAFF` hoặc `MANAGER`, và chọn phòng ban nếu là `STAFF`. Người dùng đăng nhập SSO lần đầu mà không có trong danh sách này sẽ mặc định là `STUDENT`.
- **Quy tắc vai trò và phòng ban:** `STAFF` phải có đúng một phòng ban. `MANAGER` và `STUDENT` không có phòng ban.
- **Tài khoản bị vô hiệu hóa:** không đăng nhập được và không được phân công hồ sơ.
- **Đổi phòng ban, đổi vai trò hoặc vô hiệu hóa một `STAFF`:** chỉ được làm khi nhân viên đó không còn phụ trách hồ sơ nào chưa đóng.
- **Chống khóa hệ thống:** Không được tự hạ vai trò hoặc tự vô hiệu hóa chính mình. Hệ thống luôn phải còn ít nhất một `MANAGER` đang hoạt động.
- Định danh SSO là duy nhất.
- Bắt buộc gửi `version` của tài khoản khi sửa.
- Rate limit: 30 thao tác ghi mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu định danh SSO sai định dạng, hoặc vai trò `STAFF` thiếu phòng ban, hệ thống trả 400.
- Nếu thao tác tạo mới dùng định danh đã tồn tại, trả 409 và hướng dẫn dùng chức năng tìm/cấp quyền tài khoản có sẵn; thao tác cấp quyền đúng account_id/version trả 200.
- Nếu nhân viên còn phụ trách hồ sơ chưa đóng, hệ thống trả 409 kèm danh sách mã hồ sơ cần phân công lại.
- Nếu thao tác khiến hệ thống không còn `MANAGER` nào đang hoạt động, hoặc người dùng tự hạ quyền chính mình, hệ thống trả 409.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Thêm tài khoản `nva@aurora.edu.vn` (ví dụ) với vai trò `STAFF` thuộc Phòng Đào tạo -> 201. Khi người này đăng nhập SSO, họ vào Cổng Nhân viên và thấy hàng đợi của Phòng Đào tạo.
- **AC-02 [Validation]:** Tạo tài khoản `STAFF` không có phòng ban -> 400. Thêm mới định danh đã có -> 409; tìm email STUDENT có sẵn rồi cấp STAFF/phòng bằng account_id/version -> 200, không tạo duplicate account.
- **AC-03 [Business Logic]:** Đổi phòng ban của nhân viên đang phụ trách 2 hồ sơ chưa đóng -> 409 kèm 2 mã hồ sơ. Sau khi 2 hồ sơ được phân công lại cho người khác, đổi phòng ban -> 200.
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API phân quyền -> 403. Ban quản lý tự hạ vai trò của chính mình -> 409. Nhân viên bị hạ về `STUDENT` -> request kế tiếp vào Cổng Nhân viên bị 403.
- **AC-05 [Concurrency]:** Hai người trong Ban quản lý cùng sửa một tài khoản -> người lưu sau nhận 409. Hai người cạnh tranh thao tác khiến không còn MANAGER active -> chỉ thao tác giữ invariant được commit; tự deactivate/hạ quyền luôn 409. Phân công staff cạnh tranh deactivate/đổi phòng không để assignee ticket mở mất quyền.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, phân quyền giữ nguyên. Thành công -> có bản ghi nhật ký `ROLE_CHANGE` (người thực hiện, tài khoản bị đổi, giá trị cũ, giá trị mới).

**Ví dụ Edge Case**
Một nhân viên chuyển công tác từ Phòng Đào tạo sang Phòng Tài chính nhưng còn 3 hồ sơ đang xử lý. Hệ thống từ chối đổi phòng ban và liệt kê 3 mã hồ sơ đó. Đồng nghiệp ở Phòng Đào tạo phân công lại 3 hồ sơ (FR-STF-07), sau đó Ban quản lý mới đổi phòng ban được.

**Expected Result:** Mỗi tài khoản có đúng vai trò và phòng ban hợp lệ, không có hồ sơ nào bị bỏ lại với người phụ trách không còn quyền, và hệ thống luôn còn người quản trị.

---

### [FR-MGT-07] Thiết lập SLA cho từng loại yêu cầu

**Mô tả**
Ban quản lý đặt thời hạn xử lý (SLA) cho từng loại yêu cầu, làm căn cứ để hệ thống xác định hồ sơ quá hạn.

**Actor**
Ban quản lý đã đăng nhập.

**Preconditions**
- Người dùng có vai trò `MANAGER`.
- Danh mục loại yêu cầu đã được cấu hình (CORE-05).

**Luồng chính**
1. Ban quản lý mở màn hình Thiết lập SLA.
2. Hệ thống hiển thị danh sách loại yêu cầu kèm SLA hiện tại.
3. Ban quản lý sửa SLA của một loại yêu cầu và lưu.
4. Hệ thống áp dụng SLA mới cho các hồ sơ tạo từ thời điểm này trở đi.

**Input**
- **Người dùng/request:** List loại yêu cầu/SLA; cập nhật: request_type_id, version cấu hình, sla_hours số nguyên 1–720.
- **Hệ thống:** MANAGER, cấu hình SLA hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: request_type_id/name, sla_hours, version; thay đổi trả version mới, no-op giữ version.
- **Thay đổi/sự kiện:** Chỉ ảnh hưởng ticket tạo sau điểm commit cấu hình; audit SLA_CHANGE; không ghi đè SLA đã sửa khi seed lại.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- List và response lưu luôn có version cấu hình. Đọc SLA để tạo hồ sơ và cập nhật SLA có điểm hiệu lực xác định theo transaction: mỗi hồ sơ lưu đúng một SLA snapshot đã commit, không trộn hai giá trị. Seed danh mục không ghi đè SLA manager đã đổi.
- SLA tính bằng giờ (giờ lịch, tính cả ngoài giờ hành chính và ngày nghỉ). Là số nguyên từ 1 đến 720.
- Mỗi loại yêu cầu có đúng một giá trị SLA. Giá trị ban đầu được nạp sẵn khi triển khai.
- Hạn xử lý của mỗi hồ sơ được chốt tại thời điểm tạo hồ sơ. Đổi SLA không làm thay đổi hạn của hồ sơ đã tạo.
- Không thêm hoặc xóa loại yêu cầu ở màn hình này (danh mục cố định theo CORE-05).
- Bắt buộc gửi `version`. Lưu cùng giá trị hiện tại trả 200 và không ghi nhật ký.
- Rate limit: 30 thao tác ghi mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- Nếu SLA không phải số nguyên, nhỏ hơn 1 hoặc lớn hơn 720, hệ thống trả 400.
- Nếu loại yêu cầu không tồn tại, hệ thống trả 404.
- Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Đổi SLA của loại "Học phí" từ 72 giờ sang 48 giờ -> 200. Hồ sơ "Học phí" tạo sau đó có hạn bằng thời điểm gửi cộng 48 giờ.
- **AC-02 [Validation/Boundary]:** SLA = 0, 721 hoặc 2,5 -> 400. SLA = 1 và 720 -> 200.
- **AC-03 [Business Logic]:** Hồ sơ tạo trước khi đổi SLA -> giữ nguyên hạn cũ (72 giờ).
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403.
- **AC-05 [Concurrency/Idempotency]:** Hai người trong Ban quản lý cùng sửa SLA một loại -> người lưu sau nhận 409. Lưu lại cùng giá trị với version hiện tại -> 200, không thêm audit/tăng version. Seed lại không ghi đè SLA đã sửa; tạo ticket cạnh tranh đổi SLA lưu đúng một snapshot đã commit.
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, SLA giữ nguyên. Thành công -> có bản ghi nhật ký `SLA_CHANGE` (loại yêu cầu, giá trị cũ, giá trị mới, người thực hiện).

**Ví dụ Edge Case**
Ban quản lý rút SLA của một loại yêu cầu từ 72 giờ xuống 24 giờ. Một hồ sơ cùng loại đã được gửi 30 giờ trước đó. Hồ sơ này không bị chuyển ngay sang `OVERDUE`, vì hạn của nó đã được chốt là 72 giờ lúc tạo.

**Expected Result:** Mỗi loại yêu cầu có đúng một SLA hợp lệ, thay đổi SLA chỉ ảnh hưởng hồ sơ mới, và mọi thay đổi đều được ghi lại.
