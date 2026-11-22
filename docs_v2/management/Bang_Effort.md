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

## PHẦN 2: PHƯƠNG PHÁP ƯỚC LƯỢNG, BÓC TÁCH EFFORT VÀ QUY TẮC QUẢN LÝ GIỜ

Effort được bóc tách theo 6 milestone `MS-01` đến `MS-06` và việc quản lý dự án xuyên suốt, đúng với các issue trên GitHub và hạng mục `Q-xx` trong báo giá. Mỗi hạng mục có một quỹ giờ dự phòng (Buffer) cho các rủi ro đã ghi trong sổ rủi ro; bảng dưới chỉ dẫn mã `R-xx`, mô tả, owner và phương án nằm ở [risk-register.md](risk-register.md).

**Cách cộng theo milestone (Baseline v1.0/v1.1):** tổng giờ của mục 2.1 đến 2.6 không gồm việc quản lý dự án `Q-09` (26 giờ, mục 2.7). Sheet WBS của Master Plan chia 26 giờ này vào từng milestone (5, 6, 5, 3, 3, 4 giờ cho `MS-01` đến `MS-06`), nên tổng theo milestone ở WBS là 364, 134, 88, 46, 76, 162 giờ. Tổng chung baseline là **870 giờ** (797 giờ cơ sở + 73 giờ dự phòng rủi ro); tổng theo hạng mục `Q-xx` khớp tuyệt đối với [quotation.xlsx](quotation.xlsx).

**Cơ sở công nghệ dùng để ước lượng (`A-14`, `ADR-005`):** Toàn bộ định mức giờ phát triển, tích hợp và triển khai dưới đây được ước lượng dựa trên giả định **`A-14`** (đội thực thi sử dụng ngăn xếp web, CSDL quan hệ và bộ nhớ đệm phía server phổ biến phù hợp năng lực đội như đề xuất tại [ADR-005](../architecture/technical-decisions/ADR-005-technology-stack-and-cache.md), trong đó Redis là lựa chọn triển khai đề xuất cho yêu cầu cache 15 phút, đóng gói Docker Compose trên server trường). Ngăn xếp chốt chính thức tại WBS `1.04–1.05` (`08/09–11/09/2026`) và xác nhận hạ tầng ở WBS `1.11–1.14` (`14/09–17/09/2026`); thay đổi tương đương xử lý trong dự phòng `Q-01` (`R-27`), còn thay đổi bắt buộc vượt `A-14` phải lập CR đánh giá lại effort và giá trước khi lập trình.

### 2.0a. Phương pháp ước lượng từ khối lượng công việc (Bottom-up)

Để tránh nhược điểm chia ngược từ tổng giờ cố định theo số ngày trên lịch, dự án áp dụng công thức ước lượng từ khối lượng công việc thực tế làm chuẩn đối chiếu và làm cơ sở ước lượng cho mọi Change Request (`CR-xx`):

| Nhóm công việc | Đơn vị đo khối lượng | Định mức ước lượng chuẩn | Vai trò thực hiện |
|---|---|---|---|
| **Phát triển chức năng (FR)** — Mức Đơn giản (`S`) | FR có 1 luồng/màn hình đơn, ít trạng thái, ≤ 7 AC | `BE: 3h`, `FE: 2h` (0h nếu FR hệ thống không có UI) | BE Dev / Mid, FE Dev / Junior |
| **Phát triển chức năng (FR)** — Mức Trung bình (`M`) | FR có nhiều quy tắc nghiệp vụ, lọc/phân trang, cache, snapshot hoặc phạm vi dữ liệu, 7–8 AC | `BE: 5h`, `FE: 3h` (0h nếu không có UI) | BE Dev / Mid–Senior, FE Dev / Junior–Senior |
| **Phát triển chức năng (FR)** — Mức Phức tạp (`C`) | FR có đồng thời/idempotency, state machine, SLA/job nền, upload tệp/magic bytes, polling hoặc báo cáo tổng hợp, ≥ 8 AC | `BE: 8h`, `FE: 5h` (0h nếu không có UI) | BE Dev / Senior–Mid, FE Dev / Senior–Junior |
| **Phần dùng chung theo milestone (Shared)** | Số AC dùng chung triển khai lần đầu tại milestone (`FR-SEC-04..06`, `FR-CORE-06, 08, 09`) | `0,6–0,85 giờ / AC` theo mảng BE hoặc FE | BE Dev / Mid, FE Dev / Senior–Junior |
| **Viết & duyệt test case từ AC** | Tổng số AC của milestone, phủ đủ 5 nhóm kiểm thử (`Happy Path`, `Validation`, `Security`, `Concurrency`, `Resilience`) | `0,12 giờ / AC` (~7 phút/AC gồm thiết kế dữ liệu, bước kiểm và review; ~30% QA Senior, ~70% Tester Junior) | QA / Senior, Tester / Junior |
| **Tích hợp, chạy test trọn luồng & regression** | Số AC chạy lần đầu + số kịch bản trọn luồng/chéo + số milestone trước cần regression | `0,10 giờ / AC` + `4h` kịch bản trọn luồng/chéo + `4h × số milestone trước` (regression) + `2h` seed/môi trường | QA / Senior, Tester / Junior |
| **Sửa lỗi sau test & retest** | Tỷ lệ trên giờ phát triển của milestone (việc biết trước, không lấy từ quỹ dự phòng rủi ro) | `20%` giờ phát triển (BE + FE) của milestone | BE Dev, FE Dev |
| **Quỹ dự phòng rủi ro (Buffer)** | Tỷ lệ trên tổng effort cơ sở của hạng mục, gắn trực tiếp mã `R-xx` | `8% – 10%` effort cơ sở; chỉ giải ngân khi rủi ro trong `risk-register.md` xảy ra | Quỹ dự phòng (PM / Senior duyệt) |

### 2.0b. Ví dụ ước lượng chi tiết cho `Q-03 / MS-02` và tác động của 87 AC bổ sung

`Q-03 / MS-02` (Gửi và tiếp nhận yêu cầu) có **16 FR chuyên biệt (131 AC, 104 BR, 46 Error flow)** và **FR dùng chung độc quyền của MS-02 là `FR-SEC-05` (7 AC, 7 BR, 5 Error flow)** → tổng **138 AC cốt lõi**; ngoài ra còn **15 AC** thuộc lát cắt MS-02 của 5 FR dùng chung (`FR-SEC-04`: 2 AC, `FR-SEC-06`: 2 AC, `FR-CORE-06`: 3 AC, `FR-CORE-08`: 4 AC, `FR-CORE-09`: 4 AC) → tổng **153 AC** cần nghiệm thu lần đầu tại `MS-02`.

| Mã FR | Tên chức năng | Số AC / BR / E | Mức phức tạp | BE (h) | FE (h) | Tổng Dev (h) |
|---|---|---:|---|---:|---:|---:|
| `FR-STU-02` | Tạo yêu cầu hỗ trợ (idempotency, rate limit, snapshot) | 10 / 8 / 6 | Phức tạp (`C`) | 8 | 5 | 13 |
| `FR-STU-03` | Hiển thị phòng ban tiếp nhận | 7 / 5 / 2 | Đơn giản (`S`) | 3 | 2 | 5 |
| `FR-STU-04` | Đính kèm tệp minh chứng (5 tệp, 10 MiB, rollback) | 9 / 5 / 4 | Phức tạp (`C`) | 8 | 5 | 13 |
| `FR-STU-05` | Nhận mã hồ sơ và xác nhận gửi thành công | 7 / 5 / 2 | Đơn giản (`S`) | 3 | 2 | 5 |
| `FR-STU-06` | Xem danh sách yêu cầu của mình (lọc, phân trang) | 8 / 6 / 2 | Trung bình (`M`) | 5 | 3 | 8 |
| `FR-STU-08` | Nhận thông báo trong app (polling 30s, `read_before`) | 10 / 7 / 3 | Phức tạp (`C`) | 8 | 5 | 13 |
| `FR-STF-01` | Hàng đợi hồ sơ của phòng ban (lọc, sắp xếp ưu tiên/SLA) | 9 / 8 / 3 | Phức tạp (`C`) | 8 | 5 | 13 |
| `FR-STF-02` | Xem thông tin sinh viên và nội dung yêu cầu | 7 / 4 / 3 | Trung bình (`M`) | 5 | 3 | 8 |
| `FR-STF-03` | Xem và tải tệp đính kèm | 8 / 4 / 3 | Trung bình (`M`) | 5 | 3 | 8 |
| `FR-CORE-01` | Sinh mã hồ sơ đồng thời (`USP-YYYY-NNNNN`) | 8 / 5 / 2 | Trung bình (`M`, no UI) | 5 | 0 | 5 |
| `FR-CORE-02` | Trạng thái hồ sơ và quy tắc chuyển trạng thái | 8 / 10 / 3 | Phức tạp (`C`, no UI) | 8 | 0 | 8 |
| `FR-CORE-03` | Tính hạn xử lý (SLA) và tác vụ quá hạn ≤ 60 giây | 9 / 10 / 2 | Phức tạp (`C`, no UI) | 8 | 0 | 8 |
| `FR-CORE-04` | Mức độ ưu tiên mặc định và thứ tự | 7 / 5 / 2 | Đơn giản (`S`, no UI) | 3 | 0 | 3 |
| `FR-CORE-05` | Danh mục loại yêu cầu và phòng ban (nạp khi khởi động) | 8 / 8 / 2 | Trung bình (`M`, no UI) | 5 | 0 | 5 |
| `FR-CORE-07` | Giới hạn tệp đính kèm và kiểm tra magic bytes | 8 / 7 / 4 | Trung bình (`M`, no UI) | 5 | 0 | 5 |
| `FR-MGT-07` | Thiết lập SLA cho từng loại yêu cầu (1–720h, không hồi tố) | 8 / 7 / 3 | Trung bình (`M`) | 5 | 3 | 8 |
| `FR-SEC-05` | Bảo vệ tệp đính kèm (`#123`, riêng `MS-02`) | 7 / 7 / 5 | Dùng chung BE | 6 | 0 | 6 |
| Shared BE | `FR-SEC-04` (2 AC), `FR-SEC-06` (2 AC), `FR-CORE-06` (3 AC) (`#123`) | 7 AC | Dùng chung BE | 5 | 0 | 5 |
| Shared FE | `FR-CORE-08` (4 AC), `FR-CORE-09` (4 AC) (`#124`) | 8 AC | Dùng chung FE | 0 | 6 | 6 |
| **TỔNG PHÁT TRIỂN (`Q-03`)** | **16 FR + 6 phần dùng chung (138 AC cốt lõi; 153 AC tổng)** | **153 AC** | — | **103** | **42** | **145** |

**Tổng hợp đối chiếu `Q-03 / MS-02` (Baseline v1.0/v1.1 vs Bottom-up):**

| Nhóm việc trong `Q-03` | Baseline v1.0/v1.1 (`quotation.xlsx`) | Ước lượng Bottom-up | Chênh lệch | Nhận xét đối chiếu |
|---|---:|---:|---:|---|
| Phát triển (BE + FE, gồm phần dùng chung) | 62 h (BE 41h, FE 21h) | **145 h** (BE 103h, FE 42h) | **+83 h** | 62h ở baseline tương đương ~3,2–3,9h/FR cả BE lẫn FE, không đủ cho 16 FR + 6 phần dùng chung có SLA, upload, state machine và polling |
| Viết & duyệt test case từ AC | 4 h (QA Sr 3h, Tester Jr 1h) | **18 h** (`153 × 0,12 ≈ 18,4h`; riêng 138 AC cốt lõi = `16,6h`) | **+14 h** | 4h ở baseline tương đương ~1,6 phút/AC cho 138–153 AC, không khả thi |
| Tích hợp, chạy test trọn luồng & regression | 22 h (QA Sr 3h, Tester Jr 19h) | **25 h** (`153 × 0,10 + 4 + 4 + 2 ≈ 25,3h`) | **+3 h** | Phủ toàn bộ 153 AC + `TS-02, TS-03, TS-11, TS-12, TS-16, TS-17` + regression `MS-01` |
| Sửa lỗi sau test & retest | 29 h (BE 17h, FE 12h) | **29 h** (`20% × 145h`) | **0 h** | Giờ sửa lỗi baseline sát định mức 20% phát triển |
| Quỹ dự phòng rủi ro (`R-07, R-08, R-09`) | 11 h (9,4% cơ sở) | **19 h** (~8,8% cơ sở mới) | **+8 h** | Giữ tỷ lệ dự phòng ~9% theo mức rủi ro |
| **TỔNG `Q-03 / MS-02`** | **128 h** | **236 h** | **+108 h** | Chênh lệch cần ban quản lý phê duyệt phương án xử lý ở mục 2.0c |

**Rà soát tác động của 87 AC được bổ sung (từ 252 AC → 339 AC, tăng +34,5%):**
- Trong đợt bổ sung chi tiết đặc tả (`252 → 339 AC`), toàn dự án tăng **+87 AC** (chủ yếu bổ sung các ca `Validation`, `Security`, `Concurrency`, `Resilience` như khóa lạc quan, idempotency, rate limit, lỗi 502/503/504 và biên ngày/múi giờ); riêng `Q-03 / MS-02` tăng **+36 AC** (từ 102 → 138 AC cốt lõi, +35,3%).
- Tác động trực tiếp lên khối lượng kiểm thử: toàn dự án tăng `+87 × 0,12 = +10,4h` viết test case và `+87 × 0,10 = +8,7h` chạy test/retest → tối thiểu **+19,1 giờ QA/Test** (làm tròn **+20 giờ**); riêng `Q-03 / MS-02` tăng **+7,9 giờ QA/Test** (`+4,3h` viết + `+3,6h` chạy) và khoảng **+16–20 giờ phát triển** cho các ca xử lý lỗi và đồng thời bổ sung.

### 2.0c. Đối chiếu toàn dự án với Baseline (870 giờ / 332.316.000 VNĐ) và phương án trình duyệt

Áp dụng cùng bộ định mức ở mục 2.0a cho toàn bộ 9 hạng mục giờ công (`Q-01` đến `Q-09`):

| Hạng mục | Milestone | Khối lượng chính | Baseline v1.0/v1.1 (h) | Ước lượng Bottom-up (h) | Chênh lệch (h) |
|---|---|---|---:|---:|---:|
| `Q-01` | `MS-01` | Phân tích 42 FR, kiến trúc, wireframe, prototype, repo/CI | 285 | 285 | 0 |
| `Q-02` | `MS-01` | 5 FR (40 AC) + 11 AC dùng chung = 51 AC; Staging & seed | 74 | 92 | +18 |
| `Q-03` | `MS-02` | 16 FR (131 AC) + `FR-SEC-05` (7 AC) + 15 AC dùng chung = 153 AC | 128 | 236 | +108 |
| `Q-04` | `MS-03` | 7 FR (57 AC) + 8 AC dùng chung = 65 AC | 83 | 118 | +35 |
| `Q-05` | `MS-04` | 3 FR (24 AC) + 3 AC dùng chung = 27 AC | 43 | 53 | +10 |
| `Q-06` | `MS-05` | 5 FR (39 AC) + 4 AC dùng chung = 43 AC; bộ 200 ticket mẫu | 73 | 95 | +22 |
| `Q-07` | `MS-06` | Triển khai production, SSO thật, tài liệu, usability test | 105 | 105 | 0 |
| `Q-08` | `MS-06` | Bảo hành 1 tháng sau nghiệm thu | 53 | 53 | 0 |
| `Q-09` | Xuyên suốt | Quản lý dự án, theo dõi giờ công, báo cáo tuần | 26 | 26 | 0 |
| **TỔNG CỘNG** | — | **42 FR / 339 AC** | **870 h** | **1.063 h** | **+193 h (+22,2%)** |

**Trạng thái Baseline và 2 phương án chờ ban quản lý phê duyệt qua quy trình thay đổi (`03` mục 5):**
- Hồ sơ hiện hành **giữ nguyên baseline 870 giờ và 332.316.000 VNĐ** (đã phát hành trong Proposal v1.0/v1.1, `quotation.xlsx` v1.0/v1.1 và Master Plan v1.0/v1.1) để bảo toàn tính nhất quán tài chính cho đến khi ban quản lý biểu quyết chọn phương án:
  1. **Phương án A — Giữ nguyên giá chào 332.316.000 VNĐ (870 giờ baseline) và quản trị chênh lệch nội bộ:** ghi nhận chênh lệch `+193h` là rủi ro vượt giờ nội bộ (`R-22`), dùng quỹ dự phòng lao động tài chính 20% (`28.533.846 VNĐ`) và biên lợi nhuận để hấp thụ phần giờ vượt nếu xảy ra (nhà thầu chịu 100% chi phí, không thu thêm Aurora University); đồng thời ưu tiên hoàn thành toàn bộ FR `Must` trước, thương lượng chuyển một số FR `Should` sang giai đoạn sau nếu cần giữ mốc 15 tuần.
  2. **Phương án B — Điều chỉnh báo giá lên phiên bản `v2.0` trước khi ký hợp đồng:** cập nhật baseline lên **1.063 giờ** (+193 giờ, ước tính giá sau VAT tăng thêm khoảng **66–72 triệu VNĐ** lên mức **398–404 triệu VNĐ** tùy tỷ trọng Senior/Mid/Junior), đồng bộ lại `quotation.xlsx`, Master Plan (WBS, Resources, Change log) và phát hành Proposal `v2.0`.

### 2.0d. Phân biệt 4 loại giờ và quy tắc xử lý khi vượt giờ

Quy tắc dưới đây áp dụng thống nhất trên `Bang_Effort.md`, `proposal.docx`, `quotation.xlsx`, Master Plan (`WBS`, `Resources`), `master-plan.md`, `team-charter.md` và chỉ số Giờ công ở `03-quan-tri-giam-sat.md`:

| Loại giờ | Số giờ (Baseline v1.1) | Có nằm trong giá 332.316.000 VNĐ? | Có tính vào dự báo (EAC) so với 870 giờ? | Mục đích và quy tắc sử dụng |
|---|---:|---|---|---|
| **1. Giờ thực thi cơ sở trong báo giá** | **797 h** | **Có** | **Có** | Toàn bộ việc biết trước trong WBS (phân tích, thiết kế, phát triển, viết test case, tích hợp/test/regression, sửa lỗi sau test, triển khai, tài liệu, bảo hành, quản lý) |
| **2. Quỹ giờ dự phòng rủi ro (Buffer)** | **73 h** | **Có** | **Có** (khi giải ngân) | 8 dòng `Dự phòng Q-xx` trong WBS; chỉ giải ngân khi rủi ro đã nhận diện (`R-01` đến `R-27`) xảy ra và được PM / Senior duyệt; báo cáo hằng tuần |
| **3. Năng lực nhân sự còn trống (`Resources` cột `Dư`)** | Xem sheet `Resources` | **Không** | **Có**, nếu dùng cho việc thuộc phạm vi baseline; **Không**, nếu dùng cho CR đã duyệt riêng | Cho biết số giờ/tuần còn rảnh của đội thực thi để (a) tiếp nhận CR có báo giá riêng, hoặc (b) tăng cường lực lượng giữ mốc lịch khi việc baseline vượt giờ |
| **4. Giờ của Change Request (`CR-xx`)** | Phát sinh riêng | **Không** (báo giá bổ sung riêng) | Theo dõi bằng ngân sách CR riêng (chỉ cộng vào baseline sau khi khách ký duyệt CR) | Áp dụng cho yêu cầu thay đổi phạm vi hoặc giả định phía khách sai (`A-01` đến `A-14`) theo `03` mục 5 |

- **Ai chịu chi phí khi vượt giờ:** Nếu giờ thực tế vượt quá baseline **870 giờ** (hoặc vượt giờ kế hoạch từng hạng mục) trong **phạm vi 42 FR đã chốt** do sai lệch ước lượng nội bộ, làm lại hoặc phát sinh lỗi, **bên phát triển chịu 100% chi phí phát sinh** (hấp thụ bằng khoản dự phòng lao động tài chính 20% = 28.533.846 VNĐ và biên lợi nhuận nội bộ), **tuyệt đối không tính thêm tiền cho Aurora University**. Aurora University chỉ thanh toán thêm khi ký duyệt Change Request mở rộng phạm vi hoặc khi giả định phía khách (`A-01` đến `A-14`) sai thực tế.
- **Giờ nào tính vào dự báo tổng giờ (`EAC`):** `Dự báo tổng giờ baseline = Giờ thực tế lũy kế của phạm vi baseline (gồm cả giờ lấy thêm từ năng lực dư ở sheet Resources để làm hoặc test phạm vi baseline) + Giờ kế hoạch còn lại của phạm vi baseline`. Khi `EAC > 870 giờ`, chỉ số Giờ công ở `03` mục 3.1 chuyển mức **Đỏ** để ban quản lý phát hiện sớm độ lệch thực tế, không che giấu bằng năng lực dư của `Resources`.
- **Khi nào phải báo khách bằng văn bản:** PM / Senior bắt buộc thông báo bằng văn bản cho Aurora University khi xảy ra ít nhất một trong ba trường hợp: (1) Vượt giờ có nguy cơ làm trễ mốc milestone hoặc mốc Go-live 22/12/2026 dù đã điều phối quỹ dự phòng và năng lực dư; (2) Cần thương lượng ưu tiên lại hoặc dời FR mức `Should` để bảo vệ mốc Go-live; (3) Nguyên nhân vượt giờ xuất phát từ thay đổi phạm vi hoặc giả định phía khách sai (`A-xx`), cần kích hoạt quy trình CR (`03` mục 5).

> **Lưu ý về phân bổ baseline v1.0/v1.1 trong bảng 2.1–2.8 dưới đây:** các bảng bên dưới phản ánh chính xác phân bổ baseline **870 giờ** đang khớp 100% với `quotation.xlsx` v1.1 và Master Plan v1.1. Khi ban quản lý duyệt thay đổi theo Phương án B (nếu chọn), các bảng này sẽ được cập nhật đồng bộ theo quy trình thay đổi.

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

**Dự phòng 24 giờ dùng cho rủi ro:** R-01, R-02, R-03, R-27. Mô tả, owner và phương án: [risk-register.md](risk-register.md).

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

- Nơi gốc của giá là [quotation.xlsx](quotation.xlsx): tổng các hạng mục trước VAT 302.105.412 đồng, sau VAT 332.315.953 đồng (sheet Hạng mục, ô F15 và F17), làm tròn lên 332.316.000 đồng. Bảng 3.1 dưới đây tính theo chi phí nội bộ nên ra 302.105.383 đồng trước VAT; chênh 29 đồng do đơn giá bán từng vai trò trong báo giá được làm tròn đến đồng. Cả hai cách tính đều làm tròn ra cùng giá chào.
- Giá thầu tính toán ban đầu (theo Bảng 3.1) sau thuế VAT là 332.315.921 đồng.
- Khi làm tròn lên mốc thương mại 332.316.000 đồng, giá trị sau thuế tăng thêm 79 đồng.
- Tương ứng, Doanh thu thuần trước thuế VAT tăng thêm 72 đồng (từ 302.105.383 đ lên thành 302.105.455 đ).

**Hạch toán kết chuyển thặng dư:**

- Phần thặng dư 72 đồng trước thuế này được kết chuyển toàn bộ vào Lợi nhuận của dự án.
- Lợi nhuận thực tế thu về được nâng từ 60.421.077 đ lên thành 60.421.149 đồng.
- Tỷ suất biên lợi nhuận thực tế đạt đúng: 60.421.149 ÷ 302.105.455 ≈ 20,00%.

Phương án này bảo đảm hồ sơ thương mại đạt tính thẩm mỹ cao, số tiền tròn đẹp, hóa đơn VAT xuất chuẩn xác từng đồng và duy trì đúng biên lợi nhuận mục tiêu của doanh nghiệp.
