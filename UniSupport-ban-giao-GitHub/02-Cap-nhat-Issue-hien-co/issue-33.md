**FR:** FR-STU-05  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #34), FE Dev / Junior (frontend #35)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Sau khi gửi thành công, sinh viên nhận mã hồ sơ duy nhất và màn hình xác nhận, để biết chắc yêu cầu đã được tiếp nhận và có mã để tra cứu.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-05](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-05-nh%E1%BA%ADn-m%C3%A3-h%E1%BB%93-s%C6%A1-v%C3%A0-x%C3%A1c-nh%E1%BA%ADn-g%E1%BB%ADi-th%C3%A0nh-c%C3%B4ng)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-05/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ đầu tiên của năm 2027 -> mã `USP-2027-00001`; màn hình xác nhận hiển thị đủ thông tin.

- [ ] **AC-02** [Boundary]: Hồ sơ tạo lúc 23:59:59 ngày 31/12/2026 (giờ VN) -> mã năm 2026. Hồ sơ tạo lúc 00:00:00 ngày 01/01/2027 -> `USP-2027-00001`.

- [ ] **AC-03** [Security]: Sinh viên A tra mã hồ sơ của sinh viên B -> 404.

- [ ] **AC-04** [Concurrency]: 50 hồ sơ gửi đồng thời -> 50 mã khác nhau, không trùng.

- [ ] **AC-05** [Idempotency]: Gửi lại cùng `Idempotency-Key` -> trả về mã cũ, không sinh mã mới.

- [ ] **AC-06** [Resilience & Audit]: Sinh mã lỗi -> 500, không có hồ sơ thiếu mã. Tạo thành công -> mã hồ sơ được ghi trong bản ghi nhật ký `CREATE_TICKET`.
- [ ] **AC-07** [Boundary]: Năm đã có 99.999 hồ sơ, tạo hồ sơ tiếp theo -> mã `USP-YYYY-100000`, không lỗi. Mã hồ sơ giữ nguyên sau khi phân công, chuyển phòng và đóng; mã không chứa MSSV hay họ tên.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-05/AC-01` đến `FR-STU-05/AC-07` trên Staging, kiểm bằng kịch bản `TS-25`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
