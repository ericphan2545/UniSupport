**FR:** FR-STU-01  
**Milestone:** MS-01 · Chốt yêu cầu, đăng nhập và phân quyền (kết thúc 25/09/2026)  
**Owner:** BE Dev / Mid (backend #31), FE Dev / Junior (frontend #32)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 23/09/2026  
**WBS:** 1.21, 1.23, 1.24, 1.25  
**Thời gian:** 21/09/2026 → 23/09/2026  

## Mô tả (theo PRD)

Cho phép sinh viên xem thông tin cá nhân (họ tên, MSSV, khoa/lớp, email, SĐT) đồng bộ từ SSO. MSSV và Họ tên phải có; các trường tùy chọn thiếu được chuẩn hóa null và hiển thị --. Thiếu trường bắt buộc trả lỗi nguồn SSO, không hiển thị dữ liệu giả.



**Đặc tả đầy đủ:** [docs_v2/modules/M01-student-portal/prd.md – FR-STU-01](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M01-student-portal/prd.md#fr-stu-01-xem-th%C3%B4ng-tin-c%C3%A1-nh%C3%A2n-l%E1%BA%A5y-t%E1%BB%AB-sso)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-STU-01/AC-xx`)

- [ ] **AC-01** [Happy Path]: SSO trả về đủ 6 trường -> 200, hiển thị đúng họ tên, MSSV, khoa, lớp, email, SĐT.

- [ ] **AC-02** [Validation]: SSO chỉ trả về MSSV, Họ tên, Email -> 200; `phone_number`, `faculty_name`, `class_name` = `null`; UI hiển thị `--`.

- [ ] **AC-03** [Boundary]: SSO trả về SĐT là `""` hoặc `"   "` -> hệ thống trim và chuyển thành `null`.

- [ ] **AC-04** [Security]: Request thiếu cookie phiên ứng dụng, cookie sai chữ ký hoặc phiên hết hạn/đã thu hồi -> 401. Cookie hợp lệ không cần header Authorization.

- [ ] **AC-05** [Concurrency/Cache]: Sinh viên tải lại trang 3 lần trong 1 phút -> lần 2 và lần 3 lấy dữ liệu từ Redis, không gọi lại SSO.

- [ ] **AC-06** [Resilience]: Cache hết hạn và SSO không phản hồi quá 3 giây -> 504, không treo. SSO thiếu MSSV/Họ tên hoặc chuỗi rỗng -> 502. Cookie hợp lệ không có Authorization vẫn đọc được thông tin.
- [ ] **AC-07** [Security/Cache]: Sinh viên A tải thông tin (dữ liệu đã vào cache), ngay sau đó sinh viên B tải thông tin -> B nhận đúng dữ liệu của B, không nhận dữ liệu trong cache của A. Sau 15 phút kể từ lần gọi SSO trước, A tải lại -> hệ thống gọi lại SSO.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-STU-01/AC-01` đến `FR-STU-01/AC-07` trên Staging, kiểm bằng kịch bản `TS-21`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
