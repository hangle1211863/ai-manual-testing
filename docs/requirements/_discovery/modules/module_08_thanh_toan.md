# Khám phá module — Thanh toán (`PAY`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `PAY` — Payments

| Mục | Giá trị |
|---|---|
| Tên trên UI | Payments (menu Sales ▸ Payments) |
| Bí danh | Thanh toán · khoản thu |
| Route | `/admin/payments` |
| Loại màn hình | Danh sách |
| Nút thanh công cụ | Export — **không có nút tạo** |
| Cột bảng (7) | Payment # · Invoice # · Payment Mode · Transaction ID · Customer · Amount · Date |
| CRUD | Xem ✅ · Tạo: ❔ qua nút `Payment` ở chi tiết Invoice hoặc `Batch Payments` · Sửa/Xoá ❔ |
| Status flow | Không thấy cột trạng thái |
| Ước REQ | 15–25 |
| Risk | 🔴 — tiền; ảnh hưởng trạng thái thanh toán của Invoice; Payment Mode cấu hình ở Setup (bị chặn) |

### Ranh giới

Không gộp với `INV` dù phụ thuộc chặt — cả hai đều 🔴, mỗi module một file để soi kỹ.

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Màn hình chi tiết / biên nhận thanh toán.
- Danh sách Payment Mode (cấu hình ở Setup — bị chặn).

### Evidence (2026-09-15, số liệu DOM)

- 1 nút · 7 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [payments_list_viewport.png](../evidence/payments_list_viewport.png) | Payments — danh sách | Mặc định, 1 bản ghi | **Không có nút tạo** — chỉ Export · 7 cột · cột Invoice # trỏ về hoá đơn nguồn, Payment Mode = Bank |
