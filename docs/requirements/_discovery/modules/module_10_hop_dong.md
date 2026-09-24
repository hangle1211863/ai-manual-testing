# 10 — Hợp đồng

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Contracts — `CTR`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Contracts` | UI thực tế |
| Route | Danh sách `/admin/contracts` · Tạo mới `/admin/contracts/contract` · Chi tiết `/admin/contracts/contract/{id}` | UI thực tế · `a[href]` |
| Loại màn hình | Danh sách + khối tổng hợp + 2 biểu đồ | UI thực tế |
| Nút thanh công cụ | `New Contract` · icon · `Export` · `Reload` · nút lọc | DOM |
| Khối tổng hợp | `Active` · `Expired` · `About to Expire` · `Recently Added` · `Trash` | UI thực tế |
| Biểu đồ | `Contracts by Type` · `Contracts Value by Type` (USD) | UI thực tế |
| Cột bảng danh sách | `#` · `Subject` · `Customer` · `Contract Type` · `Contract Value` · `Start Date` · `End Date` · `Project` · `Signature` | DOM |
| CRUD | Tạo · Xem · Export · có **Trash** (xoá mềm) | DOM · UI thực tế |
| Status flow | Theo thời gian: `Active` / `About to Expire` / `Expired` · cộng `Trash` · cộng trạng thái chữ ký (`Signature`) | UI thực tế |
| Tương tác phía khách hàng | Thông báo "New comment from customer on contract…" ở header → khách hàng bình luận được trên hợp đồng | UI thực tế |
| Ước lượng độ lớn | 9 cột · trạng thái theo ngày · chữ ký · bình luận · loại hợp đồng | — |
| Risk | 🟡 — có giá trị tiền và chữ ký, nhưng không làm đổi số liệu tài chính khác | — |

### Network

Chưa ghi nhận — trang được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Ngưỡng "About to Expire" (bao nhiêu ngày) — cấu hình thuộc `SETUP`, bị cấm
- Luồng ký hợp đồng, bình luận của khách
- Khôi phục từ `Trash`
- Danh mục `Contract Type`

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
