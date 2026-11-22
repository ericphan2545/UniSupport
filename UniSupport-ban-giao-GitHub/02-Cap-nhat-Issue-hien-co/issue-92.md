**FR:** FR-MGT-05  
**Milestone:** MS-05 · Thống kê báo cáo cho Admin (kết thúc 11/12/2026)  
**Owner:** BE Dev / Senior (backend #93), FE Dev / Senior (frontend #94)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 07/12/2026  
**WBS:** 5.01, 5.03, 5.05, 5.06  
**Thời gian:** 30/11/2026 → 07/12/2026  

## Mô tả (theo PRD)

Thống kê điểm đánh giá và nhận xét của sinh viên, giúp Admin đánh giá chất lượng dịch vụ hỗ trợ.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-05](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-05-b%C3%A1o-c%C3%A1o-m%E1%BB%A9c-%C4%91%E1%BB%99-h%C3%A0i-l%C3%B2ng-c%E1%BB%A7a-sinh-vi%C3%AAn)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-05/AC-xx`)

- [ ] **AC-01** [Happy Path]: Trong kỳ có 10 hồ sơ đóng, 4 hồ sơ được đánh giá 5, 4, 4, 3 sao -> điểm trung bình 4,00; tổng 4 lượt; tỷ lệ đánh giá 40%.

- [ ] **AC-02** [Validation]: Lọc theo số sao = 6 -> 400.

- [ ] **AC-03** [Boundary]: Không có đánh giá trong kỳ -> điểm trung bình hiển thị "Chưa có đánh giá" (không hiển thị 0), không lỗi chia cho 0.

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API -> 403. Response không chứa tên hoặc MSSV của sinh viên. Nhận xét chứa `<script>` -> hiển thị dạng văn bản, không thực thi.

- [ ] **AC-05** [Rate Limit]: Request thứ 31 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.
- [ ] **AC-07** [Business Logic]: Với dữ liệu của AC-01, lọc 4 sao -> danh sách nhận xét chỉ còn các lượt đánh giá 4 sao; điểm trung bình, tổng lượt và tỷ lệ đánh giá không đổi; phân bố hiển thị 5 sao: 1, 4 sao: 2, 3 sao: 1, 2 sao: 0, 1 sao: 0. Danh sách nhận xét có mã hồ sơ, loại, phòng ban, số sao, nhận xét, ngày đánh giá; mới nhất trước; 20 dòng mỗi trang.

- [ ] **AC-08** [Boundary]: Kỳ không có hồ sơ `CLOSED` -> tỷ lệ đánh giá `null`, hiển thị `--`. Hồ sơ chuyển từ phòng A sang B rồi đóng ở B -> lọc phòng B có tính đánh giá của hồ sơ này.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-05/AC-01` đến `FR-MGT-05/AC-08` trên Staging, kiểm bằng kịch bản `TS-45`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
