# Khám phá module — Hỗ trợ · Cơ sở tri thức (`TKT` · `KB`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì cùng mảng chăm sóc khách hàng sau bán. **Vẫn là 2 prefix, 2 tài liệu requirements riêng.**

## `TKT` — Support

| Mục | Giá trị |
|---|---|
| Tên trên UI | Support (sidebar) · `document.title` = `Support Tickets` |
| Bí danh | Tickets · Hỗ trợ |
| Route | Danh sách `/admin/tickets` · tạo `/admin/tickets/add` |
| Loại màn hình | Danh sách |
| Nút thanh công cụ | New Ticket · Export · Bulk Actions |
| Cột bảng (11) | checkbox · # · Subject · Tags · Department · Service · Contact · Status · Priority · Last Reply · Created |
| CRUD | Tạo ✅ · Bulk Actions ✅ · Xem/Sửa/Xoá ❔ |
| Status flow | Có — **5 trạng thái** đọc từ khối "Tickets Summary": `Open` · `In Progress` · `Answered` · `On Hold` · `Closed`. Giá trị `Priority` ❔ chưa đọc được |
| Dữ liệu hiện tại | ⚠️ `Showing 0 to 0 of 0 entries` — bảng rỗng |
| Ước REQ | 30–45 |
| Risk | 🟡 — trao đổi với khách (Contact), phụ thuộc Department/Service cấu hình ở Setup |

## `KB` — Knowledge Base

| Mục | Giá trị |
|---|---|
| Tên trên UI | Knowledge Base |
| Bí danh | Cơ sở tri thức · Article |
| Route | Danh sách `/admin/knowledge_base` · tạo `/admin/knowledge_base/article` |
| Loại màn hình | Danh sách + Groups |
| Nút thanh công cụ | New Article · Groups · Export |
| Cột bảng (3) | Article Name · Group · Date Published |
| CRUD | Tạo ✅ · Groups ✅ · Xem/Sửa/Xoá ❔ |
| Status flow | ❔ Không có cột Status |
| Dữ liệu hiện tại | ⚠️ `No entries found` — bảng rỗng (ảnh 2026-09-18) |
| Ước REQ | 15–20 |
| Risk | 🟢 — nội dung tĩnh, ít phụ thuộc |

### Tầng network

| Request | Ghi chú |
|---|---|
| `POST /admin/tickets?bulk_actions=true` · `200` | Phát sinh ngay khi **tải** trang Support — cần xác nhận chỉ để tải bảng, không có tác dụng phụ |

### Vùng chưa xác minh

- **Vì sao Support 0 bản ghi** trong khi Dashboard có widget "Staff Tickets Report" (Q3 ở index).
- Department, Service, Priority, Status cấu hình ở Setup (bị chặn).
- Trả lời ticket, đính kèm, gộp ticket.
- KB: trang công khai cho khách (client portal — ngoài phạm vi).

### Evidence (2026-09-15, số liệu DOM)

- Support: 3 nút · 11 cột · `.dataTables_info` = `Showing 0 to 0 of 0 entries`.
- Knowledge Base: 3 nút · 3 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [support_tickets_list_empty_viewport.png](../evidence/support_tickets_list_empty_viewport.png) | Support — danh sách | **Rỗng** — No entries found | Khối Tickets Summary nêu đủ **5 trạng thái** · 11 cột gồm Department và Service |
| [knowledge_base_list_viewport.png](../evidence/knowledge_base_list_viewport.png) | Knowledge Base — danh sách | **Rỗng** — No entries found | Nút New Article · Groups · nút chế độ xem · 3 cột |
