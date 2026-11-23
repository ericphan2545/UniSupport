**FR:** FR-SEC-01  
**Milestone:** MS-01 · Chốt yêu cầu, đăng nhập và phân quyền (kết thúc 25/09/2026)  
**Owner:** BE Dev / Senior (backend #5), FE Dev / Senior (frontend #6)  
**Hỗ trợ / duyệt:** —  
**Hạn:** 23/09/2026  
**WBS:** 1.20, 1.22, 1.24, 1.25  
**Thời gian:** 21/09/2026 → 23/09/2026  

## Mô tả (theo PRD)

Người dùng đăng nhập bằng tài khoản trường cấp thông qua SSO, không cần tạo mật khẩu riêng, và được đưa vào đúng cổng theo vai trò.



**Đặc tả đầy đủ:** [docs_v2/modules/M04-authentication-security/prd.md – FR-SEC-01](https://github.com/ericphan2545/UniSupport/blob/baseline-v2.0/docs_v2/modules/M04-authentication-security/prd.md#fr-sec-01-%C4%91%C4%83ng-nh%E1%BA%ADp-b%E1%BA%B1ng-sso-t%C3%A0i-kho%E1%BA%A3n-tr%C6%B0%E1%BB%9Dng)



## Acceptance Criteria (theo PRD, mã đầy đủ `FR-SEC-01/AC-xx`)

- [ ] **AC-01** [Happy Path]: Sinh viên đăng nhập SSO lần đầu -> tài khoản `STUDENT` được tạo, người dùng vào Cổng Sinh viên. Nhân viên đã được thêm ở FR-MGT-06 đăng nhập -> vào Cổng Nhân viên.

- [ ] **AC-02** [Validation]: Callback có `state` đã được dùng trước đó -> 400, không có phiên mới. Token SSO sai chữ ký -> 401.

- [ ] **AC-03** [Boundary]: Phiên được tạo lúc 08:00 -> request lúc 15:59 thành công; request lúc 16:01 nhận 401 và người dùng được chuyển về trang đăng nhập.

- [ ] **AC-04** [Security]: Tài khoản bị vô hiệu hóa đăng nhập -> 403, không tạo phiên. Cookie phiên có đủ các cờ `HttpOnly`, `Secure`, `SameSite=Lax`. Response trả về trình duyệt không chứa access token của SSO.

- [ ] **AC-05** [Rate Limit]: Lần gọi callback thứ 11 trong 1 phút từ cùng một IP -> 429.

- [ ] **AC-06** [Resilience & Audit]: SSO không phản hồi trong 5 giây -> 503 với thông báo thân thiện, ứng dụng không treo. Đăng nhập thành công -> có bản ghi `LOGIN_SUCCESS`. Đăng nhập thất bại -> có bản ghi `LOGIN_FAILED` kèm lý do.
- [ ] **AC-07** [Validation]: Callback có `state` không khớp, hoặc `state` đã tạo quá 10 phút -> 400, không tạo phiên. Người dùng bấm hủy trên trang SSO -> về trang đăng nhập với thông báo "Bạn đã hủy đăng nhập".

- [ ] **AC-08** [Security]: SSO trả token không có email đã được xác thực -> không tạo tài khoản và phiên; log có mã lý do, không chứa token. Cấu hình bật SSO giả lập ở production -> ứng dụng không khởi động. Chạy lại bước nạp tài khoản Admin đầu tiên -> không tạo bản trùng, không đổi quyền của tài khoản đã có.

- [ ] **AC-09** [Business Logic]: Tài khoản `STAFF` chưa có phòng ban đăng nhập -> đăng nhập thành công; Cổng Nhân viên hiển thị "Tài khoản chưa được gán phòng ban".


## Điều kiện hoàn thành

- [ ] Mọi sub-issue backend và frontend đã đóng
- [ ] Đạt mọi AC `FR-SEC-01/AC-01` đến `FR-SEC-01/AC-09` trên Staging, kiểm bằng kịch bản `TS-48`
- [ ] Đạt Definition of Done mức chức năng (QMP mục 5)


_Kế hoạch v2.0: ngày và giờ công theo sheet WBS của `docs_v2/management/UniSupport_Masterplan_nhom2.xlsx`._
