# Impact Report — `CRM-LOGIN-101` · Module `LOGIN` · 29-09-2026

> ⛔ **Đã có đợt tiếp theo:** [impact_CRM-LOGIN-101-B.md](impact_CRM-LOGIN-101-B.md). PO trả lời 10 AMB, thêm `REQ-LOGIN-50` → `54`. File B **gộp** cả đợt này, nên `/update-testcases-from-impact` đọc **file B**, không đọc file này. Giữ file này để truy vết.

> **Nguồn thay đổi:** ticket `CRM-LOGIN-101` — *Khoá tài khoản khi đăng nhập sai nhiều lần*. PO chốt 29-09-2026, nội dung dán qua chat (không có file ticket).
>
> Tài liệu module: [../REQUIREMENTS_LOGIN_SUMMARY.md](../REQUIREMENTS_LOGIN_SUMMARY.md) · REQ chi tiết: [../web/requirements_login_web.md](../web/requirements_login_web.md) mục 3.3 · Danh mục: [../../README.md](../../README.md)
>
> **Mốc git trước khi sửa:** `428e8eb` — bộ TC đối chiếu là bản **working tree** chưa commit của `docs/testcases/login/web/test_cases_login_web.md` (51 TC).

## Nội dung ticket (nguyên văn)

| AC | Nội dung |
|---|---|
| AC#1 | Đăng nhập sai mật khẩu 5 lần liên tiếp với cùng một email thì tài khoản bị khoá 15 phút. |
| AC#2 | Khi bị khoá, trang đăng nhập hiển thị: "Your account is locked. Please try again in 15 minutes." |
| AC#3 | Trong thời gian bị khoá, nhập đúng mật khẩu cũng không đăng nhập được. |
| AC#4 | Đăng nhập thành công trước khi đủ 5 lần thì bộ đếm lần sai được đặt lại về 0. |
| AC#5 | Chỉ khoá theo email, không khoá theo IP. |
| AC#6 | Môi trường dùng chung: TC phải dùng tài khoản Project Manager, KHÔNG dùng tài khoản Admin. |

**Xác nhận thêm của user (29-09-2026):** (1) **có** tài khoản `Project Manager` để kiểm thử · (2) tính năng **chưa deploy** lên `crm.anhtester.com`.

---

## Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới | 5 (trạng thái ⚪ — chưa deploy) | `REQ-LOGIN-45` → `49` |
| 🟡 Sửa | 1 | `REQ-LOGIN-41` — từ *không khoá* sang *khoá sau 5 lần sai* |
| 🔴 Bỏ | 0 | — |
| ⏸️ Trùng, không tác động | 0 | — |
| Không cấp REQ | 1 | AC#6 — ràng buộc **kiểm thử**, không phải hành vi hệ thống → ghi vào `RISK-LOGIN-03` và AC của các REQ |

| Ánh xạ AC → REQ | |
|---|---|
| AC#1 — ngưỡng 5 lần | `REQ-LOGIN-41` 🟡 |
| AC#1 — thời gian 15 phút | `REQ-LOGIN-45` ⚪ |
| AC#2 | `REQ-LOGIN-46` ⚪ |
| AC#3 | `REQ-LOGIN-47` ⚪ |
| AC#4 | `REQ-LOGIN-48` ⚪ |
| AC#5 | `REQ-LOGIN-49` ⚪ |

**Tổng REQ module:** 44 → **49** · **Phạm vi viết TC:** 40 → **45** · Story: STORY-LOGIN-03 9 → **14** REQ.

> ⚠️ **Ticket đảo một quyết định đã chốt.** `REQ-LOGIN-41` trước đây ghi *"hệ thống không khoá tài khoản"* (PO trả lời `AMB-LOGIN-02` ngày 18-08-2026) và bộ TC đã dựa vào đó để coi việc thử sai lặp lại là **an toàn**. Kết luận đó hết hiệu lực ngay khi tính năng được deploy.

---

## 🚨 Việc khẩn — phải xong TRƯỚC ngày deploy

Sau khi deploy, **chạy bộ TC hiện tại sẽ khoá tài khoản `Admin` trên môi trường dùng chung**:

| Nguồn khoá | Số lần sai với `admin@example.com` |
|---|---|
| `CRM_LOGIN_TC_015-a` | **6** lần liên tiếp — tự nó đã vượt ngưỡng |
| `TC_013-a` · `TC_016` · `TC_017-b` · `TC_047-a,b` · `TC_050` (Chrome, Edge, Firefox) | 1 + 1 + 1 + 2 + 3 = **8** lần — chạy liền nhau không có lần đăng nhập đúng xen giữa là vượt ngưỡng |

→ Admin bị khoá 15 phút, cả đội bị chặn. Đây là lý do `RISK-LOGIN-03` được **mở lại**. Cần chạy `/update-testcases-from-impact` **trước** khi PO báo deploy (`AMB-LOGIN-30`).

---

## Test case cần xử lý

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| `CRM_LOGIN_TC_015` | REQ-LOGIN-41 | ⚠️ **Viết lại toàn bộ** + 🚨 khẩn | Kỳ vọng **đảo ngược**: từ "không khoá" sang "khoá sau 5 lần". Đổi tài khoản `Admin` → `Project Manager` (AC#6). Biến thể `b` (5 email khác nhau, không khoá theo IP) chuyển neo sang `REQ-LOGIN-49`. Gắn skip tới khi deploy |
| `CRM_LOGIN_TC_013` | REQ-LOGIN-14, 15 | ⚠️ Review & sửa | Biến thể `a` gửi sai mật khẩu với `admin@example.com` → cộng bộ đếm Admin. Biến thể này **cần** một email có thật để so với email không tồn tại (REQ-15) → đổi sang `Project Manager`. Kèm: kết quả so sánh phụ thuộc `AMB-LOGIN-21` |
| `CRM_LOGIN_TC_016` | REQ-LOGIN-16 | ⚠️ Review & sửa | Gửi sai mật khẩu với `admin@example.com`. TC chỉ cần kiểm ô Email giữ giá trị → đổi sang email không tồn tại hoặc `Project Manager` |
| `CRM_LOGIN_TC_017` | REQ-LOGIN-14 | ⚠️ Review & sửa | Biến thể `b` (tiêm SQL) dùng `admin@example.com` → đổi sang `Project Manager` hoặc email không tồn tại |
| `CRM_LOGIN_TC_047` | REQ-LOGIN-14 | ⚠️ Review & sửa | 2 biến thể gửi mật khẩu rất dài với `admin@example.com` → đổi email |
| `CRM_LOGIN_TC_050` | REQ-LOGIN-02, 06, 14 | ⚠️ Review & sửa | Bước 5 gửi sai mật khẩu với `admin@example.com` ở **mỗi** trình duyệt (3 lần) → đổi email ở bước 5 |
| `CRM_LOGIN_TC_011` | REQ-LOGIN-10, 11, 12 | ⚠️ Review có điều kiện | Biến thể `c` gửi `admin@example.com` + mật khẩu **trống**. Chỉ cộng bộ đếm nếu PO trả lời `AMB-LOGIN-23` là *có tính*. Đổi sang email không tồn tại cho chắc — TC không cần email có thật |
| Nhóm C (ghi chú đầu nhóm) | REQ-LOGIN-41 | ⚠️ Sửa ghi chú | Câu *"🔓 Thử sai mật khẩu lặp lại là an toàn — REQ-LOGIN-41 … RISK-LOGIN-03 đã đóng"* **không còn đúng** |
| — | REQ-LOGIN-45 | ➕ Viết mới, skip | Tự mở khoá sau 15 phút. TC chạy **trên 15 phút**, không xếp vào smoke. Không gửi biểu mẫu nào trong lúc chờ (`AMB-LOGIN-26`) |
| — | REQ-LOGIN-46 | ➕ Viết mới, skip | Thông báo khoá nguyên văn. Gắn `@AssumptionBased` (`AMB-LOGIN-21`, `28`) |
| — | REQ-LOGIN-47 | ➕ Viết mới, skip | Đúng mật khẩu vẫn bị chặn khi đang khoá |
| — | REQ-LOGIN-48 | ➕ Viết mới, skip | 4 sai → đúng → 4 sai → đúng vẫn thành công |
| — | REQ-LOGIN-49 | ➕ Viết mới, skip | AC1 cần tài khoản thứ hai không phải Admin — gắn `@AssumptionBased` (`AMB-LOGIN-29`) · AC2 nhận biến thể `b` cũ của TC_015 |
| `CRM_LOGIN_TC_006` | REQ-LOGIN-06 | ♻️ Khôi phục (gỡ `@Deprecated`) | User xác nhận 29-09-2026 **có tài khoản `Project Manager`** — lý do Deprecated ngày 24-09-2026 (*"hệ thống chỉ có role Admin"*) là **sai**. Giữ nguyên TC ID |
| `CRM_LOGIN_TC_023` | REQ-LOGIN-19, 20 | ♻️ Khôi phục biến thể `c`, `d` | Cùng lý do — hai biến thể vai trò `Project Manager` bị bỏ 24-09-2026 |
| `CRM_LOGIN_TC_019`, `TC_020` | REQ-LOGIN-43 | ❔ Hỏi user trước | Cần tài khoản `Customer`. User mới xác nhận tài khoản **PM**; tài khoản Customer chưa được xác nhận lại. Có thì khôi phục như TC_006 |

> **Mốc của bộ TC:** TC ID giữ nguyên — **không** đánh lại. TC_015 sửa **tại chỗ**, không Deprecated rồi viết TC mới, vì REQ-LOGIN-41 giữ nguyên mã.

---

## Ambiguity

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| `AMB-LOGIN-02` | ✅ giữ nguyên, **ghi chú bị thay thế** | Kết luận cũ *"không khoá"* bị ticket đảo. **Khác** kết luận cũ → mọi TC dựa trên nó phải sửa (bảng trên) |
| `AMB-LOGIN-21` | (mới) ❓ 🔴 | Email không tồn tại / email khách hàng có hiện thông báo khoá không — nguy cơ lộ email, phá `REQ-LOGIN-15` |
| `AMB-LOGIN-22` | (mới) ❓ 🟡 | Lần sai thứ 5 hiện thông báo gì |
| `AMB-LOGIN-23` | (mới) ❓ 🟡 | Lần gửi nào tính là "sai" (bỏ trống mật khẩu, lỗi định dạng, CSRF, tài khoản khách hàng) |
| `AMB-LOGIN-24` | (mới) ❓ 🟡 | "Liên tiếp" có cửa sổ thời gian không |
| `AMB-LOGIN-25` | (mới) ❓ 🟡 | Hết khoá thì bộ đếm về 0 hay chưa |
| `AMB-LOGIN-26` | (mới) ❓ 🟡 | Mốc 15 phút tính từ đâu, gửi thêm trong lúc khoá có gia hạn không |
| `AMB-LOGIN-27` | (mới) ❓ 🟡 | Email khác kiểu chữ / có khoảng trắng có tính chung bộ đếm không |
| `AMB-LOGIN-28` | (mới) ❓ 🟢 | Chuỗi "15 minutes" cố định hay theo thời gian còn lại |
| `AMB-LOGIN-29` | (mới) ❓ 🟡 | Tài khoản thứ hai (ngoài Admin/PM) cho `REQ-LOGIN-49` AC1 |
| `AMB-LOGIN-30` | (mới) ❓ 🔴 | Lịch deploy / bản build có tính năng |

Ambiguity còn treo của module: **0 → 10**.

## Rủi ro

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| `RISK-LOGIN-03` | ✅ Đóng → 🔓 **Mở lại** | Thử sai mật khẩu có thể khoá tài khoản dùng chung — cả khi cố ý (TC khoá PM) lẫn do cộng dồn (TC gửi sai bằng Admin) |
| `RISK-LOGIN-01` | Cập nhật | Sau khi deploy chặn được dò **một** tài khoản, nhưng không chặn dò nhiều tài khoản từ một IP |
| `RISK-LOGIN-09` | (mới) | Khoá theo email → ai biết email cũng khoá được chủ tài khoản 15 phút, lặp lại vô hạn. Hệ quả thiết kế — báo PO/bảo mật |

---

## Cảnh báo

- 🚨 **Sửa TC trước ngày deploy.** Bộ TC hiện tại gửi sai mật khẩu với `admin@example.com` ít nhất 14 lần (TC_015-a 6 lần + 8 lần rải ở 5 TC khác). Sau khi deploy, chạy bộ này sẽ khoá Admin trên môi trường dùng chung.
- 🔀 **Mâu thuẫn tài khoản đã gỡ.** Bộ TC (24-09-2026) ghi *"hệ thống chỉ có role Admin"*, trong khi requirements ghi 3 vai trò. User xác nhận 29-09-2026 **có tài khoản PM** → requirements đúng. Tài khoản `Customer` **chưa xác nhận lại**.
- 🔒 **Khoá PM cũng chặn người khác.** Tài khoản `Project Manager` dùng chung — mỗi lần chạy TC khoá tài khoản sẽ khoá PM 15 phút. Báo đội trước khi chạy.
- ⏱️ `REQ-LOGIN-45` chạy trên 15 phút → không xếp vào smoke.
- ❔ **Chưa kiểm chứng trên UI.** Cả 6 REQ khoá tài khoản chỉ có nguồn là ticket. Khi PO báo deploy (`AMB-LOGIN-30`), cần một lượt kiểm chứng thực tế bằng tài khoản PM để chuyển ⚪ → 🟢 và trả lời `AMB-LOGIN-22`, `26`, `28` bằng quan sát thật.

---

## Bước kế tiếp

```
/update-testcases-from-impact docs/requirements/login/impact/impact_CRM-LOGIN-101.md
```

Sau đó chạy `/generate-traceability-matrix` để cập nhật RTM.
