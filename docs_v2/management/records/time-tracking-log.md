# Bảng theo dõi giờ công (Mẫu chuẩn & dữ liệu Mô phỏng)

> **Trạng thái dữ liệu:** Dữ liệu dưới đây là **Mô phỏng**, tổng hợp giờ công tính đến **27/11/2026 (tuần 12, `MS-04`)**. Nhật ký chi tiết theo ngày giữ ví dụ **Tuần 6 (`12/10 – 16/10/2026`, `MS-02`)** để đối chiếu trực tiếp với ví dụ Báo cáo tuần ở [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2.
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

## 2. Tổng hợp đến 27/11/2026

**Mô phỏng, đến 27/11/2026 (tuần 12).** `MS-01`, `MS-02`, `MS-03` đã nghiệm thu và đóng ([decision-log.md](decision-log.md) `DL-05`, `DL-06`, `DL-07`; biên bản [MS-01](nghiem-thu-MS-01.md), [MS-02](nghiem-thu-MS-02.md), [MS-03](nghiem-thu-MS-03.md)). `MS-04` đã xong phát triển, test trọn luồng (23–24/11) và sửa lỗi (25–26/11); còn kiểm lại và demo nghiệm thu ngày 27/11. Giờ thực tế lấy theo effort WBS của các dòng đã xong, cộng giờ dự phòng đã dùng; giờ dự phòng chưa dùng của milestone đã đóng không cộng vào dự báo.

| Mốc | Kế hoạch | Thực tế lũy kế | Dự phòng đã dùng / của mốc | Còn lại (`ETC`) | Dự báo (`EAC`) | Trạng thái |
|---|---:|---:|---:|---:|---:|---|
| MS-01 | 388 | 380 | 23 / 31 | 0 | 380 | Đã đóng 25/09/2026 |
| MS-02 | 245 | 239 | 13 / 19 | 0 | 239 | Đã đóng 23/10/2026 |
| MS-03 | 138 | 136 | 9 / 11 | 0 | 136 | Đã đóng 13/11/2026 |
| MS-04 | 67 | 61 | 2 / 5 | 6 | 67 | Đang chạy, demo nghiệm thu 27/11/2026 |
| MS-05 | 112 | 0 | 0 / 9 | 112 | 112 | Bắt đầu 30/11/2026 |
| MS-06 | 159 | 0 | 0 / 13 | 159 | 159 | Bắt đầu 14/12/2026 |
| **Tổng** | **1109** | **816** | **47 / 88** | **277** | **1093** | |

Chỉ số Giờ công: `EAC` = 816 + 277 = 1093 giờ ≤ 1109 giờ và không task nào vượt quá 20% giờ kế hoạch dòng WBS → **Xanh**. Còn lại của `MS-04` gồm kiểm lại trước demo (`4.17`, 2 giờ), quản lý ngày 27/11 (`4.15`, 1 giờ) và 3 giờ dự phòng `Q-05` chưa dùng.

### Nhật ký mẫu tuần 6 (12/10 – 16/10/2026)

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
