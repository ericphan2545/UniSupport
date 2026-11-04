# Yêu cầu phi chức năng

Các yêu cầu về chất lượng vận hành của UniSupport. Quy tắc của từng chức năng nằm trong `modules/`, cách dựng hệ thống nằm trong `architecture/`, kế hoạch và kịch bản kiểm thử nằm trong `acceptance/`.

| Tài liệu | Nội dung |
|---|---|
| [performance.md](performance.md) | Quy mô, mục tiêu hiệu năng, timeout, polling, cache, giới hạn dữ liệu, test hiệu năng |
| [security.md](security.md) | Mức bảo mật, cách kiểm chứng, rate limit |
| [reliability.md](reliability.md) | Toàn vẹn dữ liệu, tác vụ nền, hành vi khi lỗi, lưu trữ dữ liệu |
| [usability.md](usability.md) | Song ngữ, responsive, trình duyệt, usability test |
| [observability.md](observability.md) | Mã truy vết, log vận hành, nhật ký hệ thống |
| [deployment.md](deployment.md) | Dữ liệu ban đầu, triển khai production, trách nhiệm sau bàn giao |

Mã kịch bản trong cột Kiểm chứng (E2E-xx, X-xx) nằm ở `acceptance/test-scenarios.md`.

Đổi một ngưỡng trong các tài liệu này (timeout, cache, giới hạn dữ liệu, rate limit) phải ghi lại và xử lý như một thay đổi theo Kế hoạch quản trị và giám sát mục 5.
