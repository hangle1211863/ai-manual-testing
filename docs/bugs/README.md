# Danh mục Bug Report — Perfex CRM (Anh Tester Demo)

| Mục | Giá trị |
|---|---|
| Hệ thống | Perfex CRM demo — `https://crm.anhtester.com` |
| Quy ước mã bug | `BUG_<module>_<timestamp>_<TC_ID>` |
| Đường dẫn file | `docs/bugs/<module>/<nền-tảng>/BUG_<module>_<timestamp>_<TC_ID>.md` |
| Ngày cập nhật | 24-09-2026 |

## 1. Danh mục bug

| Mã bug | Module | Nền tảng | Tiêu đề ngắn | Severity | Priority | Trạng thái | TC liên quan | Ngày phát hiện |
|---|---|---|---|---|---|---|---|---|
| [BUG_login_1790253180_TC039](login/web/BUG_login_1790253180_TC039.md) | `LOGIN` | `web` | Trang đăng nhập không ép chuyển HTTP sang HTTPS, thiếu HSTS | 🟠 Major | P1 | 🔴 **Đang mở** | `CRM_LOGIN_TC_039` | 20-08-2026 |
| [BUG_login_1790253180_TC029](login/web/BUG_login_1790253180_TC029.md) | `LOGIN` | `web` | Quên mật khẩu — ô Email không trở về rỗng sau khi báo "Email not found" | 🟢 Trivial | P3 | 🔴 **Đang mở** | `CRM_LOGIN_TC_029` | 24-09-2026 |
