# Hồ sơ quản trị dự án UniSupport

Thư mục này tập hợp các **mẫu nhật ký quản trị chuẩn trong repo** và **bảng định vị tập trung** mọi công cụ theo dõi dự án (GitHub Issues, Milestones, Bảng theo dõi giờ, Decision Log, Biên bản họp và Báo cáo định kỳ) để ban quản lý và đội thực thi tìm và sử dụng ngay từ [mục lục tài liệu](../../README.md) hoặc [Kế hoạch quản trị và giám sát](../03-quan-tri-giam-sat.md).

> **Lưu ý dữ liệu mẫu:** Khách hàng Aurora University, đội thực thi 10 vai trò và mọi số liệu ví dụ trong các tệp mẫu dưới đây đều là **Mô phỏng**.

## Danh mục hồ sơ, đường dẫn, người quản lý và quyền xem

| Hồ sơ / Công cụ | Nguồn chính thức & đường dẫn | Trạng thái xác minh | Người quản lý | Quyền xem / ghi |
|---|---|---|---|---|
| **GitHub Issues (Task, Bug, Feedback, CR)** | [github.com/ericphan2545/UniSupport/issues](https://github.com/ericphan2545/UniSupport/issues) | **Đã xác minh** (truy cập công khai) | Phan Tiến Thịnh | Công khai xem; 4 thành viên ban quản lý có quyền ghi/gán nhãn |
| **GitHub Milestones (`MS-01`–`MS-06`, `Hotfix`)** | [github.com/ericphan2545/UniSupport/milestones](https://github.com/ericphan2545/UniSupport/milestones) | **Đã xác minh** (truy cập công khai) | Phan Tiến Thịnh | Công khai xem; ban quản lý & PM / Senior quản lý đóng mốc |
| **Bảng tiến độ** | [GitHub Issues](https://github.com/ericphan2545/UniSupport/issues), trường Hạn và bộ lọc theo [mục 3.1](../03-quan-tri-giam-sat.md#31-ngưỡng-cảnh-báo) | Công khai | Phan Tiến Thịnh | Công khai xem; ban quản lý cập nhật |
| **Bảng theo dõi giờ công** | [time-tracking-log.md](time-tracking-log.md) (số liệu `Mô phỏng`) | **Nơi lưu chính thức** (trong repo, lịch sử qua Git) | `PM / Senior` (ban quản lý: Lê Hoàng Tùng) | Đội thực thi ghi giờ hằng ngày trước 21h30; PM và ban quản lý xem toàn bộ |
| **Decision Log** | [decision-log.md](decision-log.md) (số liệu `Mô phỏng`) | **Nơi lưu chính thức** (trong repo, lịch sử qua Git) | `PM / Senior` (ban quản lý: Lê Hoàng Tùng) | Toàn bộ nhóm được xem; chỉ PM và ban quản lý ghi nhận quyết định |
| **Biên bản họp tuần & milestone** | [meeting-minutes-template.md](meeting-minutes-template.md) (mẫu và ví dụ `Mô phỏng`); mỗi biên bản thật lưu thành một file mới trong thư mục này | **Nơi lưu chính thức** (trong repo, lịch sử qua Git) | Thành viên ghi theo lượt; `PM / Senior` kiểm duyệt | Toàn bộ nhóm được xem; thư ký phiên họp cập nhật trong 24 giờ sau họp |
| **Sổ rủi ro (`R-01` đến `R-27`)** | [risk-register.md](../risk-register.md) (nơi gốc ngay trong repo) | **Đã xác minh** (trong repo) | Phan Tiến Thịnh | Công khai xem; cập nhật qua PR sau họp tuần |
| **Báo cáo tuần / milestone / tổng kết** | Mẫu tại [03-quan-tri-giam-sat.md](../03-quan-tri-giam-sat.md) mục 6.2; bản đã gửi khách lưu thành file trong thư mục này | **Nơi lưu chính thức** (trong repo, lịch sử qua Git) | `PM / Senior` (ban quản lý: Lê Hoàng Tùng) | Ban quản lý xem toàn bộ; bản tóm tắt gửi Aurora University qua email tối thiểu 1 lần/tuần |

## Cách cập nhật

- Các file trong thư mục này là nơi lưu chính thức của bảng giờ, Decision Log, biên bản họp và báo cáo; dự án không dùng nơi lưu ngoài repo cho các hồ sơ này.
- Người quản lý cập nhật qua PR theo [Team Charter](../team-charter.md) mục 9; ai có quyền xem repo đều xem được toàn bộ hồ sơ.
