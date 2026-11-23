**FR:** FR-STF-09  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Senior (backend #63), FE Dev / Senior (frontend #64)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.01, 3.03, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên chuyển hồ sơ sang đúng phòng ban khi hồ sơ không thuộc trách nhiệm của phòng mình, để sinh viên không phải gửi lại yêu cầu.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-09](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-09-chuy%E1%BB%83n-h%E1%BB%93-s%C6%A1-sang-ph%C3%B2ng-ban-kh%C3%A1c)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-09/AC-xx`)

- [ ] **AC-01** [Happy Path]: Chuyển hồ sơ `IN_PROGRESS` sang phòng ban B -> 200; hồ sơ nằm ở hàng đợi B, mục "Chưa phân công"; trạng thái vẫn `IN_PROGRESS`.

- [ ] **AC-02** [Validation]: Chọn phòng ban đích trùng phòng ban hiện tại -> 400. Lý do 9 ký tự -> 400.

- [ ] **AC-03** [Security]: Ngay sau khi chuyển, nhân viên phòng ban cũ mở hồ sơ -> 404.

- [ ] **AC-04** [Concurrency]: Hai thao tác dùng cùng version cạnh tranh: một cập nhật được commit. Nếu chuyển phòng đã commit, staff phòng cũ gọi tiếp nhận 404 do mất phạm vi; nếu vẫn trong phạm vi nhưng version cũ thì 409. Không bảo đảm thứ tự theo thời điểm bấm Gửi.

- [ ] **AC-05** [Boundary]: Hồ sơ `PENDING_INFO` có yêu cầu bổ sung đang mở được chuyển đi -> yêu cầu vẫn mở; khi sinh viên bổ sung, nội dung đến phòng ban mới.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback, hồ sơ vẫn ở phòng ban cũ cùng người phụ trách cũ. Thành công -> có audit TRANSFER (phòng cũ/mới, transfer_event_id); lý do lưu trong lịch sử nghiệp vụ nội bộ, không nhúng vào audit.
- [ ] **AC-07** [Validation]: Phòng ban đích không tồn tại -> 422. Lý do đúng 500 ký tự -> 200; 501 ký tự -> 400. Hồ sơ đã `CLOSED` -> 409. Thiếu `version` -> 400.

- [ ] **AC-08** [Business Logic]: Sau khi chuyển: `due_at` và loại yêu cầu của hồ sơ giữ nguyên; sinh viên thấy phòng ban mới trong danh sách và chi tiết hồ sơ, không nhận thông báo nào và không thấy lý do chuyển.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-09/AC-01` đến `FR-STF-09/AC-08` trên Staging, kiểm bằng kịch bản `TS-39`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
