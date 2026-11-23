**FR:** FR-STU-10  
**Milestone:** MS-04 · Trả kết quả cho sinh viên (kết thúc 27/11/2026)  
**Owner:** BE Dev / Mid (backend #46), FE Dev / Junior (frontend #47)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 20/11/2026  
**WBS:** 4.02, 4.04, 4.05, 4.06  
**Thời gian:** 16/11/2026 → 20/11/2026  

## Mô tả (theo PRD)

Sau khi hồ sơ đóng, sinh viên chấm 1–5 sao kèm nhận xét, cung cấp dữ liệu hài lòng cho báo cáo của Admin (MGT-05).



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-10](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-10-%C4%91%C3%A1nh-gi%C3%A1-ch%E1%BA%A5t-l%C6%B0%E1%BB%A3ng-h%E1%BB%97-tr%E1%BB%A3)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-10/AC-xx`)

- [ ] **AC-01** [Happy Path]: Hồ sơ `CLOSED`, chọn 4 sao và nhập nhận xét -> 201; đánh giá hiển thị ở chế độ chỉ đọc và được tính vào MGT-05.

- [ ] **AC-02** [Validation/Boundary]: `rating` = 0, 6 hoặc 3.5 -> 400. Nhận xét 1000 ký tự -> 201; 1001 ký tự -> 400. Không nhập nhận xét -> 201.

- [ ] **AC-03** [Security]: Đánh giá hồ sơ của sinh viên khác -> 404. Nhận xét chứa `<script>` -> lưu dạng văn bản, khi hiển thị không thực thi.

- [ ] **AC-04** [Business Logic]: Đánh giá hồ sơ đang `IN_PROGRESS` -> 409.

- [ ] **AC-05** [Concurrency/Idempotency]: Bấm Gửi 2 lần liên tiếp -> chỉ lưu 1 đánh giá, lần thứ hai trả 409.

- [ ] **AC-06** [Resilience & Audit]: Lưu đánh giá lỗi -> 500, không có đánh giá dở dang, sinh viên gửi lại được. Lưu thành công -> nhật ký có bản ghi `RATE_TICKET`.
- [ ] **AC-07** [Business Logic]: Hồ sơ đóng 90 ngày trước, chưa đánh giá -> gửi đánh giá được (201). Đánh giá đã gửi hiển thị chỉ đọc, không có nút sửa. Trang hồ sơ đã đóng không có nút mở lại, chỉ có gợi ý tạo yêu cầu mới.

- [ ] **AC-08** [Resilience]: Đánh giá đã lưu nhưng mất phản hồi; sinh viên gửi lại và nhận 409 -> UI tải lại và hiển thị đánh giá đã lưu, không báo lỗi chung.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-10/AC-01` đến `FR-STU-10/AC-08` trên Staging, kiểm bằng kịch bản `TS-30`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
