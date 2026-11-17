# TÀI LIỆU DỰ TOÁN EFFORT, BÓC TÁCH NGUỒN LỰC & GIÁ THẦU THƯƠNG MẠI

**DỰ ÁN: HỆ THỐNG TIẾP NHẬN VÀ XỬ LÝ YÊU CẦU SINH VIÊN TRỰC TUYẾN (UniSupport)**

> **Tài liệu nội bộ, không gửi khách.** Khách hàng Aurora University và đội thực thi là **Mô phỏng**. Đội thực thi gồm 10 người, mỗi vị trí ở Phần 1 là 1 người; ban quản lý dự án là 4 thành viên nhóm (xem [team-charter.md](team-charter.md)).
>
> Nơi gốc: giá và hạng mục gửi khách ở [quotation.xlsx](quotation.xlsx) (mã `Q-xx`); lịch, việc và nguồn lực theo tuần ở [UniSupport_Masterplan_nhom2.xlsx](UniSupport_Masterplan_nhom2.xlsx) (sheet WBS, Resources); rủi ro ở [risk-register.md](risk-register.md) (mã `R-xx`). Tài liệu này giữ cơ sở chi phí và thuyết minh phân bổ giờ.

## PHẦN 1: BẢNG DỰ TOÁN TIỀN CÔNG LAO ĐỘNG THEO NGƯỜI THAM GIA (ĐỘI THỰC THI MÔ PHỎNG)

| STT | Vị trí nhân sự & Cấp bậc | Lương gross/tháng (đồng) | Đơn giá/giờ (đồng) | Tổng giờ (h) | Tổng quy đổi (PD) | Thành tiền công (đồng) |
|---|---|---|---|---|---|---|
| 1 | PM / Senior | 50.100.000 | 240.865,38 | 74 | 9,25 | 17.824.038 |
| 2 | BA / Mid | 28.000.000 | 134.615,38 | 90 | 11,25 | 12.115.385 |
| 3 | UI/UX / Mid | 26.500.000 | 127.403,85 | 68 | 8,50 | 8.663.462 |
| 4 | BE Dev / Senior | 51.800.000 | 249.038,46 | 94 | 11,75 | 23.409.615 |
| 5 | BE Dev / Mid | 32.000.000 | 153.846,15 | 150 | 18,75 | 23.076.923 |
| 6 | FE Dev / Senior | 51.800.000 | 249.038,46 | 70 | 8,75 | 17.432.692 |
| 7 | FE Dev / Junior | 16.500.000 | 79.326,92 | 110 | 13,75 | 8.725.962 |
| 8 | QA / Senior | 46.200.000 | 222.115,38 | 52 | 6,50 | 11.550.000 |
| 9 | Tester / Junior | 13.500.000 | 64.903,85 | 88 | 11,00 | 5.711.538 |
| 10 | DevOps / Mid | 39.800.000 | 191.346,15 | 74 | 9,25 | 14.159.615 |
| — | **TỔNG CỘNG** | — | — | **870 h** | **108,75 PD** | **142.669.230** |

**Ghi chú chuyên môn:**

- Đơn giá/giờ = Lương gross/tháng ÷ 208 giờ, hiển thị làm tròn 2 chữ số thập phân; thành tiền tính từ đơn giá chưa làm tròn. 1 PD = 8 giờ.
- Senior tập trung vào kiến trúc, phần kỹ thuật phức tạp và review mã nguồn; Mid và Junior thực hiện phần chuẩn hóa dưới sự kèm cặp.

## PHẦN 2: BÓC TÁCH EFFORT THEO MILESTONE VÀ HẠNG MỤC BÁO GIÁ

Effort được bóc tách theo 6 milestone `MS-01` đến `MS-06` và việc quản lý dự án xuyên suốt, đúng với các issue trên GitHub và hạng mục `Q-xx` trong báo giá. Mỗi hạng mục có một quỹ giờ dự phòng (Buffer) cho các rủi ro đã ghi trong sổ rủi ro; bảng dưới chỉ dẫn mã `R-xx`, mô tả, owner và phương án nằm ở [risk-register.md](risk-register.md).

**Cơ sở phân bổ:** tổng giờ của từng vị trí ở Phần 1 được chia cho các issue người đó phụ trách, theo tỷ trọng số ngày công ước lượng của issue; mỗi issue tính tối đa 10 ngày; vai trò hỗ trợ/duyệt tính 25%. Lịch thực hiện của từng issue nằm ở sheet WBS; đổi lịch không làm đổi giờ.

### 2.1. MS-01 · CHỐT YÊU CẦU, ĐĂNG NHẬP VÀ PHÂN QUYỀN — TỔNG: 359 GIỜ (44,88 PD)

Milestone gồm hai hạng mục báo giá: `Q-01` (285 giờ) và `Q-02` (74 giờ). Cuối milestone khách đăng nhập thử được trên Staging.

#### 2.1a. Q-01 · Phân tích, thiết kế, prototype và chốt yêu cầu — 285 giờ (35,63 PD)

**Mục tiêu cốt lõi:** Khách bấm thử prototype 3 cổng trên dữ liệu mẫu; nhóm bàn giao bộ tài liệu phân tích kỹ thuật, luồng dữ liệu, UI/UX và chốt phạm vi cùng tiêu chí chất lượng bằng văn bản.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| PM / Senior | Kickoff, phân vai, lập kế hoạch; xác nhận với trường danh mục loại yêu cầu, phòng ban và cách đăng nhập; chốt phạm vi và tiêu chí chất lượng bằng văn bản | 38 | 4,75 |
| BA / Mid | Viết đặc tả chức năng, chỉnh theo phản hồi của khách; soạn bộ tài liệu phân tích kỹ thuật, luồng dữ liệu, UI/UX để khách duyệt khi chốt yêu cầu | 72 | 9,00 |
| UI/UX / Mid | Thiết kế wireframe và giao diện 3 cổng (VN/EN, điện thoại và máy tính); hỗ trợ chỉnh prototype theo phản hồi | 50 | 6,25 |
| BE Dev / Senior | Thiết kế kiến trúc hệ thống, ERD, sơ đồ luồng dữ liệu | 20 | 2,50 |
| BE Dev / Mid | Hỗ trợ dựng ERD và sơ đồ luồng dữ liệu | 7 | 0,88 |
| FE Dev / Senior | Dựng prototype Cổng Nhân viên và Cổng Admin; chỉnh prototype theo phản hồi | 20 | 2,50 |
| FE Dev / Junior | Dựng prototype Cổng Sinh viên | 28 | 3,50 |
| QA / Senior | Soạn tiêu chí chất lượng và kế hoạch test | 18 | 2,25 |
| DevOps / Mid | Dựng repo, CI, Docker, môi trường dev và SSO giả lập | 32 | 4,00 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 261 h | 32,63 PD |
| **Buffer dự phòng (9,2%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 24 h | 3,00 PD |
| **TỔNG KẾ HOẠCH (Q-01)** | Tổng effort được phê duyệt | **285 h** | **35,63 PD** |

**Dự phòng 24 giờ dùng cho rủi ro:** R-01, R-02, R-03. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

#### 2.1b. Q-02 · Đăng nhập và phân quyền — 74 giờ (9,25 PD)

**Mục tiêu cốt lõi:** Admin thêm email nhân viên, cấp/thu quyền, vô hiệu hóa tài khoản; 3 vai trò đăng nhập đúng cổng bằng tài khoản trường; sinh viên xem thông tin lấy từ tài khoản trường. Có Staging và dữ liệu seed.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| BE Dev / Senior | Đăng nhập bằng tài khoản trường, phân quyền theo 3 vai trò, quản lý phân quyền cho Admin; sửa lỗi backend của milestone | 14 | 1,75 |
| BE Dev / Mid | Đăng xuất, thông tin cá nhân lấy từ tài khoản trường; chặn truy cập sai vai trò và ghi nhật ký đăng nhập, cấp/thu quyền; hỗ trợ dữ liệu seed | 13 | 1,63 |
| FE Dev / Senior | Màn hình đăng nhập, đăng xuất, quản lý phân quyền; sửa lỗi frontend của milestone | 11 | 1,38 |
| FE Dev / Junior | Màn hình thông tin cá nhân; VN/EN và responsive cho các màn của milestone | 7 | 0,88 |
| QA / Senior | Viết test case từ tiêu chí chấp nhận của milestone | 5 | 0,63 |
| Tester / Junior | Chuẩn bị dữ liệu seed (tài khoản 3 vai trò, danh mục, SLA mặc định, hồ sơ ở mọi trạng thái); chạy test trọn luồng trên Staging | 14 | 1,75 |
| DevOps / Mid | Dựng Staging và CI tự động deploy lên Staging | 10 | 1,25 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 68 h | 8,50 PD |
| **Buffer dự phòng (8,8%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 6 h | 0,75 PD |
| **TỔNG KẾ HOẠCH (Q-02)** | Tổng effort được phê duyệt | **74 h** | **9,25 PD** |

**Dự phòng 6 giờ dùng cho rủi ro:** R-04, R-05, R-06. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.2. MS-02 · GỬI VÀ TIẾP NHẬN YÊU CẦU (Q-03) — TỔNG: 128 GIỜ (16,00 PD)

**Mục tiêu cốt lõi:** Sinh viên chọn loại yêu cầu, thấy phòng ban tiếp nhận, gửi kèm tệp và nhận mã hồ sơ; có SLA và chuông thông báo; nhân viên thấy hồ sơ trong hàng đợi, xem chi tiết và tệp; Admin thiết lập SLA.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| BE Dev / Senior | Trạng thái hồ sơ và quy tắc chuyển trạng thái, tính hạn xử lý (SLA), lưu tệp đính kèm, thông báo trong app, hàng đợi phòng ban, thiết lập SLA; sửa lỗi backend | 24 | 3,00 |
| BE Dev / Mid | Tạo yêu cầu, danh mục loại yêu cầu và phòng ban, sinh mã hồ sơ, mức ưu tiên, giới hạn tệp, danh sách yêu cầu của sinh viên, chi tiết hồ sơ và tải tệp cho nhân viên; phạm vi dữ liệu theo vai trò, nhật ký, lịch sử | 39 | 4,88 |
| FE Dev / Senior | Đính kèm tệp, danh sách yêu cầu của sinh viên, hàng đợi phòng ban, màn hình thiết lập SLA; sửa lỗi frontend | 14 | 1,75 |
| FE Dev / Junior | Form tạo yêu cầu, hiển thị phòng ban tiếp nhận, xác nhận và mã hồ sơ, chuông thông báo, chi tiết hồ sơ và tệp cho nhân viên; VN/EN và responsive | 22 | 2,75 |
| QA / Senior | Viết test case từ tiêu chí chấp nhận của milestone | 7 | 0,88 |
| Tester / Junior | Chạy test trọn luồng trên Staging | 22 | 2,75 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 117 h | 14,63 PD |
| **Buffer dự phòng (9,4%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 11 h | 1,38 PD |
| **TỔNG KẾ HOẠCH (MS-02)** | Tổng effort được phê duyệt | **128 h** | **16,00 PD** |

**Dự phòng 11 giờ dùng cho rủi ro:** R-07, R-08, R-09. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.3. MS-03 · PHÂN CÔNG, XỬ LÝ, CHUYỂN PHÒNG BAN (Q-04) — TỔNG: 83 GIỜ (10,38 PD)

**Mục tiêu cốt lõi:** Nhân viên phân công người phụ trách, ghi nhật ký, yêu cầu sinh viên bổ sung; sinh viên bổ sung thông tin; nhân viên đặt ưu tiên, escalate và chuyển hồ sơ sang phòng ban khác.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| BE Dev / Senior | Phân công người phụ trách, yêu cầu bổ sung thông tin, chuyển phòng ban; sửa lỗi backend | 15 | 1,88 |
| BE Dev / Mid | Ghi nhật ký xử lý, đặt ưu tiên, escalate, sinh viên bổ sung thông tin; chỉ người phụ trách được xử lý, phòng cũ mất quyền khi chuyển; lịch sử và thông báo đổi trạng thái | 19 | 2,38 |
| FE Dev / Senior | Màn hình phân công, yêu cầu bổ sung, chuyển phòng ban; sửa lỗi frontend | 11 | 1,38 |
| FE Dev / Junior | Màn hình nhật ký, đặt ưu tiên, escalate, form bổ sung của sinh viên; VN/EN và responsive | 15 | 1,88 |
| QA / Senior | Viết test case từ tiêu chí chấp nhận của milestone | 6 | 0,75 |
| Tester / Junior | Chạy test trọn luồng trên Staging | 17 | 2,13 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 76 h | 9,50 PD |
| **Buffer dự phòng (9,2%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 7 h | 0,88 PD |
| **TỔNG KẾ HOẠCH (MS-03)** | Tổng effort được phê duyệt | **83 h** | **10,38 PD** |

**Dự phòng 7 giờ dùng cho rủi ro:** R-10, R-11, R-12. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.4. MS-04 · TRẢ KẾT QUẢ CHO SINH VIÊN (Q-05) — TỔNG: 43 GIỜ (5,38 PD)

**Mục tiêu cốt lõi:** Người phụ trách đóng hồ sơ kèm kết quả; sinh viên nhận thông báo, xem trạng thái, tiến độ, kết quả và đánh giá một lần.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| BE Dev / Senior | Đóng hồ sơ kèm kết quả; sửa lỗi backend | 7 | 0,88 |
| BE Dev / Mid | Xem trạng thái và tiến độ, đánh giá chất lượng hỗ trợ; sinh viên chỉ thấy kết quả của mình, nhật ký, lịch sử và thông báo khi đóng hồ sơ | 11 | 1,38 |
| FE Dev / Senior | Màn hình đóng hồ sơ, trạng thái và tiến độ cho sinh viên; sửa lỗi frontend | 6 | 0,75 |
| FE Dev / Junior | Màn hình đánh giá; VN/EN và responsive | 6 | 0,75 |
| QA / Senior | Viết test case từ tiêu chí chấp nhận của milestone | 4 | 0,50 |
| Tester / Junior | Chạy test trọn luồng trên Staging | 9 | 1,13 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 39 h | 4,88 PD |
| **Buffer dự phòng (10,3%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 4 h | 0,50 PD |
| **TỔNG KẾ HOẠCH (MS-04)** | Tổng effort được phê duyệt | **43 h** | **5,38 PD** |

**Dự phòng 4 giờ dùng cho rủi ro:** R-13, R-14, R-15. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.5. MS-05 · THỐNG KÊ BÁO CÁO CHO ADMIN (Q-06) — TỔNG: 73 GIỜ (9,13 PD)

**Mục tiêu cốt lõi:** Admin xem dashboard (hồ sơ mới, đang xử lý, quá hạn, thời gian xử lý trung bình) và 3 báo cáo; có bộ 200 ticket mẫu; feature freeze.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| BE Dev / Senior | Dashboard số hồ sơ theo trạng thái và thời gian xử lý trung bình, báo cáo mức độ hài lòng; sửa lỗi backend | 11 | 1,38 |
| BE Dev / Mid | Báo cáo loại yêu cầu phổ biến và khối lượng theo phòng ban; Admin chỉ xem số liệu tổng hợp; hỗ trợ bộ 200 ticket mẫu | 16 | 2,00 |
| FE Dev / Senior | Giao diện dashboard và báo cáo mức độ hài lòng; sửa lỗi frontend | 8 | 1,00 |
| FE Dev / Junior | Giao diện báo cáo loại yêu cầu và khối lượng theo phòng ban; VN/EN và responsive | 9 | 1,13 |
| QA / Senior | Viết test case từ tiêu chí chấp nhận của milestone | 5 | 0,63 |
| Tester / Junior | Tạo bộ 200 ticket mẫu đủ mọi trạng thái; chạy test trọn luồng trên Staging | 24 | 3,00 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 67 h | 8,38 PD |
| **Buffer dự phòng (9,0%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 6 h | 0,75 PD |
| **TỔNG KẾ HOẠCH (MS-05)** | Tổng effort được phê duyệt | **73 h** | **9,13 PD** |

**Dự phòng 6 giờ dùng cho rủi ro:** R-16, R-17, R-18. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.6. MS-06 · TRIỂN KHAI, BÀN GIAO VÀ BẢO HÀNH (Q-07, Q-08) — TỔNG: 158 GIỜ (19,75 PD)

**Mục tiêu cốt lõi:** Hoàn thiện hồ sơ bàn giao, triển khai lên server của trường với đăng nhập thật, Go-live và bảo hành. Trong 158 giờ, bảo hành (`Q-08`) gồm BE Dev / Mid 30 giờ và FE Dev / Junior 23 giờ; phần còn lại thuộc `Q-07`.

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| PM / Senior | Chuẩn bị và điều phối buổi demo nghiệm thu với khách trên server trường | 10 | 1,25 |
| BA / Mid | Viết hướng dẫn sử dụng cho Sinh viên, Nhân viên, Admin; hỗ trợ buổi demo nghiệm thu | 18 | 2,25 |
| UI/UX / Mid | Usability test và Usability Test Report | 18 | 2,25 |
| BE Dev / Senior | Hỗ trợ triển khai production và xử lý sự cố trước Go-live | 3 | 0,38 |
| BE Dev / Mid | OpenAPI/Swagger, tài liệu mã lỗi; sửa lỗi backend trong bảo hành | 45 | 5,63 |
| FE Dev / Junior | Sửa lỗi frontend trong bảo hành | 23 | 2,88 |
| QA / Senior | Tổng hợp báo cáo kiểm thử chức năng; hỗ trợ usability test | 7 | 0,88 |
| Tester / Junior | Hỗ trợ tổng hợp báo cáo kiểm thử | 2 | 0,25 |
| DevOps / Mid | Docker Compose production và .env.example; triển khai lên server trường, nạp tài khoản Admin đầu tiên, nối đăng nhập thật; dự phòng sự cố trước Go-live | 32 | 4,00 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 145 h | 18,13 PD |
| **Buffer dự phòng (9,0%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 13 h | 1,63 PD |
| **TỔNG KẾ HOẠCH (MS-06)** | Tổng effort được phê duyệt | **158 h** | **19,75 PD** |

**Dự phòng 13 giờ dùng cho rủi ro:** R-19, R-20, R-21. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.7. QUẢN LÝ DỰ ÁN XUYÊN SUỐT (Q-09) — TỔNG: 26 GIỜ (3,25 PD)

**Mục tiêu cốt lõi:** Theo dõi effort kế hoạch và thực tế, báo cáo khách hằng tuần trong suốt dự án (việc không gắn milestone).

| Nhân sự tham gia | Vai trò tác nghiệp chính | Số giờ (h) | Quy đổi (PD) |
|---|---|---|---|
| PM / Senior | Quản lý dự án, theo dõi effort kế hoạch/thực tế, báo cáo khách hằng tuần | 26 | 3,25 |
| **Effort cơ sở (Baseline)** | Tổng giờ kỹ thuật thực thi trực tiếp | 24 h | 3,00 PD |
| **Buffer dự phòng (8,3%)** | Quỹ giờ giải quyết các rủi ro kỹ thuật phát sinh | 2 h | 0,25 PD |
| **TỔNG KẾ HOẠCH (QUẢN LÝ DỰ ÁN)** | Tổng effort được phê duyệt | **26 h** | **3,25 PD** |

**Dự phòng 2 giờ dùng cho rủi ro:** R-22, R-23. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

### 2.8. MA TRẬN ĐỐI SOÁT KIỂM TRA CHÉO NGUỒN LỰC (CROSS-CHECK MATRIX)

Khớp 100% giữa chiều dọc nhân sự (Phần 1) và chiều ngang milestone. Giờ/tuần và độ sẵn sàng của từng người theo lịch nằm ở sheet Resources của Master Plan; sheet đó tách 73 giờ dự phòng thành Quỹ dự phòng.

| Nhân sự | MS-01 (Q-01) | MS-01 (Q-02) | MS-02 | MS-03 | MS-04 | MS-05 | MS-06 | Quản lý dự án (Q-09) | TỔNG (h) |
|---|---|---|---|---|---|---|---|---|---|
| PM / Senior | 38 | 0 | 0 | 0 | 0 | 0 | 10 | 26 | 74 |
| BA / Mid | 72 | 0 | 0 | 0 | 0 | 0 | 18 | 0 | 90 |
| UI/UX / Mid | 50 | 0 | 0 | 0 | 0 | 0 | 18 | 0 | 68 |
| BE Dev / Senior | 20 | 14 | 24 | 15 | 7 | 11 | 3 | 0 | 94 |
| BE Dev / Mid | 7 | 13 | 39 | 19 | 11 | 16 | 45 | 0 | 150 |
| FE Dev / Senior | 20 | 11 | 14 | 11 | 6 | 8 | 0 | 0 | 70 |
| FE Dev / Junior | 28 | 7 | 22 | 15 | 6 | 9 | 23 | 0 | 110 |
| QA / Senior | 18 | 5 | 7 | 6 | 4 | 5 | 7 | 0 | 52 |
| Tester / Junior | 0 | 14 | 22 | 17 | 9 | 24 | 2 | 0 | 88 |
| DevOps / Mid | 32 | 10 | 0 | 0 | 0 | 0 | 32 | 0 | 74 |
| **TỔNG MILESTONE** | 285 | 74 | 128 | 83 | 43 | 73 | 158 | 26 | **870** |
| Cơ sở (Baseline) | 261 | 68 | 117 | 76 | 39 | 67 | 145 | 24 | 797 |
| Buffer dự phòng | 24 | 6 | 11 | 7 | 4 | 6 | 13 | 2 | 73 |

## PHẦN 3: TỔNG GIÁ THẦU DỰ TOÁN & PHƯƠNG ÁN BÁO GIÁ KHÁCH HÀNG

**Các tham số định mức tài chính chuẩn:**

- Dự phòng lao động triển khai: 20% tính trên Lương trực tiếp.
- Phúc lợi và bảo hiểm bắt buộc: 25% tính trên tổng Lương gồm dự phòng.
- Chi phí vận hành và quản lý dự án (Overhead): 15% tính trên tổng Lương gồm dự phòng.
- Biên lợi nhuận mục tiêu: 20% tính trên Doanh thu trước thuế VAT.

### BẢNG 3.1: CƠ SỞ TÍNH TOÁN CHI PHÍ & GIÁ THẦU NỘI BỘ

| STT | Khoản mục tài chính | Cơ sở và công thức xác định | Thành tiền (đồng) |
|---|---|---|---|
| 1 | Lương trực tiếp | 870 giờ công theo phân bổ nguồn lực thực tế | 142.669.230 |
| 2 | Dự phòng lao động triển khai | 20% x 142.669.230 đồng | 28.533.846 |
| — | Căn cứ tính chi phí gián tiếp (Lương + Dự phòng) | 142.669.230 + 28.533.846 | 171.203.076 |
| 3 | Phúc lợi và bảo hiểm | 25% x 171.203.076 đồng | 42.800.769 |
| 4 | Chi phí vận hành & Quản lý (Overhead) | 15% x 171.203.076 đồng | 25.680.461 |
| 5 | Công cụ, hạ tầng và bản quyền phần mềm | Tạm tính chi phí server test, domain, SSL, tool | 2.000.000 |
| **A** | **TỔNG CHI PHÍ CƠ SỞ THỰC HIỆN** | Tổng các mục (1 + 2 + 3 + 4 + 5) | **241.684.306** |
| 6 | Lợi nhuận dự toán ban đầu | 20% tính trên Giá trước VAT | 60.421.077 |
| **B** | **GIÁ THẦU TÍNH TOÁN TRƯỚC THUẾ VAT** | 241.684.306 đồng chia cho 80% | **302.105.383** |
| 7 | Thuế giá trị gia tăng (VAT 10%) | 10% x 302.105.383 đồng | 30.210.538 |
| **C** | **GIÁ THẦU TÍNH TOÁN SAU THUẾ VAT 10%** | Giá trước VAT + Thuế VAT 10% | **332.315.921** |

### BẢNG 3.2: ĐỐI CHIẾU GIÁ CHÀO VỚI CHI PHÍ (NỘI BỘ, KHÔNG GỬI KHÁCH)

Giá chào khách hàng nằm ở [quotation.xlsx](quotation.xlsx) sheet Hạng mục: **332.316.000 đồng** (đã gồm 10% VAT), làm tròn lên hàng nghìn từ tổng các hạng mục `Q-01` đến `Q-10`. Đơn giá bán từng vai trò = đơn giá chi phí ở Phần 1 × 2,1 (= 1,2 dự phòng lao động × 1,4 phúc lợi và vận hành ÷ 0,8 biên lợi nhuận). Bảng dưới chỉ dùng nội bộ để đối chiếu lợi nhuận; phần thặng dư sau khi làm tròn được kết chuyển vào lợi nhuận.

**Hai loại dự phòng khác nhau:** 73 giờ dự phòng ở Phần 2 là quỹ giờ cho rủi ro đã nhận diện (`R-xx`), đã nằm trong 870 giờ và trong kế hoạch. Dự phòng lao động 20% ở Bảng 3.1 là dự phòng tài chính cho sai lệch chi phí nhân sự thực tế so với ước lượng (đơn giá, giờ thực tế), không cấp thêm giờ trong kế hoạch. Change request do khách yêu cầu được báo giá riêng, không lấy từ hai khoản này.

| STT | Khoản mục thương mại | Cơ sở và công thức xác định | Giá trị thực tế (đồng) |
|---|---|---|---|
| **I** | **TỔNG GIÁ TRỊ HỢP ĐỒNG (SAU VAT 10%)** | Giá làm tròn chính thức gửi khách hàng | **332.316.000** |
| II | Thuế giá trị gia tăng (VAT 10%) | Tách thuế từ tổng giá: 332.316.000 / 1,1 x 10% | 30.210.545 |
| **III** | **GIÁ CHÀO THẦU TRƯỚC THUẾ VAT** | Doanh thu thuần: 332.316.000 / 1,1 | **302.105.455** |
| IV | Tổng chi phí thực tế thực hiện dự án | Chi phí nhân sự, dự phòng, bảo hiểm, vận hành | 241.684.306 |
| **V** | **LỢI NHUẬN THỰC TẾ THU VỀ** | Doanh thu trước VAT trừ Tổng chi phí | **60.421.149** |

### THUYẾT MINH TÀI CHÍNH & HẠCH TOÁN THẶNG DƯ LÀM TRÒN

**Phân tích số liệu làm tròn:**

- Giá thầu tính toán ban đầu sau thuế VAT là 332.315.921 đồng.
- Khi làm tròn lên mốc thương mại 332.316.000 đồng, giá trị sau thuế tăng thêm 79 đồng.
- Tương ứng, Doanh thu thuần trước thuế VAT tăng thêm 72 đồng (từ 302.105.383 đ lên thành 302.105.455 đ).

**Hạch toán kết chuyển thặng dư:**

- Phần thặng dư 72 đồng trước thuế này được kết chuyển toàn bộ vào Lợi nhuận của dự án.
- Lợi nhuận thực tế thu về được nâng từ 60.421.077 đ lên thành 60.421.149 đồng.
- Tỷ suất biên lợi nhuận thực tế đạt đúng: 60.421.149 ÷ 302.105.455 ≈ 20,00%.

Phương án này bảo đảm hồ sơ thương mại đạt tính thẩm mỹ cao, số tiền tròn đẹp, hóa đơn VAT xuất chuẩn xác từng đồng và duy trì đúng biên lợi nhuận mục tiêu của doanh nghiệp.
