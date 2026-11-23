**FR:** FR-CORE-02  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #13)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Định nghĩa 5 trạng thái của hồ sơ và các bước chuyển hợp lệ, để mọi module xử lý trạng thái thống nhất và hồ sơ luôn theo dõi được.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-02](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-02-tr%E1%BA%A1ng-th%C3%A1i-h%E1%BB%93-s%C6%A1-v%C3%A0-quy-t%E1%BA%AFc-chuy%E1%BB%83n-tr%E1%BA%A1ng-th%C3%A1i)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-02/AC-xx`)

- [ ] **AC-01** [Happy Path]: Một hồ sơ đi đủ luồng `NEW` -> `IN_PROGRESS` -> `PENDING_INFO` -> `IN_PROGRESS` -> `CLOSED` -> mỗi bước hợp lệ có history theo bảng gộp/tách CORE-06; không bắt một hành động chỉ có đúng một mốc khi có public/internal riêng.

- [ ] **AC-02** [Validation]: Gọi đóng hồ sơ đang `NEW` -> 409. Gọi yêu cầu bổ sung cho hồ sơ đang `PENDING_INFO` -> 409. Mọi thao tác thay đổi ticket CLOSED -> 409; tạo rating STU-10 và đánh dấu notification đã đọc vẫn hợp lệ, vì không sửa ticket.

- [ ] **AC-03** [Boundary]: Hồ sơ `IN_PROGRESS` có hạn lúc 10:00:00 -> lúc 09:59:59 vẫn `IN_PROGRESS`; muộn nhất 10:01:00 chuyển `OVERDUE`. Hồ sơ `PENDING_INFO` đã qua mốc hạn ban đầu -> vẫn `PENDING_INFO`.

- [ ] **AC-04** [Security]: Client gửi trực tiếp `status = CLOSED` trong body của một API khác -> hệ thống bỏ qua. Trạng thái chỉ đổi qua các sự kiện trong bảng.

- [ ] **AC-05** [Concurrency/Idempotency]: Job quá hạn và close dùng cùng version cạnh tranh -> một thay đổi commit, bên kia retry theo trạng thái/version mới; CLOSED không đổi lại. Close dùng version OVERDUE mới vẫn hợp lệ và tạo CLOSED, không coi chuỗi hợp lệ đó là race lỗi. Tác vụ chạy lại trên hồ sơ đã `OVERDUE` -> không đổi gì, không tạo thêm lịch sử hay thông báo.

- [ ] **AC-06** [Resilience & Audit]: Tác vụ quá hạn bị dừng giữa chừng -> lần chạy sau xử lý tiếp các hồ sơ còn lại. Mỗi lần hệ thống tự chuyển `OVERDUE` -> có bản ghi `STATUS_CHANGE` với người thực hiện là `SYSTEM`, và sinh viên nhận thông báo.
- [ ] **AC-07** [Business Logic]: Ghi chú (FR-STF-04) trên hồ sơ `IN_PROGRESS` -> `version` tăng 1, trạng thái không đổi. Đặt lại đúng mức ưu tiên hiện tại (no-op) -> `version` không đổi. Sinh viên đánh giá hoặc đọc thông báo -> `version` của hồ sơ không đổi. Thao tác gửi `version` cũ -> 409. Hồ sơ `IN_PROGRESS` đã qua `due_at` nhưng tác vụ quá hạn chưa chạy: yêu cầu bổ sung -> 409; đóng hồ sơ -> 200, hồ sơ `CLOSED` và không bị đổi sang `OVERDUE` sau đó.

- [ ] **AC-08** [Business Logic]: Hồ sơ `OVERDUE` được phân công lại, được yêu cầu bổ sung, nhận bổ sung của sinh viên -> vẫn `OVERDUE`; đóng hồ sơ -> `CLOSED`. Hồ sơ `IN_PROGRESS` được chuyển phòng, đổi mức ưu tiên, escalate, ghi chú -> trạng thái không đổi.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-02/AC-01` đến `FR-CORE-02/AC-08` trên Staging, kiểm bằng kịch bản `TS-55`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
