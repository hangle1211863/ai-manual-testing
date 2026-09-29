# Báo Cáo Review Test Cases — `LOGIN` · Web

> Workflow: `/review-testcases` **Mode REVIEW** — chỉ báo cáo, **không** sửa file TC nào.
> Ngày review: 24-09-2026 · Rubric: `skills-testcase-reviewer` (6 tiêu chí × 0–2 điểm).

## Tổng quan

- **Nguồn:** [web/test_cases_login_web.md](../web/test_cases_login_web.md) (theo `## Bản đồ tài liệu` của [TEST_CASES_LOGIN_SUMMARY.md](../TEST_CASES_LOGIN_SUMMARY.md))
- **Requirements đối chiếu:** [REQUIREMENTS_LOGIN_SUMMARY.md](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) · [requirements_login_web.md](../../../requirements/login/web/requirements_login_web.md)
- **Số TC review:** 51 (`CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_051`) · `@Deprecated`: 0
- **Kết quả:** 🟢 51 tốt | 🟡 0 cần sửa | 🔴 0 nên viết lại
- **Điểm trung bình:** 11,5/12 (34 TC 12 điểm · 11 TC 11 điểm · 6 TC 10 điểm)
- **Loại trừ theo requirements:** `REQ-LOGIN-27` (`AMB-LOGIN-04`) · `REQ-LOGIN-34` (`AMB-LOGIN-06`) · `REQ-LOGIN-35` (`AMB-LOGIN-07`) · `REQ-LOGIN-40` (`AMB-LOGIN-15`) · kiểm chứng đầu-cuối popup timer (`AMB-LOGIN-14` → `TASK`) · biên trường Password (`AMB-LOGIN-09`) · đăng nhập cổng khách hàng · quản lý staff/role · đổi ngôn ngữ (`PROF`) · `RISK-LOGIN-04`, `RISK-LOGIN-05` đã chấp nhận

> ⚠️ **Rubric xanh toàn bộ không có nghĩa bộ TC chạy được ngay.** Hai vấn đề cấp bộ ở mục *Vấn đề cấp bộ TC* (tài khoản `.env` không khớp, tài liệu truy vết lệch nhau) chặn việc thực thi nhiều hơn mọi điểm trừ ở từng TC.

---

## Vấn đề cấp bộ TC (ưu tiên xử lý trước)

| # | Vấn đề | Căn cứ | Ảnh hưởng | Đề xuất |
|---|---|---|---|---|
| S1 | 🔴 **Tên biến `.env` trong TC không khớp `.env` thật** | TC dùng `ADMIN_PASSWORD`, `PM_EMAIL`, `PM_PASSWORD`, `CUSTOMER_EMAIL`, `CUSTOMER_PASSWORD`, `TC014_EMAIL`, `TC014_PASSWORD`. `.env` hiện chỉ có 3 khoá: `BASE_URL`, `EMAIL_ADMIN`, `PASSWORD_ADMIN` | **Mọi** TC đăng nhập thành công tra sai tên khoá. `TC_006`, `TC_014`, `TC_019`, `TC_020`, `TC_023-c/d` **không chạy được** — thiếu hẳn tài khoản PM, Customer, staff test | Chọn một: (a) bổ sung khoá vào `.env` theo đúng tên TC đang dùng (không đụng TC) — **khuyến nghị**; hoặc (b) đổi `ADMIN_PASSWORD` → `PASSWORD_ADMIN` trong TC qua Mode FIX. Tài khoản PM/Customer/staff test phải xin cấp lại — nếu không có, 5 TC trên chấm `BLOCKED` khi thực thi |
| S2 | 🔴 **Nhật ký requirements mâu thuẫn với bộ TC đang có** | Dòng 24-09-2026 (chưa commit) trong `REQUIREMENTS_LOGIN_SUMMARY.md` ghi *"Bộ TC module được sinh lại toàn bộ (48 TC, `001`→`048`)"* và *"TC mới là `CRM_LOGIN_TC_045`"* cho `REQ-LOGIN-44`. File TC trên đĩa có **51 TC**, `REQ-LOGIN-44` ↔ **`TC_039`**; `TC_045` là TC biên email 64 ký tự | Người đọc requirements sẽ mở bug HTTPS gắn nhầm `TC_045` — đúng loại lỗi truy vết mà quy tắc bất biến cấm | Xác định bộ nào có hiệu lực, rồi sửa dòng Nhật ký đó cho khớp (xem thêm S3) |
| S3 | 🟡 **Thư mục làm việc ở trạng thái lẫn lộn** | `git status`: `web/parts/part_01_…md` (TC 001→023) và `part_02_…md` (TC 024→054) — một bộ **54 TC** khác — đang bị xoá chưa commit; index không trỏ tới chúng | Ba con số cùng tồn tại: 48 (Nhật ký REQ) · 51 (file hiện tại) · 54 (HEAD) | User xác nhận bộ nào chính thức. Báo cáo này chấm **bộ 51 TC** mà index đang trỏ tới |
| S4 | 🟡 **Liên kết gãy tới `executions/` và `bugs/`** | Thư mục `docs/executions/login/` và `docs/bugs/login/` **không tồn tại**. Bị trỏ tới ở: `TC_039` (`BUG_login_1785678750_TC039`), `TC_046-c` (`BUG_login_1787226515_TC018`), index — Nhật ký (`run_1787215085`, `run_1789759574`), mục *Vùng chưa có evidence*, Assumptions `ASM-02/03/04/08` | Căn cứ gỡ `@NeedsVerify` và trạng thái bug không còn tra được | Nếu bug/run đã xoá có chủ đích: đổi link thành mô tả *"đã xoá khỏi repo, tra `git show <mốc>`"*. `TC_039` cần mở bug mới bằng `/create-bug-report` |
| S5 | 🟡 **Index có số liệu đã cũ** | (1) Bản đồ tài liệu ghi *"39/39 REQ trong phạm vi"*, Bảng Đối Soát ghi **40/40**. (2) *Bộ chạy đề xuất* còn dòng `@NeedsVerify` cho `TC_024` 🔧`2` — nhưng Nhật ký 19-09-2026 đã gỡ, và TC không mang tag này. (3) Tổng **69 biến thể** — đếm lại được **83** (17 TC có Bảng biến thể = 49 biến thể + 34 TC đơn). (4) Bảng ISO ghi *"TC_001 → TC_048"* | Số liệu tổng không tin được | Sửa 4 chỗ trên khi chạy Mode FIX. Nếu quy ước đếm biến thể khác (VD không đếm TC đơn), ghi quy ước đó cạnh con số |
| S6 | 🟡 **TC có dòng 🔧 nhưng thiếu tag `@TechCheck`** | `TC_042` và `TC_044` có dòng `🔧 Ghi chú kỹ thuật (cần DevTools)` nhưng Tags không có `@TechCheck`. Index quy ước *"TC có dòng 🔧 gắn tag `@TechCheck`"* | Bộ chạy `@TechCheck` (25 TC) bỏ sót 2 TC | Thêm `@TechCheck` vào 2 TC; bộ chạy thành 27 |
| S7 | 🟢 **`TC_034` lệch với quyết định phạm vi của requirements** | Requirements (mục *Ngoài phạm vi* + `AMB-LOGIN-14` ⏭️) vẫn ghi *"kiểm chứng đầu-cuối popup timer chuyển sang `TASK`"*, `REQ-LOGIN-30` bằng chứng *"mức đọc mã nguồn"*. `TC_034` (sửa 19-09-2026) đã chạy thật đầu-cuối | Điều kiện rà lại của `AMB-LOGIN-14` (kiểm chứng đầu-cuối rồi cập nhật ngược REQ) **đã xảy ra**, nhưng requirements chưa cập nhật | **Giữ `TC_034`** — đây là cách duy nhất kiểm `REQ-LOGIN-30` (vẫn trong phạm vi) bằng giao diện. Cập nhật ngược requirements bằng `/update-requirements-from-ticket` |
| S8 | 🟢 Viewport chuẩn lệch với `CLAUDE.md` | TC/index ghi `1600×750`; `CLAUDE.md` chốt headed `1600×770` | Nhỏ — không đổi kết quả TC nào | Đồng bộ một lần khi sửa index |

---

## Chi tiết từng TC

> Cột điểm: tổng /12. Chỉ TC dưới 12 điểm mới có vấn đề + đề xuất. Mọi nhận xét về Expected đã đối chiếu Pre-Condition + Test Data của chính TC đó.

| TC ID | Điểm | Xếp loại | Vấn đề chính | Đề xuất sửa |
|---|---|---|---|---|
| TC_001 | 12/12 | 🟢 | — | — |
| TC_002 | 11/12 | 🟢 | Tiêu chí 6: Bảng kiểm Kiểu B là **quan sát tĩnh**, nhưng mục `5` (gõ mật khẩu) và `6` (bấm chữ `Remember me`) là **thao tác**. Mục `5` trùng `TC_042` bước 1–2 | Bỏ mục `5` (đã có ở `TC_042`), ghi vào Expected: *"Mục 5 cũ (che ký tự mật khẩu) chuyển sang `CRM_LOGIN_TC_042` — 24-09-2026"*. Mục `6` giữ, hoặc chuyển sang `TC_042` cùng nhóm UI Behavior |
| TC_003 | 11/12 | 🟢 | Tiêu chí 2: mục `3` *"không có hiệu ứng chờ tải kịch bản"* không chứng minh được `REQ-LOGIN-37` bằng mắt — trang có script vẫn có thể không có hiệu ứng chờ | Viết thật: *"`3` (REQ-37) Phần chính không kiểm được — xem 🔧"*, hoặc bỏ mục `3` khỏi Bảng kiểm, để `REQ-37` chỉ ở dòng 🔧 |
| TC_004 | 12/12 | 🟢 | — | — |
| TC_005 | 12/12 | 🟢 | — | — |
| TC_006 | 10/12 | 🟢 | Tiêu chí 4: `PM_EMAIL` / `PM_PASSWORD` **không có** trong `.env` (S1). Tiêu chí 2: *"đếm được đúng 9 mục"* nhưng không nêu tên 9 mục — tester đếm có tính menu con không? Dẫn *"Admin có 14 — đối chiếu `TC_005`"* nhưng Expected của `TC_005` không nêu con số 14 | Expected bước 5 liệt kê nguyên văn 9 mục theo thứ tự trên xuống (lấy từ recon). Bỏ vế *"đối chiếu với TC_005"* hoặc thêm *"menu trái đếm được 14 mục"* vào `TC_005`. Bổ sung khoá PM vào `.env` |
| TC_007 | 12/12 | 🟢 | — | — |
| TC_008 | 12/12 | 🟢 | Phần chính chỉ tới *"đăng nhập thành công"* — đã giải thích ở `ASM-09`, không trừ | — |
| TC_009 | 12/12 | 🟢 | — | — |
| TC_010 | 12/12 | 🟢 | — | — |
| TC_011 | 12/12 | 🟢 | — | — |
| TC_012 | 12/12 | 🟢 | — | — |
| TC_013 | 12/12 | 🟢 | — | — |
| TC_014 | 10/12 | 🟢 | Tiêu chí 3 + 4: Pre-Condition *"Đã có tài khoản staff test… khai trong `.env` bằng `TC014_EMAIL`"* — khoá không tồn tại, và Pre-Condition không nói **ai tạo / tạo thế nào**; môi trường chỉ có tài khoản Admin demo | Pre-Condition thêm: *"Tài khoản do <người/đội> cấp; chưa có thì chấm `BLOCKED`, không tự tạo staff trên môi trường dùng chung"*. Xin cấp tài khoản rồi thêm khoá vào `.env` |
| TC_015 | 12/12 | 🟢 | — | — |
| TC_016 | 12/12 | 🟢 | — | — |
| TC_017 | 12/12 | 🟢 | — | — |
| TC_018 | 12/12 | 🟢 | — | — |
| TC_019 | 10/12 | 🟢 | Tiêu chí 1: Pre-Condition *"Ba bước phải chạy nối tiếp"* nhưng TC có **4** bước; bước 2 *"bấm đăng nhập"* ở `/login` không nêu tên nút. Tiêu chí 4: `CUSTOMER_*` không có trong `.env` | *"Bốn bước phải chạy nối tiếp"*; bước 2 ghi đúng tên nút trên trang `/login` (cần nhìn lại màn hình — chưa có evidence của trang này). Bổ sung khoá Customer |
| TC_020 | 10/12 | 🟢 | Tiêu chí 2: Expected `1` *"Phiên cổng khách hàng đang hoạt động"* — không nói nhìn vào đâu để biết. Tiêu chí 4: `CUSTOMER_*` không có trong `.env` | Expected `1`: *"Tab trình duyệt ghi `HKD Anh Tester`, góc trên hiện <tên/nút đăng xuất của khách hàng>"* (lấy dấu hiệu đã dùng ở `TC_019` bước 2) |
| TC_021 | 12/12 | 🟢 | — | — |
| TC_022 | 12/12 | 🟢 | — | — |
| TC_023 | 10/12 | 🟢 | Tiêu chí 3: Pre-Condition *"Đã đăng nhập bằng vai trò của biến thể"* lại lặp ở bước 1; không nói **đăng xuất giữa biến thể khác vai trò** (`b` → `c` đổi Admin sang PM) — mà khi đang có phiên thì không mở được trang đăng nhập (chính `REQ-19`). Tiêu chí 4: `PM_*` thiếu | Pre-Condition: *"Chưa đăng nhập. Giữa `b` và `c` mở `/admin/authentication/logout` để đổi vai trò"*. Bước 1 giữ nguyên |
| TC_024 | 11/12 | 🟢 | Tiêu chí 6: phần chính trùng `TC_005` + tích Remember me; bước 2 (F5) không có kỳ vọng nhìn thấy nào ở phần chính. Giá trị của TC nằm hết ở 🔧 với 3 ý khác nhau | Chấp nhận được vì `@TechCheck`. Nếu muốn gọn: bỏ bước tích `Remember me` (không phục vụ kiểm CSRF — tham số `remember` ở 🔧`3` có thể kiểm trên `TC_008`) |
| TC_025 | 11/12 | 🟢 | Tiêu chí 2: bước 1 ở **phần chính** dùng ngôn ngữ DOM *"sửa `value` của `input[name=csrf_token_name]`"* — không nằm dưới 🔧 | Đưa thao tác DevTools xuống 🔧 và ghi rõ ở phần chính *"Bước 1 làm theo 🔧"*; hoặc — nếu đo được — dùng cách không cần DevTools: mở trang đăng nhập, xoá dữ liệu trang của `crm.anhtester.com` qua biểu tượng ổ khoá trên thanh địa chỉ, **không nạp lại**, rồi bấm `Login`. ⚠️ Cách này **chưa được đo** — phải recon trước khi dùng |
| TC_026 | 12/12 | 🟢 | Pre-Condition và bước 1 cùng nói "đã đăng nhập" — không đổi kết quả, không trừ | Cân nhắc (chưa kiểm chứng): nếu Dashboard có yêu cầu nền định kỳ, để yên tab `/admin/` có thể tự gia hạn phiên → `TC_026` FAIL giả. Nên đo một lần xem tab Dashboard có gửi yêu cầu nền không trước khi chạy 65 phút |
| TC_051 | 12/12 | 🟢 | — | — |
| TC_027 | 12/12 | 🟢 | — | — |
| TC_028 | 12/12 | 🟢 | — | — |
| TC_029 | 12/12 | 🟢 | — | — |
| TC_030 | 11/12 | 🟢 | Tiêu chí 5: truy vết `REQ-LOGIN-26` (*email không tồn tại*) — trong khi TC kiểm chặn định dạng tại trình duyệt, căn cứ là `REQ-LOGIN-23` (`input#email[type=email]`) | Cột REQ: `REQ-LOGIN-23`. Mở rộng thành Bảng biến thể — xem Gap #3 |
| TC_031 | 12/12 | 🟢 | `Auto Type` = `API` — ngôn ngữ HTTP ở phần chính hợp lệ | — |
| TC_032 | 11/12 | 🟢 | Tiêu chí 3: Expected `4` *"bắt đầu chuyển hướng đăng xuất"* phụ thuộc điều kiện **không có bộ đếm giờ** — Pre-Condition và Test Data không nêu (có timer thì `REQ-30` bật lớp xác nhận). Kết quả cuối không được chấm | Pre-Condition thêm *"không có bộ đếm giờ công việc nào đang chạy"* (như `TC_035`). Expected `4`: *"dừng ở `https://crm.anhtester.com/admin/authentication`"* |
| TC_033 | 11/12 | 🟢 | Tiêu chí 3: cùng lỗi với `TC_032` — không bảo đảm không có timer | Pre-Condition thêm *"không có bộ đếm giờ công việc nào đang chạy"* |
| TC_034 | 12/12 | 🟢 | Xem S7 — lệch với requirements, **không** phải lỗi của TC | Cập nhật ngược requirements |
| TC_035 | 12/12 | 🟢 | — | — |
| TC_036 | 12/12 | 🟢 | — | — |
| TC_037 | 12/12 | 🟢 | — | — |
| TC_038 | 12/12 | 🟢 | — | — |
| TC_039 | 12/12 | 🟢 | Link bug gãy — xem S4 | — |
| TC_040 | 11/12 | 🟢 | Tiêu chí 5: truy vết `REQ-LOGIN-14` (*thông báo sai thông tin*) — TC không kiểm thông báo đó; thứ được kiểm là *không tạo phiên* | Cột REQ: `REQ-LOGIN-06` (đăng nhập hợp lệ mới tạo phiên), giữ loại `Error Guessing` |
| TC_041 | 11/12 | 🟢 | Tiêu chí 2: bước 1 phần chính *"Mở DevTools → tab Network, đặt `Slow 3G`"* — thao tác DevTools ở phần chính. Expected `4` *"trang không đứng hẳn"* khó chấm | Chuyển bước 1 xuống 🔧 như `TC_025`. Expected `4`: *"trong vòng <N> giây trang chuyển sang Dashboard"* — hoặc bỏ vế *"không đứng hẳn"*, đã có bước 5 chấm điểm dừng |
| TC_042 | 11/12 | 🟢 | Tiêu chí 6: gộp 3 hành vi khác nhau của ô Password (che ký tự · không có nút hiện/ẩn · cho dán) — không đúng Kiểu A (khác thao tác) cũng không đúng Kiểu B (có thao tác). Thiếu tag `@TechCheck` (S6) | Chấp nhận giữ 1 TC nếu chuyển về dạng **Bảng kiểm** đánh mã `1`/`2`/`3` như `TC_002` để báo FAIL từng mục. Thêm `@TechCheck` |
| TC_043 | 12/12 | 🟢 | — | — |
| TC_044 | 10/12 | 🟢 | Tiêu chí 2: Expected `3` *"hộp hỏi `Lưu mật khẩu?`"* chỉ ghi tiếng Việt, trong khi bước 4 đã tính cả `Lưu` / `Save` — Chrome tiếng Anh ghi chữ khác. Tiêu chí 5: `REQ-LOGIN-02` (thành phần biểu mẫu) không nói gì về tự điền. Thiếu `@TechCheck` (S6) | Expected `3`: *"Trình duyệt hiện hộp đề nghị lưu mật khẩu (Chrome tiếng Việt `Lưu mật khẩu?` · tiếng Anh `Save password?`)"*. Ghi rõ TC là `Error Guessing` không có REQ riêng, hoặc đề xuất REQ mới qua `/update-requirements-from-ticket` |
| TC_045 | 12/12 | 🟢 | — | — |
| TC_046 | 12/12 | 🟢 | Link bug gãy — xem S4 | — |
| TC_047 | 11/12 | 🟢 | Tiêu chí 6 / ghi nhãn: đặt trong Nhóm I *"Giá trị biên"*, tag `@Boundary`, tính vào nhánh `BVA` — trong khi đầu Nhóm C ghi *"🚫 KHÔNG viết TC biên cho trường Password"* (`AMB-LOGIN-09`). Thực chất TC không có ngưỡng nào — đây là kiểm **độ bền** với chuỗi dài bất thường | **Không** deprecate — nội dung đúng. Gỡ `@Boundary`, xếp vào nhánh `Error Guessing` / `Security`, sửa đếm nhánh BVA ở index (còn `TC_045`, `TC_046`, `TC_048`) |
| TC_048 | 12/12 | 🟢 | — | — |
| TC_049 | 12/12 | 🟢 | — | — |
| TC_050 | 12/12 | 🟢 | — | — |

---

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | `TC_002`, `TC_003` (đăng nhập) · `TC_027` (Quên mật khẩu) — nhãn nguyên văn, thứ tự, trạng thái mặc định |
| 1 | Open form | ✅ | `TC_001`, `TC_027` |
| 1 | Display | ✅ | `TC_002`-`2`, `TC_005`, `TC_006` |
| 1 | Input valid data | ✅ | `TC_005`, `TC_006` (⚠️ `TC_006` thiếu tài khoản — S1) |
| 1 | Save | ✅ | `TC_005` — tạo phiên |
| 1 | Verify data | ✅ | `TC_005`, `TC_006`, `TC_008`, `TC_009` |
| 2 | UI Behavior | ✅ | `TC_042`, `TC_043`, `TC_044` |
| 2 | Required | ✅ | `TC_011` (4 biến thể) · `TC_028` |
| 2 | Validation | 🟡 Nông | Email đăng nhập thiếu 2/8 mục áp dụng (*domain không hợp lệ* · *ký tự đặc biệt trước `@`*) — index ghi *"đủ 8/8"* là chưa đúng. Email ở Quên mật khẩu chỉ có 1 ca (`abc`). Password: đủ phần áp dụng (`⏭️ AMB-LOGIN-09` cho min/max/độ mạnh) |
| 2 | Equivalence Partitioning | ✅ | `TC_012`, `TC_013`, `TC_045`, `TC_046` |
| 2 | Boundary Value Analysis | ✅ | `TC_045`, `TC_046`, `TC_048`. `TC_047` gắn nhãn sai nhánh (xem chi tiết TC) |
| 2 | Business Rule | ✅ | `TC_013`, `TC_015`, `TC_019` |
| 2 | Decision Table | ➖ | Chấp nhận — tổ hợp *email trống / password trống / thông tin đúng* thực tế đã phủ đủ bởi `TC_011`-`a/b/c` + `TC_013` + `TC_005` |
| 2 | State Transition | ✅ (ghi nhãn sai) | Index ghi ➖ *"chỉ có 2 trạng thái"* — **sai** so với requirements mục 7: có ≥ 4 trạng thái phiên (chưa đăng nhập · đã đăng nhập · đã đăng nhập + lớp xác nhận timer · đã đăng xuất còn cookie). Các chuyển tiếp **đã có TC**: `TC_021`, `TC_023`, `TC_026`, `TC_034`, `TC_035`, `TC_036`, `TC_037`, `TC_051` → đổi nhãn sang ✅ và liệt kê |
| 2 | Dependency | ✅ | `TC_034`, `TC_037` |
| 2 | Use Case / Scenario | ✅ | Chuỗi `TC_005` → `TC_036` |
| 2 | Save / Edit / Delete | ➖ | Không có bản ghi nghiệp vụ |
| 2 | Error Guessing | ✅ | `TC_010`, `TC_016`, `TC_018`, `TC_036`, `TC_040` (+ `TC_047` sau khi đổi nhãn) |
| 3 | Permission | ✅ | `TC_006`, `TC_019`, `TC_020`, `TC_021`, `TC_022`, `TC_023` — ⚠️ 4/6 TC không chạy được vì thiếu tài khoản PM/Customer (S1) |
| 3 | Security | 🟡 Nông | CSRF chỉ có ca phủ định cho **biểu mẫu đăng nhập** (`TC_025`). Biểu mẫu Quên mật khẩu có mã CSRF (`REQ-LOGIN-23`, `TC_027` 🔧) nhưng không TC nào kiểm mã sai bị chặn — Gap #4 |
| 3 | API | ➖ → nên là ⏭️ | Lỗi ghi nhãn: *"QA không có quyền — đội Dev xác minh"* là **có nhưng không kiểm** → `⏭️` (đã có người quyết: PO 11-09-2026; thiếu điều kiện rà lại) |
| 3 | Database | ➖ → nên là ⏭️ | Như trên |
| 3 | Integration | ➖ | Hợp lệ — `TC_003` đã chứng minh không có đăng nhập bên thứ ba |
| 3 | Logging / Audit | ➖ → nên là ⏭️ | Lỗi ghi nhãn: Activity Log **có tồn tại**, chỉ thiếu quyền → `⏭️`, điều kiện rà lại *"khi có tài khoản Super Admin"* đã ghi sẵn — chỉ cần đổi ký hiệu |
| 4 | Compatibility | ✅ | `TC_050` (Chrome, Edge, Firefox) |
| 4 | Responsive | ✅ | `TC_049`, `TC_033` |
| 4 | Accessibility | ✅ | `TC_038`, `TC_002` |
| 4 | Performance | ✅ mức thô | `TC_041` |
| 4 | Regression | ➖ → nên là ⏭️ | Điều kiện *"rà lại khi bug đầu tiên được fix"* là của `⏭️`. Nguồn dẫn `docs/bugs/login/` không còn tồn tại (S4) |
| 4 | E2E | ➖ | Hợp lệ — thuộc `/generate-cross-module-test-plan` |

---

## Đối soát bảng 15 loại field

| Field | Loại | Mục đã có TC | Mục thiếu |
|---|---|---|---|
| Email (đăng nhập) | Email | Format hợp lệ `TC_005` · Thiếu `@` `TC_012-a` · Thiếu domain `TC_012-b` · Nhiều `@` `TC_012-d` · Max length `TC_045`/`046`/`048` · Case sensitivity `TC_007` · (Required `TC_011`) | **Domain không hợp lệ** (VD `admin@example` không có đuôi, `admin@.com`, `admin@example..com`) · **Ký tự đặc biệt trước `@`** (hợp lệ: `first.last+qa@example.com`; không hợp lệ: `adm(in)@example.com`). *Email đã tồn tại* ➖ — màn không tạo tài khoản |
| Password (đăng nhập) | Password | Copy-paste `TC_042` · Hiện/ẩn `TC_042` · (Required `TC_011` · phân biệt hoa/thường `TC_014`) | — (Min/Max · ký tự đặc biệt · chữ hoa · chữ số: `⏭️ AMB-LOGIN-09` · Confirm password ➖ không có ô) |
| Remember me | Checkbox | Mặc định `TC_002-3` · Check `TC_002-6`, `TC_038-6` | **Uncheck** — tích rồi bấm lại cho về rỗng (nhỏ, có thể thêm mục `7` vào Bảng kiểm `TC_002`). Required ➖ |
| Email (Quên mật khẩu) | Email | Thiếu `@` `TC_030` · (Required `TC_028` · không tồn tại `TC_029`) | **Thiếu domain** · **Domain không hợp lệ** · **Nhiều `@`** · **Ký tự đặc biệt** · **Max length** (có áp mốc 64 như trang đăng nhập không?). Case sensitivity + format hợp lệ với email có thật: `⏭️ AMB-LOGIN-04` |

---

## Coverage Gaps (TC còn thiếu)

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất | Cách bổ sung |
|---|---|---|---|---|
| 1 | Email đăng nhập có **domain không hợp lệ** mà trình duyệt vẫn cho gửi (VD `admin@example`) — máy chủ trả gì? | V2 · Validation | Medium | Phản hồi **chưa biết** → recon trước, rồi thêm TC mới `CRM_LOGIN_TC_052` (không đoán Expected) |
| 2 | Email đăng nhập có **ký tự đặc biệt hợp lệ** trước `@` (`first.last+qa@example.com`) được coi là đúng định dạng → `Invalid email or password` | V2 · Validation | Low | Nếu recon xác nhận cùng loại phản hồi với `TC_045` → thêm biến thể `c` vào `TC_045` (Kiểu A) |
| 3 | Email Quên mật khẩu sai định dạng: `abc@` · `@example.com` · `a@b@example.com` | V2 · Validation | Medium | Cùng loại phản hồi (trình duyệt chặn) → đổi `TC_030` thành Bảng biến thể `a`–`d`. Ca vượt 64 ký tự trước `@` phản hồi chưa biết → recon riêng |
| 4 | Biểu mẫu Quên mật khẩu gửi với mã CSRF sai/rỗng bị chặn | V3 · Security | Medium | Recon trước (phản hồi có giống `419 Page Expired!` không), rồi TC mới nối tiếp dải |
| 5 | Nhấn `Enter` ngay trong ô Password để gửi biểu mẫu (không Tab tới nút) | V2 · Error Guessing | Low | Thêm biến thể vào `TC_038` hoặc TC mới — thao tác phổ biến nhất của người dùng thật |

---

## TC trùng lặp — đề xuất merge

- `TC_002`-`5` ≈ `TC_042` bước 1–2 (cùng kiểm ô Password hiện dấu chấm che) → giữ ở `TC_042`; bỏ mục `5` khỏi Bảng kiểm `TC_002` và ghi lý do ngay trong Expected. **Không** cần `@Deprecated` — chỉ bỏ một mục, TC vẫn giữ.
- Không phát hiện cặp TC nào trùng trọn vẹn. Các cặp gần nhau (`TC_035` ↔ `TC_036-a`, `TC_021-a` ↔ `TC_022`, `TC_008` ↔ `TC_037`) khác đường vào hoặc khác điều cần chứng minh — giữ nguyên.

## Priority

Hợp lý với rủi ro. Không đề xuất đổi.

## Đối chiếu kết quả chạy

`docs/executions/login/` **không tồn tại** — không đối chiếu được. Ghi chú đã cũ phát hiện từ chính index: dòng `@NeedsVerify` của `TC_024` ở *Bộ chạy đề xuất* (S5). Không TC nào còn mang tag `@NeedsVerify` hay `⚠️ chưa có evidence`.

---

## Kết luận & Khuyến nghị

1. **Chặn thực thi — làm trước:** khớp tên khoá `.env` với TC và xin tài khoản PM / Customer / staff test (S1). Không có bước này, 5 TC phân quyền + `TC_014` chấm `BLOCKED`.
2. **Chốt bộ TC có hiệu lực** (48 · 51 · 54?) rồi sửa dòng Nhật ký 24-09-2026 trong requirements cho khớp (S2, S3); mở lại bug HTTPS cho `TC_039` bằng `/create-bug-report` (S4).
3. **Chạy `/review-testcases` Mode FIX** cho 17 TC dưới 12 điểm — chủ yếu sửa Pre-Condition (`TC_023`, `TC_032`, `TC_033`), cột REQ (`TC_030`, `TC_040`), đưa thao tác DevTools xuống 🔧 (`TC_025`, `TC_041`), đổi nhãn `TC_047` — kèm đồng bộ index (S5, S6, nhãn 4 vòng).
4. **Recon bổ sung** trước khi viết 3 TC mới: Gap #1, #3 (phần vượt 64 ký tự), #4 — phản hồi của hệ thống chưa biết, không được đoán Expected.
5. **Cập nhật ngược requirements** cho `REQ-LOGIN-30` / `AMB-LOGIN-14` (S7) bằng `/update-requirements-from-ticket`.

---

## Kết quả Mode FIX — 24-09-2026

**Quyết định của user:** (1) hệ thống chỉ có role Admin, đăng nhập bằng tài khoản Admin trong `.env` · (2) dùng file TC hiện tại (51 TC), bỏ qua Nhật ký requirements · (3) coi `docs/executions/` và `docs/bugs/` như chưa tồn tại.

**Mốc git trước khi sửa:** `e637fbb` (cả file TC lẫn index) — xem bản cũ: `git show e637fbb:docs/testcases/login/web/test_cases_login_web.md`

| Mục | Kết quả |
|---|---|
| S1 — khoá `.env` | ✅ `ADMIN_PASSWORD` → `PASSWORD_ADMIN`. Email Admin trong `.env` trùng `admin@example.com` → giữ nguyên |
| S1 — tài khoản PM / Customer / staff test | 🗑️ `TC_006`, `TC_014`, `TC_019`, `TC_020` → `@Deprecated`; `TC_023` bỏ biến thể `c`, `d` |
| S2, S3 — Nhật ký requirements, bộ 48/54 TC | ⏭️ Không xử lý theo quyết định user |
| S4 — liên kết gãy | ✅ Gỡ ở `TC_039`, `TC_046`, Assumptions, mục *Vùng chưa có evidence*, nhánh Regression. Nhật ký cũ giữ nguyên |
| S5 — số liệu index | ✅ Coverage 39/40 · 80 biến thể · bỏ dòng `@NeedsVerify` · ISO |
| S6 — `@TechCheck` | ✅ `TC_042`, `TC_044` |
| S7 — `REQ-LOGIN-30` | ⏭️ Chưa xử lý — việc của requirements |
| S8 — viewport | ⏭️ Chưa xử lý |
| Chi tiết từng TC | ✅ `TC_002`, `003`, `023`, `025`, `030`, `032`, `033`, `040`, `041`, `042`, `044`, `047`. `TC_024` giữ nguyên (đã chấp nhận ở bảng chi tiết). `TC_006`, `014`, `019`, `020` chuyển `@Deprecated` thay vì sửa |
| Nhãn 4 vòng | ✅ State Transition ✅ · Validation, Security 🟡 · API, Database, Logging, Regression ⏭️ |
| Gap #3 | ✅ `TC_030` mở rộng 4 biến thể (phần trình duyệt chặn). Ca vượt 64 ký tự chưa làm |
| Gap #1, #2, #4, #5 | ⏸️ Chưa viết — cần recon trên hệ thống thật trước, không đoán Expected |

**Phát sinh mới:** `REQ-LOGIN-43` (Customer không vào được `/admin`) **không còn TC nào** vì user xác nhận không có role Customer — trong khi requirements ghi hệ thống có 3 vai trò đã kiểm chứng. Cần chốt lại bằng `/update-requirements-from-ticket`: hoặc REQ-43 ra ngoài phạm vi / 🔴 Deprecated, hoặc cấp lại tài khoản Customer rồi gỡ `@Deprecated` cho `TC_019`, `TC_020`.
