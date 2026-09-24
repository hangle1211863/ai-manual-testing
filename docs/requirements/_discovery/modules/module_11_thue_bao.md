# 11 — Thuê bao

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Subscriptions — `SUB`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Subscriptions` | UI thực tế |
| Route | Danh sách `/admin/subscriptions` · Tạo mới `/admin/subscriptions/create` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + khối `Subscriptions Summary` | DOM |
| Nút thanh công cụ | `New Subscription` · icon · `Export` · `Reload` | DOM |
| Cột bảng danh sách | `#` · `Subscription Name` · `Customer` · `Project` · `Status` · `Next Billing Cycle` · `Date Subscribed` · `Last Sent` | DOM |
| CRUD | Tạo · Xem · Export | DOM |
| Status flow | Có cột `Status` — danh sách trạng thái ❔ | DOM |
| Ước lượng độ lớn | 8 cột · chu kỳ thanh toán · gửi cho khách | — |
| Risk | 🟡 — thu tiền định kỳ, nhưng phụ thuộc cổng thanh toán bên ngoài (❔) | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/subscriptions/table` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Danh sách trạng thái, khối Summary gồm những gì
- Tích hợp cổng thanh toán (nghi Stripe) — cấu hình thuộc `SETUP`, bị cấm
- Subscription có sinh Invoice tự động không — ❔

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
