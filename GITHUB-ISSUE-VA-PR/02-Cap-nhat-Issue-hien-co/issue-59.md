**FR:** FR-STF-06  
**Milestone:** MS-04 · Trả kết quả cho sinh viên (kết thúc 27/11/2026)  
**Owner:** BE Dev / Senior (backend #60), FE Dev / Senior (frontend #61)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 20/11/2026  
**WBS:** 4.01, 4.03, 4.05, 4.06  
**Thời gian:** 16/11/2026 → 20/11/2026  

## Mô tả (theo PRD)

Nhân viên phụ trách đóng hồ sơ khi đã xử lý xong, thông báo kết quả cho sinh viên và kết thúc tính SLA.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-06](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-06-%C4%91%C3%B3ng-h%E1%BB%93-s%C6%A1)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-06/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `IN_PROGRESS`, nhập kết quả hợp lệ -> 200; hồ sơ chuyển `CLOSED`; sinh viên nhận thông báo và thấy form đánh giá.

- [ ] **AC-02** [Boundary]: PENDING_INFO có info request mở -> close hợp lệ, request đóng, cộng bù pause cuối đến closed_at, version tăng một lần; retry cùng key trả kết quả cũ, không thêm history/noti.

- [ ] **AC-03** [Validation]: Kết quả xử lý 9 ký tự -> 400. Đóng hồ sơ đã `CLOSED` -> 409.

- [ ] **AC-04** [Security]: Nhân viên không phụ trách đóng hồ sơ -> 403.

- [ ] **AC-05** [Concurrency]: Nhân viên đóng và sinh viên bổ sung dùng cùng version đã đọc -> chỉ một thao tác commit; bên kia nhận 409. Nếu bên đóng tải version mới sau bổ sung rồi đóng thì hợp lệ.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback, trạng thái giữ nguyên. Thành công -> có bản ghi nhật ký `CLOSE_TICKET` và mốc trong lịch sử hồ sơ.
- [ ] **AC-07** [Validation]: Đóng hồ sơ đang `NEW` -> 409. Thiếu `resolution` -> 400; `resolution` đúng 2000 ký tự -> 200, 2001 ký tự -> 400. Thiếu `version` hoặc `Idempotency-Key` -> 400.

- [ ] **AC-08** [Business Logic]: Đóng hồ sơ lúc 10:00 -> `closed_at` = 10:00 theo giờ server; hồ sơ được tính vào thời gian xử lý trung bình (FR-MGT-02) theo mốc này. Sau khi đóng, mọi thao tác đổi trạng thái hồ sơ -> 409.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-06/AC-01` đến `FR-STF-06/AC-08` trên Staging, kiểm bằng kịch bản `TS-36`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
