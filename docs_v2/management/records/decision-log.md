# Decision Log (Mẫu chuẩn & dữ liệu Mô phỏng)

> **Trạng thái dữ liệu:** Các bản ghi dưới đây là **Mô phỏng**, làm mẫu theo quy định ở [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 1.2, 4 và 6.1.
>
> **Người quản lý:** `PM / Senior` (ban quản lý: Lê Hoàng Tùng) · **Quyền đọc:** công khai (repo công khai). **Cập nhật:** `PM / Senior` ghi; Lê Hoàng Tùng mở PR, Phan Tiến Thịnh duyệt. Nơi lưu chính thức: file này trong repo, cập nhật qua PR ([records/README.md](README.md)).

## Quy tắc ghi nhận

- Ghi mọi quyết định chốt với Aurora University, quyết định kỹ thuật trọng yếu, quyết định xử lý xung đột (Team Charter mục 8.1) và kết quả duyệt Change Request (`CR-xx`).
- Quyết định chốt trên Discord hoặc trong cuộc họp phải được cập nhật vào bảng này và vào issue/tài liệu liên quan trong vòng **24 giờ**.

## Danh sách quyết định (Mô phỏng)

| Mã | Ngày | Nội dung quyết định | Người quyết định | Lý do / căn cứ | Tài liệu / Issue cần cập nhật | Trạng thái |
|---|---|---|---|---|---|---|
| `DL-01` | 18/09/2026 | Chốt phạm vi baseline 42 FR / 339 AC và tiêu chí chất lượng sau buổi demo prototype `MS-01` (`Mô phỏng`) | Aurora University (Director of Student Services) & Lê Hoàng Tùng | Hai bên xác nhận đủ luồng 3 cổng trên prototype; giữ nguyên giá 424.721.000 VNĐ và mốc Go-live 22/12/2026 | Đóng `#114`; gắn tag `baseline-v2.0` cho permalink đặc tả | Đã thực hiện |
| `DL-02` | 17/09/2026 | Phê duyệt phương án ngăn xếp công nghệ và bộ nhớ đệm phía server TTL 15 phút theo `ADR-005` (`Mô phỏng`) | BE Dev / Senior, FE Dev / Senior, DevOps / Mid; bộ phận CNTT trường xác nhận hạ tầng | Tương thích Docker Compose trên server Linux của trường (`A-11`, `A-14`), không phát sinh tăng effort/giá | Cập nhật trạng thái `ADR-005`, đóng `#105`, `#113` | Đã thực hiện |
| `DL-03` | 16/10/2026 | Xác nhận sẵn sàng test trọn luồng MS-02 (Mô phỏng) | PM / Senior, QA / Senior | Xong phần phát triển và dùng chung theo WBS; test 19–20/10, sửa 21–22/10 và kiểm lại 23/10 | Đối chiếu #126 và ghi kết quả vào báo cáo milestone | Đã thực hiện (Mô phỏng) |
| `DL-04` | 27/11/2026 | Ghi nhận ngoại lệ quy trình: 12 hạng mục rà soát `V1-01` đến `V3-03` được xử lý chung trong PR #172 và gộp khi chưa có `Approve`, trái Team Charter mục 9 (mỗi hạng mục một nhánh, một PR, liên kết issue, ít nhất 1 `Approve`). Không hoàn tác PR #172 | Lê Hoàng Tùng (trưởng nhóm) | Nội dung #172 đã được kiểm lại theo điều kiện "Hoàn thành khi" của từng hạng mục; hoàn tác chỉ làm lịch sử khó đọc hơn. Từ PR #173 mọi thay đổi đi PR riêng có `Approve`; hạng mục còn dở xử lý bằng PR hoặc cập nhật issue riêng có mã hạng mục | Mở issue theo dõi riêng cho từng hạng mục `V1-01` đến `V3-03`, bổ sung liên kết các issue này vào mô tả PR #172; đóng issue `V1-05` bằng PR ghi nhận này | Đã thực hiện |
