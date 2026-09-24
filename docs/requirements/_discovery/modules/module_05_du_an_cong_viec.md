# 05 — Dự án · Công việc

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** quan hệ cha–con chặt (Task là tab của Project). Gộp file **không** gộp prefix — vẫn là 2 module, 2 tài liệu requirements riêng.

## Projects — `PRJ`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Projects` | UI thực tế |
| Route | Danh sách `/admin/projects` · Tạo mới `/admin/projects/project` · Chi tiết `/admin/projects/view/{id}` | UI thực tế |
| Loại màn hình | Danh sách + khối tổng hợp + chi tiết nhiều tab | UI thực tế |
| Nút thanh công cụ | `New Project` · nút icon (chưa rõ chức năng) · `Export` · `Reload` · nút lọc | UI thực tế |
| Khối tổng hợp (= trạng thái) | `Not Started` · `In Progress` · `On Hold` · `Cancelled` · `Finished` | UI thực tế |
| Cột bảng danh sách | `#` · `Project Name` · `Customer` · `Tags` · `Start Date` · `Deadline` · `Members` · `Status` | UI thực tế |
| CRUD | Tạo · Xem · Export. Sửa/xoá: chưa quan sát | UI thực tế |
| Status flow | ✅ 5 trạng thái | UI thực tế |
| Tab chi tiết (12) | `Overview` · `Tasks` · `Timesheets` · `Milestones` · `Files` · `Discussions` · `Gantt` · `Tickets` · `Contracts` · `Sales` · `Notes` · `Activity` | DOM |
| Tab con của `Sales` (6, ẩn tới khi mở) | `Proposals` · `Estimates` · `Invoices` · `Subscriptions` · `Expenses` · `Credit Notes` | DOM |
| Ước lượng độ lớn | 8 cột · 5 trạng thái · 12 tab (≥ 5 tab riêng của Project: Overview, Milestones, Discussions, Gantt, Notes, Activity) | — |
| Risk | 🔴 — entity nền thứ hai (≥ 10 module tham chiếu) · status flow · nhiều tab | — |

> **Ranh giới:** `Milestones`, `Discussions`, `Gantt`, `Notes`, `Activity`, `Files` thuộc `PRJ` (không tồn tại ngoài dự án). `Tasks`, `Tickets`, `Contracts`, `Sales` là góc nhìn của module khác. `Timesheets` thuộc `TASK`.
>
> ⚠️ Module có ≥ 5 tab riêng — recon cấp module nhiều khả năng phải **tách tài liệu** theo Story.

### Network

Chưa ghi nhận — trang được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Form tạo dự án (billing type, members, visible tabs cho khách…) chưa mở
- Nội dung từng tab — mới đọc nhãn tab
- Quy tắc chuyển trạng thái (được chuyển từ đâu sang đâu, ai được chuyển)
- Chức năng nút icon cạnh `New Project`

---

## Tasks — `TASK`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Tasks` | UI thực tế |
| Route | Danh sách `/admin/tasks` · `/admin/tasks/list_tasks` · Chi tiết `/admin/tasks/view/{id}` | UI thực tế |
| Loại màn hình | Danh sách + khối tổng hợp + chế độ xem khác (nút icon lưới) + `Tasks Overview` | UI thực tế |
| Nút thanh công cụ | `New Task` · nút icon lưới (nghi Kanban) · `Tasks Overview` · `Export` · `Bulk Actions` · `Reload` · nút lọc | UI thực tế |
| Khối tổng hợp (= trạng thái) | `Not Started` · `In Progress` · `Testing` · `Awaiting Feedback` · `Complete` — mỗi ô kèm "Tasks assigned to me" | UI thực tế |
| Cột bảng danh sách | `#` · `Name` · `Status` (dropdown đổi trực tiếp trên dòng) · `Start Date` · `Due Date` · `Assigned to` · `Tags` · … (bảng cuộn ngang, còn cột bị khuất) | UI thực tế |
| CRUD | Tạo · Xem · Bulk Actions · Export · **Xoá** (có link `/admin/tasks/delete_task/{id}` trong Dashboard — **không** mở) | UI thực tế · `a[href]` |
| Status flow | ✅ 5 trạng thái · đổi trạng thái ngay trên dòng bảng | UI thực tế |
| Nhãn đặc biệt | `Recurring Task` (task lặp lại) | UI thực tế |
| Thành phần liên quan | Timer ở header (`Stop Timer`) · `My Timesheets` · `Timesheets overview` (Reports) · Reminders | UI thực tế |
| Ước lượng độ lớn | ≥ 7 cột · 5 trạng thái · task lặp · timer/timesheet · checklist/bình luận (❔) | — |
| Risk | 🟡 — dùng hằng ngày, status flow 5 bước, gắn với thời gian làm việc | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/tasks/table` · `200` | Tải bảng (ghi nhận khi Dashboard tải widget task) |

### Vùng chưa xác minh

- `/admin/tasks/view/{id}` hiển thị lại trang danh sách Tasks — nghi mở chi tiết dạng **modal**, chưa xác minh
- Cột bị khuất bên phải bảng
- Chế độ xem Kanban, `Tasks Overview` chưa mở
- Quy tắc task lặp lại, timer, checklist — chưa quan sát

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
