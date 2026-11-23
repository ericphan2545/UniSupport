**FR:** FR-STU-03  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** FE Dev / Junior (frontend #49)  
**Hỗ trợ / duyệt:** BE Dev / Mid  
**Hạn:** 16/10/2026  
**WBS:** 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Khi sinh viên chọn loại yêu cầu, hệ thống hiển thị phòng ban tiếp nhận tương ứng và gán phòng ban đó khi tạo hồ sơ, giúp sinh viên biết yêu cầu của mình sẽ được gửi đến đâu.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-03](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-03-hi%E1%BB%83n-th%E1%BB%8B-ph%C3%B2ng-ban-ti%E1%BA%BFp-nh%E1%BA%ADn)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-03/AC-xx`)

- [ ] **AC-01** [Happy Path]: Chọn loại "Học phí" (ví dụ) -> hiển thị phòng ban ánh xạ (ví dụ "Phòng Tài chính"); hồ sơ tạo ra có đúng `department_id` này.

- [ ] **AC-02** [Validation]: Đổi loại yêu cầu 3 lần rồi gửi -> phòng ban hiển thị và phòng ban được gán đều theo loại cuối cùng.

- [ ] **AC-03** [Security]: Client gửi kèm `department_id` khác -> hệ thống bỏ qua, gán phòng ban theo ánh xạ.

- [ ] **AC-04** [Concurrency/Rate Limit]: Nhiều sinh viên mở form cùng lúc -> danh mục được trả từ cache. Request API danh mục thứ 61 trong 1 phút -> 429.

- [ ] **AC-05** [Resilience]: Loại yêu cầu thiếu ánh xạ phòng ban -> 422, không có hồ sơ nào được tạo mà thiếu phòng ban.

- [ ] **AC-06** [Audit]: Hồ sơ vừa tạo -> bản ghi đầu tiên trong lịch sử hồ sơ (CORE-06) có `department_id` ban đầu.
- [ ] **AC-07** [Resilience]: Không tải được danh mục loại yêu cầu -> 503; UI khóa nút Gửi.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-03/AC-01` đến `FR-STU-03/AC-07` trên Staging, kiểm bằng kịch bản `TS-23`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
