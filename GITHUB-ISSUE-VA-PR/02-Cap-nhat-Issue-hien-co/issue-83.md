**FR:** FR-MGT-07  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #84), FE Dev / Senior (frontend #85)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.03, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Admin đặt thời hạn xử lý (SLA) cho từng loại yêu cầu, làm căn cứ để hệ thống xác định hồ sơ quá hạn.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-07](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-07-thi%E1%BA%BFt-l%E1%BA%ADp-sla-cho-t%E1%BB%ABng-lo%E1%BA%A1i-y%C3%AAu-c%E1%BA%A7u)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-07/AC-xx`)

- [ ] **AC-01** [Happy Path]: Đổi SLA của loại "Học phí" từ 72 giờ sang 48 giờ -> 200. Hồ sơ "Học phí" tạo sau đó có hạn bằng thời điểm gửi cộng 48 giờ.

- [ ] **AC-02** [Validation/Boundary]: SLA = 0, 721 hoặc 2,5 -> 400. SLA = 1 và 720 -> 200.

- [ ] **AC-03** [Business Logic]: Hồ sơ tạo trước khi đổi SLA -> giữ nguyên hạn cũ (72 giờ).

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API -> 403.

- [ ] **AC-05** [Concurrency/Idempotency]: Hai người trong Admin cùng sửa SLA một loại -> người lưu sau nhận 409. Lưu lại cùng giá trị với version hiện tại -> 200, không thêm audit/tăng version. Seed lại không ghi đè SLA đã sửa; tạo ticket cạnh tranh đổi SLA lưu đúng một snapshot đã commit.

- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback, SLA giữ nguyên. Thành công -> có bản ghi nhật ký `SLA_CHANGE` (loại yêu cầu, giá trị cũ, giá trị mới, người thực hiện).
- [ ] **AC-07** [Validation/Rate Limit]: Sửa SLA của loại yêu cầu không tồn tại -> 404. Thiếu `version` -> 400. Thao tác ghi thứ 31 trong 1 phút -> 429.

- [ ] **AC-08** [Business Logic]: Màn hình SLA liệt kê mỗi loại yêu cầu đúng một giá trị (giá trị ban đầu từ dữ liệu nạp khi triển khai) kèm `version`; không có nút thêm hoặc xóa loại yêu cầu.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-07/AC-01` đến `FR-MGT-07/AC-08` trên Staging, kiểm bằng kịch bản `TS-47`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
