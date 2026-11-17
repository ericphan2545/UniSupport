# Yêu cầu phi chức năng

Các yêu cầu về chất lượng vận hành của UniSupport. Mỗi yêu cầu có mã `NFR-…`, ngưỡng đo và cách đo; cột "Cách đo" dẫn tới kịch bản test `TS-xx` trong [test-scenarios.md](../acceptance/test-scenarios.md) hoặc mã AC trong `modules/`. Quy tắc của từng chức năng nằm trong `modules/`, cách dựng hệ thống trong `architecture/`, kế hoạch kiểm thử trong `acceptance/`.

| Tài liệu | Mã | Nội dung |
|---|---|---|
| [performance.md](performance.md) | `NFR-PERF-01` đến `NFR-PERF-14` | Quy mô, mục tiêu hiệu năng, timeout, polling, cache, giới hạn dữ liệu, test hiệu năng |
| [security.md](security.md) | `NFR-SEC-01` đến `NFR-SEC-13` | Mức bảo mật, yêu cầu và cách kiểm chứng, rate limit |
| [reliability.md](reliability.md) | `NFR-REL-01` đến `NFR-REL-10` | Toàn vẹn dữ liệu, tác vụ nền, hành vi khi lỗi, lưu trữ dữ liệu |
| [usability.md](usability.md) | `NFR-USE-01` đến `NFR-USE-05` | Usability test, song ngữ, responsive, trình duyệt, thông báo lỗi |
| [observability.md](observability.md) | `NFR-OBS-01` đến `NFR-OBS-04` | Mã truy vết, log vận hành, nhật ký hệ thống |
| [deployment.md](deployment.md) | `NFR-DEP-01` đến `NFR-DEP-04` | Dữ liệu ban đầu, triển khai production, trách nhiệm sau bàn giao |

Đổi một ngưỡng trong các tài liệu này (timeout, cache, giới hạn dữ liệu, rate limit) phải ghi lại và xử lý như một thay đổi theo [Kế hoạch quản trị và giám sát](../management/03-quan-tri-giam-sat.md) mục 5.
