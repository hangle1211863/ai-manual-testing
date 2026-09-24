# 16 — Thanh đầu trang · Cá nhân · Tiện ích

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** ba nhóm màn hình nhỏ cùng loại (thành phần toàn cục / tiện ích cá nhân), không có module 🔴. Gộp file **không** gộp prefix.

## Thanh đầu trang — `HDR`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | Không có tên — thanh cố định đầu mọi trang Admin | UI thực tế |
| Route | Toàn cục | — |
| Loại màn hình | Thành phần toàn cục | UI thực tế |
| Tìm kiếm | Ô `Search...` (có lịch sử tìm kiếm `#search-history`) | UI thực tế · DOM |
| Quick create `(+)` — 13 mục | `Invoice` · `Estimate` · `Proposal` · `Credit Note` · `Customer` · `Subscription` · `Project` · `Task` (mở modal, không có URL) · `Expense` · `Contract` · `Article` · `Ticket` · `Event` | DOM |
| Newsfeed | `Share documents, ideas..` | DOM |
| Timer | Danh sách timer đang chạy `#started-timers-top` · `Stop Timer` | DOM |
| Thông báo | Badge đếm · danh sách thông báo · `Mark all as read` · `View all notifications` → `/admin/profile?notifications=true` | DOM |
| Menu hồ sơ | `My Profile` · `My Timesheets` · `Edit Profile` · `Language` (25 ngôn ngữ, route `/admin/staff/change_language/<ngôn ngữ>`) · `Logout` | DOM |
| Ước lượng độ lớn | Tìm kiếm toàn cục · 13 lối tạo nhanh · thông báo · đổi ngôn ngữ · đăng xuất | — |
| Risk | 🟡 — đổi ngôn ngữ ảnh hưởng mọi nhãn/locator; Quick create là lối tắt vào form của 13 module | — |

### Vùng chưa xác minh

- Phạm vi tìm kiếm (những entity nào được tìm)
- Đổi ngôn ngữ **không** thử — thay đổi cấu hình người dùng trên môi trường dùng chung
- Newsfeed chưa mở

---

## Cá nhân — `PERS`

| Trang | Route | Nội dung quan sát được | Nguồn |
|---|---|---|---|
| My Profile | `/admin/profile` · `/admin/profile/{id}` | Chưa mở | `a[href]` |
| Edit Profile | `/admin/staff/edit_profile` | Chưa mở | `a[href]` |
| My To Do Items | `/admin/todo` | `New To Do` · `Unfinished to do's` · `Latest finished to do's` · `Load More` | DOM |
| Reminders | `/admin/misc/reminders` — **không có trong menu** | Mô tả "Showing your reminders and reminders created by you." · cột `Related to` · `Description` · `Date` · `Remind` · `Is notified?` | DOM |
| My Timesheets | `/admin/staff/timesheets` | Chưa mở riêng (bản `view=all` thuộc `RPT`) | `a[href]` |

| Mục | Ghi nhận |
|---|---|
| Loại màn hình | Danh sách + form hồ sơ |
| CRUD | To Do: tạo, đánh dấu hoàn thành (widget Dashboard có icon sửa/xoá) · Reminders: xem |
| Ước lượng độ lớn | 5 trang nhỏ |
| Risk | 🟢 — dữ liệu của chính người dùng |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/todo` · `200` (2 lần) | Tải danh sách to-do chưa xong / đã xong |
| `POST /admin/misc/reminders_table` · `200` | Tải bảng Reminders |

### Vùng chưa xác minh

- My Profile, Edit Profile (đổi mật khẩu, ảnh đại diện) chưa mở
- Reminders được tạo từ đâu (tab `Reminders` ở Customer, Task…) — ❔

---

## Utilities — `UTIL`

| Trang | Route | Nội dung quan sát được | Nguồn |
|---|---|---|---|
| Media | `/admin/utilities/media` | Tiêu đề trang `Files` — trình quản lý file, không đọc được thành phần qua DOM | UI thực tế |
| Bulk PDF Export | `/admin/utilities/bulk_pdf_exporter` | `* Select Type` (bắt buộc) · `From Date:` · `To Date:` · `Include Tag` · nút `Export` | DOM |
| Calendar | `/admin/utilities/calendar` | Nút `today` · `month` · `week` · `day` · `filter by` | DOM |

| Mục | Ghi nhận |
|---|---|
| Loại màn hình | Quản lý file · form xuất · lịch |
| CRUD | Media: ❔ · Calendar: tạo Event (qua Quick create) ❔ |
| Ước lượng độ lớn | 3 trang |
| Risk | 🟢 — tiện ích, không tác động nghiệp vụ chính |

### Network

| Request | Ghi chú |
|---|---|
| `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 ký tự hex>&start=<ISO datetime>&end=<ISO datetime>` · `200` | Dữ liệu sự kiện theo khoảng ngày đang xem |

### Vùng chưa xác minh

- Media: upload/xoá file — **không** thử trên môi trường dùng chung
- Bulk PDF Export: danh sách `Select Type` chưa mở
- Calendar: loại sự kiện hiển thị, tạo event

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
