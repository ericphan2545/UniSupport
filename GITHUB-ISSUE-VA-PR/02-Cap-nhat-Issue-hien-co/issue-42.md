**FR:** FR-STU-09  
**Milestone:** MS-03 · Phân công, xử lý, chuyển phòng ban (kết thúc 13/11/2026)  
**Owner:** BE Dev / Mid (backend #43), FE Dev / Junior (frontend #44)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 05/11/2026  
**WBS:** 3.02, 3.04, 3.05, 3.06, 3.07  
**Thời gian:** 26/10/2026 → 05/11/2026  

## Mô tả (theo PRD)

Sinh viên gửi nội dung và tệp bổ sung khi nhân viên yêu cầu, để hồ sơ tiếp tục được xử lý mà không phải tạo yêu cầu mới.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-09](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-09-b%E1%BB%95-sung-th%C3%B4ng-tin-khi-%C4%91%C6%B0%E1%BB%A3c-y%C3%AAu-c%E1%BA%A7u)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-09/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `PENDING_INFO`, gửi nội dung hợp lệ kèm 1 tệp -> 201; hồ sơ chuyển `IN_PROGRESS`; yêu cầu bổ sung được đóng; SLA tiếp tục tính.

- [ ] **AC-02** [Boundary]: Hồ sơ `OVERDUE` có yêu cầu bổ sung đang mở, gửi bổ sung -> 201; trạng thái vẫn `OVERDUE`; yêu cầu bổ sung được đóng.

- [ ] **AC-03** [Validation]: Nội dung 9 ký tự -> 400. Gửi 6 tệp -> 400.

- [ ] **AC-04** [Security]: Bổ sung cho hồ sơ của sinh viên khác -> 404.

- [ ] **AC-05** [Concurrency/Idempotency]: Sinh viên bổ sung và nhân viên đóng dùng cùng version đã đọc -> một thao tác commit, bên kia 409; nếu hồ sơ đóng trước thì không lưu bổ sung. Bên đóng tải version mới sau bổ sung vẫn có thể đóng. Bấm Gửi 2 lần cùng `Idempotency-Key` -> chỉ lưu 1 lần bổ sung.

- [ ] **AC-06** [Resilience & Audit]: Lưu tệp lỗi -> rollback, trạng thái và yêu cầu bổ sung giữ nguyên. Thành công -> nhật ký có bản ghi `SUPPLEMENT_SUBMITTED`, lịch sử hồ sơ có mốc bổ sung.
- [ ] **AC-07** [Validation]: Thiếu `version` hoặc `info_request_id` -> 400. `info_request_id` không phải yêu cầu đang mở, hoặc `version` cũ -> 409. Thiếu header `Idempotency-Key` -> 400.

- [ ] **AC-08** [Business Logic]: Yêu cầu bổ sung đã được trả lời, gửi bổ sung lần nữa khi không còn yêu cầu mở -> 409. Hồ sơ đã `CLOSED` -> 409 "Hồ sơ đã đóng". Bổ sung thành công -> `version` tăng đúng 1.

- [ ] **AC-09** [Idempotency]: Gửi lại cùng `Idempotency-Key` nhưng nội dung khác -> 409. Gửi lại cùng key, cùng nội dung sau khi bổ sung đã thành công -> trả kết quả cũ, không báo lỗi version.

- [ ] **AC-10** [Resilience]: Lỗi cơ sở dữ liệu khi lưu bổ sung -> 500; hồ sơ giữ trạng thái cũ, yêu cầu bổ sung vẫn mở. Tệp `.exe` đổi đuôi `.pdf` -> 415.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-09/AC-01` đến `FR-STU-09/AC-10` trên Staging, kiểm bằng kịch bản `TS-29`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
