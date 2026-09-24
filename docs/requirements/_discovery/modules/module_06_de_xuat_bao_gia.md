# 06 — Đề xuất · Báo giá

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** hai module nhỏ cùng loại (tài liệu bán hàng trước hoá đơn), cùng phân hệ `Sales`, cùng risk 🟡. Gộp file **không** gộp prefix.

## Proposals — `PROP`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Proposals` | UI thực tế |
| Route | Danh sách `/admin/proposals` · `/admin/proposals/list_proposals?status=<n>` · Tạo mới `/admin/proposals/proposal` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + tài liệu bán hàng | UI thực tế |
| Nút thanh công cụ | `New Proposal` · icon · `Toggle Table` · `Export` · `Reload` · nút lọc | DOM |
| Cột bảng danh sách | `Proposal #` · `Subject` · `To` · `Total` · `Date` · `Open Till` · `Project` · `Tags` · `Date Created` · `Status` | DOM |
| CRUD | Tạo · Xem · Export. Sửa/xoá: chưa quan sát | DOM |
| Status flow | ✅ 6 trạng thái (widget "Proposal overview" ở Dashboard): `Draft` · `Sent` · `Open` · `Revised` · `Declined` · `Accepted` — link lọc `status=1…6` | UI thực tế · `a[href]` |
| Ước lượng độ lớn | 10 cột · 6 trạng thái · form tài liệu có dòng hàng | — |
| Risk | 🟡 — tài liệu có giá trị tiền, status flow 6 bước | — |

### Vùng chưa xác minh

- Cột `To` (không phải `Customer`) — đối tượng nhận có thể là Customer **hoặc** Lead, chưa xác minh
- Form tạo, dòng hàng lấy từ `Items`, chuyển đổi Proposal → Estimate/Invoice — chưa quan sát
- Ánh xạ `status=<n>` ↔ nhãn trạng thái — chưa đọc

---

## Estimates — `EST`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Estimates` | UI thực tế |
| Route | Danh sách `/admin/estimates` · `/admin/estimates/list_estimates?status=<n>` · `?not_sent=1` · Tạo mới `/admin/estimates/estimate` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + tài liệu bán hàng | UI thực tế |
| Nút thanh công cụ | `Create New Estimate` · icon · `Toggle Table` · `View Quick Stats` · `Export` · `Reload` · nút lọc | DOM |
| Cột bảng danh sách | `Estimate #` · `Amount` · `Total Tax` · `Customer` · `Project` · `Tags` · `Date` · `Expiry Date` · `Reference #` · `Status` | DOM |
| CRUD | Tạo · Xem · Export | DOM |
| Status flow | ✅ 5 trạng thái (widget "Estimate overview"): `Draft` · `Sent` · `Expired` · `Declined` · `Accepted` · cộng bộ lọc `Not Sent` (tham số `not_sent=1`, **không** phải trạng thái) | UI thực tế · `a[href]` |
| Ước lượng độ lớn | 10 cột · 5 trạng thái + tự hết hạn theo `Expiry Date` | — |
| Risk | 🟡 — tài liệu có giá trị tiền, trạng thái `Expired` phụ thuộc thời gian | — |

### Vùng chưa xác minh

- `Expired` chuyển tự động theo `Expiry Date` hay thủ công
- Chuyển Estimate → Invoice
- `View Quick Stats` chưa mở

### Network

Chưa ghi nhận cho cả hai module — trang được mở trước khi bật theo dõi network.

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
