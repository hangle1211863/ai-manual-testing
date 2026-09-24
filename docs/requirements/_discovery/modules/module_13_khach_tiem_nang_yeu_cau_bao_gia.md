# 13 — Khách tiềm năng · Yêu cầu báo giá

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** cùng là kênh tiếp nhận yêu cầu từ bên ngoài, cùng cấu trúc bảng (`Assigned` · `Status` · `Tags`), không có module 🔴. Gộp file **không** gộp prefix.

## Leads — `LEAD`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Leads` | UI thực tế |
| Route | `/admin/leads` | UI thực tế |
| Loại màn hình | Danh sách | DOM |
| Nút thanh công cụ | `New Lead` · icon (nghi đổi chế độ Kanban) · `Export` · `Bulk Actions` · `Reload` | DOM |
| Cột bảng danh sách | `-` · `#` · `Name` · `Company` · `Email` · `Phone` · `Value` · `Tags` · `Assigned` · `Status` · `Source` · `Last Contact` · `Created` | DOM |
| CRUD | Tạo · Bulk Actions · Export | DOM |
| Status flow | Có cột `Status` — danh sách trạng thái ❔ (cấu hình thuộc `SETUP`) | DOM |
| Ước lượng độ lớn | 12 cột · trạng thái · nguồn · phân công · chuyển thành khách hàng (❔) | — |
| Risk | 🟡 — dữ liệu cá nhân (email, phone) nhưng chưa phải khách hàng | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/leads/table` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Danh mục `Status`, `Source` (cấu hình thuộc `SETUP`, bị cấm)
- Chuyển Lead → Customer, gửi Proposal cho Lead
- Chế độ Kanban

---

## Estimate Request — `ESTREQ`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Estimate Request` | UI thực tế |
| Route | `/admin/estimate_request` | UI thực tế |
| Loại màn hình | Danh sách yêu cầu + form builder (`New Form`) | DOM |
| Nút thanh công cụ | `New Form` · `Export` · `Reload` | DOM |
| Cột bảng danh sách | `#` · `Email` · `Tags` · `Assigned` · `Status` · `Created` | DOM |
| CRUD | Tạo form · Xem · Export | DOM |
| Status flow | Có cột `Status` — ❔ | DOM |
| Ước lượng độ lớn | 6 cột · form builder · nhúng form công khai (❔) | — |
| Risk | 🟢 — tiếp nhận thông tin, không tác động tiền hay dữ liệu khách hàng hiện có | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/estimate_request/table` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Form builder và URL form công khai
- Yêu cầu được chuyển thành Lead / Estimate không — ❔

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
