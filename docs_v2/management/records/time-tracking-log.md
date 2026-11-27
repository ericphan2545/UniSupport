# Bảng theo dõi giờ công (Mẫu chuẩn & dữ liệu Mô phỏng)

> **Trạng thái dữ liệu:** Dữ liệu dưới đây là **Mô phỏng**, minh họa cách ghi nhận và tổng hợp giờ công tính đến hết **Tuần 6 (`12/10 – 16/10/2026`, `MS-02`)** để đối chiếu trực tiếp với ví dụ Báo cáo tuần ở [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2.
>
> **Người quản lý:** `PM / Senior` (ban quản lý: Lê Hoàng Tùng) · **Quyền đọc:** công khai (repo công khai). **Cập nhật:** mỗi vai trò đội thực thi ghi giờ trước 21h30 mỗi ngày làm việc; Lê Hoàng Tùng mở PR, Phan Tiến Thịnh duyệt. Nơi lưu chính thức: file này trong repo, cập nhật qua PR ([records/README.md](README.md)).

## 1. Nguồn dữ liệu và công thức tính chỉ số Giờ công

Chỉ số **Giờ công** trong [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 3.1 sử dụng trực tiếp hai nguồn gốc là **Bảng theo dõi giờ này** và **sheet `WBS`** của [UniSupport_Masterplan.xlsx](../UniSupport_Masterplan.xlsx) (baseline **1109 giờ** = 1021 giờ cơ sở + 88 giờ Quỹ dự phòng rủi ro):

1. **Giờ thực tế trong tuần:** tổng cột `Số giờ (h)` của tất cả bản ghi có `Phạm vi = Baseline` (gồm cả giờ lấy thêm từ năng lực dư ở sheet `Resources` để thực hiện/kiểm thử phạm vi baseline và giờ đã giải ngân từ Quỹ dự phòng `Dự phòng Q-xx`) trong kỳ báo cáo.
2. **Giờ thực tế lũy kế (`AC`):** tổng cột `Số giờ (h)` của tất cả bản ghi `Phạm vi = Baseline` từ Tuần 1 đến hết kỳ báo cáo.
3. **Giờ kế hoạch còn lại (`ETC`):** tổng `Effort (giờ)` trên sheet `WBS` của các dòng baseline **chưa hoàn thành**, gồm:
   - Giờ kế hoạch còn lại của các dòng thuộc milestone đang chạy (tính bằng `Giờ WBS kế hoạch − Giờ thực tế đã làm` của phần việc chưa đóng, tối thiểu `0` nếu dòng đang vượt; kèm phần giờ `Dự phòng Q-xx` chưa giải ngân của milestone đó).
   - Toàn bộ `Effort (giờ)` trên sheet `WBS` của các milestone tương lai chưa bắt đầu.
4. **Dự báo tổng giờ baseline (`EAC`):**
   $$\text{EAC} = \text{Giờ thực tế lũy kế (AC)} + \text{Giờ kế hoạch còn lại (ETC)}$$
   - **Xanh:** `EAC ≤ 1109 giờ` và không có task nào vượt quá `20%` giờ kế hoạch dòng WBS.
   - **Vàng:** `EAC ≤ 1109 giờ`, nhưng có ít nhất 1 task vượt quá `20%` giờ kế hoạch.
   - **Đỏ:** `EAC > 1109 giờ` (bên phát triển chịu 100% chi phí vượt nội bộ; PM thông báo khách bằng văn bản nếu ảnh hưởng mốc bàn giao hoặc phát sinh từ thay đổi phạm vi/giả định, xem [Bang_Effort.md](../Bang_Effort.md) mục 2.0d).
5. **Tách riêng giờ Change Request (`CR-xx`):** các bản ghi có `Phạm vi = CR-xx` được cộng riêng theo ngân sách CR đã duyệt, không cộng lẫn vào `EAC` baseline 1109 giờ.

## 2. Bảng theo dõi đến hết tuần 6

**Mô phỏng, đến 16/10/2026.** MS-01 đã đóng và tiết kiệm 8 giờ dự phòng. Công sức MS-02 đến hết tuần 6 lấy theo phân bổ ngày làm việc của từng dòng WBS đang thực hiện. Đợt test trọn luồng bắt đầu ngày 19/10. Các dòng sau ngày 16/10 và dự phòng chưa giải ngân nằm trong phần còn lại.

| Mốc | Kế hoạch | Thực tế trước tuần 6 | Tuần 6 | Lũy kế | Còn lại | Dự báo |
|---|---:|---:|---:|---:|---:|---:|
| MS-01 | 388 | 380 | 0 | 380 | 0 | 380 |
| MS-02 | 245 | 118 | 52 | 170 | 75 | 245 |
| MS-03 | 138 | 0 | 0 | 0 | 138 | 138 |
| MS-04 | 67 | 0 | 0 | 0 | 67 | 67 |
| MS-05 | 112 | 0 | 0 | 0 | 112 | 112 |
| MS-06 | 159 | 0 | 0 | 0 | 159 | 159 |
| **Tổng** | **1109** | **498** | **52** | **550** | **551** | **1101** |

### Nhật ký mô phỏng tuần 6

| Ngày | Vai trò | Tham chiếu issue | WBS | Phạm vi | Công việc | Giờ |
|---|---|---|---|---|---|---:|
| 12/10/2026 | BE Dev / Mid | #123 | 2.05 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 1 |
| 12/10/2026 | BE Dev / Mid | #2, #23, #25, #27, #29, #34, #37, #66, #69 | 2.02 | Baseline | Phát triển backend | 3 |
| 12/10/2026 | BE Dev / Senior | #13, #15, #17, #20, #51, #84 | 2.01 | Baseline | Phát triển backend | 2 |
| 12/10/2026 | FE Dev / Junior | #3, #18, #35, #49, #67, #70 | 2.04 | Baseline | Phát triển frontend | 2 |
| 12/10/2026 | FE Dev / Senior | #21, #38, #52, #85 | 2.03 | Baseline | Phát triển frontend | 1 |
| 13/10/2026 | BE Dev / Mid | #123 | 2.05 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 1 |
| 13/10/2026 | BE Dev / Mid | #2, #23, #25, #27, #29, #34, #37, #66, #69 | 2.02 | Baseline | Phát triển backend | 3 |
| 13/10/2026 | BE Dev / Senior | #13, #15, #17, #20, #51, #84 | 2.01 | Baseline | Phát triển backend | 3 |
| 13/10/2026 | FE Dev / Junior | #3, #18, #35, #49, #67, #70 | 2.04 | Baseline | Phát triển frontend | 2 |
| 13/10/2026 | FE Dev / Senior | #21, #38, #52, #85 | 2.03 | Baseline | Phát triển frontend | 1 |
| 13/10/2026 | PM / Senior | #101 | 2.16 | Baseline | Quản lý dự án, báo cáo tuần | 1 |
| 14/10/2026 | BE Dev / Mid | #123 | 2.05 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 1 |
| 14/10/2026 | BE Dev / Mid | #2, #23, #25, #27, #29, #34, #37, #66, #69 | 2.02 | Baseline | Phát triển backend | 4 |
| 14/10/2026 | BE Dev / Senior | #13, #15, #17, #20, #51, #84 | 2.01 | Baseline | Phát triển backend | 2 |
| 14/10/2026 | FE Dev / Junior | #3, #18, #35, #49, #67, #70 | 2.04 | Baseline | Phát triển frontend | 2 |
| 14/10/2026 | FE Dev / Senior | #21, #38, #52, #85 | 2.03 | Baseline | Phát triển frontend | 2 |
| 15/10/2026 | BE Dev / Mid | #123 | 2.05 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 2 |
| 15/10/2026 | BE Dev / Mid | #2, #23, #25, #27, #29, #34, #37, #66, #69 | 2.02 | Baseline | Phát triển backend | 3 |
| 15/10/2026 | BE Dev / Senior | #13, #15, #17, #20, #51, #84 | 2.01 | Baseline | Phát triển backend | 3 |
| 15/10/2026 | FE Dev / Junior | #3, #18, #35, #49, #67, #70 | 2.04 | Baseline | Phát triển frontend | 2 |
| 15/10/2026 | PM / Senior | #101 | 2.16 | Baseline | Quản lý dự án, báo cáo tuần | 1 |
| 16/10/2026 | BE Dev / Mid | #2, #23, #25, #27, #29, #34, #37, #66, #69 | 2.02 | Baseline | Phát triển backend | 4 |
| 16/10/2026 | FE Dev / Junior | #124 | 2.07 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 4 |
| 16/10/2026 | FE Dev / Senior | #124 | 2.06 | Baseline | Phần dùng chung (phạm vi dữ liệu, nhật ký, lịch sử, VN/EN, responsive) | 2 |
| | | | | | **Tổng tuần 6** | **52** |
