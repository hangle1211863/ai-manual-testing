# Báo Cáo Review Test Cases — `LOGIN` · Web

## Tổng quan

| Mục | Giá trị |
|---|---|
| **Nguồn** | [web/test_cases_login_web.md](../web/test_cases_login_web.md) — 51 TC · index [TEST_CASES_LOGIN_SUMMARY.md](../TEST_CASES_LOGIN_SUMMARY.md) |
| **Ngoài phạm vi chấm** | [web/parts/](../web/parts/) — 54 TC, **không** có trong `## Bản đồ tài liệu` của index → xem **Phát hiện P1** |
| **Requirements đối chiếu** | [REQUIREMENTS_LOGIN_SUMMARY.md](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) · [requirements_login_web.md](../../../requirements/login/web/requirements_login_web.md) — 44 REQ (40 trong phạm vi), 0 AMB treo |
| **Kết quả chạy đối chiếu** | ⚠️ Không đối chiếu được — xem **Phát hiện P4** |
| **Ngày review** | 23-09-2026 · Mode **REVIEW** (không sửa file TC nào) |
| **Kết quả** | 🟢 51 tốt · 🟡 0 cần sửa · 🔴 0 viết lại · `@Deprecated`: 0 |
| **Điểm trung bình** | **11.5 / 12** (28 TC 12/12 · 23 TC 11/12) |

> 📌 **Cách viết từng TC tốt** — không TC nào dưới 11/12. Vấn đề nằm ở **mức bộ TC và hạ tầng quanh nó**: một bộ TC thứ hai đánh số lại đang nằm trong `web/parts/`, tên biến `.env` trong TC không khớp `.env` thật, mốc git trong Nhật ký không tồn tại, và index khai "đủ 8/8 mục Email" trong khi thực tế thiếu 2 mục. Rubric 6 tiêu chí **không** nhìn thấy những lỗi này — đọc kỹ phần *Phát hiện mức bộ TC* trước bảng điểm.

---

## Phát hiện mức bộ TC (ưu tiên xử lý)

| # | Mức | Phát hiện | Căn cứ | Đề xuất |
|---|---|---|---|---|
| **P1** | 🔴 | **Hai bộ TC song song, TC ID xung đột.** `web/parts/part_01…` + `part_02…` chứa 54 TC `CRM_LOGIN_TC_001`→`054`, **đánh số lại** so với file chính: cùng mã, khác hành vi | `parts` `TC_007` = *"Đăng nhập bằng tài khoản Admin hợp lệ"* ↔ file chính `TC_007` = chuẩn hoá email · `parts` `TC_042` = CSRF ↔ file chính `TC_042` = ô mật khẩu · `parts` `TC_043` mang bug `…_TC039`. File chính dòng 9 ghi *"giữ 1 file theo quyết định user 19-09-2026 (không tách `parts/`)"*; index `## Bản đồ tài liệu` chỉ trỏ file chính | User quyết một trong hai: **(a)** xoá `web/parts/` (bản nháp chưa ai dùng) · **(b)** dùng `parts/` thì phải **giữ TC ID cũ** `001`→`051`, TC mới nối từ `052` — không được đánh lại, vì bug report, execution report, RTM đều trỏ theo mã cũ. Chưa quyết thì mọi workflow đọc thư mục `web/` có thể đọc nhầm |
| **P2** | 🔴 | **Tên biến `.env` trong TC không tồn tại.** `.env` hiện chỉ có `BASE_URL`, `EMAIL_ADMIN`, `PASSWORD_ADMIN` | File chính dùng `ADMIN_PASSWORD` (≈30 TC), `PM_EMAIL`/`PM_PASSWORD` (`TC_006`, `TC_023-c,d`), `CUSTOMER_EMAIL`/`CUSTOMER_PASSWORD` (`TC_019`, `TC_020`), `TC014_EMAIL`/`TC014_PASSWORD` (`TC_014`). Index dòng 14 ghi *"`ADMIN_*`, `PM_*`, `CUSTOMER_*`"* | Chốt **một** quy ước tên rồi sửa đồng loạt: đề xuất theo `.env` thật → `EMAIL_ADMIN`/`PASSWORD_ADMIN`, thêm `EMAIL_PM`/`PASSWORD_PM`, `EMAIL_CUSTOMER`/`PASSWORD_CUSTOMER`, `EMAIL_TC014`/`PASSWORD_TC014` vào `.env`. Chưa có tài khoản PM / Customer / staff TC014 → 5 TC (`006`, `014`, `019`, `020`, `023-c,d`) sẽ **BLOCKED** khi chạy |
| **P3** | 🔴 | **Mốc git trong Nhật ký không lấy lại được bản cũ.** Cả thư mục `docs/testcases/login/` đang **chưa được git theo dõi** (`??`) | `git cat-file -t` trả *"Not a valid object name"* cho cả 3 mốc `4550fd6`, `05efd17`, `5d10845`. Dòng Nhật ký 19-09-2026 ghi *"(chưa commit)"* | Commit bộ TC hiện tại **trước** mọi lần sửa tiếp. Ghi 1 dòng Nhật ký: *"Các mốc git trước 23-09-2026 không còn hiệu lực (docs dựng lại) — mốc gốc mới: `<hash>`"* |
| **P4** | 🟡 | **Không đối chiếu được kết quả chạy / bug.** `docs/executions/login/web/` có 2 thư mục `run_*` lúc bắt đầu review nhưng đã biến mất trong lúc review; `docs/bugs/` không tồn tại; cả hai chưa bao giờ được git theo dõi | Link gãy: `TC_039` → `BUG_login_1785678750_TC039.md`, `TC_046` → `BUG_login_1787226515_TC018.md`, index → `run_1789759574/execution_report.md` | Xác nhận với user thư mục đã bị xoá có chủ ý không. Nếu có → gỡ link gãy hoặc ghi *"(bug report không còn trong repo)"*; nếu không → khôi phục rồi chạy lại mục *Đối chiếu kết quả chạy* |
| **P5** | 🟡 | **Có thể đang thiếu 2 bug đã biết trong TC.** Tên ảnh evidence của lượt chạy đầu gợi ý FAIL ở 2 TC không mang `@KnownBug` | `TC_004_logo_redirect_actual.png` + `parts` `TC_004` ghi *"bug mở `BUG_login_1787226513_TC004` … địa chỉ gốc tự chuyển sang cổng đăng nhập khách hàng"* · `TC_029_email_not_cleared.png` ↔ Expected `TC_029`: *"ô Email trở về rỗng"*. Index mục Regression ghi *"5 bug đang mở"* nhưng bảng *"Ba TC nhiều khả năng FAIL"* chỉ có 3 | Cần execution report để chốt (P4). Nếu đúng: thêm `@KnownBug` + dòng 🐞 cho `TC_004`, `TC_029`; bảng FAIL đã biết thành 5 dòng |
| **P6** | 🟡 | **Index khai phủ Email đủ, thực tế thiếu 2 mục** | Index 4 vòng: *"Bảng Email: đủ 8/8 mục áp dụng"* — xem *Đối soát bảng 15 loại field* bên dưới | Bổ sung 2 biến thể (Gap #1, #2), sửa dòng khai về *"9/9 áp dụng: 8 ✅ · 1 ➖"* sau khi bổ sung |
| **P7** | 🟢 | **Số liệu index lệch nhau** | Dòng 7: *"40 trong phạm vi"* · Bản đồ tài liệu: *"39/39 REQ trong phạm vi"* · Bảng Coverage: *"40/40"*. Tổng biến thể khai **69**; đếm lại được **83** (17 TC gộp = 49 biến thể + 34 TC đơn) | Thống nhất 40/40. Ghi rõ cách đếm biến thể (có tính mục Bảng kiểm không) rồi cập nhật con số |
| **P8** | 🟢 | **Ghi chú đã cũ ở index** | Bộ chạy: *"Chờ recon bổ sung (`@NeedsVerify`) — TC_024 (chỉ mục 🔧`2`)"* trong khi bảng *Vùng chưa có evidence* ghi *"không còn vùng nào"* | Xoá dòng bộ chạy `@NeedsVerify` |
| **P9** | 🟢 | **2 TC có dòng 🔧 nhưng thiếu tag `@TechCheck`** → lọt khỏi bộ chạy DevTools | `TC_042`, `TC_044` | Thêm `@TechCheck` vào cột Tags, thêm 2 TC vào bộ chạy `@TechCheck` (25 → 27) |

---

## Chi tiết từng TC

Tiêu chí: ① Rõ ràng · ② Expected đo được · ③ Độc lập · ④ Test data · ⑤ Truy vết · ⑥ Trọng tâm

> Tên biến `.env` sai (P2) là lỗi **hệ thống**, sửa một lần cho cả bộ — **không** trừ điểm từng TC. Chỉ trừ ④ ở TC phụ thuộc **tài khoản chưa có** (PM, Customer, staff TC014).

### TC 11/12 — dùng được, có đề xuất nhỏ

| TC ID | Điểm | ① ② ③ ④ ⑤ ⑥ | Vấn đề chính (trích nguyên văn) | Đề xuất sửa |
|---|---|---|---|---|
| TC_002 | 11/12 | 2 2 2 2 2 1 | Mục `1`: *"Nhìn thấy đủ 5 thành phần … `Forgot Password?` nằm **dưới** nút Login"* — chỉ kiểm thứ tự 1 cặp; `TC_049`/`TC_050` lại kiểm *"đủ 7 thành phần"* → hai TC định nghĩa "đủ thành phần" khác nhau | Mục `1` → *"Từ trên xuống đúng thứ tự: logo `ANHTESTER` · tiêu đề `Login` · nhãn `Email Address` + ô · nhãn `Password` + ô · ô tích `Remember me` · nút `Login` · liên kết `Forgot Password?`"* (7 thành phần, khớp `TC_049`) |
| TC_003 | 11/12 | 2 1 2 2 2 2 | Mục `3` (REQ-37): *"Trang hiển thị đầy đủ và bấm được ngay khi vừa tải — không có hiệu ứng chờ tải kịch bản"* — trang có JS vẫn thoả câu này, nên mục này không phân biệt được đạt/không đạt | Chuyển REQ-37 sang kiểm chứng thực: *"`3` Tắt JavaScript cho `crm.anhtester.com` (`chrome://settings/content/javascript`), nạp lại → trang hiển thị và bấm `Login` được y hệt"*; hoặc ghi rõ *"phần chính không chứng minh được REQ-37 — chấm ở dòng 🔧"* |
| TC_004 | 11/12 | 2 2 2 2 2 1 | Expected: *"Thanh địa chỉ dừng ở `https://crm.anhtester.com/`"* — không nhắc bug đã biết (xem P5) | Nếu P5 đúng: thêm *"🐞 Hiện trạng FAIL — địa chỉ gốc tự chuyển sang cổng đăng nhập khách hàng (ghi địa chỉ dừng thực tế). Bug `BUG_login_1787226513_TC004` đang mở"* + tag `@KnownBug` |
| TC_006 | 11/12 | 2 2 2 1 2 2 | Test Data: *"giá trị `PM_EMAIL` trong `.env`"* — `.env` không có biến này, không rõ tài khoản PM còn tồn tại | Pre-Condition thêm *"Có tài khoản Project Manager, khai `EMAIL_PM`/`PASSWORD_PM` trong `.env`. Chưa có → BLOCKED"* |
| TC_008 | 11/12 | 2 1 2 2 2 2 | Bước 6: *"Thực hiện phần 🔧 nếu có người chạy được"* — REQ-08 **chỉ** kiểm chứng được ở 🔧; bỏ 🔧 thì TC vẫn PASS mà REQ chưa hề được kiểm | Thêm vào Expected: *"Bỏ phần 🔧 → ghi kết quả `PASS (chưa kiểm REQ-LOGIN-08)`, **không** tính REQ-08 là đã phủ"* — hoặc bắt buộc 🔧 cho riêng TC này |
| TC_009 | 11/12 | 2 1 2 2 2 2 | Như `TC_008` — *"Thực hiện phần 🔧 nếu có người chạy được"* | Như `TC_008`, cho REQ-LOGIN-09 |
| TC_014 | 11/12 | 2 2 2 1 2 2 | Pre-Condition: *"Đã có tài khoản staff test … `TC014_EMAIL` / `TC014_PASSWORD`"* — `.env` không có | Tạo tài khoản staff test (môi trường cho phép submit), khai `EMAIL_TC014`/`PASSWORD_TC014`; chưa có → BLOCKED |
| TC_016 | 11/12 | 1 2 2 2 2 2 | Pre-Condition: *"tắt tính năng tự điền của trình duyệt"* — không nói tắt ở đâu | *"Chrome: `chrome://settings/autofill` → tắt `Save and fill addresses`; `chrome://password-manager/settings` → tắt `Offer to save passwords` và `Sign in automatically`"* |
| TC_019 | 11/12 | 2 2 2 1 2 2 | *"`CUSTOMER_EMAIL` + `CUSTOMER_PASSWORD` trong `.env`"* — không có trong `.env` | Như `TC_006`, biến `EMAIL_CUSTOMER`/`PASSWORD_CUSTOMER` |
| TC_020 | 11/12 | 2 2 2 1 2 2 | Như `TC_019` | Như `TC_019` |
| TC_023 | 11/12 | 2 2 2 1 2 2 | Biến thể `c`,`d`: *"`PM_EMAIL` trong `.env`"* | Như `TC_006` |
| TC_024 | 11/12 | 2 1 2 2 2 2 | Bước 5: *"Thực hiện phần 🔧 nếu có người chạy được"*; phần chính tự nhận *"chỉ chứng minh biểu mẫu gửi được bình thường"* | Như `TC_008`, cho REQ-LOGIN-21 |
| TC_025 | 11/12 | 2 1 2 2 2 2 | Bước 1: *"Mở DevTools, sửa `value` của `input[name=csrf_token_name]`"* — selector CSS ở phần chính, TC `Auto Type` = `UI` | Bước 1 → *"Sửa mã chống giả mạo ẩn trong biểu mẫu theo biến thể (cách làm ở dòng 🔧)"*; chuyển *"DevTools → Elements → `input[name=csrf_token_name]` → sửa `value`"* xuống dòng 🔧 |
| TC_029 | 11/12 | 2 2 2 2 2 1 | Expected bước 5: *"ô Email trở về rỗng"* — hành vi phụ, không thuộc REQ-26, và có dấu hiệu đang FAIL (P5) | Tách *"ô Email trở về rỗng"* khỏi TC này (REQ-26 chỉ nói về thông báo). Nếu hành vi ô Email là yêu cầu thật → TC mới `CRM_LOGIN_TC_052` gắn REQ tương ứng |
| TC_030 | 11/12 | 2 2 2 2 1 2 | Cột REQ: *"REQ-LOGIN-26"* (Email không tồn tại) — TC kiểm **định dạng** email | Đổi REQ sang `REQ-LOGIN-23` (thành phần trang Quên mật khẩu, field spec `type=email`); nếu cần REQ riêng cho định dạng → đề nghị `/update-requirements-from-ticket` cấp `REQ-LOGIN-45` |
| TC_031 | 11/12 | 1 2 2 2 2 2 | Bước 2: *"Đọc mã trạng thái HTTP của phản hồi"* — Pre-Condition không nói phải mở DevTools / `curl` | Pre-Condition thêm *"Mở DevTools → tab Network trước bước 1 (hoặc chạy `curl -i <URL>`)"* |
| TC_032 | 11/12 | 2 1 2 2 2 2 | Expected bước 4: *"Bấm được, trình duyệt bắt đầu chuyển hướng đăng xuất"* — không có trạng thái cuối để chấm | *"4. Dừng ở `https://crm.anhtester.com/admin/authentication`, biểu mẫu đăng nhập rỗng"* — hoặc bỏ bước 4 (đã phủ ở `TC_035`) |
| TC_033 | 11/12 | 1 2 2 2 2 2 | Bước 1: *"Đặt cửa sổ về kích thước mobile `375×812`"* — cửa sổ Chrome trên desktop không thu được tới 375 px | *"DevTools → Toggle device toolbar (`Ctrl+Shift+M`) → nhập `375` × `812` → nạp lại `/admin/`"* (cách `TC_049` đang dùng) |
| TC_035 | 11/12 | 2 2 1 2 2 2 | Pre-Condition: *"**không** có bộ đếm giờ công việc nào đang chạy"* — tài khoản Admin **dùng chung**, người khác có thể đang bật timer | Thêm bước 0: *"Bấm đồng hồ đầu trang; nếu có timer đang chạy → dừng lại chỉ khi là task `Auto_LOGIN_*` do mình tạo, còn lại → BLOCKED, ghi tên task"* |
| TC_036 | 11/12 | 2 2 2 2 2 1 | Bước 4–5: *"(chỉ biến thể `a`)"* — biến thể mang bước riêng, thực chất là 2 hành vi (REQ-32 kết thúc phiên · REQ-33 URL nội bộ bị chặn) | Chấp nhận được. Nếu muốn gọn: chuyển bước 4–5 sang TC mới `CRM_LOGIN_TC_053` (REQ-33), `TC_036` chỉ còn REQ-32 |
| TC_040 | 11/12 | 2 2 2 2 1 2 | Cột REQ: *"REQ-LOGIN-14"* (thông báo sai thông tin) — TC kiểm mất mạng | Đổi sang `REQ-LOGIN-06` (như `TC_041`) + ghi *"Error Guessing"* |
| TC_042 | 11/12 | 2 2 2 2 2 1 | Gộp 3 quan sát khác loại: che ký tự · **không** có nút hiện/ẩn · dán được + đăng nhập. Thiếu tag `@TechCheck` dù có 🔧 | Chấp nhận Kiểu B cho 2 mục tĩnh; thêm `@TechCheck` (P9) |
| TC_044 | 11/12 | 2 1 2 2 2 2 | Expected bước 3: *"hộp hỏi `Lưu mật khẩu?`"* — chữ do **trình duyệt** sinh, đổi theo ngôn ngữ (cùng lý do `TC_012` cấm chấm nội dung bong bóng). Thiếu `@TechCheck` | *"3. Trình duyệt bật hộp đề nghị lưu mật khẩu (biểu tượng chìa khoá ở thanh địa chỉ) — **không** chấm theo chữ trong hộp"*; thêm `@TechCheck` |

### TC 12/12 — giữ nguyên

`TC_001`, `005`, `007`, `010`, `011`, `012`, `013`, `015`, `017`, `018`, `021`, `022`, `026`, `027`, `028`, `034`, `037`, `038`, `039`, `041`, `043`, `045`, `046`, `047`, `048`, `049`, `050`, `051` — 28 TC.

---

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | 🟡 Nông | Nhãn nguyên văn ✅ · trạng thái mặc định ✅ (`TC_002`). **Thứ tự** các thành phần chỉ kiểm 1 cặp (`Forgot Password?` dưới `Login`) — xem đề xuất `TC_002` |
| 1 | Open form | ✅ | `TC_001`, `TC_027` |
| 1 | Display | ✅ | `TC_002`, `TC_005`, `TC_006` |
| 1 | Input valid data | ✅ | `TC_005`, `TC_006` |
| 1 | Save | ✅ | `TC_005` — tạo phiên |
| 1 | Verify data | ✅ | `TC_005`, `TC_006`, `TC_008`, `TC_009` |
| 2 | UI Behavior | ✅ | `TC_042`, `TC_043`, `TC_044` |
| 2 | Required | ✅ | `TC_011` (đăng nhập) · `TC_028` (Quên mật khẩu) |
| 2 | Validation | 🟡 Nông | Email đăng nhập thiếu 2 mục · Email Quên mật khẩu chỉ có 1 mục — xem bảng 15 loại bên dưới |
| 2 | Equivalence Partitioning | ✅ | `TC_012`, `TC_013`, `TC_045`, `TC_046` |
| 2 | Boundary Value Analysis | ✅ | `TC_045`, `TC_046`, `TC_047`, `TC_048` |
| 2 | Business Rule | ✅ | `TC_013`, `TC_015`, `TC_019` |
| 2 | Decision Table | ➖ | Không có tổ hợp ≥ 3 điều kiện — đồng ý với index |
| 2 | State Transition | ✅ *(index ghi ➖ — lỗi ghi nhãn)* | Index: *"Chỉ có 2 trạng thái"*. Thực tế có ≥ 3: Chưa đăng nhập → Đã đăng nhập → {Hết hạn · Đã đăng xuất}, cộng nhánh Đã đăng nhập + timer → Chờ xác nhận. Các chuyển trạng thái **đã có TC** (`021`, `005`, `026`, `036`, `034`) → đổi nhãn thành ✅ và liệt kê |
| 2 | Dependency | ✅ | `TC_034`, `TC_037` |
| 2 | Use Case / Scenario | ✅ | Chuỗi `TC_005` → `TC_036` |
| 2 | Save / Edit / Delete | ➖ | Không có bản ghi nghiệp vụ |
| 2 | Error Guessing | 🟡 Nông | `TC_010`, `016`, `018`, `036`, `040` — thiếu đăng nhập cùng tài khoản ở 2 trình duyệt, đăng xuất ở tab khác (Gap #4, #5) — đáng kể vì tài khoản Admin **dùng chung** |
| 3 | Permission | ✅ ⚠️ | `TC_006`, `019`–`023` — viết đủ, nhưng 4/6 TC phụ thuộc tài khoản PM/Customer **không có trong `.env`** (P2). Chưa cấp tài khoản thì nhánh này thực chất chỉ chạy được `TC_021`, `TC_022` |
| 3 | Security | ✅ | `TC_017`, `024`, `025`, `026`, `036`, `037`, `039`, `047`, `051` |
| 3 | API | ⏭️ *(index ghi ➖ — lỗi ghi nhãn)* | Có API nhưng QA không có quyền — là **cố ý bỏ**, có người quyết (PO 11-09-2026). Đổi `➖` → `⏭️` + điều kiện rà lại *"khi QA được cấp quyền gọi API"* |
| 3 | Database | ⏭️ *(index ghi ➖)* | Như API |
| 3 | Integration | ➖ | Không có đăng nhập bên thứ ba (`TC_003`) — bề mặt tích hợp bằng 0, ➖ đúng |
| 3 | Logging / Audit | ⏭️ *(index ghi ➖ — thiếu người quyết)* | Có Activity Log nhưng tài khoản bị chặn → `⏭️`. Index chỉ ghi *"đề nghị PO hoặc đội Dev cấp"* — **thiếu người quyết** và ngày quyết |
| 4 | Compatibility | ✅ | `TC_050` — Chrome, Edge, Firefox |
| 4 | Responsive | ✅ | `TC_049`, `TC_033` |
| 4 | Accessibility | ✅ | `TC_038`, `TC_002` mục `6` (nhãn ăn khớp ô tích) |
| 4 | Performance | ✅ mức thô | `TC_041` |
| 4 | Regression | ➖ | Chưa bug nào được fix — hợp lệ, nhưng nguồn `docs/bugs/login/` hiện **không tồn tại** (P4) nên không kiểm lại được |
| 4 | E2E | ➖ | Thuộc `/generate-cross-module-test-plan` |

---

## Đối soát bảng 15 loại field

| Field | Loại | Mục đã có | Mục thiếu (nêu đích danh) |
|---|---|---|---|
| Email Address — trang Đăng nhập | Email (9 mục) | Format hợp lệ `TC_005` · Thiếu `@` `TC_012-a` · Thiếu domain `TC_012-b` · Nhiều `@` `TC_012-d` · Max length `TC_045`/`046`/`048` · Case `TC_007-a,b` · *(thêm)* khoảng trắng `TC_007-c,d` | 🔴 **Domain không hợp lệ** — VD `abc@example` (thiếu đuôi), `abc@example..com`, `abc@-example.com` · 🔴 **Ký tự đặc biệt hợp lệ trước `@`** — VD `first.last+tag@example.com` phải được coi là đúng định dạng → `Invalid email or password`. ➖ *Email đã tồn tại* — màn đăng nhập không tạo tài khoản |
| Password — trang Đăng nhập | Password (7 mục) | Max length `TC_047`/`048` · Copy-paste `TC_042` · Hiện/ẩn `TC_042` | ➖ Min length / ký tự đặc biệt / hoa-thường / số — màn đăng nhập không áp chính sách (`AMB-LOGIN-09`) · ➖ Confirm password — không có ô. **Đủ** |
| Remember me | Checkbox (4 mục) | Mặc định `TC_002-3` · Check/Uncheck `TC_002-6`, `TC_038` | ➖ Required · ➖ Nhóm radio. **Đủ** |
| Email Address — trang Quên mật khẩu | Email (9 mục) | Thiếu `@` `TC_030` · *(bắt buộc)* `TC_028` | 🟡 Thiếu domain · Nhiều `@` · Max length (phần trước `@` > 64 ký tự) — gửi được mà **không** kích hoạt gửi mail (email sai định dạng/không tồn tại). ➖ Case / khoảng trắng với email có thật — kích hoạt gửi mail, `REQ-LOGIN-27` ngoài phạm vi |

---

## Coverage Gaps (TC còn thiếu)

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất |
|---|---|---|---|
| 1 | Email đăng nhập **domain không hợp lệ**: `abc@example`, `abc@example..com` → ghi nhận thông báo (trình duyệt chặn hay máy chủ trả `The Email Address field must contain a valid email address.`) — cần recon trước, có thể thành biến thể mới của `TC_012` hoặc `TC_046` tuỳ loại phản hồi | V2 · Validation | High |
| 2 | Email đăng nhập có **ký tự đặc biệt hợp lệ** trước `@`: `first.last+tag@example.com`, `o'brien@example.com` → hệ thống chấp nhận định dạng, trả `Invalid email or password` (không bị chặn nhầm) | V2 · Validation / EP | Medium |
| 3 | Email trang Quên mật khẩu: thiếu domain `abc@` · nhiều `@` `a@b@example.com` · phần trước `@` 65 ký tự → thêm biến thể vào `TC_030` | V2 · Validation | Medium |
| 4 | Đăng nhập cùng tài khoản Admin ở 2 trình duyệt → phiên thứ nhất còn dùng được không (ghi nhận hành vi, không có REQ → mở AMB) | V2 · Error Guessing / V3 · Security | Medium |
| 5 | Mở `/admin/clients` ở tab A, đăng xuất ở tab B, quay lại tab A bấm menu → bị đưa về trang đăng nhập | V3 · Security | Medium |
| 6 | Nhấn `Enter` khi con trỏ đang ở ô `Password` → biểu mẫu gửi đi (hiện `TC_038` chỉ kiểm `Enter` trên nút `Login`) | V2 · UI Behavior | Low |

> TC mới (nếu làm Mode FIX) nối tiếp từ **`CRM_LOGIN_TC_052`** — sau khi giải quyết P1, vì `web/parts/` đang dùng dải `052`–`054` cho hành vi khác.

---

## TC trùng lặp — đề xuất merge

Không có cặp nào cần merge. Chồng lấn **có chủ ý**, giữ nguyên: `TC_043-a` ↔ `TC_011-a` (`TC_043` ghi rõ không chấm nội dung dải lỗi) · `TC_024` phần chính ↔ `TC_005` (khác nhau ở dòng 🔧).

---

## Ưu tiên / Priority

Hợp lý với risk. Một điểm lệch nhỏ: `TC_027` Risk `Medium` nhưng Priority `High` — `TC_028`/`029` cùng trang cũng `High`, nên chấp nhận; nếu muốn khớp thì hạ `TC_027` xuống `Medium`.

---

## Kết luận & Khuyến nghị

1. **Giải quyết P1 trước tiên** — quyết xoá hay dùng `web/parts/`. Dùng thì phải giữ TC ID `001`→`051` như file chính; hai bộ TC cùng mã khác nghĩa là lỗi truy vết nặng nhất của module
2. **Commit `docs/testcases/login/`** rồi ghi mốc gốc mới vào Nhật ký (P3) — hiện không có cách nào lấy lại bản cũ nếu sửa sai
3. **Chốt tên biến `.env`** và xin tài khoản PM / Customer / staff TC014 (P2) — không có thì nhánh Permission chỉ chạy được 2/6 TC
4. **Bổ sung 3 gap Validation** (#1–#3) và sửa dòng khai *"đủ 8/8 mục Email"* ở index (P6)
5. **Dọn index**: nhãn `➖`/`⏭️` ở V2 State Transition, V3 API/DB/Logging · số REQ 39 → 40 · tổng biến thể · dòng `@NeedsVerify` cũ · thêm `@TechCheck` cho `TC_042`, `TC_044` (P7–P9)

> Muốn agent sửa luôn → chạy lại `/review-testcases FIX docs\testcases\login\web` sau khi đã commit và quyết xong P1.

---

## Kết quả Mode FIX — 24-09-2026

**Quyết định của user ở checkpoint:**

| Điểm | Quyết định |
|---|---|
| Mốc git | Sửa luôn, **không** có mốc git — thư mục chưa được git theo dõi |
| P1 `web/parts/` | Giữ nguyên, không đụng — index ghi cảnh báo *không có hiệu lực* |
| P2 tên biến `.env` | Giữ tên trong TC — user tự đổi `.env` cho khớp |
| Phạm vi | **Chỉ** thêm TC cho Gap #1–#6. **Không** sửa 23 TC 11/12, **không** dọn index P6–P9 |

**TC đã thêm** (TC_001→TC_051 giữ nguyên):

| TC mới | Gap | Vòng / Nhánh | Căn cứ Expected |
|---|---|---|---|
| `CRM_LOGIN_TC_052` (4 biến thể) | #1 — tên miền sai, trình duyệt chặn | V2 · Validation | Đo 24-09-2026: ô Email đánh giá không hợp lệ với cả 4 chuỗi |
| `CRM_LOGIN_TC_053` | #1 — tên miền không đuôi `abc@example` | V2 · Validation | Đo 24-09-2026: trình duyệt cho qua, máy chủ trả `The Email Address field must contain a valid email address.` |
| `CRM_LOGIN_TC_054` (3 biến thể) | #2 — ký tự đặc biệt hợp lệ trước `@` | V2 · Validation / EP | Đo 24-09-2026: cả 3 trả `Invalid email or password` |
| `CRM_LOGIN_TC_055` (3 biến thể) | #3 — Quên mật khẩu, trình duyệt chặn | V2 · Validation | Đo 24-09-2026 |
| `CRM_LOGIN_TC_056` (2 biến thể) | #3 — Quên mật khẩu, máy chủ chặn | V2 · Validation | Đo 24-09-2026: trả lỗi định dạng, không phải `Email not found` |
| `CRM_LOGIN_TC_057` | #4 — hai phiên cùng tài khoản | V2 · Error Guessing | `@AssumptionBased` `@NeedsVerify` — giả định `ASM-10` |
| `CRM_LOGIN_TC_058` | #5 — đăng xuất ở tab khác | V3 · Security | `@NeedsVerify` — theo REQ-32/33 |
| `CRM_LOGIN_TC_059` | #6 — Enter trong ô Password | V2 · UI Behavior | `@NeedsVerify` — hành vi gửi form chuẩn HTML |

**Còn mở** (ngoài phạm vi lần FIX này): P1–P9, đề xuất cho 23 TC 11/12, và phát hiện phụ lúc đo: trang Quên mật khẩu **giữ lại** email trong ô sau lỗi định dạng (`TC_056`), cần đối chiếu với Expected *"ô Email trở về rỗng"* của `TC_029` (P5).
