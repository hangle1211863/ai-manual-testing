# Khám phá module — Đề xuất · Báo giá (`PROP` · `EST`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì cùng loại chứng từ bán hàng trước hoá đơn. **Vẫn là 2 prefix, 2 tài liệu requirements riêng.**

## `PROP` — Proposals

| Mục | Giá trị |
|---|---|
| Tên trên UI | Proposals (menu Sales ▸ Proposals) |
| Bí danh | Đề xuất |
| Route | Danh sách `/admin/proposals` · tạo `/admin/proposals/proposal` · sửa `/admin/proposals/proposal/{id}` · xem `/admin/proposals/list_proposals/{id}` · lọc `list_proposals?status={1..6}` |
| Loại màn hình | Danh sách + khung xem chứng từ (split view) |
| Nút thanh công cụ | New Proposal · Export |
| Cột bảng (10) | Proposal # · Subject · To · Total · Date · Open Till · Project · Tags · Date Created · Status |
| CRUD | Tạo ✅ · Xem ✅ · Sửa ✅ (link) · Xoá ❔ |
| Status flow | Có — **6 trạng thái** đọc từ widget "Proposal overview" trên Dashboard: `Draft` · `Sent` · `Open` · `Revised` · `Declined` · `Accepted`. Ánh xạ sang mã `status=1…6` ❔ chưa xác minh |
| Tab chi tiết (6) | Proposal · Comments · Reminders · Tasks · Notes · Templates |
| Ước REQ | 30–45 |
| Risk | 🟡 — chứng từ gửi khách, có status flow, có Comments từ khách (client portal) |

## `EST` — Estimates

| Mục | Giá trị |
|---|---|
| Tên trên UI | Estimates (menu Sales ▸ Estimates) |
| Bí danh | Báo giá |
| Route | Danh sách `/admin/estimates` · tạo `/admin/estimates/estimate` · lọc `list_estimates?status={1..5}` · `list_estimates?not_sent=1` |
| Loại màn hình | Danh sách + khung xem chứng từ |
| Nút thanh công cụ | Create New Estimate · Export |
| Cột bảng (10) | Estimate # · Amount · Total Tax · Customer · Project · Tags · Date · Expiry Date · Reference # · Status |
| CRUD | Tạo ✅ · Xem/Sửa/Xoá ❔ (chưa mở chi tiết) |
| Status flow | Có — **6 trạng thái** đọc từ widget "Estimate overview" trên Dashboard: `Draft` · `Not Sent` · `Sent` · `Expired` · `Declined` · `Accepted`. Ánh xạ sang mã `status=1…5` + `not_sent` ❔ chưa xác minh |
| Ước REQ | 30–45 |
| Risk | 🟡 — có thuế, hết hạn (Expiry Date), ❔ chuyển thành Invoice |

### Tầng network

| Request | Ghi chú |
|---|---|
| `POST /admin/proposals/table` · `200` | Tải bảng Proposals |

### Vùng chưa xác minh

- Ánh xạ mã trạng thái ↔ tên trạng thái của cả hai module.
- Proposal gửi tới `To` là Customer hay Lead (cột `To`, không phải `Customer`).
- Luồng chuyển Estimate → Invoice, Proposal → Estimate/Invoice.
- Link công khai `/proposal/{id}/<32 ký tự hex>` — thuộc client portal, ngoài phạm vi.

### Evidence (2026-09-15, số liệu DOM)

- Proposals: 2 nút · 10 cột · chi tiết 6 tab.
- Estimates: 2 nút · 10 cột.
- Dashboard: 6 link `list_proposals?status=` · 5 link `list_estimates?status=` + 1 `not_sent`.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [proposals_list_viewport.png](../evidence/proposals_list_viewport.png) | Proposals — danh sách | Mặc định, 5 bản ghi | 10 cột · cột To chứa địa chỉ/email (không phải tên khách hàng) · nhãn trạng thái Sent và Open |
| [estimates_list_viewport.png](../evidence/estimates_list_viewport.png) | Estimates — danh sách | Mặc định, 3 bản ghi | 10 cột · nhãn trạng thái Expired · có thêm nút biểu đồ tổng hợp trên thanh công cụ |
