**FR:** FR-STU-04  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Senior (backend #20), FE Dev / Senior (frontend #21)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.01, 2.03, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Cho phép sinh viên đính kèm tệp minh chứng khi tạo yêu cầu, để nhân viên có đủ thông tin xử lý mà không phải hỏi lại.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-04](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-04-%C4%91%C3%ADnh-k%C3%A8m-t%E1%BB%87p-minh-ch%E1%BB%A9ng)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-04/AC-xx`)

- [ ] **AC-01** [Happy Path]: Gửi 2 tệp PDF 1MB -> 201; hai tệp gắn vào hồ sơ, hiển thị đúng tên gốc.

- [ ] **AC-02** [Boundary]: Tệp đúng 10MB -> chấp nhận; tệp 10MB + 1 byte -> 413. Gửi 5 tệp -> chấp nhận; gửi 6 tệp -> 400.

- [ ] **AC-03** [Validation]: Tệp `.exe` đổi đuôi thành `.pdf` -> 415.

- [ ] **AC-04** [Security]: Gọi API tải tệp không có token -> 401. Sinh viên khác gọi API tải tệp -> 404.

- [ ] **AC-05** [Idempotency]: Gửi lại request cùng `Idempotency-Key` -> tệp không bị lưu thêm lần nữa.

- [ ] **AC-06** [Resilience & Audit]: Lưu tệp thứ 3 bị lỗi -> rollback, không có hồ sơ, tệp không tham chiếu không tải được và được dọn theo quy ước chung. Lưu thành công -> nhật ký có bản ghi `UPLOAD_FILE` cho từng tệp.
- [ ] **AC-07** [Validation]: Gửi yêu cầu không kèm tệp -> 201. Gửi 3 tệp, trong đó 1 tệp sai định dạng -> 415, không tạo hồ sơ và không lưu tệp nào.

- [ ] **AC-08** [Security]: Tệp đã lưu có tên trong kho ngẫu nhiên, khác tên gốc; mở đường dẫn web công khai tới tệp -> không tải được. Giao diện vẫn hiển thị tên gốc.

- [ ] **AC-09** [Resilience]: Kho lưu trữ không phản hồi khi lưu tệp -> 503; không có hồ sơ, các tệp đã lưu của request bị xóa.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-04/AC-01` đến `FR-STU-04/AC-09` trên Staging, kiểm bằng kịch bản `TS-24`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
