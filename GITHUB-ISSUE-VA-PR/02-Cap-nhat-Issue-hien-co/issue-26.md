**FR:** FR-CORE-04  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #27)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Định nghĩa 3 mức ưu tiên dùng chung, giúp phòng ban sắp xếp và lọc hồ sơ cần xử lý trước.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-04](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-04-m%E1%BB%A9c-%C4%91%E1%BB%99-%C6%B0u-ti%C3%AAn)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-04/AC-xx`)

- [ ] **AC-01** [Happy Path]: Sinh viên tạo hồ sơ -> mức ưu tiên là `MEDIUM`.

- [ ] **AC-02** [Validation]: Gửi mức `URGENT` -> 400.

- [ ] **AC-03** [Boundary]: Hàng đợi có hồ sơ ở cả 3 mức -> thứ tự hiển thị là `HIGH`, `MEDIUM`, `LOW`.

- [ ] **AC-04** [Security]: Sinh viên gửi `priority = HIGH` khi tạo hồ sơ -> hồ sơ vẫn là `MEDIUM`. API chi tiết hồ sơ của sinh viên không trả về trường mức ưu tiên.

- [ ] **AC-05** [Concurrency]: Hai nhân viên đổi mức ưu tiên cùng lúc -> người sau nhận 409 (theo FR-STF-08).

- [ ] **AC-06** [Resilience & Audit]: Đổi mức ưu tiên lỗi -> rollback, mức cũ giữ nguyên. Mỗi lần đổi -> có bản ghi `CHANGE_PRIORITY` hoặc `ESCALATE`.
- [ ] **AC-07** [Business Logic]: Đổi mức ưu tiên của hồ sơ từ `MEDIUM` sang `HIGH` -> `due_at` không đổi. Giao diện VN hiển thị Thấp / Trung bình / Cao; giao diện EN hiển thị Low / Medium / High.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-04/AC-01` đến `FR-CORE-04/AC-07` trên Staging, kiểm bằng kịch bản `TS-57`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
