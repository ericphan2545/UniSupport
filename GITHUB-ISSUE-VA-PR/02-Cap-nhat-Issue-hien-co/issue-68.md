**FR:** FR-STF-02  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #69), FE Dev / Junior (frontend #70)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Nhân viên xem đầy đủ thông tin một hồ sơ để xử lý, không cần hỏi lại sinh viên những thông tin đã có.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-02](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-02-xem-th%C3%B4ng-tin-sinh-vi%C3%AAn-v%C3%A0-n%E1%BB%99i-dung-y%C3%AAu-c%E1%BA%A7u)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-02/AC-xx`)

- [ ] **AC-01** [Happy Path]: Mở hồ sơ hợp lệ của phòng ban -> 200, hiển thị đủ thông tin sinh viên, nội dung, tệp, nhật ký và lịch sử.

- [ ] **AC-02** [Validation]: Snapshot không có SĐT -> UI hiển thị `--`, không lỗi.

- [ ] **AC-03** [Security]: Mở hồ sơ của phòng ban khác -> 404. Nhân viên không phụ trách mở hồ sơ -> 200, nhưng không thấy các nút xử lý.

- [ ] **AC-04** [Concurrency]: Hai nhân viên cùng mở một hồ sơ -> cả hai nhận cùng giá trị `version`. Sau khi một người cập nhật, người kia thấy `version` mới khi tải lại.

- [ ] **AC-05** [Boundary]: Hồ sơ `CLOSED` -> hiển thị đầy đủ ở chế độ chỉ đọc, không có nút thao tác.

- [ ] **AC-06** [Resilience]: API lịch sử lỗi -> thông tin chính vẫn hiển thị, không trắng trang.
- [ ] **AC-07** [Validation/Rate Limit]: Mở hồ sơ với mã sai định dạng (`USP-26-1`) -> 400. Request thứ 121 trong 1 phút -> 429 kèm `Retry-After`.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-02/AC-01` đến `FR-STF-02/AC-07` trên Staging, kiểm bằng kịch bản `TS-32`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
