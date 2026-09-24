# Bản đồ hệ thống — Perfex CRM (Admin)

> **INDEX tầng khám phá — tên file bất biến.** Tài liệu này **không chứa mã REQ** — chỉ cấp prefix.
> Trạng thái recon của từng module nằm **duy nhất** ở [`../README.md`](../README.md), không nhân bản ở đây.

## Bản đồ tài liệu

| File | Module bao phủ | Prefix |
|---|---|---|
| [modules/module_01_dang_nhap.md](modules/module_01_dang_nhap.md) | Login | `LOGIN` |
| [modules/module_02_khach_hang.md](modules/module_02_khach_hang.md) | Customers | `CUST` |
| [modules/module_03_lien_he.md](modules/module_03_lien_he.md) | Contacts | `CTC` |
| [modules/module_04_danh_muc_hang_hoa.md](modules/module_04_danh_muc_hang_hoa.md) | Items | `ITEM` |
| [modules/module_05_du_an_cong_viec.md](modules/module_05_du_an_cong_viec.md) | Projects · Tasks | `PRJ` · `TASK` |
| [modules/module_06_de_xuat_bao_gia.md](modules/module_06_de_xuat_bao_gia.md) | Proposals · Estimates | `PROP` · `EST` |
| [modules/module_07_hoa_don.md](modules/module_07_hoa_don.md) | Invoices | `INV` |
| [modules/module_08_thanh_toan.md](modules/module_08_thanh_toan.md) | Payments | `PAY` |
| [modules/module_09_giay_bao_co.md](modules/module_09_giay_bao_co.md) | Credit Notes | `CRN` |
| [modules/module_10_hop_dong.md](modules/module_10_hop_dong.md) | Contracts | `CTR` |
| [modules/module_11_thue_bao.md](modules/module_11_thue_bao.md) | Subscriptions | `SUB` |
| [modules/module_12_chi_phi.md](modules/module_12_chi_phi.md) | Expenses | `EXP` |
| [modules/module_13_khach_tiem_nang_yeu_cau_bao_gia.md](modules/module_13_khach_tiem_nang_yeu_cau_bao_gia.md) | Leads · Estimate Request | `LEAD` · `ESTREQ` |
| [modules/module_14_ho_tro_co_so_tri_thuc.md](modules/module_14_ho_tro_co_so_tri_thuc.md) | Support · Knowledge Base | `TKT` · `KBASE` |
| [modules/module_15_dashboard_bao_cao.md](modules/module_15_dashboard_bao_cao.md) | Dashboard · Reports | `DASH` · `RPT` |
| [modules/module_16_thanh_dau_trang_ca_nhan_tien_ich.md](modules/module_16_thanh_dau_trang_ca_nhan_tien_ich.md) | Header · Cá nhân · Utilities | `HDR` · `PERS` · `UTIL` |
| [modules/module_17_thiet_lap_he_thong.md](modules/module_17_thiet_lap_he_thong.md) | Setup | `SETUP` |

**Kiểm tổng:** 17 file · 1+1+1+1+2+2+1+1+1+1+1+1+2+2+2+3+1 = **24 module** = 24 dòng bảng mục 3 = 24 dòng danh mục `README.md` ✔

---

## 1. Bối cảnh khảo sát

| Mục | Giá trị |
|---|---|
| Ngày khảo sát | 2026-09-14 |
| Mode | **UI** — không có tài liệu QA cung cấp, sự thật 100% từ UI |
| URL | Xem `BASE_URL` trong `.env` |
| Role đã dùng | 1 tài khoản đăng nhập vào phân hệ Admin (thông tin trong `.env`). **Tài khoản này KHÔNG có quyền Setup** — xem mục 5 |
| Cách đăng nhập | User tự đăng nhập trên Chrome; agent dùng lại phiên (agent không nhập mật khẩu) |
| Trình duyệt khảo sát | Google Chrome qua extension Claude in Chrome · viewport đo được `1280×585` (Dashboard) và `1280×529` (chi tiết Customer). ⚠️ Lệch quy định dự án (Playwright MCP `1600×750`) — Playwright MCP không khả dụng trong phiên này |
| Môi trường dùng chung | ✅ Có — chỉ đọc. Không bấm Save, không mở link xoá/sửa (ví dụ đã bỏ qua `/admin/tasks/delete_task/{id}`) |
| Phạm vi crawl | Toàn bộ sidebar (kể cả menu cấp 2), menu Setup, header, gom `a[href]` trên Dashboard, thử URL chỉ-đọc của Setup, mở chi tiết 1 Customer · 1 Project · 1 Task |
| Ngoài phạm vi | Client portal (phía khách hàng) · role khác Admin (user cung cấp sau) |
| Evidence | ⚠️ **Không có ảnh lưu ra đĩa** — extension Claude in Chrome chụp được ảnh để quan sát nhưng không trả về tệp (`save_to_disk` không sinh đường dẫn). Bằng chứng là số liệu DOM ghi trong từng file module. Recon cấp module phải chụp lại bằng Playwright MCP vào `<module>/evidence/` |

## 2. Sơ đồ điều hướng

```
Header (toàn cục)
├── Ô tìm kiếm
├── (+) Quick create: Invoice · Estimate · Proposal · Credit Note · Customer · Subscription
│                     Project · Task · Expense · Contract · Article · Ticket · Event
├── Share documents, ideas..     (newsfeed)
├── To Do (badge)                → /admin/todo
├── Menu hồ sơ: My Profile → /admin/profile · My Timesheets → /admin/staff/timesheets
│               Edit Profile → /admin/staff/edit_profile · Language (25 ngôn ngữ) · Logout
├── Timer: Stop Timer
└── Notifications: Mark all as read · View all → /admin/profile?notifications=true

Sidebar
├── Dashboard            → /admin/
├── Customers            → /admin/clients
├── Projects             → /admin/projects
├── Tasks                → /admin/tasks
├── Contracts            → /admin/contracts
├── Sales ▸
│   ├── Proposals        → /admin/proposals
│   ├── Estimates        → /admin/estimates
│   ├── Invoices         → /admin/invoices
│   ├── Payments         → /admin/payments
│   ├── Credit Notes     → /admin/credit_notes
│   └── Items            → /admin/invoice_items
├── Subscriptions        → /admin/subscriptions
├── Expenses             → /admin/expenses
├── Support              → /admin/tickets
├── Leads                → /admin/leads
├── Estimate Request     → /admin/estimate_request
├── Knowledge Base       → /admin/knowledge_base
├── Utilities ▸
│   ├── Media            → /admin/utilities/media
│   ├── Bulk PDF Export  → /admin/utilities/bulk_pdf_exporter
│   └── Calendar         → /admin/utilities/calendar
└── Reports ▸
    ├── Sales                → /admin/reports/sales
    ├── Expenses             → /admin/reports/expenses
    ├── Expenses vs Income   → /admin/reports/expenses_vs_income
    ├── Leads                → /admin/reports/leads
    ├── Timesheets overview  → /admin/staff/timesheets?view=all
    └── KB Articles          → /admin/reports/knowledge_base_articles

Setup (panel riêng #setup-menu)  → chỉ có tiêu đề "Setup", KHÔNG có mục con với tài khoản này

Route KHÔNG có trong menu (phát hiện qua a[href])
└── /admin/misc/reminders        → trang "Reminders" mở được
```

## 3. Bảng module tổng

| # | Tên trên UI | Bí danh | Prefix | File khám phá | Loại màn hình | Risk | Ước REQ |
|---|---|---|---|---|---|---|---|
| 1 | Login | Đăng nhập | `LOGIN` | [01](modules/module_01_dang_nhap.md) | Form | 🔴 | 15–25 |
| 2 | Customers | Khách hàng | `CUST` | [02](modules/module_02_khach_hang.md) | Danh sách + chi tiết nhiều tab | 🔴 | 60–90 |
| 3 | Contacts | Liên hệ | `CTC` | [03](modules/module_03_lien_he.md) | Danh sách (chưa mở) + tab trong Customer | 🔴 | 25–40 |
| 4 | Items | Hàng hoá / dịch vụ | `ITEM` | [04](modules/module_04_danh_muc_hang_hoa.md) | Danh sách + Import + Groups | 🟡 | 20–30 |
| 5 | Projects | Dự án | `PRJ` | [05](modules/module_05_du_an_cong_viec.md) | Danh sách + chi tiết 12 tab | 🔴 | 60–90 |
| 6 | Tasks | Công việc | `TASK` | [05](modules/module_05_du_an_cong_viec.md) | Danh sách + Kanban/Overview | 🟡 | 40–60 |
| 7 | Proposals | Đề xuất | `PROP` | [06](modules/module_06_de_xuat_bao_gia.md) | Danh sách + tài liệu bán hàng | 🟡 | 30–45 |
| 8 | Estimates | Báo giá | `EST` | [06](modules/module_06_de_xuat_bao_gia.md) | Danh sách + tài liệu bán hàng | 🟡 | 30–45 |
| 9 | Invoices | Hoá đơn | `INV` | [07](modules/module_07_hoa_don.md) | Danh sách + tài liệu bán hàng | 🔴 | 45–70 |
| 10 | Payments | Thanh toán | `PAY` | [08](modules/module_08_thanh_toan.md) | Danh sách (không có nút tạo) | 🔴 | 15–25 |
| 11 | Credit Notes | Giấy báo có | `CRN` | [09](modules/module_09_giay_bao_co.md) | Danh sách + tài liệu bán hàng | 🔴 | 25–35 |
| 12 | Contracts | Hợp đồng | `CTR` | [10](modules/module_10_hop_dong.md) | Danh sách + biểu đồ | 🟡 | 30–45 |
| 13 | Subscriptions | Thuê bao | `SUB` | [11](modules/module_11_thue_bao.md) | Danh sách | 🟡 | 20–30 |
| 14 | Expenses | Chi phí | `EXP` | [12](modules/module_12_chi_phi.md) | Danh sách + Import | 🟡 | 25–40 |
| 15 | Leads | Khách tiềm năng | `LEAD` | [13](modules/module_13_khach_tiem_nang_yeu_cau_bao_gia.md) | Danh sách | 🟡 | 30–45 |
| 16 | Estimate Request | Yêu cầu báo giá | `ESTREQ` | [13](modules/module_13_khach_tiem_nang_yeu_cau_bao_gia.md) | Danh sách + form builder | 🟢 | 15–25 |
| 17 | Support | Tickets · Hỗ trợ | `TKT` | [14](modules/module_14_ho_tro_co_so_tri_thuc.md) | Danh sách | 🟡 | 30–45 |
| 18 | Knowledge Base | Cơ sở tri thức | `KBASE` | [14](modules/module_14_ho_tro_co_so_tri_thuc.md) | Danh sách + Groups | 🟢 | 15–20 |
| 19 | Dashboard | Bảng điều khiển | `DASH` | [15](modules/module_15_dashboard_bao_cao.md) | Dashboard | 🟢 | 15–25 |
| 20 | Reports | Báo cáo | `RPT` | [15](modules/module_15_dashboard_bao_cao.md) | Báo cáo (6 trang) | 🟡 | 30–45 |
| 21 | (Header) | Thanh đầu trang | `HDR` | [16](modules/module_16_thanh_dau_trang_ca_nhan_tien_ich.md) | Thành phần toàn cục | 🟡 | 20–30 |
| 22 | My Profile · To Do · Reminders · My Timesheets | Cá nhân | `PERS` | [16](modules/module_16_thanh_dau_trang_ca_nhan_tien_ich.md) | Danh sách + form hồ sơ | 🟢 | 20–30 |
| 23 | Utilities | Tiện ích | `UTIL` | [16](modules/module_16_thanh_dau_trang_ca_nhan_tien_ich.md) | Quản lý file · form xuất · lịch | 🟢 | 15–25 |
| 24 | Setup | Thiết lập hệ thống | `SETUP` | [17](modules/module_17_thiet_lap_he_thong.md) | ❔ Không truy cập được | ❔ | ❔ |

**Tổng: 24 module** · Ước REQ (bỏ `SETUP`): ~655 → ~1.000 REQ.

> ⚠️ Mọi module đều ⬜ Trắng tài liệu, nên tiêu chí "mức phủ ⬜ → 🔴" không phân biệt được — risk trên chỉ xét theo nghiệp vụ (tiền · dữ liệu khách hàng · quyền · số module phụ thuộc).

## 4. Bản đồ entity & phụ thuộc

### 4.1. Quan hệ giữa các entity

| Entity | Phụ thuộc vào (cha) | Được tham chiếu bởi | Căn cứ |
|---|---|---|---|
| Customer (`CUST`) | — | Contact · Project · Invoice · Estimate · Credit Note · Contract · Subscription · Expense · Payment · Ticket · Reminder | 19 tab ở chi tiết Customer + cột `Customer` ở các bảng danh sách |
| Contact (`CTC`) | Customer | Ticket (cột `Contact`) | Tab `Contacts` trong Customer · cột `Primary Contact` |
| Item (`ITEM`) | — | ❔ Proposal · Estimate · Invoice · Credit Note | ❔ Suy từ tên menu "Sales ▸ Items", **chưa xác minh** |
| Project (`PRJ`) | Customer | Task · Timesheet · Milestone · Discussion · Ticket · Contract · Proposal · Estimate · Invoice · Subscription · Expense · Credit Note | 12 tab + 6 tab con của `Sales` ở chi tiết Project · cột `Project` ở các bảng |
| Task (`TASK`) | ❔ Project (tuỳ chọn) | Timesheet · Reminder | Tab `Tasks` trong Project · cột `Task` ở Timesheets overview |
| Proposal (`PROP`) | Customer **hoặc** ❔ Lead | — | Cột `To` (không phải `Customer`) → đối tượng nhận có thể không phải khách hàng |
| Estimate (`EST`) | Customer | ❔ Invoice (chuyển đổi) | Cột `Customer` · luồng chuyển đổi **chưa xác minh** |
| Invoice (`INV`) | Customer · Project | Payment · Expense (cột `Invoice`) · ❔ Credit Note | Cột `Invoice #` ở Payments, cột `Invoice` ở Expenses |
| Payment (`PAY`) | Invoice | — | Cột `Invoice #` · không có nút tạo trên danh sách |
| Credit Note (`CRN`) | Customer · Project | ❔ áp vào Invoice | Cột `Remaining Amount` gợi ý số dư được trừ dần — **chưa xác minh** |
| Contract (`CTR`) | Customer · Project | — | Cột `Customer`, `Project` |
| Subscription (`SUB`) | Customer · Project | ❔ sinh Invoice | Cột `Next Billing Cycle` — **chưa xác minh** |
| Expense (`EXP`) | ❔ Customer · Project · Invoice (tuỳ chọn) | — | Cột `Project`, `Customer`, `Invoice` |
| Lead (`LEAD`) | — | ❔ Proposal · ❔ chuyển thành Customer | **Chưa xác minh** |
| Estimate Request (`ESTREQ`) | — | ❔ Lead / Estimate | **Chưa xác minh** |
| Ticket (`TKT`) | Contact · ❔ Project | — | Cột `Contact` · tab `Tickets` trong Project |

**Module nền (nhiều module khác phụ thuộc):** `CUST` (≥ 11 module) → `PRJ` (≥ 10) → `INV` (≥ 2). Đây là lý do thứ tự khảo sát đặt `CUST` lên đầu.

### 4.2. Phát hiện tầng network cấp hệ thống

> Ghi nhận thụ động request do UI tự phát sinh. **Không** gọi API trực tiếp. Theo dõi network chỉ bắt đầu sau khi đã mở Customers → Contracts → Proposals → Estimates → Invoices → Payments, nên các trang đó **chưa có** dữ liệu network.

| Quan sát | Chi tiết | Ý nghĩa cho recon cấp module |
|---|---|---|
| Bảng danh sách tải qua `POST /admin/<entity>/table` · `200` | Gặp ở `credit_notes`, `invoice_items`, `subscriptions`, `expenses`, `leads`, `estimate_request`, `tasks` · Reminders dùng `POST /admin/misc/reminders_table` | Bảng render phía server (DataTables) — phân trang, lọc, sắp xếp đi qua request này. Recon phải bật network khi đổi bộ lọc |
| CSRF token truyền trong **query string** của request GET | Ví dụ `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 ký tự hex>&start=…&end=…` · `GET /admin/misc/get_currency/{id}?csrf_token_name=<32 ký tự hex>` | Token lộ trong URL (log máy chủ, lịch sử). Ghi nhận để hỏi ở tầng module — **không** tự kết luận là lỗi |
| Trang bị cấm chuyển tới `/admin/access_denied` | Request gốc trả `200` rồi chuyển hướng | Ranh giới quyền được áp ở server — dùng làm căn cứ ma trận phân quyền |
| `POST /admin/tickets?bulk_actions=true` phát sinh ngay khi **tải** trang Support | Không có thao tác nào của người dùng | Cần xác minh ở recon `TKT`: request này chỉ để tải bảng hay có tác dụng phụ |
| `/admin/modules` chuyển về `/admin/` (Dashboard), **không** qua `access_denied` | Khác hành vi với các URL Setup khác | Cần làm rõ ở `SETUP` |

## 5. Ma trận phân quyền sơ bộ (cấp module)

| Module | Tài khoản Admin trong `.env` | Các role khác |
|---|---|---|
| `LOGIN` | ✅ Trang đăng nhập mở được | ❔ |
| `CUST` | ✅ Vào được danh sách + chi tiết | ❔ |
| `CTC` | ❔ Thấy nút `Contacts`, **chưa mở** trang danh sách | ❔ |
| `ITEM` | ✅ | ❔ |
| `PRJ` | ✅ Vào được danh sách + chi tiết | ❔ |
| `TASK` | ✅ | ❔ |
| `PROP` | ✅ | ❔ |
| `EST` | ✅ | ❔ |
| `INV` | ✅ | ❔ |
| `PAY` | ✅ | ❔ |
| `CRN` | ✅ | ❔ |
| `CTR` | ✅ | ❔ |
| `SUB` | ✅ | ❔ |
| `EXP` | ✅ | ❔ |
| `LEAD` | ✅ | ❔ |
| `ESTREQ` | ✅ | ❔ |
| `TKT` | ✅ | ❔ |
| `KBASE` | ✅ | ❔ |
| `DASH` | ✅ | ❔ |
| `RPT` | ✅ Mở được cả 6 trang | ❔ |
| `HDR` | ✅ | ❔ |
| `PERS` | ✅ `todo`, `misc/reminders`, `staff/timesheets` mở được | ❔ |
| `UTIL` | ✅ Mở được cả 3 trang | ❔ |
| `SETUP` | ❌ `staff` · `roles` · `settings` · `custom_fields` · `emails` · `clients/groups` → `/admin/access_denied`; `modules` → Dashboard | ❔ |

```
Tổng 48 ô = Đã kiểm chứng 23 · Suy diễn 0 · Chưa rõ 25 · Không áp dụng 0
Kiểm chứng ở đây = MỞ ĐƯỢC TRANG (mức truy cập), chưa phải quyền từng hành động (xem/tạo/sửa/xoá).
Ô ❔ của CTC × Admin: thấy nút nhưng chưa mở trang. Cột "Các role khác": chưa có danh sách role
(màn hình Roles bị cấm) và chưa có account — user cung cấp sau.
```

## 6. Thứ tự khảo sát đã chốt

Xếp theo *phụ thuộc trước, risk sau* — user chốt 2026-09-14.

| Thứ tự | Module | Lý do |
|---|---|---|
| 1 | `LOGIN` | Cổng vào mọi module |
| 2 | `CUST` | Entity nền của ≥ 11 module |
| 3 | `CTC` | Con trực tiếp của Customer, Ticket phụ thuộc |
| 4 | `ITEM` | Danh mục dùng cho các tài liệu bán hàng |
| 5 | `PRJ` | Entity nền thứ hai |
| 6 | `TASK` | Con của Project, nguồn Timesheet |
| 7 | `PROP` | Đầu luồng bán hàng |
| 8 | `EST` | Luồng bán hàng |
| 9 | `INV` | Luồng bán hàng — tiền |
| 10 | `PAY` | Phụ thuộc Invoice |
| 11 | `CRN` | Phụ thuộc Invoice |
| 12 | `CTR` | |
| 13 | `SUB` | |
| 14 | `EXP` | |
| 15 | `LEAD` | |
| 16 | `ESTREQ` | |
| 17 | `TKT` | Phụ thuộc Contact |
| 18 | `KBASE` | |
| 19 | `DASH` | Tổng hợp số liệu từ nhiều module — khảo sát sau khi hiểu nguồn |
| 20 | `RPT` | Như trên |
| 21 | `HDR` | Quick create gọi form của nhiều module |
| 22 | `PERS` | |
| 23 | `UTIL` | |
| 24 | `SETUP` | ⛔ **BLOCKED** — thiếu quyền |

### Câu hỏi mở cấp hệ thống

| # | Câu hỏi | Chặn gì |
|---|---|---|
| Q1 | Tài khoản Admin trong `.env` bị giới hạn quyền có chủ đích (bản demo khoá Setup) hay cần tài khoản admin đầy đủ? | `SETUP` và toàn bộ cột phân quyền role khác |
| Q2 | Hệ thống có những role nào? (màn hình Roles bị cấm nên không đọc được) | Ma trận phân quyền ở mọi module |
| Q3 | Recon cấp module chạy trên môi trường dùng chung — các luồng bắt buộc ghi dữ liệu (trigger validation khi Save, chuyển trạng thái) sẽ xử lý thế nào? | Độ sâu recon của mọi module có form |

## 7. Nhật ký khám phá

| Ngày | Mode | Phạm vi | Kết quả | Nguồn |
|---|---|---|---|---|
| 2026-09-14 | Recon module | `LOGIN` | Lệch so với bản đồ: (1) module **có tài liệu** — `Reqs/SRS_Login_Module.md` (mục 1 ghi "không có tài liệu"); (2) 49 REQ, vượt ước lượng 15–25 vì gồm cả Forgot Password + đăng xuất + CSRF/HTTPS; (3) tài khoản dùng chung đang có **timer chạy** → Logout phải xác nhận qua popup — ảnh hưởng recon `HDR`, `TASK`; (4) mở `http://` không chuyển HTTPS — áp cho toàn site, cần kiểm ở module khác | `/generate-requirements-from-website LOGIN` |
| 2026-09-14 | UI | Toàn bộ phân hệ Admin với 1 tài khoản | Khởi tạo bản đồ: 24 module · 24 prefix · 17 file. `SETUP` bị chặn do thiếu quyền. Phát hiện route ngoài menu `/admin/misc/reminders`. Không lưu được ảnh evidence (giới hạn extension) | UI thực tế · user chốt checkpoint |
