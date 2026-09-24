# 08 — Thanh toán

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Payments — `PAY`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Payments` | UI thực tế |
| Route | `/admin/payments` | UI thực tế |
| Loại màn hình | Danh sách | DOM |
| Nút thanh công cụ | `Export` · `Reload` — **không có nút tạo mới** | DOM |
| Cột bảng danh sách | `Payment #` · `Invoice #` · `Payment Mode` · `Transaction ID` · `Customer` · `Amount` · `Date` | DOM |
| CRUD | Xem · Export. Tạo: không có trên danh sách — nghi tạo từ hoá đơn hoặc `Batch Payments` | DOM |
| Status flow | Không có cột trạng thái | DOM |
| Ước lượng độ lớn | 7 cột · form ghi nhận thanh toán (ở Invoice) · phiếu thu | — |
| Risk | 🔴 — tiền · làm thay đổi trạng thái Invoice | — |

### Network

Chưa ghi nhận — trang được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Nơi tạo Payment (từ chi tiết Invoice? `Batch Payments`?) — ❔
- Danh mục `Payment Mode` (cấu hình thuộc `SETUP`, bị cấm)
- Sửa/xoá Payment và ảnh hưởng ngược lên trạng thái Invoice

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
