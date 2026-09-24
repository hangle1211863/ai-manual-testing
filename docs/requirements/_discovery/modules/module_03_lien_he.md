# Khám phá module — Liên hệ (`CTC`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> ⚠️ `CTC` = **Contacts**. Không nhầm với `CONTR` = Contracts.

## `CTC` — Contacts

| Mục | Giá trị |
|---|---|
| Tên trên UI | Contacts |
| Bí danh | Liên hệ · người liên hệ của khách hàng |
| Route | Danh sách tổng `/admin/clients/all_contacts` (không có trong sidebar — mở từ nút `Contacts` ở Customers) · theo khách hàng `/admin/clients/client/{id}?group=contacts` · `?contactid={id}` |
| Loại màn hình | Danh sách toàn hệ thống (chỉ đọc) + tab CRUD trong chi tiết Customer |
| Nút thanh công cụ (danh sách tổng) | Export |
| Cột bảng (8) | First Name · Last Name · Email · Company · Phone · Position · Last Login · Active |
| CRUD | Danh sách tổng: chỉ Xem + Export · Tạo/Sửa: ❔ qua tab `contacts` của Customer (chưa mở) |
| Status flow | Chỉ cột `Active` |
| Ước REQ | 25–40 |
| Risk | 🔴 — dữ liệu cá nhân (email, điện thoại); cột `Last Login` cho thấy contact **đăng nhập được client portal** → liên quan xác thực; Ticket phụ thuộc |

### Ranh giới

User chốt 2026-09-15: **tách** khỏi `CUST` — Contact có vòng đời riêng (đăng nhập, active, primary contact).

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Form tạo/sửa Contact (modal trong tab Customer) — field, quyền của contact, mật khẩu portal.
- Quy tắc Primary Contact (cột `Primary Contact` ở Customers).

### Evidence (2026-09-15, số liệu DOM)

- `/admin/clients/all_contacts`: `document.title` = `Contacts` · 1 nút · 8 cột.
- Chi tiết Customer: tab hiển thị nhãn `Contacts <số>`.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [contacts_list_viewport.png](../evidence/contacts_list_viewport.png) | Contacts — danh sách tổng | Mặc định | Chỉ có nút Export · 8 cột gồm Last Login và Active (contact đăng nhập được client portal) |
