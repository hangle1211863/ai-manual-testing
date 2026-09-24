# 14 — Hỗ trợ · Cơ sở tri thức

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** cùng phân hệ hỗ trợ khách hàng (bài viết tri thức phục vụ giảm ticket), không có module 🔴. Gộp file **không** gộp prefix.

## Support — `TKT`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Support` (tiêu đề trang `Support Tickets`) | UI thực tế |
| Route | Danh sách `/admin/tickets` · Tạo mới `/admin/tickets/add` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + khối `Tickets Summary` | DOM |
| Nút thanh công cụ | `New Ticket` · icon · `Export` · `Bulk Actions` · `Reload` | DOM |
| Cột bảng danh sách | `-` · `#` · `Subject` · `Tags` · `Department` · `Service` · `Contact` · `Status` · `Priority` · `Last Reply` · `Created` | DOM |
| CRUD | Tạo · Bulk Actions · Export | DOM |
| Status flow | Có cột `Status` + `Priority` — danh sách ❔ | DOM |
| Ước lượng độ lớn | 10 cột · trạng thái · độ ưu tiên · phòng ban · trả lời ticket | — |
| Risk | 🟡 — giao tiếp trực tiếp với khách hàng qua Contact | — |

### Network

| Request | Ghi chú |
|---|---|
| `GET /admin/tickets` · `200` | Tải trang |
| `POST /admin/tickets?bulk_actions=true` · `200` | ⚠️ Phát sinh **ngay khi tải trang**, không có thao tác người dùng — cần xác minh là request tải bảng hay có tác dụng phụ |

### Vùng chưa xác minh

- Danh mục `Department`, `Service`, `Status`, `Priority` (cấu hình thuộc `SETUP`, bị cấm)
- Luồng trả lời, gộp ticket, chuyển trạng thái
- Request `bulk_actions=true` khi tải trang

---

## Knowledge Base — `KBASE`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Knowledge Base` | UI thực tế |
| Route | Danh sách `/admin/knowledge_base` · Tạo mới `/admin/knowledge_base/article` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách bài viết + nhóm | DOM |
| Nút thanh công cụ | `New Article` · `Groups` · icon · `Export` · `Reload` | DOM |
| Cột bảng danh sách | `Article Name` · `Group` · `Date Published` | DOM |
| CRUD | Tạo bài viết · quản lý Groups · Export | DOM |
| Status flow | Không quan sát được | — |
| Ước lượng độ lớn | 3 cột · form bài viết (editor) · nhóm | — |
| Risk | 🟢 — nội dung tĩnh | — |

### Network

| Request | Ghi chú |
|---|---|
| `POST /admin/knowledge_base` · `200` | Tải bảng danh sách |

### Vùng chưa xác minh

- Hiển thị bài viết phía khách hàng (client portal — ngoài phạm vi)
- Báo cáo `KB Articles` thuộc `RPT`

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
