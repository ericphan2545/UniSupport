# Kiến trúc hệ thống

Mô tả cách UniSupport được dựng để đáp ứng các chức năng trong `modules/` và các quy tắc chung về quyền, dữ liệu, tệp, nhật ký.

| Tài liệu | Nội dung |
|---|---|
| [system-context.md](system-context.md) | Hệ thống nằm ở đâu: người dùng, hệ thống bên ngoài, ranh giới phạm vi |
| [architecture-overview.md](architecture-overview.md) | Các thành phần, luồng dữ liệu chính, môi trường triển khai |
| [data-model.md](data-model.md) | ERD và các bảng dữ liệu |
| [api-design.md](api-design.md) | Quy ước API và danh sách endpoint |
| [security-design.md](security-design.md) | Phiên, phân quyền, phạm vi dữ liệu, bảo vệ tệp, nhật ký, rate limit |
| [technical-decisions/](technical-decisions/README.md) | Các quyết định kỹ thuật (ADR) |

**Nguồn:** `modules/M04-authentication-security/prd.md` (quy tắc quyền, bảo mật, mã lỗi) và `modules/M05-core-platform/prd.md` (quy tắc dữ liệu, version, idempotency, lưu tệp). Khi tài liệu này và module mâu thuẫn, module là chuẩn.

**Trạng thái lựa chọn công nghệ:** **Chấp nhận** ngày 17/09/2026 (Mô phỏng) tại [ADR-005](technical-decisions/ADR-005-technology-stack-and-cache.md): TypeScript/NestJS, React và TypeScript, PostgreSQL, Redis cho cache 15 phút, kho tệp là volume Docker ngoài web root. `BE Dev / Senior`, `FE Dev / Senior` và `DevOps / Mid` đề xuất ở **WBS `1.04–1.05` (`09/09–11/09/2026`, `#105`)**; bộ phận CNTT của trường xác nhận tương thích hạ tầng ở **WBS `1.11–1.14` (`14/09–17/09/2026`)**. Lựa chọn nằm trong giả định ước lượng **`A-14`** ([assumptions.md](../project/assumptions.md)); rủi ro `R-27` đã đóng.

Sơ đồ viết bằng Mermaid, hiển thị trực tiếp trên GitHub.
