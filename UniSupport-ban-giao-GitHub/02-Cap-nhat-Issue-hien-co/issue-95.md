**FR:** FR-MGT-03  
**Milestone:** MS-05 · Thống kê báo cáo cho Admin (kết thúc 11/12/2026)  
**Owner:** BE Dev / Mid (backend #96), FE Dev / Junior (frontend #97)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 07/12/2026  
**WBS:** 5.02, 5.04, 5.05, 5.06  
**Thời gian:** 30/11/2026 → 07/12/2026  

## Mô tả (theo PRD)

Thống kê số hồ sơ theo từng loại yêu cầu, giúp Admin biết sinh viên thường gặp vấn đề gì để cải thiện quy trình.



**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-03](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-03-b%C3%A1o-c%C3%A1o-lo%E1%BA%A1i-y%C3%AAu-c%E1%BA%A7u-ph%E1%BB%95-bi%E1%BA%BFn)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-03/AC-xx`)

- [ ] **AC-01** [Happy Path]: Trong kỳ có 60 hồ sơ "Học phí" và 40 hồ sơ "Xác nhận sinh viên" (ví dụ) -> bảng hiển thị Học phí 60 (60,0%) ở trên, Xác nhận sinh viên 40 (40,0%) ở dưới.

- [ ] **AC-02** [Validation]: `from` sau `to` -> 400.

- [ ] **AC-03** [Boundary]: Không có hồ sơ trong kỳ -> mọi loại hiển thị 0, tỷ lệ `--`, không lỗi chia cho 0.

- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API -> 403.

- [ ] **AC-05** [Rate Limit]: Request thứ 31 trong 1 phút -> 429.

- [ ] **AC-06** [Resilience]: Truy vấn quá 10 giây -> 503, UI hiển thị nút Thử lại.
- [ ] **AC-07** [Business Logic]: Hai loại cùng 30 hồ sơ -> loại có mã nhỏ hơn đứng trước. Ba loại, mỗi loại 1 hồ sơ -> mỗi loại 33,3%; không chỉnh số liệu để ép tổng thành 100%. Hồ sơ ở mọi trạng thái, kể cả `CLOSED`, đều được đếm theo ngày gửi.

- [ ] **AC-08** [Validation]: Lọc theo phòng B -> đếm hồ sơ hiện thuộc phòng B, kể cả hồ sơ được chuyển từ phòng A sang. Người dùng chọn EN -> tên loại yêu cầu hiển thị bằng tiếng Anh.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-03/AC-01` đến `FR-MGT-03/AC-08` trên Staging, kiểm bằng kịch bản `TS-43`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
