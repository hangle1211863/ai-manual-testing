# Khám phá module — Dashboard · Báo cáo (`DASH` · `RPT`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì cùng bản chất tổng hợp số liệu từ nhiều module. **Vẫn là 2 prefix, 2 tài liệu requirements riêng.**

## `DASH` — Dashboard

| Mục | Giá trị |
|---|---|
| Tên trên UI | Dashboard |
| Bí danh | Bảng điều khiển · trang chủ Admin |
| Route | `/admin/` · đặt lại bố cục `/admin/staff/reset_dashboard` (chưa mở — có tác dụng ghi) |
| Loại màn hình | Dashboard gồm nhiều widget |
| Widget (13) | Finance Overview · Quick Statistics · User Widget · Upcoming Events · Calendar · Payment Records · Contracts Expiring Soon · Staff Tickets Report · My To Do Items · Leads Chart · Projects Chart · Tickets Chart · Latest Project Activity |
| Link điều hướng từ widget | Lọc Invoices (`status=1,2,3,4,6` + `not_sent`) · lọc Estimates (`status=1…5` + `not_sent`) · lọc Proposals (`status=1…6`) · `tasks/list_tasks` · `tasks/view/{id}` · `projects/view/{id}` · `misc/reminders` · `profile/{id}` |
| CRUD | Không áp dụng — ❔ sắp xếp/ẩn widget (có `reset_dashboard`) |
| Ước REQ | 15–25 |
| Risk | 🟢 — chỉ hiển thị; số liệu phụ thuộc module nguồn, khảo sát sau |

## `RPT` — Reports

| Mục | Giá trị |
|---|---|
| Tên trên UI | Reports ▸ Sales · Expenses · Expenses vs Income · Leads · Timesheets overview · KB Articles |
| Bí danh | Báo cáo |
| Route | `/admin/reports/sales` · `/admin/reports/expenses` · `/admin/reports/expenses_vs_income` · `/admin/reports/leads` · `/admin/staff/timesheets?view=all` · `/admin/reports/knowledge_base_articles` |
| Loại màn hình | Báo cáo (biểu đồ canvas + bảng) |
| Sales (3 canvas) | 10 báo cáo con: Invoices Report · Items Report · Payments Received · Credit Notes Report · Proposals Report · Estimates Report · Customers Report · Total Income · Payment Modes (Transactions) · Total Value By Customer Groups |
| Expenses (2 canvas) | Nút Detailed Report · bảng: Category · January → December · Year (<năm hiện tại>) |
| Expenses vs Income | 1 biểu đồ |
| Leads (3 canvas) | Nút "Switch to staff report" · chọn tháng · ⚠️ `document.title` chung chung `Perfex CRM \| Anh Tester Demo` |
| Timesheets overview (1 canvas) | `document.title` = `Today - All Staff Members` · cột (9): Staff Member · Task · Timesheet Tags · Start Time · End Time · Note · Related · Time (h) · Time (decimal) |
| KB Articles | Chọn nhóm (`Choose Group` · `Nothing selected`) |
| CRUD | Không áp dụng — chỉ xem, lọc, xuất |
| Ước REQ | 30–45 |
| Risk | 🟡 — số liệu tài chính tổng hợp; đúng/sai phụ thuộc tính toán ở nhiều module |

### Tầng network

| Request | Ghi chú |
|---|---|
| `GET /admin/misc/get_currency/{id}?csrf_token_name=<32 ký tự hex>` · `200` | Phát sinh ở Expenses vs Income |
| `GET /admin/reports/leads_monthly_report/{n}?csrf_token_name=<32 ký tự hex>` · `200` | Báo cáo Leads theo tháng |
| `POST /admin/staff/timesheets?view=all` · `200` | Bảng Timesheets overview |

### Vùng chưa xác minh

- Nội dung từng báo cáo con của Sales (bộ lọc, cột, xuất file).
- Báo cáo Leads có số liệu không, khi danh sách Leads đang 0 bản ghi (Q3 ở index).
- Widget Dashboard có thể ẩn/sắp xếp, `reset_dashboard` làm gì.

### Evidence (2026-09-15, số liệu DOM)

- Dashboard: 13 widget (`[data-name]`, `.widget`) · 26 link ngoài menu.
- Reports: Sales 10 link + 3 canvas · Expenses 13 cột + 2 canvas · Expenses vs Income 1 canvas · Leads 3 canvas · Timesheets 9 cột + 1 canvas.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [dashboard_default_viewport.png](../evidence/dashboard_default_viewport.png) | Dashboard | Mặc định | Nút Dashboard Options · 3 widget overview nêu đủ trạng thái của Invoice (6), Estimate (6), Proposal (6) · khối Outstanding/Past Due/Paid Invoices · các widget đếm |
| [reports_sales_viewport.png](../evidence/reports_sales_viewport.png) | Reports ▸ Sales | Mặc định, các mục đang thu gọn | Cột **Sales Report** 7 mục · cột **Charts Based Report** 3 mục · dòng cảnh báo "Cancelled invoices are excluded from the report" |
