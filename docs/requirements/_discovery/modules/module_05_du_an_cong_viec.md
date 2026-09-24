# Khám phá module — Dự án · Công việc (`PRJ` · `TASK`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> Gộp file vì quan hệ cha–con (Project ↔ Task). **Vẫn là 2 prefix, 2 tài liệu requirements riêng.**

## `PRJ` — Projects

| Mục | Giá trị |
|---|---|
| Tên trên UI | Projects |
| Bí danh | Dự án |
| Route | Danh sách `/admin/projects` · tạo `/admin/projects/project` · sửa `/admin/projects/project/{id}` · chi tiết `/admin/projects/view/{id}` |
| Loại màn hình | Danh sách + khối "Projects Summary" · form 2 tab · chi tiết nhiều tab |
| Nút thanh công cụ | New Project · Export |
| Cột bảng (8) | # · Project Name · Customer · Tags · Start Date · Deadline · Members · Status |
| CRUD | Tạo ✅ · Xem ✅ · Sửa ✅ link `projects/project/{id}` · Xoá ✅ link GET `/admin/projects/delete/{id}` (chưa mở) |
| Status flow | Có — **5 trạng thái** đọc từ khối "Projects Summary": `Not Started` · `In Progress` · `On Hold` · `Cancelled` · `Finished` |
| Form tạo mới | 40 control · 2 tab: `Project` · `Project Settings` · bắt buộc: `* Project Name` · `* Customer` · `* Billing Type` · `* Start Date` |
| Tab chi tiết (12) | Overview · Tasks · Timesheets · Milestones · Files · Discussions · Gantt · Tickets · Contracts · Sales · Notes · Activity |
| Tab con của Sales (6) | Proposals · Estimates · Invoices · Subscriptions · Expenses · Credit Notes |
| Ước REQ | 60–90 |
| Risk | 🔴 — entity nền của ≥ 12 module; có Billing Type (tính tiền theo giờ/cố định); status flow; xoá qua link GET |

## `TASK` — Tasks

| Mục | Giá trị |
|---|---|
| Tên trên UI | Tasks |
| Bí danh | Công việc |
| Route | Danh sách `/admin/tasks` · chi tiết `/admin/tasks/view/{id}` · danh sách lọc `/admin/tasks/list_tasks` |
| Loại màn hình | Danh sách + khối "Tasks Summary" · chế độ `Tasks Overview` · trang chi tiết |
| Nút thanh công cụ | New Task · Tasks Overview · Export · Bulk Actions |
| Cột bảng (9) | checkbox · # · Name · Status · Start Date · Due Date · Assigned to · Tags · Priority |
| CRUD | Tạo ✅ (nút; từ header mở modal) · Xem ✅ · Xoá ✅ link GET `/admin/tasks/delete_task/{id}` (chưa mở) · Bulk Actions |
| Status flow | Có — 5 trạng thái quan sát ở trang chi tiết: `Not Started` · `In Progress` · `Testing` · `Awaiting Feedback` · `Complete` (các nút "Mark as …") |
| Priority | `Low` · `Medium` · `High` · `Urgent` |
| Trang chi tiết | Related (liên kết tới Project) · Description · Comments · Task Info (Status · Start Date · Due Date · Priority · Hourly Rate · Billable · Billable Amount · Your logged time) |
| Ước REQ | 40–60 |
| Risk | 🟡 — CRUD dùng hằng ngày, có status flow và tính tiền (Billable); timer/timesheet liên quan |

### Tầng network

| Request | Ghi chú |
|---|---|
| `POST /admin/projects/table` · `200` | Tải bảng Projects |
| `POST /admin/tasks/table` · `200` | Tải bảng Tasks (từ widget Dashboard) |
| `POST /admin/tasks/table?bulk_actions=true` · `200` | Phát sinh khi **tải** trang Tasks — cần xác nhận không có tác dụng phụ |

### Vùng chưa xác minh

- Danh sách trạng thái Project đầy đủ; tuỳ chọn Billing Type.
- Form New Task (modal) — field, bắt buộc, Related to entity nào ngoài Project.
- Tasks Overview, Gantt, Milestones, Discussions, Timer bắt đầu/dừng.
- Timer đang chạy trên tài khoản dùng chung — ảnh hưởng timesheet (ghi nhận từ phiên trước, **chưa xác minh lại**).

### Evidence (2026-09-15, số liệu DOM)

- Projects: 2 nút · 8 cột · form 40 control / 2 tab · chi tiết 12 tab + 6 tab con.
- Tasks: 4 nút · 9 cột · chi tiết đọc được 5 trạng thái + 4 mức Priority.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [projects_list_viewport.png](../evidence/projects_list_viewport.png) | Projects — danh sách | Mặc định | Khối Projects Summary nêu đủ **5 trạng thái** · 8 cột · cột Status hiển thị nhãn On Hold / In Progress |
| [tasks_list_viewport.png](../evidence/tasks_list_viewport.png) | Tasks — danh sách | Mặc định | Khối Tasks Summary nêu đủ **5 trạng thái** · cột Status và Priority là dropdown sửa tại chỗ · nhãn Recurring Task |
