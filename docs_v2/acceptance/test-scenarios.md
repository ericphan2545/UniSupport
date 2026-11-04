# Kịch bản kiểm thử

Kịch bản trọn luồng (E2E) kiểm tra một hồ sơ đi qua nhiều vai trò. Kịch bản chéo (X) kiểm tra quy tắc áp dụng cho mọi chức năng. Test case chi tiết từng tiêu chí chấp nhận xem [traceability-matrix.md](traceability-matrix.md).

**Dữ liệu dùng chung:** phòng A và phòng B; nhân viên A1, A2 thuộc phòng A, B1 thuộc phòng B; sinh viên SV1, SV2; một Admin; loại yêu cầu X ánh xạ phòng A với SLA 48 giờ; loại yêu cầu Y có SLA 1 giờ để test quá hạn.

## Kịch bản trọn luồng

### E2E-01. Đăng nhập, phân quyền và cấp quyền nhân viên

**Milestone:** 1 · **Chức năng:** SEC-01, SEC-02, SEC-03, MGT-06, STU-01

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | SV1 đăng nhập lần đầu | Tạo tài khoản `STUDENT`, vào Cổng Sinh viên; cookie phiên có HttpOnly, Secure, SameSite=Lax; response không chứa token SSO |
| 2 | SV1 mở thông tin cá nhân | Hiện họ tên, MSSV; trường SSO không trả về hiện `--` |
| 3 | SV1 gọi API hàng đợi của Cổng Nhân viên | 403 |
| 4 | Admin thêm email của A1, vai trò `STAFF`, phòng A | Lưu thành công; có bản ghi `ROLE_CHANGE` |
| 5 | A1 đăng nhập | Vào Cổng Nhân viên |
| 6 | Admin vô hiệu hóa A2 (không phụ trách hồ sơ mở) khi A2 còn phiên | Request kế tiếp của A2 nhận 403, phiên bị thu hồi |
| 7 | Admin tự hạ vai trò hoặc tự vô hiệu hóa chính mình | Bị từ chối; hệ thống luôn còn ít nhất một Admin hoạt động |
| 8 | A1 đăng xuất rồi gửi lại cookie cũ | 401 |

### E2E-02. Sinh viên gửi yêu cầu kèm tệp đến đúng phòng

**Milestone:** 2 · **Chức năng:** STU-02, STU-03, STU-04, STU-05, STU-06, CORE-01, CORE-03, CORE-04, CORE-05, CORE-07, STF-01, STF-02, STF-03, MGT-07

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | SV1 mở form, chọn loại X | Hiện phòng A là nơi tiếp nhận |
| 2 | SV1 nhập mô tả 50 ký tự, đính kèm 1 PDF, 1 JPG, 1 PNG (mỗi tệp 2 MB), gửi | 201; có mã `USP-YYYY-NNNNN`; trạng thái `NEW`, ưu tiên `MEDIUM`; hạn xử lý = lúc gửi + 48 giờ (nhân viên thấy, sinh viên không thấy) |
| 3 | SV1 mở danh sách yêu cầu | Hồ sơ mới ở đầu, có mã, loại, phòng A, trạng thái |
| 4 | A1 mở hàng đợi | Thấy hồ sơ trong vòng 30 giây; B1 không thấy |
| 5 | A1 mở chi tiết, xem và tải 3 tệp | Thấy thông tin sinh viên lúc gửi và mô tả; tệp mở đúng nội dung, tải về đúng tên gốc |
| 6 | Admin đổi SLA loại X thành 24 giờ | Hồ sơ đã tạo giữ hạn cũ; hồ sơ tạo sau đó dùng 24 giờ |
| 7 | SV1 gửi mô tả 19 ký tự; tệp `.docx`; tệp 10 MiB + 1 byte; 6 tệp | Lần lượt 400; 415; 413; 400; không tạo hồ sơ nào |

### E2E-03. Thông báo và trạng thái cho sinh viên

**Milestone:** 2–4 · **Chức năng:** STU-07, STU-08, CORE-02, CORE-06, CORE-08

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 phân công A1 cho hồ sơ `NEW` của SV1 | Hồ sơ `IN_PROGRESS`; SV1 nhận 1 thông báo, chuông tăng 1 trong vòng 30 giây |
| 2 | SV1 mở chi tiết | Lịch sử công khai có mốc tạo hồ sơ và mốc đang xử lý; không có tên nhân viên, hạn xử lý, mức ưu tiên |
| 3 | A1 yêu cầu bổ sung | SV1 nhận thông báo; lịch sử công khai có nội dung yêu cầu, không có tên nhân viên |
| 4 | SV1 bổ sung | Không có thông báo cho hành động của chính SV1 |
| 5 | SV1 đổi giao diện sang EN | Thông báo cũ hiển thị bằng tiếng Anh |
| 6 | SV1 đánh dấu tất cả đã đọc | Số chưa đọc về 0; thông báo đến sau mốc `read_before` vẫn chưa đọc |

### E2E-04. Phân công, ghi nhật ký, yêu cầu bổ sung và bổ sung

**Milestone:** 3 · **Chức năng:** STF-04, STF-05, STF-07, STU-09, CORE-03

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 phân công A1 | `IN_PROGRESS`; mốc phân công chỉ hiện trong lịch sử nội bộ |
| 2 | A2 (không phụ trách) ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ | Cả ba trả 403 |
| 3 | A1 ghi nhật ký xử lý | Nhân viên phòng A thấy; SV1 không thấy |
| 4 | A1 yêu cầu bổ sung (nội dung 10–1000 ký tự) khi hồ sơ còn hạn | `PENDING_INFO`; SLA tạm dừng, thời gian còn lại giữ cố định |
| 5 | A1 gửi thêm một yêu cầu bổ sung khi yêu cầu cũ còn mở | 409 |
| 6 | Sau 6 giờ, SV1 bổ sung (nội dung 10–2000 ký tự, tối đa 5 tệp) | `IN_PROGRESS`; hạn xử lý lùi đúng 6 giờ; có mốc nội bộ thay đổi hạn |
| 7 | SV1 bổ sung lần hai cho cùng yêu cầu | Bị từ chối; mỗi yêu cầu chỉ được trả lời một lần |

### E2E-05. Ưu tiên, escalate và chuyển phòng

**Milestone:** 3 · **Chức năng:** STF-08, STF-09, STF-10, CORE-04, SEC-04

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A1 đặt ưu tiên `LOW` cho một hồ sơ, có hồ sơ khác ở `MEDIUM` và `HIGH` | Hàng đợi xếp `HIGH`, `MEDIUM`, `LOW`; SV1 không thấy mức ưu tiên |
| 2 | A1 escalate kèm lý do 10–500 ký tự, hai lần | `LOW` → `MEDIUM` → `HIGH`; hạn xử lý không đổi |
| 3 | A1 chuyển hồ sơ sang phòng B kèm lý do | Trạng thái, hạn xử lý, loại yêu cầu và yêu cầu bổ sung đang mở giữ nguyên; SV1 không nhận thông báo |
| 4 | A1 mở lại hồ sơ hoặc tải tệp của hồ sơ đó | 404 |
| 5 | B1 mở hàng đợi và chi tiết | Thấy hồ sơ, xem được tệp và lý do chuyển |
| 6 | SV1 mở chi tiết | Thấy hồ sơ đã chuyển đến phòng B, không thấy lý do |
| 7 | B1 chuyển hồ sơ sang chính phòng B | Bị từ chối; phòng đích phải khác phòng hiện tại |

### E2E-06. Quá hạn tự động

**Milestone:** 2–3 · **Chức năng:** CORE-02, CORE-03, STU-08, STF-07, MGT-01

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | Hồ sơ `IN_PROGRESS` loại Y, hạn 10:00:00 | Lúc 09:59:59 vẫn `IN_PROGRESS`; muộn nhất 10:01:00 thành `OVERDUE`; có `STATUS_CHANGE` do `SYSTEM`; SV nhận thông báo; số Quá hạn trên dashboard Admin tăng trong vòng 30 giây |
| 2 | Hồ sơ `PENDING_INFO` đã qua hạn ban đầu | Giữ `PENDING_INFO` |
| 3 | Hồ sơ `NEW` quá hạn chưa phân công, sau đó được phân công | Thành `OVERDUE`; sau khi phân công vẫn `OVERDUE` |
| 4 | Người phụ trách yêu cầu bổ sung cho hồ sơ `OVERDUE`, sinh viên bổ sung | Trạng thái giữ `OVERDUE` suốt quá trình; sinh viên vẫn nhận thông báo yêu cầu bổ sung |
| 5 | Dừng tác vụ quá hạn giữa chừng rồi chạy lại | Hồ sơ còn lại được xử lý tiếp; không tạo trùng lịch sử hay thông báo |

### E2E-07. Đóng hồ sơ, trả kết quả và đánh giá

**Milestone:** 4 · **Chức năng:** STF-06, STU-07, STU-10

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A1 đóng hồ sơ `IN_PROGRESS` với kết quả 10–2000 ký tự | `CLOSED`; SV1 nhận thông báo |
| 2 | Đóng một hồ sơ đang `NEW` | 409 |
| 3 | SV1 mở chi tiết | Thấy kết quả xử lý, thời điểm đóng và form đánh giá |
| 4 | SV1 đánh giá 4 sao kèm nhận xét | 201; đánh giá hiển thị chỉ đọc và được tính vào báo cáo hài lòng |
| 5 | SV1 đánh giá lần hai; đánh giá 0, 6 hoặc 3,5 sao; nhận xét 1001 ký tự | 409; 400; 400 |
| 6 | Gọi bất kỳ thao tác sửa nào trên hồ sơ `CLOSED` | 409; không có chức năng mở lại |
| 7 | Đóng một hồ sơ đang `PENDING_INFO` | Yêu cầu bổ sung đang mở được đóng; khoảng tạm dừng cuối tính đến lúc đóng |

### E2E-08. Báo cáo của Admin khớp dữ liệu

**Milestone:** 5 · **Chức năng:** MGT-01, MGT-02, MGT-03, MGT-04, MGT-05, SEC-04

**Tiền điều kiện:** đã nạp bộ 200 ticket mẫu và có bảng kết quả tính sẵn cho từng chỉ số.

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | Admin mở dashboard | Mới = số `NEW`; Đang xử lý = `IN_PROGRESS` + `PENDING_INFO`; Quá hạn = `OVERDUE`; không tính `CLOSED`; khớp bảng tính sẵn |
| 2 | Bấm từng chỉ số | Danh sách tóm tắt (mã, loại, phòng, trạng thái, ngày gửi); không có tên, MSSV, email, số điện thoại; không có thao tác xử lý |
| 3 | Xem thời gian xử lý trung bình | Bằng thời điểm đóng trừ thời điểm gửi, trừ tổng thời gian chờ bổ sung; khớp bảng tính sẵn |
| 4 | Xem báo cáo loại yêu cầu, khối lượng phòng ban, mức độ hài lòng với khoảng ngày mặc định và một khoảng tự chọn | Khớp bảng tính sẵn |
| 5 | Nhập `from` sau `to`, hoặc khoảng dài hơn 365 ngày | 400 |
| 6 | Chọn khoảng không có dữ liệu | Hiện 0 hoặc trống, không lỗi |
| 7 | Admin gọi API chi tiết hồ sơ hoặc tải tệp | 403 |

## Kịch bản chéo

### X-01. Phạm vi dữ liệu và truy cập chéo

**Chức năng:** SEC-03, SEC-04, SEC-05

- SV1 đổi mã hồ sơ trên URL thành hồ sơ của SV2 → 404, giống hệt hồ sơ không tồn tại.
- A1 mở hồ sơ của phòng B → 404.
- Gửi `student_id` hoặc `department_id` khác trong body hoặc query → hệ thống bỏ qua.
- Admin gọi API chi tiết hồ sơ, tải tệp, đóng hồ sơ → 403.
- Truy cập thẳng đường dẫn lưu tệp trên server → không truy cập được.
- Sửa giao diện để hiện nút của vai trò khác rồi gọi API → 403.

### X-02. Thao tác đồng thời

**Chức năng:** STF-06, STF-07, STF-09, STU-09, MGT-06, MGT-07, CORE-01, CORE-02

- A1 và A2 phân công cùng một hồ sơ với cùng version → một người thành công, người kia nhận 409.
- SV1 bổ sung và A1 đóng hồ sơ với cùng version → chỉ một thao tác thành công.
- Admin vô hiệu hóa A2 đúng lúc A1 phân công hồ sơ cho A2 → không có hồ sơ mở nào được gán cho tài khoản đã vô hiệu hóa.
- Sau khi hồ sơ chuyển sang phòng B, A1 gửi thao tác với version cũ → 404, không phải 409.
- Admin đổi SLA đúng lúc sinh viên tạo hồ sơ → hồ sơ lưu đúng một giá trị SLA đã commit.
- Tác vụ quá hạn và thao tác đóng cùng lúc → chỉ một thay đổi commit; hồ sơ đã `CLOSED` không bị đổi lại.
- 100 hồ sơ tạo đồng thời → 100 mã khác nhau.

### X-03. Mất phản hồi và gửi lại

**Chức năng:** STU-02, STU-09, STU-10, STF-04, STF-05, STF-06

- Tạo hồ sơ, mất mạng sau khi server đã commit, gửi lại cùng `Idempotency-Key` → nhận lại đúng mã cũ; không có hồ sơ, tệp, nhật ký hay thông báo thứ hai.
- Gửi lại cùng key nhưng khác nội dung → 409 `IDEMPOTENCY_PAYLOAD_MISMATCH`.
- Hai request cùng key gửi đồng thời → một request chạy, request kia nhận 409 `REQUEST_IN_PROGRESS` kèm `Retry-After`.
- Mất mạng trước khi commit → không có hồ sơ; gửi lại cùng key thì tạo thành công.
- Lặp lại các ca trên cho bổ sung thông tin, ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ.
- Gửi lại đánh giá sau khi mất phản hồi → 409; giao diện tải lại đánh giá đã lưu thay vì báo lỗi chung.

### X-04. Lỗi tệp, kho tệp và CSDL

**Chức năng:** STU-04, STF-03, CORE-07, SEC-05

- Tệp PNG đổi đuôi thành `.pdf` → chấp nhận và lưu với định dạng PNG.
- Tên tệp `../../etc/passwd.pdf` → bỏ ký tự đường dẫn; tệp lưu với tên ngẫu nhiên.
- Tệp rỗng → 400. Tệp đúng 10 MiB → chấp nhận. 5 tệp, mỗi tệp 10 MiB trong một lần gửi → hợp lệ.
- CSDL lỗi sau khi tệp đã lưu tạm → không có hồ sơ; tệp tạm không tải được và được dọn.
- Kho tệp không phản hồi khi tải → 503. Tệp đã mất trên kho → 404 và có ghi lỗi.
- Ghi nhật ký khởi tạo tải bị lỗi → 503, không trả tệp.

### X-05. Lịch sử hồ sơ và nhật ký hệ thống

**Chức năng:** CORE-06, SEC-06

- Hồ sơ đi đủ luồng tạo → phân công → yêu cầu bổ sung → bổ sung → đóng: lịch sử công khai có đúng 5 mốc; nhân viên thấy thêm mốc phân công và thay đổi hạn.
- Lịch sử công khai không chứa hạn xử lý, mức ưu tiên, tên nhân viên hay lý do chuyển phòng.
- Mỗi thao tác có đúng một bản ghi nhật ký; bản ghi tạo hồ sơ chỉ chứa mã hồ sơ, không chứa mô tả.
- Ứng dụng thử sửa hoặc xóa bảng nhật ký → CSDL từ chối.
- Bảng nhật ký không ghi được → thao tác nghiệp vụ bị rollback, hồ sơ không đổi.

### X-06. Phiên, đăng xuất và cache

**Chức năng:** SEC-01, SEC-02, SEC-03

- Phiên tạo lúc 08:00 → request lúc 15:59 thành công; lúc 16:01 nhận 401 và về trang đăng nhập.
- Đăng nhập trên 2 thiết bị, đăng xuất ở thiết bị A → thiết bị B vẫn dùng bình thường; cookie cũ của thiết bị A nhận 401.
- Đăng xuất hai lần liên tiếp, hoặc đăng xuất khi không có phiên → đều 200.
- Sau khi đăng xuất bấm Back → trang chứa dữ liệu cá nhân không hiện lại từ cache.
- Kho phiên không đọc được → API trả 503, không cho request đi tiếp.

### X-07. VN/EN, responsive và trình duyệt

**Chức năng:** CORE-08, CORE-09

- Đổi sang EN → nhãn, trạng thái, tên loại yêu cầu, thông báo và lỗi đều bằng tiếng Anh; nội dung người dùng nhập giữ nguyên; lần đăng nhập sau vẫn là EN.
- Thiếu bản dịch tiếng Anh của một nhãn → hiện bản tiếng Việt, không hiện mã khóa thô.
- Mọi màn hình ở độ rộng 360px → không có thanh cuộn ngang, không cắt chữ hay nút; vùng bấm tối thiểu 44×44px; bảng chuyển sang dạng thẻ.
- Độ rộng 767px dùng bố cục điện thoại; 768px chuyển sang bố cục máy tính bảng.
- Trên điện thoại: đính kèm ảnh chụp từ camera; xoay màn hình hoặc đổi ngôn ngữ khi đang điền form không mất dữ liệu đã nhập.
- Chạy luồng chính trên 2 phiên bản mới nhất của Chrome, Safari, Edge và Firefox; ghi lại phiên bản đã test.

### X-08. Danh mục và SLA khi triển khai lại

**Chức năng:** CORE-05, MGT-07

- Nạp lại danh mục sau khi Admin đã đổi SLA → SLA Admin đã đổi được giữ nguyên.
- Dữ liệu nạp có loại yêu cầu không ánh xạ phòng ban → ứng dụng không khởi động, log ghi rõ loại bị lỗi.
- Dữ liệu nạp cố xóa loại yêu cầu đã có hồ sơ → lỗi, giữ dữ liệu cũ.
- Thiếu tên tiếng Anh → dùng tên tiếng Việt và ghi cảnh báo.
- Gọi API thêm, sửa, xóa danh mục → không tồn tại (404).
- Admin nhập SLA 0 hoặc 721 giờ → bị từ chối; SLA hợp lệ là số nguyên 1–720.

### X-09. Múi giờ và biên ngày

**Chức năng:** CORE-01, CORE-03, MGT-02, MGT-03, MGT-04, MGT-05

- Hồ sơ tạo lúc 06:59 ngày 01/01 giờ Việt Nam (23:59 ngày 31/12 UTC) → mã mang năm mới.
- Thời gian hiển thị dạng `dd/MM/yyyy HH:mm` giờ Việt Nam ở cả VN và EN.
- Báo cáo theo khoảng ngày tính theo ngày Việt Nam: từ đầu ngày `from` đến trước đầu ngày sau `to`.

### X-10. Rate limit

**Chức năng:** các chức năng có rate limit, xem bảng trong `non-functional-requirements/security.md`

- Với mỗi dòng trong bảng rate limit: gửi request vượt ngưỡng → 429 kèm `Retry-After`; request trong ngưỡng không bị chặn.
- Gửi lại thao tác đã thành công cùng `Idempotency-Key` → không tính thêm vào giới hạn 10 hồ sơ mỗi giờ.

### X-11. Hiệu năng

Theo `non-functional-requirements/performance.md`: chạy kịch bản 100–200 người dùng đồng thời trên hạ tầng của trường trước Go-live.

### X-12. Usability

Theo `non-functional-requirements/usability.md`: người dùng thật thực hiện các tác vụ chính của 3 vai trò.
