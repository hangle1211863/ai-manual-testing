# Impact Report — `CRM-LOGIN-101-B` · Module `LOGIN` · 29-09-2026

> **Nguồn thay đổi:** PO trả lời **10 ambiguity** `AMB-LOGIN-21` → `30`, mở ra từ ticket `CRM-LOGIN-101` trong cùng ngày. Trả lời qua chat, không có file.
>
> 📌 **File này GỘP cả đợt A** ([impact_CRM-LOGIN-101.md](impact_CRM-LOGIN-101.md)). Bộ TC **chưa** được cập nhật theo đợt A, nên `/update-testcases-from-impact` chỉ cần đọc **file này**. Bảng "Test case cần xử lý" bên dưới là bảng **cuối cùng**, thay thế bảng của đợt A.
>
> Tài liệu module: [../REQUIREMENTS_LOGIN_SUMMARY.md](../REQUIREMENTS_LOGIN_SUMMARY.md) · REQ chi tiết: [../web/requirements_login_web.md](../web/requirements_login_web.md) mục 3.3 · Danh mục: [../../README.md](../../README.md)
>
> **Mốc git trước khi sửa:** `428e8eb`. Bộ TC đối chiếu là bản **working tree** chưa commit của `docs/testcases/login/web/test_cases_login_web.md` (51 TC).

## Câu trả lời của PO (nguyên văn)

| AMB | Câu trả lời | So với Giả định tạm |
|---|---|---|
| `AMB-LOGIN-21` | không (email không tồn tại không bị khoá) | ⚠️ **KHÁC** |
| `AMB-LOGIN-30` | 30/09/2026 | — |
| `AMB-LOGIN-22` | hiện luôn `Your account is locked…` ngay lần 5 | Trùng |
| `AMB-LOGIN-23` | chỉ tính khi đủ email + mật khẩu, đúng định dạng, sai mật khẩu | Trùng |
| `AMB-LOGIN-24` | không giới hạn thời gian. Bộ đếm chỉ về 0 khi đăng nhập thành công hoặc hết khoá | Trùng |
| `AMB-LOGIN-25` | Hết 15 phút khoá thì bộ đếm về 0 | Trùng |
| `AMB-LOGIN-26` | tính từ lần sai thứ 5, không gia hạn | Trùng |
| `AMB-LOGIN-27` | tính chung | Trùng |
| `AMB-LOGIN-29` | được dùng Admin đúng 1 lần | Trùng |
| `AMB-LOGIN-28` | cố định, đúng nguyên văn ticket | Trùng |
| Tài khoản `Customer` | skip | — không xác nhận |

---

## Tóm tắt (gộp đợt A + B)

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới (⚪ chờ deploy) | 10 | `REQ-LOGIN-45` → `49` (đợt A) · `REQ-LOGIN-50` → `54` (đợt B) |
| 🟡 Sửa | 2 | `REQ-LOGIN-41` (đợt A — không khoá → khoá sau 5 lần) · `REQ-LOGIN-15` (đợt B — thu hẹp phạm vi) |
| ✏️ Biên tập AC, không đổi hành vi | 4 | `REQ-LOGIN-41`, `45`, `46`, `49` — điền kết luận AMB |
| 🔴 Bỏ | 0 | — |

**REQ mới của đợt B — mỗi câu trả lời sinh một rule kiểm được độc lập:**

| REQ | Rule | Từ AMB |
|---|---|---|
| `REQ-LOGIN-50` | Email không có tài khoản khu quản trị (không tồn tại / tài khoản khách hàng) **không** bị khoá | `AMB-LOGIN-21` |
| `REQ-LOGIN-51` | Hết khoá thì bộ đếm về 0 | `AMB-LOGIN-25` |
| `REQ-LOGIN-52` | Lần gửi bị chặn ở bước kiểm tra dữ liệu không tính là lần sai | `AMB-LOGIN-23` |
| `REQ-LOGIN-53` | Gửi biểu mẫu trong lúc khoá không gia hạn thời gian khoá | `AMB-LOGIN-26` |
| `REQ-LOGIN-54` | Email khác kiểu chữ / thừa khoảng trắng tính chung một bộ đếm | `AMB-LOGIN-27` |

`AMB-LOGIN-22`, `24`, `28`, `29` **không** sinh REQ mới, vì kết luận chỉ làm rõ AC của REQ có sẵn. Riêng `AMB-24` (bộ đếm không hết hạn theo thời gian) là khẳng định phủ định trên khoảng thời gian không giới hạn, không TC nào chứng minh được nên chỉ ghi vào AC của `REQ-LOGIN-41`.

**Module:** 44 → 49 → **54 REQ** · phạm vi viết TC **50** · STORY-LOGIN-03 **19** REQ · ambiguity treo **0** (30/30).

---

## ⚠️ Kết luận khác Giả định tạm — `AMB-LOGIN-21`

Giả định tạm là email có thật và email không tồn tại cho kết quả **giống hệt** nhau. PO chốt: email không tồn tại **không** bị khoá.

| Lần sai thứ | Email có tài khoản (PM) | Email không tồn tại |
|---|---|---|
| 1 → 4 | `Invalid email or password` | `Invalid email or password` |
| **5 trở đi** | `Your account is locked. Please try again in 15 minutes.` | `Invalid email or password` |

→ Từ lần thứ 5, **nhìn thông báo là biết email có tài khoản**. Hệ quả:
- `REQ-LOGIN-15` 🟡: thu hẹp còn *"dưới ngưỡng khoá"*, đổi email có thật từ `Admin` sang `Project Manager`.
- `RISK-LOGIN-10` mới, **PO đã chấp nhận**. Không viết TC cố chứng minh lỗ hổng.

---

## 🚨 Deploy NGÀY MAI 30-09-2026: phải sửa TC TRONG HÔM NAY

Sau khi deploy, chạy bộ TC hiện tại sẽ **khoá tài khoản `Admin` trên môi trường dùng chung** 15 phút:

| Nguồn | Số lần sai với `admin@example.com` |
|---|---|
| `CRM_LOGIN_TC_015-a` | **6** lần liên tiếp — tự nó đã vượt ngưỡng |
| `TC_013-a` · `TC_016` · `TC_017-b` · `TC_047-a,b` · `TC_050` × 3 trình duyệt | 1 + 1 + 1 + 2 + 3 = **8** lần |

✅ **Tin tốt từ đợt B:** email không tồn tại **không bao giờ bị khoá** (`REQ-LOGIN-50`), nên mọi TC chỉ cần lấy thông báo lỗi đều đổi sang `notexist_<timestamp>@auto.test` được, an toàn tuyệt đối. `TC_011-c` (bỏ trống mật khẩu) **không** cộng bộ đếm (`REQ-LOGIN-52`), nên không phải sửa.

---

## Test case cần xử lý (bảng cuối cùng, thay thế đợt A)

### A. Sửa TC đang có. 🚨 Xong trước 30-09-2026

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| `CRM_LOGIN_TC_015` | REQ-LOGIN-41 | ⚠️ **Viết lại toàn bộ** | Kỳ vọng đảo ngược: không khoá → **khoá sau 5 lần**, thông báo khoá hiện **ngay lần 5**. Đổi `Admin` → `Project Manager`. Mở đầu bằng 1 lần đăng nhập đúng để bộ đếm = 0. Thêm vế AC1 (4 sai → đúng vẫn thành công). Biến thể `b` (5 email không tồn tại khác nhau) chuyển neo sang `REQ-LOGIN-49` AC2. Gắn skip tới khi kiểm chứng bản deploy |
| `CRM_LOGIN_TC_013` | REQ-LOGIN-14, 15 | ⚠️ Review & sửa | `REQ-15` 🟡 thu hẹp. Biến thể `a` đổi `admin@example.com` → `Project Manager` (cần email có thật để so với email không tồn tại). Ghi rõ TC chỉ đúng **dưới ngưỡng khoá** — chạy 1 lần sai với PM |
| `CRM_LOGIN_TC_016` | REQ-LOGIN-16 | ⚠️ Sửa dữ liệu | Đổi `admin@example.com` → email không tồn tại. TC chỉ kiểm ô Email giữ giá trị, không cần email có thật |
| `CRM_LOGIN_TC_017` | REQ-LOGIN-14 | ⚠️ Sửa dữ liệu | Biến thể `b` (tiêm SQL) đổi `admin@example.com` → email không tồn tại |
| `CRM_LOGIN_TC_047` | REQ-LOGIN-14 | ⚠️ Sửa dữ liệu | 2 biến thể đổi `admin@example.com` → email không tồn tại |
| `CRM_LOGIN_TC_050` | REQ-LOGIN-02, 06, 14 | ⚠️ Sửa dữ liệu | Bước 5 (mật khẩu sai, mỗi trình duyệt 1 lần) đổi sang email không tồn tại. Bước 3 (đăng nhập đúng bằng Admin) giữ nguyên |
| Nhóm C — ghi chú đầu nhóm | REQ-LOGIN-41 | ⚠️ Viết lại ghi chú | Câu *"🔓 Thử sai mật khẩu lặp lại là an toàn… RISK-LOGIN-03 đã đóng"* sai kể từ 30-09-2026. Thay bằng: *cấm gửi sai mật khẩu bằng `Admin`; cần thông báo lỗi thì dùng email không tồn tại (không bao giờ bị khoá — `REQ-LOGIN-50`); cần email có thật thì dùng `Project Manager`* |
| `CRM_LOGIN_TC_011` | REQ-LOGIN-10, 11, 12 | ✅ Không sửa | Biến thể `c` bỏ trống mật khẩu **không** cộng bộ đếm (`AMB-LOGIN-23` ✅) |

### B. Viết mới. Skip tới khi kiểm chứng trên bản deploy

Dùng tài khoản **`Project Manager`**. Mọi TC **bắt buộc** mở đầu bằng 1 lần đăng nhập đúng để đưa bộ đếm về 0 (`AMB-LOGIN-24`). **Không** gắn `@AssumptionBased`, vì mọi kỳ vọng đã được PO chốt.

| REQ | Nội dung TC | Ghi chú |
|---|---|---|
| `REQ-LOGIN-45` | Khoá lúc T0, **không** gửi gì, T0+16 phút đăng nhập đúng → thành công | ⏱️ > 15 phút, `@Slow`, không vào smoke |
| `REQ-LOGIN-46` | Lần sai **thứ 5** đã hiện nguyên văn `Your account is locked. Please try again in 15 minutes.`, và vẫn nguyên chuỗi đó ở các lần sau | Assert **khớp nguyên văn** (chuỗi cố định — `AMB-LOGIN-28`) |
| `REQ-LOGIN-47` | Đang khoá → nhập đúng mật khẩu → không tạo phiên; mở `/admin/` bị đưa về trang đăng nhập | Ghép chung phiên chạy với 46 được, chấm riêng |
| `REQ-LOGIN-48` | 4 sai → đúng → đăng xuất → 4 sai → đúng vẫn thành công | |
| `REQ-LOGIN-49` | AC1: PM bị khoá → **Admin đăng nhập đúng 1 lần** từ cùng trình duyệt → thành công · AC2: 5 email không tồn tại khác nhau × 1 lần → PM đăng nhập đúng → thành công | Admin **chỉ** 1 lần đăng nhập đúng, không gửi sai (`AMB-LOGIN-29`). AC2 nhận biến thể `b` cũ của TC_015 |
| `REQ-LOGIN-50` | Biến thể `a`: email không tồn tại × 6 lần → cả 6 lần `Invalid email or password` | ⏭️ Biến thể `b` (tài khoản khách hàng) **không viết**, user skip xác nhận tài khoản `Customer` |
| `REQ-LOGIN-51` | Khoá lúc T0 → T0+16 sai 1 lần → `Invalid email or password` (không khoá) → đúng → thành công | ⏱️ > 15 phút. Ghép phiên chạy với 45 được, chấm riêng |
| `REQ-LOGIN-52` | 4 sai → 3 lần bỏ trống mật khẩu → đúng → thành công | |
| `REQ-LOGIN-53` | Khoá lúc T0 → gửi sai ở T0+5 và T0+10 → T0+16 đúng → thành công | ⏱️ > 15 phút |
| `REQ-LOGIN-54` | Sai với 5 cách viết email PM (thường · HOA · Lẫn · 2 khoảng trắng đầu · 2 khoảng trắng cuối) → lần 5 khoá → email gốc + đúng → bị từ chối | |

### C. Khôi phục TC Deprecated sai lý do

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| `CRM_LOGIN_TC_006` | REQ-LOGIN-06 | ♻️ Khôi phục, gỡ `@Deprecated` | User xác nhận 29-09-2026 **có tài khoản `Project Manager`**. Lý do Deprecated ngày 24-09-2026 (*"chỉ có role Admin"*) là sai. Giữ nguyên TC ID |
| `CRM_LOGIN_TC_023` | REQ-LOGIN-19, 20 | ♻️ Khôi phục biến thể `c`, `d` | Cùng lý do |
| `CRM_LOGIN_TC_019`, `TC_020` | REQ-LOGIN-43 | ⏭️ **Giữ `@Deprecated`** | User **skip** xác nhận tài khoản `Customer` (29-09-2026). Sửa lý do Deprecated thành *"chưa xác nhận tài khoản Customer"* thay cho *"chỉ có role Admin"* |

> TC ID giữ nguyên, **không** đánh lại. TC_015 sửa **tại chỗ**, vì REQ-LOGIN-41 giữ nguyên mã.

---

## Ambiguity

| Mã | Chuyển trạng thái | Kết luận |
|---|---|---|
| `AMB-LOGIN-21` | ❓ → ✅ | ⚠️ **Khác giả định** — email không tồn tại không bị khoá → `REQ-LOGIN-50`, `REQ-LOGIN-15` 🟡, `RISK-LOGIN-10` |
| `AMB-LOGIN-22` | ❓ → ✅ | Thông báo khoá hiện ngay lần 5 → AC của `41`, `46` |
| `AMB-LOGIN-23` | ❓ → ✅ | Chỉ tính sai mật khẩu thật → `REQ-LOGIN-52` |
| `AMB-LOGIN-24` | ❓ → ✅ | Không giới hạn thời gian → AC của `41` |
| `AMB-LOGIN-25` | ❓ → ✅ | Hết khoá bộ đếm về 0 → `REQ-LOGIN-51` |
| `AMB-LOGIN-26` | ❓ → ✅ | Tính từ lần 5, không gia hạn → AC của `45` + `REQ-LOGIN-53` |
| `AMB-LOGIN-27` | ❓ → ✅ | Tính chung bộ đếm → `REQ-LOGIN-54` |
| `AMB-LOGIN-28` | ❓ → ✅ | Chuỗi cố định → AC của `46` |
| `AMB-LOGIN-29` | ❓ → ✅ | Được dùng Admin 1 lần đăng nhập đúng → AC của `49` |
| `AMB-LOGIN-30` | ❓ → ✅ | Deploy 30-09-2026 |

Ambiguity còn treo: **10 → 0**.

## Rủi ro

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| `RISK-LOGIN-10` | (mới) ⚠️ **Đã chấp nhận** | Thông báo khoá cho biết email nào có tài khoản — hệ quả của `AMB-LOGIN-21` |
| `RISK-LOGIN-03` | 🔓 Mở (từ đợt A) · cập nhật | Ghi rõ hạn chót 30-09-2026 và ngoại lệ Admin 1 lần đăng nhập đúng |
| `RISK-LOGIN-01` | Cập nhật | Né ngưỡng bằng đổi kiểu chữ email đã được chặn ở mức yêu cầu (`REQ-LOGIN-54`) |

---

## Cảnh báo

- 🚨 **Hạn chót hôm nay 29-09-2026**: sửa 6 TC ở mục A. Không sửa kịp thì **tạm ngưng chạy** `TC_013`, `015`, `016`, `017`, `047`, `050` từ 30-09-2026.
- 🔒 **Khoá PM chặn cả đội**: 6 TC khoá tài khoản (`41`, `46`, `47`, `49`, `53`, `54`) mỗi lần chạy khoá PM 15 phút. Báo đội và gom vào một khung giờ.
- ⏱️ **4 TC chạy trên 15 phút** (`45`, `51`, `53` + chờ hết khoá sau mỗi TC khoá). Ghép các vế chung một lần khoá để giảm thời gian, nhưng chấm riêng từng REQ.
- ❔ **Sau deploy phải kiểm chứng**: cả 11 REQ khoá tài khoản mới chỉ có nguồn là ticket và câu trả lời PO. Sau lượt kiểm chứng trên UI ngày 30-09-2026, chạy lại `/update-requirements-from-ticket` để chuyển ⚪ → 🟢 (hoặc mở bug nếu lệch).

---

## Bước kế tiếp

```
/update-testcases-from-impact docs/requirements/login/impact/impact_CRM-LOGIN-101-B.md
```
