**FR:** FR-STF-04  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Mid (backend #72), FE Dev / Junior (frontend #73)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.02, 3.04, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Nhân viên phụ trách ghi lại các bước đã làm với hồ sơ, giúp người khác nắm được lịch sử xử lý khi tiếp nhận hoặc kiểm tra lại.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-04](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-04-ghi-nh%E1%BA%ADt-k%C3%BD-x%E1%BB%AD-l%C3%BD)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-04/AC-xx`)

- [ ] **AC-01** [Happy Path]: Người phụ trách lưu ghi chú 100 ký tự -> 201; ghi chú hiển thị kèm tên người ghi và thời gian; trạng thái hồ sơ không đổi.

- [ ] **AC-02** [Validation/Boundary]: Nội dung toàn khoảng trắng -> 400. Đúng 2000 ký tự -> 201. 2001 ký tự -> 400.

- [ ] **AC-03** [Security]: Nhân viên cùng phòng ban nhưng không phụ trách -> 403. Sinh viên xem hồ sơ -> không thấy ghi chú nội bộ.

- [ ] **AC-04** [Idempotency]: Bấm Lưu 2 lần cùng `Idempotency-Key` -> chỉ có 1 ghi chú.

- [ ] **AC-05** [Business Logic]: Ghi chú vào hồ sơ `CLOSED` -> 409.

- [ ] **AC-06** [Resilience & Audit]: Lưu ghi chú lỗi -> 500, UI giữ nội dung đã nhập để lưu lại. Lưu thành công -> có bản ghi nhật ký `ADD_NOTE`.
- [ ] **AC-07** [Security]: Ghi chú chứa `<script>` -> lưu dạng văn bản, khi hiển thị không thực thi. Ghi chú đã lưu không có nút hoặc API sửa, xóa.

- [ ] **AC-08** [Validation/Rate Limit]: Thiếu header `Idempotency-Key` -> 400. Ghi chú thứ 31 trong 1 phút -> 429. Lưu ghi chú thành công -> `version` của hồ sơ tăng đúng 1.

- [ ] **AC-09** [Business Logic]: Người phụ trách ghi chú "Hồ sơ gửi nhầm phòng" rồi chuyển hồ sơ sang phòng B (FR-STF-09) -> nhân viên phòng B thấy ghi chú này trong nhật ký nội bộ. Nhân viên không phụ trách vẫn chuyển được hồ sơ khi nhập lý do, không cần ghi chú trước.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-04/AC-01` đến `FR-STF-04/AC-09` trên Staging, kiểm bằng kịch bản `TS-34`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
