# UniSupport – Kế hoạch quản lý chất lượng (QMP)

Phiên bản 1.4

**Phạm vi:** thế nào là đạt, đo ra sao, xử lý lỗi thế nào. Không chứa phạm vi sản phẩm, tiến độ, giờ công, quyền duyệt, luật làm việc nhóm (xem [mục lục](../README.md)). Các tài liệu khác trỏ vào số mục của tài liệu này, nên đổi số mục thì phải sửa chỗ trỏ.

## 1. Mục tiêu chất lượng

| # | Mục tiêu | Đo bằng |
|---|---|---|
| 1 | Mọi tiêu chí chấp nhận (AC) của 42 FR đã chốt có test case và đạt | 100% AC có test case; 100% AC đạt khi Go-live (mục 2.2) |
| 2 | Không còn lỗi Critical và Major khi Go-live | Critical = 0, Major = 0 |
| 3 | Chạy ổn với 100–200 người dùng đồng thời | p95 ≤ 3 giây, tỷ lệ lỗi ≤ 5% |
| 4 | Đúng quyền cho 3 vai trò | 0 lỗi lộ dữ liệu ngoài quyền |
| 5 | Dùng được trên điện thoại và máy tính, có VN/EN | Đổi VN/EN đầy đủ; ở màn hình 360px không có thanh cuộn ngang |

## 2. Tiêu chí nghiệm thu

### 2.1 Đóng một milestone

Cần đủ cả 5 điều:

- Mọi issue của milestone đạt DoD (mục 5).
- 100% test case của milestone đã chạy và mọi AC của các FR (kể cả phần AC dùng chung) thuộc milestone đạt; đã test trọn luồng qua các kịch bản `TS-01` đến `TS-08` tương ứng với chức năng đã có trên Staging tại milestone đó (không phụ thuộc chức năng của milestone sau).
- Không còn lỗi Critical hoặc Major đang mở.
- Regression của các milestone đã đóng đều đạt.
- QA / Senior xác nhận, PM / Senior đóng milestone trên GitHub. Sau đó áp dụng mục 6.

Lưu ý khi đánh giá:

- Test case bị chặn hoặc chưa chạy không tính là đã hoàn thành kiểm thử.
- Nếu milestone dùng dữ liệu seed hoặc SSO giả lập, báo cáo ghi rõ giới hạn. Kết quả chỉ chứng minh phần đã kiểm tra; luồng tích hợp thật vẫn phải được kiểm thử trước Go-live.

### 2.2 Go-live

Mục này là nơi gốc phía đội của điều kiện Go-live. Điều kiện nghiệm thu với khách là Proposal mục 6.2; mục này không đặt ngưỡng lỏng hơn Proposal. Team Charter và `project/constraints.md` chỉ dẫn lại mục này.

- Mọi AC của 42 FR trong bộ đặc tả đã chốt ở `MS-01` được kiểm bằng kịch bản test và đạt.
- Critical = 0 và Major = 0.
- Lỗi Minor chỉ được còn khi không làm AC nào không đạt, và phải nằm trong danh sách lỗi còn tồn (QA đề xuất, PM duyệt).
- Đạt mục tiêu `G-01` đến `G-05` ([product/README.md](../product/README.md)); riêng `G-05`: đạt `NFR-PERF-01` đến `NFR-PERF-03`, không có lỗi chặn khi chạy 100–200 người dùng đồng thời trên hạ tầng trường.
- Có Báo cáo kiểm thử và Usability Test Report.
- Các hạng mục bàn giao còn lại đủ theo Proposal 6.1 (PM kiểm).

## 3. Quyết định

Ai quyết về chất lượng, mức lỗi, thứ tự sửa lỗi và Go-live: xem [03](03-quan-tri-giam-sat.md) mục 4.

## 4. QA/QC

### 4.1 QA (kiểm soát quy trình)

Test case được viết và QA / Senior duyệt trước khi chức năng lên Staging. Luật làm việc nhóm: Team Charter.

### 4.2 QC (kiểm tra sản phẩm)

- Chuẩn đạt của mỗi chức năng là tiêu chí chấp nhận đã chốt bằng văn bản với khách (Proposal 6.2), không chép lại ở đây. Gồm quyền, luồng trạng thái, VN/EN, tệp đính kèm, thông báo lỗi.
- Test đủ 5 nhóm tiêu chí: Happy Path, Validation, Security, Concurrency, Resilience.
- Môi trường: Staging; Chrome, Safari, Edge, Firefox (2 phiên bản mới nhất); điện thoại và máy tính; kiểm tra ở màn hình 360px.
- Regression: các milestone đã đóng phải đạt khi đóng milestone mới.

## 5. Definition of Done

| Cấp | Hoàn thành khi |
|---|---|
| Task | Theo Team Charter (mục 5, bước 4) |
| Chức năng | Mọi task backend và frontend của chức năng đã xong · đạt đủ tiêu chí chấp nhận · phần dùng chung (VN/EN, responsive, quyền, nhật ký, lịch sử) đã làm cho các màn hình của chức năng *(nên Hạn của FR parent task trên GitHub phải ≥ ngày hoàn thành của BE, FE và phần dùng chung tương ứng trong WBS, xong trước đợt test trọn luồng cuối milestone)* |
| Milestone | Đạt checklist 2.1 · đã đóng trên GitHub |
| Dự án | Đạt checklist 2.2 · hệ thống chạy trên server của trường với SSO thật |

## 6. Xử lý lỗi

**Trạng thái:** Mới → Đã xác nhận → Đang sửa → Chờ kiểm tra → Đóng (hoặc Mở lại nếu kiểm tra không đạt).

**Quy trình:**

- **Phát hiện:** Tester tạo issue bằng mẫu Bug trên GitHub (`.github/ISSUE_TEMPLATE/2-bug.md`), mẫu tự gắn nhãn `bug`; gắn nhãn mức `sev: Critical`, `sev: Major` hoặc `sev: Minor` và milestone đang làm.
- **Xác nhận:** QA / Senior xác nhận và xếp mức lỗi trong 1 ngày làm việc.
- **Sửa:** dev phụ trách mảng tạo PR, có người review, merge, deploy lên Staging.
- **Kiểm tra lại:** Tester test lại. Đạt thì Đóng, không đạt thì Mở lại.

Lỗi trùng, không tái hiện được hoặc thực chất là yêu cầu thay đổi: ghi lý do và liên kết issue liên quan. "Không tái hiện được" không có nghĩa là "đã sửa".

**Một issue lỗi phải ghi:**

- chức năng và tiêu chí chấp nhận liên quan
- các bước tái hiện
- kết quả mong đợi và thực tế
- trình duyệt, thiết bị
- tài khoản test
- correlation_id
- ảnh chụp

| Mức | Nghĩa là | Hạn sửa |
|---|---|---|
| Critical | Chặn luồng chính; mất hoặc sai dữ liệu; lộ dữ liệu ngoài quyền | 1 ngày làm việc |
| Major | Sai so với tiêu chí chấp nhận nhưng có cách khác để làm | 3 ngày làm việc |
| Minor | Lỗi giao diện, chữ, bản dịch | Trước khi đóng milestone, hoặc vào danh sách lỗi còn tồn |

**Lỗi phát hiện sau khi milestone đã đóng.** Milestone đã đóng thì **không mở lại**.

- Mọi mức lỗi đều tạo issue mới (mẫu Bug), gắn milestone `Hotfix`, nhãn `bug` và mức `sev: …`.
- Critical: QA / Senior xác nhận mức trong 1 ngày làm việc; PM / Senior phê duyệt việc sửa ngay (hạn sửa 1 ngày làm việc); sửa xong bắt buộc chạy lại regression các chức năng liên quan (`TS-01` đến `TS-18`).
- Major và Minor: PM / Senior quyết thứ tự sửa. Vì Go-live yêu cầu Major = 0, mọi Major phải sửa trước Go-live.
- **Hạch toán WBS và thanh toán:** công sức sửa lỗi `Hotfix` trong giai đoạn đang phát triển hạch toán vào dòng WBS "Sửa lỗi sau test" của milestone hiện tại (ví dụ `4.11–4.14` tại `MS-04`, `5.13–5.16` tại `MS-05`), ghi giờ thực tế vào Bảng theo dõi giờ và cộng vào dự báo baseline 870 giờ (bên phát triển chịu chi phí nếu vượt). Việc phát hiện lỗi `Hotfix` không mở lại milestone đã đóng và không đảo ngược biên bản nghiệm thu hay thanh toán đợt 2 của `MS-03` đã ký; tuy nhiên lỗi Critical/Major phải đóng hoàn toàn trước khi ký biên bản nghiệm thu tổng thể và thanh toán đợt 3.

**Điều kiện dừng test hoặc dừng release:**

- Có lỗi Critical đang mở.
- Regression làm hỏng một luồng chính.
- Phát hiện lộ dữ liệu ngoài quyền.
- Test case của một chức năng chưa chạy đủ.
- Staging không ổn định (được test tiếp khi nguyên nhân đã xử lý và Tester / Junior xác nhận lại).

## 7. Metrics chất lượng

Đo cuối mỗi milestone, và mỗi tuần trong giai đoạn test. Chỉ số tiến độ và giờ công nằm ở [03](03-quan-tri-giam-sat.md) mục 3.

| Chỉ số | Cách tính | Ngưỡng |
|---|---|---|
| Test case đạt | Đạt / đã chạy | Theo dõi hằng tuần; khi đóng milestone và Go-live mọi AC phải đạt (mục 2) |
| Critical đang mở | Đếm | 0 khi đóng milestone và Go-live |
| Major đang mở | Đếm | 0 khi đóng milestone và Go-live |
| Độ phủ tiêu chí chấp nhận | Số tiêu chí có test case / tổng tiêu chí của milestone | 100% |
| Tỷ lệ lỗi mở lại | Lỗi bị mở lại / lỗi đã đóng | ≤10% |
| Thời gian xác nhận lỗi | Từ lúc tạo issue đến lúc QA xác nhận | ≤1 ngày làm việc |
| Thời gian sửa lỗi | Từ lúc xác nhận đến lúc đóng | Theo bảng mục 6 |
| Lỗi lọt qua milestone đã đóng | Số issue trong milestone Hotfix | 0 Critical |
| Hiệu năng | p95 và tỷ lệ lỗi ở 100–200 người dùng | p95 ≤ 3s, lỗi ≤ 5% |
| Usability | Tỷ lệ người tham gia hoàn thành các tác vụ chính | ≥80%, hoặc theo Usability Test Report |

Quy tắc khi báo cáo:

- Không loại test case thất bại khỏi mẫu số chỉ để đạt tỷ lệ yêu cầu.
- Nếu mẫu số bằng 0, ghi "Chưa có dữ liệu", không báo 100%.
- Nếu chưa đủ dữ liệu, QA kết luận "Chưa đủ cơ sở xác nhận", không suy diễn thành đạt.

## 8. Thay đổi chất lượng

Thay đổi mục tiêu, ngưỡng metrics hoặc phạm vi kiểm thử (ví dụ bỏ test hiệu năng, hạ tỷ lệ test case đạt) đi theo quy trình ở [03](03-quan-tri-giam-sat.md) mục 5. QA / Senior đánh giá tác động chất lượng. Cập nhật phiên bản QMP.
