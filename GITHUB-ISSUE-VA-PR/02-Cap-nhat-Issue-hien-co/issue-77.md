**FR:** FR-STF-10  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Mid (backend #78), FE Dev / Junior (frontend #79)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.02, 3.04, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên tăng mức ưu tiên của hồ sơ kèm lý do khi phát hiện vấn đề gấp hơn dự kiến, để phòng ban xử lý sớm hơn.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-10](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-10-t%C4%83ng-m%E1%BB%A9c-%C6%B0u-ti%C3%AAn-escalate)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-10/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `MEDIUM`, escalate có lý do hợp lệ -> 200; mức ưu tiên thành `HIGH`; lịch sử ghi một lần escalate.

- [ ] **AC-02** [Validation]: Escalate không có lý do -> 400.

- [ ] **AC-03** [Boundary]: Hồ sơ `LOW` escalate 2 lần -> `HIGH`. Escalate lần thứ 3 -> 409.

- [ ] **AC-04** [Security]: Nhân viên phòng ban khác -> 404.

- [ ] **AC-05** [Concurrency]: Hai nhân viên cùng escalate một hồ sơ `MEDIUM` cùng lúc -> chỉ tăng một bậc lên `HIGH`, người sau nhận 409.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback. Thành công -> có audit ESCALATE (mức cũ/mới, escalation_event_id); lý do lưu lịch sử nghiệp vụ nội bộ.
- [ ] **AC-07** [Validation/Business Logic]: Lý do 9 ký tự hoặc 501 ký tự -> 400. Escalate thành công -> `due_at` không đổi; sinh viên không thấy lý do escalate. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-10/AC-01` đến `FR-STF-10/AC-07` trên Staging, kiểm bằng kịch bản `TS-40`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
