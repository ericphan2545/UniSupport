**FR:** FR-SEC-02  
**Milestone:** MS-01 · Chốt yêu cầu, đăng nhập và phân quyền (kết thúc 25/09/2026)  
**Owner:** BE Dev / Mid (backend #8), FE Dev / Senior (frontend #9)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 23/09/2026  
**WBS:** 1.21, 1.22, 1.24, 1.25  
**Thời gian:** 21/09/2026 → 23/09/2026  

## Mô tả (theo PRD)

Người dùng kết thúc phiên làm việc để người khác dùng chung thiết bị không truy cập được vào tài khoản.



**Đặc tả đầy đủ:** [docs_v2/modules/M04-authentication-security/prd.md – FR-SEC-02](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M04-authentication-security/prd.md#fr-sec-02-%C4%91%C4%83ng-xu%E1%BA%A5t)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-SEC-02/AC-xx`)

- [ ] **AC-01** [Happy Path]: Người dùng bấm Đăng xuất -> cookie bị xóa, người dùng về trang đăng nhập.

- [ ] **AC-02** [Validation]: Gọi đăng xuất khi không có cookie phiên -> vẫn trả 200 và chuyển về trang đăng nhập.

- [ ] **AC-03** [Security]: Sau khi đăng xuất, gửi lại JWT cũ (chưa hết hạn) -> 401.

- [ ] **AC-04** [Idempotency]: Bấm Đăng xuất 2 lần liên tiếp -> cả hai đều thành công, không lỗi.

- [ ] **AC-05** [Boundary]: Người dùng đăng nhập trên 2 thiết bị, đăng xuất ở thiết bị A -> thiết bị B vẫn dùng được bình thường.

- [ ] **AC-06** [Resilience & Audit]: SSO logout endpoint lỗi nhưng thu hồi ứng dụng thành công -> logout app vẫn 200 và có LOGOUT. Kho thu hồi lỗi -> 500, cookie bị xóa nhưng không claim JWT đã vô hiệu; API không đọc được kho phiên -> 503 mặc định từ chối.
- [ ] **AC-07** [Security]: Response của API chi tiết hồ sơ và API tải tệp có header `Cache-Control: private, no-store`. Sau khi đăng xuất, bấm Back trên trình duyệt -> trang không hiện lại dữ liệu cũ, hệ thống kiểm tra lại phiên và chuyển về trang đăng nhập.


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-SEC-02/AC-01` đến `FR-SEC-02/AC-07` trên Staging, kiểm bằng kịch bản `TS-49`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
