**FR:** FR-MGT-06  
**Milestone:** MS-01 · Chốt yêu cầu, đăng nhập và phân quyền (kết thúc 25/09/2026)  
**Owner:** BE Dev / Senior (backend #81), FE Dev / Senior (frontend #82)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 23/09/2026  
**WBS:** 1.20, 1.22, 1.24, 1.25  
**Thời gian:** 21/09/2026 → 23/09/2026  

## Mô tả (theo PRD)
Admin gán vai trò và phòng ban cho từng tài khoản, để hệ thống biết ai là nhân viên phòng nào và ai thuộc Admin.

**Đặc tả đầy đủ:** [docs_v2/modules/M03-management-portal/prd.md – FR-MGT-06](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M03-management-portal/prd.md#fr-mgt-06-qu%E1%BA%A3n-l%C3%BD-ph%C3%A2n-quy%E1%BB%81n)

## Acceptance Criteria (theo PRD, mã đầy đủ `FR-MGT-06/AC-xx`)
- [ ] **AC-01** [Happy Path]: Thêm tài khoản `nva@aurora.edu.vn` (ví dụ) với vai trò `STAFF` thuộc Phòng Đào tạo -> 201. Khi người này đăng nhập SSO, họ vào Cổng Nhân viên và thấy hàng đợi của Phòng Đào tạo.
- [ ] **AC-02** [Validation]: Tạo tài khoản `STAFF` không có phòng ban -> 400. Thêm mới định danh đã có -> 409; tìm email STUDENT có sẵn rồi cấp STAFF/phòng bằng account_id/version -> 200, không tạo duplicate account.
- [ ] **AC-03** [Business Logic]: Đổi phòng ban của nhân viên đang phụ trách 2 hồ sơ chưa đóng -> 409 kèm 2 mã hồ sơ. Sau khi 2 hồ sơ được phân công lại cho người khác, đổi phòng ban -> 200.
- [ ] **AC-04** [Security]: Tài khoản `STAFF` gọi API phân quyền -> 403. Admin tự hạ vai trò của chính mình -> 409. Nhân viên bị hạ về `STUDENT` -> request kế tiếp vào Cổng Nhân viên bị 403.
- [ ] **AC-05** [Concurrency]: Hai người trong Admin cùng sửa một tài khoản -> người lưu sau nhận 409. Hai người cạnh tranh thao tác khiến không còn ADMIN active -> chỉ thao tác giữ invariant được commit; tự deactivate/hạ quyền luôn 409. Phân công staff cạnh tranh deactivate/đổi phòng không để assignee ticket mở mất quyền.
- [ ] **AC-06** [Resilience & Audit]: Lưu lỗi -> rollback, phân quyền giữ nguyên. Thành công -> có bản ghi nhật ký `ROLE_CHANGE` (người thực hiện, tài khoản bị đổi, giá trị cũ, giá trị mới).
- [ ] **AC-07** [Validation]: Định danh `abc` (sai định dạng email) -> 400. Thiếu `version` khi sửa -> 400. Cấp vai trò `ADMIN` -> tài khoản không có phòng ban. Tìm `nva@AURORA.edu.vn ` (có khoảng trắng, miền viết hoa) -> tìm thấy tài khoản `nva@aurora.edu.vn`; kết quả chỉ có id, email, tên, vai trò, trạng thái hoạt động, version; không có MSSV, khoa/lớp, SĐT hay hồ sơ của sinh viên.
- [ ] **AC-08** [Business Logic]: Vô hiệu hóa nhân viên C (không còn hồ sơ mở) -> C đăng nhập SSO bị từ chối và không có trong danh sách người nhận phân công. Người dùng đăng nhập SSO lần đầu, không có trong danh sách phân quyền -> tài khoản `STUDENT`. Hai tài khoản có định danh khác nhau không bị tự gộp. Nhật ký `ROLE_CHANGE` ghi đúng subtype (`ACCOUNT_ADD`, `ROLE_SET`, `DEPARTMENT_SET`, `ACTIVATE`, `DEACTIVATE`).
- [ ] **AC-09** [Security/Rate Limit]: Sau triển khai, tài khoản `ADMIN` ban đầu là định danh do trường xác nhận; không có tài khoản Admin dùng email ví dụ. Thao tác ghi thứ 31 trong 1 phút -> 429.

## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-MGT-06/AC-01` đến `FR-MGT-06/AC-09` trên Staging, kiểm bằng kịch bản `TS-46`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
