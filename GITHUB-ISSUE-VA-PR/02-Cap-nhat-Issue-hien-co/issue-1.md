**FR:** FR-STU-02  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #2), FE Dev / Junior (frontend #3)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Sinh viên tạo yêu cầu hỗ trợ bằng cách chọn loại yêu cầu và nhập mô tả vấn đề, để mọi yêu cầu đi vào một kênh duy nhất và theo dõi được.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-02](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-02-t%E1%BA%A1o-y%C3%AAu-c%E1%BA%A7u-h%E1%BB%97-tr%E1%BB%A3)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-02/AC-xx`)

- [ ] **AC-01** [Happy Path]: Chọn loại yêu cầu có SLA 48 giờ, mô tả 50 ký tự, gửi lúc 08:00 ngày 01/10 -> 201; hồ sơ `NEW`, ưu tiên `MEDIUM`, có mã, có snapshot thông tin sinh viên, `due_at` = 08:00 ngày 03/10.

- [ ] **AC-02** [Validation/Boundary]: Mô tả 19 ký tự, 2001 ký tự hoặc toàn khoảng trắng -> 400, không tạo hồ sơ. Mô tả đúng 20 hoặc 2000 ký tự -> 201.

- [ ] **AC-03** [Security]: Body chứa `student_id`, `priority` hoặc `due_at` -> hệ thống bỏ qua tất cả, dùng giá trị do hệ thống xác định. Token vai trò `STAFF` -> 403.

- [ ] **AC-04** [Idempotency]: Gửi 2 lần cùng `Idempotency-Key` trong 24 giờ -> chỉ tạo 1 hồ sơ, lần thứ hai trả về đúng mã hồ sơ đã tạo.

- [ ] **AC-05** [Rate Limit]: Gửi hồ sơ thứ 11 trong cùng 1 giờ -> 429 kèm `Retry-After`.

- [ ] **AC-06** [Resilience & Audit]: SSO không phản hồi khi lấy snapshot -> 504, không có hồ sơ nào được tạo. Lỗi cơ sở dữ liệu giữa chừng -> 500, không có hồ sơ dở dang; tệp tạm được dọn bù theo quy ước chung. Tạo thành công -> nhật ký có bản ghi `CREATE_TICKET`.
- [ ] **AC-07** [Validation]: Không chọn loại yêu cầu -> 400, danh sách trường lỗi có `request_type_id`. `request_type_id` không có trong danh mục, hoặc loại yêu cầu chưa có SLA -> 422. Cả hai trường hợp không tạo hồ sơ.

- [ ] **AC-08** [Idempotency]: Gửi yêu cầu không có header `Idempotency-Key` -> 400, không tạo hồ sơ.

- [ ] **AC-09** [Business Logic]: Tạo hồ sơ khi loại yêu cầu có SLA 48 giờ; sau đó Admin đổi SLA của loại này thành 24 giờ và SSO đổi số điện thoại của sinh viên -> `due_at` và snapshot của hồ sơ đã tạo giữ nguyên. SSO không trả số điện thoại lúc tạo -> snapshot lưu `null`.

- [ ] **AC-10** [Resilience]: Lấy snapshot khi phiên đã hết hạn -> 401; SSO trả payload thiếu MSSV -> 502; cả hai không tạo hồ sơ. Thông tin SSO còn trong cache 15 phút -> dùng cache, không gọi SSO mới. Lỗi cơ sở dữ liệu -> UI giữ nguyên nội dung form để sinh viên gửi lại.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-02/AC-01` đến `FR-STU-02/AC-10` trên Staging, kiểm bằng kịch bản `TS-22`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
