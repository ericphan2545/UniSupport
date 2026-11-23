**FR:** FR-STF-07  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Senior (backend #54), FE Dev / Senior (frontend #55)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.01, 3.03, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên trong phòng ban phân công người phụ trách cho hồ sơ, để mọi hồ sơ đều có người chịu trách nhiệm rõ ràng.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-07](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-07-ph%C3%A2n-c%C3%B4ng-nh%C3%A2n-vi%C3%AAn-ph%E1%BB%A5-tr%C3%A1ch)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-07/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `NEW`, phân công nhân viên A -> 200; hồ sơ chuyển `IN_PROGRESS`; sinh viên nhận thông báo đổi trạng thái.

- [ ] **AC-02** [Validation]: Chọn người phòng khác hoặc inactive -> 422. Phân công cạnh tranh deactivate/đổi phòng: không để ticket mở gán người mất quyền, một thao tác bị chặn nguyên tử.

- [ ] **AC-03** [Boundary]: Hồ sơ `OVERDUE` chưa có người phụ trách, phân công A -> 200, trạng thái vẫn `OVERDUE`.

- [ ] **AC-04** [Security]: Nhân viên phòng ban khác gọi API phân công -> 404.

- [ ] **AC-05** [Concurrency/Idempotency]: Hai nhân viên phân công cùng lúc với cùng `version` -> người đầu thành công, người sau nhận 409. Phân công lại đúng người hiện tại -> 200, không có mốc lịch sử mới.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback. Thành công -> có bản ghi nhật ký `ASSIGN` (người cũ, người mới).
- [ ] **AC-07** [Business Logic]: Hồ sơ `PENDING_INFO` đổi người phụ trách từ A sang B -> 200, trạng thái vẫn `PENDING_INFO`. Hồ sơ `NEW` đã qua `due_at` nhưng tác vụ quá hạn chưa chạy, phân công A -> 200, `due_at` không đổi; lần chạy kế tiếp của tác vụ chuyển hồ sơ sang `OVERDUE`.

- [ ] **AC-08** [Validation]: Chọn tài khoản `ADMIN` hoặc `STUDENT` làm người phụ trách -> 422. Thiếu `version` -> 400. Hồ sơ đã `CLOSED` -> 409.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-07/AC-01` đến `FR-STF-07/AC-08` trên Staging, kiểm bằng kịch bản `TS-37`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
