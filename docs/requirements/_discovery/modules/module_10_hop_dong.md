# Khám phá module — Hợp đồng (`CONTR`)

> Thuộc bản đồ [`../system_map.md`](../system_map.md). **Không chứa mã REQ.** Trạng thái recon xem [`../../README.md`](../../README.md).
> ⚠️ `CONTR` = **Contracts**. Không nhầm với `CTC` = Contacts (user chốt 2026-09-15).

## `CONTR` — Contracts

| Mục | Giá trị |
|---|---|
| Tên trên UI | Contracts |
| Bí danh | Hợp đồng |
| Route | Danh sách `/admin/contracts` · tạo `/admin/contracts/contract` · chi tiết/sửa `/admin/contracts/contract/{id}` |
| Loại màn hình | Danh sách + khối "Contract Summary" |
| Nút thanh công cụ | New Contract · Export |
| Cột bảng (9) | # · Subject · Customer · Contract Type · Contract Value · Start Date · End Date · Project · Signature |
| CRUD | Tạo ✅ · Xem/Sửa ✅ (link từ thông báo trên header) · Xoá ❔ |
| Status flow | ❔ Không có cột Status; có trạng thái ký (`Signature`) và hết hạn (widget Dashboard "Contracts Expiring Soon") |
| Ước REQ | 30–45 |
| Risk | 🟡 — có giá trị hợp đồng và chữ ký; khách hàng bình luận qua client portal (thông báo "New comment from customer on contract…") |

### Tầng network

Chưa ghi nhận (request bảng rơi khỏi bộ đệm).

### Vùng chưa xác minh

- Luồng ký hợp đồng (phía khách — client portal, ngoài phạm vi) và hiển thị phía Admin.
- Contract Type cấu hình ở đâu (❔ Setup).
- Gia hạn hợp đồng, bình luận.

### Evidence (2026-09-15, số liệu DOM)

- 2 nút · 9 cột · header có link thông báo tới `/admin/contracts/contract/{id}`.

### Danh mục Evidence (2026-09-18)

> Chụp bằng Chrome headless, viewport `1600×750`, hồ sơ Chrome riêng đã đăng nhập. Ảnh **viewport** chứ không full-page: ở tầng khám phá chỉ cần chứng minh module tồn tại và thấy thanh công cụ + hàng tiêu đề bảng; full-page sẽ kéo theo toàn bộ dữ liệu nghiệp vụ không liên quan. Mọi ảnh đã được mở lại xác nhận đúng trạng thái.

| Tệp | Màn hình | Trạng thái | Chứng minh |
|---|---|---|---|
| [contracts_list_viewport.png](../evidence/contracts_list_viewport.png) | Contracts — danh sách | Mặc định | Khối Contract Summary: Active · Expired · About to Expire · Recently Added · **Trash** · 2 biểu đồ Contracts by Type và Contracts Value by Type (USD) |
