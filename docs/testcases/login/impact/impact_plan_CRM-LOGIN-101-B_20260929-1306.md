# Kế hoạch cập nhật Test Cases — `CRM-LOGIN-101-B` · module `LOGIN`

| Mục | Giá trị |
|---|---|
| Mã delta | `CRM-LOGIN-101-B` — ticket `CRM-LOGIN-101` (*Khoá tài khoản khi đăng nhập sai nhiều lần*) + câu trả lời PO cho `AMB-LOGIN-21` → `30` |
| Ngày lập | 29-09-2026 13:06 |
| Mode | **PLAN** — chưa sửa TC nào. Chờ user duyệt để chạy APPLY |
| Impact Report nguồn | [impact_CRM-LOGIN-101-B.md](../../../requirements/login/impact/impact_CRM-LOGIN-101-B.md) — **gộp** cả đợt A |
| Impact Report user chỉ định | [impact_CRM-LOGIN-101.md](../../../requirements/login/impact/impact_CRM-LOGIN-101.md) — ⛔ **đã bị thay thế**: đầu file ghi *"`/update-testcases-from-impact` đọc file B, không đọc file này"*. Lập kế hoạch theo đợt A sẽ sai 3 chỗ: `TC_011` bị đánh dấu phải sửa (đợt B: không cần), thiếu `REQ-LOGIN-15` 🟡, thiếu 5 REQ `50` → `54` |
| Bộ TC hiện hành | [TEST_CASES_LOGIN_SUMMARY.md](../TEST_CASES_LOGIN_SUMMARY.md) → [web/test_cases_login_web.md](../web/test_cases_login_web.md) — 51 TC (47 hiệu lực · 4 `@Deprecated`) · 80 biến thể · độ hạt **GỘP** · 1 nền tảng (web) |
| Automation | Chưa có script nào trỏ vào bộ TC → **không** có việc cho `/update-automation-from-impact` |
| Hạn chót | 🚨 **Hết ngày 29-09-2026** — deploy 30-09-2026 (`AMB-LOGIN-30` ✅) |

---

## 1. REQ đã đổi

| REQ | Loại | Trước | Sau |
|---|---|---|---|
| `REQ-LOGIN-41` | 🟡 Sửa — **đảo ngược** | Không khoá tài khoản, không khoá theo IP (PO 18-08-2026, `AMB-LOGIN-02`) | Sai **5 lần liên tiếp** cùng email → khoá; thông báo khoá hiện **ngay lần 5**. AC1: 4 sai → đúng vẫn thành công. Chỉ tính sai mật khẩu thật. Dùng `Project Manager`, **CẤM** `Admin`. ⚪ deploy 30-09-2026 |
| `REQ-LOGIN-15` | 🟡 Sửa — thu hẹp | Thông báo lỗi không tiết lộ email nào có thật (email có thật: `admin@example.com`) | Chỉ đúng **dưới ngưỡng khoá**. Email có thật đổi sang `Project Manager`, bộ đếm < 4 trước khi gửi, gửi sai **1 lần** |
| `REQ-LOGIN-06`, `19`, `20` | Không đổi REQ — **đổi dữ kiện** | Bộ TC (24-09-2026) ghi *"hệ thống chỉ có role Admin"* | User xác nhận 29-09-2026 **có** tài khoản `Project Manager` → lý do Deprecated của `TC_006`, `TC_023-c,d` sai |
| `REQ-LOGIN-43` | Không đổi REQ | TC_019, TC_020 Deprecated vì *"chỉ có role Admin"* | Vẫn Deprecated nhưng lý do đúng là *"chưa xác nhận tài khoản Customer"* (user skip 29-09-2026) |
| `REQ-LOGIN-45` → `54` | 🟢 Mới (⚪ chờ deploy) | — | 10 REQ khoá tài khoản — **ngoài phạm vi** workflow này, xem mục 7 |

## 2. Ánh xạ REQ → TC

### ✅ Mapping chắc chắn (cột `REQ ID` + Bảng Đối Soát Coverage)

| REQ | TC | Vị trí | Cách map |
|---|---|---|---|
| `REQ-LOGIN-41` | `CRM_LOGIN_TC_015` (`a`, `b`) | `web/test_cases_login_web.md:51` (Nhóm C) | Cột `REQ ID` · Coverage `TEST_CASES_LOGIN_SUMMARY.md:121` |
| `REQ-LOGIN-15` | `CRM_LOGIN_TC_013` (`a`, `b`, bước 5) | `:49` (Nhóm C) | Cột `REQ ID` · Coverage `:99` |
| `REQ-LOGIN-06` | `CRM_LOGIN_TC_006` | `:31` (Nhóm B) | Cột `REQ ID` |
| `REQ-LOGIN-19`, `20` | `CRM_LOGIN_TC_023` (`c`, `d` đã bỏ) | `:70` (Nhóm D) | Cột `REQ ID` · bản trước khi bỏ ở `git show 428e8eb:docs/testcases/login/web/test_cases_login_web.md` |
| `REQ-LOGIN-43` | `CRM_LOGIN_TC_019`, `TC_020` | `:55`, `:56` (Nhóm C) | Cột `REQ ID` · Coverage `:123` |

### ⚠️ Tác động qua dữ liệu test — không map qua REQ, map qua **tài khoản dùng chung**

Các TC dưới đây **không** neo vào REQ đổi, nhưng gửi sai mật khẩu với `admin@example.com` → cộng dồn bộ đếm của Admin. Đã rà **toàn bộ 51 TC** (grep `admin@example.com`), không chỉ danh sách của Impact Report:

| TC | Vị trí | Lần sai với Admin | Kết luận |
|---|---|---|---|
| `TC_013-a` | `:49` | 1 | ⚠️ Sửa (trùng mục ✅ ở trên) |
| `TC_015-a` | `:51` | **6** | ⚠️ Viết lại |
| `TC_016` | `:52` | 1 | ⚠️ Sửa dữ liệu |
| `TC_017-b` | `:53` | 1 | ⚠️ Sửa dữ liệu |
| `TC_047-a,b` | `:136` | 2 | ⚠️ Sửa dữ liệu |
| `TC_050-a,b,c` bước 5 | `:148` | 3 | ⚠️ Sửa dữ liệu |
| `TC_011-c` | `:47` | 0 — mật khẩu **trống**, không tính (`REQ-LOGIN-52`) | ✅ Không sửa (theo Impact Report) — xem mục 8 câu hỏi 3 |
| `TC_012-e` | `:48` | 0 — bị **trình duyệt** chặn, không gửi request | ✅ Không sửa *(Impact Report không nhắc — agent tự rà)* |
| `TC_024`, `TC_040` | `:71`, `:109` | 0 — mật khẩu **đúng** | ✅ Không sửa *(agent tự rà)* |
| `TC_025-a,b` | Nhóm D | 0 — mật khẩu đúng, bị chặn ở CSRF | ✅ Không sửa *(agent tự rà)* |
| `TC_014` | `:50` | — | `@Deprecated`, không chạy |

### ❓ REQ chưa có TC

`REQ-LOGIN-45`, `46`, `47`, `48`, `49`, `50`, `51`, `52`, `53`, `54` — chuyển mục 7.

## 3. Kế hoạch sửa từng TC

> Tất cả sửa **tại chỗ** trong `web/test_cases_login_web.md`, giữ nguyên TC ID. `<ts>` = timestamp lúc chạy, theo quy ước `notexist_<ts>@auto.test` đang dùng trong bộ TC.

### Nhóm A — Chặn khoá tài khoản Admin (🚨 xong trong 29-09-2026)

| # | TC | Vòng · Nhánh | Ô sửa | Nội dung sửa | Evidence |
|---|---|---|---|---|---|
| A1 | `TC_015` `:51` | V2 · Business Rule | **Toàn dòng** trừ TC ID | Viết lại theo `REQ-LOGIN-41` — xem 3.1 | ❌ Không có — UI chưa deploy. Thông báo khoá lấy **nguyên văn** từ ticket (`AMB-LOGIN-28` ✅ chuỗi cố định) |
| A2 | `TC_013` `:49` | V3 · Security (+ V2 · EP) | `Test Scenario` · `Pre-Condition` · `Test Data` biến thể `a` · `Expected` bước 5 | • Scenario: thêm *"… (dưới ngưỡng khoá)"*<br>• Pre-Condition: thêm *"Bộ đếm lần sai của PM = 0: đăng nhập đúng bằng `PM_EMAIL`/`PM_PASSWORD` rồi đăng xuất"*<br>• `a`: `admin@example.com` → giá trị `PM_EMAIL` trong `.env` / `SaiMatKhau_<ts>` — **đúng 1 lần**<br>• Expected bước 5: thêm 📌 *"Chỉ đúng dưới ngưỡng khoá. Từ lần sai thứ 5, email có thật hiện thông báo khoá còn email không tồn tại thì không — rủi ro PO chấp nhận (`RISK-LOGIN-10`), không phải FAIL của TC này"* | ❌ Không cần — message giữ nguyên `Invalid email or password` (đã có evidence `login_form_wrong_credentials_fullpage.png`) |
| A3 | `TC_016` `:52` | V2 · Error Guessing | `Test Steps` bước 2 · `Test Data` · `Expected` bước 5 | `admin@example.com` → `notexist_<ts>@auto.test` ở cả 3 ô. Nhãn 🐞 `@KnownBug` giữ nguyên | ❌ Không cần |
| A4 | `TC_017` `:53` | V3 · Security | `Test Data` biến thể `b` | `admin@example.com` → `notexist_<ts>@auto.test` | ❌ Không cần |
| A5 | `TC_047` `:136` | V2 · Error Guessing | `Test Steps` bước 1 · `Test Data` | `admin@example.com` → `notexist_<ts>@auto.test` | ❌ Không cần |
| A6 | `TC_050` `:148` | V4 · Compatibility | `Test Steps` bước 5 · `Test Data` | Bước 5: *"Đăng xuất, gửi `notexist_<ts>@auto.test` + mật khẩu `SaiMatKhau_<ts>`"*. Bước 3 (Admin đúng mật khẩu) **giữ nguyên** | ❌ Không cần |
| A7 | Ghi chú đầu Nhóm C `:41` | — | Blockquote `🔓` | Thay bằng: *"🔒 **CẤM gửi sai mật khẩu bằng `Admin`** — từ 30-09-2026 sai 5 lần liên tiếp sẽ khoá tài khoản dùng chung 15 phút (`REQ-LOGIN-41`, `RISK-LOGIN-03` mở lại). Cần thông báo lỗi → dùng `notexist_<ts>@auto.test` (không bao giờ bị khoá — `REQ-LOGIN-50`). Cần email có thật → dùng `Project Manager`, bắt đầu bằng 1 lần đăng nhập đúng để bộ đếm = 0"* | — |

### 3.1. Nháp `CRM_LOGIN_TC_015` (A1)

| Cột | Nội dung mới |
|---|---|
| REQ ID | `REQ-LOGIN-41, REQ-LOGIN-49` |
| Test Scenario | Sai mật khẩu 5 lần liên tiếp với cùng một email thì tài khoản bị khoá ngay tại lần thứ 5; dưới ngưỡng hoặc sai rải trên nhiều email thì không khoá |
| Pre-Condition | ⚪ **Skip tới khi đã kiểm chứng trên bản deploy 30-09-2026.** Tài khoản `Project Manager` (`PM_EMAIL`/`PM_PASSWORD` trong `.env`). 🔒 **KHÔNG dùng Admin.** Mỗi biến thể bắt đầu bằng: đăng nhập đúng bằng PM → đăng xuất (bộ đếm = 0). ⚠️ Biến thể `c` khoá PM 15 phút — báo đội trước khi chạy, chạy **sau cùng** |
| Bảng biến thể | `a` Dưới ngưỡng (`REQ-41` AC1) → 4 lần PM + `SaiMatKhau_<ts>`, lần 5 nhập **đúng** mật khẩu<br>`b` Không khoá theo IP (`REQ-49` AC2 — giữ ký tự `b` của biến thể cũ) → 5 email khác nhau `notexist_a_<ts>@auto.test` → `notexist_e_<ts>@auto.test`, mỗi email 1 lần, mật khẩu `Sai@123`; sau đó PM + mật khẩu **đúng**<br>`c` Đủ ngưỡng (`REQ-41` AC2) → 5 lần PM + `SaiMatKhau_<ts>`; lần 6 nhập **đúng** mật khẩu |
| Expected | `a` → lần 1–4 mỗi lần đúng 1 dải `Invalid email or password`; lần 5 đăng nhập **thành công**, dừng ở `https://crm.anhtester.com/admin/`<br>`b` → cả 5 lần `Invalid email or password`; lần đăng nhập PM **thành công**, không thông báo khoá<br>`c` → lần 1–4 `Invalid email or password`; **lần 5** đã hiện nguyên văn `Your account is locked. Please try again in 15 minutes.`; lần 6 (mật khẩu đúng) **vẫn** hiện chuỗi đó, vẫn ở `/admin/authentication`, không tạo phiên |
| Tags | Thêm `@NeedsVerify` (chưa kiểm chứng trên bản deploy — tag có sẵn trong skill, không đặt tag skip mới), `@Security`; bỏ nội dung *"Đã xác nhận hệ thống không có cơ chế khoá nên thao tác này an toàn"* |
| Priority | Critical (tăng từ High — sai là khoá cả đội) |

> Biến thể `c` trùng một phần với `REQ-LOGIN-46`, `47` (thông báo khoá, đúng mật khẩu vẫn bị chặn) — **không** tính là phủ hai REQ này; TC riêng của chúng sinh ở mục 7. Độ hạt GỘP: 3 biến thể, dưới trần 6.

### Nhóm C — Khôi phục TC Deprecated sai lý do

| # | TC | Vòng · Nhánh | Ô sửa | Nội dung sửa | Evidence |
|---|---|---|---|---|---|
| C1 | `TC_006` `:31` | V3 · Permission | `Test Scenario` · `Tags` | Gỡ tiền tố `🗑️ Deprecated (…, 24-09-2026) —` và tag `@Deprecated` → về đúng nguyên văn tại `428e8eb`. Các cột khác đã khớp bản gốc | ⚠️ Kỳ vọng *"9 mục menu"*, class `user-id-3` lấy từ recon trước 24-09-2026, **không** có ảnh PM trong `evidence/`. Không phải V1 nên không chặn sửa, nhưng nên chạy thử 1 lần với PM |
| C2 | `TC_023` `:70` | V3 · Permission | `Pre-Condition` · `Test Steps` bước 1 · Bảng biến thể · `Expected` · ghi chú 🔧 | Khôi phục biến thể `c`, `d` (PM × trang đăng nhập / Quên mật khẩu) và dòng *"`c`,`d` chứng minh hành vi giống nhau giữa hai vai trò"*, `user-id-3` ở `c`,`d` — lấy nguyên văn từ `428e8eb`. Xoá dòng 📌 *"Biến thể c, d đã bỏ 24-09-2026"* | Như C1 |
| C3 | `TC_019` `:55`, `TC_020` `:56` | V3 · Permission · Security | `Test Scenario` (tiền tố) | Giữ `@Deprecated`. Đổi lý do: `🗑️ Deprecated (chưa xác nhận tài khoản Customer — user skip 29-09-2026) —` thay cho *"hệ thống chỉ có role Admin — không có tài khoản Customer, 24-09-2026"* | — |

### Index `TEST_CASES_LOGIN_SUMMARY.md` — sửa kèm

| # | Chỗ | Sửa |
|---|---|---|
| I1 | Tiêu đề + dòng Web ở `## Bản đồ tài liệu` | 51 TC (**48** hiệu lực · **3** `@Deprecated`: `014`, `019`, `020`) |
| I2 | Coverage `REQ-LOGIN-06` `:90` | Thêm `TC_006`; bỏ *"(1 vai trò — hệ thống chỉ có Admin)"* |
| I3 | Coverage `REQ-LOGIN-15` `:99` | Tên REQ thêm *"dưới ngưỡng khoá"* |
| I4 | Coverage `REQ-LOGIN-19`, `20` `:103-104` | `TC_023-a,c` / `TC_023-b,d` |
| I5 | Coverage `REQ-LOGIN-41` `:121` | Tên REQ → *"Khoá tài khoản sau 5 lần sai liên tiếp"* · `TC_015-a,c` |
| I6 | Coverage `REQ-LOGIN-43` `:123` | Giữ 🔴, đổi lý do → *"chưa xác nhận tài khoản Customer (user skip 29-09-2026)"* — **gỡ** chữ *"mâu thuẫn với requirements"* |
| I7 | Coverage — thêm 10 dòng `REQ-LOGIN-45` → `54` | Ghi *"chưa có TC — ngoài phạm vi DELTA"* + command. `REQ-LOGIN-49` ghi `TC_015-b` (chỉ AC2) — AC1 vẫn thiếu. Mẫu số đổi 40 → **50** REQ trong phạm vi |
| I8 | `## Assumptions đã áp dụng` / mục 2 tài khoản | Đổi *"chỉ có role Admin"* → Admin + Project Manager có tài khoản; Customer chưa xác nhận |
| I9 | Bảng 4 vòng — xem mục 4 | |
| I10 | `## Bộ chạy đề xuất` | Thêm cảnh báo: `TC_015` skip tới khi kiểm chứng bản deploy; biến thể `c` khoá PM 15 phút → không vào smoke, chạy cuối |
| I11 | `## Nhật ký thay đổi` | 1 dòng / TC như mẫu workflow, mốc git từng file |

## 4. Nhánh 4 vòng bị chạm

```
Nhánh bị chạm: V2 · Business Rule (TC_015 viết lại) · V2 · State Transition (thêm trạng thái "bị khoá")
               · V2 · Error Guessing (TC_016, TC_047 — chỉ dữ liệu) · V3 · Permission (khôi phục TC_006, TC_023-c,d)
               · V3 · Security (TC_013, TC_015, TC_017) · V4 · Compatibility (TC_050 — chỉ dữ liệu)
Nhánh KHÔNG đụng: toàn bộ V1 — ticket không thêm field, không đổi nhãn/bố cục/giá trị mặc định của biểu mẫu
                  (thông báo khoá là dải báo lỗi mới, thuộc V2) · V2 · UI Behavior · Required · Validation · BVA · V4 còn lại
```

| Nhánh | Trạng thái sau APPLY | Ghi chú |
|---|---|---|
| V2 · Business Rule | ✅ | TC_013, TC_015 (`a`,`c`) |
| V2 · **State Transition** | 🔴 **Thiếu** | Requirements `REQUIREMENTS_LOGIN_SUMMARY.md:118-125` thêm 8 chuyển tiếp của trạng thái *bị khoá*. `TC_015-c` phủ *chưa khoá → khoá*; **hết khoá → mở** (`45`, `51`) và **không gia hạn** (`53`) chưa có TC → lấp bằng lượt sinh TC ở mục 7 |
| V3 · Permission | ✅ | TC_006, TC_021, TC_022, TC_023 (4 TC · khách chưa đăng nhập × Admin × PM). Customer chưa xác nhận |
| V3 · Security | 🟡 nông (giữ) | Thêm TC_013, TC_015 vào danh sách. Chưa có TC cho *né ngưỡng bằng đổi kiểu chữ email* (`REQ-LOGIN-54`) → mục 7 |
| V2 · Error Guessing · V4 · Compatibility | ✅ giữ nguyên | Chỉ đổi dữ liệu, không đổi dải TC |

> Ticket **không** chạm V1: đã đối chiếu 7 thành phần biểu mẫu trong `TC_002` — thông báo khoá hiện ở cùng vị trí dải báo lỗi hiện có, không thêm thành phần nào lên màn hình mặc định.

## 5. Tác động lan toả

| Kiểm | Kết quả |
|---|---|
| TC nào lấy `TC_015` làm precondition? | Không có |
| TC nào khác dùng Admin + sai mật khẩu mà Impact Report bỏ sót? | **Không** — đã grep toàn bộ, `TC_012-e`, `TC_024`, `TC_025`, `TC_040` đều an toàn (mục 2) |
| `TC_015-c` khoá PM 15 phút → ảnh hưởng TC nào dùng PM? | `TC_006`, `TC_013-a`, `TC_023-c,d` sẽ **FAIL giả** nếu chạy trong 15 phút sau `TC_015-c`. → Ghi thứ tự ở Bộ chạy đề xuất: nhóm dùng PM chạy **trước**, `TC_015-c` **cuối cùng** |
| `TC_013-a` dùng PM → cộng bộ đếm PM 1 lần | Precondition "đăng nhập đúng rồi đăng xuất" đưa về 0 — không cộng dồn qua các lần chạy |
| Số TC sau sửa có vượt ngưỡng tách `parts/`? | Không thêm TC → vẫn 51 (đã vượt 50 từ trước, **giữ 1 file theo quyết định user 19-09-2026**). Lượt sinh 10 REQ ở mục 7 sẽ đẩy lên ~61 → **khi đó** phải quyết định tách `parts/` |
| Bug report / execution cũ trỏ vào TC đổi? | `docs/bugs/login/web/` có bug của `TC_029`, `TC_039` — **không** đụng TC nào trong kế hoạch này |

## 6. Việc phải làm trước APPLY

| Việc | Vì sao |
|---|---|
| ⚠️ **Commit** `docs/testcases/login/web/test_cases_login_web.md` và `TEST_CASES_LOGIN_SUMMARY.md` | Cả hai đang có thay đổi **chưa commit** (`git status`: `M`). Workflow Bước 5 yêu cầu mốc git sạch — agent **không** tự commit. Không commit thì APPLY sẽ dừng ở bước đầu |
| Xác nhận `.env` có `PM_EMAIL`, `PM_PASSWORD` | `TC_006`, `013`, `015`, `023` phụ thuộc |

## 7. Ngoài phạm vi

| REQ | Việc còn lại | Command |
|---|---|---|
| `REQ-LOGIN-45` → `48`, `50` (biến thể `a`), `51` → `54` | Chưa có TC — 9 REQ, đặc tả TC đã có sẵn ở mục B của Impact Report | `/generate-testcases-manual-rbt` (hoặc `/generate-testcases-from-requirements`) — chạy **sau** APPLY, không gấp: tất cả skip tới khi kiểm chứng bản deploy |
| `REQ-LOGIN-49` AC1 | PM bị khoá → Admin đăng nhập đúng **1 lần** | Như trên. AC2 đã có ở `TC_015-b` |
| `REQ-LOGIN-50` biến thể `b` | Cần tài khoản `Customer` | ⏭️ Chờ user xác nhận tài khoản |
| Kiểm chứng trên bản deploy | 11 REQ khoá tài khoản ⚪ → 🟢 | Sau 30-09-2026: `/update-requirements-from-ticket` → nếu lệch thì `/update-testcases-from-impact` đợt mới |
| RTM | Khớp lại | `/generate-traceability-matrix` |

## 8. Cần duyệt

1. **Nguồn:** lập kế hoạch theo **`impact_CRM-LOGIN-101-B.md`** thay vì file user chỉ định (`impact_CRM-LOGIN-101.md` đã bị thay thế). Đồng ý?
2. **`TC_015` bảng biến thể:** giữ `b` = không khoá theo IP (neo thêm `REQ-LOGIN-49`), thêm `c` = khoá đủ ngưỡng, `a` đổi nghĩa thành dưới ngưỡng. Phương án khác: bỏ `b` khỏi TC_015, để TC mới của `REQ-49` nhận — nhưng như vậy là **xoá một biến thể** và REQ-49 AC2 mất TC cho tới khi sinh TC mới. Khuyến nghị giữ `b`.
3. **`TC_011-c`:** theo Impact Report **không sửa** (mật khẩu trống không tính — `REQ-LOGIN-52`). Nhưng `REQ-52` cũng ⚪ chưa kiểm chứng; nếu bản deploy lỡ tính thì mỗi lần chạy cộng 1 lần sai cho Admin. Đổi sang `notexist_<ts>@auto.test` không làm mất gì (TC không cần email thật). Khuyến nghị: **đổi luôn** cho an toàn — cần user đồng ý vì vượt phạm vi Impact Report.
4. **Khôi phục `TC_006`, `TC_023-c,d`** từ bản `428e8eb`, chấp nhận kỳ vọng "9 mục menu" chưa có ảnh evidence.
5. **Nhánh bị chạm:** V2 Business Rule · State Transition · Error Guessing · V3 Permission · Security · V4 Compatibility. **Không** chạm V1.
