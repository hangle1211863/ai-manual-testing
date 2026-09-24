# Khám phá module — Hàng hoá / dịch vụ (`ITEM`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `ITEM` — Items

| Mục | Giá trị |
|---|---|
| Tên trên UI | Items (menu Sales ▸ Items) · `document.title` = `Invoice Items` |
| Bí danh | Hàng hoá · dịch vụ · Invoice Items |
| Route | `/admin/invoice_items` |
| Loại màn hình | Danh sách + Import + quản lý nhóm |
| Nút thanh công cụ | New Item · Import Items · Groups · Export · Bulk Actions |
| Cột bảng (8) | checkbox · Description · Long Description · Rate · Tax 1 · Tax 2 · Unit · Group Name |
| CRUD | Tạo ✅ (nút) · Import ✅ · Bulk Actions ✅ · Sửa/Xoá ❔ (chưa soi dòng) |
| Status flow | Không có |
| Ước REQ | 20–30 |
| Risk | 🟡 — ảnh hưởng giá và thuế trên chứng từ bán hàng, nhưng là danh mục CRUD thường |

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Form New Item (modal?) · định dạng file Import · màn hình Groups.
- Item được chọn vào Proposal/Estimate/Invoice/Credit Note như thế nào (❔ suy từ vị trí menu).

### Evidence (2026-09-15, số liệu DOM)

- 5 nút · 8 cột (`thead th`).

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [items_list_viewport.png](../evidence/items_list_viewport.png) | Items — danh sách | Mặc định, menu Sales đang mở | Nút New Item · Import Items · Groups · Export · Bulk Actions · 8 cột gồm Rate, Tax 1, Tax 2, Unit |
