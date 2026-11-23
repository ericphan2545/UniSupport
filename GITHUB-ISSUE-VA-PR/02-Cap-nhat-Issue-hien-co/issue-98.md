**FR:** FR-MGT-04  
**Milestone:** MS-05 · Thống kê báo cáo cho Admin (kết thúc 11/12/2026)  
**Owner:** BE Dev / Mid (backend #99), FE Dev / Junior (frontend #100)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 07/12/2026  
**WBS:** 5.02, 5.04, 5.05, 5.06  
**Thời gian:** 30/11/2026 → 07/12/2026  

## Mô tả (theo PRD)

Thống kê khối lượng hồ sơ của từng phòng ban, giúp Admin phát hiện phòng ban quá tải để điều phối nhân sự.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-04](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-04-b%C3%A1o-c%C3%A1o-kh%E1%BB%91i-l%C6%B0%E1%BB%A3ng-c%C3%B4ng-vi%E1%BB%87c-theo-ph%C3%B2ng-ban)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-04/AC-xx`)

- [ ] **AC-01** [Happy Path]: Phòng A có 12 hồ sơ đang mở, trong đó 3 quá hạn; phòng B có 4 hồ sơ đang mở -> phòng A đứng trên phòng B, với số liệu đúng.

- [ ] **AC-02** [Validation]: Khoảng thời gian dài 366 ngày -> 400.

- [ ] **AC-03** [Boundary]: Phòng ban không có hồ sơ và không có nhân viên -> vẫn hiển thị, các chỉ số bằng 0.

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API -> 403.

- [ ] **AC-05** [Rate Limit]: Request thứ 31 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.
- [ ] **AC-07** [Business Logic]: Trong kỳ, phòng A có 5 hồ sơ tạo trong kỳ hiện thuộc A, 4 hồ sơ được đóng khi đang ở A, 3 tài khoản `STAFF` đang hoạt động và 1 tài khoản đã vô hiệu hóa -> bảng hiển thị đúng 5, 4 và số nhân viên 3. Cột hiển thị đúng tên "Hồ sơ tạo trong kỳ, hiện thuộc phòng ban". Hai phòng có cùng số hồ sơ đang mở -> phòng có mã nhỏ hơn đứng trước.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-04/AC-01` đến `FR-MGT-04/AC-07` trên Staging, kiểm bằng kịch bản `TS-44`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
