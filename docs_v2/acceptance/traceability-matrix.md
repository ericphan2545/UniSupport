# Ma trận truy vết

Mỗi chức năng được nối tới tiêu chí chấp nhận, test case, milestone, issue trên GitHub và kịch bản kiểm thử. Ma trận dùng để đo độ phủ tiêu chí chấp nhận theo QMP mục 7.

**Cách đọc**

- **Số AC:** số tiêu chí chấp nhận của chức năng trong `modules/`. Mỗi AC là một test case.
- **Test case:** mã `TC-<mã chức năng>-AC<số>`, ví dụ `TC-STU-02-AC03` là test case cho tiêu chí AC-03 của STU-02.
- **Milestone:** số milestone trên GitHub (1 Tạo tài khoản và phân quyền · 2 Tiếp nhận và xử lý ticket · 3 Phân công, xử lý, chuyển phòng ban · 4 Trả kết quả · 5 Thống kê báo cáo). "Dùng chung" là chức năng làm dần qua nhiều milestone.
- **Issue:** issue cha (các issue backend/frontend) hoặc các issue "Dùng chung" của từng milestone.
- **Kịch bản:** mã kịch bản trong [test-scenarios.md](test-scenarios.md).
- **Trạng thái:** Tester cập nhật sau mỗi lần chạy: Chưa chạy / Đạt / Không đạt / Bị chặn.

## Cổng Sinh viên (`modules/M01-student-portal`)

| Mã | Chức năng | Số AC | Test case | Milestone | Issue | Kịch bản | Trạng thái |
|---|---|---|---|---|---|---|---|
| STU-01 | Xem thông tin cá nhân lấy từ SSO | 6 | TC-STU-01-AC01…AC06 | 1 | #30 (#31, #32) | E2E-01 | Chưa chạy |
| STU-02 | Tạo yêu cầu hỗ trợ | 6 | TC-STU-02-AC01…AC06 | 2 | #1 (#2, #3) | E2E-02, X-03, X-10 | Chưa chạy |
| STU-03 | Hiển thị phòng ban tiếp nhận | 6 | TC-STU-03-AC01…AC06 | 2 | #48 (#49) | E2E-02, X-10 | Chưa chạy |
| STU-04 | Đính kèm tệp minh chứng | 6 | TC-STU-04-AC01…AC06 | 2 | #19 (#20, #21) | E2E-02, X-04 | Chưa chạy |
| STU-05 | Nhận mã hồ sơ và xác nhận gửi thành công | 6 | TC-STU-05-AC01…AC06 | 2 | #33 (#34, #35) | E2E-02 | Chưa chạy |
| STU-06 | Xem danh sách yêu cầu của mình | 6 | TC-STU-06-AC01…AC06 | 2 | #36 (#37, #38) | E2E-02, X-10 | Chưa chạy |
| STU-07 | Xem trạng thái và tiến độ của từng yêu cầu | 6 | TC-STU-07-AC01…AC06 | 4 | #39 (#40, #41) | E2E-03, E2E-07, X-10 | Chưa chạy |
| STU-08 | Nhận thông báo trong app | 6 | TC-STU-08-AC01…AC06 | 2 | #16 (#17, #18) | E2E-03, E2E-06 | Chưa chạy |
| STU-09 | Bổ sung thông tin khi được yêu cầu | 6 | TC-STU-09-AC01…AC06 | 3 | #42 (#43, #44) | E2E-04, X-02, X-03 | Chưa chạy |
| STU-10 | Đánh giá chất lượng hỗ trợ | 6 | TC-STU-10-AC01…AC06 | 4 | #45 (#46, #47) | E2E-07, X-03 | Chưa chạy |

## Cổng Nhân viên (`modules/M02-staff-portal`)

| Mã | Chức năng | Số AC | Test case | Milestone | Issue | Kịch bản | Trạng thái |
|---|---|---|---|---|---|---|---|
| STF-01 | Hàng đợi hồ sơ của phòng ban | 6 | TC-STF-01-AC01…AC06 | 2 | #50 (#51, #52) | E2E-02, X-10 | Chưa chạy |
| STF-02 | Xem thông tin sinh viên và nội dung yêu cầu | 6 | TC-STF-02-AC01…AC06 | 2 | #68 (#69, #70) | E2E-02, X-10 | Chưa chạy |
| STF-03 | Xem và tải tệp đính kèm | 6 | TC-STF-03-AC01…AC06 | 2 | #65 (#66, #67) | E2E-02, X-04, X-10 | Chưa chạy |
| STF-04 | Ghi nhật ký xử lý | 6 | TC-STF-04-AC01…AC06 | 3 | #71 (#72, #73) | E2E-04, X-03, X-10 | Chưa chạy |
| STF-05 | Yêu cầu sinh viên bổ sung thông tin | 6 | TC-STF-05-AC01…AC06 | 3 | #56 (#57, #58) | E2E-04, X-03 | Chưa chạy |
| STF-06 | Đóng hồ sơ | 6 | TC-STF-06-AC01…AC06 | 4 | #59 (#60, #61) | E2E-07, X-02, X-03 | Chưa chạy |
| STF-07 | Phân công nhân viên phụ trách | 6 | TC-STF-07-AC01…AC06 | 3 | #53 (#54, #55) | E2E-04, E2E-06, X-02 | Chưa chạy |
| STF-08 | Đặt mức ưu tiên | 6 | TC-STF-08-AC01…AC06 | 3 | #74 (#75, #76) | E2E-05 | Chưa chạy |
| STF-09 | Chuyển hồ sơ sang phòng ban khác | 6 | TC-STF-09-AC01…AC06 | 3 | #62 (#63, #64) | E2E-05, X-02 | Chưa chạy |
| STF-10 | Tăng mức ưu tiên (escalate) | 6 | TC-STF-10-AC01…AC06 | 3 | #77 (#78, #79) | E2E-05 | Chưa chạy |

## Cổng Admin (`modules/M03-management-portal`)

| Mã | Chức năng | Số AC | Test case | Milestone | Issue | Kịch bản | Trạng thái |
|---|---|---|---|---|---|---|---|
| MGT-01 | Dashboard: số hồ sơ mới, đang xử lý, quá hạn | 6 | TC-MGT-01-AC01…AC06 | 5 | #86 (#87, #88) | E2E-06, E2E-08, X-10 | Chưa chạy |
| MGT-02 | Dashboard: thời gian xử lý trung bình | 6 | TC-MGT-02-AC01…AC06 | 5 | #89 (#90, #91) | E2E-08, X-09, X-10 | Chưa chạy |
| MGT-03 | Báo cáo loại yêu cầu phổ biến | 6 | TC-MGT-03-AC01…AC06 | 5 | #95 (#96, #97) | E2E-08, X-09, X-10 | Chưa chạy |
| MGT-04 | Báo cáo khối lượng công việc theo phòng ban | 6 | TC-MGT-04-AC01…AC06 | 5 | #98 (#99, #100) | E2E-08, X-09, X-10 | Chưa chạy |
| MGT-05 | Báo cáo mức độ hài lòng của sinh viên | 6 | TC-MGT-05-AC01…AC06 | 5 | #92 (#93, #94) | E2E-08, X-09, X-10 | Chưa chạy |
| MGT-06 | Quản lý phân quyền | 6 | TC-MGT-06-AC01…AC06 | 1 | #80 (#81, #82) | E2E-01, X-02, X-10 | Chưa chạy |
| MGT-07 | Thiết lập SLA cho từng loại yêu cầu | 6 | TC-MGT-07-AC01…AC06 | 2 | #83 (#84, #85) | E2E-02, X-02, X-08, X-10 | Chưa chạy |

## Xác thực và bảo mật (`modules/M04-authentication-security`)

| Mã | Chức năng | Số AC | Test case | Milestone | Issue | Kịch bản | Trạng thái |
|---|---|---|---|---|---|---|---|
| SEC-01 | Đăng nhập bằng SSO tài khoản trường | 6 | TC-SEC-01-AC01…AC06 | 1 | #4 (#5, #6) | E2E-01, X-06, X-10 | Chưa chạy |
| SEC-02 | Đăng xuất | 6 | TC-SEC-02-AC01…AC06 | 1 | #7 (#8, #9) | E2E-01, X-06 | Chưa chạy |
| SEC-03 | Phân quyền theo 3 vai trò | 6 | TC-SEC-03-AC01…AC06 | 1 | #10 (#11) | E2E-01, X-01, X-06 | Chưa chạy |
| SEC-04 | Giới hạn phạm vi dữ liệu theo vai trò | 6 | TC-SEC-04-AC01…AC06 | Dùng chung: 1, 2, 3, 4, 5 | #117, #123, #129, #155, #157 | E2E-05, E2E-08, X-01 | Chưa chạy |
| SEC-05 | Bảo vệ tệp đính kèm | 6 | TC-SEC-05-AC01…AC06 | Dùng chung: 2 | #123 | X-01, X-04 | Chưa chạy |
| SEC-06 | Ghi nhật ký hệ thống | 6 | TC-SEC-06-AC01…AC06 | Dùng chung: 1, 2, 3, 4 | #117, #123, #129, #155 | X-05 | Chưa chạy |

## Lõi hệ thống (`modules/M05-core-platform`)

| Mã | Chức năng | Số AC | Test case | Milestone | Issue | Kịch bản | Trạng thái |
|---|---|---|---|---|---|---|---|
| CORE-01 | Sinh mã hồ sơ | 6 | TC-CORE-01-AC01…AC06 | 2 | #24 (#25) | E2E-02, X-02, X-09 | Chưa chạy |
| CORE-02 | Trạng thái hồ sơ và quy tắc chuyển trạng thái | 6 | TC-CORE-02-AC01…AC06 | 2 | #12 (#13) | E2E-03, E2E-06, X-02 | Chưa chạy |
| CORE-03 | Tính hạn xử lý (SLA) | 6 | TC-CORE-03-AC01…AC06 | 2 | #14 (#15) | E2E-02, E2E-04, E2E-06, X-09 | Chưa chạy |
| CORE-04 | Mức độ ưu tiên | 6 | TC-CORE-04-AC01…AC06 | 2 | #26 (#27) | E2E-02, E2E-05 | Chưa chạy |
| CORE-05 | Danh mục loại yêu cầu và phòng ban | 6 | TC-CORE-05-AC01…AC06 | 2 | #22 (#23) | E2E-02, X-08 | Chưa chạy |
| CORE-06 | Lịch sử thay đổi của hồ sơ | 6 | TC-CORE-06-AC01…AC06 | Dùng chung: 2, 3, 4 | #123, #129, #155 | E2E-03, X-05 | Chưa chạy |
| CORE-07 | Giới hạn tệp đính kèm | 6 | TC-CORE-07-AC01…AC06 | 2 | #28 (#29) | E2E-02, X-04 | Chưa chạy |
| CORE-08 | Giao diện song ngữ VN/EN | 6 | TC-CORE-08-AC01…AC06 | Dùng chung: 1, 2, 3, 4, 5 | #118, #124, #130, #156, #158 | E2E-03, X-07 | Chưa chạy |
| CORE-09 | Giao diện responsive cho mobile và desktop | 6 | TC-CORE-09-AC01…AC06 | Dùng chung: 1, 2, 3, 4, 5 | #118, #124, #130, #156, #158 | X-07 | Chưa chạy |

## Tổng hợp

- 42 chức năng, 252 tiêu chí chấp nhận, tương ứng 252 test case.
- Mọi chức năng đều nằm trong ít nhất một kịch bản kiểm thử.
