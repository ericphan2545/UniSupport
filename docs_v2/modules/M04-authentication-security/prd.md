# M4. Xác thực & Bảo mật – Functional Requirement Specification

> Cập nhật: 09/10/2026. Giữ nguyên 42 chức năng của 5 module trong repo; bổ sung và thống nhất đặc tả trực tiếp tại module. Các giả định chưa được trường xác nhận được ghi tại phần đầu module.

**Mục đích module:** SSO, phiên ứng dụng, phân quyền, bảo vệ tệp và audit.

**Actor chính:** Mọi vai trò và hệ thống. Actor là người dùng/hệ thống, không phải tên người lập trình.

**Phụ thuộc:** Nhà cung cấp SSO, kho phiên, tài khoản/phòng ban và cơ sở dữ liệu.

## Luồng xác thực và liên kết module

Trường/DevOps xác nhận SSO và nạp MANAGER đầu → manager cấp quyền staff → người dùng login → xác minh danh tính trường → tìm account hoặc tạo STUDENT → kiểm tra active → phát cookie phiên ứng dụng → đưa vào M1/M2/M3. Mỗi request đọc quyền hiện tại, kiểm tra phạm vi rồi mới ghi/đọc dữ liệu. Logout thu hồi phiên hiện tại khi server xác nhận thành công.

**Input module:** callback SSO, cookie phiên, đối tượng/hành động và policy endpoint.
**Output module:** phiên ứng dụng hoặc lỗi, quyết định quyền/phạm vi, stream tệp có quyền và audit. Token SSO không trả browser.

**Cần xác nhận trước tích hợp:** giao thức thật OIDC/SAML, issuer/tenant/domain/client/redirect, định danh/email đã xác minh, Userinfo, sandbox/logout và người quản lý bootstrap. Luồng authorization code hiện tại là giả định thiết kế; nếu trường dùng giao thức khác phải sửa SEC-01. Không dùng email ví dụ hoặc bật mock production.

**Thông tin vận hành cần chốt:** quyền truy vấn audit, thời gian lưu/backup và người tiếp nhận. Không tự thêm màn hình audit hoặc tính năng xóa dữ liệu.

## Quy tắc quyền, bảo mật và mã lỗi dùng cho M1–M5

### Phiên và quyền

- JWT phiên ứng dụng do UniSupport phát sau SSO, lưu cookie HttpOnly/Secure/SameSite=Lax, TTL 8 giờ không gia hạn; SSO token chỉ server. Không bắt Authorization header với cookie hợp lệ.
- Session ID/thu hồi được kiểm tra phía server; DB role/department/active đọc mỗi request. Không thể xác minh phiên/quyền trả 503, không mặc định cho phép.
- Kiểm tra theo thứ tự: phiên → role/active → scope đối tượng → quyền thao tác → kết quả idempotency đã lưu → version/trạng thái/validation nghiệp vụ → cập nhật nguyên tử. Validate cú pháp input có thể sớm, nhưng không tiết lộ dữ liệu ngoài scope.
- Sau transfer, staff phòng cũ nhận 404 trước version. Cùng phòng có quyền xem nhưng không quyền xử lý nhận 403. Filter phòng manager và đích transfer là input nghiệp vụ, không dùng xác định phòng caller.
- API public được khai báo rõ: login/callback/tài nguyên login. Logout xử lý cookie hết hạn theo SEC-02. API khác không khai báo policy thì từ chối.
- Request ghi cookie phải kiểm tra CSRF token phiên và Origin; API ghi không dùng GET. Callback SSO kiểm tra state/giao thức theo SEC-01.
- Staff account bị deactivate/đổi role/phòng không được giữ assignee ticket mở. Phân công và cập nhật quyền bảo vệ ràng buộc cùng transaction/khóa phù hợp; Lead chọn cách triển khai và có test race.

### History, audit và notification

- History là nội dung nghiệp vụ theo CORE-06; audit là mã tham chiếu/đổi trường whitelist, không có mô tả/note/comment/reason nhạy cảm. Hai bảng khác mục đích, không đồng nhất tên gọi.
- Public timeline lọc whitelist cả old/new: không due_at/priority/assignee/tên staff/reason. Thông tin staff public thay bằng department_at_event. Staff phòng hiện tại mới đọc internal; manager không đọc chi tiết.
- Audit DB nghiệp vụ cùng transaction; LOGIN_FAILED transaction độc lập; DOWNLOAD_FILE là khởi tạo tải được cấp quyền, FILE_READ_RESULT phân biệt kho đọc thành công/lỗi. Không cam kết client nhận đủ bytes chỉ bằng audit server.
- Audit append-only; unique event/action/object tránh duplicate. Log vận hành không lưu cookie/token/nội dung nhạy cảm; record timestamp UTC, truy vấn hiển thị UTC+7.
- Mỗi event cần báo tối đa một notification student. Poll 30 giây chỉ khi trang đang mở/phiên hợp lệ; lỗi giữ giá trị cũ có chỉ báo stale và retry, không tạo retry dồn vô hạn. Khi hết phiên dừng polling/chuyển login.

### Mã lỗi và UI

Response lỗi có error_code cố định, message VN/EN, field_errors nếu cần, correlation_id; không stack trace/SQL/path kho/token.

| HTTP | Ý nghĩa |
|---|---|
| 400 | Input/cú pháp/giới hạn hoặc cấu hình input sai |
| 401 | Thiếu/sai/hết hạn/thu hồi phiên |
| 403 | Sai role, inactive, CSRF hoặc trong scope xem nhưng không có quyền ghi |
| 404 | Đối tượng không tồn tại hoặc ngoài scope; cùng response chung |
| 409 | Version/status/unique/idempotency conflict |
| 413 / 415 | Quá dung lượng / định dạng tệp |
| 422 | Mapping/SLA cấu hình nghiệp vụ thiếu, người nhận/phòng đích không hợp lệ theo FR |
| 429 | Rate limit, có Retry-After |
| 500 | Lỗi DB/ghi nghiệp vụ chưa commit |
| 502 | Payload Userinfo SSO thiếu trường bắt buộc/không hợp lệ |
| 503 | Dịch vụ/storage/quyền tạm lỗi hoặc timeout query theo FR |
| 504 | Userinfo vượt timeout 3 giây theo STU-01 |

Tạo lỗi giữ form/files còn trong browser để thử lại; không giữ nội dung nhạy cảm lâu dài ở localStorage. Đọc lỗi hiện retry/stale; không trắng trang. Rate limit theo người dùng và endpoint/group được tài liệu hóa; callback SSO 10/phút/IP chỉ là ngưỡng đề xuất cần kiểm chứng mạng trường dùng NAT (ngưỡng cần kiểm chứng với hạ tầng trường), không tùy ý đổi mà không ghi lại.

### Thuật ngữ nhanh

| Từ | Nghĩa trong tài liệu |
|---|---|
| SSO / Userinfo | Dịch vụ đăng nhập trường / dữ liệu hồ sơ tài khoản mà dịch vụ cung cấp |
| JWT / cookie HttpOnly | Chuỗi phiên có chữ ký / cookie không đọc bằng JavaScript; server vẫn phải kiểm tra thu hồi/quyền |
| Snapshot | Bản lưu dữ liệu tại một thời điểm, không cập nhật ngược khi nguồn thay đổi |
| Idempotency-Key | Mã cho một thao tác để thử lại không thực hiện lần hai |
| Version / optimistic locking | Số phiên bản; chỉ lưu nếu dữ liệu chưa đổi kể từ lúc người dùng đọc |
| Atomic / transaction | Một nhóm thay đổi DB commit cùng nhau hoặc không commit; không tự bao gồm object storage/mạng |
| Projection / whitelist | Chỉ dựng response bằng các trường được phép trả cho người xem |
| Audit / history | Nhật ký bảo mật bằng tham chiếu / dòng thời gian nghiệp vụ cho giao diện |
| Cron / polling | Tác vụ server chạy định kỳ / browser hỏi dữ liệu định kỳ |
| p95 | 95% request có thời gian phản hồi không vượt giá trị này |
| No-op | Lưu cùng dữ liệu hiện tại, không có thay đổi mới |
| CSRF / IDOR | Request ghi giả mạo từ nguồn khác / truy cập đối tượng bằng cách đổi ID ngoài quyền |
| TTL / RPO / RTO | Thời gian tồn tại / mức mất dữ liệu cho phép / thời gian khôi phục mục tiêu |

## Quy ước chung (áp dụng cho mọi FR trong module)

- **Kết nối:** Môi trường thực tế chỉ phục vụ qua HTTPS (chứng chỉ SSL do nhà trường cung cấp). Request HTTP được chuyển hướng sang HTTPS.
- **Phiên làm việc:** Sau khi đăng nhập SSO, hệ thống phát JWT phiên làm việc của ứng dụng, lưu trong cookie `HttpOnly`, `Secure`, `SameSite=Lax`. Access token của SSO chỉ lưu phía server (để gọi Userinfo ở FR-STU-01), không gửi xuống trình duyệt.
- **Vai trò:** Mỗi tài khoản có đúng một vai trò: `STUDENT`, `STAFF` hoặc `MANAGER`. Vai trò và phòng ban được đọc từ cơ sở dữ liệu ở mỗi request.
- **Mặc định từ chối:** API nào không khai báo vai trò được phép thì mặc định từ chối truy cập.
- **Mã lỗi phân quyền:** Sai hoặc thiếu phiên trả 401. Sai vai trò trả 403. Đối tượng nằm ngoài phạm vi dữ liệu được phép trả 404 (không tiết lộ đối tượng có tồn tại hay không).
- **Nhật ký:** Mọi bản ghi nhật ký tuân theo FR-SEC-06.

---

### [FR-SEC-01] Đăng nhập bằng SSO tài khoản trường

**Mô tả**
Người dùng đăng nhập bằng tài khoản trường cấp thông qua SSO, không cần tạo mật khẩu riêng, và được đưa vào đúng cổng theo vai trò.

**Actor**
Người dùng chưa đăng nhập (sinh viên, nhân viên, Ban quản lý).

**Preconditions**
- Hệ thống đã được cấu hình kết nối với SSO của trường. Ở môi trường dev và test thì dùng SSO giả lập.
- Người dùng có tài khoản SSO đang hoạt động.

**Luồng chính**
1. Người dùng bấm "Đăng nhập bằng tài khoản trường".
2. Hệ thống tạo tham số `state` ngẫu nhiên và chuyển hướng sang trang đăng nhập SSO.
3. Người dùng đăng nhập trên trang SSO.
4. SSO chuyển hướng về hệ thống kèm mã xác thực.
5. Hệ thống kiểm tra `state`, đổi mã xác thực lấy token, xác minh token và lấy định danh người dùng.
6. Hệ thống tìm tài khoản theo định danh SSO:
   - Nếu tài khoản đã có: dùng vai trò hiện có.
   - Nếu chưa có: tạo tài khoản mới với vai trò `STUDENT`.
7. Hệ thống kiểm tra tài khoản có bị vô hiệu hóa không.
8. Hệ thống tạo phiên làm việc, ghi nhật ký và chuyển người dùng đến cổng theo vai trò: `STUDENT` vào Cổng Sinh viên, `STAFF` vào Cổng Nhân viên, `MANAGER` vào Cổng Quản lý.

**Input**
- **Người dùng/request:** Bấm login; callback code/state theo giao thức SSO đã cấu hình.
- **Hệ thống:** SSO trường đã xác nhận; định danh xác thực; tài khoản DB và trạng thái active.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Phiên JWT ứng dụng trong cookie HttpOnly/Secure/SameSite=Lax và redirect đúng cổng; token SSO chỉ server. Lỗi không tạo phiên.
- **Thay đổi/sự kiện:** Tạo STUDENT lần đầu hoặc liên kết tài khoản đã cấp quyền trước; audit login; định danh MANAGER đầu do triển khai nạp.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Phương án hiện tại là SSO theo luồng authorization code; giả định OIDC cần xác nhận với trường. Trước tích hợp thật phải chốt issuer, client/redirect, khóa xác minh, audience, định danh ổn định, email đã được nhà cung cấp xác thực và domain/tenant được phép. Nếu không đủ định danh thì từ chối tạo tài khoản/phiên và log mã lý do, không ghi token. Các kiểm tra PKCE/nonce theo đặc tả giao thức nằm trong thiết kế tích hợp.
- Khi triển khai, DevOps nạp một định danh MANAGER đã được trường xác nhận; thao tác idempotent, không ghi đè quyền người dùng đã có. Mock luôn tắt ở production.
- Định danh SSO (email trường) là khóa nhận diện tài khoản. Tài khoản `STAFF` và `MANAGER` phải được Ban quản lý thêm trước (FR-MGT-06).
- Tham số `state` dùng một lần, hết hạn sau 10 phút.
- Phiên làm việc hết hạn sau 8 giờ, không gia hạn. Hết phiên thì người dùng phải đăng nhập lại.
- SSO giả lập chỉ bật được ở môi trường dev và test. Nếu cấu hình bật SSO giả lập ở môi trường production, ứng dụng từ chối khởi động.
- Rate limit callback đăng nhập: 10 lần mỗi phút cho mỗi IP.
- Tài khoản `STAFF` chưa có phòng ban vẫn đăng nhập được, nhưng Cổng Nhân viên hiển thị thông báo "Tài khoản chưa được gán phòng ban" (theo quy ước của M2).

**Alternative / Error Flows**
- Nếu người dùng hủy đăng nhập trên trang SSO, hệ thống quay về trang đăng nhập với thông báo "Bạn đã hủy đăng nhập".
- Nếu `state` không khớp, đã dùng hoặc đã hết hạn, hệ thống trả 400 và không tạo phiên.
- Nếu token từ SSO không hợp lệ (sai chữ ký, sai issuer, hết hạn), hệ thống trả 401.
- Nếu tài khoản bị vô hiệu hóa, hệ thống trả 403 "Tài khoản đã bị vô hiệu hóa, vui lòng liên hệ nhà trường".
- Nếu SSO không phản hồi trong 5 giây, hệ thống trả 503 "Hệ thống đăng nhập của trường đang gián đoạn".
- Nếu vượt rate limit, hệ thống trả 429.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên đăng nhập SSO lần đầu -> tài khoản `STUDENT` được tạo, người dùng vào Cổng Sinh viên. Nhân viên đã được thêm ở FR-MGT-06 đăng nhập -> vào Cổng Nhân viên.
- **AC-02 [Validation]:** Callback có `state` đã được dùng trước đó -> 400, không có phiên mới. Token SSO sai chữ ký -> 401.
- **AC-03 [Boundary]:** Phiên được tạo lúc 08:00 -> request lúc 15:59 thành công; request lúc 16:01 nhận 401 và người dùng được chuyển về trang đăng nhập.
- **AC-04 [Security]:** Tài khoản bị vô hiệu hóa đăng nhập -> 403, không tạo phiên. Cookie phiên có đủ các cờ `HttpOnly`, `Secure`, `SameSite=Lax`. Response trả về trình duyệt không chứa access token của SSO.
- **AC-05 [Rate Limit]:** Lần gọi callback thứ 11 trong 1 phút từ cùng một IP -> 429.
- **AC-06 [Resilience & Audit]:** SSO không phản hồi trong 5 giây -> 503 với thông báo thân thiện, ứng dụng không treo. Đăng nhập thành công -> có bản ghi `LOGIN_SUCCESS`. Đăng nhập thất bại -> có bản ghi `LOGIN_FAILED` kèm lý do.

**Ví dụ Edge Case**
Một nhân viên mới đăng nhập trước khi Ban quản lý kịp thêm tài khoản của họ, nên hệ thống tạo tài khoản với vai trò `STUDENT`. Sau đó Ban quản lý đổi vai trò tài khoản này thành `STAFF` và gán phòng ban (FR-MGT-06). Ở request kế tiếp, người này truy cập được Cổng Nhân viên mà không cần đăng nhập lại, vì vai trò được đọc từ cơ sở dữ liệu ở mỗi request.

**Expected Result:** Chỉ người có tài khoản SSO hợp lệ và chưa bị vô hiệu hóa mới tạo được phiên, và mỗi người được đưa vào đúng cổng theo vai trò.

---

### [FR-SEC-02] Đăng xuất

**Mô tả**
Người dùng kết thúc phiên làm việc để người khác dùng chung thiết bị không truy cập được vào tài khoản.

**Actor**
Người dùng đã đăng nhập (mọi vai trò).

**Preconditions**
- Người dùng có phiên làm việc (có thể đã hết hạn).

**Luồng chính**
1. Người dùng bấm Đăng xuất.
2. Hệ thống thu hồi phiên phía server và xóa cookie phiên.
3. Hệ thống xóa access token SSO đã lưu phía server cho phiên này.
4. Hệ thống chuyển hướng sang trang kết thúc phiên của SSO (nếu SSO hỗ trợ), sau đó quay về trang đăng nhập.

**Input**
- **Người dùng/request:** Lệnh logout của phiên hiện tại; không cần phiên còn hạn để xóa cookie.
- **Hệ thống:** Session ID/danh sách phiên phía server và token SSO của phiên.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200 khi không có phiên hoặc thu hồi thành công; xóa cookie, UI về login. 500 nếu thu hồi thất bại, không tuyên bố logout server đã thành công.
- **Thay đổi/sự kiện:** Thu hồi riêng phiên hiện tại; xóa token SSO/cache UI nhạy cảm; không ảnh hưởng thiết bị khác.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Trang/API chứa dữ liệu cá nhân và tệp trả Cache-Control: private, no-store; frontend xóa cache dữ liệu khi logout, kiểm tra lại phiên khi khôi phục trang từ Back/bfcache. Không thể thu hồi bản tệp người có quyền đã tải về trước đó.
- Nếu kho phiên không đọc được, API bảo vệ trả 503 (mặc định từ chối). Khi kho khôi phục, phiên chỉ bị coi là thu hồi nếu trạng thái thu hồi đã commit; không hứa lỗi logout tự vô hiệu hóa JWT.
- Phiên bị thu hồi ngay lập tức. Mọi request sau đó dùng JWT cũ đều bị từ chối, kể cả khi JWT chưa hết hạn (thu hồi bằng danh sách phiên phía server).
- Đăng xuất chỉ thu hồi phiên hiện tại, không ảnh hưởng phiên trên thiết bị khác.
- Đăng xuất là idempotent: gọi khi không có phiên hoặc phiên đã hết hạn vẫn trả thành công và chuyển về trang đăng nhập.

**Alternative / Error Flows**
- Nếu không gọi được trang kết thúc phiên của SSO, hệ thống vẫn hoàn tất đăng xuất phía ứng dụng và chuyển về trang đăng nhập.
- Nếu lưu thu hồi phiên lỗi, hệ thống xóa cookie của trình duyệt và dữ liệu UI/token SSO của phiên nếu có thể, trả 500 và ghi log kỹ thuật. Không tuyên bố JWT cũ đã bị thu hồi; chỉ 200 sau khi thu hồi xác nhận thành công. Người dùng được hướng dẫn thử lại/liên hệ hỗ trợ.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Người dùng bấm Đăng xuất -> cookie bị xóa, người dùng về trang đăng nhập.
- **AC-02 [Validation]:** Gọi đăng xuất khi không có cookie phiên -> vẫn trả 200 và chuyển về trang đăng nhập.
- **AC-03 [Security]:** Sau khi đăng xuất, gửi lại JWT cũ (chưa hết hạn) -> 401.
- **AC-04 [Idempotency]:** Bấm Đăng xuất 2 lần liên tiếp -> cả hai đều thành công, không lỗi.
- **AC-05 [Boundary]:** Người dùng đăng nhập trên 2 thiết bị, đăng xuất ở thiết bị A -> thiết bị B vẫn dùng được bình thường.
- **AC-06 [Resilience & Audit]:** SSO logout endpoint lỗi nhưng thu hồi ứng dụng thành công -> logout app vẫn 200 và có LOGOUT. Kho thu hồi lỗi -> 500, cookie bị xóa nhưng không claim JWT đã vô hiệu; API không đọc được kho phiên -> 503 mặc định từ chối.

**Ví dụ Edge Case**
Sinh viên logout được server xác nhận thành công trên máy chung. Frontend xóa cache, các trang nhạy cảm no-store và kiểm tra phiên khi Back/bfcache; JWT cũ trả 401, UI về login. Nếu thu hồi lỗi 500 thì UI không báo hoàn tất logout server và phải xử lý thử lại theo luồng lỗi.

**Expected Result:** Sau khi đăng xuất được server xác nhận thành công, phiên cũ không dùng được. Nếu thu hồi lỗi trả 500 thì không bảo đảm vô hiệu JWT cũ; xử lý theo luồng lỗi và không báo thành công giả.

---

### [FR-SEC-03] Phân quyền theo 3 vai trò

**Mô tả**
Hệ thống giới hạn chức năng mà mỗi người dùng được dùng theo vai trò, để mỗi nhóm chỉ thao tác được trong phạm vi của mình.

**Actor**
Hệ thống (áp dụng cho mọi request của mọi người dùng).

**Preconditions**
- Request có phiên làm việc hợp lệ.

**Luồng chính**
1. Hệ thống nhận request.
2. Hệ thống xác thực phiên.
3. Hệ thống đọc vai trò, phòng ban và trạng thái tài khoản từ cơ sở dữ liệu.
4. Hệ thống so vai trò với danh sách vai trò được phép của API.
5. Nếu được phép, request đi tiếp sang kiểm tra phạm vi dữ liệu (FR-SEC-04). Nếu không, hệ thống từ chối.

**Input**
- **Người dùng/request:** Request đến API; policy public/roles của endpoint.
- **Hệ thống:** Phiên hợp lệ, role/department/active từ DB mỗi request.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Cho đi tiếp nếu hợp lệ; 401 phiên lỗi, 403 role/active lỗi; 503 không kiểm tra được quyền.
- **Thay đổi/sự kiện:** Chỉ quyết định quyền; vô hiệu hóa thu hồi phiên; API public login/callback khai báo tường minh.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Login, callback và tài nguyên trang đăng nhập được khai báo public tường minh; logout cho phép xóa cookie ngay cả khi phiên hết hạn theo SEC-02. Các API nghiệp vụ còn lại mặc định từ chối nếu không có policy.
- Request ghi dùng cookie phải kiểm tra CSRF token gắn phiên và Origin theo danh sách ứng dụng; sai kiểm tra trả 403, không cập nhật. Callback SSO dùng state một lần và kiểm tra giao thức, không áp cùng CSRF token của phiên chưa có. Nội dung nhập là văn bản thuần và được encode khi render theo quy ước chung.
- Ma trận quyền theo cổng:
  - `STUDENT`: chỉ dùng Cổng Sinh viên (M1).
  - `STAFF`: chỉ dùng Cổng Nhân viên (M2).
  - `MANAGER`: chỉ dùng Cổng Quản lý (M3).
- Vai trò và phòng ban được gán qua FR-MGT-06. Tài khoản tạo lần đầu qua SSO mặc định là `STUDENT`.
- Mỗi tài khoản có đúng một vai trò.
- Thay đổi vai trò có hiệu lực ngay ở request kế tiếp.
- Tài khoản bị vô hiệu hóa thì mọi request đều bị từ chối, kể cả khi phiên chưa hết hạn.
- Giao diện chỉ hiển thị menu và nút thuộc vai trò hiện tại. Tuy vậy, việc chặn quyền luôn được thực hiện ở backend, không dựa vào giao diện.

**Alternative / Error Flows**
- Nếu không có phiên hoặc phiên không hợp lệ, hệ thống trả 401.
- Nếu vai trò không được phép dùng API, hệ thống trả 403.
- Nếu tài khoản bị vô hiệu hóa trong lúc phiên còn hiệu lực, hệ thống trả 403 và thu hồi phiên.
- Nếu không đọc được thông tin phân quyền từ cơ sở dữ liệu, hệ thống trả 503 và không cho request đi tiếp.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên gọi API tạo yêu cầu -> được phép. Nhân viên gọi API hàng đợi -> được phép. Ban quản lý gọi API dashboard -> được phép.
- **AC-02 [Validation]:** Sinh viên gọi API hàng đợi của Cổng Nhân viên -> 403. Nhân viên gọi API phân quyền -> 403.
- **AC-03 [Boundary]:** Một API mới được thêm mà quên khai báo vai trò -> mọi vai trò đều nhận 403.
- **AC-04 [Security]:** Client tự sửa giao diện để hiện nút của vai trò khác rồi gọi API -> backend vẫn trả 403.
- **AC-05 [Concurrency]:** Ban quản lý hạ một nhân viên về `STUDENT` trong lúc người đó đang mở Cổng Nhân viên -> request kế tiếp của họ nhận 403.
- **AC-06 [Resilience & Audit]:** Cơ sở dữ liệu phân quyền không phản hồi -> 503, request không được xử lý tiếp (không mặc định cho phép). Mỗi lần thay đổi vai trò -> có bản ghi `ROLE_CHANGE` (theo FR-MGT-06).

**Ví dụ Edge Case**
Staff không còn phụ trách ticket mở được manager vô hiệu hóa khi còn phiên 5 giờ và đang xem một ticket cùng phòng. Request tiếp theo đọc chi tiết trả 403, thu hồi phiên và UI về login. Không dùng ví dụ vô hiệu hóa staff còn assignee ticket mở vì MGT-06 phải chặn thao tác đó.

**Expected Result:** Mọi request chỉ được xử lý khi vai trò hiện tại của tài khoản, đọc từ cơ sở dữ liệu, cho phép dùng API đó.

---

### [FR-SEC-04] Giới hạn phạm vi dữ liệu theo vai trò

**Mô tả**
Ngoài việc được dùng chức năng nào, mỗi người dùng chỉ truy cập được đúng phần dữ liệu thuộc phạm vi của mình, để bảo vệ dữ liệu sinh viên.

**Actor**
Hệ thống (áp dụng cho mọi request đọc hoặc ghi hồ sơ, tệp và thông báo).

**Preconditions**
- Request đã qua kiểm tra vai trò (FR-SEC-03).

**Luồng chính**
1. Hệ thống xác định phạm vi dữ liệu theo vai trò và phòng ban.
2. Hệ thống thêm điều kiện phạm vi vào truy vấn dữ liệu.
3. Nếu thao tác là ghi, hệ thống kiểm tra thêm quyền xử lý.
4. Hệ thống trả kết quả nằm trong phạm vi, hoặc từ chối.

**Input**
- **Người dùng/request:** Định danh đối tượng cần đọc/ghi và hành động.
- **Hệ thống:** User/role/department từ phiên và DB; phòng/chủ hiện tại của đối tượng.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Dữ liệu nằm trong phạm vi hoặc 404 không phân biệt ngoài phạm vi/không tồn tại; 403 không có quyền xử lý dù có quyền xem.
- **Thay đổi/sự kiện:** Kiểm tra quyền trước version/trạng thái; filter department của manager chỉ giới hạn báo cáo, không tăng quyền.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Thứ tự kiểm tra theo quy tắc chung tại M4/M5: phiên → vai trò/active → phạm vi → quyền ghi → idempotency → version/trạng thái. Đối tượng chuyển khỏi phòng trả 404 trước khi xét version.
- department_id filter ở MGT báo cáo hoặc target_department_id/assignee_id của thao tác điều phối là input nghiệp vụ hợp lệ và phải validate; không dùng chúng để xác định phòng của người gọi. MGT-06 tìm email chính xác là ngoại lệ dữ liệu tài khoản tối thiểu phục vụ cấp quyền, không cấp quyền xem hồ sơ sinh viên.
- Phạm vi theo vai trò:
  - `STUDENT`: chỉ hồ sơ, tệp và thông báo của chính mình.
  - `STAFF`: xem hồ sơ của phòng ban mình. Chỉ người phụ trách được xử lý (ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ). Mọi nhân viên trong phòng ban được điều phối (phân công, đặt ưu tiên, chuyển phòng ban, escalate).
  - `MANAGER`: chỉ dữ liệu của Cổng Quản lý, gồm số liệu tổng hợp, danh sách tóm tắt hồ sơ ở FR-MGT-01 (mã, loại, phòng ban, trạng thái, ngày gửi), đánh giá và nhận xét ở FR-MGT-05, dữ liệu phân quyền và SLA. Không xem chi tiết hồ sơ, không xem thông tin cá nhân của sinh viên, không xem hoặc tải tệp, không thao tác trên hồ sơ.
- Phạm vi được áp dụng ngay trong truy vấn (điều kiện theo `student_id` hoặc `department_id`), không lọc sau khi đã lấy dữ liệu và không dựa vào giao diện.
- Định danh và phòng ban của người dùng chỉ lấy từ phiên và cơ sở dữ liệu. Mọi `student_id` hoặc `department_id` do client gửi lên đều bị bỏ qua khi xác định phạm vi.
- Đối tượng ngoài phạm vi trả 404. Đối tượng trong phạm vi xem nhưng không có quyền ghi trả 403.
- Phạm vi được đánh giá lại ở mỗi request. Hồ sơ vừa chuyển phòng ban thì phòng ban cũ mất quyền ngay lập tức.

**Alternative / Error Flows**
- Nếu đối tượng không tồn tại hoặc nằm ngoài phạm vi, hệ thống trả 404.
- Nếu người dùng nằm trong phạm vi xem nhưng không có quyền ghi, hệ thống trả 403.
- Nếu không xác định được phòng ban của `STAFF`, hệ thống trả 403.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên A mở hồ sơ của mình -> 200. Nhân viên phòng Đào tạo mở hồ sơ của phòng Đào tạo -> 200. Ban quản lý mở danh sách tóm tắt ở dashboard -> 200.
- **AC-02 [Validation]:** Sinh viên A đổi mã hồ sơ trên URL thành mã hồ sơ của sinh viên B -> 404. Nhân viên đổi mã thành hồ sơ của phòng ban khác -> 404.
- **AC-03 [Boundary]:** Nhân viên cùng phòng ban nhưng không phụ trách gọi API đóng hồ sơ -> 403. Cùng nhân viên đó gọi API phân công -> 200.
- **AC-04 [Security]:** Gửi `department_id` của phòng ban khác trong body hoặc query -> hệ thống bỏ qua. Ban quản lý gọi API chi tiết hồ sơ, API tải tệp hoặc API đóng hồ sơ -> 403. Danh sách tóm tắt và báo cáo trả cho Ban quản lý không chứa tên, MSSV, email hay SĐT của sinh viên.
- **AC-05 [Concurrency]:** Hồ sơ được chuyển từ phòng A sang phòng B -> request kế tiếp của nhân viên phòng A tới hồ sơ đó nhận 404.
- **AC-06 [Resilience & Audit]:** Mọi request bị chặn vì ngoài phạm vi -> trả 404 với cùng nội dung như khi đối tượng không tồn tại. Request ghi bị từ chối không tạo bản ghi nhật ký nghiệp vụ.

**Ví dụ Edge Case**
Một sinh viên thử đoán mã hồ sơ bằng cách tăng dần số thứ tự (USP-2026-00001, 00002, ...). Mọi hồ sơ không thuộc sinh viên đó đều trả 404 giống hệt hồ sơ không tồn tại. Đồng thời rate limit của FR-STU-07 giới hạn số lần thử.

**Expected Result:** Mỗi người dùng chỉ đọc và ghi được đúng phần dữ liệu trong phạm vi vai trò và phòng ban của mình. Thông tin cá nhân và tệp của sinh viên chỉ sinh viên đó và nhân viên phòng ban phụ trách truy cập được.

---

### [FR-SEC-05] Bảo vệ tệp đính kèm

**Mô tả**
Tệp minh chứng của sinh viên chỉ người có quyền mới mở được, không thể truy cập qua đường dẫn công khai.

**Actor**
Hệ thống (áp dụng cho mọi thao tác lưu và tải tệp).

**Preconditions**
- Tệp đã được tải lên hợp lệ (FR-STU-04, FR-STU-09).

**Luồng chính**
1. Người dùng yêu cầu xem hoặc tải một tệp.
2. Hệ thống kiểm tra phiên, vai trò và phạm vi của hồ sơ chứa tệp.
3. Hệ thống ghi audit khởi tạo tải có quyền.
4. Hệ thống đọc tệp; nếu đọc được thì stream trả về, nếu lỗi thì trả lỗi và ghi kết quả đọc gắn cùng event tham chiếu.

**Input**
- **Người dùng/request:** file_id, mode=inline/download.
- **Hệ thống:** Phiên, role, phòng/chủ hồ sơ chứa file; metadata MIME/tên xác minh.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** 200 stream tệp; Content-Type, Content-Disposition đúng mode, nosniff, Cache-Control private/no-store; không trả presigned/public URL.
- **Thay đổi/sự kiện:** Audit DOWNLOAD_FILE theo nghĩa khởi tạo tải có quyền; kết quả đọc/storage được ghi riêng theo event tham chiếu, không ghi nội dung file.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Content-Disposition dùng inline khi xem, attachment khi tải; filename được sanitize/encode header. Cache-Control private, no-store cho tệp nhạy cảm. Kiểm tra quyền tại lúc bắt đầu stream; dữ liệu đã tải trên máy người có quyền không thể thu hồi bằng đổi phòng.
- Chỉ hai nhóm được xem và tải tệp:
  - Sinh viên chủ hồ sơ.
  - Nhân viên thuộc phòng ban hiện đang giữ hồ sơ.
- Ban quản lý không có quyền với tệp.
- Tệp lưu ngoài thư mục web công khai, với tên ngẫu nhiên. Không tồn tại đường dẫn trực tiếp nào tới tệp.
- Tệp chỉ được trả qua API có kiểm tra quyền. Response có:
  - `Content-Type` theo định dạng đã xác minh lúc tải lên.
  - `Content-Disposition` với tên gốc.
  - `X-Content-Type-Options: nosniff`.
- Tệp đã tải lên không được sửa hoặc xóa qua giao diện.
- Ghi DOWNLOAD_FILE trước khi đọc/stream theo nghĩa đã cấp quyền và khởi tạo tải, result=AUTHORIZED. Nếu audit khởi tạo lỗi thì không trả tệp. Ghi FILE_READ_RESULT riêng với result=STREAM_STARTED/STORAGE_FAILED gắn download_event_id; không cam kết log chứng minh client đã nhận đủ byte. Mất kết nối client không hoàn tác audit khởi tạo.

**Alternative / Error Flows**
- Nếu không có phiên, hệ thống trả 401.
- Nếu tài khoản `MANAGER` gọi API tải tệp, hệ thống trả 403.
- Nếu tệp nằm ngoài phạm vi của người dùng, hệ thống trả 404.
- Nếu tệp không còn trên kho lưu trữ, hệ thống trả 404 và ghi log lỗi hệ thống.
- Nếu kho lưu trữ không phản hồi, hoặc ghi nhật ký lỗi, hệ thống trả 503.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Sinh viên chủ hồ sơ tải tệp -> 200, đúng tên gốc và đúng nội dung.
- **AC-02 [Validation]:** Gọi API với `file_id` không tồn tại -> 404.
- **AC-03 [Security]:** Truy cập thẳng đường dẫn lưu trữ thật của tệp trên server -> không truy cập được. Nhân viên phòng ban khác tải tệp -> 404. Tài khoản `MANAGER` tải tệp -> 403. Response có header `X-Content-Type-Options: nosniff`.
- **AC-04 [Boundary]:** Hồ sơ vừa được chuyển sang phòng ban khác -> nhân viên phòng ban mới tải tệp được, nhân viên phòng ban cũ nhận 404.
- **AC-05 [Concurrency]:** Nhiều người có quyền tải cùng một tệp cùng lúc -> tất cả nhận đúng nội dung, mỗi lượt tải có một bản ghi nhật ký riêng.
- **AC-06 [Resilience & Audit]:** Audit khởi tạo lỗi -> 503, không stream. Đọc kho thành công -> DOWNLOAD_FILE=AUTHORIZED và FILE_READ_RESULT=STREAM_STARTED; object mất/storage lỗi -> 404/503, có result lỗi theo cùng event, không claim client nhận đủ tệp.

**Ví dụ Edge Case**
Nhân viên sao chép đường dẫn tải tệp rồi gửi qua tin nhắn cho một người ngoài phòng ban. Người nhận mở đường dẫn: nếu chưa đăng nhập thì nhận 401, nếu đã đăng nhập nhưng không có quyền thì nhận 404 (hoặc 403 nếu là Ban quản lý). Đường dẫn tải tệp không mang quyền truy cập theo nó.

**Expected Result:** Tệp chỉ được trả cho chủ hồ sơ hoặc staff phòng hiện tại tại thời điểm cấp quyền; mọi lần khởi tạo tải có quyền được ghi lại, kết quả đọc kho được phân biệt với nhận đủ tệp ở client.   
---

### [FR-SEC-06] Ghi nhật ký hệ thống

**Mô tả**
Hệ thống ghi lại các thao tác quan trọng trên hồ sơ, tệp, đăng nhập và cấu hình, làm căn cứ truy vết khi có sự cố hoặc tranh chấp.

**Actor**
Hệ thống (tự động ghi khi các thao tác được thực hiện).

**Preconditions**
- Có một thao tác thuộc danh sách cần ghi nhật ký.

**Luồng chính**
1. Người dùng (hoặc hệ thống) thực hiện một thao tác.
2. Với thao tác DB nghiệp vụ, hệ thống tạo audit trong cùng transaction. Với LOGIN_FAILED hoặc đọc/stream file, dùng transaction audit riêng theo quy tắc bên dưới.
3. DB nghiệp vụ commit cùng audit/history; rollback hủy phần đó. Audit khởi tạo tải đã commit hoặc login thất bại không bị xóa chỉ vì mạng/SSO/storage lỗi sau đó.

**Input**
- **Người dùng/request:** Event nghiệp vụ/hệ thống và mã tham chiếu; không có API người dùng tạo/xóa audit.
- **Hệ thống:** Actor/role/IP/thời gian, loại đối tượng, trước/sau các trường được phép.
- Phiên, phạm vi dữ liệu, validation, version và idempotency theo [quy tắc nghiệp vụ dùng chung ở M5](../M05-core-platform/prd.md) và [quy tắc quyền/lỗi ở M4](../M04-authentication-security/prd.md). Các FR hệ thống nhận sự kiện nội bộ, không tự tạo API công khai mới.

**Output**
- **Kết quả:** Audit append-only gắn event_id; lưu timestamp UTC, hiển thị UTC+7. Người vận hành truy vấn DB theo quyền vận hành.
- **Thay đổi/sự kiện:** Cùng transaction với dữ liệu DB nghiệp vụ; login thất bại và tải tệp dùng transaction audit riêng được mô tả, không mất log do rollback nghiệp vụ.
- **Khi lỗi:** theo Alternative / Error Flows của FR và mã lỗi chung. Đọc lỗi không thay đổi dữ liệu nghiệp vụ; ghi lỗi không commit nghiệp vụ một phần. Ngoại lệ audit/tệp/đăng xuất được nêu ở SEC-02, SEC-05, SEC-06 và quy ước lưu tệp.

**Business Rules**
- Audit của ghi DB nghiệp vụ cùng transaction; LOGIN_FAILED dùng transaction audit riêng vì không có thao tác đăng nhập thành công. Audit tải tệp theo SEC-05 dùng transaction riêng, không áp lời hứa atomic DB lên mạng/kho tệp.
- Mỗi event nghiệp vụ có tối đa một bản ghi cho mỗi action; một hành động có thể có thêm UPLOAD_FILE cho từng file và DUE_DATE_CHANGE nếu cộng bù. event_id+action+object_id dùng chống trùng. Một lần REQUEST_INFO không ghi thêm STATUS_CHANGE audit giống hệt; STATUS_CHANGE dùng cho đổi do hệ thống. Cấu hình ROLE_CHANGE có subtype như MGT-06.
- Các thao tác được ghi:
  - Đăng nhập và đăng xuất: `LOGIN_SUCCESS`, `LOGIN_FAILED`, `LOGOUT`.
  - Hồ sơ: `CREATE_TICKET`, `SUPPLEMENT_SUBMITTED`, `RATE_TICKET`, `ADD_NOTE`, `REQUEST_INFO`, `CLOSE_TICKET`, `ASSIGN`, `CHANGE_PRIORITY`, `ESCALATE`, `TRANSFER`.
  - Trạng thái do hệ thống tự đổi: `STATUS_CHANGE` (ví dụ hồ sơ chuyển sang `OVERDUE`), với người thực hiện là `SYSTEM`.
  - Hạn xử lý: `DUE_DATE_CHANGE`, khi `due_at` được cộng bù lúc hồ sơ rời `PENDING_INFO` (FR-CORE-03). Người thực hiện là người gây ra sự kiện, ví dụ sinh viên bổ sung hoặc nhân viên đóng hồ sơ.
  - Tệp: UPLOAD_FILE, DOWNLOAD_FILE (khởi tạo tải có quyền), FILE_READ_RESULT (kết quả đọc kho theo download_event_id).
  - Cấu hình: `ROLE_CHANGE`, `SLA_CHANGE`.
- Mỗi bản ghi gồm: thời điểm lưu UTC, khi đọc hiển thị UTC+7, người thực hiện, vai trò, mã thao tác, loại và mã đối tượng, giá trị cũ và mới (nếu có), địa chỉ IP, kết quả.
- Nhật ký không lưu nội dung nhạy cảm như mô tả yêu cầu, nội dung ghi chú, nhận xét hay nội dung tệp. Nhật ký chỉ lưu mã tham chiếu tới các đối tượng đó; lý do chuyển/escalate cũng lưu ở lịch sử nghiệp vụ nội bộ, audit không nhúng reason. Mã subtype cấu hình thuộc whitelist, không nhúng input tùy ý.
- Nhật ký chỉ được thêm mới. Không có API sửa hoặc xóa. Tài khoản cơ sở dữ liệu của ứng dụng chỉ có quyền thêm và đọc trên bảng nhật ký.
- Không có màn hình xem nhật ký. Khi cần tra cứu, người vận hành truy vấn trực tiếp cơ sở dữ liệu.

**Alternative / Error Flows**
- Nếu audit DB nghiệp vụ lỗi, rollback cả thao tác và trả 500. Audit khởi tạo tải lỗi: 503, không stream. FILE_READ_RESULT ghi lỗi sau stream đã bắt đầu: ghi cảnh báo vận hành/retry audit gắn cùng event, không hứa rollback bytes đã trả; login thất bại vẫn bị từ chối và log kỹ thuật nếu audit không ghi được.
- Nếu thao tác bị từ chối do lỗi dữ liệu hoặc lỗi phân quyền, hệ thống không ghi bản ghi nghiệp vụ. Riêng đăng nhập thất bại vẫn được ghi `LOGIN_FAILED`.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Nhân viên chuyển phòng ban cho một hồ sơ -> có đúng 1 bản ghi `TRANSFER` với phòng ban cũ, phòng ban mới, người thực hiện và thời điểm.
- **AC-02 [Validation]:** Sinh viên tạo yêu cầu -> bản ghi `CREATE_TICKET` chỉ chứa mã hồ sơ, không chứa nội dung mô tả.
- **AC-03 [Boundary]:** Hồ sơ tự chuyển sang `OVERDUE` khi vượt SLA -> có bản ghi `STATUS_CHANGE` với người thực hiện là `SYSTEM`. Sinh viên bổ sung xong cho hồ sơ đang `PENDING_INFO` -> có bản ghi `SUPPLEMENT_SUBMITTED` và bản ghi `DUE_DATE_CHANGE` với `due_at` cũ và mới.
- **AC-04 [Security]:** Ứng dụng thử chạy câu lệnh sửa hoặc xóa trên bảng nhật ký -> cơ sở dữ liệu từ chối do không có quyền.
- **AC-05 [Concurrency]:** 50 thao tác đổi ưu tiên hợp lệ, không no-op, trên 50 ticket khác nhau -> 50 CHANGE_PRIORITY. Nếu thao tác có nhiều file/cộng bù thì có thêm audit action tương ứng, mỗi event/action/object không trùng.
- **AC-06 [Resilience]:** Bảng nhật ký không ghi được -> thao tác nghiệp vụ bị rollback, dữ liệu hồ sơ không đổi.

**Ví dụ Edge Case**
Sinh viên khiếu nại rằng hồ sơ bị chuyển `OVERDUE` dù mình đã bổ sung đúng hạn. Người vận hành truy vấn nhật ký theo mã hồ sơ và thấy các bản ghi `REQUEST_INFO`, `SUPPLEMENT_SUBMITTED`, `DUE_DATE_CHANGE` và `STATUS_CHANGE`, với thời điểm cụ thể của từng bước. Từ đó xác định được hạn đã được cộng bù đúng hay chưa.

**Expected Result:** Mọi thao tác thuộc danh sách đều có đúng một bản ghi nhật ký không sửa được, và không có thao tác nào được lưu mà thiếu nhật ký.