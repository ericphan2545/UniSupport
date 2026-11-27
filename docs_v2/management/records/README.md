# Hồ sơ quản trị dự án UniSupport

Thư mục này tập hợp các **mẫu nhật ký quản trị chuẩn trong repo** và **bảng định vị tập trung** mọi công cụ theo dõi dự án (GitHub Issues, Milestones, Bảng theo dõi giờ, Decision Log, Biên bản họp và Báo cáo định kỳ) để ban quản lý và đội thực thi tìm và sử dụng ngay từ [mục lục tài liệu](../../README.md) hoặc [Kế hoạch quản trị và giám sát](../03-quan-tri-giam-sat.md).

> **Lưu ý dữ liệu mẫu:** Khách hàng Aurora University, đội thực thi 10 vai trò và mọi số liệu ví dụ trong các tệp mẫu dưới đây đều là **Mô phỏng**.

## Danh mục hồ sơ, quyền đọc và quyền cập nhật

**Quyền đọc:** repo UniSupport công khai, nên mọi người (ban quản lý, đội thực thi Mô phỏng, Aurora University Mô phỏng và người ngoài) đều đọc được toàn bộ hồ sơ mô phỏng trong repo, Issues và Milestones. Vì vậy hồ sơ không ghi dữ liệu cá nhân thật hay thông tin bí mật.

**Quyền cập nhật** khác nhau theo từng loại hồ sơ, như bảng dưới.

| Hồ sơ / Công cụ | Nơi lưu chính thức | Ai cập nhật nội dung | Cách cập nhật và người duyệt |
|---|---|---|---|
| **GitHub Issues (Task, Bug, Feedback, CR)** | [github.com/ericphan2545/UniSupport/issues](https://github.com/ericphan2545/UniSupport/issues) | 4 thành viên ban quản lý tạo và cập nhật issue; Phan Tiến Thịnh quản lý nhãn và mẫu issue | Sửa trực tiếp trên GitHub, không qua PR; lịch sử thay đổi lưu trong issue |
| **GitHub Milestones (`MS-01`–`MS-06`, `Hotfix`)** | [github.com/ericphan2545/UniSupport/milestones](https://github.com/ericphan2545/UniSupport/milestones) | Phan Tiến Thịnh; đóng mốc khi `PM / Senior` xác nhận đủ điều kiện (QMP mục 2.1) | Sửa trực tiếp trên GitHub, không qua PR |
| **Bảng tiến độ** | [GitHub Issues](https://github.com/ericphan2545/UniSupport/issues), trường Hạn và bộ lọc theo [mục 3.1](../03-quan-tri-giam-sat.md#31-ngưỡng-cảnh-báo) | Không cập nhật riêng: số liệu lấy từ Issues và Milestones | Không áp dụng |
| **Bảng theo dõi giờ công** | [time-tracking-log.md](time-tracking-log.md) (số liệu `Mô phỏng`) | Mỗi vai trò đội thực thi (Mô phỏng) ghi giờ của mình hằng ngày trước 21h30; `PM / Senior` tổng hợp | Lê Hoàng Tùng mở PR; Phan Tiến Thịnh duyệt |
| **Decision Log** | [decision-log.md](decision-log.md) (số liệu `Mô phỏng`) | `PM / Senior` ghi quyết định trong 24 giờ sau khi chốt | Lê Hoàng Tùng mở PR; Phan Tiến Thịnh duyệt |
| **Biên bản họp tuần & milestone** | Mẫu [meeting-minutes-template.md](meeting-minutes-template.md); mỗi biên bản lưu thành một file mới trong thư mục này | Thư ký phiên họp (thành viên ban quản lý ghi theo lượt) soạn trong 24 giờ; `PM / Senior` kiểm duyệt nội dung | Thư ký mở PR; Lê Hoàng Tùng duyệt (Phan Tiến Thịnh duyệt nếu Lê Hoàng Tùng là thư ký) |
| **Sổ rủi ro (`R-01` đến `R-27`)** | [risk-register.md](../risk-register.md) | Phan Tiến Thịnh, sau họp tuần; owner từng rủi ro báo dấu hiệu | Phan Tiến Thịnh mở PR; Lê Hoàng Tùng duyệt |
| **Báo cáo tuần / milestone / tổng kết** | Mẫu tại [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2; bản đã gửi khách lưu thành file trong thư mục này | `PM / Senior`; bản tóm tắt gửi Aurora University qua email tối thiểu 1 lần/tuần | Lê Hoàng Tùng mở PR; Phan Tiến Thịnh duyệt |

**Biên bản đã lưu (Mô phỏng):** họp đóng milestone [`MS-01`](nghiem-thu-MS-01.md), [`MS-02`](nghiem-thu-MS-02.md), [`MS-03`](nghiem-thu-MS-03.md); ví dụ họp tuần 6 ở [meeting-minutes-template.md](meeting-minutes-template.md) phần B.

## Cách cập nhật

- **Đọc:** mọi người đọc trực tiếp trên repo công khai; không cần cấp quyền.
- **Hồ sơ trong repo** (bảng giờ, Decision Log, biên bản họp, sổ rủi ro, báo cáo): người ở cột "Cách cập nhật và người duyệt" tạo nhánh `docs/...`, mở Pull Request theo [Team Charter](../team-charter.md) mục 9, người duyệt là thành viên khác tác giả, gộp bằng squash merge. Không push thẳng lên `main`.
- **Issues và Milestones:** sửa trực tiếp trên GitHub theo vai trò ở bảng trên.
- Dự án không dùng nơi lưu ngoài repo cho các hồ sơ này.
