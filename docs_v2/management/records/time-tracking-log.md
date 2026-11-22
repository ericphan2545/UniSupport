# Bảng theo dõi giờ công (Mẫu chuẩn & dữ liệu Mô phỏng)

> **Trạng thái dữ liệu:** Dữ liệu dưới đây là **Mô phỏng**, minh họa cách ghi nhận và tổng hợp giờ công tính đến hết **Tuần 6 (`12/10 – 16/10/2026`, `MS-02`)** để đối chiếu trực tiếp với ví dụ Báo cáo tuần ở [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2.
>
> **Người quản lý:** `PM / Senior` (ban quản lý: Lê Hoàng Tùng) · **Quyền truy cập:** Toàn bộ đội thực thi ghi giờ trước 21h30 mỗi ngày làm việc; PM và ban quản lý xem trực tiếp bất cứ lúc nào. Đường dẫn Google Drive bản vận hành thật: *chờ Lê Hoàng Tùng xác nhận và gắn tại [records/README.md](README.md)*.

## 1. Nguồn dữ liệu và công thức tính chỉ số Giờ công

Chỉ số **Giờ công** trong [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 3.1 sử dụng trực tiếp hai nguồn gốc là **Bảng theo dõi giờ này** và **sheet `WBS`** của [UniSupport_Masterplan_nhom2.xlsx](../UniSupport_Masterplan_nhom2.xlsx) (baseline **870 giờ** = 797 giờ cơ sở + 73 giờ Quỹ dự phòng rủi ro):

1. **Giờ thực tế trong tuần:** tổng cột `Số giờ (h)` của tất cả bản ghi có `Phạm vi = Baseline` (gồm cả giờ lấy thêm từ năng lực dư ở sheet `Resources` để thực hiện/kiểm thử phạm vi baseline và giờ đã giải ngân từ Quỹ dự phòng `Dự phòng Q-xx`) trong kỳ báo cáo.
2. **Giờ thực tế lũy kế (`AC`):** tổng cột `Số giờ (h)` của tất cả bản ghi `Phạm vi = Baseline` từ Tuần 1 đến hết kỳ báo cáo.
3. **Giờ kế hoạch còn lại (`ETC`):** tổng `Effort (giờ)` trên sheet `WBS` của các dòng baseline **chưa hoàn thành**, gồm:
   - Giờ kế hoạch còn lại của các dòng thuộc milestone đang chạy (tính bằng `Giờ WBS kế hoạch − Giờ thực tế đã làm` của phần việc chưa đóng, tối thiểu `0` nếu dòng đang vượt; kèm phần giờ `Dự phòng Q-xx` chưa giải ngân của milestone đó).
   - Toàn bộ `Effort (giờ)` trên sheet `WBS` của các milestone tương lai chưa bắt đầu.
4. **Dự báo tổng giờ baseline (`EAC`):**
   $$\text{EAC} = \text{Giờ thực tế lũy kế (AC)} + \text{Giờ kế hoạch còn lại (ETC)}$$
   - **Xanh:** `EAC ≤ 870 giờ` và không có task nào vượt quá `20%` giờ kế hoạch dòng WBS.
   - **Vàng:** `EAC ≤ 870 giờ`, nhưng có ít nhất 1 task vượt quá `20%` giờ kế hoạch.
   - **Đỏ:** `EAC > 870 giờ` (bên phát triển chịu 100% chi phí vượt nội bộ; PM thông báo khách bằng văn bản nếu ảnh hưởng mốc bàn giao hoặc phát sinh từ thay đổi phạm vi/giả định, xem [Bang_Effort.md](../Bang_Effort.md) mục 2.0d).
5. **Tách riêng giờ Change Request (`CR-xx`):** các bản ghi có `Phạm vi = CR-xx` được cộng riêng theo ngân sách CR đã duyệt, không cộng lẫn vào `EAC` baseline 870 giờ.

## 2. Bảng tổng hợp lũy kế đến hết Tuần 6 (`16/10/2026` — Mô phỏng)

Khớp 100% với số liệu ví dụ Báo cáo tuần 6 trong [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2 (`tuần này 52 giờ, lũy kế 455 giờ, còn lại 407 giờ, dự báo 862 giờ ≤ 870 giờ → Xanh`):

| Milestone | Trạng thái tại `16/10/2026` | Kế hoạch WBS (h) | Thực tế trước Tuần 6 (h) | Thực tế Tuần 6 (h) | Lũy kế thực tế `AC` (h) | Kế hoạch còn lại `ETC` (h) | Dự báo `EAC` (h) | Ghi chú (Mô phỏng) |
|---|---|---:|---:|---:|---:|---:|---:|---|
| `MS-01` | Đã đóng (`25/09`) | 364 | 356 | 0 | 356 | 0 | 356 | Tiết kiệm 8h (chỉ dùng 22/30h dự phòng `Q-01/Q-02`) |
| `MS-02` | Đang chạy (hết Tuần 6/7) | 134 | 47 | **52** | 99 | 35 | 134 | Tuần 4: 22h, Tuần 5: 25h, Tuần 6: 52h; còn lại Tuần 7: 24h cơ sở (`2.10–2.16`) + 11h dự phòng `Q-03` chưa dùng |
| `MS-03` | Chưa bắt đầu | 88 | 0 | 0 | 0 | 88 | 88 | Theo baseline WBS |
| `MS-04` | Chưa bắt đầu | 46 | 0 | 0 | 0 | 46 | 46 | Theo baseline WBS |
| `MS-05` | Chưa bắt đầu | 76 | 0 | 0 | 0 | 76 | 76 | Theo baseline WBS |
| `MS-06` | Chưa bắt đầu | 162 | 0 | 0 | 0 | 162 | 162 | Theo baseline WBS |
| **TỔNG BASELINE** | — | **870** | **403** | **52** | **455** | **407** | **862** | **Chênh −8h so với 870h; không task nào lệch > 20% → Xanh** |

## 3. Nhật ký ghi giờ chi tiết Tuần 6 (`12/10 – 16/10/2026` — Mô phỏng)

| Ngày | Vai trò (Mô phỏng) | Issue | Mã WBS | Phạm vi | Nội dung công việc | Số giờ (h) | Nguồn giờ |
|---|---|---:|---|---|---|---|---|
| 12/10/2026 | BE Dev / Senior | `#13`, `#15` | `2.01` | Baseline | Hoàn thiện state machine (`FR-CORE-02`) và SLA (`FR-CORE-03`) | 4 | Cơ sở WBS |
| 12/10/2026 | BE Dev / Mid | `#2`, `#25` | `2.02` | Baseline | API tạo yêu cầu (`FR-STU-02`) và sinh mã hồ sơ (`FR-CORE-01`) | 6 | Cơ sở WBS |
| 13/10/2026 | BE Dev / Senior | `#17`, `#20` | `2.01` | Baseline | API thông báo in-app (`FR-STU-08`) và upload tệp (`FR-STU-04`) | 4 | Cơ sở WBS |
| 13/10/2026 | BE Dev / Mid | `#123` | `2.05` | Baseline | Bảo vệ tệp (`FR-SEC-05`), phạm vi dữ liệu & lịch sử `MS-02` | 3 | Cơ sở WBS |
| 13/10/2026 | FE Dev / Senior | `#21`, `#52` | `2.03` | Baseline | Màn hình đính kèm tệp và hàng đợi phòng ban | 3 | Cơ sở WBS |
| 13/10/2026 | FE Dev / Junior | `#3`, `#18` | `2.04` | Baseline | Form tạo yêu cầu và chuông thông báo polling 30 giây | 5 | Cơ sở WBS |
| 14/10/2026 | BE Dev / Mid | `#34`, `#37`, `#66`, `#69` | `2.02` | Baseline | Danh sách SV, chi tiết hồ sơ và tải tệp nhân viên | 6 | Cơ sở WBS |
| 14/10/2026 | FE Dev / Senior | `#124` | `2.06` | Baseline | Đồng bộ VN/EN và bố cục responsive màn hình `MS-02` | 1 | Cơ sở WBS |
| 14/10/2026 | FE Dev / Junior | `#124` | `2.07` | Baseline | Hoàn thiện nhãn VN/EN và giao diện thẻ mobile 360px `MS-02` | 2 | Cơ sở WBS |
| 15/10/2026 | BE Dev / Senior | `#127` | `2.12` | Baseline | Hỗ trợ xử lý đồng thời `FR-CORE-01` và sửa lỗi sớm sau test | 4 | Cơ sở WBS |
| 15/10/2026 | FE Dev / Senior | `#128` | `2.14` | Baseline | Sửa lỗi giao diện đính kèm tệp & hàng đợi sau test sớm | 3 | Cơ sở WBS |
| 15/10/2026 | QA / Senior | `#126` | `2.10` | Baseline | Review kết quả test chức năng sớm `MS-02` | 1 | Cơ sở WBS |
| 15/10/2026 | Tester / Junior | `#126` | `2.11` | Baseline | Chạy test chức năng `TS-22` đến `TS-26` trên Staging | 4 | Cơ sở WBS |
| 16/10/2026 | Tester / Junior | `#126` | `2.11` | Baseline | Chạy test chức năng `TS-28`, `TS-31..33`, `TS-47`, `TS-52` | 4 | Cơ sở WBS |
| 16/10/2026 | PM / Senior | `#101` | `2.16` | Baseline | Tổng hợp giờ Tuần 6, đối chiếu tiến độ và gửi báo cáo tuần cho trường | 2 | Cơ sở WBS |
| — | **TỔNG TUẦN 6** | — | — | — | **Khớp 100% Báo cáo tuần 6 (`03` mục 6.2)** | **52** | — |
