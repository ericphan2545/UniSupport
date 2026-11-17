# M03. Cổng Admin – Functional Requirement Specification

> Đặc tả thuộc bộ 42 chức năng của 5 module, chốt cùng khách ở `MS-01` (`A-13`); sau khi chốt, thay đổi đi qua change request ([03-quan-tri-giam-sat.md](../../management/03-quan-tri-giam-sat.md) mục 5). Lịch sử sửa xem trong các PR. Điểm còn chờ trường xác nhận có một nơi gốc là [assumptions.md](../../project/assumptions.md) (mã `A-xx`, cột Trạng thái); phần đầu module chỉ dẫn lại mã.

**Mục đích module:** Báo cáo toàn hệ thống, quản lý quyền và SLA.

**Actor chính:** ADMIN. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M04 xác thực/phạm vi; dữ liệu hồ sơ, đánh giá và danh mục từ M01, M02, M05.

## Luồng quản lý và liên kết module

Admin đăng nhập M04 → dashboard số lượng hiện tại/trung bình CLOSED → danh sách tóm tắt nếu chọn chỉ số → báo cáo loại/khối lượng/hài lòng từ dữ liệu M01/M02 → cấp quyền tài khoản và thiết lập SLA. Admin không xử lý ticket hoặc xem chi tiết/tệp sinh viên.

**Input module:** phiên ADMIN, bộ lọc kỳ/phòng/loại/sao, định danh/quyền/phòng/active và SLA/version cấu hình.
**Output module:** số liệu/tables/charts cùng dữ liệu, danh sách tóm tắt không thông tin cá nhân, quyền và SLA đã cập nhật.

**Kỳ báo cáo:** count dashboard dùng hiện tại; loại phổ biến dùng created_at/phòng hiện tại; trung bình/hài lòng dùng closed_at/phòng lúc đóng. Cột khối lượng gọi đúng “Hồ sơ tạo trong kỳ, hiện thuộc phòng ban”, không gọi lượt tiếp nhận thực tế.

**Điểm chờ xác nhận:** tên chỉ số, nhãn báo cáo, giá trị lọc mặc định và ngưỡng trong đặc tả chốt cùng bộ đặc tả ở `MS-01` (`A-13`); danh mục, SLA ban đầu và tài khoản Admin đầu tiên (`A-10`); định danh SSO (`A-05`). Trạng thái theo dõi ở [assumptions.md](../../project/assumptions.md).

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Xác thực:** Mọi API yêu cầu phiên ứng dụng hợp lệ theo SEC-01 và vai trò ADMIN. Sai hoặc thiếu token trả 401. Sai vai trò trả 403.
- **Kiểm tra quyền:** Vai trò và phòng ban được đọc từ cơ sở dữ liệu ở mỗi request, không đọc từ claim trong JWT. Nhờ vậy thay đổi phân quyền (FR-MGT-06) có hiệu lực ngay ở request kế tiếp.
- **Phạm vi dữ liệu:** Admin xem số liệu của toàn bộ phòng ban.
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
Hiển thị số lượng hồ sơ theo trạng thái tại thời điểm hiện tại, giúp Admin biết ngay khối lượng đang chờ và số hồ sơ đang trễ hạn.

**Actor**
Admin đã đăng nhập.

**Priority**
`Must`: Số hồ sơ mới, đang xử lý, quá hạn là thông tin tối thiểu để ban quản lý điều phối (G-03).

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở Dashboard.
2. Hệ thống đếm số hồ sơ theo từng nhóm trạng thái tại thời điểm hiện tại.
3. Hệ thống hiển thị 3 chỉ số: Mới, Đang xử lý, Quá hạn.
4. Admin lọc theo phòng ban nếu cần.
5. Dashboard tự cập nhật định kỳ.

**Input**
- **Người dùng/request:** department_id tùy chọn; drill-down thêm group=new/in_progress/overdue, page=1, page_size=20 (1–100).
- **Hệ thống:** ADMIN active; trạng thái hiện tại theo phòng hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: new_count, in_progress_count, overdue_count, updated_at; drill-down có items/total/page và chỉ code, loại, phòng, trạng thái, ngày gửi.
- **Thay đổi/sự kiện:** Chỉ đọc; polling cả số liệu và danh sách mở mỗi 30 giây.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Drill-down có page/page_size, total_items/total_pages, sắp created_at giảm dần rồi ticket_id giảm dần. Bất kỳ filter/giá trị group không hợp lệ trả 400. Danh sách chỉ tóm tắt, không có liên kết thao tác xử lý/chi tiết staff.
- **BR-02:** Mới = số hồ sơ `NEW`.
- **BR-03:** Đang xử lý = số hồ sơ `IN_PROGRESS` cộng `PENDING_INFO`.
- **BR-04:** Quá hạn = số hồ sơ `OVERDUE`.
- **BR-05:** Hồ sơ `CLOSED` không được tính.
- **BR-06:** Số liệu là ảnh chụp tại thời điểm hiện tại, không phụ thuộc khoảng thời gian.
- **BR-07:** Bộ lọc: phòng ban (mặc định là tất cả).
- **BR-08:** Tự cập nhật bằng polling mỗi 30 giây. Hiển thị thời điểm cập nhật gần nhất.
- **BR-09:** Bấm vào một chỉ số thì mở danh sách các hồ sơ thuộc chỉ số đó (mã, loại, phòng ban, trạng thái, ngày gửi), ở chế độ chỉ đọc.
- **BR-10:** Rate limit: 60 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu `department_id` không tồn tại, hệ thống trả 400.
- **E-02:** Nếu truy vấn quá 10 giây, hệ thống trả 503. UI giữ số liệu lần trước kèm cảnh báo "Số liệu chưa được cập nhật".

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hệ thống có 5 hồ sơ `NEW`, 8 `IN_PROGRESS`, 2 `PENDING_INFO`, 3 `OVERDUE`, 20 `CLOSED` -> Dashboard hiển thị Mới 5, Đang xử lý 10, Quá hạn 3. _(Kiểm cho: luồng chính, BR-02, BR-03, BR-04, BR-05, BR-07)_
- **AC-02 [Validation]:** Lọc theo một phòng ban -> chỉ đếm hồ sơ hiện đang thuộc phòng ban đó. `department_id` không tồn tại -> 400. _(Kiểm cho: BR-07, E-01)_
- **AC-03 [Boundary]:** Hệ thống chưa có hồ sơ nào -> cả 3 chỉ số hiển thị 0, không lỗi. _(Kiểm cho: BR-02, BR-03, BR-04)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API dashboard -> 403. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Concurrency/Rate Limit]:** Một hồ sơ chuyển sang `OVERDUE` trong lúc dashboard đang mở -> số Quá hạn tăng trong vòng 30 giây. Request thứ 61 trong 1 phút -> 429. _(Kiểm cho: BR-08, BR-10)_
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503; UI vẫn hiển thị số liệu lần trước kèm cảnh báo, không trắng trang. _(Kiểm cho: E-02)_
- **AC-07 [Business Logic]:** Bấm chỉ số "Quá hạn" (3 hồ sơ) -> danh sách 3 hồ sơ có mã, loại, phòng ban, trạng thái, ngày gửi; sắp `created_at` giảm dần, cùng `created_at` thì `ticket_id` giảm dần; `total_items` = 3, `total_pages` = 1; không có nút xử lý hay liên kết sang chi tiết hồ sơ. Giá trị nhóm chỉ số không hợp lệ -> 400. _(Kiểm cho: BR-01, BR-09)_
- **AC-08 [Boundary]:** Hồ sơ `NEW` tạo từ 400 ngày trước vẫn được đếm vào "Mới", vì số liệu là ảnh chụp hiện tại, không lọc theo thời gian. Dashboard hiển thị thời điểm cập nhật gần nhất, đổi sau mỗi lần polling. _(Kiểm cho: BR-06, BR-08)_

**Ví dụ Edge Case**
Một hồ sơ vừa được chuyển từ phòng ban A sang B. Khi lọc theo phòng ban A, hồ sơ không còn được đếm. Khi lọc theo B, hồ sơ được đếm, vì chỉ số tính theo phòng ban hiện tại của hồ sơ.

**Expected Result:** Ba chỉ số phản ánh đúng số hồ sơ đang mở theo trạng thái tại thời điểm xem.

---

### [FR-MGT-02] Dashboard: thời gian xử lý trung bình

**Mô tả**
Hiển thị thời gian xử lý trung bình của các hồ sơ đã đóng, giúp Admin đánh giá tốc độ phục vụ của từng phòng ban.

**Actor**
Admin đã đăng nhập.

**Priority**
`Should`: Bổ sung cho dashboard; ban quản lý vẫn điều phối được bằng FR-MGT-01 nếu làm muộn hơn.

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở Dashboard hoặc chọn khoảng thời gian.
2. Hệ thống lấy các hồ sơ có thời điểm đóng nằm trong khoảng thời gian.
3. Hệ thống tính thời gian xử lý của từng hồ sơ rồi lấy trung bình.
4. Hệ thống hiển thị kết quả, kèm số hồ sơ được dùng để tính.

**Input**
- **Người dùng/request:** from/to, department_id, request_type_id tùy chọn.
- **Hệ thống:** ADMIN; ticket CLOSED trong kỳ closed_at, phòng lúc đóng, tổng pause PENDING_INFO.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: average_hours hoặc null, closed_count, updated_at; ≥48 giờ có average_days. Số ngày làm tròn một chữ số.
- **Thay đổi/sự kiện:** Chỉ đọc; polling 30 giây khi nằm trên dashboard đang mở.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Chỉ số trên dashboard polling 30 giây khi trang mở; hiển thị updated_at. Chỉ trừ khoảng PENDING_INFO; yêu cầu bổ sung trên ticket OVERDUE không tạo pause và thời gian chờ đó vẫn tính. Ticket đóng trong khoảng trễ cron vẫn tính thời lượng từ timestamp, không phụ thuộc job đã đổi status hay chưa.
- **BR-02:** Thời gian xử lý của một hồ sơ = thời điểm đóng trừ thời điểm gửi, sau đó trừ tổng thời gian hồ sơ ở `PENDING_INFO` (khớp với cách tính SLA ở CORE-03).
- **BR-03:** Chỉ tính hồ sơ `CLOSED` có thời điểm đóng nằm trong khoảng thời gian.
- **BR-04:** Hiển thị theo giờ, làm tròn 1 chữ số thập phân. Nếu từ 48 giờ trở lên thì hiển thị thêm số ngày tương ứng.
- **BR-05:** Bộ lọc: khoảng thời gian, phòng ban (phòng ban tại thời điểm đóng), loại yêu cầu.
- **BR-06:** Rate limit: 60 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu `from` sau `to` hoặc khoảng thời gian dài hơn 365 ngày, hệ thống trả 400.
- **E-02:** Nếu không có hồ sơ nào đóng trong khoảng thời gian, hệ thống trả 200 với giá trị `null` và UI hiển thị "Chưa có dữ liệu".
- **E-03:** Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hai hồ sơ đóng trong kỳ, mất 10 giờ và 20 giờ -> hiển thị 15,0 giờ, dựa trên 2 hồ sơ. _(Kiểm cho: luồng chính, BR-03, BR-04)_
- **AC-02 [Business Logic]:** Hồ sơ gửi lúc 08:00, chờ bổ sung từ 10:00 đến 14:00, đóng lúc 18:00 -> thời gian xử lý được tính là 6 giờ. _(Kiểm cho: BR-02)_
- **AC-03 [Validation/Boundary]:** Khoảng thời gian 366 ngày -> 400. Đúng 365 ngày -> 200. Không có hồ sơ đóng trong kỳ -> 200, giá trị `null`, UI hiển thị "Chưa có dữ liệu" (không hiển thị 0). _(Kiểm cho: E-01, E-02)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Rate Limit]:** Request thứ 61 trong 1 phút -> 429. _(Kiểm cho: BR-06)_
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503 với thông báo thử lại, không làm treo dashboard. _(Kiểm cho: E-03)_
- **AC-07 [Business Logic]:** Hồ sơ `OVERDUE` có yêu cầu bổ sung từ 10:00 đến 14:00 -> 4 giờ chờ này vẫn được tính vào thời gian xử lý. Hồ sơ đóng khi đã qua `due_at` nhưng tác vụ quá hạn chưa chạy -> thời gian xử lý tính từ `created_at` và `closed_at` như hồ sơ khác. Hồ sơ đóng ngoài khoảng thời gian -> không được tính. _(Kiểm cho: BR-01, BR-03)_
- **AC-08 [Validation/Boundary]:** Trung bình 50,26 giờ -> hiển thị 50,3 giờ kèm số ngày tương ứng. `from` sau `to` -> 400. Hồ sơ chuyển từ phòng A sang B rồi đóng ở B -> lọc phòng B có tính hồ sơ này, lọc phòng A không tính. Lọc theo loại yêu cầu -> chỉ tính hồ sơ thuộc loại đó. Dashboard hiển thị thời điểm cập nhật gần nhất. _(Kiểm cho: BR-01, BR-04, BR-05, E-01)_

**Ví dụ Edge Case**
Một hồ sơ được gửi trong tháng 9 nhưng đến tháng 10 mới đóng. Khi xem báo cáo tháng 10, hồ sơ được tính vào trung bình của tháng 10, vì kỳ báo cáo lọc theo thời điểm đóng.

**Expected Result:** Thời gian xử lý trung bình chỉ tính các hồ sơ đã đóng trong kỳ, và không tính thời gian chờ sinh viên bổ sung.

---

### [FR-MGT-03] Báo cáo loại yêu cầu phổ biến

**Mô tả**
Thống kê số hồ sơ theo từng loại yêu cầu, giúp Admin biết sinh viên thường gặp vấn đề gì để cải thiện quy trình.

**Actor**
Admin đã đăng nhập.

**Priority**
`Should`: Báo cáo phân tích trong hợp đồng; không chặn luồng xử lý.

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở báo cáo Loại yêu cầu.
2. Admin chọn khoảng thời gian và phòng ban (nếu cần).
3. Hệ thống đếm số hồ sơ theo từng loại yêu cầu.
4. Hệ thống hiển thị bảng và biểu đồ cột, sắp xếp giảm dần theo số lượng.

**Input**
- **Người dùng/request:** from/to và department_id tùy chọn.
- **Hệ thống:** ADMIN; ticket created_at trong kỳ; phòng hiện tại; toàn bộ loại yêu cầu.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: rows gồm type_id/name, ticket_count, percentage hoặc null; total_count và updated_at.
- **Thay đổi/sự kiện:** Chỉ đọc; bảng và biểu đồ dùng cùng dữ liệu, không tự thêm nhóm vấn đề chưa có trong danh mục.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Phòng ban dùng giá trị hiện tại nên báo cáo kỳ cũ có thể đổi sau chuyển phòng. Cùng số lượng thì sắp mã loại tăng dần. Tỷ lệ làm tròn có thể không cộng đúng 100%; không chỉnh số liệu gốc để ép tổng.
- **BR-02:** Đếm các hồ sơ có thời điểm gửi nằm trong khoảng thời gian, ở mọi trạng thái.
- **BR-03:** Mỗi dòng gồm: tên loại yêu cầu, số hồ sơ, tỷ lệ % trên tổng (làm tròn 1 chữ số thập phân).
- **BR-04:** Hiển thị tất cả loại yêu cầu trong danh mục, kể cả loại có 0 hồ sơ.
- **BR-05:** Bộ lọc: khoảng thời gian, phòng ban (phòng ban hiện tại của hồ sơ).
- **BR-06:** Tên loại yêu cầu hiển thị theo ngôn ngữ người dùng.
- **BR-07:** Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu khoảng thời gian không hợp lệ, hệ thống trả 400.
- **E-02:** Nếu không có hồ sơ trong kỳ, hệ thống trả 200 với tất cả loại bằng 0 và tỷ lệ hiển thị `--`.
- **E-03:** Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Trong kỳ có 60 hồ sơ "Học phí" và 40 hồ sơ "Xác nhận sinh viên" (ví dụ) -> bảng hiển thị Học phí 60 (60,0%) ở trên, Xác nhận sinh viên 40 (40,0%) ở dưới. _(Kiểm cho: luồng chính, BR-02, BR-03)_
- **AC-02 [Validation]:** `from` sau `to` -> 400. _(Kiểm cho: E-01)_
- **AC-03 [Boundary]:** Không có hồ sơ trong kỳ -> mọi loại hiển thị 0, tỷ lệ `--`, không lỗi chia cho 0. _(Kiểm cho: BR-04, E-02)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-07)_
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại. _(Kiểm cho: E-03)_
- **AC-07 [Business Logic]:** Hai loại cùng 30 hồ sơ -> loại có mã nhỏ hơn đứng trước. Ba loại, mỗi loại 1 hồ sơ -> mỗi loại 33,3%; không chỉnh số liệu để ép tổng thành 100%. Hồ sơ ở mọi trạng thái, kể cả `CLOSED`, đều được đếm theo ngày gửi. _(Kiểm cho: BR-01, BR-02)_
- **AC-08 [Validation]:** Lọc theo phòng B -> đếm hồ sơ hiện thuộc phòng B, kể cả hồ sơ được chuyển từ phòng A sang. Người dùng chọn EN -> tên loại yêu cầu hiển thị bằng tiếng Anh. _(Kiểm cho: BR-01, BR-05, BR-06)_

**Ví dụ Edge Case**
Một hồ sơ loại "Học phí" bị chuyển sang phòng ban khác với phòng ban ánh xạ mặc định. Hồ sơ vẫn được đếm vào loại "Học phí", vì chuyển phòng ban không làm đổi loại yêu cầu (FR-STF-09).

**Expected Result:** Báo cáo cho thấy đúng số lượng và tỷ lệ hồ sơ của từng loại yêu cầu trong kỳ.

---

### [FR-MGT-04] Báo cáo khối lượng công việc theo phòng ban

**Mô tả**
Thống kê khối lượng hồ sơ của từng phòng ban, giúp Admin phát hiện phòng ban quá tải để điều phối nhân sự.

**Actor**
Admin đã đăng nhập.

**Priority**
`Should`: Báo cáo phân tích trong hợp đồng; không chặn luồng xử lý.

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở báo cáo Khối lượng theo phòng ban.
2. Admin chọn khoảng thời gian.
3. Hệ thống tính các chỉ số cho từng phòng ban.
4. Hệ thống hiển thị bảng, sắp xếp giảm dần theo số hồ sơ đang mở.

**Input**
- **Người dùng/request:** from/to.
- **Hệ thống:** ADMIN; hồ sơ/phòng hiện tại, closed_at/phòng lúc đóng, staff active.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: rows theo phòng gồm open_count, overdue_count, created_in_period_current_department_count, closed_in_period_count, active_staff_count; updated_at.
- **Thay đổi/sự kiện:** Chỉ đọc; ảnh chụp hiện tại và số liệu theo kỳ được gắn nhãn riêng.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Chọn cách thống kê đơn giản theo created_at và phòng hiện tại; không đo số lượt chuyển đến. UI ghi rõ cột hiện tại và cột trong kỳ. Cùng open_count sắp mã phòng tăng dần.
- **BR-02:** Mỗi phòng ban có 5 chỉ số:
  - Đang mở: số hồ sơ chưa `CLOSED` hiện thuộc phòng ban (ảnh chụp tại thời điểm hiện tại).
  - Quá hạn: số hồ sơ `OVERDUE` hiện thuộc phòng ban (ảnh chụp tại thời điểm hiện tại).
  - Hồ sơ tạo trong kỳ, hiện thuộc phòng ban: số hồ sơ có created_at trong kỳ và department_id hiện tại của phòng. Không gọi chỉ số này là lượt tiếp nhận/chuyển đến trong kỳ.
  - Đã đóng trong kỳ: số hồ sơ được đóng trong kỳ, tính cho phòng ban tại thời điểm đóng.
  - Số nhân viên: số tài khoản `STAFF` đang hoạt động của phòng ban.
- **BR-03:** Hiển thị tất cả phòng ban, kể cả phòng ban có 0 hồ sơ.
- **BR-04:** Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu khoảng thời gian không hợp lệ, hệ thống trả 400.
- **E-02:** Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Phòng A có 12 hồ sơ đang mở, trong đó 3 quá hạn; phòng B có 4 hồ sơ đang mở -> phòng A đứng trên phòng B, với số liệu đúng. _(Kiểm cho: luồng chính, BR-02)_
- **AC-02 [Validation]:** Khoảng thời gian dài 366 ngày -> 400. _(Kiểm cho: E-01)_
- **AC-03 [Boundary]:** Phòng ban không có hồ sơ và không có nhân viên -> vẫn hiển thị, các chỉ số bằng 0. _(Kiểm cho: BR-03)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-04)_
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại. _(Kiểm cho: E-02)_
- **AC-07 [Business Logic]:** Trong kỳ, phòng A có 5 hồ sơ tạo trong kỳ hiện thuộc A, 4 hồ sơ được đóng khi đang ở A, 3 tài khoản `STAFF` đang hoạt động và 1 tài khoản đã vô hiệu hóa -> bảng hiển thị đúng 5, 4 và số nhân viên 3. Cột hiển thị đúng tên "Hồ sơ tạo trong kỳ, hiện thuộc phòng ban". Hai phòng có cùng số hồ sơ đang mở -> phòng có mã nhỏ hơn đứng trước. _(Kiểm cho: BR-01, BR-02)_

**Ví dụ Edge Case**
Một hồ sơ được phòng A tiếp nhận rồi chuyển sang phòng B, và phòng B đóng hồ sơ. Hồ sơ được tính vào "Đã đóng trong kỳ" của phòng B. Hồ sơ không bị đếm ở cả hai phòng ban.

**Expected Result:** Mỗi hồ sơ chỉ được tính cho đúng một phòng ban ở mỗi chỉ số, nên tổng các phòng ban luôn khớp với tổng toàn hệ thống.

---

### [FR-MGT-05] Báo cáo mức độ hài lòng của sinh viên

**Mô tả**
Thống kê điểm đánh giá và nhận xét của sinh viên, giúp Admin đánh giá chất lượng dịch vụ hỗ trợ.

**Actor**
Admin đã đăng nhập.

**Priority**
`Should`: Báo cáo phân tích trong hợp đồng; cần dữ liệu từ FR-STU-10.

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở báo cáo Mức độ hài lòng.
2. Admin chọn khoảng thời gian, phòng ban, loại yêu cầu (nếu cần).
3. Hệ thống tính các chỉ số đánh giá.
4. Hệ thống hiển thị chỉ số tổng hợp và danh sách nhận xét.

**Input**
- **Người dùng/request:** from/to, department_id, request_type_id; danh sách nhận xét thêm rating 1–5, page=1, page_size=20 (1–100).
- **Hệ thống:** ADMIN; ticket CLOSED trong kỳ closed_at; rating hiện có; phòng lúc đóng.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: average_rating hoặc null, distribution_1_to_5, rating_count, closed_count, rating_percentage hoặc null; comments phân trang không có trường định danh sinh viên.
- **Thay đổi/sự kiện:** Chỉ đọc; lọc rating chỉ áp dụng danh sách nhận xét, không đổi các chỉ số tổng; nhận xét tự nhập không được cam kết ẩn danh tuyệt đối.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** Lọc sao chỉ áp dụng danh sách nhận xét, không thay đổi chỉ số tổng. Không có hồ sơ CLOSED trong kỳ: rating_percentage=null hiển thị --; không có rating: average_rating=null, rating_count=0, phân bố sao đều 0.
- **BR-02:** API không trả trường định danh sinh viên; nhận xét có thể tự chứa tên/MSSV do người dùng nhập, vì vậy không cam kết ẩn danh nội dung tuyệt đối. Không tự thêm bộ lọc/AI xóa nội dung.
- **BR-03:** Chỉ số tổng hợp:
  - Điểm trung bình (làm tròn 2 chữ số thập phân).
  - Phân bố số lượt theo từng mức 1–5 sao.
  - Tổng số lượt đánh giá.
  - Tỷ lệ đánh giá = số hồ sơ được đánh giá / số hồ sơ đóng trong kỳ.
- **BR-04:** Tính theo các hồ sơ có thời điểm đóng nằm trong khoảng thời gian. Phòng ban được tính theo phòng ban tại thời điểm đóng.
- **BR-05:** Danh sách nhận xét: mã hồ sơ, loại yêu cầu, phòng ban, số sao, nhận xét, ngày đánh giá. Mới nhất trước, 20 dòng mỗi trang. Lọc được theo số sao.
- **BR-06:** Không hiển thị tên và MSSV của sinh viên trong báo cáo.
- **BR-07:** Nhận xét hiển thị dạng văn bản đã escape.
- **BR-08:** Rate limit: 30 request mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu khoảng thời gian hoặc bộ lọc số sao không hợp lệ, hệ thống trả 400.
- **E-02:** Nếu không có lượt đánh giá nào, hệ thống trả 200; điểm trung bình là `null` và UI hiển thị "Chưa có đánh giá".
- **E-03:** Nếu truy vấn quá 10 giây, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Trong kỳ có 10 hồ sơ đóng, 4 hồ sơ được đánh giá 5, 4, 4, 3 sao -> điểm trung bình 4,00; tổng 4 lượt; tỷ lệ đánh giá 40%. _(Kiểm cho: luồng chính, BR-03, BR-04)_
- **AC-02 [Validation]:** Lọc theo số sao = 6 -> 400. _(Kiểm cho: E-01)_
- **AC-03 [Boundary]:** Không có đánh giá trong kỳ -> điểm trung bình hiển thị "Chưa có đánh giá" (không hiển thị 0), không lỗi chia cho 0. _(Kiểm cho: BR-01, E-02)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. Response không chứa tên hoặc MSSV của sinh viên. Nhận xét chứa `<script>` -> hiển thị dạng văn bản, không thực thi. _(Kiểm cho: BR-02, BR-06, BR-07)_
- **AC-05 [Rate Limit]:** Request thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-08)_
- **AC-06 [Resilience]:** Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại. _(Kiểm cho: E-03)_
- **AC-07 [Business Logic]:** Với dữ liệu của AC-01, lọc 4 sao -> danh sách nhận xét chỉ còn các lượt đánh giá 4 sao; điểm trung bình, tổng lượt và tỷ lệ đánh giá không đổi; phân bố hiển thị 5 sao: 1, 4 sao: 2, 3 sao: 1, 2 sao: 0, 1 sao: 0. Danh sách nhận xét có mã hồ sơ, loại, phòng ban, số sao, nhận xét, ngày đánh giá; mới nhất trước; 20 dòng mỗi trang. _(Kiểm cho: BR-01, BR-03, BR-05)_
- **AC-08 [Boundary]:** Kỳ không có hồ sơ `CLOSED` -> tỷ lệ đánh giá `null`, hiển thị `--`. Hồ sơ chuyển từ phòng A sang B rồi đóng ở B -> lọc phòng B có tính đánh giá của hồ sơ này. _(Kiểm cho: BR-01, BR-04)_

**Ví dụ Edge Case**
Một hồ sơ được đóng ngày 30/9 nhưng đến ngày 05/10 sinh viên mới đánh giá. Đánh giá được tính vào báo cáo tháng 9, vì báo cáo lọc theo thời điểm đóng. Vì vậy báo cáo tháng 9 có thể thay đổi nhẹ khi xem lại sau đó.

**Expected Result:** Báo cáo phản ánh đúng mức độ hài lòng của các hồ sơ đã đóng trong kỳ, và không trả các trường định danh sinh viên trong dữ liệu báo cáo.

---

### [FR-MGT-06] Quản lý phân quyền

**Mô tả**
Admin gán vai trò và phòng ban cho từng tài khoản, để hệ thống biết ai là nhân viên phòng nào và ai thuộc Admin.

**Actor**
Admin đã đăng nhập.

**Priority**
`Must`: Không cấp được quyền nhân viên thì phòng ban không dùng được hệ thống.

**Preconditions**
- Người dùng có vai trò `ADMIN`.

**Luồng chính**
1. Admin mở màn hình Phân quyền.
2. Hệ thống hiển thị STAFF/ADMIN và ô tìm email trường chính xác để cấp quyền cho tài khoản STUDENT đã có; không liệt kê hàng loạt sinh viên.
3. Admin thực hiện một trong các thao tác: thêm định danh chưa có, cấp quyền cho tài khoản có sẵn tìm theo email, đổi vai trò/phòng ban, vô hiệu hóa hoặc kích hoạt lại tài khoản.
4. Hệ thống kiểm tra quy tắc rồi lưu.
5. Thay đổi có hiệu lực ngay từ request kế tiếp của tài khoản đó.

**Input**
- **Người dùng/request:** List STAFF/ADMIN; tìm cấp quyền: email trường chính xác; thêm/cấp quyền: email, role, department_id nếu STAFF; sửa/kích hoạt: account_id, version, giá trị mới.
- **Hệ thống:** ADMIN active; tài khoản SSO đã có hoặc danh sách cấp quyền trước; ràng buộc người nhận hồ sơ.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200 list/search: id, email, tên nếu có, role, department, active, version; tìm STUDENT chỉ bằng email chính xác. Thêm mới 201, sửa/cấp quyền tài khoản có sẵn 200 với version mới.
- **Thay đổi/sự kiện:** Cấp/đổi quyền/phòng/active; tác dụng request kế tiếp; audit ROLE_CHANGE kèm subtype. Không trả MSSV/khoa/lớp/SĐT hoặc hồ sơ sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** ADMIN đầu được nạp khi triển khai từ định danh do trường xác nhận (SEC-01); không tạo tài khoản Admin bằng email ví dụ.
- **BR-02:** Tìm chính xác email đã chuẩn hóa có thể trả tài khoản STUDENT với id/email/tên/role/active/version tối thiểu để cấp quyền. Không trả MSSV, khoa/lớp, SĐT hoặc hồ sơ của sinh viên. Tài khoản có sẵn được cập nhật theo account_id/version, không tạo bản sao.
- **BR-03:** Chuẩn hóa định danh theo hợp đồng SSO được xác nhận; trim email, so sánh miền không phân biệt hoa/thường, quy tắc phần local theo nhà cung cấp. Không tự gộp hai tài khoản khi chưa xác nhận định danh.
- **BR-04:** Việc kiểm tra hồ sơ mở và cập nhật quyền/phòng/active phải nguyên tử với phân công (STF-07). Version tài khoản riêng không thay thế ràng buộc này. ROLE_CHANGE có subtype ACCOUNT_ADD/ROLE_SET/DEPARTMENT_SET/ACTIVATE/DEACTIVATE.
- **BR-05:** **Thêm tài khoản:** Admin nhập định danh SSO (email trường) của người dùng, chọn vai trò `STAFF` hoặc `ADMIN`, và chọn phòng ban nếu là `STAFF`. Người dùng đăng nhập SSO lần đầu mà không có trong danh sách này sẽ mặc định là `STUDENT`.
- **BR-06:** **Quy tắc vai trò và phòng ban:** `STAFF` phải có đúng một phòng ban. `ADMIN` và `STUDENT` không có phòng ban.
- **BR-07:** **Tài khoản bị vô hiệu hóa:** không đăng nhập được và không được phân công hồ sơ.
- **BR-08:** **Đổi phòng ban, đổi vai trò hoặc vô hiệu hóa một `STAFF`:** chỉ được làm khi nhân viên đó không còn phụ trách hồ sơ nào chưa đóng.
- **BR-09:** **Chống khóa hệ thống:** Không được tự hạ vai trò hoặc tự vô hiệu hóa chính mình. Hệ thống luôn phải còn ít nhất một `ADMIN` đang hoạt động.
- **BR-10:** Định danh SSO là duy nhất.
- **BR-11:** Bắt buộc gửi `version` của tài khoản khi sửa.
- **BR-12:** Rate limit: 30 thao tác ghi mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu định danh SSO sai định dạng, hoặc vai trò `STAFF` thiếu phòng ban, hệ thống trả 400.
- **E-02:** Nếu thao tác tạo mới dùng định danh đã tồn tại, trả 409 và hướng dẫn dùng chức năng tìm/cấp quyền tài khoản có sẵn; thao tác cấp quyền đúng account_id/version trả 200.
- **E-03:** Nếu nhân viên còn phụ trách hồ sơ chưa đóng, hệ thống trả 409 kèm danh sách mã hồ sơ cần phân công lại.
- **E-04:** Nếu thao tác khiến hệ thống không còn `ADMIN` nào đang hoạt động, hoặc người dùng tự hạ quyền chính mình, hệ thống trả 409.
- **E-05:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Thêm tài khoản `nva@aurora.edu.vn` (ví dụ) với vai trò `STAFF` thuộc Phòng Đào tạo -> 201. Khi người này đăng nhập SSO, họ vào Cổng Nhân viên và thấy hàng đợi của Phòng Đào tạo. _(Kiểm cho: luồng chính, BR-05)_
- **AC-02 [Validation]:** Tạo tài khoản `STAFF` không có phòng ban -> 400. Thêm mới định danh đã có -> 409; tìm email STUDENT có sẵn rồi cấp STAFF/phòng bằng account_id/version -> 200, không tạo duplicate account. _(Kiểm cho: BR-02, BR-06, BR-10, E-01, E-02)_
- **AC-03 [Business Logic]:** Đổi phòng ban của nhân viên đang phụ trách 2 hồ sơ chưa đóng -> 409 kèm 2 mã hồ sơ. Sau khi 2 hồ sơ được phân công lại cho người khác, đổi phòng ban -> 200. _(Kiểm cho: BR-08, E-03)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API phân quyền -> 403. Admin tự hạ vai trò của chính mình -> 409. Nhân viên bị hạ về `STUDENT` -> request kế tiếp vào Cổng Nhân viên bị 403. _(Kiểm cho: BR-09, E-04)_
- **AC-05 [Concurrency]:** Hai người trong Admin cùng sửa một tài khoản -> người lưu sau nhận 409. Hai người cạnh tranh thao tác khiến không còn ADMIN active -> chỉ thao tác giữ invariant được commit; tự deactivate/hạ quyền luôn 409. Phân công staff cạnh tranh deactivate/đổi phòng không để assignee ticket mở mất quyền. _(Kiểm cho: BR-04, BR-09, BR-11, E-04, E-05)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, phân quyền giữ nguyên. Thành công -> có bản ghi nhật ký `ROLE_CHANGE` (người thực hiện, tài khoản bị đổi, giá trị cũ, giá trị mới). _(Kiểm cho: BR-04)_
- **AC-07 [Validation]:** Định danh `abc` (sai định dạng email) -> 400. Thiếu `version` khi sửa -> 400. Cấp vai trò `ADMIN` -> tài khoản không có phòng ban. Tìm `nva@AURORA.edu.vn ` (có khoảng trắng, miền viết hoa) -> tìm thấy tài khoản `nva@aurora.edu.vn`; kết quả chỉ có id, email, tên, vai trò, trạng thái hoạt động, version; không có MSSV, khoa/lớp, SĐT hay hồ sơ của sinh viên. _(Kiểm cho: BR-02, BR-03, BR-06, BR-11, E-01)_
- **AC-08 [Business Logic]:** Vô hiệu hóa nhân viên C (không còn hồ sơ mở) -> C đăng nhập SSO bị từ chối và không có trong danh sách người nhận phân công. Người dùng đăng nhập SSO lần đầu, không có trong danh sách phân quyền -> tài khoản `STUDENT`. Hai tài khoản có định danh khác nhau không bị tự gộp. Nhật ký `ROLE_CHANGE` ghi đúng subtype (`ACCOUNT_ADD`, `ROLE_SET`, `DEPARTMENT_SET`, `ACTIVATE`, `DEACTIVATE`). _(Kiểm cho: BR-03, BR-04, BR-05, BR-07)_
- **AC-09 [Security/Rate Limit]:** Sau triển khai, tài khoản `ADMIN` ban đầu là định danh do trường xác nhận; không có tài khoản Admin dùng email ví dụ. Thao tác ghi thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-01, BR-12)_

**Ví dụ Edge Case**
Một nhân viên chuyển công tác từ Phòng Đào tạo sang Phòng Tài chính nhưng còn 3 hồ sơ đang xử lý. Hệ thống từ chối đổi phòng ban và liệt kê 3 mã hồ sơ đó. Đồng nghiệp ở Phòng Đào tạo phân công lại 3 hồ sơ (FR-STF-07), sau đó Admin mới đổi phòng ban được.

**Expected Result:** Mỗi tài khoản có đúng vai trò và phòng ban hợp lệ, không có hồ sơ nào bị bỏ lại với người phụ trách không còn quyền, và hệ thống luôn còn Admin.

---

### [FR-MGT-07] Thiết lập SLA cho từng loại yêu cầu

**Mô tả**
Admin đặt thời hạn xử lý (SLA) cho từng loại yêu cầu, làm căn cứ để hệ thống xác định hồ sơ quá hạn.

**Actor**
Admin đã đăng nhập.

**Priority**
`Must`: Mỗi loại yêu cầu cần SLA để tính hạn xử lý ngay khi tạo hồ sơ (FR-CORE-03).

**Preconditions**
- Người dùng có vai trò `ADMIN`.
- Danh mục loại yêu cầu đã được cấu hình (CORE-05).

**Luồng chính**
1. Admin mở màn hình Thiết lập SLA.
2. Hệ thống hiển thị danh sách loại yêu cầu kèm SLA hiện tại.
3. Admin sửa SLA của một loại yêu cầu và lưu.
4. Hệ thống áp dụng SLA mới cho các hồ sơ tạo từ thời điểm này trở đi.

**Input**
- **Người dùng/request:** List loại yêu cầu/SLA; cập nhật: request_type_id, version cấu hình, sla_hours số nguyên 1–720.
- **Hệ thống:** ADMIN, cấu hình SLA hiện tại.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M05](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M04](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200: request_type_id/name, sla_hours, version; thay đổi trả version mới, no-op giữ version.
- **Thay đổi/sự kiện:** Chỉ ảnh hưởng ticket tạo sau điểm commit cấu hình; audit SLA_CHANGE; không ghi đè SLA đã sửa khi seed lại.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- **BR-01:** List và response lưu luôn có version cấu hình. Đọc SLA để tạo hồ sơ và cập nhật SLA có điểm hiệu lực xác định theo transaction: mỗi hồ sơ lưu đúng một SLA snapshot đã commit, không trộn hai giá trị. Seed danh mục không ghi đè SLA admin đã đổi.
- **BR-02:** SLA tính bằng giờ (giờ lịch, tính cả ngoài giờ hành chính và ngày nghỉ). Là số nguyên từ 1 đến 720.
- **BR-03:** Mỗi loại yêu cầu có đúng một giá trị SLA. Giá trị ban đầu được nạp sẵn khi triển khai.
- **BR-04:** Hạn xử lý của mỗi hồ sơ được chốt tại thời điểm tạo hồ sơ. Đổi SLA không làm thay đổi hạn của hồ sơ đã tạo.
- **BR-05:** Không thêm hoặc xóa loại yêu cầu ở màn hình này (danh mục cố định theo CORE-05).
- **BR-06:** Bắt buộc gửi `version`. Lưu cùng giá trị hiện tại trả 200 và không ghi nhật ký.
- **BR-07:** Rate limit: 30 thao tác ghi mỗi phút cho mỗi người dùng.

**Alternative / Error Flows**
- **E-01:** Nếu SLA không phải số nguyên, nhỏ hơn 1 hoặc lớn hơn 720, hệ thống trả 400.
- **E-02:** Nếu loại yêu cầu không tồn tại, hệ thống trả 404.
- **E-03:** Nếu lệch `version`, hệ thống trả 409.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Đổi SLA của loại "Học phí" từ 72 giờ sang 48 giờ -> 200. Hồ sơ "Học phí" tạo sau đó có hạn bằng thời điểm gửi cộng 48 giờ. _(Kiểm cho: luồng chính, BR-02)_
- **AC-02 [Validation/Boundary]:** SLA = 0, 721 hoặc 2,5 -> 400. SLA = 1 và 720 -> 200. _(Kiểm cho: BR-02, E-01)_
- **AC-03 [Business Logic]:** Hồ sơ tạo trước khi đổi SLA -> giữ nguyên hạn cũ (72 giờ). _(Kiểm cho: BR-04)_
- **AC-04 [Security]:** Tài khoản `STAFF` gọi API -> 403. _(Kiểm cho: quy ước chung của module)_
- **AC-05 [Concurrency/Idempotency]:** Hai người trong Admin cùng sửa SLA một loại -> người lưu sau nhận 409. Lưu lại cùng giá trị với version hiện tại -> 200, không thêm audit/tăng version. Seed lại không ghi đè SLA đã sửa; tạo ticket cạnh tranh đổi SLA lưu đúng một snapshot đã commit. _(Kiểm cho: BR-01, BR-06, E-03)_
- **AC-06 [Resilience & Audit]:** Lưu lỗi -> rollback, SLA giữ nguyên. Thành công -> có bản ghi nhật ký `SLA_CHANGE` (loại yêu cầu, giá trị cũ, giá trị mới, người thực hiện). _(Kiểm cho: quy ước chung của module)_
- **AC-07 [Validation/Rate Limit]:** Sửa SLA của loại yêu cầu không tồn tại -> 404. Thiếu `version` -> 400. Thao tác ghi thứ 31 trong 1 phút -> 429. _(Kiểm cho: BR-06, BR-07, E-02)_
- **AC-08 [Business Logic]:** Màn hình SLA liệt kê mỗi loại yêu cầu đúng một giá trị (giá trị ban đầu từ dữ liệu nạp khi triển khai) kèm `version`; không có nút thêm hoặc xóa loại yêu cầu. _(Kiểm cho: BR-01, BR-03, BR-05)_

**Ví dụ Edge Case**
Admin rút SLA của một loại yêu cầu từ 72 giờ xuống 24 giờ. Một hồ sơ cùng loại đã được gửi 30 giờ trước đó. Hồ sơ này không bị chuyển ngay sang `OVERDUE`, vì hạn của nó đã được chốt là 72 giờ lúc tạo.

**Expected Result:** Mỗi loại yêu cầu có đúng một SLA hợp lệ, thay đổi SLA chỉ ảnh hưởng hồ sơ mới, và mọi thay đổi đều được ghi lại.
