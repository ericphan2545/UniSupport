**FR:** FR-STF-03  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #66), FE Dev / Junior (frontend #67)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.04, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Nhân viên xem trực tiếp hoặc tải tệp minh chứng của hồ sơ để kiểm tra khi xử lý.



**Đặc tả đầy đủ:** [docs_v2/modules/M02-staff-portal/prd.md – FR-STF-03](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M02-staff-portal/prd.md#fr-stf-03-xem-v%C3%A0-t%E1%BA%A3i-t%E1%BB%87p-%C4%91%C3%ADnh-k%C3%A8m)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STF-03/AC-xx`)

- [ ] **AC-01** [Happy Path]: Mở một tệp PDF -> hiển thị trên trình duyệt với đúng tên gốc.

- [ ] **AC-02** [Validation]: Gọi API với `file_id` không tồn tại -> 404.

- [ ] **AC-03** [Security]: Nhân viên phòng ban khác gọi trực tiếp API tải tệp -> 404. Gọi API không có token -> 401.

- [ ] **AC-04** [Rate Limit]: Lần tải thứ 31 trong 1 phút -> 429.

- [ ] **AC-05** [Boundary]: Tải tệp 10MB -> tải trọn vẹn, đúng dung lượng.

- [ ] **AC-06** [Resilience & Audit]: Kho lưu trữ không phản hồi -> 503, ứng dụng không treo. Mỗi lần tải thành công -> có một bản ghi `DOWNLOAD_FILE`.
- [ ] **AC-07** [Validation]: Tải tệp PNG và tệp PDF -> header `Content-Type` lần lượt là `image/png` và `application/pdf`; tên tệp tải về là tên gốc.

- [ ] **AC-08** [Resilience & Audit]: Tệp có metadata trong hồ sơ nhưng object trong kho đã mất -> 404 với thông báo chung; nhật ký có bản ghi khởi tạo tải và `FILE_READ_RESULT` = `OBJECT_MISSING`; log kỹ thuật không chứa đường dẫn kho hay token.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STF-03/AC-01` đến `FR-STF-03/AC-08` trên Staging, kiểm bằng kịch bản `TS-33`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
