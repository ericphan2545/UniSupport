**FR:** FR-STF-01  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #51), FE Dev / Senior (frontend #52)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.03, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Hiển thị danh sách hồ sơ thuộc phòng ban của nhân viên, lọc được theo trạng thái, giúp phòng ban không bỏ sót yêu cầu nào.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-01](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-01-h%C3%A0ng-%C4%91%E1%BB%A3i-h%E1%BB%93-s%C6%A1-c%E1%BB%A7a-ph%C3%B2ng-ban)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-01/AC-xx`)

- [ ] **AC-01** [Happy Path]: Phòng ban có 45 hồ sơ -> trang 1 có 20 hồ sơ, sắp xếp đúng theo mức ưu tiên rồi đến ngày gửi.

- [ ] **AC-02** [Validation]: Lọc "Chưa phân công" -> chỉ trả về hồ sơ chưa có người phụ trách. Lọc trạng thái `ABC` -> 400.

- [ ] **AC-03** [Boundary]: `page_size = 100` -> 200. `page_size = 101` -> 400.

- [ ] **AC-04** [Security]: Truyền `department_id` của phòng ban khác trên query -> hệ thống bỏ qua, chỉ trả hồ sơ của phòng ban nhân viên.

- [ ] **AC-05** [Concurrency/Rate Limit]: Hồ sơ mới gửi vào phòng ban trong lúc hàng đợi đang mở -> xuất hiện trong vòng 30 giây. Request thứ 121 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Cơ sở dữ liệu phản hồi quá 5 giây -> 503 với thông báo "Vui lòng thử lại", trang không bị trắng.
- [ ] **AC-07** [Validation]: Lọc trạng thái `NEW` và `OVERDUE` cùng mức ưu tiên `HIGH` -> chỉ trả hồ sơ thỏa cả hai điều kiện. Lọc "Của tôi" -> chỉ trả hồ sơ do nhân viên đang đăng nhập phụ trách. `page = 0` -> 400.

- [ ] **AC-08** [Business Logic]: Hai hồ sơ cùng mức ưu tiên và cùng ngày gửi -> hồ sơ có `ticket_id` nhỏ hơn đứng trước. Mỗi dòng có mã, loại yêu cầu, trạng thái, mức ưu tiên, người phụ trách hoặc "Chưa phân công", ngày gửi, hạn xử lý. Hồ sơ `PENDING_INFO` hiển thị "Chờ sinh viên" và SLA tạm dừng, không hiển thị số giờ còn lại âm.

- [ ] **AC-09** [Security]: Tài khoản `STAFF` chưa được gán phòng ban mở hàng đợi -> 403 "Tài khoản chưa được gán phòng ban".


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-01/AC-01` đến `FR-STF-01/AC-09` trên Staging, kiểm bằng kịch bản `TS-31`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
