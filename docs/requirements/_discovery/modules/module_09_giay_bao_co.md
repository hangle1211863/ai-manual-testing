# 09 — Giấy báo có

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Credit Notes — `CRN`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Credit Notes` | UI thực tế |
| Route | Danh sách `/admin/credit_notes` · Tạo mới `/admin/credit_notes/credit_note` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + tài liệu bán hàng | DOM |
| Nút thanh công cụ | `New Credit Note` · icon · `Toggle Table` · `Export` · `Reload` | DOM |
| Cột bảng danh sách | `Credit Note #` · `Credit Note Date` · `Customer` · `Status` · `Project` · `Reference #` · `Amount` · `Remaining Amount` | DOM |
| CRUD | Tạo · Xem · Export | DOM |
| Status flow | Có cột `Status` — danh sách trạng thái ❔ chưa thấy | DOM |
| Ước lượng độ lớn | 8 cột · số dư còn lại · áp vào hoá đơn | — |
| Risk | 🔴 — tiền · số dư `Remaining Amount` trừ dần | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/credit_notes/table` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Danh sách trạng thái (bảng danh sách không có bản ghi mẫu để đọc badge)
- Cơ chế áp Credit Note vào Invoice và hoàn tiền — ❔

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
