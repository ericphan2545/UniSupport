# Quyết định kỹ thuật (ADR)

Mỗi quyết định ghi: bối cảnh, quyết định, hệ quả. Trạng thái **Chấp nhận** nghĩa là đã nằm trong đặc tả module; **Đề xuất** nghĩa là nhóm còn phải chốt.

| ADR | Quyết định | Trạng thái |
|---|---|---|
| [ADR-001](ADR-001-ticket-id-generation.md) | Sinh mã hồ sơ `USP-YYYY-NNNNN` bằng sequence theo năm | Chấp nhận |
| [ADR-002](ADR-002-idempotency.md) | Idempotency-Key cho các thao tác tạo và xử lý | Chấp nhận |
| [ADR-003](ADR-003-notification-processing.md) | Thông báo trong app, ghi cùng transaction, polling 30 giây | Chấp nhận |
| [ADR-004](ADR-004-ticket-claim-concurrency.md) | Optimistic locking bằng version cho thao tác đồng thời trên hồ sơ | Chấp nhận |
| [ADR-005](ADR-005-technology-stack-and-cache.md) | Ngăn xếp công nghệ và lựa chọn bộ nhớ đệm phía server (cache 15 phút, đề xuất Redis) | Đề xuất (chốt ở WBS `1.04–1.05`, xác nhận hạ tầng `1.11–1.14`) |

Thêm ADR mới khi có quyết định ảnh hưởng nhiều module hoặc khó đảo ngược, ví dụ chọn hệ quản trị CSDL hay kho tệp.
