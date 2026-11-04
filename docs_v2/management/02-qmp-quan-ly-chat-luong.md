# UniSupport – Kế hoạch quản lý chất lượng (QMP)

Phiên bản 1.3

**Phạm vi:** thế nào là đạt, đo ra sao, xử lý lỗi thế nào. Không chứa phạm vi sản phẩm, tiến độ, giờ công, quyền duyệt, luật làm việc nhóm (xem [00-README](00-README.md)). Các tài liệu khác trỏ vào số mục của tài liệu này, nên đổi số mục thì phải sửa chỗ trỏ.

## 1. Mục tiêu chất lượng

| # | Mục tiêu | Đo bằng |
|---|---|---|
| 1 | Mọi chức năng đã thống nhất có test case và đạt | 100% chức năng có test case; ≥95% test case đạt |
| 2 | Không còn lỗi Critical và Major khi Go-live | Critical = 0, Major = 0 |
| 3 | Chạy ổn với 100–200 người dùng đồng thời | p95 ≤ 3 giây, tỷ lệ lỗi ≤ 5% |
| 4 | Đúng quyền cho 3 vai trò | 0 lỗi lộ dữ liệu ngoài quyền |
| 5 | Dùng được trên điện thoại và máy tính, có VN/EN | Đổi VN/EN đầy đủ; ở màn hình 360px không có thanh cuộn ngang |

## 2. Tiêu chí nghiệm thu

### 2.1 Đóng một milestone

Cần đủ cả 5 điều:

- Mọi issue của milestone đạt DoD (mục 5).
- 100% test case của milestone đã chạy; đã test trọn luồng qua các vai trò trên Staging.
- Không còn lỗi Critical hoặc Major đang mở.
- Regression của các milestone đã đóng đều đạt.
- QA / Senior xác nhận, PM / Senior đóng milestone trên GitHub. Sau đó áp dụng mục 6.

Lưu ý khi đánh giá:

- Test case bị chặn hoặc chưa chạy không tính là đã hoàn thành kiểm thử.
- Nếu milestone dùng dữ liệu seed hoặc SSO giả lập, báo cáo ghi rõ giới hạn. Kết quả chỉ chứng minh phần đã kiểm tra; luồng tích hợp thật vẫn phải được kiểm thử trước Go-live.

### 2.2 Go-live

- Critical = 0 và Major = 0.
- Minor được còn, nhưng phải nằm trong danh sách lỗi còn tồn (QA đề xuất, PM duyệt).
- ≥95% test case đạt; không có lỗi chặn khi chạy 100–200 người dùng đồng thời.
- Mức 95% không cho phép bỏ qua lỗi phân quyền, lộ dữ liệu hoặc lỗi chặn luồng chính. Những lỗi này xử lý theo mức độ ảnh hưởng thực tế.
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
| Chức năng | Mọi task backend và frontend của chức năng đã xong · đạt đủ tiêu chí chấp nhận · phần dùng chung (VN/EN, responsive, quyền, nhật ký) đã làm cho các màn hình của chức năng |
| Milestone | Đạt checklist 2.1 · đã đóng trên GitHub |
| Dự án | Đạt checklist 2.2 · hệ thống chạy trên server của trường với SSO thật |

## 6. Xử lý lỗi

**Trạng thái:** Mới → Đã xác nhận → Đang sửa → Chờ kiểm tra → Đóng (hoặc Mở lại nếu kiểm tra không đạt).

**Quy trình:**

- **Phát hiện:** Tester tạo issue GitHub, gắn label `bug` và milestone đang làm.
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

- Mọi mức lỗi đều tạo issue mới, gắn milestone `Hotfix`.
- Critical: QA / Senior xác nhận mức; PM / Senior phê duyệt việc sửa; sửa xong chạy regression các chức năng liên quan.
- Major và Minor: PM / Senior quyết thứ tự sửa. Vì Go-live yêu cầu Major = 0, mọi Major phải sửa trước Go-live.

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
| Test case đạt | Đạt / đã chạy | ≥95% |
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
