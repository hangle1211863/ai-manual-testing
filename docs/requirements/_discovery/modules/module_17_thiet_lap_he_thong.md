# 17 — Thiết lập hệ thống

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Setup — `SETUP`

> ⛔ **BLOCKED** — tài khoản dùng khảo sát không có quyền. Prefix đã cấp và giữ nguyên.

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Setup` — panel `#setup-menu` chỉ có tiêu đề và nút đóng, **không có mục con** | DOM |
| Route đã thử (chỉ đọc) | `/admin/staff` · `/admin/roles` · `/admin/settings` · `/admin/custom_fields` · `/admin/emails` · `/admin/clients/groups` → đều chuyển tới `/admin/access_denied` | UI thực tế · network |
| Route bất thường | `/admin/modules` → chuyển về `/admin/` (Dashboard), **không** qua `access_denied` | UI thực tế · network |
| Loại màn hình | ❔ Cấu hình | — |
| Ước lượng độ lớn | ❔ | — |
| Risk | ❔ — nhưng chứa cấu hình mà **nhiều module khác phụ thuộc** (xem bên dưới) | — |

### Module khác đang bị chặn một phần vì thiếu quyền Setup

| Module | Cấu hình thiếu |
|---|---|
| `CUST` | Customer Groups · danh sách staff cho `Customer Admins` |
| `ITEM` · `INV` · `EST` · `PROP` | Thuế (`Tax 1`, `Tax 2`) · tiền tố số chứng từ |
| `PAY` · `EXP` | `Payment Mode` · `Category` chi phí |
| `CTR` | `Contract Type` · ngưỡng "About to Expire" |
| `LEAD` | Trạng thái · nguồn lead |
| `TKT` | `Department` · `Service` · `Status` · `Priority` |
| `SUB` | Cổng thanh toán |
| Mọi module | Danh sách role → ma trận phân quyền |

### Vùng chưa xác minh

- Toàn bộ nội dung Setup
- Việc thiếu quyền là **có chủ đích** (bản demo khoá cấu hình) hay tài khoản cần được nâng quyền — câu hỏi Q1 ở [system_map.md mục 6](../system_map.md#6-thứ-tự-khảo-sát-đã-chốt)
- Nội dung thông báo trên trang `access_denied` chưa đọc được (trang chuyển hướng tiếp)

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
