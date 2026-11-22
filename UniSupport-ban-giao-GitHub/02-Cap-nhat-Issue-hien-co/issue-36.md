**FR:** FR-STU-06  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #37), FE Dev / Senior (frontend #38)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.03, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Sinh viên xem toàn bộ yêu cầu mình đã gửi cùng trạng thái hiện tại, không phải liên hệ phòng ban để hỏi tiến độ.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-06](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-06-xem-danh-s%C3%A1ch-y%C3%AAu-c%E1%BA%A7u-c%E1%BB%A7a-m%C3%ACnh)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-06/AC-xx`)

- [ ] **AC-01** [Happy Path]: Sinh viên có 25 hồ sơ -> trang 1 có 10 hồ sơ mới nhất, tổng số trang là 3.

- [ ] **AC-02** [Validation]: Lọc `OVERDUE` -> chỉ trả về hồ sơ quá hạn. Không có kết quả -> 200 với danh sách rỗng và dòng chữ "Chưa có yêu cầu nào".

- [ ] **AC-03** [Boundary]: `page_size = 51` -> 400. `page` lớn hơn tổng số trang -> 200 với danh sách rỗng.

- [ ] **AC-04** [Security]: Truyền `student_id` của người khác trên query -> hệ thống bỏ qua, chỉ trả hồ sơ của người dùng trong token.

- [ ] **AC-05** [Rate Limit]: Request thứ 61 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Cơ sở dữ liệu phản hồi quá 5 giây -> 503 với thông báo "Vui lòng thử lại", trang không bị trắng.
- [ ] **AC-07** [Validation]: `page = 0` hoặc trạng thái `ABC` -> 400. Lọc đồng thời `NEW` và `OVERDUE` -> chỉ trả hồ sơ thuộc hai trạng thái này. Tìm đúng mã `USP-2026-00012` -> chỉ trả hồ sơ đó; nhập một phần mã -> không trả hồ sơ nào.

- [ ] **AC-08** [Business Logic]: Hai hồ sơ có cùng `created_at` -> hồ sơ có `ticket_id` lớn hơn đứng trước, thứ tự giữ nguyên giữa các lần tải. Mỗi dòng có mã, loại yêu cầu, phòng ban hiện tại, trạng thái, ngày gửi; hồ sơ có yêu cầu bổ sung đang mở có dấu "Cần bổ sung".


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-06/AC-01` đến `FR-STU-06/AC-08` trên Staging, kiểm bằng kịch bản `TS-26`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
