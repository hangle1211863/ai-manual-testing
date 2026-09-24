# Khám phá module — Giấy báo có (`CRN`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `CRN` — Credit Notes

| Mục | Giá trị |
|---|---|
| Tên trên UI | Credit Notes (menu Sales ▸ Credit Notes) |
| Bí danh | Giấy báo có |
| Route | Danh sách `/admin/credit_notes` · tạo `/admin/credit_notes/credit_note` |
| Loại màn hình | Danh sách + chứng từ bán hàng |
| Nút thanh công cụ | New Credit Note · Export |
| Cột bảng (8) | Credit Note # · Credit Note Date · Customer · Status · Project · Reference # · Amount · Remaining Amount |
| CRUD | Tạo ✅ · Xem/Sửa/Xoá ❔ |
| Status flow | Có — cột `Status`; Bulk PDF Export có nhóm `Open · Closed · Void` (❔ chưa xác nhận thuộc Credit Note) |
| Dữ liệu hiện tại | ⚠️ `No entries found` — bảng rỗng (ảnh 2026-09-18) |
| Ước REQ | 25–35 |
| Risk | 🔴 — tiền; số dư `Remaining Amount` gợi ý được trừ dần vào hoá đơn |

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Luồng áp Credit Note vào Invoice, hoàn tiền.
- Tên trạng thái thật.

### Evidence (2026-09-15, số liệu DOM)

- 2 nút · 8 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [credit_notes_list_viewport.png](../evidence/credit_notes_list_viewport.png) | Credit Notes — danh sách | **Rỗng** — No entries found | Nút New Credit Note · 8 cột gồm Remaining Amount · xác nhận module chưa có dữ liệu để recon |
