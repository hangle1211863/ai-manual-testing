# 03 — Liên hệ

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Contacts — `CTC`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Contacts` | UI thực tế |
| Route | ❔ Nút `Contacts` trên danh sách Customers — **chưa mở**, chưa biết route · Tab `Contacts` ở `/admin/clients/client/{id}` | UI thực tế |
| Loại màn hình | ❔ Danh sách (dự kiến) + tab trong chi tiết Customer | Nút + tab quan sát được |
| Dấu hiệu là entity riêng | Nút riêng trên thanh công cụ Customers · khối tổng hợp đếm `Active Contacts` / `Inactive Contacts` / `Contacts Logged In Today` (contact **đăng nhập được**) · cột `Primary Contact` ở Customers · cột `Contact` ở Tickets | UI thực tế |
| CRUD | ❔ Chưa quan sát | — |
| Status flow | Cờ active/inactive (suy từ khối tổng hợp) | UI thực tế |
| Ước lượng độ lớn | ❔ | — |
| Risk | 🔴 — dữ liệu cá nhân · là tài khoản đăng nhập phía khách hàng · Tickets phụ thuộc | — |

> **Ranh giới:** tách khỏi `CUST` vì Contact có vòng đời riêng (active/inactive, đăng nhập). Không gộp chung file với `CUST` vì cả hai đều 🔴.

### Network

Chưa ghi nhận.

### Vùng chưa xác minh

- Route và giao diện trang danh sách Contacts — **chưa mở**
- Form tạo/sửa contact, phân quyền contact trên client portal
- Cơ chế đăng nhập của contact (thuộc client portal — ngoài phạm vi đợt này, cần làm rõ ranh giới khi recon)

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
