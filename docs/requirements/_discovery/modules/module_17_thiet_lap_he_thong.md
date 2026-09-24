# Khám phá module — Thiết lập hệ thống (`SETUP`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> ⏸️ **Hoãn** — user chốt 2026-09-15. Prefix giữ nguyên.

## `SETUP` — Setup

| Mục | Giá trị |
|---|---|
| Tên trên UI | Setup (panel `#setup-menu`) |
| Bí danh | Thiết lập hệ thống · cấu hình |
| Route | ❔ Panel không có mục con. URL chỉ-đọc đã thử: `/admin/staff` · `/admin/roles` · `/admin/settings` · `/admin/custom_fields` · `/admin/emails` · `/admin/modules` |
| Loại màn hình | ❔ Không truy cập được |
| Kết quả thử truy cập | `staff` · `roles` · `settings` · `custom_fields` · `emails` → chuyển tới `/admin/access_denied` (`200`), nội dung: *"Something went wrong. Try again"* · `modules` → chuyển về `/admin/` (Dashboard) |
| CRUD · Status flow · Tab | ❔ |
| Ước REQ | ❔ |
| Risk | ❔ — nhiều module phụ thuộc cấu hình ở đây (Payment Modes, Contract Types, Expense Categories, Lead Sources/Statuses, Ticket Departments/Services, Customer Groups, Roles, Staff) |

### Tầng network

| Request | Ghi chú |
|---|---|
| `GET /admin/access_denied` · `200` | Đích chuyển hướng của mọi URL Setup bị cấm |

### Vùng chưa xác minh

- Toàn bộ module. Cần tài khoản có quyền Setup (Q1 ở index).
- Vì sao `/admin/modules` hành xử khác các URL Setup còn lại.
- Trang cấm không nói rõ là thiếu quyền — xác nhận đây là hành vi mong muốn.

### Evidence (2026-09-15, số liệu DOM)

- `#setup-menu` chỉ có 1 `<li>` gồm nút đóng + tiêu đề "Setup".
- 5 URL → `location.pathname` = `/admin/access_denied`, `.content` innerText = `Something went wrong. Try again` · 1 URL → `/admin/`.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [setup_access_denied_viewport.png](../evidence/setup_access_denied_viewport.png) | `/admin/settings` → `/admin/access_denied` | Bị chặn | Toàn bộ nội dung chỉ là dòng "Something went wrong. Try again" — **không** nói là thiếu quyền; sidebar vẫn hiển thị bình thường |
