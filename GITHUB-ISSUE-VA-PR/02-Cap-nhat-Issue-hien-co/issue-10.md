**FR:** FR-SEC-03  
**Milestone:** MS-01 · Chốt yêu cầu, đăng nhập và phân quyền (kết thúc 25/09/2026)  
**Owner:** BE Dev / Senior (backend #11)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 23/09/2026  
**WBS:** 1.20, 1.24, 1.25  
**Thời gian:** 21/09/2026 → 23/09/2026  

## Mô tả (theo PRD)

Hệ thống giới hạn chức năng mà mỗi người dùng được dùng theo vai trò, để mỗi nhóm chỉ thao tác được trong phạm vi của mình.



**Đặc tả đầy đủ:** [docs_v2/modules/M04-authentication-security/prd.md – FR-SEC-03](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M04-authentication-security/prd.md#fr-sec-03-ph%C3%A2n-quy%E1%BB%81n-theo-3-vai-tr%C3%B2)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-SEC-03/AC-xx`)

- [ ] **AC-01** [Happy Path]: Sinh viên gọi API tạo yêu cầu -> được phép. Nhân viên gọi API hàng đợi -> được phép. Admin gọi API dashboard -> được phép.

- [ ] **AC-02** [Validation]: Sinh viên gọi API hàng đợi của Cổng Nhân viên -> 403. Nhân viên gọi API phân quyền -> 403.

- [ ] **AC-03** [Boundary]: Một API mới được thêm mà quên khai báo vai trò -> mọi vai trò đều nhận 403.

- [ ] **AC-04** [Security]: Client tự sửa giao diện để hiện nút của vai trò khác rồi gọi API -> backend vẫn trả 403.

- [ ] **AC-05** [Concurrency]: Admin hạ một nhân viên về `STUDENT` trong lúc người đó đang mở Cổng Nhân viên -> request kế tiếp của họ nhận 403.

- [ ] **AC-06** [Resilience & Audit]: Cơ sở dữ liệu phân quyền không phản hồi -> 503, request không được xử lý tiếp (không mặc định cho phép). Mỗi lần thay đổi vai trò -> có bản ghi `ROLE_CHANGE` (theo FR-MGT-06).
- [ ] **AC-07** [Security]: Request ghi dùng cookie nhưng thiếu CSRF token, hoặc `Origin` không thuộc ứng dụng -> 403, dữ liệu không đổi. Gọi API nghiệp vụ không có phiên -> 401.

- [ ] **AC-08** [Business Logic]: Admin vô hiệu hóa một tài khoản đang có phiên còn hạn -> request kế tiếp của tài khoản đó nhận 403 và phiên bị thu hồi. Người dùng đăng nhập SSO lần đầu -> vai trò `STUDENT`, giao diện chỉ có menu của Cổng Sinh viên.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-SEC-03/AC-01` đến `FR-SEC-03/AC-08` trên Staging, kiểm bằng kịch bản `TS-50`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
