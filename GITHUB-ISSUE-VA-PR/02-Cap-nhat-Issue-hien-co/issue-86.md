**FR:** FR-MGT-01  
**Milestone:** MS-05 · Thống kê báo cáo cho Admin (kết thúc 11/12/2026)  
**Owner:** BE Dev / Senior (backend #87), FE Dev / Senior (frontend #88)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 07/12/2026  
**WBS:** 5.01, 5.03, 5.05, 5.06  
**Thời gian:** 30/11/2026 → 07/12/2026  

## Mô tả (theo PRD)

Hiển thị số lượng hồ sơ theo trạng thái tại thời điểm hiện tại, giúp Admin biết ngay khối lượng đang chờ và số hồ sơ đang trễ hạn.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-01](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-01-dashboard-s%E1%BB%91-h%E1%BB%93-s%C6%A1-m%E1%BB%9Bi-%C4%91ang-x%E1%BB%AD-l%C3%BD-qu%C3%A1-h%E1%BA%A1n)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-01/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hệ thống có 5 hồ sơ `NEW`, 8 `IN_PROGRESS`, 2 `PENDING_INFO`, 3 `OVERDUE`, 20 `CLOSED` -> Dashboard hiển thị Mới 5, Đang xử lý 10, Quá hạn 3.

- [ ] **AC-02** [Validation]: Lọc theo một phòng ban -> chỉ đếm hồ sơ hiện đang thuộc phòng ban đó. `department_id` không tồn tại -> 400.

- [ ] **AC-03** [Boundary]: Hệ thống chưa có hồ sơ nào -> cả 3 chỉ số hiển thị 0, không lỗi.

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API dashboard -> 403.

- [ ] **AC-05** [Concurrency/Rate Limit]: Một hồ sơ chuyển sang `OVERDUE` trong lúc dashboard đang mở -> số Quá hạn tăng trong vòng 30 giây. Request thứ 61 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Truy vấn quá 10 giây -> 503; UI vẫn hiển thị số liệu lần trước kèm cảnh báo, không trắng trang.
- [ ] **AC-07** [Business Logic]: Bấm chỉ số "Quá hạn" (3 hồ sơ) -> danh sách 3 hồ sơ có mã, loại, phòng ban, trạng thái, ngày gửi; sắp `created_at` giảm dần, cùng `created_at` thì `ticket_id` giảm dần; `total_items` = 3, `total_pages` = 1; không có nút xử lý hay liên kết sang chi tiết hồ sơ. Giá trị nhóm chỉ số không hợp lệ -> 400.

- [ ] **AC-08** [Boundary]: Hồ sơ `NEW` tạo từ 400 ngày trước vẫn được đếm vào "Mới", vì số liệu là ảnh chụp hiện tại, không lọc theo thời gian. Dashboard hiển thị thời điểm cập nhật gần nhất, đổi sau mỗi lần polling.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-01/AC-01` đến `FR-MGT-01/AC-08` trên Staging, kiểm bằng kịch bản `TS-41`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
