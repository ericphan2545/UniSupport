**FR:** FR-CORE-01  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #25)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Hệ thống sinh cho mỗi hồ sơ một mã duy nhất, dễ đọc, dùng để sinh viên và nhân viên tra cứu và trao đổi.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-01](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-01-sinh-m%C3%A3-h%E1%BB%93-s%C6%A1)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-01/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ đầu tiên tạo trong năm 2027 -> mã `USP-2027-00001`. Hồ sơ kế tiếp -> `USP-2027-00002`.

- [ ] **AC-02** [Validation]: Mọi mã sinh ra đều khớp mẫu `^USP-\d{4}-\d{5,}$`.

- [ ] **AC-03** [Boundary]: Hồ sơ tạo lúc 06:59 ngày 01/01/2027 giờ Việt Nam (tức 23:59 ngày 31/12/2026 theo UTC) -> mã năm 2027.

- [ ] **AC-04** [Security]: Mã không chứa MSSV, tên sinh viên hay thông tin cá nhân nào khác.

- [ ] **AC-05** [Concurrency]: 100 hồ sơ được tạo đồng thời -> 100 mã khác nhau, không trùng.

- [ ] **AC-06** [Resilience]: Transaction tạo hồ sơ bị rollback sau khi đã lấy số thứ tự -> không có hồ sơ nào mang mã đó; hồ sơ tiếp theo dùng số kế tiếp.
- [ ] **AC-07** [Boundary]: Năm đã có 99.999 hồ sơ, tạo hồ sơ tiếp theo -> mã `USP-YYYY-100000`, vẫn khớp mẫu ở AC-02. Mã của một hồ sơ giữ nguyên sau khi phân công, chuyển phòng và đóng.

- [ ] **AC-08** [Resilience]: Không lấy được số thứ tự từ sequence -> 500, không tạo hồ sơ. Vi phạm unique constraint của cột mã -> 500, rollback, log lỗi hệ thống.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-01/AC-01` đến `FR-CORE-01/AC-08` trên Staging, kiểm bằng kịch bản `TS-54`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
