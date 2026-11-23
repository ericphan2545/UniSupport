**FR:** FR-STF-05  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Senior (backend #57), FE Dev / Senior (frontend #58)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.01, 3.03, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên phụ trách gửi yêu cầu sinh viên bổ sung thông tin khi hồ sơ chưa đủ dữ liệu để xử lý.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-05](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-05-y%C3%AAu-c%E1%BA%A7u-sinh-vi%C3%AAn-b%E1%BB%95-sung-th%C3%B4ng-tin)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-05/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `IN_PROGRESS`, gửi yêu cầu hợp lệ -> 201; hồ sơ chuyển `PENDING_INFO`; SLA tạm dừng; sinh viên nhận thông báo.

- [ ] **AC-02** [Boundary]: Hồ sơ `OVERDUE`, gửi yêu cầu -> 201; trạng thái vẫn `OVERDUE`; sinh viên vẫn nhận thông báo.

- [ ] **AC-03** [Validation]: Nội dung 9 ký tự -> 400. Hồ sơ đang `PENDING_INFO` -> 409 "Đã có yêu cầu bổ sung đang mở".

- [ ] **AC-04** [Security]: Nhân viên không phụ trách gửi yêu cầu -> 403.

- [ ] **AC-05** [Concurrency/Idempotency]: Bấm Gửi 2 lần cùng `Idempotency-Key` -> 1 yêu cầu, 1 thông báo. Gửi với version cũ khác key -> 409. Retry cùng key đã thành công trả kết quả cũ dù version đổi; cùng key payload khác 409. now > due_at khi còn IN_PROGRESS -> 409, không tạo pause muộn.

- [ ] **AC-06** [Resilience & Audit]: Tạo thông báo lỗi -> rollback cả yêu cầu bổ sung và việc đổi trạng thái. Thành công -> có bản ghi nhật ký `REQUEST_INFO` và mốc trong lịch sử hồ sơ.
- [ ] **AC-07** [Validation]: Hồ sơ `NEW` hoặc `CLOSED` -> 409. Thiếu `version` hoặc `Idempotency-Key` -> 400. Nội dung đúng 1000 ký tự -> 201; 1001 ký tự -> 400.

- [ ] **AC-08** [Business Logic]: Sinh viên trả lời yêu cầu bổ sung (FR-STU-09) -> yêu cầu đóng, người phụ trách tạo được yêu cầu mới. Hồ sơ được đóng (FR-STF-06) khi còn yêu cầu mở -> yêu cầu đóng theo.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-05/AC-01` đến `FR-STF-05/AC-08` trên Staging, kiểm bằng kịch bản `TS-35`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
