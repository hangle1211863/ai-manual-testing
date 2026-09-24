# 07 — Hoá đơn

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Invoices — `INV`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Invoices` | UI thực tế |
| Route | Danh sách `/admin/invoices` · `/admin/invoices/list_invoices?status=<n>` · `?filter=not_sent` · Tạo mới `/admin/invoices/invoice` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + tài liệu bán hàng | UI thực tế |
| Nút thanh công cụ | `Create New Invoice` · `Batch Payments` · `Recurring Invoices` · `Toggle Table` · `View Quick Stats` · `Filter by status` · `Export` · `Reload` | DOM |
| Cột bảng danh sách | `Invoice #` · `Amount` · `Total Tax` · `Year` · `Date` · `Customer` · `Project` · `Tags` · `Due Date` · `Status` | DOM |
| CRUD | Tạo · Xem · Export · ghi nhận thanh toán hàng loạt (`Batch Payments`) | DOM |
| Status flow | ✅ 5 trạng thái (widget "Invoice overview"): `Draft` · `Unpaid` · `Partially Paid` · `Overdue` · `Paid` · cộng bộ lọc `Not Sent` (`filter=not_sent`) | UI thực tế · `a[href]` |
| Định dạng số hoá đơn | Quan sát cả `INV-<6 số>` và một tiền tố khác (`ABC-<6 số>`) trong cùng danh sách | UI thực tế |
| Ước lượng độ lớn | 10 cột · 5 trạng thái · hoá đơn định kỳ · thanh toán hàng loạt · thuế | — |
| Risk | 🔴 — tiền · status flow phụ thuộc Payment và thời gian (`Overdue`) · Payment, Expense tham chiếu | — |

### Network

Chưa ghi nhận — trang được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Hai tiền tố số hoá đơn cùng tồn tại — do đổi cấu hình tiền tố theo thời gian hay cho phép sửa số (cấu hình thuộc `SETUP`, bị cấm)
- Luồng `Batch Payments`, `Recurring Invoices` chưa mở
- Quy tắc chuyển `Unpaid` → `Partially Paid` → `Paid` theo Payment, `Overdue` theo `Due Date`
- Áp Credit Note vào hoá đơn
- ⚠️ Môi trường dùng chung: tạo hoá đơn/thanh toán thử sẽ làm lệch số liệu Dashboard & Reports của người khác

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
