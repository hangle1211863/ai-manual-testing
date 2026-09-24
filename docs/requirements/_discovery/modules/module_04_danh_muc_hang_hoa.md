# 04 — Danh mục hàng hoá

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Items — `ITEM`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Items` (menu `Sales ▸ Items`, tiêu đề trang `Invoice Items`) | UI thực tế |
| Route | `/admin/invoice_items` | UI thực tế |
| Loại màn hình | Danh sách | UI thực tế |
| Nút thanh công cụ | `New Item` · `Import Items` · `Groups` · `Export` · `Bulk Actions` · `Reload` | DOM |
| Cột bảng danh sách | `-` (checkbox) · `Description` · `Long Description` · `Rate` · `Tax 1` · `Tax 2` · `Unit` · `Group Name` | DOM |
| CRUD | Tạo · Import · Bulk Actions · quản lý Groups | DOM |
| Status flow | Không quan sát được | — |
| Ước lượng độ lớn | 7 cột dữ liệu · form item · Import · Groups | — |
| Risk | 🟡 — CRUD thường, nhưng giá/thuế chảy vào mọi tài liệu bán hàng | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/invoice_items/table` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Trang có **2** vùng thông tin phân trang DataTables — nghi có bảng thứ hai (Groups, trong modal) chưa hiển thị
- Nguồn danh mục `Tax 1` / `Tax 2` (cấu hình thuế thuộc `SETUP` — bị cấm)
- Item có được chọn trong Proposal / Estimate / Invoice / Credit Note không — ❔ chưa xác minh

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
