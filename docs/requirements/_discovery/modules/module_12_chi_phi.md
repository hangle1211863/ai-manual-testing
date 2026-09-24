# Khám phá module — Chi phí (`EXP`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `EXP` — Expenses

| Mục | Giá trị |
|---|---|
| Tên trên UI | Expenses |
| Bí danh | Chi phí |
| Route | Danh sách `/admin/expenses` · tạo `/admin/expenses/expense` |
| Loại màn hình | Danh sách + Import |
| Nút thanh công cụ | Record Expense · Import Expenses · Export · Bulk Actions |
| Cột bảng (11) | checkbox · Category · Amount · Name · Receipt · Date · Project · Customer · Invoice · Reference # · Payment Mode |
| CRUD | Tạo ✅ (Record Expense) · Import ✅ · Bulk Actions ✅ · Sửa/Xoá ❔ |
| Status flow | ❔ Không có cột Status; cột `Invoice` gợi ý chi phí có thể được lập hoá đơn cho khách |
| Dữ liệu hiện tại | ⚠️ `No entries found` — bảng rỗng (ảnh 2026-09-18) |
| Ước REQ | 25–40 |
| Risk | 🟡 — tiền; đính kèm biên lai (upload file); liên kết Project/Customer/Invoice |

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Expense Category cấu hình ở đâu (❔ Setup).
- Luồng chuyển chi phí thành Invoice; chi phí định kỳ.
- Định dạng file Import.

### Evidence (2026-09-15, số liệu DOM)

- 4 nút · 11 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [expenses_list_viewport.png](../evidence/expenses_list_viewport.png) | Expenses — danh sách | **Rỗng** — No entries found | Nút Record Expense · Import Expenses · Bulk Actions · 11 cột gồm Receipt, Invoice, Payment Mode |
