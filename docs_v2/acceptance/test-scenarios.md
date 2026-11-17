# Kịch bản kiểm thử

Nơi gốc của mã kịch bản test `TS-xx`. Mỗi kịch bản ghi mã AC hoặc NFR mà nó kiểm; PRD không ghi ngược lại mã test. Có ba nhóm:

- `TS-01` đến `TS-08`: kịch bản trọn luồng, một hồ sơ đi qua nhiều vai trò; client dùng các kịch bản này khi demo từng milestone.
- `TS-09` đến `TS-20`: kịch bản chéo, kiểm quy tắc áp dụng cho nhiều chức năng (quyền, đồng thời, gửi lại, lỗi, giao diện, rate limit, hiệu năng, usability).
- `TS-21` đến `TS-62`: kịch bản chức năng, mỗi FR một kịch bản chạy lần lượt mọi AC của FR đó theo đúng điều kiện, dữ liệu và kết quả mong đợi ghi trong AC.

Cột **Regression** = có: kịch bản thuộc bộ chạy lại trước mỗi lần bàn giao (dòng WBS "Test trọn luồng và regression"), để phần mới không làm hỏng phần đã giao. Bảng tổng hợp theo từng AC: [traceability-matrix.md](traceability-matrix.md).

## Danh sách kịch bản trọn luồng và kịch bản chéo

| Mã | Kịch bản | Số AC được kiểm | NFR | Milestone chạy lần đầu | Regression |
|---|---|---|---|---|---|
| `TS-01` | Đăng nhập, phân quyền và cấp quyền nhân viên | 6 | — | MS-01 | có |
| `TS-02` | Sinh viên gửi yêu cầu kèm tệp đến đúng phòng | 16 | — | MS-02 | có |
| `TS-03` | Thông báo và trạng thái cho sinh viên | 5 | — | MS-02 | có |
| `TS-04` | Phân công, ghi nhật ký, yêu cầu bổ sung và bổ sung | 7 | — | MS-03 | có |
| `TS-05` | Ưu tiên, escalate và chuyển phòng | 5 | — | MS-03 | có |
| `TS-06` | Quá hạn tự động | 6 | NFR-PERF-09, NFR-REL-07 | MS-03 | có |
| `TS-07` | Đóng hồ sơ, trả kết quả và đánh giá | 4 | — | MS-04 | có |
| `TS-08` | Báo cáo của Admin khớp dữ liệu | 7 | NFR-SEC-12 | MS-05 | có |
| `TS-09` | Phạm vi dữ liệu và truy cập chéo | 3 | NFR-SEC-03, NFR-SEC-06 | MS-01 đến MS-05 | có |
| `TS-10` | Thao tác đồng thời | 8 | NFR-REL-04 | MS-01 đến MS-04 | có |
| `TS-11` | Mất phản hồi và gửi lại | 6 | NFR-REL-03 | MS-02 đến MS-04 | có |
| `TS-12` | Lỗi tệp, kho tệp và CSDL | 4 | NFR-REL-01, NFR-REL-06, NFR-PERF-14 | MS-02 | có |
| `TS-13` | Lịch sử hồ sơ và nhật ký hệ thống | 12 | NFR-REL-02, NFR-SEC-09, NFR-OBS-04 | MS-01 đến MS-04 | có |
| `TS-14` | Phiên, đăng xuất và cache | 6 | NFR-SEC-02, NFR-SEC-07 | MS-01 | có |
| `TS-15` | VN/EN, responsive và trình duyệt | 12 | NFR-USE-02, NFR-USE-03, NFR-USE-04 | MS-01 đến MS-05 | có |
| `TS-16` | Danh mục và SLA khi triển khai lại | 12 | NFR-DEP-01, NFR-DEP-02 | MS-02 | có |
| `TS-17` | Múi giờ và biên ngày | 6 | — | MS-02, MS-05 | có |
| `TS-18` | Rate limit | 11 | NFR-SEC-13 | MS-01 đến MS-05 | có |
| `TS-19` | Hiệu năng | 0 | NFR-PERF-01, NFR-PERF-02, NFR-PERF-03 | MS-06 | không |
| `TS-20` | Usability | 0 | NFR-USE-01 | MS-06 | không |

**Dữ liệu dùng chung:** phòng A và phòng B; nhân viên A1, A2 thuộc phòng A, B1 thuộc phòng B; sinh viên SV1, SV2; một Admin; loại yêu cầu X ánh xạ phòng A với SLA 48 giờ; loại yêu cầu Y có SLA 1 giờ để test quá hạn.

## Kịch bản trọn luồng (TS-01 đến TS-08)

### TS-01. Đăng nhập, phân quyền và cấp quyền nhân viên

**Kiểm AC:** `FR-SEC-01/AC-01`, `FR-SEC-02/AC-01`, `FR-SEC-03/AC-01`, `FR-MGT-06/AC-01`, `FR-MGT-06/AC-03`, `FR-STU-01/AC-01`

**Milestone chạy lần đầu:** MS-01 · **Regression:** có

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

### TS-02. Sinh viên gửi yêu cầu kèm tệp đến đúng phòng

**Kiểm AC:** `FR-STU-02/AC-01`, `FR-STU-03/AC-01`, `FR-STU-04/AC-01`, `FR-STU-05/AC-01`, `FR-STU-06/AC-01`, `FR-CORE-01/AC-01`, `FR-CORE-03/AC-01`, `FR-CORE-03/AC-02`, `FR-CORE-04/AC-01`, `FR-CORE-05/AC-01`, `FR-CORE-07/AC-01`, `FR-STF-01/AC-01`, `FR-STF-02/AC-01`, `FR-STF-03/AC-01`, `FR-MGT-07/AC-01`, `FR-MGT-07/AC-03`

**Milestone chạy lần đầu:** MS-02 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | SV1 mở form, chọn loại X | Hiện phòng A là nơi tiếp nhận |
| 2 | SV1 nhập mô tả 50 ký tự, đính kèm 1 PDF, 1 JPG, 1 PNG (mỗi tệp 2 MB), gửi | 201; có mã `USP-YYYY-NNNNN`; trạng thái `NEW`, ưu tiên `MEDIUM`; hạn xử lý = lúc gửi + 48 giờ (nhân viên thấy, sinh viên không thấy) |
| 3 | SV1 mở danh sách yêu cầu | Hồ sơ mới ở đầu, có mã, loại, phòng A, trạng thái |
| 4 | A1 mở hàng đợi | Thấy hồ sơ trong vòng 30 giây; B1 không thấy |
| 5 | A1 mở chi tiết, xem và tải 3 tệp | Thấy thông tin sinh viên lúc gửi và mô tả; tệp mở đúng nội dung, tải về đúng tên gốc |
| 6 | Admin đổi SLA loại X thành 24 giờ | Hồ sơ đã tạo giữ hạn cũ; hồ sơ tạo sau đó dùng 24 giờ |
| 7 | SV1 gửi mô tả 19 ký tự; tệp `.docx`; tệp 10 MiB + 1 byte; 6 tệp | Lần lượt 400; 415; 413; 400; không tạo hồ sơ nào |

### TS-03. Thông báo và trạng thái cho sinh viên

**Kiểm AC:** `FR-STU-07/AC-01`, `FR-STU-08/AC-01`, `FR-CORE-02/AC-01`, `FR-CORE-06/AC-01`, `FR-CORE-08/AC-01`

**Milestone chạy lần đầu:** MS-02 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 phân công A1 cho hồ sơ `NEW` của SV1 | Hồ sơ `IN_PROGRESS`; SV1 nhận 1 thông báo, chuông tăng 1 trong vòng 30 giây |
| 2 | SV1 mở chi tiết | Lịch sử công khai có mốc tạo hồ sơ và mốc đang xử lý; không có tên nhân viên, hạn xử lý, mức ưu tiên |
| 3 | A1 yêu cầu bổ sung | SV1 nhận thông báo; lịch sử công khai có nội dung yêu cầu, không có tên nhân viên |
| 4 | SV1 bổ sung | Không có thông báo cho hành động của chính SV1 |
| 5 | SV1 đổi giao diện sang EN | Thông báo cũ hiển thị bằng tiếng Anh |
| 6 | SV1 đánh dấu tất cả đã đọc | Số chưa đọc về 0; thông báo đến sau mốc `read_before` vẫn chưa đọc |

### TS-04. Phân công, ghi nhật ký, yêu cầu bổ sung và bổ sung

**Kiểm AC:** `FR-STF-04/AC-01`, `FR-STF-04/AC-05`, `FR-STF-05/AC-01`, `FR-STF-07/AC-01`, `FR-STU-09/AC-01`, `FR-CORE-03/AC-01`, `FR-CORE-03/AC-02`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 phân công A1 | `IN_PROGRESS`; mốc phân công chỉ hiện trong lịch sử nội bộ |
| 2 | A2 (không phụ trách) ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ | Cả ba trả 403 |
| 3 | A1 ghi nhật ký xử lý | Nhân viên phòng A thấy; SV1 không thấy |
| 4 | A1 yêu cầu bổ sung (nội dung 10–1000 ký tự) khi hồ sơ còn hạn | `PENDING_INFO`; SLA tạm dừng, thời gian còn lại giữ cố định |
| 5 | A1 gửi thêm một yêu cầu bổ sung khi yêu cầu cũ còn mở | 409 |
| 6 | Sau 6 giờ, SV1 bổ sung (nội dung 10–2000 ký tự, tối đa 5 tệp) | `IN_PROGRESS`; hạn xử lý lùi đúng 6 giờ; có mốc nội bộ thay đổi hạn |
| 7 | SV1 bổ sung lần hai cho cùng yêu cầu | Bị từ chối; mỗi yêu cầu chỉ được trả lời một lần |

### TS-05. Ưu tiên, escalate và chuyển phòng

**Kiểm AC:** `FR-STF-08/AC-01`, `FR-STF-09/AC-01`, `FR-STF-10/AC-01`, `FR-CORE-04/AC-01`, `FR-SEC-04/AC-01`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A1 đặt ưu tiên `LOW` cho một hồ sơ, có hồ sơ khác ở `MEDIUM` và `HIGH` | Hàng đợi xếp `HIGH`, `MEDIUM`, `LOW`; SV1 không thấy mức ưu tiên |
| 2 | A1 escalate kèm lý do 10–500 ký tự, hai lần | `LOW` → `MEDIUM` → `HIGH`; hạn xử lý không đổi |
| 3 | A1 chuyển hồ sơ sang phòng B kèm lý do | Trạng thái, hạn xử lý, loại yêu cầu và yêu cầu bổ sung đang mở giữ nguyên; SV1 không nhận thông báo |
| 4 | A1 mở lại hồ sơ hoặc tải tệp của hồ sơ đó | 404 |
| 5 | B1 mở hàng đợi và chi tiết | Thấy hồ sơ, xem được tệp và lý do chuyển |
| 6 | SV1 mở chi tiết | Thấy hồ sơ đã chuyển đến phòng B, không thấy lý do |
| 7 | B1 chuyển hồ sơ sang chính phòng B | Bị từ chối; phòng đích phải khác phòng hiện tại |

### TS-06. Quá hạn tự động

**Kiểm AC:** `FR-CORE-02/AC-01`, `FR-CORE-03/AC-01`, `FR-CORE-03/AC-02`, `FR-STU-08/AC-01`, `FR-STF-07/AC-01`, `FR-MGT-01/AC-01`

**Kiểm NFR:** `NFR-PERF-09`, `NFR-REL-07`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | Hồ sơ `IN_PROGRESS` loại Y, hạn 10:00:00 | Lúc 09:59:59 vẫn `IN_PROGRESS`; muộn nhất 10:01:00 thành `OVERDUE`; có `STATUS_CHANGE` do `SYSTEM`; SV nhận thông báo; số Quá hạn trên dashboard Admin tăng trong vòng 30 giây |
| 2 | Hồ sơ `PENDING_INFO` đã qua hạn ban đầu | Giữ `PENDING_INFO` |
| 3 | Hồ sơ `NEW` quá hạn chưa phân công, sau đó được phân công | Thành `OVERDUE`; sau khi phân công vẫn `OVERDUE` |
| 4 | Người phụ trách yêu cầu bổ sung cho hồ sơ `OVERDUE`, sinh viên bổ sung | Trạng thái giữ `OVERDUE` suốt quá trình; sinh viên vẫn nhận thông báo yêu cầu bổ sung |
| 5 | Dừng tác vụ quá hạn giữa chừng rồi chạy lại | Hồ sơ còn lại được xử lý tiếp; không tạo trùng lịch sử hay thông báo |

### TS-07. Đóng hồ sơ, trả kết quả và đánh giá

**Kiểm AC:** `FR-STF-06/AC-01`, `FR-STU-07/AC-01`, `FR-STU-10/AC-01`, `FR-STU-10/AC-04`

**Milestone chạy lần đầu:** MS-04 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A1 đóng hồ sơ `IN_PROGRESS` với kết quả 10–2000 ký tự | `CLOSED`; SV1 nhận thông báo |
| 2 | Đóng một hồ sơ đang `NEW` | 409 |
| 3 | SV1 mở chi tiết | Thấy kết quả xử lý, thời điểm đóng và form đánh giá |
| 4 | SV1 đánh giá 4 sao kèm nhận xét | 201; đánh giá hiển thị chỉ đọc và được tính vào báo cáo hài lòng |
| 5 | SV1 đánh giá lần hai; đánh giá 0, 6 hoặc 3,5 sao; nhận xét 1001 ký tự | 409; 400; 400 |
| 6 | Gọi bất kỳ thao tác sửa nào trên hồ sơ `CLOSED` | 409; không có chức năng mở lại |
| 7 | Đóng một hồ sơ đang `PENDING_INFO` | Yêu cầu bổ sung đang mở được đóng; khoảng tạm dừng cuối tính đến lúc đóng |

### TS-08. Báo cáo của Admin khớp dữ liệu

**Kiểm AC:** `FR-MGT-01/AC-01`, `FR-MGT-02/AC-01`, `FR-MGT-02/AC-02`, `FR-MGT-03/AC-01`, `FR-MGT-04/AC-01`, `FR-MGT-05/AC-01`, `FR-SEC-04/AC-01`

**Kiểm NFR:** `NFR-SEC-12`

**Milestone chạy lần đầu:** MS-05 · **Regression:** có

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

## Kịch bản chéo (TS-09 đến TS-20)

### TS-09. Phạm vi dữ liệu và truy cập chéo

**Kiểm AC:** `FR-SEC-03/AC-04`, `FR-SEC-04/AC-04`, `FR-SEC-05/AC-03`

**Kiểm NFR:** `NFR-SEC-03`, `NFR-SEC-06`

**Milestone chạy lần đầu:** MS-01 đến MS-05 · **Regression:** có

- SV1 đổi mã hồ sơ trên URL thành hồ sơ của SV2 → 404, giống hệt hồ sơ không tồn tại.
- A1 mở hồ sơ của phòng B → 404.
- Gửi `student_id` hoặc `department_id` khác trong body hoặc query → hệ thống bỏ qua.
- Admin gọi API chi tiết hồ sơ, tải tệp, đóng hồ sơ → 403.
- Truy cập thẳng đường dẫn lưu tệp trên server → không truy cập được.
- Sửa giao diện để hiện nút của vai trò khác rồi gọi API → 403.

### TS-10. Thao tác đồng thời

**Kiểm AC:** `FR-STF-06/AC-05`, `FR-STF-07/AC-05`, `FR-STF-09/AC-04`, `FR-STU-09/AC-05`, `FR-MGT-06/AC-05`, `FR-MGT-07/AC-05`, `FR-CORE-01/AC-05`, `FR-CORE-02/AC-05`

**Kiểm NFR:** `NFR-REL-04`

**Milestone chạy lần đầu:** MS-01 đến MS-04 · **Regression:** có

- A1 và A2 phân công cùng một hồ sơ với cùng version → một người thành công, người kia nhận 409.
- SV1 bổ sung và A1 đóng hồ sơ với cùng version → chỉ một thao tác thành công.
- Admin vô hiệu hóa A2 đúng lúc A1 phân công hồ sơ cho A2 → không có hồ sơ mở nào được gán cho tài khoản đã vô hiệu hóa.
- Sau khi hồ sơ chuyển sang phòng B, A1 gửi thao tác với version cũ → 404, không phải 409.
- Admin đổi SLA đúng lúc sinh viên tạo hồ sơ → hồ sơ lưu đúng một giá trị SLA đã commit.
- Tác vụ quá hạn và thao tác đóng cùng lúc → chỉ một thay đổi commit; hồ sơ đã `CLOSED` không bị đổi lại.
- 100 hồ sơ tạo đồng thời → 100 mã khác nhau.

### TS-11. Mất phản hồi và gửi lại

**Kiểm AC:** `FR-STU-02/AC-04`, `FR-STU-09/AC-05`, `FR-STU-10/AC-05`, `FR-STF-04/AC-04`, `FR-STF-05/AC-05`, `FR-STF-06/AC-01`

**Kiểm NFR:** `NFR-REL-03`

**Milestone chạy lần đầu:** MS-02 đến MS-04 · **Regression:** có

- Tạo hồ sơ, mất mạng sau khi server đã commit, gửi lại cùng `Idempotency-Key` → nhận lại đúng mã cũ; không có hồ sơ, tệp, nhật ký hay thông báo thứ hai.
- Gửi lại cùng key nhưng khác nội dung → 409 `IDEMPOTENCY_PAYLOAD_MISMATCH`.
- Hai request cùng key gửi đồng thời → một request chạy, request kia nhận 409 `REQUEST_IN_PROGRESS` kèm `Retry-After`.
- Mất mạng trước khi commit → không có hồ sơ; gửi lại cùng key thì tạo thành công.
- Lặp lại các ca trên cho bổ sung thông tin, ghi nhật ký, yêu cầu bổ sung, đóng hồ sơ.
- Gửi lại đánh giá sau khi mất phản hồi → 409; giao diện tải lại đánh giá đã lưu thay vì báo lỗi chung.

### TS-12. Lỗi tệp, kho tệp và CSDL

**Kiểm AC:** `FR-STU-04/AC-06`, `FR-STF-03/AC-06`, `FR-CORE-07/AC-06`, `FR-SEC-05/AC-06`

**Kiểm NFR:** `NFR-REL-01`, `NFR-REL-06`, `NFR-PERF-14`

**Milestone chạy lần đầu:** MS-02 · **Regression:** có

- Tệp PNG đổi đuôi thành `.pdf` → chấp nhận và lưu với định dạng PNG.
- Tên tệp `../../etc/passwd.pdf` → bỏ ký tự đường dẫn; tệp lưu với tên ngẫu nhiên.
- Tệp rỗng → 400. Tệp đúng 10 MiB → chấp nhận. 5 tệp, mỗi tệp 10 MiB trong một lần gửi → hợp lệ.
- CSDL lỗi sau khi tệp đã lưu tạm → không có hồ sơ; tệp tạm không tải được và được dọn.
- Kho tệp không phản hồi khi tải → 503. Tệp đã mất trên kho → 404 và có ghi lỗi.
- Ghi nhật ký khởi tạo tải bị lỗi → 503, không trả tệp.

### TS-13. Lịch sử hồ sơ và nhật ký hệ thống

**Kiểm AC:** `FR-CORE-06/AC-01`, `FR-CORE-06/AC-02`, `FR-CORE-06/AC-03`, `FR-CORE-06/AC-04`, `FR-CORE-06/AC-05`, `FR-CORE-06/AC-06`, `FR-SEC-06/AC-01`, `FR-SEC-06/AC-02`, `FR-SEC-06/AC-03`, `FR-SEC-06/AC-04`, `FR-SEC-06/AC-05`, `FR-SEC-06/AC-06`

**Kiểm NFR:** `NFR-REL-02`, `NFR-SEC-09`, `NFR-OBS-04`

**Milestone chạy lần đầu:** MS-01 đến MS-04 · **Regression:** có

- Hồ sơ đi đủ luồng tạo → phân công → yêu cầu bổ sung → bổ sung → đóng: lịch sử công khai có đúng 5 mốc; nhân viên thấy thêm mốc phân công và thay đổi hạn.
- Lịch sử công khai không chứa hạn xử lý, mức ưu tiên, tên nhân viên hay lý do chuyển phòng.
- Mỗi thao tác có đúng một bản ghi nhật ký; bản ghi tạo hồ sơ chỉ chứa mã hồ sơ, không chứa mô tả.
- Ứng dụng thử sửa hoặc xóa bảng nhật ký → CSDL từ chối.
- Bảng nhật ký không ghi được → thao tác nghiệp vụ bị rollback, hồ sơ không đổi.

### TS-14. Phiên, đăng xuất và cache

**Kiểm AC:** `FR-SEC-01/AC-04`, `FR-SEC-01/AC-06`, `FR-SEC-02/AC-03`, `FR-SEC-02/AC-06`, `FR-SEC-03/AC-04`, `FR-SEC-03/AC-06`

**Kiểm NFR:** `NFR-SEC-02`, `NFR-SEC-07`

**Milestone chạy lần đầu:** MS-01 · **Regression:** có

- Phiên tạo lúc 08:00 → request lúc 15:59 thành công; lúc 16:01 nhận 401 và về trang đăng nhập.
- Đăng nhập trên 2 thiết bị, đăng xuất ở thiết bị A → thiết bị B vẫn dùng bình thường; cookie cũ của thiết bị A nhận 401.
- Đăng xuất hai lần liên tiếp, hoặc đăng xuất khi không có phiên → đều 200.
- Sau khi đăng xuất bấm Back → trang chứa dữ liệu cá nhân không hiện lại từ cache.
- Kho phiên không đọc được → API trả 503, không cho request đi tiếp.

### TS-15. VN/EN, responsive và trình duyệt

**Kiểm AC:** `FR-CORE-08/AC-01`, `FR-CORE-08/AC-02`, `FR-CORE-08/AC-03`, `FR-CORE-08/AC-04`, `FR-CORE-08/AC-05`, `FR-CORE-08/AC-06`, `FR-CORE-09/AC-01`, `FR-CORE-09/AC-02`, `FR-CORE-09/AC-03`, `FR-CORE-09/AC-04`, `FR-CORE-09/AC-05`, `FR-CORE-09/AC-06`

**Kiểm NFR:** `NFR-USE-02`, `NFR-USE-03`, `NFR-USE-04`

**Milestone chạy lần đầu:** MS-01 đến MS-05 · **Regression:** có

- Đổi sang EN → nhãn, trạng thái, tên loại yêu cầu, thông báo và lỗi đều bằng tiếng Anh; nội dung người dùng nhập giữ nguyên; lần đăng nhập sau vẫn là EN.
- Thiếu bản dịch tiếng Anh của một nhãn → hiện bản tiếng Việt, không hiện mã khóa thô.
- Mọi màn hình ở độ rộng 360px → không có thanh cuộn ngang, không cắt chữ hay nút; vùng bấm tối thiểu 44×44px; bảng chuyển sang dạng thẻ.
- Độ rộng 767px dùng bố cục điện thoại; 768px chuyển sang bố cục máy tính bảng.
- Trên điện thoại: đính kèm ảnh chụp từ camera; xoay màn hình hoặc đổi ngôn ngữ khi đang điền form không mất dữ liệu đã nhập.
- Chạy luồng chính trên 2 phiên bản mới nhất của Chrome, Safari, Edge và Firefox; ghi lại phiên bản đã test.

### TS-16. Danh mục và SLA khi triển khai lại

**Kiểm AC:** `FR-CORE-05/AC-01`, `FR-CORE-05/AC-02`, `FR-CORE-05/AC-03`, `FR-CORE-05/AC-04`, `FR-CORE-05/AC-05`, `FR-CORE-05/AC-06`, `FR-MGT-07/AC-01`, `FR-MGT-07/AC-02`, `FR-MGT-07/AC-03`, `FR-MGT-07/AC-04`, `FR-MGT-07/AC-05`, `FR-MGT-07/AC-06`

**Kiểm NFR:** `NFR-DEP-01`, `NFR-DEP-02`

**Milestone chạy lần đầu:** MS-02 · **Regression:** có

- Nạp lại danh mục sau khi Admin đã đổi SLA → SLA Admin đã đổi được giữ nguyên.
- Dữ liệu nạp có loại yêu cầu không ánh xạ phòng ban → ứng dụng không khởi động, log ghi rõ loại bị lỗi.
- Dữ liệu nạp cố xóa loại yêu cầu đã có hồ sơ → lỗi, giữ dữ liệu cũ.
- Thiếu tên tiếng Anh → dùng tên tiếng Việt và ghi cảnh báo.
- Gọi API thêm, sửa, xóa danh mục → không tồn tại (404).
- Admin nhập SLA 0 hoặc 721 giờ → bị từ chối; SLA hợp lệ là số nguyên 1–720.

### TS-17. Múi giờ và biên ngày

**Kiểm AC:** `FR-CORE-01/AC-03`, `FR-CORE-03/AC-03`, `FR-MGT-02/AC-03`, `FR-MGT-03/AC-03`, `FR-MGT-04/AC-03`, `FR-MGT-05/AC-03`

**Milestone chạy lần đầu:** MS-02, MS-05 · **Regression:** có

- Hồ sơ tạo lúc 06:59 ngày 01/01 giờ Việt Nam (23:59 ngày 31/12 UTC) → mã mang năm mới.
- Thời gian hiển thị dạng `dd/MM/yyyy HH:mm` giờ Việt Nam ở cả VN và EN.
- Báo cáo theo khoảng ngày tính theo ngày Việt Nam: từ đầu ngày `from` đến trước đầu ngày sau `to`.

### TS-18. Rate limit

**Kiểm AC:** `FR-STU-02/AC-05`, `FR-STU-03/AC-04`, `FR-STU-06/AC-05`, `FR-STF-01/AC-05`, `FR-STF-03/AC-04`, `FR-MGT-01/AC-05`, `FR-MGT-02/AC-05`, `FR-MGT-03/AC-05`, `FR-MGT-04/AC-05`, `FR-MGT-05/AC-05`, `FR-SEC-01/AC-05`

**Kiểm NFR:** `NFR-SEC-13`

**Milestone chạy lần đầu:** MS-01 đến MS-05 · **Regression:** có

- Với mỗi dòng trong bảng rate limit: gửi request vượt ngưỡng → 429 kèm `Retry-After`; request trong ngưỡng không bị chặn.
- Gửi lại thao tác đã thành công cùng `Idempotency-Key` → không tính thêm vào giới hạn 10 hồ sơ mỗi giờ.

### TS-19. Hiệu năng

**Kiểm NFR:** `NFR-PERF-01`, `NFR-PERF-02`, `NFR-PERF-03`

**Milestone chạy lần đầu:** MS-06 · **Regression:** không

Theo `non-functional-requirements/performance.md` (`NFR-PERF-01` đến `NFR-PERF-03`): chạy kịch bản 100–200 người dùng đồng thời trên hạ tầng của trường trước Go-live.

### TS-20. Usability

**Kiểm NFR:** `NFR-USE-01`

**Milestone chạy lần đầu:** MS-06 · **Regression:** không

Theo `non-functional-requirements/usability.md` (`NFR-USE-01`): người dùng thật thực hiện các tác vụ chính của 3 vai trò.

## Kịch bản chức năng (TS-21 đến TS-62)

Mỗi kịch bản chạy lần lượt mọi AC của một FR trên Staging, với dữ liệu dùng chung ở đầu tài liệu và dữ liệu riêng ghi trong AC. **Các bước** và **kết quả mong đợi** của từng AC là phần "điều kiện → kết quả" ghi trong AC ở `modules/*/prd.md`. Kịch bản đạt khi mọi AC đạt. Chạy trong milestone của FR; các milestone sau kiểm lại bằng kịch bản trọn luồng và kịch bản chéo có Regression = có.

| Mã | FR | Kiểm AC | Milestone chạy lần đầu | Regression |
|---|---|---|---|---|
| `TS-21` | `FR-STU-01` | `FR-STU-01/AC-01` đến `FR-STU-01/AC-06` | MS-01 | không |
| `TS-22` | `FR-STU-02` | `FR-STU-02/AC-01` đến `FR-STU-02/AC-06` | MS-02 | không |
| `TS-23` | `FR-STU-03` | `FR-STU-03/AC-01` đến `FR-STU-03/AC-06` | MS-02 | không |
| `TS-24` | `FR-STU-04` | `FR-STU-04/AC-01` đến `FR-STU-04/AC-06` | MS-02 | không |
| `TS-25` | `FR-STU-05` | `FR-STU-05/AC-01` đến `FR-STU-05/AC-06` | MS-02 | không |
| `TS-26` | `FR-STU-06` | `FR-STU-06/AC-01` đến `FR-STU-06/AC-06` | MS-02 | không |
| `TS-27` | `FR-STU-07` | `FR-STU-07/AC-01` đến `FR-STU-07/AC-06` | MS-04 | không |
| `TS-28` | `FR-STU-08` | `FR-STU-08/AC-01` đến `FR-STU-08/AC-06` | MS-02 | không |
| `TS-29` | `FR-STU-09` | `FR-STU-09/AC-01` đến `FR-STU-09/AC-06` | MS-03 | không |
| `TS-30` | `FR-STU-10` | `FR-STU-10/AC-01` đến `FR-STU-10/AC-06` | MS-04 | không |
| `TS-31` | `FR-STF-01` | `FR-STF-01/AC-01` đến `FR-STF-01/AC-06` | MS-02 | không |
| `TS-32` | `FR-STF-02` | `FR-STF-02/AC-01` đến `FR-STF-02/AC-06` | MS-02 | không |
| `TS-33` | `FR-STF-03` | `FR-STF-03/AC-01` đến `FR-STF-03/AC-06` | MS-02 | không |
| `TS-34` | `FR-STF-04` | `FR-STF-04/AC-01` đến `FR-STF-04/AC-06` | MS-03 | không |
| `TS-35` | `FR-STF-05` | `FR-STF-05/AC-01` đến `FR-STF-05/AC-06` | MS-03 | không |
| `TS-36` | `FR-STF-06` | `FR-STF-06/AC-01` đến `FR-STF-06/AC-06` | MS-04 | không |
| `TS-37` | `FR-STF-07` | `FR-STF-07/AC-01` đến `FR-STF-07/AC-06` | MS-03 | không |
| `TS-38` | `FR-STF-08` | `FR-STF-08/AC-01` đến `FR-STF-08/AC-06` | MS-03 | không |
| `TS-39` | `FR-STF-09` | `FR-STF-09/AC-01` đến `FR-STF-09/AC-06` | MS-03 | không |
| `TS-40` | `FR-STF-10` | `FR-STF-10/AC-01` đến `FR-STF-10/AC-06` | MS-03 | không |
| `TS-41` | `FR-MGT-01` | `FR-MGT-01/AC-01` đến `FR-MGT-01/AC-06` | MS-05 | không |
| `TS-42` | `FR-MGT-02` | `FR-MGT-02/AC-01` đến `FR-MGT-02/AC-06` | MS-05 | không |
| `TS-43` | `FR-MGT-03` | `FR-MGT-03/AC-01` đến `FR-MGT-03/AC-06` | MS-05 | không |
| `TS-44` | `FR-MGT-04` | `FR-MGT-04/AC-01` đến `FR-MGT-04/AC-06` | MS-05 | không |
| `TS-45` | `FR-MGT-05` | `FR-MGT-05/AC-01` đến `FR-MGT-05/AC-06` | MS-05 | không |
| `TS-46` | `FR-MGT-06` | `FR-MGT-06/AC-01` đến `FR-MGT-06/AC-06` | MS-01 | không |
| `TS-47` | `FR-MGT-07` | `FR-MGT-07/AC-01` đến `FR-MGT-07/AC-06` | MS-02 | không |
| `TS-48` | `FR-SEC-01` | `FR-SEC-01/AC-01` đến `FR-SEC-01/AC-06` | MS-01 | không |
| `TS-49` | `FR-SEC-02` | `FR-SEC-02/AC-01` đến `FR-SEC-02/AC-06` | MS-01 | không |
| `TS-50` | `FR-SEC-03` | `FR-SEC-03/AC-01` đến `FR-SEC-03/AC-06` | MS-01 | không |
| `TS-51` | `FR-SEC-04` | `FR-SEC-04/AC-01` đến `FR-SEC-04/AC-06` | MS-01, MS-02, MS-03, MS-04, MS-05 | không |
| `TS-52` | `FR-SEC-05` | `FR-SEC-05/AC-01` đến `FR-SEC-05/AC-06` | MS-02 | không |
| `TS-53` | `FR-SEC-06` | `FR-SEC-06/AC-01` đến `FR-SEC-06/AC-06` | MS-01, MS-02, MS-03, MS-04 | không |
| `TS-54` | `FR-CORE-01` | `FR-CORE-01/AC-01` đến `FR-CORE-01/AC-06` | MS-02 | không |
| `TS-55` | `FR-CORE-02` | `FR-CORE-02/AC-01` đến `FR-CORE-02/AC-06` | MS-02 | không |
| `TS-56` | `FR-CORE-03` | `FR-CORE-03/AC-01` đến `FR-CORE-03/AC-06` | MS-02 | không |
| `TS-57` | `FR-CORE-04` | `FR-CORE-04/AC-01` đến `FR-CORE-04/AC-06` | MS-02 | không |
| `TS-58` | `FR-CORE-05` | `FR-CORE-05/AC-01` đến `FR-CORE-05/AC-06` | MS-02 | không |
| `TS-59` | `FR-CORE-06` | `FR-CORE-06/AC-01` đến `FR-CORE-06/AC-06` | MS-02, MS-03, MS-04 | không |
| `TS-60` | `FR-CORE-07` | `FR-CORE-07/AC-01` đến `FR-CORE-07/AC-06` | MS-02 | không |
| `TS-61` | `FR-CORE-08` | `FR-CORE-08/AC-01` đến `FR-CORE-08/AC-06` | MS-01, MS-02, MS-03, MS-04, MS-05 | không |
| `TS-62` | `FR-CORE-09` | `FR-CORE-09/AC-01` đến `FR-CORE-09/AC-06` | MS-01, MS-02, MS-03, MS-04, MS-05 | không |
