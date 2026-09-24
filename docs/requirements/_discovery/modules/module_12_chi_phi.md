# 12 — Chi phí

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Expenses — `EXP`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Expenses` | UI thực tế |
| Route | Danh sách `/admin/expenses` · Tạo mới `/admin/expenses/expense` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách | DOM |
| Nút thanh công cụ | `Record Expense` · `Import Expenses` · icon · `View Quick Stats` · `Toggle Table` · `Export` · `Bulk Actions` · `Reload` | DOM |
| Cột bảng danh sách | `-` (checkbox) · `Category` · `Amount` · `Name` · `Receipt` · `Date` · `Project` · `Customer` · `Invoice` · `Reference #` · `Payment Mode` | DOM |
| CRUD | Tạo (`Record Expense`) · Import · Bulk Actions · Export | DOM |
| Status flow | Không có cột trạng thái | DOM |
| Ước lượng độ lớn | 10 cột · đính kèm biên lai (`Receipt`) · Import · liên kết Invoice | — |
| Risk | 🟡 — số tiền chảy vào `Reports ▸ Expenses` và `Expenses vs Income` | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/expenses/table` · `200` | Tải bảng danh sách |
| `POST /admin/expenses/get_expenses_total` · `200` | Tổng chi phí cho khối Quick Stats |

### Vùng chưa xác minh

- Cột `Invoice` — chi phí được tính lại cho khách qua hoá đơn? ❔
- Danh mục `Category` (cấu hình thuộc `SETUP`, bị cấm)
- Upload biên lai, chi phí định kỳ

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
