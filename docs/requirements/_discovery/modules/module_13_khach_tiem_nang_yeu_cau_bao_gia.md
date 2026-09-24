# Khám phá module — Khách tiềm năng · Yêu cầu báo giá (`LEAD` · `ESTREQ`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì cùng giai đoạn tiền bán hàng (thu nhận nhu cầu khách). **Vẫn là 2 prefix, 2 tài liệu requirements riêng.**

## `LEAD` — Leads

| Mục | Giá trị |
|---|---|
| Tên trên UI | Leads |
| Bí danh | Khách tiềm năng |
| Route | Danh sách `/admin/leads` · chuyển chế độ Kanban `/admin/leads/switch_kanban/1` (chưa mở — có thể lưu tuỳ chọn người dùng) |
| Loại màn hình | Danh sách + Kanban |
| Nút thanh công cụ | New Lead · Export · Bulk Actions · nút chuyển Kanban |
| Cột bảng (13) | checkbox · # · Name · Company · Email · Phone · Value · Tags · Assigned · Status · Source · Last Contact · Created |
| CRUD | Tạo ✅ (nút) · Bulk Actions ✅ · Xem/Sửa/Xoá ❔ |
| Status flow | Có — cột `Status` + Kanban theo trạng thái (giá trị ❔) |
| Dữ liệu hiện tại | ⚠️ `Showing 0 to 0 of 0 entries` — bảng rỗng |
| Ước REQ | 30–45 |
| Risk | 🟡 — dữ liệu liên hệ cá nhân; ❔ chuyển Lead thành Customer; Kanban kéo thả |

## `ESTREQ` — Estimate Request

| Mục | Giá trị |
|---|---|
| Tên trên UI | Estimate Request |
| Bí danh | Yêu cầu báo giá |
| Route | `/admin/estimate_request` |
| Loại màn hình | Danh sách yêu cầu + tạo form thu thập (form builder) |
| Nút thanh công cụ | New Form · Export |
| Cột bảng (6) | # · Email · Tags · Assigned · Status · Created |
| CRUD | Tạo form ✅ (nút) · Xem/Sửa/Xoá ❔ |
| Status flow | Có — cột `Status` (giá trị ❔) |
| Dữ liệu hiện tại | ⚠️ `No entries found` — bảng rỗng (ảnh 2026-09-18) |
| Ước REQ | 15–25 |
| Risk | 🟢 — tra cứu và phân công; form công khai thu yêu cầu từ bên ngoài (❔) |

### Tầng network

| Request | Ghi chú |
|---|---|
| `POST /admin/leads/table` · `200` | Tải bảng Leads — trả về 0 bản ghi |
| `GET /admin/reports/leads_monthly_report/{n}?csrf_token_name=<32 ký tự hex>` · `200` | Thuộc báo cáo Leads (`RPT`) — ghi chéo để tham chiếu |

### Vùng chưa xác minh

- **Vì sao Leads 0 bản ghi** trong khi Dashboard có widget "Leads Chart" — bộ lọc mặc định, quyền chỉ xem lead được giao, hay dữ liệu trống thật (Q3 ở index).
- Form New Lead · Source/Status cấu hình ở đâu (❔ Setup) · luồng Convert to Customer.
- Form builder của Estimate Request và form công khai sinh ra.

### Evidence (2026-09-15, số liệu DOM)

- Leads: 13 cột · `.dataTables_info` = `Showing 0 to 0 of 0 entries` · có link `leads/switch_kanban/1`.
- Estimate Request: 2 nút · 6 cột.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [leads_list_empty_viewport.png](../evidence/leads_list_empty_viewport.png) | Leads — danh sách | **Rỗng** — No entries found | Nút New Lead + 2 nút chế độ xem (biểu đồ, Kanban) · 13 cột gồm Value, Source, Assigned |
| [estimate_request_list_viewport.png](../evidence/estimate_request_list_viewport.png) | Estimate Request — danh sách | **Rỗng** — No entries found | Nút New Form · 6 cột |
