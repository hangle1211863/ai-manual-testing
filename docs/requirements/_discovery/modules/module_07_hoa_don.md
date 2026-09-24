# Khám phá module — Hoá đơn (`INV`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `INV` — Invoices

| Mục | Giá trị |
|---|---|
| Tên trên UI | Invoices (menu Sales ▸ Invoices) |
| Bí danh | Hoá đơn |
| Route | Danh sách `/admin/invoices` · tạo `/admin/invoices/invoice` · sửa `/admin/invoices/invoice/{id}` · xem `/admin/invoices/list_invoices/{id}` · lọc `list_invoices?status={1,2,3,4,6}` · `list_invoices?filter=not_sent` |
| Loại màn hình | Danh sách + khung xem chứng từ (split view) · form tạo dài |
| Nút thanh công cụ | Create New Invoice · Batch Payments · Recurring Invoices · Filter by status · Export |
| Cột bảng (9) | Invoice # · Amount · Total Tax · Date · Customer · Project · Tags · Due Date · Status |
| CRUD | Tạo ✅ · Xem ✅ · Sửa ✅ (link) · Xoá ❔ |
| Status flow | Có — **6 trạng thái** đọc từ widget "Invoice overview" trên Dashboard: `Draft` · `Not Sent` · `Unpaid` · `Partially Paid` · `Overdue` · `Paid`. Ngoài ra trang Sales Reports ghi *"Cancelled invoices are excluded from the report"* → còn trạng thái **Cancelled**. Ánh xạ sang mã `status=1,2,3,4,6` ❔ chưa xác minh |
| Form tạo mới | **63 control** · bắt buộc: `* Customer` · `* Invoice Number` · `* Invoice Date` · `* Currency` |
| Khung xem chi tiết | Nút: Export · More · Payment · Tab (6): Invoice · Payments · Tasks · Activity Log · Reminders · Notes |
| Ước REQ | 50–75 |
| Risk | 🔴 — tiền, thuế, tiền tệ; hoá đơn định kỳ; thanh toán hàng loạt; Payment và Expense phụ thuộc |

### Tầng network

| Request | Ghi chú |
|---|---|
| `POST /admin/invoices/table` · `200` | Tải bảng Invoices |

### Vùng chưa xác minh

- Ánh xạ mã trạng thái ↔ tên; trạng thái 5 không có link trên Dashboard (❔ bị ẩn hay không tồn tại).
- Batch Payments · Recurring Invoices · menu More (gửi mail, nhân bản, xoá…).
- Cách chọn Item vào dòng hoá đơn; tính thuế, giảm giá, điều chỉnh.
- Link công khai `/invoice/{id}/<32 ký tự hex>` — thuộc client portal, ngoài phạm vi.

### Evidence (2026-09-15, số liệu DOM)

- 5 nút · 9 cột · form 63 control, 4 nhãn bắt buộc · chi tiết 3 nút + 6 tab.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [invoices_list_viewport.png](../evidence/invoices_list_viewport.png) | Invoices — danh sách | Mặc định, 6 bản ghi | Nút Create New Invoice · Batch Payments · Recurring Invoices · Filter by status · 9 cột · nhãn trạng thái Paid và Unpaid |
