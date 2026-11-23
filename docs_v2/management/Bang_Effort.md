# TÀI LIỆU DỰ TOÁN EFFORT, BÓC TÁCH NGUỒN LỰC & GIÁ THẦU THƯƠNG MẠI

**DỰ ÁN: HỆ THỐNG TIẾP NHẬN VÀ XỬ LÝ YÊU CẦU SINH VIÊN TRỰC TUYẾN (UniSupport)**

> **Tài liệu nội bộ, không gửi khách.** Khách hàng Aurora University và đội thực thi là **Mô phỏng**. Đội thực thi gồm 10 người, mỗi vị trí ở Phần 1 là 1 người; ban quản lý dự án là 4 thành viên nhóm (xem [team-charter.md](team-charter.md)).
>
> Nơi gốc: giá và hạng mục gửi khách ở [quotation.xlsx](quotation.xlsx) (mã `Q-xx`); lịch, việc và nguồn lực theo tuần ở [UniSupport_Masterplan_nhom2.xlsx](UniSupport_Masterplan_nhom2.xlsx) (sheet WBS, Resources); rủi ro ở [risk-register.md](risk-register.md) (mã `R-xx`). Tài liệu này giữ cơ sở chi phí và thuyết minh phân bổ giờ.

## PHẦN 1: BẢNG DỰ TOÁN TIỀN CÔNG LAO ĐỘNG THEO NGƯỜI THAM GIA (ĐỘI THỰC THI MÔ PHỎNG)

| STT | Vai trò | Lương gross/tháng (đồng) | Giờ dự toán gồm giờ dự phòng phân bổ | Chi phí lương (đồng) |
|---|---|---:|---:|---:|
| 1 | PM / Senior | 50.100.000 | 93 | 22.400.481 |
| 2 | BA / Mid | 28.000.000 | 89 | 11.980.769 |
| 3 | UI/UX / Mid | 26.500.000 | 69 | 8.790.865 |
| 4 | BE Dev / Senior | 51.800.000 | 141 | 35.114.423 |
| 5 | BE Dev / Mid | 32.000.000 | 224 | 34.461.538 |
| 6 | FE Dev / Senior | 51.800.000 | 84 | 20.919.231 |
| 7 | FE Dev / Junior | 16.500.000 | 143 | 11.343.750 |
| 8 | QA / Senior | 46.200.000 | 67 | 14.881.731 |
| 9 | Tester / Junior | 13.500.000 | 121 | 7.853.365 |
| 10 | DevOps / Mid | 39.800.000 | 78 | 14.925.000 |
| | **Tổng** | | **1109** | **182.671.153** |

Chi phí lương tính từ lương gross chia cho 208 giờ/tháng. Giá bán tính bằng đơn giá theo vai trò tại sheet Đơn giá của báo giá. Giờ dự phòng được phân bổ cho các vai trò dự kiến xử lý rủi ro khi tính giá, còn trong WBS do PM quản lý tập trung.

## PHẦN 2. CƠ SỞ ƯỚC LƯỢNG VÀ PHÂN BỔ GIỜ

### 2.0a. Cách ước lượng theo khối lượng

- **Phát triển.** Chia FR theo độ phức tạp và phạm vi BE/FE. Định mức tham khảo BE là 3/5/8 giờ và FE là 2/3/5 giờ cho chức năng đơn giản/trung bình/phức tạp. Logic hệ thống không có giao diện chỉ tính BE. Quyền, audit, lịch sử, VN/EN và responsive được tính thêm theo số nhánh AC triển khai ở từng mốc, không tính lại nguyên FR khi regression. Giờ mỗi vai trò và từng mốc có thể kiểm lại trong bảng WBS bên dưới.
- **Kiểm thử và sửa lỗi.** Viết hoặc duyệt test từ AC lấy mốc 0,12 giờ/AC. Chạy lần đầu lấy mốc 0,10 giờ/AC, cộng giờ trọn luồng, hồi quy, chuẩn bị môi trường và kiểm lại sau sửa. Tách các lượt kiểm theo mốc có chức năng tương ứng. Sửa lỗi thường lấy khoảng 20% giờ phát triển và hiệu chỉnh theo độ phức tạp, khối lượng tích hợp và kiểm lại.
- **Dự phòng.** Lấy xấp xỉ 9% giờ cơ sở của các hạng mục có rủi ro xác định, làm tròn đến giờ. Bảo hành đã ước lượng riêng theo số ca dự kiến. PM ghi mã rủi ro khi giải ngân.
- **Công nghệ.** Ước lượng dựa trên A-14 và [ADR-005](../architecture/technical-decisions/ADR-005-technology-stack-and-cache.md). Chốt đề xuất kỹ thuật tại WBS 1.04–1.05 và xác nhận tương thích trước lập trình. Thay đổi bắt buộc ngoài giả định được đánh giá theo CR.

### 2.0b. Cơ sở từng hạng mục

| Mã | Khối lượng và cách ra giờ cơ sở | Giờ cơ sở | Dự phòng | Tổng |
|---|---|---:|---:|---:|
| Q-01 | Khởi động 11h; đặc tả 42 FR và 339 AC qua 6h PM + 38h BA; kiến trúc và ERD 23h; 3 cổng gồm wireframe 32h và prototype 45h; kế hoạch test 16h; 6 lượt phối hợp/chốt với trường và rà đặc tả tổng 64h; repo, CI và SSO giả lập 31h. | 266 | 24 | 290 |
| Q-02 | 40 AC của 5 FR và 11 AC dùng chung. Staging/seed 13h, phát triển 37h, viết test 6h, kiểm trọn luồng 11h và sửa/kiểm lại 16h. | 83 | 7 | 90 |
| Q-03 | 16 FR chuyên biệt, tổng 153 AC gồm 22 AC dùng chung. Phát triển 145h (97h BE và 48h FE); viết test 18h (153 × 0,12 làm tròn); tích hợp, test và kiểm lại 24h; sửa lỗi 29h. | 216 | 19 | 235 |
| Q-04 | 7 FR chuyên biệt và 8 AC dùng chung mới, tổng 65 AC. Phát triển 76h, thiết kế/duyệt test 8h, chạy trọn luồng/hồi quy/kiểm lại 20h, sửa lỗi 15h. | 119 | 11 | 130 |
| Q-05 | 3 FR chuyên biệt và 3 AC dùng chung mới, tổng 27 AC. Phát triển 32h, viết test 3h, test/hồi quy/kiểm lại 15h, sửa lỗi 6h. | 56 | 5 | 61 |
| Q-06 | 5 FR chuyên biệt và 4 AC dùng chung mới, tổng 43 AC. Phát triển 47h, tạo/đối chiếu 200 ticket mẫu 16h, viết test 5h, test/hồi quy/kiểm lại 20h và sửa lỗi 9h. | 97 | 9 | 106 |
| Q-07 | Đóng gói 10h, tích hợp production, SSO và kiểm tải 18h, hỗ trợ Go-live 7h, OpenAPI/mã lỗi 14h, ba hướng dẫn sử dụng 12h, báo cáo kết quả test 8h, kiểm khả dụng 21h và demo/bàn giao 9h. | 99 | 9 | 108 |
| Q-08 | Một tháng bảo hành mô phỏng với 6 ca BE × 4h và 6 ca FE × 3h, gồm chẩn đoán, sửa và xác nhận lại. | 42 | 0 | 42 |
| Q-09 | 15 báo cáo tuần × 2h = 30h, 6 lần theo dõi mốc × 1,5h = 9h và 4h chuẩn bị/tổng hợp bảo hành. Các buổi demo nghiệm thu đã tính trong hạng mục triển khai. | 43 | 4 | 47 |
| | **Tổng baseline v2.0** | **1021** | **88** | **1109** |

### 2.0c. Baseline phát hành trước khởi động

Baseline v2.0 phản ánh tổng ước lượng theo công việc. Ngày phát hành trong kịch bản mô phỏng là 06/09/2026, trước ngày khởi động 07/09/2026. Sáu thời hạn milestone và mốc Go-live 22/12/2026 được giữ. Hệ thống số liệu lịch, nguồn lực và giá nằm trong hai workbook đính kèm Proposal.

### 2.0d. Giờ vượt dự toán

Giờ cơ sở, giờ dự phòng đã dùng và giờ bổ sung cho việc trong phạm vi baseline đều tính vào giờ thực tế và dự báo. Khi dự báo vượt baseline, chuyển chỉ số Giờ công sang Đỏ. Bên phát triển chịu chi phí vượt nội bộ. PM thông báo khách bằng văn bản nếu ngày bàn giao bị ảnh hưởng. Giờ và tiền cho phạm vi/giả định thay đổi theo CR được ước lượng và theo dõi riêng sau khi khách duyệt. Khoản dự phòng tài chính trong đơn giá không cấp thêm giờ; mọi giờ bổ sung cần được bố trí trong lịch và Resources.

### 2.1. MS-01 · 388 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 1.01 | Khởi động và lập kế hoạch | PM / Senior | Q-01 | 11 |
| 1.02 | Đặc tả chức năng M01–M05 | PM / Senior | Q-01 | 6 |
| 1.03 | Đặc tả chức năng M01–M05 | BA / Mid | Q-01 | 38 |
| 1.04 | Kiến trúc, ERD, luồng dữ liệu và đề xuất chốt công nghệ (ADR-005) | BE Dev / Senior | Q-01 | 16 |
| 1.05 | Kiến trúc, ERD, luồng dữ liệu và đề xuất chốt công nghệ (ADR-005) | BE Dev / Mid | Q-01 | 7 |
| 1.06 | Wireframe và giao diện 3 cổng | UI/UX / Mid | Q-01 | 32 |
| 1.07 | Prototype 3 cổng và chỉnh theo phản hồi | UI/UX / Mid | Q-01 | 7 |
| 1.08 | Prototype 3 cổng và chỉnh theo phản hồi | FE Dev / Senior | Q-01 | 16 |
| 1.09 | Prototype 3 cổng và chỉnh theo phản hồi | FE Dev / Junior | Q-01 | 22 |
| 1.10 | Kế hoạch test và tiêu chí chất lượng | QA / Senior | Q-01 | 16 |
| 1.11 | Chuẩn bị xác nhận yêu cầu và hạ tầng với trường | PM / Senior | Q-01 | 17 |
| 1.12 | Xác nhận yêu cầu với trường, chốt phạm vi | BA / Mid | Q-01 | 29 |
| 1.13 | Xác nhận yêu cầu với trường, chốt phạm vi | UI/UX / Mid | Q-01 | 7 |
| 1.14 | Xác nhận yêu cầu, hạ tầng/công nghệ (ADR-005, A-14) và chốt phạm vi | BE Dev / Senior | Q-01 | 6 |
| 1.15 | Xác nhận yêu cầu với trường, chốt phạm vi | QA / Senior | Q-01 | 3 |
| 1.16 | Repo, CI, môi trường dev, SSO giả lập | DevOps / Mid | Q-01 | 31 |
| 1.17 | Staging, CI deploy và dữ liệu seed | BE Dev / Mid | Q-02 | 1 |
| 1.18 | Staging, CI deploy và dữ liệu seed | Tester / Junior | Q-02 | 3 |
| 1.19 | Staging, CI deploy và dữ liệu seed | DevOps / Mid | Q-02 | 9 |
| 1.20 | Phát triển backend | BE Dev / Senior | Q-02 | 10 |
| 1.21 | Phát triển backend | BE Dev / Mid | Q-02 | 8 |
| 1.22 | Phát triển frontend | FE Dev / Senior | Q-02 | 8 |
| 1.23 | Phát triển frontend | FE Dev / Junior | Q-02 | 4 |
| 1.24 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | BE Dev / Mid | Q-02 | 4 |
| 1.25 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Junior | Q-02 | 3 |
| 1.26 | Viết test case từ AC | QA / Senior | Q-02 | 2 |
| 1.27 | Viết test case từ AC | Tester / Junior | Q-02 | 4 |
| 1.28 | Test trọn luồng và regression | QA / Senior | Q-02 | 3 |
| 1.29 | Test trọn luồng và regression | Tester / Junior | Q-02 | 8 |
| 1.30 | Sửa lỗi sau test | BE Dev / Senior | Q-02 | 6 |
| 1.31 | Sửa lỗi sau test | BE Dev / Mid | Q-02 | 4 |
| 1.32 | Sửa lỗi sau test | FE Dev / Senior | Q-02 | 4 |
| 1.33 | Sửa lỗi sau test | FE Dev / Junior | Q-02 | 2 |
| 1.34 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 8 |
| 1.35 | Dự phòng Q-01 (rủi ro R-01, R-02, R-03, R-27) | Quỹ dự phòng | Q-01 | 24 |
| 1.36 | Dự phòng Q-02 (rủi ro R-04, R-05, R-06) | Quỹ dự phòng | Q-02 | 7 |
| 1.37 | Xác nhận phạm vi và tiêu chí với trường | PM / Senior | Q-01 | 2 |

### 2.2. MS-02 · 245 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 2.01 | Phát triển backend | BE Dev / Senior | Q-03 | 35 |
| 2.02 | Phát triển backend | BE Dev / Mid | Q-03 | 51 |
| 2.03 | Phát triển frontend | FE Dev / Senior | Q-03 | 14 |
| 2.04 | Phát triển frontend | FE Dev / Junior | Q-03 | 28 |
| 2.05 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | BE Dev / Mid | Q-03 | 11 |
| 2.06 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Senior | Q-03 | 2 |
| 2.07 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Junior | Q-03 | 4 |
| 2.08 | Viết test case từ AC | QA / Senior | Q-03 | 6 |
| 2.09 | Viết test case từ AC | Tester / Junior | Q-03 | 12 |
| 2.10 | Test trọn luồng và regression | QA / Senior | Q-03 | 7 |
| 2.11 | Test trọn luồng và regression | Tester / Junior | Q-03 | 15 |
| 2.12 | Sửa lỗi sau test | BE Dev / Senior | Q-03 | 10 |
| 2.13 | Sửa lỗi sau test | BE Dev / Mid | Q-03 | 8 |
| 2.14 | Sửa lỗi sau test | FE Dev / Senior | Q-03 | 7 |
| 2.15 | Sửa lỗi sau test | FE Dev / Junior | Q-03 | 4 |
| 2.16 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 10 |
| 2.17 | Dự phòng Q-03 (rủi ro R-07, R-08, R-09) | Quỹ dự phòng | Q-03 | 19 |
| 2.18 | Kiểm lại sau sửa lỗi và đối chiếu trước demo | Tester / Junior | Q-03 | 2 |

### 2.3. MS-03 · 138 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 3.01 | Phát triển backend | BE Dev / Senior | Q-04 | 12 |
| 3.02 | Phát triển backend | BE Dev / Mid | Q-04 | 28 |
| 3.03 | Phát triển frontend | FE Dev / Senior | Q-04 | 6 |
| 3.04 | Phát triển frontend | FE Dev / Junior | Q-04 | 18 |
| 3.05 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | BE Dev / Mid | Q-04 | 6 |
| 3.06 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Senior | Q-04 | 2 |
| 3.07 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Junior | Q-04 | 4 |
| 3.08 | Viết test case từ AC | QA / Senior | Q-04 | 3 |
| 3.09 | Viết test case từ AC | Tester / Junior | Q-04 | 5 |
| 3.10 | Test trọn luồng và regression | QA / Senior | Q-04 | 4 |
| 3.11 | Test trọn luồng và regression | Tester / Junior | Q-04 | 14 |
| 3.12 | Sửa lỗi sau test | BE Dev / Senior | Q-04 | 5 |
| 3.13 | Sửa lỗi sau test | BE Dev / Mid | Q-04 | 5 |
| 3.14 | Sửa lỗi sau test | FE Dev / Senior | Q-04 | 2 |
| 3.15 | Sửa lỗi sau test | FE Dev / Junior | Q-04 | 3 |
| 3.16 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 8 |
| 3.17 | Dự phòng Q-04 (rủi ro R-10, R-11, R-12) | Quỹ dự phòng | Q-04 | 11 |
| 3.18 | Kiểm lại sau sửa lỗi và đối chiếu trước demo | Tester / Junior | Q-04 | 2 |

### 2.4. MS-04 · 67 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 4.01 | Phát triển backend | BE Dev / Senior | Q-05 | 5 |
| 4.02 | Phát triển backend | BE Dev / Mid | Q-05 | 10 |
| 4.03 | Phát triển frontend | FE Dev / Senior | Q-05 | 4 |
| 4.04 | Phát triển frontend | FE Dev / Junior | Q-05 | 6 |
| 4.05 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | BE Dev / Mid | Q-05 | 4 |
| 4.06 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Junior | Q-05 | 3 |
| 4.07 | Viết test case từ AC | QA / Senior | Q-05 | 1 |
| 4.08 | Viết test case từ AC | Tester / Junior | Q-05 | 2 |
| 4.09 | Test trọn luồng và regression | QA / Senior | Q-05 | 3 |
| 4.10 | Test trọn luồng và regression | Tester / Junior | Q-05 | 10 |
| 4.11 | Sửa lỗi sau test | BE Dev / Senior | Q-05 | 2 |
| 4.12 | Sửa lỗi sau test | BE Dev / Mid | Q-05 | 2 |
| 4.13 | Sửa lỗi sau test | FE Dev / Senior | Q-05 | 1 |
| 4.14 | Sửa lỗi sau test | FE Dev / Junior | Q-05 | 1 |
| 4.15 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 6 |
| 4.16 | Dự phòng Q-05 (rủi ro R-13, R-14, R-15) | Quỹ dự phòng | Q-05 | 5 |
| 4.17 | Kiểm lại sau sửa lỗi và đối chiếu trước demo | Tester / Junior | Q-05 | 2 |

### 2.5. MS-05 · 112 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 5.01 | Phát triển backend | BE Dev / Senior | Q-06 | 12 |
| 5.02 | Phát triển backend | BE Dev / Mid | Q-06 | 13 |
| 5.03 | Phát triển frontend | FE Dev / Senior | Q-06 | 8 |
| 5.04 | Phát triển frontend | FE Dev / Junior | Q-06 | 9 |
| 5.05 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | BE Dev / Mid | Q-06 | 3 |
| 5.06 | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | FE Dev / Junior | Q-06 | 2 |
| 5.07 | Bộ 200 ticket mẫu | BE Dev / Mid | Q-06 | 5 |
| 5.08 | Bộ 200 ticket mẫu | Tester / Junior | Q-06 | 11 |
| 5.09 | Viết test case từ AC | QA / Senior | Q-06 | 2 |
| 5.10 | Viết test case từ AC | Tester / Junior | Q-06 | 3 |
| 5.11 | Test trọn luồng và regression | QA / Senior | Q-06 | 4 |
| 5.12 | Test trọn luồng và regression | Tester / Junior | Q-06 | 14 |
| 5.13 | Sửa lỗi sau test | BE Dev / Senior | Q-06 | 3 |
| 5.14 | Sửa lỗi sau test | BE Dev / Mid | Q-06 | 2 |
| 5.15 | Sửa lỗi sau test | FE Dev / Senior | Q-06 | 2 |
| 5.16 | Sửa lỗi sau test | FE Dev / Junior | Q-06 | 2 |
| 5.17 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 6 |
| 5.18 | Dự phòng Q-06 (rủi ro R-16, R-17, R-18) | Quỹ dự phòng | Q-06 | 9 |
| 5.19 | Kiểm lại sau sửa lỗi và đối chiếu trước demo | Tester / Junior | Q-06 | 2 |

### 2.6. MS-06 · 159 giờ

| WBS | Công việc | Vai trò | Hạng mục | Giờ |
|---|---|---|---|---:|
| 6.01 | Docker Compose production và .env.example | DevOps / Mid | Q-07 | 10 |
| 6.02 | Triển khai lên server trường, Admin đầu tiên, nối SSO thật | BE Dev / Senior | Q-07 | 2 |
| 6.03 | Triển khai production, nối SSO và kiểm TS-19, TS-64 | DevOps / Mid | Q-07 | 16 |
| 6.04 | Hỗ trợ Go-live và xử lý sự cố triển khai | BE Dev / Senior | Q-07 | 2 |
| 6.05 | Hỗ trợ Go-live và xử lý sự cố triển khai | DevOps / Mid | Q-07 | 5 |
| 6.06 | OpenAPI và tài liệu mã lỗi | BE Dev / Mid | Q-07 | 14 |
| 6.07 | Hướng dẫn sử dụng 3 vai trò | BA / Mid | Q-07 | 12 |
| 6.08 | Báo cáo kiểm thử chức năng | QA / Senior | Q-07 | 5 |
| 6.09 | Báo cáo kiểm thử chức năng | Tester / Junior | Q-07 | 3 |
| 6.10 | Usability test và Usability Test Report | UI/UX / Mid | Q-07 | 18 |
| 6.11 | Usability test và Usability Test Report | QA / Senior | Q-07 | 3 |
| 6.12 | Demo nghiệm thu: quyết định Go-live sau TS-19, TS-20 và TS-64 | PM / Senior | Q-07 | 6 |
| 6.13 | Buổi demo nghiệm thu với khách | BA / Mid | Q-07 | 3 |
| 6.14 | Bảo hành: sửa lỗi backend | BE Dev / Mid | Q-08 | 24 |
| 6.15 | Bảo hành: sửa lỗi frontend | FE Dev / Junior | Q-08 | 18 |
| 6.16 | Quản lý dự án, báo cáo tuần | PM / Senior | Q-09 | 5 |
| 6.17 | Dự phòng Q-07 (rủi ro R-19, R-20, R-21) | Quỹ dự phòng | Q-07 | 9 |
| 6.18 | Dự phòng Q-09 (rủi ro R-22, R-23) | Quỹ dự phòng | Q-09 | 4 |

### 2.7. Quản lý xuyên suốt · 47 giờ

Q-09 đã nằm trong tổng của sáu milestone. Đối chiếu từng kỳ qua issue #101 và WBS 1.34, 2.16, 3.16, 4.15, 5.17 và 6.16. Giờ dự phòng Q-09 nằm tại 6.18.

### 2.8. Đối soát nguồn lực

Tổng WBS, bảng chi tiết effort và phân bổ đơn giá đều bằng **1109 giờ**. Trong đó 1021 giờ giao việc chuyên môn trực tiếp, 88 giờ PM giữ dự phòng. Sheet Resources ghi đỉnh giờ/tuần theo lịch WBS, tính bằng cách chia giờ mỗi dòng cho các ngày làm việc trong khoảng dự kiến, rồi cộng theo vai trò và tuần. Việc duyệt lịch phải kiểm cả tải từng người và quan hệ phụ thuộc.

## PHẦN 3. GIÁ CHÀO THƯƠNG MẠI

Từng dòng giá = số giờ theo vai trò × đơn giá vai trò ở sheet Đơn giá. Q-10 là chi phí trọn gói theo báo giá. Dự phòng tài chính, phúc lợi, vận hành và biên lợi nhuận thuộc cơ cấu đơn giá bán, theo căn cứ ở sheet Đơn giá. Giá chào làm tròn lên hàng nghìn sau VAT.

| Hạng mục | Giờ | Thành tiền trước VAT (đồng) |
|---|---:|---:|
| Q-01 | 290 | 105.279.643 |
| Q-02 | 90 | 32.224.915 |
| Q-03 | 235 | 80.786.428 |
| Q-04 | 130 | 41.009.586 |
| Q-05 | 61 | 18.556.738 |
| Q-06 | 106 | 32.349.097 |
| Q-07 | 108 | 38.877.253 |
| Q-08 | 42 | 10.752.414 |
| Q-09 | 47 | 23.773.399 |
| Q-10 | 0 | 2.500.000 |
| **Tổng trước VAT** | **1109** | **386.109.473** |
| VAT 10% (hiển thị đến đồng) | | 38.610.947 |
| Tổng sau VAT (hiển thị đến đồng) | | 424.720.420 |
| **Giá chào** | | **424.721.000** |

Ba khoản thanh toán tại Proposal mục 5.2 lần lượt là 127.416.300, 169.888.400 và 127.416.300 đồng.
