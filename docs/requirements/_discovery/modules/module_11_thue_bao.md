# Khám phá module — Thuê bao (`SUB`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).

## `SUB` — Subscriptions

| Mục | Giá trị |
|---|---|
| Tên trên UI | Subscriptions |
| Bí danh | Thuê bao · gói định kỳ |
| Route | Danh sách `/admin/subscriptions` · tạo `/admin/subscriptions/create` |
| Loại màn hình | Danh sách |
| Nút thanh công cụ | New Subscription · Export |
| Cột bảng (8) | # · Subscription Name · Customer · Project · Status · Next Billing Cycle · Date Subscribed · Last Sent |
| CRUD | Tạo ✅ · Xem/Sửa/Xoá ❔ |
| Status flow | Có — **8 trạng thái** đọc từ khối "Subscriptions Summary": `Not Subscribed` · `Active` · `Future` · `Past Due` · `Unpaid` · `Incomplete` · `Canceled` · `Incomplete Expired` |
| Dữ liệu hiện tại | ⚠️ `No entries found` — bảng rỗng (ảnh 2026-09-18) |
| Ước REQ | 20–30 |
| Risk | 🟡 — thu tiền định kỳ; ❔ phụ thuộc cổng thanh toán cấu hình ở Setup |

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Form tạo — có yêu cầu cổng thanh toán (Stripe…) không; nếu chưa cấu hình thì tạo được không.
- Subscription sinh Invoice theo chu kỳ (❔).

### Evidence (2026-09-15, số liệu DOM)

- 2 nút · 8 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [subscriptions_list_viewport.png](../evidence/subscriptions_list_viewport.png) | Subscriptions — danh sách | **Rỗng** — No entries found | Khối Subscriptions Summary mang **logo Stripe** và nêu đủ **8 trạng thái** · 8 cột |
