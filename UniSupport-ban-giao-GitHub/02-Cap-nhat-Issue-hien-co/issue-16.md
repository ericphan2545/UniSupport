**FR:** FR-STU-08  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #17), FE Dev / Junior (frontend #18)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Hệ thống thông báo trong app cho sinh viên khi trạng thái hồ sơ thay đổi hoặc khi có yêu cầu bổ sung, để sinh viên không phải tự vào kiểm tra.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-08](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-08-nh%E1%BA%ADn-th%C3%B4ng-b%C3%A1o-trong-app)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-08/AC-xx`)

- [ ] **AC-01** [Happy Path]: Nhân viên chuyển hồ sơ sang `PENDING_INFO` -> sinh viên có 1 thông báo chưa đọc, số trên chuông tăng 1 trong vòng 30 giây.

- [ ] **AC-02** [Validation]: Sinh viên bổ sung xong và hồ sơ về `IN_PROGRESS` -> không có thông báo mới cho sinh viên.

- [ ] **AC-03** [Boundary]: Có 120 thông báo chưa đọc -> chuông hiển thị "99+".

- [ ] **AC-04** [Security]: Sinh viên A đánh dấu đọc thông báo của sinh viên B -> 404.

- [ ] **AC-05** [Idempotency]: Đánh dấu đã đọc cùng một thông báo 2 lần -> cả hai lần đều 200, kết quả như nhau. Một event REQUEST_INFO đồng thời đổi PENDING_INFO, hoặc xử lý lại event -> một thông báo; read-all tại snapshot không đánh dấu thông báo tạo sau snapshot.

- [ ] **AC-06** [Resilience]: Thao tác đổi trạng thái bị rollback -> không có thông báo nào được tạo.
- [ ] **AC-07** [Validation/Boundary]: Không truyền `page_size` -> 20 thông báo, mới nhất trước. `page_size = 100` -> 200; `page_size = 101` hoặc `page = 0` -> 400. Đánh dấu đã đọc một thông báo -> chỉ thông báo đó đổi trạng thái.

- [ ] **AC-08** [Business Logic]: Hồ sơ chuyển `OVERDUE` -> sinh viên có 1 thông báo. Nhân viên tạo yêu cầu bổ sung cho hồ sơ đang `OVERDUE` (trạng thái không đổi) -> sinh viên có 1 thông báo. Nhân viên đóng hồ sơ -> sinh viên có 1 thông báo.

- [ ] **AC-09** [Validation]: Cùng một thông báo, người dùng đổi ngôn ngữ từ VN sang EN -> nội dung hiển thị bằng tiếng Anh; dữ liệu lưu là `event_type` kèm tham số, không lưu câu đã dịch.

- [ ] **AC-10** [Resilience]: API số chưa đọc lỗi -> chuông giữ số cũ, lần polling sau gọi lại.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-08/AC-01` đến `FR-STU-08/AC-10` trên Staging, kiểm bằng kịch bản `TS-28`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
