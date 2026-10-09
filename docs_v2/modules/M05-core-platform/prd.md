# M5. Lõi hệ thống & Nền tảng – Functional Requirement Specification

> Cập nhật: 09/10/2026. Giữ nguyên 42 chức năng của 5 module trong repo; bổ sung và thống nhất đặc tả trực tiếp tại module. Các giả định chưa được trường xác nhận được ghi tại phần đầu module.

**Mục đích module:** Mã hồ sơ, trạng thái, SLA, danh mục, lịch sử, giới hạn tệp và quy tắc UI.

**Actor chính:** Hệ thống và mọi vai trò theo chức năng. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** M1–M3 gọi nghiệp vụ; M4 kiểm tra quyền và audit.

## Luồng nền tảng và liên kết module

Khi M1 tạo ticket, M5 lấy danh mục/SLA snapshot và sinh mã; khi M2/M1 thao tác, M5 kiểm tra status/version, cập nhật hạn/pause và history. Tác vụ quá hạn đổi NEW/IN_PROGRESS thành OVERDUE. M3 đọc số liệu từ timestamp và trường đã chốt; M4 bảo đảm scope/audit. VN/EN và responsive áp dụng cho cả ba cổng.

**Input module:** sự kiện nghiệp vụ, version, thời gian server, danh mục/SLA và lựa chọn giao diện.
**Output module:** mã, trạng thái, hạn nội bộ, lịch sử theo quyền, quy tắc tệp/ngôn ngữ/bố cục. Module này không tạo một portal hoặc vai trò mới.

**Cần xác nhận:** danh mục/mapping/tên VI-EN/SLA đầu, hạ tầng/storage, thời hạn lưu dữ liệu và tiêu chí chất lượng. Giờ lịch/pause PENDING_INFO là phương án giữ từ GitHub. OVERDUE chờ bổ sung không pause; đã quá hạn trước cron thì không cho IN_PROGRESS tạo pause muộn. Đơn vị file dùng 10 MiB = 10.485.760 byte, gateway 55 MiB; nhóm xác nhận trong thiết kế triển khai.

## Quy tắc dữ liệu và cập nhật dùng cho M1–M5

### Input văn bản, thời gian và danh mục

- Văn bản do người dùng nhập là plain text, không HTML; render encode đúng ngữ cảnh cho description/content/note/resolution/reason/comment và tên tệp. Backend không thực thi nội dung.
- Trim trước kiểm tra ký tự. Độ dài tính theo Unicode code point sau trim, frontend/backend cùng quy tắc; không dùng số byte UTF-8 làm độ dài ký tự. Comment không nhập lưu null; không tự dịch nội dung người dùng.
- Timestamp lưu UTC; hiển thị/đếm ngày theo Asia/Ho_Chi_Minh (UTC+7). NOW là thời gian server, không tin timestamp client để quyết hạn.
- Khoảng báo cáo nhận from/to dạng YYYY-MM-DD, cả hai cùng có hoặc cùng bỏ trống. Mặc định hôm nay và 29 ngày trước; tối đa 365 ngày tính bao gồm hai đầu. Query timestamp từ đầu ngày from đến trước đầu ngày sau to; from>to/ngày sai trả 400.
- Report filter ID không tồn tại trả 400, trừ thao tác sửa SLA đối tượng không có trả 404 theo FR. List page/page_size là số nguyên dương, giới hạn theo từng FR, page vượt tổng trả danh sách rỗng 200. Response có total_items/total_pages; không có dữ liệu total_pages=0.
- Chuẩn hóa email theo hợp đồng SSO được trường xác nhận; định danh cấp quyền phải map đến danh tính đã xác thực, không chỉ tin email trong input.

### Version và transaction nghiệp vụ

- Ticket có version tăng một lần cho mỗi thao tác thay đổi: phân công, priority/escalate, chuyển, note, info request, supplement, đóng, job quá hạn. Không cần status đổi mới tăng version.
- Mọi API thay đổi các đối tượng này yêu cầu version hiện tại. Thiếu/sai kiểu trả 400, version cũ trả 409 sau scope. No-op hợp lệ không tăng version/lặp history/audit; vẫn kiểm tra quyền và version.
- Rating là đối tượng riêng unique theo ticket; notification read là riêng. CLOSED cấm sửa ticket, không cấm rating hoặc đọc thông báo.
- Cập nhật ticket, các đối tượng DB liên quan, audit, history, notification cùng transaction. Một event dùng cùng event_id; không tạo notification trùng REQUEST_INFO/status change.
- Account và SLA có version riêng. Kiểm tra manager còn lại và phân công/account active phải nguyên tử trên ràng buộc, không chỉ dựa hai version độc lập.

### Idempotency và retry

- STU-02, STU-09, STF-04/05/06 bắt buộc Idempotency-Key; key duy nhất theo account + operation + target, TTL 24 giờ sau hoàn tất.
- Cùng key/cùng payload đã thành công: sau kiểm tra phiên/quyền hiện tại, trả lại HTTP status và dữ liệu nghiệp vụ đã lưu; không thực hiện lại hoặc tạo thêm file/audit/notification. Với transfer/role đổi khiến caller mất quyền thì không trả dữ liệu ngoài scope.
- Cùng key khác payload trả 409 IDEMPOTENCY_PAYLOAD_MISMATCH; payload bao gồm các trường nghiệp vụ và checksum file. Hai request cùng key đang xử lý: một thực hiện, bên kia nhận 409 REQUEST_IN_PROGRESS kèm Retry-After và retry cùng key, không chạy lần hai.
- Kết quả thành công và key được lưu nguyên tử trong transaction nghiệp vụ. Lỗi trước commit không lưu key thành công; UI được retry cùng key. Sau TTL không bảo đảm retry cũ không tạo mới; UI phải tra cứu kết quả trước khi gửi thao tác tạo mới.
- Kiểm tra kết quả key thành công trước version/trạng thái cũ. Một retry đã xác nhận cùng key không bị coi là đóng lại ticket CLOSED.
- Key không phải công cụ phân quyền hoặc chống mọi spam; rate limit vẫn áp dụng. Retry hợp lệ đã thành công không tính thêm vào quota 10 ticket/giờ, nhưng vẫn có giới hạn request chống spam.
- Rating dùng unique ticket + trả 409 nếu đã có, không bắt thêm key. UI tải lại đánh giá sau mất phản hồi.

### Lưu tệp và lỗi storage

- Cho PDF/JPG/PNG theo magic bytes; 0–5 file/lần, không file vẫn hợp lệ. File rỗng 400, 10 MiB = 10.485.760 byte/tệp; 55 MiB tổng multipart ở gateway. Số lượng/tên >255 ký tự sai trả 400; dung lượng 413; định dạng 415.
- UI kiểm tra sớm, backend kiểm tra lại; tên kho random, tên gốc sanitize (không đường dẫn/control chars), metadata MIME xác minh. Tệp ngoài web public, stream qua API quyền; không phát presigned URL mang quyền.
- Quy trình kỹ thuật cần đáp ứng: validate → upload vùng tạm → sẵn sàng object riêng → transaction DB lưu tham chiếu → commit → trả thành công. Nếu DB rollback, xóa object đã chuẩn bị. Idempotency gate ngăn upload duplicate retry.
- DB và object store không có transaction chung. Xóa bù lỗi thì lưu log/quarantine reference, job dọn tệp không tham chiếu xử lý lại; tệp chưa được DB tham chiếu không tải qua API. Không xóa object đã tham chiếu bởi transaction thành công.
- Mất mạng sau commit: key trả kết quả đã tạo. Mất mạng trước commit: không có nghiệp vụ thành công một phần, job dọn upload tạm. Nếu phần mềm dùng thiết kế khác vẫn phải chứng minh các kết quả trên bằng test.
- Chỉ tài khoản đang có quyền tại đầu stream được tải; không thu hồi bản đã tải trên thiết bị. Cache-Control private/no-store; nội dung header MIME/filename an toàn.

## Kiểm tra chất lượng và ranh giới phạm vi

- PRD mô tả điều kiện cần kiểm chứng, không khẳng định đã triển khai/pass. Kiểm tra đủ các AC từng module và luồng liên module: tạo → phân công → bổ sung → chuyển/quá hạn → đóng → rating → report.
- Kiểm tra race bổ sung/đóng cùng version; phân công/đổi quyền; chuyển phòng và request cũ; đổi SLA/tạo ticket; retry/key trùng và mất phản hồi; public history không lộ due_at.
- Kiểm tra upload/storage/DB lỗi và dọn bù, seed không ghi đè SLA; timezone theo ngày Việt Nam; báo cáo null/zero; logout/thu hồi/cache; VN/EN/mobile/browser.
- Quy mô tham chiếu proposal: 3.000 sinh viên và 100–200 người dùng đồng thời. Bài test phải nêu hạ tầng, dataset, số user, kịch bản/rate, p95 và tỷ lệ lỗi. Timeout 5/10 giây không phải cam kết hiệu năng. Mục tiêu hiệu năng/usability phải được nhóm/khách chốt ở prototype, không tự lấy số từ bản nháp cũ.
- Bộ 200 ticket giả phục vụ kiểm thử/bàn giao cần phủ 5 trạng thái, phòng/loại, file, info request, transfer, rating và biên thời gian. Không dùng dữ liệu sinh viên thật; số phân bổ được QA chốt theo test plan.
- Docker/env, source, OpenAPI/mã lỗi, 3 hướng dẫn, test/usability report, triển khai và nghiệm thu/bảo hành là đầu ra dự án theo proposal; không tự biến mỗi đầu ra thành FR mới hoặc đưa lịch/nhân sự vào FRS.
- Thời hạn ticket/tệp/history/audit, backup và người vận hành phải được trường xác nhận trước production; không tự chạy xóa dữ liệu. Trường chịu vận hành sau bàn giao theo proposal.

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Tính chất module:** Đây là module dùng chung. Hầu hết chức năng do hệ thống tự thực hiện hoặc được các module M1–M4 gọi tới, nên Actor thường là "Hệ thống".
- **Thời gian:** Lưu trong cơ sở dữ liệu theo UTC. Hiển thị và tính theo ngày dùng giờ Việt Nam (UTC+7).
- **Trạng thái hồ sơ:** `NEW` (Mới tiếp nhận), `IN_PROGRESS` (Đang xử lý), `PENDING_INFO` (Chờ bổ sung), `OVERDUE` (Quá hạn), `CLOSED` (Đã đóng).
- **Mức ưu tiên:** `LOW` (Thấp), `MEDIUM` (Trung bình), `HIGH` (Cao).
- **Nhật ký:** Mọi thay đổi đều tuân theo FR-SEC-06.

---

### [FR-CORE-01] Sinh mã hồ sơ

**Mô tả**
Hệ thống sinh cho mỗi hồ sơ một mã duy nhất, dễ đọc, dùng để sinh viên và nhân viên tra cứu và trao đổi.

**Actor**
Hệ thống (được gọi khi tạo hồ sơ ở FR-STU-02).

**Preconditions**
- Hồ sơ đang được tạo trong transaction của FR-STU-02.

**Luồng chính**
1. Hệ thống xác định năm hiện tại theo giờ Việt Nam.
2. Hệ thống lấy số thứ tự kế tiếp từ sequence của năm đó.
3. Hệ thống ghép mã theo định dạng và gán vào hồ sơ.
4. Transaction tạo hồ sơ được commit.

**Input**
- **Người dùng/request:** created_at cố định của transaction tạo hồ sơ.
- **Hệ thống:** Năm Việt Nam và sequence năm trong DB.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Mã duy nhất USP-YYYY-NNNNN, ít nhất 5 chữ số phần sequence.
- **Thay đổi/sự kiện:** Sequence có thể nhảy do rollback; không tái sử dụng mã, không lộ định danh sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Định dạng mã: `USP-YYYY-NNNNN`.
  - `YYYY`: năm tạo hồ sơ theo giờ Việt Nam.
  - `NNNNN`: số thứ tự 5 chữ số, thêm số 0 ở đầu nếu thiếu. Bắt đầu lại từ `00001` mỗi năm.
- Số thứ tự lấy từ sequence của cơ sở dữ liệu, mỗi năm một sequence. Cột mã có unique constraint.
- Nếu một năm có hơn 99999 hồ sơ thì phần số tự mở rộng thêm chữ số, không báo lỗi.
- Nếu transaction tạo hồ sơ bị rollback thì số thứ tự đã lấy bị bỏ qua. Mã có thể bị nhảy số, nhưng không bao giờ trùng.
- Mã không thay đổi trong suốt vòng đời hồ sơ và không chứa thông tin cá nhân.

**Alternative / Error Flows**
- Nếu không lấy được số thứ tự, hệ thống rollback việc tạo hồ sơ và trả 500.
- Nếu vi phạm unique constraint (lỗi bất thường), hệ thống rollback, trả 500 và ghi log lỗi hệ thống.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ đầu tiên tạo trong năm 2027 -> mã `USP-2027-00001`. Hồ sơ kế tiếp -> `USP-2027-00002`.
- **AC-02 [Validation]:** Mọi mã sinh ra đều khớp mẫu `^USP-\d{4}-\d{5,}$`.
- **AC-03 [Boundary]:** Hồ sơ tạo lúc 06:59 ngày 01/01/2027 giờ Việt Nam (tức 23:59 ngày 31/12/2026 theo UTC) -> mã năm 2027.
- **AC-04 [Security]:** Mã không chứa MSSV, tên sinh viên hay thông tin cá nhân nào khác.
- **AC-05 [Concurrency]:** 100 hồ sơ được tạo đồng thời -> 100 mã khác nhau, không trùng.
- **AC-06 [Resilience]:** Transaction tạo hồ sơ bị rollback sau khi đã lấy số thứ tự -> không có hồ sơ nào mang mã đó; hồ sơ tiếp theo dùng số kế tiếp.

**Ví dụ Edge Case**
Hồ sơ `USP-2026-00057` bị rollback do lỗi lưu tệp. Hồ sơ kế tiếp nhận mã `USP-2026-00058`, nên trong hệ thống không có mã 00057. Đây là hành vi đúng: mã hồ sơ được bảo đảm duy nhất, không bảo đảm liên tục.

**Expected Result:** Mỗi hồ sơ có đúng một mã duy nhất, đúng định dạng và đúng năm theo giờ Việt Nam.

---

### [FR-CORE-02] Trạng thái hồ sơ và quy tắc chuyển trạng thái

**Mô tả**
Định nghĩa 5 trạng thái của hồ sơ và các bước chuyển hợp lệ, để mọi module xử lý trạng thái thống nhất và hồ sơ luôn theo dõi được.

**Actor**
Hệ thống (áp dụng mỗi khi có thao tác làm đổi trạng thái).

**Preconditions**
- Hồ sơ đã tồn tại (trừ bước tạo mới).

**Luồng chính**
1. Một thao tác hoặc tác vụ hệ thống yêu cầu đổi trạng thái.
2. Hệ thống kiểm tra cặp (trạng thái hiện tại, sự kiện) có nằm trong bảng chuyển trạng thái hợp lệ không.
3. Nếu hợp lệ, hệ thống cập nhật trạng thái, tăng `version`, ghi lịch sử (FR-CORE-06) và nhật ký (FR-SEC-06), rồi tạo thông báo cho sinh viên nếu thuộc diện được báo (FR-STU-08).
4. Nếu không hợp lệ, hệ thống từ chối.

**Input**
- **Người dùng/request:** Sự kiện nghiệp vụ, ticket_id và version hiện tại; job quá hạn lấy version khi đọc.
- **Hệ thống:** Status và scope hiện tại; due_at đã chốt; quy tắc transition.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Trạng thái/version mới hoặc 409 khi transition/version sai; ngoài phạm vi vẫn 404.
- **Thay đổi/sự kiện:** Cập nhật nguyên tử, history/audit/notification theo event; CLOSED vẫn cho tạo rating riêng, không cho sửa ticket.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Quy tắc version áp dụng cả thay đổi không đổi status: note, phân công, priority, transfer, request/supplement/close và job quá hạn. Mỗi thao tác làm thay đổi ticket tăng version một lần; no-op không tăng; đọc/rating/notification không tăng version ticket.
- NOW dùng thời gian server. Trong khoảng due_at đã qua nhưng job chưa chạy, không cho yêu cầu bổ sung chuyển IN_PROGRESS thành PENDING_INFO (409); phân công/đóng vẫn được xử lý theo version và timestamp. Nếu đóng thắng trước job, ticket CLOSED giữ nguyên, thời lượng xử lý vẫn phản ánh thời gian thực; không tự tạo một status quá hạn sau CLOSED.
- Tác vụ quá hạn khóa mỗi ticket theo version và bỏ qua bản đã đổi; scheduler có một tiến trình hiệu lực. Ranh giới quá hạn là now > due_at, lần chạy kế tiếp đổi trong tối đa 60 giây khi job khỏe.
- Bảng chuyển trạng thái hợp lệ:

| Trạng thái hiện tại | Sự kiện | Trạng thái mới | Do ai |
|---|---|---|---|
| (chưa có) | Sinh viên gửi yêu cầu | `NEW` | FR-STU-02 |
| `NEW` | Phân công lần đầu | `IN_PROGRESS` | FR-STF-07 |
| `NEW` | Vượt hạn xử lý | `OVERDUE` | Hệ thống |
| `IN_PROGRESS` | Yêu cầu bổ sung | `PENDING_INFO` | FR-STF-05 |
| `IN_PROGRESS` | Vượt hạn xử lý | `OVERDUE` | Hệ thống |
| `PENDING_INFO` | Sinh viên bổ sung xong | `IN_PROGRESS` | FR-STU-09 |
| `IN_PROGRESS`, `PENDING_INFO`, `OVERDUE` | Đóng hồ sơ | `CLOSED` | FR-STF-06 |

- Hồ sơ `OVERDUE` giữ nguyên trạng thái này khi được phân công, phân công lại, yêu cầu bổ sung hoặc nhận bổ sung từ sinh viên. Trạng thái chỉ đổi khi hồ sơ được đóng.
- `PENDING_INFO` không chuyển sang `OVERDUE`, vì SLA đang tạm dừng (FR-CORE-03).
- `CLOSED` là trạng thái cuối, không chuyển đi đâu được nữa.
- Các thao tác không làm đổi trạng thái: phân công lại, đặt mức ưu tiên, escalate, chuyển phòng ban, ghi nhật ký xử lý.
- **Tác vụ kiểm tra quá hạn:**
  - Chạy mỗi 1 phút.
  - Chuyển sang `OVERDUE` mọi hồ sơ đang `NEW` hoặc `IN_PROGRESS` có hạn xử lý đã qua.
  - Tại một thời điểm chỉ có một tiến trình chạy tác vụ này.
- Mọi lần đổi trạng thái đều dùng khóa theo `version` (optimistic locking).

**Alternative / Error Flows**
- Nếu cặp (trạng thái, sự kiện) không hợp lệ, hệ thống trả 409 "Thao tác không hợp lệ với trạng thái hiện tại của hồ sơ".
- Nếu lệch `version`, hệ thống trả 409.
- Nếu tác vụ kiểm tra quá hạn bị lỗi giữa chừng, các hồ sơ đã xử lý được giữ nguyên, các hồ sơ còn lại được xử lý ở lần chạy sau.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Một hồ sơ đi đủ luồng `NEW` -> `IN_PROGRESS` -> `PENDING_INFO` -> `IN_PROGRESS` -> `CLOSED` -> mỗi bước hợp lệ có history theo bảng gộp/tách CORE-06; không bắt một hành động chỉ có đúng một mốc khi có public/internal riêng.
- **AC-02 [Validation]:** Gọi đóng hồ sơ đang `NEW` -> 409. Gọi yêu cầu bổ sung cho hồ sơ đang `PENDING_INFO` -> 409. Mọi thao tác thay đổi ticket CLOSED -> 409; tạo rating STU-10 và đánh dấu notification đã đọc vẫn hợp lệ, vì không sửa ticket.
- **AC-03 [Boundary]:** Hồ sơ `IN_PROGRESS` có hạn lúc 10:00:00 -> lúc 09:59:59 vẫn `IN_PROGRESS`; muộn nhất 10:01:00 chuyển `OVERDUE`. Hồ sơ `PENDING_INFO` đã qua mốc hạn ban đầu -> vẫn `PENDING_INFO`.
- **AC-04 [Security]:** Client gửi trực tiếp `status = CLOSED` trong body của một API khác -> hệ thống bỏ qua. Trạng thái chỉ đổi qua các sự kiện trong bảng.
- **AC-05 [Concurrency/Idempotency]:** Job quá hạn và close dùng cùng version cạnh tranh -> một thay đổi commit, bên kia retry theo trạng thái/version mới; CLOSED không đổi lại. Close dùng version OVERDUE mới vẫn hợp lệ và tạo CLOSED, không coi chuỗi hợp lệ đó là race lỗi. Tác vụ chạy lại trên hồ sơ đã `OVERDUE` -> không đổi gì, không tạo thêm lịch sử hay thông báo.
- **AC-06 [Resilience & Audit]:** Tác vụ quá hạn bị dừng giữa chừng -> lần chạy sau xử lý tiếp các hồ sơ còn lại. Mỗi lần hệ thống tự chuyển `OVERDUE` -> có bản ghi `STATUS_CHANGE` với người thực hiện là `SYSTEM`, và sinh viên nhận thông báo.

**Ví dụ Edge Case**
Hồ sơ `NEW` chưa ai phân công và đã vượt hạn, nên chuyển `OVERDUE`. Sau đó nhân viên phân công người phụ trách. Hồ sơ vẫn ở `OVERDUE` (không quay về `IN_PROGRESS`), và người phụ trách xử lý rồi đóng hồ sơ từ trạng thái này.

**Expected Result:** Hồ sơ chỉ chuyển trạng thái theo đúng bảng chuyển hợp lệ, và mỗi lần chuyển đều được ghi lại.

---

### [FR-CORE-03] Tính hạn xử lý (SLA)

**Mô tả**
Hệ thống tính hạn xử lý của từng hồ sơ theo SLA của loại yêu cầu, có tạm dừng khi chờ sinh viên bổ sung, làm căn cứ xác định quá hạn.

**Actor**
Hệ thống.

**Preconditions**
- Mỗi loại yêu cầu đã có giá trị SLA (FR-MGT-07).

**Luồng chính**
1. Khi hồ sơ được tạo, hệ thống lấy SLA hiện tại của loại yêu cầu và tính hạn xử lý: `due_at = created_at + sla_hours`.
2. Khi hồ sơ chuyển sang `PENDING_INFO`, hệ thống ghi thời điểm bắt đầu tạm dừng.
3. Khi hồ sơ rời `PENDING_INFO`, hệ thống cộng thêm khoảng thời gian đã tạm dừng vào `due_at`.
4. Tác vụ kiểm tra quá hạn (FR-CORE-02) so sánh `due_at` với thời điểm hiện tại.
5. Khi hồ sơ đóng, hệ thống ngừng tính SLA.

**Input**
- **Người dùng/request:** Tạo hồ sơ, bắt đầu/kết thúc PENDING_INFO hoặc đóng.
- **Hệ thống:** created_at, SLA snapshot, due_at, pause_started_at, paused_seconds và thời gian server.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** due_at/remaining_seconds/sla_paused cho staff; processing_duration khi CLOSED phục vụ báo cáo.
- **Thay đổi/sự kiện:** Cộng bù mỗi pause đúng một lần; OVERDUE không pause theo trạng thái; giá trị hạn không trả cho sinh viên.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Khi PENDING_INFO, remaining_seconds = due_at - pause_started_at và giữ cố định; khi hoạt động = due_at - now. Khi CLOSED, SLA không có countdown đang chạy.
- OVERDUE có yêu cầu bổ sung mở vẫn không chuyển PENDING_INFO, không tạo pause và không cộng bù thời gian chờ đó. MGT-02 chỉ trừ các khoảng PENDING_INFO; đây là cách tính hiện hành phương án nghiệp vụ nêu ở đầu module.
- Thời điểm kết thúc pause được lưu cùng update trạng thái/due_at/paused_seconds; chỉ cộng một lần theo event. Staff thấy pause flag; student không nhận due_at kể cả qua lịch sử.
- SLA tính bằng giờ lịch (tính cả ngoài giờ hành chính và ngày nghỉ).
- Giá trị SLA và hạn xử lý được chốt tại thời điểm tạo hồ sơ. Đổi SLA ở FR-MGT-07 không làm thay đổi hạn của hồ sơ đã tạo.
- Thời gian ở `PENDING_INFO` không bị tính vào SLA. Hồ sơ có thể tạm dừng nhiều lần, và mỗi lần đều được cộng bù vào hạn.
- Các thao tác chuyển phòng ban, phân công lại, đổi mức ưu tiên, escalate không làm thay đổi `due_at`.
- Hồ sơ đóng khi đang `PENDING_INFO` thì khoảng tạm dừng cuối được tính đến thời điểm đóng.
- Thời gian còn lại khi hoạt động = due_at - now; khi PENDING_INFO = due_at - pause_started_at và giữ nguyên đến khi kết thúc pause theo quy tắc trên.
- Thời gian xử lý thực tế dùng cho FR-MGT-02 = thời điểm đóng trừ thời điểm gửi, trừ tổng thời gian tạm dừng.

**Alternative / Error Flows**
- Nếu loại yêu cầu chưa có SLA tại thời điểm tạo hồ sơ (lỗi cấu hình), hệ thống trả 422 và không tạo hồ sơ.
- Nếu ghi thời điểm tạm dừng hoặc cộng bù bị lỗi, hệ thống rollback cả thao tác đổi trạng thái.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Loại yêu cầu có SLA 48 giờ, hồ sơ tạo lúc 08:00 ngày 01/10 -> `due_at` = 08:00 ngày 03/10.
- **AC-02 [Business Logic]:** Hồ sơ trên chờ bổ sung từ 10:00 đến 16:00 ngày 01/10 -> `due_at` mới = 14:00 ngày 03/10.
- **AC-03 [Boundary]:** Hồ sơ tạm dừng 2 lần, mỗi lần 3 giờ -> `due_at` bị lùi tổng cộng 6 giờ. SLA đổi từ 48 thành 24 giờ sau khi hồ sơ đã tạo -> `due_at` của hồ sơ đó không đổi.
- **AC-04 [Security]:** Client gửi `due_at` trong body bất kỳ API nào -> hệ thống bỏ qua.
- **AC-05 [Concurrency]:** Sinh viên bổ sung đúng lúc tác vụ quá hạn đang chạy -> hồ sơ không bị chuyển `OVERDUE` dựa trên `due_at` cũ khi chưa được cộng bù.
- **AC-06 [Resilience & Audit]:** Lỗi khi cộng bù thời gian -> rollback, hồ sơ vẫn ở `PENDING_INFO`, `due_at` giữ nguyên. Hạn xử lý ban đầu và mỗi lần cộng bù đều được lưu trong lịch sử hồ sơ (FR-CORE-06).

**Ví dụ Edge Case**
Hồ sơ còn 1 giờ đến hạn thì nhân viên yêu cầu bổ sung, và sinh viên mất 3 ngày mới trả lời. Khi sinh viên bổ sung xong, `due_at` được lùi thêm 3 ngày, nên hồ sơ vẫn còn đúng 1 giờ để xử lý, không bị chuyển `OVERDUE` ngay khi quay lại `IN_PROGRESS`.

**Expected Result:** Hạn xử lý của mỗi hồ sơ được tính đúng theo SLA lúc tạo, cộng bù đủ thời gian chờ sinh viên, và không bị ảnh hưởng bởi thao tác điều phối.

---

### [FR-CORE-04] Mức độ ưu tiên

**Mô tả**
Định nghĩa 3 mức ưu tiên dùng chung, giúp phòng ban sắp xếp và lọc hồ sơ cần xử lý trước.

**Actor**
Hệ thống (được dùng bởi FR-STU-02, FR-STF-01, FR-STF-08, FR-STF-10).

**Preconditions**
- Không có.

**Luồng chính**
1. Khi hồ sơ được tạo, hệ thống gán mức ưu tiên mặc định.
2. Nhân viên đổi mức ưu tiên qua FR-STF-08 hoặc FR-STF-10.
3. Hàng đợi sắp xếp theo mức ưu tiên.

**Input**
- **Người dùng/request:** Thao tác tạo/đổi ưu tiên/escalate từ FR tương ứng.
- **Hệ thống:** Ba mức LOW/MEDIUM/HIGH, default MEDIUM, quyền staff.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Ưu tiên hợp lệ cho staff, tên dịch VN/EN; API student loại bỏ trường này.
- **Thay đổi/sự kiện:** Đổi thứ tự hàng đợi; không ảnh hưởng due_at/status.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Ba mức, theo thứ tự tăng dần: `LOW` < `MEDIUM` < `HIGH`.
- Hồ sơ mới mặc định là `MEDIUM`. Sinh viên không chọn được mức ưu tiên.
- Mức ưu tiên không ảnh hưởng tới SLA hay hạn xử lý.
- Sinh viên không thấy mức ưu tiên của hồ sơ.
- Tên hiển thị theo ngôn ngữ: Thấp / Low, Trung bình / Medium, Cao / High.

**Alternative / Error Flows**
- Nếu API nhận giá trị ngoài 3 mức, hệ thống trả 400.
- Nếu sinh viên gửi kèm mức ưu tiên khi tạo hồ sơ, hệ thống bỏ qua và gán `MEDIUM`.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên tạo hồ sơ -> mức ưu tiên là `MEDIUM`.
- **AC-02 [Validation]:** Gửi mức `URGENT` -> 400.
- **AC-03 [Boundary]:** Hàng đợi có hồ sơ ở cả 3 mức -> thứ tự hiển thị là `HIGH`, `MEDIUM`, `LOW`.
- **AC-04 [Security]:** Sinh viên gửi `priority = HIGH` khi tạo hồ sơ -> hồ sơ vẫn là `MEDIUM`. API chi tiết hồ sơ của sinh viên không trả về trường mức ưu tiên.
- **AC-05 [Concurrency]:** Hai nhân viên đổi mức ưu tiên cùng lúc -> người sau nhận 409 (theo FR-STF-08).
- **AC-06 [Resilience & Audit]:** Đổi mức ưu tiên lỗi -> rollback, mức cũ giữ nguyên. Mỗi lần đổi -> có bản ghi `CHANGE_PRIORITY` hoặc `ESCALATE`.

**Ví dụ Edge Case**
Hồ sơ `HIGH` và hồ sơ `LOW` có cùng hạn xử lý. Hồ sơ `HIGH` đứng trên trong hàng đợi, nhưng cả hai đều chuyển `OVERDUE` cùng lúc nếu chưa đóng, vì mức ưu tiên không ảnh hưởng SLA.

**Expected Result:** Mỗi hồ sơ luôn có đúng một mức ưu tiên hợp lệ, mặc định `MEDIUM`, chỉ dùng để sắp xếp và lọc.

---

### [FR-CORE-05] Danh mục loại yêu cầu và phòng ban

**Mô tả**
Hệ thống có sẵn danh mục loại yêu cầu và phòng ban, trong đó mỗi loại yêu cầu được ánh xạ tới một phòng ban tiếp nhận, để hồ sơ tự đến đúng nơi.

**Actor**
Hệ thống. Dữ liệu danh mục do đội phát triển nạp khi triển khai.

**Preconditions**
- Danh sách loại yêu cầu và phòng ban đã được khách hàng xác nhận.

**Luồng chính**
1. Khi triển khai, hệ thống nạp danh mục phòng ban, loại yêu cầu, ánh xạ loại yêu cầu tới phòng ban, và SLA ban đầu của từng loại.
2. Các module đọc danh mục để hiển thị, gán phòng ban và tính SLA.

**Input**
- **Người dùng/request:** Seed có version dữ liệu được trường xác nhận; request đọc danh mục.
- **Hệ thống:** Mã/tên VI/EN, mapping phòng, SLA đầu hoặc SLA đã có trong DB.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Danh mục hợp lệ cho UI; tên EN thiếu fallback VI kèm cảnh báo; seed lỗi không thay đổi dữ liệu đang dùng.
- **Thay đổi/sự kiện:** Seed idempotent theo mã; không xóa tham chiếu hoặc ghi đè SLA manager đã cập nhật.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Seed idempotent theo mã ổn định và một transaction; cập nhật danh mục không ghi đè SLA đã tồn tại hoặc version SLA quản lý đã sửa. SLA đầu chỉ dùng khi tạo loại mới. Nạp lỗi giữ nguyên bộ cũ; nếu chưa có bộ hợp lệ ứng dụng không phục vụ nghiệp vụ.
- Danh mục thật, mapping và SLA đầu cần trường xác nhận; tên ví dụ trong AC không phải dữ liệu production.
- Mỗi phòng ban và mỗi loại yêu cầu có mã cố định, tên tiếng Việt và tên tiếng Anh.
- Mỗi loại yêu cầu ánh xạ tới đúng một phòng ban. Một phòng ban có thể nhận nhiều loại yêu cầu.
- Không có giao diện thêm, sửa hoặc xóa danh mục. Muốn thay đổi thì đội phát triển cập nhật dữ liệu nạp sẵn và triển khai lại.
- Riêng SLA của từng loại thì Ban quản lý chỉnh được qua FR-MGT-07.
- Không xóa loại yêu cầu hay phòng ban đã có hồ sơ tham chiếu tới.
- Danh mục được cache phía server và cập nhật lại khi triển khai.

**Alternative / Error Flows**
- Nếu dữ liệu nạp có loại yêu cầu không ánh xạ phòng ban, hoặc ánh xạ tới phòng ban không tồn tại, quá trình nạp thất bại và ứng dụng không khởi động.
- Nếu dữ liệu nạp thiếu tên tiếng Anh, hệ thống dùng tên tiếng Việt và ghi log cảnh báo.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sau khi triển khai, API danh mục trả về đầy đủ loại yêu cầu kèm phòng ban tiếp nhận và tên theo ngôn ngữ người dùng.
- **AC-02 [Validation]:** Dữ liệu nạp có một loại yêu cầu không có phòng ban -> nạp thất bại, ứng dụng không khởi động, log ghi rõ loại yêu cầu bị lỗi.
- **AC-03 [Boundary]:** Một phòng ban nhận 3 loại yêu cầu -> cả 3 loại đều gán đúng về phòng ban đó.
- **AC-04 [Security]:** Gọi API thêm, sửa hoặc xóa danh mục với bất kỳ vai trò nào -> không tồn tại API này (404).
- **AC-05 [Concurrency]:** Nhiều người dùng tải danh mục cùng lúc -> dữ liệu được trả từ cache, không truy vấn cơ sở dữ liệu cho mỗi request.
- **AC-06 [Resilience]:** Seed cố xóa loại có ticket -> lỗi, giữ dữ liệu cũ. Seed lại danh mục hợp lệ không ghi đè SLA đã đổi ở MGT-07; EN thiếu dùng VI có log, không crash.

**Ví dụ Edge Case**
Sau khi go-live, trường muốn tách loại "Học vụ" thành "Đăng ký môn học" và "Xin bảo lưu". Đội phát triển thêm 2 loại mới vào dữ liệu nạp và triển khai lại. Loại "Học vụ" cũ được giữ nguyên, vì đã có hồ sơ tham chiếu.

**Expected Result:** Danh mục luôn hợp lệ: mỗi loại yêu cầu có đúng một phòng ban, có tên tiếng Việt và bản EN hoặc fallback VI đã quy định, không mất dữ liệu đang được hồ sơ sử dụng.

---

### [FR-CORE-06] Lịch sử thay đổi của hồ sơ

**Mô tả**
Hệ thống lưu dòng thời gian các thay đổi của từng hồ sơ, để sinh viên theo dõi tiến độ và nhân viên nắm được quá trình xử lý.

**Actor**
Hệ thống (ghi tự động). Người đọc là sinh viên (FR-STU-07) và nhân viên (FR-STF-02).

**Preconditions**
- Hồ sơ đã tồn tại.

**Luồng chính**
1. Một thao tác làm thay đổi hồ sơ được thực hiện.
2. Hệ thống tạo một mốc lịch sử trong cùng transaction.
3. Mỗi mốc được gắn mức hiển thị: công khai (sinh viên đọc được) hoặc nội bộ.
4. Khi đọc, hệ thống lọc các mốc theo vai trò người xem.

**Input**
- **Người dùng/request:** event_id của hành động, ticket_id, actor và dữ liệu thay đổi.
- **Hệ thống:** Vai trò người đọc, phòng hiện tại và phòng ở thời điểm sự kiện.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Public projection cho chủ ticket; public+internal cho staff phòng hiện tại; không có API manager đọc chi tiết history.
- **Thay đổi/sự kiện:** Ghi các mốc theo quy tắc gộp/tách; append-only; không nhúng due_at/assignee/reason nội bộ vào response public.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Gộp/tách mốc: CREATE một mốc public; ASSIGN lần đầu có mốc ASSIGN nội bộ và STATUS_IN_PROGRESS public; phân công lại chỉ ASSIGN nội bộ; REQUEST_INFO một mốc public gộp trạng thái PENDING nếu có; SUPPLEMENT một mốc public gộp IN_PROGRESS nếu có, cộng thêm DUE_DATE_CHANGE nội bộ; CLOSE một mốc public gộp CLOSED/resolution, cộng DUE_DATE_CHANGE nội bộ nếu kết thúc pause; OVERDUE một mốc public; PRIORITY/ESCALATE/NOTE nội bộ. TRANSFER có public tên phòng mới và internal reason cùng event, không phát hai dòng public trùng nhau.
- Mỗi event giữ actor_id nội bộ và department_at_event; projection public thay actor staff bằng tên phòng tại thời điểm sự kiện, không lấy phòng hiện tại để đổi ngược lịch sử.
- Public projection chỉ whitelist dữ liệu công khai; loại bỏ due_at, priority, assignee_id/tên staff và reason khỏi cả old/new payload, kể cả event CREATE. Điều kiện phòng hiện tại kiểm tra ở server mỗi request.
- Các mốc **công khai** (sinh viên thấy được):
  - Tạo hồ sơ.
  - Đổi trạng thái.
  - Chuyển phòng ban (chỉ hiện tên phòng ban mới, không hiện lý do).
  - Yêu cầu bổ sung (kèm nội dung yêu cầu).
  - Sinh viên bổ sung.
  - Đóng hồ sơ (kèm kết quả xử lý).
- Các mốc **nội bộ** (chỉ nhân viên phòng ban đang giữ hồ sơ thấy):
  - Phân công.
  - Đổi mức ưu tiên.
  - Escalate.
  - Lý do chuyển phòng ban.
  - Ghi nhật ký xử lý.
  - Thay đổi hạn xử lý.
- Mỗi mốc gồm: thời điểm, loại sự kiện, người thực hiện (hoặc `SYSTEM`), giá trị cũ và mới, mức hiển thị.
- Với mốc công khai, tên người thực hiện là nhân viên được thay bằng tên phòng ban.
- Lịch sử chỉ được thêm mới, không sửa và không xóa. Sắp xếp theo thời gian tăng dần.
- **Khác với nhật ký hệ thống (FR-SEC-06):** lịch sử hồ sơ phục vụ hiển thị nghiệp vụ trên giao diện. Nhật ký hệ thống phục vụ truy vết bảo mật và không có màn hình xem.

**Alternative / Error Flows**
- Nếu ghi lịch sử lỗi, hệ thống rollback thao tác gốc và trả 500.
- Nếu đọc lịch sử lỗi, màn hình gọi tới vẫn hiển thị thông tin chính của hồ sơ (theo FR-STU-07 và FR-STF-02).

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Hồ sơ qua tạo, phân công lần đầu, yêu cầu bổ sung, bổ sung và đóng: public timeline có 5 mốc CREATE/STATUS_IN_PROGRESS/REQUEST_INFO/SUPPLEMENT/CLOSE; staff thấy thêm ASSIGN và DUE_DATE_CHANGE nếu có cộng bù. Public mốc đổi IN_PROGRESS không tiết lộ assignee. Mỗi event/action chỉ xuất hiện một lần.
- **AC-02 [Validation]:** Mốc "Yêu cầu bổ sung" trả về cho sinh viên có nội dung yêu cầu, nhưng không có tên nhân viên.
- **AC-03 [Boundary]:** Hồ sơ mới tạo -> một event CREATE có public mốc Tạo hồ sơ với phòng ban, mã, ngày gửi; hạn ban đầu chỉ nằm ở dữ liệu nội bộ của event và không trả cho sinh viên.
- **AC-04 [Security]:** API lịch sử cho sinh viên không trả về mốc nội bộ và không trả về lý do chuyển phòng ban.
- **AC-05 [Concurrency]:** Hai thao tác trên cùng hồ sơ được xử lý nối tiếp -> các mốc được ghi đúng thứ tự thời gian, không mất mốc nào.
- **AC-06 [Resilience & Audit]:** Ghi lịch sử lỗi -> thao tác gốc bị rollback, hồ sơ không đổi. Không có API nào sửa hoặc xóa mốc lịch sử.

**Ví dụ Edge Case**
Hồ sơ bị chuyển từ phòng A sang phòng B với lý do "Sinh viên chọn sai loại yêu cầu". Sinh viên thấy mốc "Hồ sơ được chuyển đến Phòng B". Nhân viên phòng B thấy cả lý do chuyển. Lý do này không bao giờ xuất hiện trong dữ liệu trả về cho sinh viên.

**Expected Result:** Mỗi sự kiện có các mốc theo bảng quy ước gộp/tách, không trùng cùng event; từng người chỉ nhận projection phù hợp với quyền của mình.

---

### [FR-CORE-07] Giới hạn tệp đính kèm

**Mô tả**
Định nghĩa một bộ quy tắc tệp dùng chung cho mọi nơi tải tệp lên, để việc kiểm tra thống nhất và bảo vệ được hệ thống.

**Actor**
Hệ thống (được dùng bởi FR-STU-04 và FR-STU-09).

**Preconditions**
- Có request tải tệp lên.

**Luồng chính**
1. Giao diện kiểm tra sơ bộ định dạng, dung lượng và số lượng tệp trước khi gửi.
2. Backend kiểm tra lại toàn bộ quy tắc.
3. Tệp hợp lệ được chuyển sang lưu trữ theo FR-SEC-05.

**Input**
- **Người dùng/request:** Files của tạo hồ sơ hoặc bổ sung.
- **Hệ thống:** Whitelist PDF/JPG/PNG, 5 tệp/lần, 10.485.760 byte/tệp và quy tắc tên.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Metadata chuẩn hóa hoặc 400/413/415; file rỗng bị 400.
- **Thay đổi/sự kiện:** Backend kiểm tra độc lập UI; dữ liệu lưu tạm và dọn bù khi lỗi.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- 10MB trong tài liệu này = 10 MiB = 10.485.760 byte. File rỗng không hợp lệ (400). Giới hạn tổng request multipart ở gateway là 55 MiB, đủ 5 tệp tối đa và phần metadata; vượt tổng trả 413. UI dùng cùng giới hạn byte.
- Lưu tạm/commit/xóa bù theo quy tắc chung tại M4/M5; nếu dọn tệp thất bại thì log mã tham chiếu và job dọn tạm xử lý lại, không báo hồ sơ thành công khi DB chưa commit.
- Định dạng cho phép: PDF, JPG, PNG. Xác minh bằng nội dung tệp (magic bytes), không chỉ dựa vào đuôi tệp hay `Content-Type` do client gửi.
- Mỗi tệp tối đa 10MB. Mỗi lần gửi tối đa 5 tệp. Không đính kèm tệp nào cũng hợp lệ.
- Tên gốc của tệp tối đa 255 ký tự. Ký tự đường dẫn (`/`, `\`, `..`) bị loại bỏ trước khi lưu tên gốc.
- Các giá trị giới hạn được khai báo ở một chỗ duy nhất trong cấu hình và dùng chung cho cả frontend lẫn backend.
- Kiểm tra ở giao diện chỉ để báo lỗi sớm. Backend luôn kiểm tra lại.

**Alternative / Error Flows**
- Nếu tệp vượt 10MB, hệ thống trả 413.
- Nếu tệp sai định dạng, hệ thống trả 415.
- Nếu có hơn 5 tệp, hệ thống trả 400.
- Nếu tên tệp dài hơn 255 ký tự, hệ thống trả 400.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Gửi 1 PDF, 1 JPG, 1 PNG, mỗi tệp 2MB -> cả 3 được chấp nhận.
- **AC-02 [Validation]:** Tệp `.docx` -> 415. Tệp PNG đổi đuôi thành `.pdf` -> chấp nhận và lưu với loại PNG đúng theo nội dung.
- **AC-03 [Boundary]:** Tệp đúng 10MB -> chấp nhận. Tệp 10MB + 1 byte -> 413. Gửi đúng 5 tệp -> chấp nhận. Gửi 6 tệp -> 400.
- **AC-04 [Security]:** Tên tệp `../../etc/passwd.pdf` -> các ký tự đường dẫn bị loại bỏ; tệp vẫn lưu với tên ngẫu nhiên. Gọi thẳng API bỏ qua bước kiểm tra ở giao diện -> backend vẫn chặn đúng.
- **AC-05 [Concurrency]:** Nhiều sinh viên tải tệp 10MB cùng lúc -> mỗi request được kiểm tra độc lập, không ảnh hưởng nhau.
- **AC-06 [Resilience]:** Request ngắt trước commit -> không ticket/bổ sung thành công, tệp tạm không tải được và dọn bù; mất response sau commit -> key trả kết quả cũ. File rỗng 400; 5×10 MiB hợp lệ trong body limit 55 MiB.

**Ví dụ Edge Case**
Sinh viên chọn 5 tệp trên điện thoại, trong đó có một ảnh 12MB. Giao diện báo lỗi ngay tệp vượt dung lượng trước khi gửi, và giữ lại 4 tệp hợp lệ để sinh viên thay tệp lỗi, không phải chọn lại từ đầu.

**Expected Result:** Chỉ tệp đúng định dạng và đúng giới hạn mới được lưu, và quy tắc kiểm tra giống nhau ở mọi nơi tải tệp.

---

### [FR-CORE-08] Giao diện song ngữ VN/EN

**Mô tả**
Toàn bộ giao diện hỗ trợ tiếng Việt và tiếng Anh, để sinh viên và nhân viên không nói tiếng Việt vẫn dùng được hệ thống.

**Actor**
Mọi người dùng.

**Preconditions**
- Không có.

**Luồng chính**
1. Người dùng mở hệ thống. Giao diện hiển thị theo ngôn ngữ đã lưu của tài khoản, hoặc tiếng Việt nếu chưa có.
2. Người dùng bấm nút chuyển ngôn ngữ.
3. Hệ thống đổi ngay toàn bộ nhãn, danh mục, thông báo và thông điệp lỗi sang ngôn ngữ mới.
4. Hệ thống lưu lựa chọn vào tài khoản.

**Input**
- **Người dùng/request:** Chọn vi hoặc en; lưu lựa chọn có phiên; header ngôn ngữ trước login.
- **Hệ thống:** Sở thích tài khoản hoặc sở thích phiên trình duyệt trước login; bản dịch.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** UI/error messages/thông báo đúng ngôn ngữ; API lỗi luôn có error_code cố định; nội dung người dùng không dịch.
- **Thay đổi/sự kiện:** Lưu theo tài khoản nếu có phiên, lỗi lưu chỉ ảnh hưởng lần đăng nhập sau; đổi ngôn ngữ không mất form/files.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Trước login, dùng lựa chọn phiên trình duyệt, mặc định vi; không có account để lưu. Sau login ưu tiên lựa chọn tài khoản. API lưu preference chỉ cho sửa tài khoản của phiên, body language=vi/en, lỗi input 400; thao tác UI bỏ qua lang query lạ và dùng mặc định.
- Nếu lưu lỗi thì giữ lựa chọn cục bộ trong phiên và thông báo nhẹ; lần sau dùng giá trị account đã lưu. Định dạng thời gian UTC+7 dd/MM/yyyy HH:mm ở cả VN/EN.
- Ngôn ngữ mặc định: tiếng Việt.
- Những gì được dịch: nhãn giao diện, tên danh mục (loại yêu cầu, phòng ban, trạng thái, mức ưu tiên), nội dung thông báo trong app, thông điệp lỗi.
- Nội dung do người dùng nhập (mô tả, ghi chú, nhận xét, kết quả xử lý) không được dịch, hiển thị đúng như khi nhập.
- API trả lỗi gồm `error_code` cố định và `message` theo ngôn ngữ của người dùng.
- Thông báo lưu dạng `event_type` kèm tham số, nên khi đổi ngôn ngữ thì cả thông báo cũ cũng hiển thị theo ngôn ngữ mới.
- Định dạng ngày giờ: `dd/MM/yyyy HH:mm` cho cả hai ngôn ngữ.
- Chuyển ngôn ngữ không làm mất dữ liệu đang nhập trên form.

**Alternative / Error Flows**
- Nếu thiếu bản dịch tiếng Anh của một nhãn, hệ thống hiển thị bản tiếng Việt và ghi log cảnh báo.
- Nếu lưu lựa chọn ngôn ngữ lỗi, giao diện vẫn đổi ngôn ngữ trong phiên hiện tại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Người dùng chuyển sang EN -> toàn bộ nhãn, trạng thái, tên loại yêu cầu và thông báo hiển thị tiếng Anh. Lần đăng nhập sau giao diện vẫn là tiếng Anh.
- **AC-02 [Validation]:** Mô tả yêu cầu nhập bằng tiếng Việt -> vẫn hiển thị tiếng Việt khi giao diện ở EN.
- **AC-03 [Boundary]:** Thiếu bản dịch tiếng Anh của một nhãn -> hiển thị bản tiếng Việt, không hiển thị mã khóa thô (ví dụ `ticket.status.new`).
- **AC-04 [Security]:** Tham số ngôn ngữ không hợp lệ (ví dụ `lang=<script>`) -> hệ thống bỏ qua và dùng ngôn ngữ mặc định.
- **AC-05 [Concurrency]:** Đổi ngôn ngữ khi đang điền form tạo yêu cầu -> nội dung đã nhập và tệp đã chọn được giữ nguyên.
- **AC-06 [Resilience]:** Lưu lựa chọn ngôn ngữ lỗi -> giao diện vẫn đổi ngôn ngữ cho phiên hiện tại, không báo lỗi chặn người dùng.

**Ví dụ Edge Case**
Một sinh viên quốc tế nhận thông báo "Hồ sơ cần bổ sung thông tin" lúc giao diện đang là tiếng Việt, sau đó chuyển sang EN. Thông báo cũ hiển thị lại là "Your request needs additional information", vì thông báo được dựng lại theo ngôn ngữ hiện tại.

**Expected Result:** Mọi thành phần do hệ thống tạo ra hiển thị đúng ngôn ngữ người dùng chọn, còn nội dung người dùng nhập được giữ nguyên.

---

### [FR-CORE-09] Giao diện responsive cho mobile và desktop

**Mô tả**
Giao diện dùng tốt trên cả điện thoại và máy tính, kể cả với người dùng không thạo công nghệ, vì nhiều sinh viên chủ yếu dùng điện thoại.

**Actor**
Mọi người dùng.

**Preconditions**
- Người dùng dùng trình duyệt nằm trong danh sách hỗ trợ.

**Luồng chính**
1. Người dùng mở hệ thống trên thiết bị bất kỳ.
2. Giao diện tự điều chỉnh bố cục theo độ rộng màn hình.
3. Người dùng thực hiện đầy đủ chức năng của vai trò mình trên thiết bị đó.

**Input**
- **Người dùng/request:** Kích thước màn hình, trình duyệt, thao tác của vai trò.
- **Hệ thống:** Breakpoints 360/768/1280 và danh sách trình duyệt kiểm thử đã ghi phiên bản.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Giao diện có đủ chức năng, không cuộn ngang toàn trang ở 360px; touch targets ≥44×44px.
- **Thay đổi/sự kiện:** Chỉ thay layout; quyền backend và idempotency không đổi.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Độ rộng hỗ trợ: từ 360px (điện thoại) trở lên. Các mốc bố cục: dưới 768px (điện thoại), 768–1279px (máy tính bảng), từ 1280px trở lên (máy tính).
- Trình duyệt hỗ trợ: 2 phiên bản mới nhất của Chrome, Safari, Edge và Firefox.
- Ở độ rộng 360px không có thanh cuộn ngang. Bảng dữ liệu chuyển sang dạng thẻ trên điện thoại.
- Vùng bấm (nút, liên kết) tối thiểu 44×44px.
- Trên điện thoại, nút đính kèm tệp cho phép chọn ảnh từ thư viện hoặc chụp bằng camera.
- Mọi chức năng của các vai trò đều dùng được trên điện thoại. Việc tối ưu bố cục ưu tiên Cổng Sinh viên.

**Alternative / Error Flows**
- Nếu trình duyệt không nằm trong danh sách hỗ trợ, hệ thống vẫn cho dùng nhưng hiển thị thông báo khuyến nghị đổi trình duyệt.
- Nếu mạng chậm hoặc mất kết nối khi gửi form, giao diện giữ nguyên nội dung đã nhập và hiển thị nút Thử lại.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên dùng điện thoại tạo yêu cầu, đính kèm ảnh chụp từ camera và gửi thành công, không cần phóng to màn hình.
- **AC-02 [Validation]:** Mở mọi màn hình ở độ rộng 360px -> không có thanh cuộn ngang, không có chữ hoặc nút bị cắt.
- **AC-03 [Boundary]:** Ở độ rộng 767px hiển thị bố cục điện thoại; ở 768px chuyển sang bố cục máy tính bảng. Hàng đợi của nhân viên ở dạng thẻ trên điện thoại, dạng bảng trên máy tính.
- **AC-04 [Security]:** Trên mọi kích thước màn hình, menu chỉ hiển thị chức năng thuộc vai trò hiện tại.
- **AC-05 [Concurrency]:** Xoay điện thoại từ dọc sang ngang khi đang điền form -> nội dung đã nhập và tệp đã chọn được giữ nguyên.
- **AC-06 [Resilience]:** Mất mạng khi bấm Gửi trên điện thoại -> form giữ nguyên dữ liệu, hiển thị thông báo mất kết nối và nút Thử lại; khi gửi lại dùng cùng `Idempotency-Key` nên không tạo hồ sơ trùng.

**Ví dụ Edge Case**
Một sinh viên dùng điện thoại Android cũ với màn hình 360px và chữ hệ thống đặt cỡ lớn. Các nút chính vẫn hiển thị đầy đủ và bấm được, nhãn dài tự xuống dòng thay vì bị cắt chữ.

**Expected Result:** Mọi người dùng hoàn thành được chức năng của vai trò mình trên cả điện thoại và máy tính, không bị vỡ bố cục hay mất dữ liệu đang nhập.