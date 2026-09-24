# Khám phá module — Thanh đầu trang · Cá nhân · Tiện ích (`HDR` · `PERS` · `UTIL`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì cả ba là thành phần dùng chung / công cụ cá nhân, mỗi phần ít màn hình. **Vẫn là 3 prefix, 3 tài liệu requirements riêng.**

## `HDR` — Thanh đầu trang (Header)

| Mục | Giá trị |
|---|---|
| Tên trên UI | Không có tên — thanh trên cùng mọi trang Admin |
| Bí danh | Header · thanh đầu trang |
| Route | Toàn cục |
| Loại màn hình | Thành phần toàn cục |
| Thành phần quan sát được | Ô tìm kiếm · nút tạo nhanh (+) · Share documents, ideas.. (newsfeed) · To Do (badge → `/admin/todo`) · menu hồ sơ · Notifications (badge) |
| Tạo nhanh (13) | Invoice · Estimate · Proposal · Credit Note · Customer · Subscription · Project · Task (`href="#"` — mở modal) · Expense · Contract · Article · Ticket · Event (`/admin/utilities/calendar?new_event=true&date=<dd-mm-yyyy>`) |
| Menu hồ sơ | My Profile · My Timesheets · Edit Profile · Language (System Default + 25 ngôn ngữ, `/admin/staff/change_language/<lang>`) · Logout |
| Notifications | Mark all as read · danh sách thông báo (link tới chứng từ) · View all notifications (`/admin/profile?notifications=true`) |
| Ước REQ | 20–30 |
| Risk | 🟡 — có mặt trên mọi trang; đổi ngôn ngữ ảnh hưởng mọi assertion theo text; tìm kiếm toàn cục |

## `PERS` — Cá nhân

| Mục | Giá trị |
|---|---|
| Tên trên UI | My Profile · Edit Profile · My To Do Items · Reminders · My Timesheets |
| Bí danh | Cá nhân · hồ sơ nhân viên đang đăng nhập |
| Route | `/admin/profile` · `/admin/staff/edit_profile` · `/admin/todo` · `/admin/misc/reminders` (**không có trong menu** — link trên Dashboard) · `/admin/staff/timesheets` |
| Loại màn hình | Trang hồ sơ · form sửa hồ sơ · danh sách to-do · danh sách nhắc việc |
| My Profile | Khối thông tin nhân viên · Projects · Notifications (Mark all as read) |
| Edit Profile (16 nhãn) | `* First Name` · `* Last Name` · `* Email` · Phone · Default Language · Direction · Facebook · LinkedIn · Skype · Email Signature · `* Old password` · `* New password` · Repeat new password · Disabled · Enable Email Two Factor Authentication · Enable Google Authenticator |
| To Do | New To Do · Unfinished to do's · Latest finished to do's · Load More (×2) |
| Reminders | Nút Export · Cột (5): Related to · Description · Date · Remind · Is notified? |
| CRUD | Sửa hồ sơ ✅ (form) · Tạo To Do ✅ (nút) · Reminders chỉ xem + Export ❔ |
| Ước REQ | 25–35 |
| Risk | 🟡 — đổi mật khẩu và bật 2FA trên **tài khoản Admin dùng chung** có thể khoá cả nhóm → ⚠️ không thử submit phần mật khẩu/2FA khi chưa có tài khoản riêng |

## `UTIL` — Utilities

| Mục | Giá trị |
|---|---|
| Tên trên UI | Utilities ▸ Media · Bulk PDF Export · Calendar |
| Bí danh | Tiện ích |
| Route | `/admin/utilities/media` (`document.title` = `Files`) · `/admin/utilities/bulk_pdf_exporter` · `/admin/utilities/calendar` |
| Loại màn hình | Media: trình quản lý file (elFinder) · Bulk PDF Export: form xuất · Calendar: lịch (FullCalendar) |
| Bulk PDF Export | `* Select Type` · From Date · To Date · Include Tag · các nhóm Status theo loại chứng từ: `All · Draft · Sent · Expired · Declined · Accepted` / `All · Open · Closed · Void` / `All · Unpaid · Paid` |
| Calendar | Nút `today` · `month` · `week` · `day` · `filter by` |
| Ước REQ | 15–25 |
| Risk | 🟢 — công cụ phụ trợ; ⚠️ Media cho upload/xoá file trên môi trường chung |

### Tầng network

| Request | Ghi chú |
|---|---|
| `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 ký tự hex>&start=…&end=…` · `200` | Calendar tải sự kiện; CSRF token nằm trong query string |
| `POST /admin/misc/reminders_table` · `200` | Bảng Reminders |
| `POST /admin/todo` · `200` (×2) | Phát sinh khi **tải** trang To Do — nhiều khả năng tải 2 danh sách (chưa xong / đã xong); cần xác nhận |

### Vùng chưa xác minh

- Header: các link `href="#"` không nhãn rõ ở vùng đầu header (nhãn giống tên nhóm/khách hàng) — ❔ chưa rõ chức năng; timer (Stop Timer) chưa thấy trong đợt này.
- Kết quả tìm kiếm toàn cục · newsfeed · luồng Logout.
- PERS: My Timesheets (chưa đọc cột) · nhãn `Disabled` trong Edit Profile gắn với field nào.
- UTIL: ánh xạ nhóm Status ↔ loại chứng từ trong Bulk PDF Export.

### Evidence (2026-09-15, số liệu DOM)

- Header: 13 link tạo nhanh · 26 mục Language · menu hồ sơ 5 mục.
- Edit Profile: 16 nhãn hiển thị · Reminders: 5 cột · To Do: 5 thành phần.
- Media có `.elfinder` · Calendar có `.fc` + 5 nút · Bulk PDF Export 20 nhãn.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [header_on_profile_viewport.png](../evidence/header_on_profile_viewport.png) | Header (trên trang My Profile) | Mặc định | Ô tìm kiếm · nút tạo nhanh (+) · biểu tượng newsfeed · badge To Do = 2 · avatar · timer · chuông thông báo. Trang Profile có 5 thẻ thời gian đã log và danh sách Projects |
| [pers_edit_profile_viewport.png](../evidence/pers_edit_profile_viewport.png) | Edit Profile | Mặc định | Cột trái: First/Last Name, Email (ô **mờ — nghi disabled**), Phone, Default Language, Direction. Cột phải: Change your password (Old/New/Repeat) và Two Factor Authentication 3 lựa chọn: Disabled · Email · Google Authenticator |
| [utilities_calendar_viewport.png](../evidence/utilities_calendar_viewport.png) | Utilities ▸ Calendar | Chế độ Month | Điều hướng ‹ › Today · nút Month/Week/Day/Filter By · ô ngày hiện sự kiện kèm liên kết "+N more" |
