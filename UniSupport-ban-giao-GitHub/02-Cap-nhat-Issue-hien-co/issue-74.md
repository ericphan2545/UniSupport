**FR:** FR-STF-08  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Mid (backend #75), FE Dev / Junior (frontend #76)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.02, 3.04, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên trong phòng ban đặt mức ưu tiên cho hồ sơ để phòng ban xử lý việc gấp trước.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-08](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-08-%C4%91%E1%BA%B7t-m%E1%BB%A9c-%C6%B0u-ti%C3%AAn)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-08/AC-xx`)

- [ ] **AC-01** [Happy Path]: Đổi Trung bình -> Thấp -> 200; hàng đợi sắp xếp lại theo mức mới.

- [ ] **AC-02** [Validation]: Gửi `URGENT` -> 400.

- [ ] **AC-03** [Boundary]: Đặt trùng mức hiện tại -> 200, không có mốc lịch sử mới.

- [ ] **AC-04** [Security]: Nhân viên phòng ban khác -> 404.

- [ ] **AC-05** [Concurrency]: Hai nhân viên đổi mức ưu tiên cùng lúc -> người sau nhận 409.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `CHANGE_PRIORITY` (mức cũ, mức mới).
- [ ] **AC-07** [Business Logic]: Đổi mức ưu tiên từ `LOW` lên `HIGH` -> `due_at` của hồ sơ không đổi. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-08/AC-01` đến `FR-STF-08/AC-07` trên Staging, kiểm bằng kịch bản `TS-38`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
