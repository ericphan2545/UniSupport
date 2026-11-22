**FR:** FR-CORE-05  
**Milestone:** MS-02 · Gửi và tiếp nhận yêu cầu (kết thúc 23/10/2026)  
**Owner:** BE Dev / Mid (backend #23)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 16/10/2026  
**WBS:** 2.02, 2.05, 2.06, 2.07  
**Thời gian:** 28/09/2026 → 16/10/2026  

## Mô tả (theo PRD)

Hệ thống có sẵn danh mục loại yêu cầu và phòng ban, trong đó mỗi loại yêu cầu được ánh xạ tới một phòng ban tiếp nhận, để hồ sơ tự đến đúng nơi.



**Đặc tả đầy đủ:** [docs_v2/modules/M05-core-platform/prd.md – FR-CORE-05](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M05-core-platform/prd.md#fr-core-05-danh-m%E1%BB%A5c-lo%E1%BA%A1i-y%C3%AAu-c%E1%BA%A7u-v%C3%A0-ph%C3%B2ng-ban)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-CORE-05/AC-xx`)

- [ ] **AC-01** [Happy Path]: Sau khi triển khai, API danh mục trả về đầy đủ loại yêu cầu kèm phòng ban tiếp nhận và tên theo ngôn ngữ người dùng.

- [ ] **AC-02** [Validation]: Dữ liệu nạp có một loại yêu cầu không có phòng ban -> nạp thất bại, ứng dụng không khởi động, log ghi rõ loại yêu cầu bị lỗi.

- [ ] **AC-03** [Boundary]: Một phòng ban nhận 3 loại yêu cầu -> cả 3 loại đều gán đúng về phòng ban đó.

- [ ] **AC-04** [Security]: Gọi API thêm, sửa hoặc xóa danh mục với bất kỳ vai trò nào -> không tồn tại API này (404).

- [ ] **AC-05** [Concurrency]: Nhiều người dùng tải danh mục cùng lúc -> dữ liệu được trả từ cache, không truy vấn cơ sở dữ liệu cho mỗi request.

- [ ] **AC-06** [Resilience]: Seed cố xóa loại có ticket -> lỗi, giữ dữ liệu cũ. Seed lại danh mục hợp lệ không ghi đè SLA đã đổi ở MGT-07; EN thiếu dùng VI có log, không crash.
- [ ] **AC-07** [Resilience]: Dữ liệu nạp có loại yêu cầu ánh xạ tới mã phòng ban không tồn tại -> nạp thất bại, ứng dụng không khởi động. Chạy seed hợp lệ 2 lần -> không tạo bản trùng. Lần nạp đầu lỗi và chưa có bộ danh mục hợp lệ -> ứng dụng không phục vụ nghiệp vụ.

- [ ] **AC-08** [Business Logic]: Triển khai lại với danh mục mới -> API danh mục trả bộ mới (cache được làm mới). Admin sửa được SLA qua FR-MGT-07 nhưng không sửa được tên, mã hay ánh xạ phòng ban. Bộ dữ liệu nạp cho production theo danh sách trường xác nhận (`A-10`), không chứa tên ví dụ dùng trong AC.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-CORE-05/AC-01` đến `FR-CORE-05/AC-08` trên Staging, kiểm bằng kịch bản `TS-58`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
