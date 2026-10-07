# M4. Xác thực & Bảo mật – Functional Requirement Specification

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

**Business Rules**
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

**Business Rules**
- Phiên bị thu hồi ngay lập tức. Mọi request sau đó dùng JWT cũ đều bị từ chối, kể cả khi JWT chưa hết hạn (thu hồi bằng danh sách phiên phía server).
- Đăng xuất chỉ thu hồi phiên hiện tại, không ảnh hưởng phiên trên thiết bị khác.
- Đăng xuất là idempotent: gọi khi không có phiên hoặc phiên đã hết hạn vẫn trả thành công và chuyển về trang đăng nhập.

**Alternative / Error Flows**
- Nếu không gọi được trang kết thúc phiên của SSO, hệ thống vẫn hoàn tất đăng xuất phía ứng dụng và chuyển về trang đăng nhập.
- Nếu lưu trạng thái thu hồi phiên bị lỗi, hệ thống vẫn xóa cookie, trả 500 và ghi log lỗi hệ thống.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Người dùng bấm Đăng xuất -> cookie bị xóa, người dùng về trang đăng nhập.
- **AC-02 [Validation]:** Gọi đăng xuất khi không có cookie phiên -> vẫn trả 200 và chuyển về trang đăng nhập.
- **AC-03 [Security]:** Sau khi đăng xuất, gửi lại JWT cũ (chưa hết hạn) -> 401.
- **AC-04 [Idempotency]:** Bấm Đăng xuất 2 lần liên tiếp -> cả hai đều thành công, không lỗi.
- **AC-05 [Boundary]:** Người dùng đăng nhập trên 2 thiết bị, đăng xuất ở thiết bị A -> thiết bị B vẫn dùng được bình thường.
- **AC-06 [Resilience & Audit]:** Trang kết thúc phiên của SSO không phản hồi -> ứng dụng vẫn đăng xuất xong. Đăng xuất thành công -> có bản ghi `LOGOUT`.

**Ví dụ Edge Case**
Sinh viên dùng máy tính chung ở thư viện, đăng xuất rồi rời đi. Người dùng sau bấm nút Back của trình duyệt để quay lại trang hồ sơ. Trang được tải lại từ server và nhận 401, nên người đó chỉ thấy trang đăng nhập, không thấy dữ liệu của sinh viên trước.

**Expected Result:** Sau khi đăng xuất, phiên cũ không thể dùng để truy cập bất kỳ dữ liệu nào.

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

**Business Rules**
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
Một nhân viên bị vô hiệu hóa tài khoản trong lúc đang mở trang chi tiết hồ sơ với phiên còn 5 giờ. Khi người này bấm lưu ghi chú, hệ thống trả 403, thu hồi phiên và chuyển về trang đăng nhập. Ghi chú không được lưu.

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

**Business Rules**
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
3. Hệ thống ghi nhật ký.
4. Hệ thống đọc tệp từ kho lưu trữ và trả về.

**Business Rules**
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
- Ghi nhật ký `DOWNLOAD_FILE` trước khi trả tệp. Nếu ghi nhật ký lỗi thì không trả tệp.

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
- **AC-06 [Resilience & Audit]:** Ghi nhật ký lỗi -> 503, tệp không được trả về. Tải thành công -> có bản ghi `DOWNLOAD_FILE` (người tải, tệp, hồ sơ, thời gian).

**Ví dụ Edge Case**
Nhân viên sao chép đường dẫn tải tệp rồi gửi qua tin nhắn cho một người ngoài phòng ban. Người nhận mở đường dẫn: nếu chưa đăng nhập thì nhận 401, nếu đã đăng nhập nhưng không có quyền thì nhận 404 (hoặc 403 nếu là Ban quản lý). Đường dẫn tải tệp không mang quyền truy cập theo nó.

**Expected Result:** Tệp chỉ được trả cho sinh viên chủ hồ sơ hoặc nhân viên phòng ban đang giữ hồ sơ tại thời điểm tải, và mọi lần tải đều được ghi lại.   
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
2. Hệ thống tạo bản ghi nhật ký trong cùng transaction với thao tác.
3. Transaction thành công thì thao tác và bản ghi nhật ký cùng được lưu. Transaction thất bại thì cả hai cùng bị hủy.

**Business Rules**
- Các thao tác được ghi:
  - Đăng nhập và đăng xuất: `LOGIN_SUCCESS`, `LOGIN_FAILED`, `LOGOUT`.
  - Hồ sơ: `CREATE_TICKET`, `SUPPLEMENT_SUBMITTED`, `RATE_TICKET`, `ADD_NOTE`, `REQUEST_INFO`, `CLOSE_TICKET`, `ASSIGN`, `CHANGE_PRIORITY`, `ESCALATE`, `TRANSFER`.
  - Trạng thái do hệ thống tự đổi: `STATUS_CHANGE` (ví dụ hồ sơ chuyển sang `OVERDUE`), với người thực hiện là `SYSTEM`.
  - Hạn xử lý: `DUE_DATE_CHANGE`, khi `due_at` được cộng bù lúc hồ sơ rời `PENDING_INFO` (FR-CORE-03). Người thực hiện là người gây ra sự kiện, ví dụ sinh viên bổ sung hoặc nhân viên đóng hồ sơ.
  - Tệp: `UPLOAD_FILE`, `DOWNLOAD_FILE`.
  - Cấu hình: `ROLE_CHANGE`, `SLA_CHANGE`.
- Mỗi bản ghi gồm: thời điểm (UTC+7), người thực hiện, vai trò, mã thao tác, loại và mã đối tượng, giá trị cũ và mới (nếu có), địa chỉ IP, kết quả.
- Nhật ký không lưu nội dung nhạy cảm như mô tả yêu cầu, nội dung ghi chú, nhận xét hay nội dung tệp. Nhật ký chỉ lưu mã tham chiếu tới các đối tượng đó.
- Nhật ký chỉ được thêm mới. Không có API sửa hoặc xóa. Tài khoản cơ sở dữ liệu của ứng dụng chỉ có quyền thêm và đọc trên bảng nhật ký.
- Không có màn hình xem nhật ký. Khi cần tra cứu, người vận hành truy vấn trực tiếp cơ sở dữ liệu.

**Alternative / Error Flows**
- Nếu ghi nhật ký thất bại, hệ thống rollback cả thao tác và trả 500 (riêng tải tệp trả 503, theo FR-SEC-05).
- Nếu thao tác bị từ chối do lỗi dữ liệu hoặc lỗi phân quyền, hệ thống không ghi bản ghi nghiệp vụ. Riêng đăng nhập thất bại vẫn được ghi `LOGIN_FAILED`.

**Acceptance Criteria**
- **AC-01 [Happy Path]:** Nhân viên chuyển phòng ban cho một hồ sơ -> có đúng 1 bản ghi `TRANSFER` với phòng ban cũ, phòng ban mới, người thực hiện và thời điểm.
- **AC-02 [Validation]:** Sinh viên tạo yêu cầu -> bản ghi `CREATE_TICKET` chỉ chứa mã hồ sơ, không chứa nội dung mô tả.
- **AC-03 [Boundary]:** Hồ sơ tự chuyển sang `OVERDUE` khi vượt SLA -> có bản ghi `STATUS_CHANGE` với người thực hiện là `SYSTEM`. Sinh viên bổ sung xong cho hồ sơ đang `PENDING_INFO` -> có bản ghi `SUPPLEMENT_SUBMITTED` và bản ghi `DUE_DATE_CHANGE` với `due_at` cũ và mới.
- **AC-04 [Security]:** Ứng dụng thử chạy câu lệnh sửa hoặc xóa trên bảng nhật ký -> cơ sở dữ liệu từ chối do không có quyền.
- **AC-05 [Concurrency]:** 50 thao tác đồng thời trên nhiều hồ sơ khác nhau -> có đúng 50 bản ghi nghiệp vụ tương ứng, không mất và không trùng.
- **AC-06 [Resilience]:** Bảng nhật ký không ghi được -> thao tác nghiệp vụ bị rollback, dữ liệu hồ sơ không đổi.

**Ví dụ Edge Case**
Sinh viên khiếu nại rằng hồ sơ bị chuyển `OVERDUE` dù mình đã bổ sung đúng hạn. Người vận hành truy vấn nhật ký theo mã hồ sơ và thấy các bản ghi `REQUEST_INFO`, `SUPPLEMENT_SUBMITTED`, `DUE_DATE_CHANGE` và `STATUS_CHANGE`, với thời điểm cụ thể của từng bước. Từ đó xác định được hạn đã được cộng bù đúng hay chưa.

**Expected Result:** Mọi thao tác thuộc danh sách đều có đúng một bản ghi nhật ký không sửa được, và không có thao tác nào được lưu mà thiếu nhật ký.