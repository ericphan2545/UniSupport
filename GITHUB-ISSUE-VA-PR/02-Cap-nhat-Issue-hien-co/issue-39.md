**FR:** FR-STU-07  
**Milestone:** MS-04 · Trả kết quả cho sinh viên (kết thúc 27/11/2026)  
**Owner:** BE Dev / Mid (backend #40), FE Dev / Senior (frontend #41)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 20/11/2026  
**WBS:** 4.02, 4.03, 4.05, 4.06  
**Thời gian:** 16/11/2026 → 20/11/2026  

## Mô tả (theo PRD)

Sinh viên xem chi tiết một hồ sơ, gồm trạng thái hiện tại, các mốc cập nhật và kết quả xử lý, để biết yêu cầu đang ở đâu và khi nào cần bổ sung thông tin.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-07](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-07-xem-tr%E1%BA%A1ng-th%C3%A1i-v%C3%A0-ti%E1%BA%BFn-%C4%91%E1%BB%99-c%E1%BB%A7a-t%E1%BB%ABng-y%C3%AAu-c%E1%BA%A7u)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-07/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ CLOSED chưa đánh giá -> trạng thái/kết quả/closed_at/timeline/form rating; đã đánh giá -> rating chỉ đọc. Public JSON/timeline không có due_at hoặc trường nội bộ.

- [ ] **AC-02** [Validation]: Nhập mã `USP-26-1` -> 400 "Mã hồ sơ không đúng định dạng".

- [ ] **AC-03** [Security]: Mở hồ sơ của sinh viên khác -> 404. Response của hồ sơ hợp lệ không chứa ghi chú nội bộ, thông tin nhân viên, mức ưu tiên, `due_at` hay lý do chuyển phòng ban.

- [ ] **AC-04** [Concurrency]: Trang đang mở và nhân viên đổi trạng thái -> trạng thái mới hiển thị trong vòng 30 giây mà không cần tải lại trang.

- [ ] **AC-05** [Boundary]: Hồ sơ `OVERDUE` có yêu cầu bổ sung đang mở -> trạng thái hiển thị "Quá hạn", đồng thời có thông báo "Cần bổ sung thông tin" và nút Bổ sung.

- [ ] **AC-06** [Resilience & Audit]: API lịch sử lỗi -> thông tin chính vẫn hiển thị, không trắng trang. Sinh viên tải tệp từ trang chi tiết -> nhật ký có bản ghi `DOWNLOAD_FILE`.
- [ ] **AC-07** [Happy Path]: Mở hồ sơ `IN_PROGRESS` -> hiển thị mã hồ sơ, loại yêu cầu, phòng ban hiện tại, trạng thái, ngày gửi, mô tả và tệp đính kèm.

- [ ] **AC-08** [Rate Limit]: Request xem chi tiết thứ 61 trong 1 phút -> 429 kèm `Retry-After`.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-07/AC-01` đến `FR-STU-07/AC-08` trên Staging, kiểm bằng kịch bản `TS-27`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
