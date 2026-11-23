**FR:** FR-CORE-03  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #15)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Hệ thống tính hạn xử lý của từng hồ sơ theo SLA của loại yêu cầu, có tạm dừng khi chờ sinh viên bổ sung, làm căn cứ xác định quá hạn.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-03](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-03-t%C3%ADnh-h%E1%BA%A1n-x%E1%BB%AD-l%C3%BD-sla)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-03/AC-xx`)

- [ ] **AC-01** [Happy Path]: Loại yêu cầu có SLA 48 giờ, hồ sơ tạo lúc 08:00 ngày 01/10 -> `due_at` = 08:00 ngày 03/10.

- [ ] **AC-02** [Business Logic]: Hồ sơ trên chờ bổ sung từ 10:00 đến 16:00 ngày 01/10 -> `due_at` mới = 14:00 ngày 03/10.

- [ ] **AC-03** [Boundary]: Hồ sơ tạm dừng 2 lần, mỗi lần 3 giờ -> `due_at` bị lùi tổng cộng 6 giờ. SLA đổi từ 48 thành 24 giờ sau khi hồ sơ đã tạo -> `due_at` của hồ sơ đó không đổi.

- [ ] **AC-04** [Security]: Client gửi `due_at` trong body bất kỳ API nào -> hệ thống bỏ qua.

- [ ] **AC-05** [Concurrency]: Sinh viên bổ sung đúng lúc tác vụ quá hạn đang chạy -> hồ sơ không bị chuyển `OVERDUE` dựa trên `due_at` cũ khi chưa được cộng bù.

- [ ] **AC-06** [Resilience & Audit]: Lỗi khi cộng bù thời gian -> rollback, hồ sơ vẫn ở `PENDING_INFO`, `due_at` giữ nguyên. Hạn xử lý ban đầu và mỗi lần cộng bù đều được lưu trong lịch sử hồ sơ (FR-CORE-06).
- [ ] **AC-07** [Business Logic]: Hồ sơ `IN_PROGRESS` có `due_at` = 14:00; lúc 10:00 chuyển `PENDING_INFO` -> thời gian còn lại giữ 4 giờ trong suốt lúc chờ. Hồ sơ `CLOSED` không còn đếm ngược. Nhân viên thấy cờ tạm dừng; response cho sinh viên không có `due_at`.

- [ ] **AC-08** [Business Logic]: Hồ sơ `OVERDUE` có yêu cầu bổ sung mở trong 4 giờ -> không tạm dừng, `due_at` không được cộng bù. Chuyển phòng, phân công lại, đổi mức ưu tiên, escalate -> `due_at` không đổi.

- [ ] **AC-09** [Boundary]: Hồ sơ gửi lúc 08:00, chờ bổ sung từ 10:00 và được đóng lúc 12:00 khi vẫn đang chờ -> khoảng tạm dừng tính đến 12:00; thời gian xử lý thực tế = 2 giờ. Loại yêu cầu chưa có SLA khi tạo hồ sơ -> 422, không tạo hồ sơ.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-03/AC-01` đến `FR-CORE-03/AC-09` trên Staging, kiểm bằng kịch bản `TS-56`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
