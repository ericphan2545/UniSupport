# Biên bản họp dự án (Mẫu chuẩn & ví dụ Mô phỏng)

> **Quy định:** Áp dụng cho buổi họp tuần của ban quản lý (thứ Hai 20h00 qua Discord, Team Charter mục 7) và họp đầu/cuối mỗi milestone (`03` mục 2.2, 2.3). Thành viên ghi biên bản theo lượt; PM kiểm duyệt và lưu bản chính thức trong vòng 24 giờ.
>
> Đường dẫn thư mục Google Drive lưu các biên bản thật: *chờ Lê Hoàng Tùng xác nhận tại [records/README.md](README.md)*.

---

## Phần A — Mẫu trống (sao chép cho mỗi phiên họp)

- **Loại cuộc họp:** Họp tuần / Họp đầu milestone `MS-xx` / Họp đóng milestone `MS-xx` / Họp gấp
- **Thời gian & kênh:** `dd/mm/yyyy HH:mm – HH:mm` · Discord
- **Người chủ trì:** `…` · **Thư ký ghi biên bản:** `…`
- **Thành phần tham dự:** `…` (Vắng: `…`, người thay thế: `…`)

### 1. Nội dung rà soát chính

- **Tiến độ & milestone hiện tại:** *(dẫn số liệu từ bộ lọc GitHub Issues ở `03` mục 3.1)*
- **Giờ công thực tế & dự báo (`EAC`):** *(dẫn từ [time-tracking-log.md](time-tracking-log.md))*
- **Chất lượng & lỗi mở:** *(theo QMP mục 7)*
- **Rà soát sổ rủi ro (`risk-register.md`):** *(rủi ro mức Cao, rủi ro mới, thay đổi điểm/trạng thái)*
- **Vướng mắc / quyết định cần chốt:** *(nếu có quyết định mới, ghi mã `DL-xx` sang [decision-log.md](decision-log.md))*

### 2. Việc cần làm (Action Items)

| STT | Việc cần làm | Issue / Tài liệu liên quan | Người phụ trách | Thời hạn | Trạng thái |
|---|---|---|---|---|---|
| 1 | | | | | Mở |

---

## Phần B — Ví dụ biên bản họp Tuần 6 (`16/10/2026` — Mô phỏng)

*(Khớp trực tiếp với ví dụ Báo cáo tuần 6 ở [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2 và [time-tracking-log.md](time-tracking-log.md).)*

- **Loại cuộc họp:** Họp tổng kết Tuần 6 (`MS-02` Gửi và tiếp nhận yêu cầu) — **Mô phỏng**
- **Thời gian & kênh:** `16/10/2026 20:00 – 20:45` · Discord
- **Người chủ trì:** PM / Senior (Lê Hoàng Tùng) · **Thư ký:** Đào Tuấn Bảo
- **Thành phần tham dự:** Lê Hoàng Tùng, Ngô Tấn Đức, Phan Tiến Thịnh, Đào Tuấn Bảo (đủ 4/4)

### 1. Nội dung rà soát chính (Mô phỏng)

- **Trạng thái chung: Vàng** (do chỉ số Tiến độ ở mức Vàng: 5/7 task đúng hạn = 71% < 90%; `#25` sinh mã hồ sơ `FR-CORE-01` chậm 2 ngày).
- **Giờ công: Xanh** — Tuần 6 dùng `52h`, lũy kế `455h`, còn lại `407h`, dự báo `862h ≤ 870h`, không task nào lệch > 20%, chưa giải ngân dự phòng `Q-03` (`11h`).
- **Chất lượng: Xanh** — 0 Critical, 0 Major, 3 Minor đang sửa, 4 task chờ test; phần dùng chung `#123` (`2.05`) và `#124` (`2.06–2.07`) đã xong trước tuần test cuối.
- **Rủi ro:** rà `R-02` (Trung bình — đã gửi email tuần nhắc Aurora University xác nhận danh mục thật) và `R-09` (Trung bình — chưa có dấu hiệu trùng mã).
- **Quyết định chốt (`DL-03`):** BE Dev / Senior hỗ trợ trực tiếp BE Dev / Mid hoàn tất `#25` trước `20/10/2026` để bảo vệ trọn tuần test cuối và buổi demo `MS-02` ngày `23/10/2026`.

### 2. Việc cần làm (Mô phỏng)

| STT | Việc cần làm | Issue / Tài liệu liên quan | Người phụ trách | Thời hạn | Trạng thái |
|---|---|---|---|---|---|
| 1 | Hỗ trợ hoàn tất giao dịch sinh mã hồ sơ đồng thời `FR-CORE-01` | `#25`, `DL-03` | BE Dev / Senior & BE Dev / Mid | 20/10/2026 | Đang làm |
| 2 | Chạy test trọn luồng `TS-02`, `TS-03` và regression `TS-01` trên Staging | `#126` (`2.10–2.11`) | QA / Senior & Tester / Junior | 22/10/2026 | Mở |
| 3 | Gửi báo cáo tuần 6 và nhắc danh mục thật (`R-02`) cho Aurora University | `03` mục 6.2, `R-02` | PM / Senior | 16/10/2026 | Xong |
| 4 | Chuẩn bị demo đóng `MS-02` và họp khởi động `MS-03` | `MS-02`, `MS-03` | PM / Senior, BA / Mid, QA / Senior | 23/10/2026 | Mở |
