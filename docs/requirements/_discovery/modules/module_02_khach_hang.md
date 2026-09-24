# 02 — Khách hàng

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Customers — `CUST`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Customers` | UI thực tế |
| Route | Danh sách `/admin/clients` · Tạo mới `/admin/clients/client` · Chi tiết `/admin/clients/client/{id}` | UI thực tế |
| Loại màn hình | Danh sách + khối tổng hợp + chi tiết nhiều tab | UI thực tế |
| Nút thanh công cụ | `New Customer` · `Import Customers` · `Contacts` · `Export` · `Bulk Actions` · nút lọc (icon) | UI thực tế |
| Khối tổng hợp | `Total Customers` · `Active Customers` · `Inactive Customers` · `Active Contacts` · `Inactive Contacts` · `Contacts Logged In Today` | UI thực tế |
| Cột bảng danh sách | `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` (công tắc bật/tắt) · `Groups` · `Date Created` | UI thực tế |
| CRUD | Tạo (`New Customer`) · Xem · Import · Export · Bulk Actions. Sửa/xoá: chưa quan sát | UI thực tế |
| Status flow | Không có status flow — chỉ cờ `Active` bật/tắt | UI thực tế |
| Tab chi tiết (19) | `Profile` · `Contacts` · `Notes` · `Statement` · `Invoices` · `Payments` · `Proposals` · `Credit Notes` · `Estimates` · `Subscriptions` · `Expenses` · `Contracts` · `Projects` · `Tasks` · `Tickets` · `Files` · `Vault` · `Reminders` · `Map` | UI thực tế · DOM |
| Tab con của `Profile` (3) | `Customer Details` · `Billing & Shipping` · `Customer Admins` | UI thực tế · DOM |
| Ước lượng độ lớn | 8 cột · 19 tab (phần lớn là danh sách lọc theo khách của module khác) · 3 tab form Profile · Import | — |
| Risk | 🔴 — dữ liệu khách hàng · entity nền của ≥ 11 module · Import hàng loạt | — |

> **Ranh giới:** các tab `Invoices`, `Projects`, `Tickets`… là **góc nhìn** lọc theo khách hàng của module tương ứng, không phải entity riêng của `CUST`. Tab thuộc riêng `CUST`: `Profile`, `Notes`, `Statement`, `Files`, `Vault`, `Reminders`, `Map`. `Contacts` tách thành module `CTC`.

### Network

Chưa ghi nhận — trang được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Form tạo/sửa chưa mở đầy đủ — mới thấy các nhãn đầu của `Customer Details` (`Company` bắt buộc, `VAT Number`, `Phone`, `Website`, `Groups`)
- `Groups` là danh mục cấu hình — trang `/admin/clients/groups` bị cấm (thuộc `SETUP`)
- `Vault` (nghi lưu thông tin nhạy cảm), `Statement`, `Map` chưa mở
- Luồng Import (định dạng file, validation) chưa mở
- Hành vi công tắc `Active` — **không** thử vì môi trường dùng chung
- `Customer Admins` — phân công nhân viên phụ trách, cần danh sách staff (bị cấm ở `SETUP`)

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
