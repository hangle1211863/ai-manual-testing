# 15 — Dashboard · Báo cáo

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)
>
> **Lý do gộp file:** cùng loại màn hình tổng hợp chỉ-đọc, số liệu lấy từ module khác, không có module 🔴. Gộp file **không** gộp prefix.

## Dashboard — `DASH`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Dashboard` | UI thực tế |
| Route | `/admin/` | UI thực tế |
| Loại màn hình | Dashboard gồm các widget kéo thả được (có tay nắm kéo) | UI thực tế |
| Nút | `Dashboard Options` · link reset `/admin/staff/reset_dashboard` (**không** mở) | UI thực tế · `a[href]` |
| Widget quan sát được | `Invoice overview` · `Estimate overview` · `Proposal overview` (mỗi trạng thái có link lọc sang danh sách) · `My To Do Items` (`View All`, `New To Do`, `Latest to do's`, `Latest finished to do's`) · widget có bảng task (cột `Name`, `Status`, `Start Date`, `Tags`, `Priority` và các ngày trong tuần) | UI thực tế · DOM |
| CRUD | Không áp dụng — chỉ tuỳ biến bố cục | — |
| Ước lượng độ lớn | ≥ 5 widget · tuỳ chọn hiển thị · tỷ lệ % trạng thái | — |
| Risk | 🟢 — chỉ đọc; nhưng sai số liệu gây hiểu nhầm cho người quản lý | — |

### Vùng chưa xác minh

- Danh sách widget đầy đủ (trang cuộn dài, mới đọc phần đầu)
- `Dashboard Options` chưa mở · `reset_dashboard` **không** thử (thay đổi cấu hình người dùng)
- Công thức % trạng thái ở các widget overview

---

## Reports — `RPT`

| Trang | Route | Nội dung quan sát được | Nguồn |
|---|---|---|---|
| Sales | `/admin/reports/sales` | Danh sách báo cáo: `Invoices Report` · `Items Report` · `Payments Received` · `Credit Notes Report` · `Proposals Report` · `Estimates Report` · `Customers Report` · nhóm `Charts Based Report`: `Total Income` · `Payment Modes (Transactions)` · `Total Value By Customer Groups` | DOM |
| Expenses | `/admin/reports/expenses` | Bảng `Category` × 12 tháng + `Year (<năm>)` | DOM |
| Expenses vs Income | `/admin/reports/expenses_vs_income` | Biểu đồ (không đọc được nhãn qua DOM) | UI thực tế |
| Leads | `/admin/reports/leads` | Tiêu đề trang chung chung; dữ liệu tải qua request riêng | UI thực tế · network |
| Timesheets overview | `/admin/staff/timesheets?view=all` | Cột `Staff Member` · `Task` · `Timesheet Tags` · `Start Time` · `End Time` · `Note` · `Related` · `Time (h)` · `Time (decimal)` | DOM |
| KB Articles | `/admin/reports/knowledge_base_articles` | Bộ lọc `Choose Group` | DOM |

| Mục | Ghi nhận |
|---|---|
| Loại màn hình | Báo cáo (6 trang, trang Sales chứa 10 báo cáo con) |
| CRUD | Không áp dụng — chỉ đọc, lọc, xuất |
| Ước lượng độ lớn | 6 trang · ≥ 10 báo cáo con · bộ lọc thời gian · biểu đồ |
| Risk | 🟡 — số liệu tài chính tổng hợp; sai công thức khó phát hiện |

### Network

| Request | Ghi chú |
|---|---|
| `GET /admin/misc/get_currency/{id}?csrf_token_name=<32 ký tự hex>` · `200` | Lấy tiền tệ cho báo cáo Expenses vs Income |
| `GET /admin/reports/leads_monthly_report/{n}?csrf_token_name=<32 ký tự hex>` · `200` | Dữ liệu báo cáo Leads theo tháng |
| `POST /admin/staff/timesheets?view=all` · `200` | Tải bảng Timesheets overview |

### Vùng chưa xác minh

- Nội dung từng báo cáo con của Sales — mới đọc tên
- Công thức tổng hợp và bộ lọc thời gian
- Timesheets overview: `view=all` có lộ timesheet của mọi nhân viên không, với role khác thì sao — ❔

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
