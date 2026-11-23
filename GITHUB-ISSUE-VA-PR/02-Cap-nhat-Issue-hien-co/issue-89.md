**FR:** FR-MGT-02  
**Milestone:** MS-05 · Thống kê báo cáo cho Admin (kết thúc 11/12/2026)  
**Owner:** BE Dev / Senior (backend #90), FE Dev / Senior (frontend #91)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 07/12/2026  
**WBS:** 5.01, 5.03, 5.05, 5.06  
**Thời gian:** 30/11/2026 → 07/12/2026  

## Mô tả (theo PRD)

Hiển thị thời gian xử lý trung bình của các hồ sơ đã đóng, giúp Admin đánh giá tốc độ phục vụ của từng phòng ban.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-02](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-02-dashboard-th%E1%BB%9Di-gian-x%E1%BB%AD-l%C3%BD-trung-b%C3%ACnh)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-02/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hai hồ sơ đóng trong kỳ, mất 10 giờ và 20 giờ -> hiển thị 15,0 giờ, dựa trên 2 hồ sơ.

- [ ] **AC-02** [Business Logic]: Hồ sơ gửi lúc 08:00, chờ bổ sung từ 10:00 đến 14:00, đóng lúc 18:00 -> thời gian xử lý được tính là 6 giờ.

- [ ] **AC-03** [Validation/Boundary]: Khoảng thời gian 366 ngày -> 400. Đúng 365 ngày -> 200. Không có hồ sơ đóng trong kỳ -> 200, giá trị `null`, UI hiển thị "Chưa có dữ liệu" (không hiển thị 0).

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API -> 403.

- [ ] **AC-05** [Rate Limit]: Request thứ 61 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Truy vấn quá 10 giây -> 503 với thông báo thử lại, không làm treo dashboard.
- [ ] **AC-07** [Business Logic]: Hồ sơ `OVERDUE` có yêu cầu bổ sung từ 10:00 đến 14:00 -> 4 giờ chờ này vẫn được tính vào thời gian xử lý. Hồ sơ đóng khi đã qua `due_at` nhưng tác vụ quá hạn chưa chạy -> thời gian xử lý tính từ `created_at` và `closed_at` như hồ sơ khác. Hồ sơ đóng ngoài khoảng thời gian -> không được tính.

- [ ] **AC-08** [Validation/Boundary]: Trung bình 50,26 giờ -> hiển thị 50,3 giờ kèm số ngày tương ứng. `from` sau `to` -> 400. Hồ sơ chuyển từ phòng A sang B rồi đóng ở B -> lọc phòng B có tính hồ sơ này, lọc phòng A không tính. Lọc theo loại yêu cầu -> chỉ tính hồ sơ thuộc loại đó. Dashboard hiển thị thời điểm cập nhật gần nhất.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-02/AC-01` đến `FR-MGT-02/AC-08` trên Staging, kiểm bằng kịch bản `TS-42`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
