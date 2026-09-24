# Danh mục Requirements — Perfex CRM (Admin)

> **Điểm vào cấp hệ thống.** Đọc file này đầu tiên để biết module nào đã có tài liệu, prefix nào đã bị chiếm, mã REQ kế tiếp bắt đầu từ đâu.
>
> Bản đồ khám phá chi tiết: [`_discovery/system_map.md`](_discovery/system_map.md)

| Thông tin | Giá trị |
|---|---|
| Hệ thống | Perfex CRM — bản demo Anh Tester, phân hệ **Admin** |
| URL · tài khoản | Xem `.env` (`BASE_URL`) — **không** ghi vào `docs/` |
| Tiền tố TC ID | **`CRM_`** → `CRM_<MODULE>_TC_<3 số>` (user xác nhận 2026-09-14) |
| Môi trường dùng chung | ✅ **Có** — chỉ đọc: KHÔNG tạo / sửa / xoá dữ liệu, KHÔNG bấm Save khi recon |
| Phạm vi | Chỉ phân hệ Admin. Client portal ngoài phạm vi. Role khác Admin: user cung cấp sau |

---

## 1. Bảng danh mục module

| Module | Prefix | Trạng thái recon | Mức phủ tài liệu | Tài liệu | REQ đã dùng | Mã kế tiếp | AMB treo | Story | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|
| Login | `LOGIN` | ✅ Đã có tài liệu | 🟨 Một phần — SRS v1.0 Draft (`Reqs/`) | [login/requirements_login.md](login/requirements_login.md) | `REQ-LOGIN-01` → `49` | `REQ-LOGIN-50` | 23 (🔴 5) | 7 | 2026-09-14 |
| Customers | `CUST` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CUST-01` | — | — | 2026-09-14 |
| Contacts | `CTC` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CTC-01` | — | — | 2026-09-14 |
| Items | `ITEM` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ITEM-01` | — | — | 2026-09-14 |
| Projects | `PRJ` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PRJ-01` | — | — | 2026-09-14 |
| Tasks | `TASK` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TASK-01` | — | — | 2026-09-14 |
| Proposals | `PROP` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROP-01` | — | — | 2026-09-14 |
| Estimates | `EST` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EST-01` | — | — | 2026-09-14 |
| Invoices | `INV` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-INV-01` | — | — | 2026-09-14 |
| Payments | `PAY` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PAY-01` | — | — | 2026-09-14 |
| Credit Notes | `CRN` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CRN-01` | — | — | 2026-09-14 |
| Contracts | `CTR` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CTR-01` | — | — | 2026-09-14 |
| Subscriptions | `SUB` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-SUB-01` | — | — | 2026-09-14 |
| Expenses | `EXP` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EXP-01` | — | — | 2026-09-14 |
| Leads | `LEAD` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-LEAD-01` | — | — | 2026-09-14 |
| Estimate Request | `ESTREQ` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ESTREQ-01` | — | — | 2026-09-14 |
| Support (Tickets) | `TKT` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TKT-01` | — | — | 2026-09-14 |
| Knowledge Base | `KBASE` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-KBASE-01` | — | — | 2026-09-14 |
| Dashboard | `DASH` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-DASH-01` | — | — | 2026-09-14 |
| Reports | `RPT` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-RPT-01` | — | — | 2026-09-14 |
| Thanh đầu trang (Header) | `HDR` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-HDR-01` | — | — | 2026-09-14 |
| Cá nhân (Profile · To Do · Reminders · My Timesheets) | `PERS` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PERS-01` | — | — | 2026-09-14 |
| Utilities (Media · Bulk PDF Export · Calendar) | `UTIL` | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-UTIL-01` | — | — | 2026-09-14 |
| Setup | `SETUP` | ⏸️ Hoãn — tài khoản không có quyền (`/admin/access_denied`) | ⬜ Trắng | — | — | `REQ-SETUP-01` | — | — | 2026-09-14 |

**Tổng: 24 module** — khớp bảng module ở [`_discovery/system_map.md`](_discovery/system_map.md) mục 3.

**Bảng mã trạng thái recon:** ⬜ Chưa khảo sát · 🟨 Đang khảo sát · ✅ Đã có tài liệu · ⏸️ Hoãn · ⚪ Chưa implement

### Danh sách prefix đã chiếm

`LOGIN` · `CUST` · `CTC` · `ITEM` · `PRJ` · `TASK` · `PROP` · `EST` · `INV` · `PAY` · `CRN` · `CTR` · `SUB` · `EXP` · `LEAD` · `ESTREQ` · `TKT` · `KBASE` · `DASH` · `RPT` · `HDR` · `PERS` · `UTIL` · `SETUP`

> Module mới **phải** chọn prefix chưa có trong danh sách. Prefix đã cấp là **vĩnh viễn** — kể cả khi module đổi tên trên UI. Lưu ý dễ nhầm: `CTC` = **Contacts**, `CTR` = **Contracts**.

---

## 2. Trạng thái REQ toàn hệ thống

| Module | 🟢 Active | 🟡 Changed | 🔴 Deprecated | ⚪ Chưa implement |
|---|---|---|---|---|
| `LOGIN` | 46 | 0 | 0 | 3 |
| **Tổng** | **46** | **0** | **0** | **3** |

> ⚪ của `LOGIN` (REQ-29, 43, 47) là vế *"tác tạo dùng được"* chưa kiểm chứng (skill mục 4.3.8) — không khẳng định hệ thống chưa build.

## 3. Ambiguity 🔴 High còn treo

| Module | Mã | Câu hỏi (tóm tắt) | Chặn REQ |
|---|---|---|---|
| `LOGIN` | AMB-01 | SRS nói giữ lại Email sau submit lỗi — thực tế không giữ | REQ-LOGIN-16, 20 |
| `LOGIN` | AMB-02 | CSRF token không đổi giữa các lần tải và qua đăng nhập — SRS nói sinh mỗi lần tải | REQ-LOGIN-46 |
| `LOGIN` | AMB-03 | Trang Login phục vụ qua HTTP, không chuyển HTTPS | REQ-LOGIN-49 |
| `LOGIN` | AMB-04 | Xin môi trường / tài khoản test riêng cho luồng sai mật khẩu, inactive, hoa-thường, khoảng trắng | REQ-LOGIN-19, 21, 22, 23 |
| `LOGIN` | AMB-05 | Hệ thống có những role nào — xin account từng role | Ma trận phân quyền `LOGIN` · trùng câu hỏi Q2 cấp hệ thống |

Câu hỏi mở cấp hệ thống (chưa mang mã AMB) nằm ở [`_discovery/system_map.md`](_discovery/system_map.md) mục 6.

## 4. Cấu trúc thư mục chuẩn

```
docs/requirements/
├── README.md                              ← DANH MỤC (file này)
├── _discovery/                            ← TẦNG KHÁM PHÁ — không chứa mã REQ
│   ├── system_map.md                      ← INDEX — TÊN FILE BẤT BIẾN
│   └── modules/module_NN_<slug>.md
└── <module>/
    ├── requirements_<module>.md           ← INDEX — TÊN FILE BẤT BIẾN
    ├── evidence/*.png
    ├── stories/story_NN_<slug>.md         ← chỉ khi tách
    ├── analysis/analysis_<TICKET-ID>.md
    └── impact/impact_<TICKET-ID>.md
```

## 5. Quy trình sử dụng

| Tình huống | Workflow | Ghi vào đâu |
|---|---|---|
| Recon một module trong danh mục | `/generate-requirements-from-website <module>` | `<module>/requirements_<module>.md` · cập nhật dòng module ở bảng 1 |
| Phân tích ticket / tài liệu cho module | `/analyze-requirement-document` | `<module>/analysis/analysis_<TICKET-ID>.md` |
| Cập nhật requirements đã có từ ticket | `/update-requirements-from-ticket` | Sửa tại chỗ + `<module>/impact/impact_<TICKET-ID>.md` |
| Phát hiện module bị sót | `/discover-system` (mode ADD) | `_discovery/` + thêm dòng bảng 1 |
| Hệ thống vừa deploy tính năng mới | `/discover-system` (mode DELTA) | `_discovery/` — nhật ký khám phá |

## 6. Nhật ký danh mục

| Ngày | Thay đổi | Nguồn |
|---|---|---|
| 2026-09-14 | `LOGIN` → ✅ Đã có tài liệu: 49 REQ · 7 Story · 23 AMB (🔴 5) · 9 RISK. Mức phủ tài liệu đổi ⬜ Trắng → 🟨 Một phần (phát hiện SRS trong `Reqs/`, tầng khám phá chưa biết) | `/generate-requirements-from-website LOGIN` |
| 2026-09-14 | Khởi tạo danh mục: 24 module, 24 prefix. `SETUP` ⏸️ Hoãn do thiếu quyền. Danh mục khớp thư mục thực tế (chưa có thư mục module nào) | `/discover-system` mode UI |
