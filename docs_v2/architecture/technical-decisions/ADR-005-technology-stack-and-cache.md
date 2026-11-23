# ADR-005: Lựa chọn ngăn xếp công nghệ và bộ nhớ đệm phía server

**Trạng thái:** Chấp nhận, ngày 17/09/2026 (Mô phỏng). Hai mốc tách biệt:

- **11/09/2026, hạn hoàn thành đề xuất kỹ thuật:** đội thực thi gửi đề xuất ngăn xếp (WBS `1.04–1.05`, `#105`).
- **17/09/2026, xác nhận tương thích và chấp nhận ADR:** bộ phận CNTT của Aurora University xác nhận tương thích hạ tầng (WBS `1.11–1.14`); ban quản lý chấp nhận ADR-005, ghi tại Decision Log `DL-02`.

**Liên quan:** `FR-STU-01/BR-05`, `NFR-PERF-10`, `NFR-PERF-11`, giả định `A-11`, `A-14`, rủi ro `R-27`, WBS `1.04`, `1.05`, `1.11`, `1.14`, `1.35`

Giá và lịch hiện hành tham chiếu Proposal và Master Plan v2.0. Việc xác nhận hồ sơ mô phỏng nằm ở WBS 1.37, sau các bước chuẩn bị và đánh giá tương thích 1.11–1.15.

## Bối cảnh

Bộ đặc tả nghiệp vụ (`modules/`) và yêu cầu phi chức năng (`non-functional-requirements/`) quy định các yêu cầu bắt buộc ở mức hành vi:
- Web application responsive 3 cổng, song ngữ VN/EN (`FR-CORE-08`, `FR-CORE-09`).
- CSDL quan hệ hỗ trợ transaction nguyên tử, sequence theo năm (`ADR-001`), ràng buộc unique, khóa lạc quan theo version (`ADR-004`) và phân quyền chỉ thêm/đọc trên bảng nhật ký (`FR-SEC-06`).
- Kho tệp lưu ngoài thư mục web, tên ngẫu nhiên, chỉ tải qua API có kiểm tra quyền (`FR-SEC-05`).
- **Bộ nhớ đệm phía server (server-side cache):** lưu thông tin cá nhân lấy từ SSO riêng theo từng tài khoản với TTL 15 phút (`FR-STU-01/BR-05`, `NFR-PERF-10`) và cache danh mục loại yêu cầu/phòng ban (`NFR-PERF-11`).
- Đóng gói chạy bằng Docker Compose trên server của Aurora University (`A-11`, `NFR-DEP-04`).

PRD chỉ quy định hành vi: **cache phía server TTL 15 phút theo tài khoản**. Công nghệ thực hiện yêu cầu đó do ADR này quyết định; PRD không ghi tên sản phẩm cụ thể.

## Quyết định

Ngăn xếp đã chốt ngày 17/09/2026 (Mô phỏng), nằm trong giả định `A-14` nên không đổi effort, giá hay mốc:

| Thành phần | Lựa chọn | Lý do chính |
|---|---|---|
| Backend API | TypeScript trên Node.js, framework NestJS | Cùng ngôn ngữ với frontend; hỗ trợ module, guard phân quyền và validation phù hợp `FR-SEC-03`, `FR-SEC-04` |
| Frontend | React và TypeScript (SPA responsive, i18n VN/EN) | Đáp ứng `FR-CORE-08`, `FR-CORE-09`; đội FE có sẵn kinh nghiệm |
| CSDL quan hệ | PostgreSQL | Có transaction, sequence theo năm (`ADR-001`), khóa dòng, version (`ADR-004`) và phân quyền `INSERT/SELECT` riêng cho bảng nhật ký (`FR-SEC-06`) |
| Bộ nhớ đệm phía server | Redis | TTL 15 phút theo tài khoản (`FR-STU-01/BR-05`, `NFR-PERF-10`), cache danh mục (`NFR-PERF-11`) và bộ đếm rate limit (`NFR-SEC-13`) |
| Kho tệp | Volume Docker gắn ngoài web root, đặt trên kho lưu tệp do trường cung cấp (`A-11`) | Không phát URL công khai; chỉ tải qua API có kiểm tra quyền (`FR-SEC-05`) |
| Đóng gói | Docker Compose trên server Linux của trường | Khớp `A-11`, `NFR-DEP-04` |

Bộ phận CNTT của trường đã xác nhận server đủ tài nguyên cho PostgreSQL và Redis và chạy được Docker Compose (Mô phỏng).

## Giả định công nghệ đã dùng để ước lượng baseline (1109 giờ / 424.721.000 VNĐ)

Baseline effort, giá và lịch (`Bang_Effort.md`, `quotation.xlsx`, `Master Plan`) được lập ngày 06/09/2026, trước khi chốt ngăn xếp, theo giả định **`A-14`**. Lựa chọn ở mục Quyết định nằm trong các phương án dưới đây:
- Ngăn xếp web phổ biến, mã nguồn mở, phù hợp năng lực đội thực thi và chạy gọn bằng Docker Compose trên server Linux của trường:
  - **Backend API:** TypeScript (Node.js/NestJS hoặc Express) hoặc Java (Spring Boot) / Python (FastAPI).
  - **Frontend:** React / Next.js hoặc Vue (SPA responsive + i18n VN/EN).
  - **CSDL quan hệ:** PostgreSQL (đáp ứng sẵn transaction, sequence, row lock, phân quyền `INSERT/SELECT` riêng trên bảng nhật ký).
  - **Bộ nhớ đệm phía server (cache 15 phút):** **Redis** là phương án triển khai đề xuất ưu tiên (hỗ trợ sẵn TTL 15 phút, rate limit và phiên chia sẻ nếu chạy nhiều worker); có thể thay bằng cache trong bộ nhớ / bảng cache CSDL nếu hệ thống chỉ chạy một instance trên hạ tầng tối giản của trường.
  - **Kho tệp:** Volume lưu trữ cục bộ gắn ngoài web root hoặc S3-compatible storage (MinIO) chạy qua Docker Compose.

## Vai trò quyết định và mốc chốt

| Bước | Vai trò quyết định | Mốc thời gian & WBS | Đầu ra bắt buộc |
|---|---|---|---|
| 1. Đề xuất & chọn ngăn xếp kỹ thuật (gồm phương án cache Redis hay tương đương) | `BE Dev / Senior` (chủ trì), `FE Dev / Senior`, `DevOps / Mid` | `09/09 – 11/09/2026` — WBS `1.04`, `1.05` (issue `#105`) | Đề xuất ngăn xếp gửi ban quản lý, hạn chậm nhất 11/09/2026 (đã thực hiện, Mô phỏng) |
| 2. Xác nhận tương thích hạ tầng với Aurora University | `PM / Senior`, `BE Dev / Senior` làm việc với bộ phận CNTT trường | `14/09 – 17/09/2026` — WBS `1.11`, `1.14` (issue `#113`, `#114`) | Xác nhận bằng văn bản tương thích Docker Compose, tài nguyên RAM/CPU (cho CSDL + cache) và SSO OIDC; ADR chuyển sang `Chấp nhận` ngày 17/09/2026, trước khi lập trình `1.20–1.23` (đã thực hiện, Mô phỏng) |

## Phần setup làm trước và sau khi chốt (WBS `1.16`, 07/09–16/09/2026)

WBS `1.16` (repo, CI, môi trường dev, SSO giả lập; DevOps / Mid) chạy trước ngày chấp nhận ADR. Phần việc chia theo mức phụ thuộc vào quyết định công nghệ:

- **Không phụ thuộc ngăn xếp, làm từ 07/09:** tạo repo và bảo vệ nhánh `main` theo Team Charter mục 9; khung Docker Compose (cách đóng gói đã cố định ở `A-11`); SSO giả lập theo OIDC (`A-05`) chỉ bật ở dev và test ([security-design.md](../security-design.md)).
- **Theo đề xuất ngày 11/09 (thứ Sáu), làm từ 14/09 (thứ Hai, ngày làm việc đầu tiên sau hạn đề xuất) đến hết `1.16` ngày 16/09:** cấu hình môi trường dev cho các thành phần trong mục Quyết định. Đây là cấu hình tạm dùng cho dev, chưa dùng cho Staging.
- **Staging (WBS `1.17–1.19`, 17/09–22/09):** bắt đầu chuẩn bị ngày 17/09 theo lịch hiện hành (`1.17`, `1.18` trong ngày 17/09; `1.19` đến 22/09), dựa trên repo và CI của `1.16`. Các cấu hình phụ thuộc kết quả xác nhận hạ tầng của trường (tài nguyên RAM/CPU cho CSDL và cache, kho tệp, Docker Compose trên server) chỉ áp dụng sau khi ADR được chấp nhận ngày 17/09. Lập trình `1.20–1.23` bắt đầu 21/09.

Nếu ngày 17/09 trường không chấp nhận một thành phần, cấu hình tạm được sửa theo mục 1 dưới đây.

## Ảnh hưởng nếu thay đổi

1. **Thay đổi trong phạm vi tương đương giả định `A-14`** sau khi đã chốt (ví dụ đổi Redis sang in-memory/DB cache cho triển khai đơn node): tác động kỹ thuật nhỏ (≤ 4–6 giờ cấu hình Docker/thư viện), xử lý nội bộ trong quỹ dự phòng `Q-01` (dòng WBS `1.35`, rủi ro `R-27`), **không thay đổi giá 424.721.000 VNĐ và không dời mốc Go-live**.
2. **Thay đổi vượt khỏi giả định `A-14` hoặc `A-11` theo yêu cầu bắt buộc của trường** (ví dụ yêu cầu CSDL thương mại bản quyền, máy chủ Windows không chạy Docker Compose, hoặc ngôn ngữ/framework đội thực thi không có sẵn nhân sự): kích hoạt rủi ro `R-27` và điều khoản giả định `A-11`, `A-14`; PM / Senior lập đánh giá tác động effort, lịch và báo giá bổ sung theo quy trình Change Request (`03-quan-tri-giam-sat.md` mục 5) trước khi bắt đầu giai đoạn lập trình `1.20–1.23`.
