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
| `TS-03` | Thông báo quá hạn, danh sách trạng thái và đổi ngôn ngữ cho sinh viên | 11 | NFR-PERF-08 | MS-02 | có |
| `TS-04` | Phân công, ghi nhật ký, yêu cầu bổ sung, bổ sung và thông báo | 10 | — | MS-03 | có |
| `TS-05` | Ưu tiên, escalate và chuyển phòng | 6 | — | MS-03 | có |
| `TS-06` | Quá hạn tự động | 5 | NFR-PERF-09, NFR-REL-07 | MS-03 | có |
| `TS-07` | Đóng hồ sơ, trả kết quả, lịch sử công khai và đánh giá | 7 | — | MS-04 | có |
| `TS-08` | Báo cáo và dashboard của Admin khớp dữ liệu | 7 | NFR-SEC-12 | MS-05 | có |
| `TS-09` | Phạm vi dữ liệu và truy cập chéo | 6 | NFR-SEC-03, NFR-SEC-06 | MS-01 đến MS-05 | có |
| `TS-10` | Thao tác đồng thời | 9 | NFR-REL-04 | MS-01 đến MS-04 | có |
| `TS-11` | Mất phản hồi và gửi lại | 7 | NFR-REL-03 | MS-02 đến MS-04 | có |
| `TS-12` | Lỗi tệp, kho tệp và CSDL | 4 | NFR-REL-01, NFR-REL-06, NFR-PERF-14 | MS-02 | có |
| `TS-13` | Lịch sử hồ sơ và nhật ký hệ thống | 13 | NFR-REL-02, NFR-SEC-09, NFR-OBS-04 | MS-01 đến MS-04 | có |
| `TS-14` | Phiên, đăng xuất và cache | 6 | NFR-SEC-02, NFR-SEC-07 | MS-01 | có |
| `TS-15` | VN/EN, responsive và trình duyệt | 14 | NFR-USE-02, NFR-USE-03, NFR-USE-04 | MS-01 đến MS-05 | có |
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
| 3 | SV1 truy cập đường dẫn hoặc API dành cho Cổng Nhân viên / Cổng Admin | 403 |
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

### TS-03. Thông báo quá hạn, danh sách trạng thái và đổi ngôn ngữ cho sinh viên

**Kiểm AC:** `FR-STU-06/AC-01`, `FR-STU-08/AC-05`, `FR-STU-08/AC-07`, `FR-STU-08/AC-08`, `FR-STU-08/AC-09`, `FR-STU-08/AC-10`, `FR-CORE-02/AC-01`, `FR-CORE-03/AC-01`, `FR-CORE-06/AC-03`, `FR-CORE-08/AC-01`, `FR-CORE-08/AC-08`

**Kiểm NFR:** `NFR-PERF-08`

**Milestone chạy lần đầu:** MS-02 · **Regression:** có

*(Chỉ sử dụng các chức năng đã có trên Staging tại `MS-02`: tạo yêu cầu loại Y có SLA ngắn trên Staging, tác vụ quá hạn tự động `NEW → OVERDUE`, danh sách yêu cầu sinh viên, chuông thông báo, lịch sử tạo hồ sơ và chuyển ngôn ngữ VN/EN. Thông báo phân công/bổ sung được kiểm ở `TS-04` tại `MS-03`; trang chi tiết và lịch sử 5 mốc của sinh viên được kiểm ở `TS-07` tại `MS-04`.)*

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | SV1 gửi một yêu cầu loại Y (loại cấu hình SLA ngắn trên Staging để test quá hạn); A1 mở chi tiết hồ sơ trên Cổng Nhân viên | Hồ sơ ở trạng thái `NEW`; mốc tạo hồ sơ (`CREATE`) có phòng ban, mã hồ sơ và ngày gửi; hạn xử lý chỉ hiện ở dữ liệu nội bộ cho A1, không lộ ở danh sách/API của SV1 |
| 2 | Chờ qua hạn SLA của hồ sơ loại Y (không cần phân công) | Muộn nhất sau 60 giây hồ sơ tự chuyển `NEW → OVERDUE`; SV1 nhận 1 thông báo quá hạn, số chưa đọc trên chuông tăng 1 trong vòng 30 giây |
| 3 | SV1 mở danh sách yêu cầu của mình (`FR-STU-06`) và mở danh sách thông báo (`FR-STU-08`) | Hồ sơ hiển thị trạng thái `OVERDUE` (Quá hạn) trên danh sách yêu cầu; danh sách thông báo hiển thị 20 mục mới nhất/trang với thông báo quá hạn ở đầu |
| 4 | SV1 đổi giao diện từ VN sang EN | Nhãn trạng thái, danh mục và cả thông báo quá hạn đã nhận trước đó hiển thị bằng tiếng Anh; mô tả SV1 nhập vẫn giữ nguyên ngôn ngữ gốc; lần đăng nhập sau vẫn là EN |
| 5 | SV1 đánh dấu đã đọc 1 thông báo (gửi lại lần 2 cùng `notification_id`), sau đó bấm "Đánh dấu tất cả đã đọc" với `read_before` | Đánh dấu lặp trả 200 idempotent; sau "Đánh dấu tất cả đã đọc", số chưa đọc về 0; thông báo mới phát sinh sau mốc `read_before` vẫn giữ trạng thái chưa đọc |
| 6 | Giả lập API đếm thông báo chưa đọc lỗi khi trang đang mở | Chuông thông báo giữ nguyên số cũ, không trắng trang và tự gọi lại ở chu kỳ polling 30 giây kế tiếp |

### TS-04. Phân công, ghi nhật ký, yêu cầu bổ sung, bổ sung và thông báo

**Kiểm AC:** `FR-STF-04/AC-01`, `FR-STF-04/AC-05`, `FR-STF-05/AC-01`, `FR-STF-07/AC-01`, `FR-STU-09/AC-01`, `FR-STU-08/AC-01`, `FR-STU-08/AC-02`, `FR-CORE-03/AC-01`, `FR-CORE-03/AC-02`, `FR-CORE-06/AC-02`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 phân công A1 cho hồ sơ `NEW` của SV1 | Hồ sơ chuyển `IN_PROGRESS`; mốc phân công chỉ hiện trong lịch sử nội bộ; SV1 nhận 1 thông báo trạng thái, chuông tăng 1 trong vòng 30 giây |
| 2 | A2 (không phụ trách) ghi nhật ký xử lý hoặc gửi yêu cầu bổ sung | Cả hai thao tác trả 403 *(quyền chặn đóng hồ sơ bởi người không phụ trách kiểm tại `TS-07` ở `MS-04`)* |
| 3 | A1 ghi nhật ký xử lý | Nhân viên phòng A thấy; SV1 không thấy |
| 4 | A1 yêu cầu bổ sung (nội dung 10–1000 ký tự) khi hồ sơ còn hạn | Hồ sơ chuyển `PENDING_INFO`; SLA tạm dừng, thời gian còn lại giữ cố định; SV1 nhận 1 thông báo yêu cầu bổ sung; mốc yêu cầu bổ sung trả cho SV1 có nội dung yêu cầu, không có tên nhân viên |
| 5 | A1 gửi thêm một yêu cầu bổ sung khi yêu cầu cũ còn mở | 409 |
| 6 | Sau 6 giờ, SV1 bổ sung (nội dung 10–2000 ký tự, tối đa 5 tệp) | Hồ sơ chuyển lại `IN_PROGRESS`; hạn xử lý lùi đúng 6 giờ; có mốc nội bộ thay đổi hạn; không tạo thông báo mới cho hành động của chính SV1 |
| 7 | SV1 bổ sung lần hai cho cùng yêu cầu | Bị từ chối; mỗi yêu cầu chỉ được trả lời một lần |

### TS-05. Ưu tiên, escalate và chuyển phòng

**Kiểm AC:** `FR-STF-08/AC-01`, `FR-STF-09/AC-01`, `FR-STF-10/AC-01`, `FR-CORE-04/AC-01`, `FR-SEC-04/AC-05`, `FR-CORE-06/AC-04`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A1 đặt ưu tiên `LOW` cho một hồ sơ, có hồ sơ khác ở `MEDIUM` và `HIGH` | Hàng đợi xếp `HIGH`, `MEDIUM`, `LOW`; SV1 không thấy mức ưu tiên |
| 2 | A1 escalate kèm lý do 10–500 ký tự, hai lần | `LOW` → `MEDIUM` → `HIGH`; hạn xử lý không đổi |
| 3 | A1 chuyển hồ sơ sang phòng B kèm lý do | Trạng thái, hạn xử lý, loại yêu cầu và yêu cầu bổ sung đang mở giữ nguyên; SV1 không nhận thông báo |
| 4 | A1 mở lại hồ sơ hoặc tải tệp của hồ sơ đó | 404 |
| 5 | B1 mở hàng đợi và chi tiết | Thấy hồ sơ, xem được tệp và lý do chuyển |
| 6 | SV1 mở danh sách yêu cầu của mình (`FR-STU-06`) và kiểm tra dữ liệu lịch sử công khai trả về | Thấy hồ sơ đã chuyển sang phòng B; API không trả về mốc nội bộ hay lý do chuyển phòng |
| 7 | B1 chuyển hồ sơ sang chính phòng B | Bị từ chối; phòng đích phải khác phòng hiện tại |

### TS-06. Quá hạn tự động

**Kiểm AC:** `FR-CORE-02/AC-01`, `FR-CORE-03/AC-01`, `FR-CORE-03/AC-02`, `FR-STU-08/AC-08`, `FR-STF-07/AC-01`

**Kiểm NFR:** `NFR-PERF-09`, `NFR-REL-07`

**Milestone chạy lần đầu:** MS-03 · **Regression:** có

*(Số đếm Quá hạn trên dashboard Admin thuộc `FR-MGT-01` ở `MS-05`, được kiểm tại `TS-08` bước 1.)*

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | Hồ sơ `IN_PROGRESS` loại Y, hạn 10:00:00 | Lúc 09:59:59 vẫn `IN_PROGRESS`; muộn nhất 10:01:00 thành `OVERDUE`; có `STATUS_CHANGE` do `SYSTEM`; SV1 nhận 1 thông báo; trạng thái `OVERDUE` cập nhật trong vòng 30 giây trên hàng đợi phòng A (`FR-STF-01`) và danh sách yêu cầu của SV1 (`FR-STU-06`) |
| 2 | Hồ sơ `PENDING_INFO` đã qua hạn ban đầu | Giữ `PENDING_INFO` |
| 3 | Hồ sơ `NEW` quá hạn chưa phân công, sau đó được phân công | Thành `OVERDUE`; sau khi phân công vẫn `OVERDUE` |
| 4 | Người phụ trách yêu cầu bổ sung cho hồ sơ `OVERDUE`, sinh viên bổ sung | Trạng thái giữ `OVERDUE` suốt quá trình; sinh viên vẫn nhận 1 thông báo yêu cầu bổ sung |
| 5 | Dừng tác vụ quá hạn giữa chừng rồi chạy lại | Hồ sơ còn lại được xử lý tiếp; không tạo trùng lịch sử hay thông báo |

### TS-07. Đóng hồ sơ, trả kết quả, lịch sử công khai và đánh giá

**Kiểm AC:** `FR-STF-06/AC-01`, `FR-STU-07/AC-01`, `FR-STU-08/AC-08`, `FR-STU-10/AC-01`, `FR-STU-10/AC-04`, `FR-CORE-06/AC-01`, `FR-SEC-04/AC-03`

**Milestone chạy lần đầu:** MS-04 · **Regression:** có

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | A2 (cùng phòng nhưng không phụ trách) gọi API đóng hồ sơ `IN_PROGRESS` của A1; sau đó A1 đóng hồ sơ với kết quả 10–2000 ký tự | Lệnh của A2 trả 403; lệnh của A1 chuyển hồ sơ sang `CLOSED` và SV1 nhận 1 thông báo đóng hồ sơ (cập nhật trong 30 giây) |
| 2 | Đóng một hồ sơ đang `NEW` | 409 |
| 3 | SV1 mở trang chi tiết hồ sơ (`FR-STU-07`) | Thấy đầy đủ 5 mốc lịch sử công khai (`CREATE` → `IN_PROGRESS` → `REQUEST_INFO` → `SUPPLEMENT` → `CLOSED`), không có tên nhân viên, hạn xử lý, mức ưu tiên hay lý do nội bộ; thấy kết quả xử lý, thời điểm đóng và form đánh giá |
| 4 | SV1 đánh giá 4 sao kèm nhận xét | 201; đánh giá được lưu, ghi nhật ký `RATE_TICKET` và hiển thị ở chế độ chỉ đọc trên trang chi tiết *(việc tổng hợp điểm đánh giá vào báo cáo hài lòng Admin thuộc `FR-MGT-05`, được kiểm tại `TS-08` ở `MS-05`)* |
| 5 | SV1 đánh giá lần hai; đánh giá 0, 6 hoặc 3,5 sao; nhận xét 1001 ký tự | 409; 400; 400 |
| 6 | Gọi bất kỳ thao tác sửa nào trên hồ sơ `CLOSED` | 409; không có chức năng mở lại |
| 7 | Đóng một hồ sơ đang `PENDING_INFO` | Yêu cầu bổ sung đang mở được đóng; khoảng tạm dừng cuối tính đến lúc đóng |

### TS-08. Báo cáo và dashboard của Admin khớp dữ liệu

**Kiểm AC:** `FR-MGT-01/AC-01`, `FR-MGT-02/AC-01`, `FR-MGT-02/AC-02`, `FR-MGT-03/AC-01`, `FR-MGT-04/AC-01`, `FR-MGT-05/AC-01`, `FR-SEC-04/AC-01`

**Kiểm NFR:** `NFR-SEC-12`

**Milestone chạy lần đầu:** MS-05 · **Regression:** có

**Tiền điều kiện:** đã nạp bộ 200 ticket mẫu và có bảng kết quả tính sẵn cho từng chỉ số.

| # | Bước | Kết quả mong đợi |
|---|---|---|
| 1 | Admin mở dashboard; cho 1 hồ sơ loại Y vừa hết hạn chuyển sang `OVERDUE` | Mới = số `NEW`; Đang xử lý = `IN_PROGRESS` + `PENDING_INFO`; Quá hạn = `OVERDUE`; không tính `CLOSED`; khớp bảng tính sẵn; số Quá hạn trên dashboard tăng thêm 1 trong vòng 30 giây sau khi hồ sơ chuyển `OVERDUE` |
| 2 | Bấm từng chỉ số | Danh sách tóm tắt (mã, loại, phòng, trạng thái, ngày gửi); không có tên, MSSV, email, số điện thoại; không có thao tác xử lý |
| 3 | Xem thời gian xử lý trung bình | Bằng thời điểm đóng trừ thời điểm gửi, trừ tổng thời gian chờ bổ sung; khớp bảng tính sẵn |
| 4 | Xem báo cáo loại yêu cầu, khối lượng phòng ban và mức độ hài lòng (bao gồm hồ sơ đã đánh giá 4 sao ở `TS-07` và bộ 200 ticket mẫu) với khoảng ngày mặc định và khoảng tự chọn | Tất cả chỉ số và phân bố sao khớp 100% bảng tính sẵn |
| 5 | Nhập `from` sau `to`, hoặc khoảng dài hơn 365 ngày | 400 |
| 6 | Chọn khoảng không có dữ liệu | Hiện 0 hoặc trống, không lỗi |
| 7 | Admin gọi API chi tiết hồ sơ hoặc tải tệp | 403 |

## Kịch bản chéo (TS-09 đến TS-20)

### TS-09. Phạm vi dữ liệu và truy cập chéo

**Kiểm AC:** `FR-SEC-03/AC-04`, `FR-SEC-04/AC-02`, `FR-SEC-04/AC-04`, `FR-SEC-04/AC-05`, `FR-SEC-04/AC-06`, `FR-SEC-05/AC-03`

**Kiểm NFR:** `NFR-SEC-03`, `NFR-SEC-06`

**Milestone chạy lần đầu:** MS-01 đến MS-05 · **Regression:** có

- SV1 đổi mã hồ sơ trên URL thành hồ sơ của SV2 → 404, giống hệt hồ sơ không tồn tại.
- A1 mở hồ sơ của phòng B → 404.
- Gửi `student_id` hoặc `department_id` khác trong body hoặc query → hệ thống bỏ qua.
- Admin gọi API chi tiết hồ sơ, tải tệp, đóng hồ sơ → 403.
- Truy cập thẳng đường dẫn lưu tệp trên server → không truy cập được.
- Sửa giao diện để hiện nút của vai trò khác rồi gọi API → 403.

### TS-10. Thao tác đồng thời

**Kiểm AC:** `FR-STF-06/AC-05`, `FR-STF-07/AC-05`, `FR-STF-09/AC-04`, `FR-STU-09/AC-05`, `FR-MGT-06/AC-05`, `FR-MGT-07/AC-05`, `FR-CORE-01/AC-05`, `FR-CORE-02/AC-05`, `FR-SEC-04/AC-07`

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

**Kiểm AC:** `FR-STU-02/AC-04`, `FR-STU-09/AC-05`, `FR-STU-10/AC-05`, `FR-STF-04/AC-04`, `FR-STF-05/AC-05`, `FR-STF-06/AC-01`, `FR-CORE-09/AC-06`

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

**Kiểm AC:** `FR-CORE-06/AC-01`, `FR-CORE-06/AC-02`, `FR-CORE-06/AC-03`, `FR-CORE-06/AC-04`, `FR-CORE-06/AC-05`, `FR-CORE-06/AC-06`, `FR-SEC-06/AC-01`, `FR-SEC-06/AC-02`, `FR-SEC-06/AC-03`, `FR-SEC-06/AC-04`, `FR-SEC-06/AC-05`, `FR-SEC-06/AC-06`, `FR-SEC-06/AC-07`

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

**Kiểm AC:** `FR-CORE-08/AC-01`, `FR-CORE-08/AC-02`, `FR-CORE-08/AC-03`, `FR-CORE-08/AC-04`, `FR-CORE-08/AC-05`, `FR-CORE-08/AC-06`, `FR-CORE-09/AC-01`, `FR-CORE-09/AC-02`, `FR-CORE-09/AC-03`, `FR-CORE-09/AC-04`, `FR-CORE-09/AC-05`, `FR-CORE-09/AC-06`, `FR-CORE-09/AC-07`, `FR-CORE-09/AC-08`

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
| `TS-21` | `FR-STU-01` | `FR-STU-01/AC-01` đến `FR-STU-01/AC-07` | MS-01 | không |
| `TS-22` | `FR-STU-02` | `FR-STU-02/AC-01` đến `FR-STU-02/AC-10` | MS-02 | không |
| `TS-23` | `FR-STU-03` | `FR-STU-03/AC-01` đến `FR-STU-03/AC-07` | MS-02 | không |
| `TS-24` | `FR-STU-04` | `FR-STU-04/AC-01` đến `FR-STU-04/AC-09` | MS-02 | không |
| `TS-25` | `FR-STU-05` | `FR-STU-05/AC-01` đến `FR-STU-05/AC-07` | MS-02 | không |
| `TS-26` | `FR-STU-06` | `FR-STU-06/AC-01` đến `FR-STU-06/AC-08` | MS-02 | không |
| `TS-27` | `FR-STU-07` | `FR-STU-07/AC-01` đến `FR-STU-07/AC-08` | MS-04 | không |
| `TS-28` | `FR-STU-08` | `FR-STU-08/AC-01` đến `FR-STU-08/AC-10` | MS-02 | không |
| `TS-29` | `FR-STU-09` | `FR-STU-09/AC-01` đến `FR-STU-09/AC-10` | MS-03 | không |
| `TS-30` | `FR-STU-10` | `FR-STU-10/AC-01` đến `FR-STU-10/AC-08` | MS-04 | không |
| `TS-31` | `FR-STF-01` | `FR-STF-01/AC-01` đến `FR-STF-01/AC-09` | MS-02 | không |
| `TS-32` | `FR-STF-02` | `FR-STF-02/AC-01` đến `FR-STF-02/AC-07` | MS-02 | không |
| `TS-33` | `FR-STF-03` | `FR-STF-03/AC-01` đến `FR-STF-03/AC-08` | MS-02 | không |
| `TS-34` | `FR-STF-04` | `FR-STF-04/AC-01` đến `FR-STF-04/AC-09` | MS-03 | không |
| `TS-35` | `FR-STF-05` | `FR-STF-05/AC-01` đến `FR-STF-05/AC-08` | MS-03 | không |
| `TS-36` | `FR-STF-06` | `FR-STF-06/AC-01` đến `FR-STF-06/AC-08` | MS-04 | không |
| `TS-37` | `FR-STF-07` | `FR-STF-07/AC-01` đến `FR-STF-07/AC-08` | MS-03 | không |
| `TS-38` | `FR-STF-08` | `FR-STF-08/AC-01` đến `FR-STF-08/AC-07` | MS-03 | không |
| `TS-39` | `FR-STF-09` | `FR-STF-09/AC-01` đến `FR-STF-09/AC-08` | MS-03 | không |
| `TS-40` | `FR-STF-10` | `FR-STF-10/AC-01` đến `FR-STF-10/AC-07` | MS-03 | không |
| `TS-41` | `FR-MGT-01` | `FR-MGT-01/AC-01` đến `FR-MGT-01/AC-08` | MS-05 | không |
| `TS-42` | `FR-MGT-02` | `FR-MGT-02/AC-01` đến `FR-MGT-02/AC-08` | MS-05 | không |
| `TS-43` | `FR-MGT-03` | `FR-MGT-03/AC-01` đến `FR-MGT-03/AC-08` | MS-05 | không |
| `TS-44` | `FR-MGT-04` | `FR-MGT-04/AC-01` đến `FR-MGT-04/AC-07` | MS-05 | không |
| `TS-45` | `FR-MGT-05` | `FR-MGT-05/AC-01` đến `FR-MGT-05/AC-08` | MS-05 | không |
| `TS-46` | `FR-MGT-06` | `FR-MGT-06/AC-01` đến `FR-MGT-06/AC-09` | MS-01 | không |
| `TS-47` | `FR-MGT-07` | `FR-MGT-07/AC-01` đến `FR-MGT-07/AC-08` | MS-02 | không |
| `TS-48` | `FR-SEC-01` | `FR-SEC-01/AC-01` đến `FR-SEC-01/AC-09` | MS-01 | không |
| `TS-49` | `FR-SEC-02` | `FR-SEC-02/AC-01` đến `FR-SEC-02/AC-07` | MS-01 | không |
| `TS-50` | `FR-SEC-03` | `FR-SEC-03/AC-01` đến `FR-SEC-03/AC-08` | MS-01 | không |
| `TS-51` | `FR-SEC-04` | `FR-SEC-04/AC-01` đến `FR-SEC-04/AC-08` *(phân tách từng AC theo mốc ở bảng dưới)* | MS-01 (`AC-06`), MS-02 (`AC-02,08`), MS-03 (`AC-05`), MS-04 (`AC-03`), MS-05 (`AC-01,04,07`) | có (theo `TS-09`) |
| `TS-52` | `FR-SEC-05` | `FR-SEC-05/AC-01` đến `FR-SEC-05/AC-07` *(đủ 7 AC ở `#123`; `AC-04` regression trọn luồng ở `MS-03`)* | MS-02 | có (theo `TS-09`, `TS-12`) |
| `TS-53` | `FR-SEC-06` | `FR-SEC-06/AC-01` đến `FR-SEC-06/AC-09` *(phân tách từng AC theo mốc ở bảng dưới)* | MS-01 (`AC-04,06,08`), MS-02 (`AC-02,09`), MS-03 (`AC-01,03,05,07`) | có (theo `TS-13`) |
| `TS-54` | `FR-CORE-01` | `FR-CORE-01/AC-01` đến `FR-CORE-01/AC-08` | MS-02 | không |
| `TS-55` | `FR-CORE-02` | `FR-CORE-02/AC-01` đến `FR-CORE-02/AC-08` | MS-02 | không |
| `TS-56` | `FR-CORE-03` | `FR-CORE-03/AC-01` đến `FR-CORE-03/AC-09` | MS-02 | không |
| `TS-57` | `FR-CORE-04` | `FR-CORE-04/AC-01` đến `FR-CORE-04/AC-07` | MS-02 | không |
| `TS-58` | `FR-CORE-05` | `FR-CORE-05/AC-01` đến `FR-CORE-05/AC-08` | MS-02 | không |
| `TS-59` | `FR-CORE-06` | `FR-CORE-06/AC-01` đến `FR-CORE-06/AC-08` *(phân tách từng AC theo mốc ở bảng dưới)* | MS-02 (`AC-03,05,06`), MS-03 (`AC-02,04,07`), MS-04 (`AC-01,08`) | có (theo `TS-13`) |
| `TS-60` | `FR-CORE-07` | `FR-CORE-07/AC-01` đến `FR-CORE-07/AC-08` | MS-02 | không |
| `TS-61` | `FR-CORE-08` | `FR-CORE-08/AC-01` đến `FR-CORE-08/AC-08` *(phân tách từng AC theo mốc ở bảng dưới)* | MS-01 (`AC-03,04,06,07`), MS-02 (`AC-01,02,05,08`) | có (theo `TS-15`) |
| `TS-62` | `FR-CORE-09` | `FR-CORE-09/AC-01` đến `FR-CORE-09/AC-08` *(phân tách từng AC theo mốc ở bảng dưới)* | MS-01 (`AC-02,04,07`), MS-02 (`AC-01,03,05,06`), MS-05 (`AC-08`) | có (theo `TS-15`) |

### Phân bổ chạy kiểm thử 48 AC của 6 FR dùng chung theo từng milestone

| Kịch bản | FR dùng chung | `MS-01` (`Q-02`, `#117`/`#118`) | `MS-02` (`Q-03`, `#123`/`#124`) | `MS-03` (`Q-04`, `#129`/`#130`) | `MS-04` (`Q-05`, `#155`/`#156`) | `MS-05` (`Q-06`, `#157`/`#158`) |
|---|---|---|---|---|---|---|
| `TS-51` | `FR-SEC-04` (8 AC) | Đạt lần đầu: `AC-06` (`#117`) | Đạt lần đầu: `AC-02, AC-08` (`#123`); triển khai phần SV/NV của `AC-01,04,07` | Đạt lần đầu: `AC-05` (`#129`); nhánh phân công `AC-03`; regression `AC-02,06,08` | Đạt lần đầu: `AC-03` (`#155`); regression `AC-02,05,06,08` | Đạt lần đầu: `AC-01, AC-04, AC-07` (`#157`); regression toàn bộ |
| `TS-52` | `FR-SEC-05` (7 AC) | — | Đạt lần đầu: **`AC-01` đến `AC-07`** (`#123`, trong đó `AC-04` test trên seed đổi phòng) | Regression toàn bộ + kiểm trọn luồng `AC-04` cùng `FR-STF-09` (`#129`, `TS-05`) | Regression `TS-09`, `TS-12` | Regression `TS-09`, `TS-12` |
| `TS-53` | `FR-SEC-06` (9 AC) | Đạt lần đầu: `AC-04, AC-06, AC-08` (`#117`) | Đạt lần đầu: `AC-02, AC-09` (`#123`); nhánh `OVERDUE`/`SYSTEM` của `AC-03`; regression `AC-04,06,08` | Đạt lần đầu: `AC-01, AC-03, AC-05, AC-07` (`#129`); regression `AC-02,04,06,08,09` | Mở rộng `CLOSE_TICKET`/`RATE_TICKET` (`#155`); regression `TS-13` | Regression `AC-08` & `TS-13` trên báo cáo Admin |
| `TS-59` | `FR-CORE-06` (8 AC) | — | Đạt lần đầu: `AC-03, AC-05, AC-06` (`#123`); nhánh chi tiết NV của `AC-08` | Đạt lần đầu: `AC-02, AC-04, AC-07` (`#129`); regression `AC-03,05,06` | Đạt lần đầu: `AC-01, AC-08` (`#155`); regression toàn bộ (`TS-07`, `TS-13`) | Regression `TS-07`, `TS-13` |
| `TS-61` | `FR-CORE-08` (8 AC) | Đạt lần đầu: `AC-03, AC-04, AC-06, AC-07` (`#118`) | Đạt lần đầu: `AC-01, AC-02, AC-05, AC-08` (`#124`); regression `AC-03,04,06,07` | Mở rộng & regression `AC-01..08` cho màn hình `MS-03` (`#130`, `TS-15`) | Mở rộng & regression `AC-01..08` cho màn hình `MS-04` (`#156`, `TS-15`) | Mở rộng & regression `AC-01..08` cho dashboard/báo cáo `MS-05` (`#158`, `TS-15`) |
| `TS-62` | `FR-CORE-09` (8 AC) | Đạt lần đầu: `AC-02, AC-04, AC-07` (`#118`) | Đạt lần đầu: `AC-01, AC-03, AC-05, AC-06` (`#124`); regression `AC-02,04,07` | Mở rộng & regression `AC-01..07` + nhánh NV mobile `AC-08` (`#130`, `TS-15`) | Mở rộng & regression `AC-02..07` + nhánh đóng mobile `AC-08` (`#156`, `TS-15`) | Đạt lần đầu: `AC-08` (`#158`, đủ NV + dashboard Admin 360px); regression toàn bộ (`TS-15`) |
