# Test Cases — Module Đăng nhập / Xác thực (`LOGIN`) — tổng 51 TC (47 hiệu lực · 4 `@Deprecated`) · 1 nền tảng · độ hạt GỘP

| Thông tin | Nội dung |
|---|---|
| **Hệ thống** | Perfex CRM — Anh Tester Demo (`https://crm.anhtester.com`) |
| **Module** | Đăng nhập / Xác thực · prefix `LOGIN` |
| **Nguồn requirement** | [REQUIREMENTS_LOGIN_SUMMARY.md](../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) — 44 REQ, **40 trong phạm vi** |
| **Mode sinh** | QUICK (`/generate-testcases-from-requirements`) · **độ hạt GỘP**<br>Bổ sung 11-09-2026: 9 TC theo **Bản Đồ Loại Kiểm Thử — 4 Vòng** (`/generate-testcases-manual-rbt`) |
| **Ngày sinh** | 20-08-2026 · cập nhật ngôn ngữ kiểm chứng 20-08-2026 · bổ sung 4 vòng 11-09-2026 · **DELTA `adhoc_2026-09-19` + chuyển tầng nền tảng 19-09-2026** |
| **Dải TC ID** | `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_051` — `001`→`041` gốc, `042`→`050` bổ sung 4 vòng, `051` bổ sung theo `AMB-LOGIN-19` |
| **Mã kế tiếp** | `CRM_LOGIN_TC_052` — **KHÔNG đánh lại từ 001** |
| **Môi trường** | ⚠️ **Dùng chung** — mọi TC chỉ đọc hoặc hoàn tác được; không có TC nào phá huỷ dữ liệu nghiệp vụ |
| **Trình duyệt chuẩn** | Google Chrome, viewport desktop `1600×750` |
| **Tài khoản** | 🔒 Hệ thống **chỉ có role Admin** (user xác nhận 24-09-2026). Email `admin@example.com` (= `EMAIL_ADMIN` trong `.env`) · mật khẩu lấy từ `PASSWORD_ADMIN` trong `.env` — **KHÔNG** ghi mật khẩu thật vào tài liệu |

## Cách đọc bộ TC này

### 1. Độ hạt GỘP

TC ở đây được viết theo **độ hạt GỘP**: các biến thể của **cùng một trường** cho **cùng loại phản hồi** nằm chung một TC, liệt kê trong **Bảng biến thể** ở cột `Test Data` (hoặc **Bảng kiểm** ở cột `Expected Result` với TC kiểm tra tĩnh).

- Mỗi biến thể có mã riêng `a`, `b`, `c`… → báo cáo FAIL **bắt buộc** ghi rõ biến thể: `CRM_LOGIN_TC_012-c FAIL`
- Chạy một TC nghĩa là chạy **hết** biến thể của nó, không được bỏ dòng nào
- Sang automation, TC gộp map thành **test data-driven** (`@DataProvider` · `test.each` · `parametrize`), mỗi biến thể một bộ dữ liệu

> Cần mỗi biến thể là một TC riêng (khách hàng tính công theo số TC, hoặc cần pass-rate từng biến thể) → sinh lại bằng `/generate-testcases-from-requirements <đường dẫn> TÁCH`.

### 2. Dòng `🔧 Ghi chú kỹ thuật` — đọc kỹ trước khi chạy

Mọi bước và mọi kết quả kỳ vọng ở phần chính đều **quan sát được bằng mắt trên màn hình**. Tester chạy được toàn bộ bộ TC này mà **không cần mở DevTools**.

Một số yêu cầu — cookie ghi nhớ, mã chống CSRF, header bảo mật, mã trạng thái HTTP — **không có biểu hiện nhìn thấy được**. Phần kiểm chứng cấp kỹ thuật của chúng nằm ở dòng riêng mở đầu bằng:

```
🔧 Ghi chú kỹ thuật (cần DevTools): ...
```

| | |
|---|---|
| **Ai chạy phần chính** | Tester nghiệp vụ — chỉ dùng trình duyệt |
| **Ai chạy dòng 🔧** | Tester có kỹ năng DevTools, hoặc để dành cho automation |
| **TC có dòng 🔧** | Gắn tag **`@TechCheck`** — xem bộ chạy riêng ở cuối tài liệu |
| **Bỏ qua dòng 🔧 được không** | ✅ Được. TC vẫn chấm PASS/FAIL bình thường ở phần chính. Khi bỏ qua thì ghi `— (bỏ phần 🔧)` vào báo cáo |

🔒 Dòng 🔧 chỉ ghi **hình thái** giá trị bí mật (`<16 ký tự hex>`), **KHÔNG** chép giá trị thật vào báo cáo.

---

## Bản đồ tài liệu

> File này là **index** — không chứa dòng TC. TC chi tiết nằm ở file nền tảng.

| Nền tảng | File | Nhóm chức năng | Số TC | TC ID | REQ bao phủ |
|---|---|---|---|---|---|
| Web | [web/test_cases_login_web.md](web/test_cases_login_web.md) | A Giao diện · B Đăng nhập thành công · C Dữ liệu đầu vào · D Phiên & CSRF · E Quên mật khẩu · F Đăng xuất · G Phi chức năng · H Hành vi ô nhập · I Giá trị biên · K Tương thích | 51 (47 hiệu lực) | 001–051 | 39/40 REQ trong phạm vi — `REQ-LOGIN-43` không còn TC (không có tài khoản Customer) |
| Mobile | — | Chưa có | 0 | — | — |
| API | — | Chưa có | 0 | — | — |

---

## Assumptions đã áp dụng

| Mã | Điểm chưa rõ | Giả định đã dùng | TC / biến thể bị ảnh hưởng |
|---|---|---|---|
| ASM-01 | ✅ **Đã giải quyết 19-09-2026** — `AMB-LOGIN-19`: phiên 1 giờ tính theo **thời gian không hoạt động** hay **tổng thời gian từ lúc đăng nhập**? | Tạm hiểu là **thời gian không hoạt động**. PO chốt **trùng giả định** — mỗi thao tác gia hạn lại | `TC_026` — gỡ `@AssumptionBased` · thêm `TC_051` cho vế gia hạn |
| ASM-02 | ✅ **Đã giải quyết 19-09-2026** — đã chạy thật đầu-cuối. `REQ-LOGIN-30` mới có bằng chứng đọc mã nguồn `logout()`, chưa chạy thật đầu-cuối (`AMB-LOGIN-14` ⏭️ chuyển module `TASK`) | Viết TC theo đúng mã nguồn đã đọc, chấp nhận bằng chứng mức mã nguồn | `TC_034` — gỡ `@AssumptionBased` + `@NeedsVerify`, thêm bước tự dựng dữ liệu, `Automation = Yes` |
| ASM-03 | ✅ **Đã giải quyết** — đã recon thật. `REQ-LOGIN-29` nhánh mobile chưa có lượt recon nào — nguồn duy nhất là quyết định PO (`AMB-LOGIN-16`) | Viết TC theo quyết định PO; kỳ vọng ở TC_033 **chưa được đo** | `TC_033` — gỡ `@NeedsVerify` |
| ASM-04 | ✅ **Đã giải quyết** — lượt chạy thật xác nhận **trùng giả định**. Hệ thống xử lý chuỗi toàn khoảng trắng ở trường bắt buộc thế nào? | Giả định **cắt khoảng trắng trước khi kiểm trường bắt buộc**, nhất quán với `REQ-LOGIN-38` | `TC_011-d` — gỡ `@NeedsVerify` |
| ASM-05 | ✅ **Đã giải quyết 19-09-2026** — đo bằng 3 lần nạp trang cùng phiên và 2 phiên riêng: mã CSRF **cố định trong phiên**, phiên mới mới đổi. **Khác giả định** nhưng khớp đúng `REQ-LOGIN-21` ("đổi theo phiên") → không phải lỗi, không mở AMB · *Ban đầu:* `REQ-LOGIN-21` ghi mã CSRF "đổi theo phiên" — không rõ có sinh lại ở mỗi lần nạp trang không | Giả định **sinh lại ở mỗi lần nạp trang** — đã huỷ | `TC_024` mục 🔧`2` — sửa kỳ vọng, gỡ `@NeedsVerify` |
| ASM-06 | ✅ **Đã giải quyết 19-09-2026** — dùng tài khoản staff test riêng (`TC014_EMAIL` / `TC014_PASSWORD` trong `.env`). Mật khẩu tài khoản demo là **6 ký tự số** (`RISK-LOGIN-07`) nên không có chữ cái để thử đảo hoa/thường | TC chỉ chạy được khi tài khoản test có mật khẩu chứa chữ cái | `TC_014` — ⚠️ **`@Deprecated` 24-09-2026**: hệ thống chỉ có tài khoản Admin (mật khẩu toàn số), không có tài khoản staff test |
| ASM-07 | ✅ **Đã giải quyết 19-09-2026** — PO chốt bắt buộc ép HTTPS (`AMB-LOGIN-20` → `REQ-LOGIN-44`), trùng giả định · *Ban đầu:* không chuyển hướng. Requirements mục 9 ghi "toàn bộ qua HTTPS" nhưng **không** khẳng định có chuyển hướng ép buộc từ HTTP | Viết TC theo **chuẩn bảo mật** (phải chuyển sang HTTPS + có HSTS) | `TC_039` — `@KnownBug`, gỡ `@NeedsVerify` |
| ASM-08 | ✅ **Đã giải quyết** — lượt chạy thật xác nhận **trùng giả định**. Mã CSRF gửi rỗng có bị chặn như mã bị sửa không (chưa có evidence) | Giả định **bị chặn giống nhau** — cùng trang lỗi `419 Page Expired!` | `TC_025-b` — gỡ `@NeedsVerify` |
| ASM-09 | `REQ-LOGIN-08` (phát hành cookie ghi nhớ) không có hệ quả nghiệp vụ nào kiểm được bằng mắt, vì `REQ-LOGIN-40` ghi nhận tính năng tự đăng nhập lại **không hoạt động** (`AMB-LOGIN-15` ⏭️) | Phần chính của `TC_008`/`TC_009` chỉ chấm tới bước đăng nhập thành công; kiểm chứng cookie đặt ở dòng 🔧 và TC gắn `@TechCheck` | `TC_008`, `TC_009` — `@TechCheck` |

> ✅ **Không phát hiện xung đột nào giữa tài liệu và evidence.** Cả 9 ảnh đều xác nhận đúng mô tả trong `REQUIREMENTS_LOGIN_SUMMARY.md`.

---

## Bảng Đối Soát Coverage (39/40 REQ trong phạm vi)

> TC `@Deprecated` (`006`, `014`, `019`, `020`) **không** tính vào bảng này.

| REQ ID | Mô tả ngắn | Số TC | TC IDs (kèm biến thể) | Đủ Positive/Negative/Boundary? |
|---|---|---|---|---|
| REQ-LOGIN-01 | Truy cập trang đăng nhập | 1 | TC_001 | ✅ |
| REQ-LOGIN-02 | Thành phần biểu mẫu đăng nhập | 8 | TC_002 (6 mục — `1`–`4`, `6`, `7`), TC_038, TC_042, TC_043, TC_044, TC_048, TC_049, TC_050 | ✅ |
| REQ-LOGIN-03 | Tự đặt con trỏ vào ô Email | 1 | TC_002-`2` | ✅ |
| REQ-LOGIN-04 | Logo dẫn về trang chủ | 1 | TC_004 | ✅ |
| REQ-LOGIN-05 | Không có CAPTCHA | 1 | TC_003-`1` | ✅ (kiểm sự vắng mặt) |
| REQ-LOGIN-06 | Đăng nhập bằng thông tin hợp lệ | 5 | TC_005, TC_010, TC_040, TC_041, TC_050 | ✅ (1 vai trò — hệ thống chỉ có Admin) |
| REQ-LOGIN-07 | Email không phân biệt hoa thường | 1 | TC_007-`a`,`b` | ✅ |
| REQ-LOGIN-08 | Ghi nhớ đăng nhập sinh cookie ghi nhớ | 1 | TC_008 | ✅ (cặp với TC_009) |
| REQ-LOGIN-09 | Không tích thì không phát hành cookie | 1 | TC_009 | ✅ (nhánh phủ định của REQ-08) |
| REQ-LOGIN-10 | Bỏ trống cả hai trường | 2 | TC_011-`a`,`d`, TC_043-`a` | ✅ |
| REQ-LOGIN-11 | Bỏ trống riêng Email | 1 | TC_011-`b` | ✅ |
| REQ-LOGIN-12 | Bỏ trống riêng Mật khẩu | 1 | TC_011-`c` | ✅ |
| REQ-LOGIN-13 | Chặn email sai định dạng tại trình duyệt | 3 | TC_012-`a`→`e`, TC_045-`a`,`b`, TC_046-`a`→`c` | ✅ (client: TC_012 · máy chủ: TC_045/046) |
| REQ-LOGIN-14 | Thông báo khi sai thông tin đăng nhập | 6 | TC_013, TC_017, TC_018, TC_045, TC_047, TC_050 | ✅ — ca đảo hoa/thường mật khẩu mất theo `TC_014` `@Deprecated` |
| REQ-LOGIN-15 | Thông báo không tiết lộ email nào có thật | 1 | TC_013 (bước 5) | ✅ |
| REQ-LOGIN-16 | 🐞 Ô Email phải giữ lại email sau lỗi | 1 | TC_016 | ✅ (TC sẽ FAIL — đúng thiết kế) |
| REQ-LOGIN-17 | Chặn URL nội bộ khi chưa đăng nhập | 1 | TC_021-`a`,`b` | ✅ |
| REQ-LOGIN-18 | Không ghi nhớ URL đích sau chuyển hướng | 1 | TC_022 | ✅ |
| REQ-LOGIN-19 | Đã đăng nhập không vào lại trang đăng nhập | 1 | TC_023-`a` | ✅ |
| REQ-LOGIN-20 | Đã đăng nhập không vào trang Quên mật khẩu | 1 | TC_023-`b` | ✅ |
| REQ-LOGIN-21 | Biểu mẫu mang mã chống CSRF | 1 | TC_024 (🔧 3 mục) | ✅ |
| REQ-LOGIN-22 | Từ chối yêu cầu có mã CSRF sai | 1 | TC_025-`a`,`b` | ✅ |
| REQ-LOGIN-23 | Truy cập trang Quên mật khẩu | 2 | TC_027-`1`,`2`, TC_030-`a`→`d` (ô Email chặn sai định dạng) | ✅ |
| REQ-LOGIN-24 | Không có lối quay lại đăng nhập | 1 | TC_027-`3` | ✅ (kiểm sự vắng mặt) |
| REQ-LOGIN-25 | 🐞 Bỏ trống email phải báo trường bắt buộc | 1 | TC_028 | ✅ (TC sẽ FAIL — đúng thiết kế) |
| REQ-LOGIN-26 | Email không tồn tại ở Quên mật khẩu | 1 | TC_029 | ✅ |
| REQ-LOGIN-28 | Liên kết đặt lại mật khẩu sai/hết hạn | 1 | TC_031-`a`,`b` | ✅ |
| REQ-LOGIN-29 | Mỗi viewport có một lối đăng xuất dùng được | 2 | TC_032, TC_033 | ✅ |
| REQ-LOGIN-30 | Cảnh báo khi còn bộ đếm giờ đang chạy | 1 | TC_034 | ✅ — chạy thật đầu-cuối 19-09-2026 |
| REQ-LOGIN-31 | Đăng xuất ngay khi không có bộ đếm giờ | 1 | TC_035 | ✅ (nhánh phủ định của REQ-30) |
| REQ-LOGIN-32 | Kết thúc phiên và về trang đăng nhập | 1 | TC_036-`a`,`b` | ✅ |
| REQ-LOGIN-33 | URL nội bộ bị chặn sau khi đăng xuất | 1 | TC_036 bước 4–5 | ✅ |
| REQ-LOGIN-36 | Không có đăng nhập mạng xã hội | 1 | TC_003-`2` | ✅ (kiểm sự vắng mặt) |
| REQ-LOGIN-37 | Trang không nạp tệp JavaScript nào | 1 | TC_003-`3` | ✅ (kiểm sự vắng mặt) |
| REQ-LOGIN-38 | Email bỏ qua khoảng trắng thừa | 1 | TC_007-`c`,`d` | ✅ |
| REQ-LOGIN-39 | Cookie không tự đăng nhập lại sau đăng xuất | 1 | TC_037 | ✅ |
| REQ-LOGIN-41 | Không khoá tài khoản sau nhiều lần sai | 1 | TC_015-`a`,`b` | ✅ |
| REQ-LOGIN-42 | Phiên hết hạn sau 1 giờ không hoạt động, thao tác thì gia hạn | 2 | TC_026 (AC1 — để yên thì hết phiên), TC_051 (AC2 — thao tác thì gia hạn) | ✅ |
| REQ-LOGIN-43 | Customer không đăng nhập được vào `/admin` | 0 | ~~TC_019, TC_020~~ — `@Deprecated` 24-09-2026 | 🔴 Không còn TC — user xác nhận hệ thống chỉ có role Admin, **mâu thuẫn** với requirements (3 vai trò, `AMB-LOGIN-01`/`18`). Cần chốt lại bằng `/update-requirements-from-ticket` |
| REQ-LOGIN-44 | Ép truy cập qua HTTPS (chuyển hướng + HSTS) | 1 | TC_039 🐞 — chuyển từ `REQ-LOGIN-01` sang 19-09-2026 | ✅ |

**Kết luận:** 39/40 REQ trong phạm vi có ≥1 TC hiệu lực. **1 dòng 🔴** — `REQ-LOGIN-43`, do không có tài khoản Customer (24-09-2026). Tổng số **case** kiểm không đổi so với bản trước lần cập nhật ngôn ngữ — chỉ đổi cách phát biểu kỳ vọng.

### REQ cố ý KHÔNG có TC (ngoài phạm vi theo quyết định PO 18-08-2026)

| REQ ID | Lý do | Ambiguity |
|---|---|---|
| REQ-LOGIN-27 | Không kiểm chứng luồng gửi mail đặt lại với email có thật | `AMB-LOGIN-04` ⏭️ |
| REQ-LOGIN-34 | Cookie ghi nhớ không bị xoá khi đăng xuất — chỉ ghi nhận hiện trạng | `AMB-LOGIN-06` ⏭️ |
| REQ-LOGIN-35 | Cookie ghi nhớ thiếu cờ `HttpOnly` — chỉ ghi nhận hiện trạng cho `RISK-LOGIN-02` | `AMB-LOGIN-07` ⏭️ |
| REQ-LOGIN-40 | Ghi nhớ đăng nhập khi phiên hết hạn tự nhiên — PO xác nhận tính năng **không hoạt động** | `AMB-LOGIN-15` ⏭️ |

---

## Bảng Đối Soát Evidence

| Ảnh evidence | Màn hình / trạng thái | TC dựa vào | Đầy đủ? |
|---|---|---|---|
| `login_form_default_fullpage.png` | Đăng nhập — mặc định, hai ô rỗng | TC_001, TC_002, TC_003, TC_004, TC_027, TC_038 | ✅ full-page, thấy trọn khối biểu mẫu tới liên kết `Forgot Password?` |
| `login_form_filled_remember_checked_fullpage.png` | Đăng nhập — đã nhập đủ, **Remember me đang tích** | TC_002-`6`, TC_008, TC_024 | ✅ full-page, thấy rõ dấu tích ✔ trên ô tích |
| `login_form_empty_submit_error_fullpage.png` | Đăng nhập — gửi biểu mẫu rỗng, 2 dải báo lỗi | TC_011-`a`,`b`,`c` | ✅ full-page, xác nhận **thứ tự** dải: `The Password field is required.` **trên**, `The Email Address field is required.` **dưới** |
| `login_form_wrong_credentials_fullpage.png` | Đăng nhập — sai thông tin, dải `Invalid email or password` | TC_013, TC_016, TC_017 | ✅ full-page. ⚠️ Ô Email nền xanh nhạt = **trình duyệt tự điền**, KHÔNG phải hệ thống trả về — đây chính là bẫy mà TC_016 phải tránh bằng cửa sổ ẩn danh |
| `login_csrf_invalid_403_fullpage.png` | Đăng nhập — mã CSRF bị sửa, trang lỗi | TC_025-`a`, TC_010 | ✅ full-page, xác nhận nguyên văn `419 Page Expired!` và `Sorry, the page has expired, return to previous page and refresh to continue.` |
| `login_success_dashboard_viewport.png` | Dashboard — ngay sau khi đăng nhập, menu ảnh đại diện đóng | TC_005, TC_022, TC_023 | ✅ viewport (cố ý — tránh chụp dữ liệu khách hàng thật). Đếm được **14** mục menu trái của Admin |
| `logout_menu_open_viewport.png` | Dashboard — menu ảnh đại diện **đang mở** | TC_032 | ✅ viewport, xác nhận 5 mục theo thứ tự `My Profile` → `My Timesheets` → `Edit Profile` → `Language ▸` → `Logout`, và thanh đầu trang **không** có lối Logout thứ hai |
| `forgot_password_form_default_fullpage.png` | Quên mật khẩu — mặc định | TC_027, TC_030 | ✅ full-page, xác nhận đúng 1 ô `Email Address` + nút `Confirm`, không liên kết quay lại đăng nhập |
| `forgot_password_empty_submit_error_fullpage.png` | Quên mật khẩu — gửi khi bỏ trống, dải `Email not found` | TC_028, TC_029 | ✅ full-page, xác nhận **hiện trạng lỗi** mà TC_028 sẽ bắt được |
| `login_form_375x700_fullpage.png` | Đăng nhập — màn hình điện thoại `375×700` | TC_049-`e` | ✅ full-page, xác nhận không cuộn ngang, đủ 7 thành phần, khung rộng 311 px. **Chụp 11-09-2026** |

### Vùng chưa có evidence — TC / biến thể gắn `@NeedsVerify`

| Vùng / trạng thái | TC · biến thể | Đề xuất recon bổ sung |
|---|---|---|
| *(không còn vùng nào)* | — | `TC_024` mục 🔧`2` đã đo 19-09-2026 — xem `ASM-05` |

> ✅ **Đã giải quyết** (gỡ `@NeedsVerify` ngày 19-09-2026 theo `/review-testcases`): `TC_011-d`, `TC_025-b`, `TC_031-b`, `TC_033`, `TC_034`, `TC_039`, `TC_040`, `TC_041`, `TC_049-a/c/d`, `TC_050` — bằng chứng từ các lượt chạy thật trước đó.

---

## Đối soát loại kiểm thử (4 vòng)

> Bảng này bổ sung ngày 11-09-2026 theo **Bản Đồ Loại Kiểm Thử — 4 Vòng** của skill `skills-rbt-manual-testing`. Nó trả lời câu hỏi mà **Bảng Đối Soát Coverage không trả lời được**: coverage đếm theo REQ, nên bộ TC có thể phủ 39/39 REQ mà vẫn thiếu hẳn một *loại* kiểm thử.
>
> Đúng như vậy: lần chấm đầu tiên phát hiện **6 nhánh trống hoặc nông** trong khi coverage vẫn xanh toàn bộ. Chín TC `TC_042`→`TC_050` sinh ra để lấp đúng các nhánh đó.

| Vòng | Nhánh | Trạng thái | TC ID / Lý do |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | TC_002, TC_003 (2 TC · 9 mục bảng kiểm) |
| 1 | Open form | ✅ | TC_001, TC_027 (2 TC) |
| 1 | Display | ✅ | TC_002-`2`, TC_005 — module không hiển thị dữ liệu nghiệp vụ nên không có định dạng tiền/ngày/badge để kiểm |
| 1 | Input valid data | ✅ | TC_005 (hệ thống chỉ có role Admin) |
| 1 | Save | ✅ | TC_005 — "lưu" của module này là **tạo phiên đăng nhập** |
| 1 | Verify data | ✅ | TC_005, TC_008, TC_009 (3 TC — phiên và cookie ghi nhớ giữ đúng) |
| 2 | **UI Behavior** | ✅ **bổ sung 11-09-2026** | TC_042, TC_043, TC_044 (3 TC · 3 biến thể) — *trước đó nhánh này TRỐNG* |
| 2 | Required | ✅ | TC_011 (1 TC · 4 biến thể) |
| 2 | Validation | 🟡 nông | TC_007, TC_012, TC_017, TC_018, TC_030, TC_045, TC_046, TC_048 (8 TC)<br>• **Bảng Email (đăng nhập): 6/8 mục áp dụng** — còn thiếu *domain không hợp lệ* (VD `admin@example`) và *ký tự đặc biệt trước `@`* (VD `first.last+qa@example.com`). Phản hồi của hệ thống **chưa đo** → recon trước rồi mới viết TC (review 24-09-2026). Mục *"email đã tồn tại"* không áp dụng<br>• **Bảng Email (Quên mật khẩu): 4 ca sai định dạng** ở `TC_030-a`→`d`; ca vượt 64 ký tự trước `@` chưa đo<br>• **Bảng Password: đủ 4/4 mục áp dụng** — 3 mục không áp dụng, có lý do: *nút hiện/ẩn* không tồn tại (TC_042 xác nhận) · *ô xác nhận mật khẩu* không có trên màn này · *rule độ mạnh* (độ dài tối thiểu, ký tự đặc biệt, chữ hoa, chữ số) thuộc màn **Đăng ký / Đổi mật khẩu**, màn Đăng nhập chỉ xác thực chứ không áp rule |
| 2 | Equivalence Partitioning | ✅ | TC_012, TC_013, TC_045, TC_046 |
| 2 | **Boundary Value Analysis** | ✅ **bổ sung 11-09-2026** | TC_045, TC_046, TC_048 (3 TC · 6 biến thể) — `TC_047` chuyển sang Error Guessing 24-09-2026 (ô mật khẩu không có ngưỡng, `AMB-LOGIN-09`) — *trước đó nhánh này TRỐNG*, mốc 64 ký tự đo được ngày 11-09-2026 |
| 2 | Business Rule | ✅ | TC_013, TC_015 (2 TC) |
| 2 | Decision Table | ➖ | Màn đăng nhập không có tổ hợp từ 3 điều kiện trở lên cùng quyết định một kết quả — không đủ điều kiện kích hoạt kỹ thuật này |
| 2 | State Transition | ✅ | Trạng thái phiên theo requirements mục 7: chưa đăng nhập · đã đăng nhập · đã đăng nhập + lớp xác nhận timer · đã đăng xuất còn cookie. Chuyển tiếp: TC_005 (chưa → đã), TC_021 (chưa + URL nội bộ), TC_023 (đã + trang đăng nhập), TC_034 (đã → lớp xác nhận), TC_035/TC_036 (đã → đăng xuất), TC_037 (đăng xuất còn cookie), TC_026/TC_051 (hết hạn / gia hạn). *Sửa nhãn ➖ → ✅ theo review 24-09-2026* |
| 2 | Dependency | ✅ | TC_034 (bộ đếm giờ đang chạy chặn đăng xuất thẳng — chạy thật 19-09-2026), TC_037 (cookie ghi nhớ sau khi đăng xuất) |
| 2 | Use Case / Scenario | ✅ | Chuỗi TC_005 → TC_036 (đăng nhập → dùng → đăng xuất) |
| 2 | Save / Edit / Delete | ➖ | Module xác thực không có bản ghi nghiệp vụ để tạo/sửa/xoá |
| 2 | Error Guessing | ✅ | TC_010, TC_016, TC_018, TC_036, TC_040, TC_044, TC_047 (7 TC) |
| 3 | Permission | ✅ | TC_021, TC_022, TC_023 (3 TC · khách chưa đăng nhập × Admin). Hệ thống **chỉ có role Admin** (user xác nhận 24-09-2026) — `TC_006` (PM), `TC_019`, `TC_020` (Customer) `@Deprecated` |
| 3 | Security | 🟡 nông | TC_017, TC_024, TC_025, TC_026, TC_036, TC_037, TC_039, TC_047, TC_051 (9 TC). Thiếu ca **mã CSRF sai trên biểu mẫu Quên mật khẩu** — biểu mẫu có mã (`TC_027` 🔧) nhưng chưa TC nào kiểm bị chặn; phản hồi chưa đo → recon trước (review 24-09-2026) |
| 3 | API | ⏭️ | QA **không có quyền** gọi API — **đội Dev xác minh**. PO quyết 11-09-2026. Rà lại khi QA được cấp quyền gọi API |
| 3 | Database | ⏭️ | QA **không có quyền** truy cập cơ sở dữ liệu — **đội Dev xác minh**. PO quyết 11-09-2026. Rà lại khi QA được cấp quyền truy vấn CSDL |
| 3 | Integration | ➖ | QA không có quyền kiểm tầng tích hợp — **đội Dev xác minh**. TC_003 đã xác nhận trang đăng nhập **không** dùng đăng nhập bên thứ ba nào, nên bề mặt tích hợp của module này bằng không |
| 3 | Logging / Audit | ⏭️ | **Tài khoản demo bị chặn quyền.** Đo ngày 11-09-2026: mở `Utilities → Activity Log` trả về trang **Từ chối truy cập**. Cần tài khoản Super Admin để kiểm đăng nhập thành công/thất bại có được ghi vết không — **đề nghị PO hoặc đội Dev cấp**, rà lại ở đợt sau |
| 4 | Compatibility | ✅ **bổ sung 11-09-2026** | TC_050 (1 TC · 3 trình duyệt) — *trước đó nhánh này TRỐNG*. Danh sách **đã chốt 19-09-2026**: Chrome, Edge, Firefox |
| 4 | Responsive / UI Stability | ✅ **bổ sung 11-09-2026** | TC_049 (1 TC · 5 kích thước), TC_033 — *trước đó chỉ có TC_033 cho màn hình hẹp, không có TC nào cho chính trang đăng nhập* |
| 4 | Accessibility | ✅ | TC_038 (điều hướng bàn phím đầy đủ, viền tiêu điểm). Rà WCAG đầy đủ cần công cụ chuyên dụng → ngoài phạm vi, **đội Dev / chuyên gia a11y** |
| 4 | Performance | ✅ mức thô | TC_041 (đường truyền chậm). Đo tải thật và ngưỡng thời gian phản hồi ➖ — **đội Hạ tầng**, chưa có công cụ tải |
| 4 | Regression | ⏭️ | Module **chưa có bug report nào** trong repo nên chưa có lỗi cũ để chống tái phát. Ba TC `@KnownBug` (`TC_016`, `TC_028`, `TC_039`) sẽ sinh bug khi chạy. Rà lại nhánh này ngay khi bug đầu tiên được fix |
| 4 | E2E | ➖ | Module xác thực nằm trọn trong một màn hình. Luồng xuyên module thuộc phạm vi `/generate-cross-module-test-plan` |

**Tổng: 51 TC (47 hiệu lực · 4 `@Deprecated`) · 80 biến thể hiệu lực** — quy ước đếm: TC có Bảng biến thể tính theo số biến thể (18 TC · 51 biến thể), TC đơn tính 1 (29 TC); Bảng kiểm không tính thành biến thể. Đếm lại 24-09-2026 — con số 69 trước đó không khớp quy ước này.

---

## Rà soát đặc tính chất lượng (ISO/IEC 25010:2023)

| Đặc tính | Trạng thái | TC ID / Lý do |
|---|---|---|
| Functional Suitability | ✅ Có TC | TC_001 → TC_048 (trừ 4 TC `@Deprecated`) — phần thân của bộ TC |
| Performance Efficiency | ✅ một phần | TC_041 (đường truyền chậm). Đo tải và ngưỡng thời gian phản hồi ➖ — **đội Hạ tầng**, đợt sau |
| Compatibility | ✅ Có TC | TC_049 (5 kích thước màn hình), TC_050 (3 trình duyệt — Chrome, Edge, Firefox, đã chốt 19-09-2026) |
| Interaction Capability | ✅ Có TC | TC_011, TC_013 (thông báo lỗi dễ hiểu, không tiết lộ thừa), TC_038 (bàn phím), TC_042, TC_043, TC_044 (hành vi ô nhập và nút) |
| Reliability | ✅ Có TC | TC_026 (hết hạn phiên), TC_051 (gia hạn phiên), TC_040 (mất mạng giữa chừng), TC_047 (dữ liệu bất thường không làm sập) |
| Security | ✅ Có TC | TC_017, TC_021 → TC_025, TC_036, TC_037, TC_039 — chặn truy cập khi chưa đăng nhập, chống CSRF, vòng đời phiên. Pentest và quét lỗ hổng ➖ — **đội bảo mật / đối tác đánh giá độc lập** |
| Maintainability | ➖ Không áp dụng | Đặc tính của mã nguồn, không kiểm được bằng manual TC — **đội Dev**, qua code review và phân tích tĩnh |
| Flexibility | ✅ một phần | TC_049 (bố cục đáp ứng trên 5 kích thước). Đổi ngôn ngữ ➖ — mục `Language` nằm **sau** khi đăng nhập (thấy ở TC_032), thuộc module Hồ sơ người dùng chứ không thuộc module này |
| Safety | ➖ Không áp dụng | Ứng dụng nghiệp vụ; lỗi ở màn đăng nhập không gây thiệt hại vật lý hay tổn hại sức khoẻ |

---

## Bộ chạy đề xuất

| Bộ | TC | Số lượng | Thời gian ước tính |
|---|---|---|---|
| **Smoke** (`@Smoke`) | TC_001, TC_002, TC_005, TC_011-`a`, TC_013-`a`, TC_021-`a`, TC_032, TC_035, TC_036-`a` | 9 | ~12 phút |
| **Regression đầy đủ** | Toàn bộ trừ `@Slow` và `@Deprecated` | 45 | ~3 giờ 20 phút |
| **QA tự chạy — KHÔNG qua `/execute-test-cases`, KHÔNG automation** (`@Slow` + `@PersonalOnly`) | TC_026, TC_051 | 2 | ~65 + ~70 phút — chạy song song được nếu mở 2 cửa sổ trình duyệt riêng |
| **Cần DevTools** (`@TechCheck`) — phần 🔧 của các TC dưới đây | TC_001, TC_002, TC_003, TC_005, TC_008, TC_009, TC_012, TC_015, TC_016, TC_017, TC_018, TC_023, TC_024, TC_025, TC_027, TC_030, TC_032, TC_036, TC_037, TC_039, TC_041, TC_042, TC_044, TC_047, TC_048 | 25 | ~55 phút (chỉ phần 🔧) |
| **Không chạy** (`@Deprecated`) | TC_006, TC_014, TC_019, TC_020 — hệ thống chỉ có role Admin | 4 | — |

> 🔝 **Thứ tự chạy:** Nhóm B (đăng nhập thành công) chạy **đầu tiên** trong mọi bộ — `RISK-LOGIN-06` chỉ ra module này là cổng vào của 23 module còn lại, hỏng ở đây là chặn toàn bộ đợt kiểm thử.
>
> 🧑‍💻 **Bộ `@TechCheck` giao cho ai:** tester có kỹ năng DevTools, hoặc để automation phủ. Tester nghiệp vụ chạy **phần chính** của các TC đó bình thường và ghi `— (bỏ phần 🔧)` vào báo cáo. **Hai TC bắt buộc có người biết DevTools mới chạy được từ đầu:** `TC_025` (phải sửa mã ẩn) và `TC_041` (phải giả lập mạng chậm).
>
> ⚠️ **Báo cáo FAIL phải ghi rõ biến thể**, ví dụ `CRM_LOGIN_TC_012-c FAIL`. Ghi mỗi `TC_012 FAIL` là mất truy vết về case cụ thể.

---

## Nhật ký thay đổi

| Ngày | Nguồn | Thay đổi | TC ảnh hưởng |
|---|---|---|---|
| 24-09-2026 | `/review-testcases` mode FIX — [báo cáo review](review/testcase_review_report_web_20260924.md) · quyết định user: hệ thống **chỉ có role Admin**, dùng tài khoản Admin trong `.env`, coi `executions/` và `bugs/` như chưa tồn tại | **Tài khoản:** đổi khoá `ADMIN_PASSWORD` → `PASSWORD_ADMIN` cho khớp `.env`.<br>**`@Deprecated` 4 TC** (không xoá dòng): `TC_006` (PM), `TC_019`, `TC_020` (Customer), `TC_014` (không có tài khoản mật khẩu chứa chữ cái). `TC_023` bỏ biến thể `c`, `d` (PM), sửa Pre-Condition. → `REQ-LOGIN-43` **không còn TC** — mâu thuẫn với requirements (3 vai trò), cần chốt lại.<br>**Sửa 12 TC:** `TC_002` bỏ mục `5` (trùng `TC_042`), thêm mục `7` bỏ tích · `TC_003` mục `3` ghi rõ chỉ kiểm được ở 🔧 · `TC_025`, `TC_041` chuyển thao tác DevTools xuống 🔧 · `TC_030` REQ-26 → REQ-23, mở rộng 4 biến thể · `TC_032`, `TC_033` thêm tiền đề không có timer, `TC_032` chấm điểm dừng · `TC_040` REQ-14 → REQ-06 · `TC_042` chuyển thành Bảng kiểm 3 mục + `@TechCheck` · `TC_044` Expected theo ngôn ngữ Chrome + `@TechCheck` · `TC_047` `@Boundary` → `@ErrorGuessing` · `TC_039`, `TC_046` gỡ liên kết bug.<br>Đồng bộ index: coverage **39/40**, 4 vòng (State Transition ➖→✅, Validation/Security ✅→🟡, API/Database/Logging/Regression ➖→⏭️), tổng **80 biến thể**, ISO, bộ chạy.<br>**Mốc git trước khi sửa:** `web/test_cases_login_web.md` @ `e637fbb` · `TEST_CASES_LOGIN_SUMMARY.md` @ `e637fbb` | 🗑️ `CRM_LOGIN_TC_006`, `014`, `019`, `020` · ✏️ `002`, `003`, `023`, `025`, `030`, `032`, `033`, `039`, `040`, `041`, `042`, `044`, `046`, `047` — TC ID giữ nguyên |
| 21-09-2026 | Quy ước đặt tên | Đổi tên file index `test_cases_login.md` → `TEST_CASES_LOGIN_SUMMARY.md` — quy ước mới: index IN HOA để khác hẳn file nền tảng. Nội dung, mã REQ/TC không đổi; mọi link trỏ tới đã sửa | — |
| 19-09-2026 | Quyết định PO `AMB-LOGIN-20` → `REQ-LOGIN-44` | **`TC_039`:** cột REQ `REQ-LOGIN-01` → `REQ-LOGIN-44` (REQ cũ không nói gì về HTTPS — truy vết nhầm). Gỡ ghi chú "chưa được xác nhận là yêu cầu đã chốt". Thêm tiền đề **tắt HTTPS-Upgrades của Chrome** — Chrome tự nâng `http://` lên `https://` nên chấm bằng thanh địa chỉ sẽ PASS giả. Đo lại bằng `curl -I`: vẫn `200`, không HSTS → vẫn FAIL, bug giữ mở. `ASM-07` ✅. Độ phủ **40/40 REQ**, số TC không đổi.<br>**Mốc git trước khi sửa:** `5d10845` | `CRM_LOGIN_TC_039` ✏️ — TC ID giữ nguyên |
| 19-09-2026 | Recon bổ sung vùng chưa có evidence | **`TC_024` mục 🔧`2`:** đổi kỳ vọng từ "sau F5 mã **khác**" sang "mã **giữ nguyên** trong phiên, phiên mới mới đổi" theo kết quả đo (cùng phiên 3 lần nạp → cùng mã; 2 phiên → 2 mã khác). Gỡ `@NeedsVerify`, `ASM-05` ✅. Module **hết vùng chưa có evidence**. Không đổi TC ID, không đổi số TC | (chưa commit) |
| 19-09-2026 | `/review-testcases` mode FIX — [báo cáo review](review/testcase_review_report_web_20260919.md) · sửa **tại chỗ** theo quyết định user (không sinh file `_improved`) | **5 TC 🟡:** `TC_014` đổi sang tài khoản staff test riêng (`TC014_EMAIL`/`TC014_PASSWORD`) · `TC_018` **bỏ biến thể `a`** (email 260 ký tự) — Expected cũ `Invalid email or password` sai với mốc RFC 5321 đã chốt và trùng `TC_046-c`; đổi Test Scenario sang mật khẩu đa ngôn ngữ · `TC_034` thêm bước 0 dựng task + bật timer và bước 6 dọn dẹp, Expected đổi "hộp thoại" → "lớp xác nhận che toàn màn hình", `Automation` No → Yes · `TC_039` thay ghi chú bug cũ bằng hiện trạng FAIL đã xác nhận · `TC_050` ghi danh sách trình duyệt đã chốt, `Automation` Partial → Yes.<br>**Dọn ghi chú đã cũ ở 8 TC:** gỡ `@NeedsVerify` / `⚠️ chưa có evidence` ở `TC_011`, `025`, `031`, `033` (`Automation` Partial → Yes), `040`, `041`, `049`; `TC_046` gỡ `@KnownBug` (bug đã đóng). `TC_020` bỏ phụ thuộc `TC_019` — tự đăng nhập cổng khách hàng.<br>Đồng bộ index: Assumptions `ASM-02/03/04/06/07/08` ✅ · bảng *Vùng chưa có evidence* còn 1 dòng (`TC_024` 🔧`2`) · 4 vòng, ISO, bộ chạy. Độ phủ REQ không đổi.<br>**Mốc git trước khi sửa:** `web/test_cases_login_web.md` @ `4550fd6` | ✏️ `CRM_LOGIN_TC_011`, `014`, `018`, `020`, `025`, `031`, `033`, `034`, `039`, `040`, `041`, `046`, `049`, `050` — TC ID giữ nguyên |
| 19-09-2026 | Kết quả chạy [run_1789759574](../../executions/login/web/run_1789759574/execution_report.md) + quyết định user | **`TC_048`: bỏ biến thể `b` (ô `Password` 300 ký tự).** Bước `Ctrl+A` → `Ctrl+C` → đếm **không làm được** trên ô mật khẩu — mọi trình duyệt chặn sao chép nội dung ra khỏi ô mật khẩu, tester chạy tay cũng không copy được. TC giờ chỉ kiểm ô `Email`; đổi Test Scenario thành *"Ô Email không tự cắt bớt ký tự…"*. Phần độ dài ô `Password` giữ ở dòng 🔧 (đo bằng DevTools), chuỗi mật khẩu dài vẫn do `TC_047` phủ. Độ phủ REQ không đổi.<br>**Mốc git trước khi sửa:** `web/test_cases_login_web.md` @ `4550fd6` | `CRM_LOGIN_TC_048` ✏️ — TC ID giữ nguyên |
| 19-09-2026 | `/update-testcases-from-impact` APPLY — `adhoc_2026-09-19` (quyết định PO `AMB-LOGIN-19`) | **Chuyển bộ TC sang tầng nền tảng**: 51 TC chuyển nguyên văn sang [web/test_cases_login_web.md](web/test_cases_login_web.md), file này thành index (thêm `## Bản đồ tài liệu`). Giữ 1 file dù vượt ngưỡng 50 — quyết định user.<br>`TC_026`: gỡ `@AssumptionBased` và cảnh báo `AMB-LOGIN-19` còn treo. `ASM-01` → ✅. Thêm `TC_051` (V3 · Security) cho vế **gia hạn khi thao tác** của `REQ-LOGIN-42`.<br>`TC_026`, `TC_051` → `Automation = No` (không automation) + `@PersonalOnly` (`/execute-test-cases` bỏ qua) — **QA tự chạy và theo dõi** (quyết định user).<br>Đổi mã AMB/RISK sang dạng có prefix module (`AMB-19` → `AMB-LOGIN-19`…). Bỏ thư mục `archive/` — bản cũ tra bằng git.<br>**Mốc git trước khi sửa:** `TEST_CASES_LOGIN_SUMMARY.md` @ `05efd17` | `CRM_LOGIN_TC_026` ✏️ · `CRM_LOGIN_TC_051` ➕ — TC ID `001`→`050` giữ nguyên |
| 11-09-2026 | `/generate-testcases-manual-rbt` — chấm **Bản Đồ Loại Kiểm Thử 4 Vòng** lên bộ TC đang có | **Bổ sung 9 TC (`TC_042`→`TC_050`), KHÔNG đụng TC_001→TC_041.** Chấm bảng 4 vòng phát hiện 6 nhánh trống hoặc nông trong khi Bảng Đối Soát Coverage vẫn xanh 39/39 REQ: `V2 · UI Behavior` (trống) · `V2 · BVA` (trống) · `V2 · Validation` bảng Password/Email chưa đối soát đủ mục · `V3 · Logging` (chưa chấm) · `V4 · Compatibility` (trống) · `V4 · Responsive` (chỉ có TC_033).<br>**Recon thật ngày 11-09-2026 chốt được:** mốc độ dài phần trước `@` là **64 ký tự** (RFC 5321) — 64 cho `Invalid email or password`, 65 đổi sang `The Email Address field must contain a valid email address.`; ô nhập **không** giới hạn số ký tự; ô mật khẩu **không** chặn dán và **không** có nút hiện/ẩn; nút `Login` **không** khoá theo trạng thái biểu mẫu; `Utilities → Activity Log` trả **Từ chối truy cập** trên tài khoản demo.<br>⚠️ **Ảnh hưởng tới bug đang mở:** phát hiện của TC_046 cho thấy ca của [BUG_login_1787226515_TC018](../../bugs/login/web/BUG_login_1787226515_TC018.md) là **ranh giới chuẩn RFC, không phải lỗi** — đề nghị xem lại và nhiều khả năng đóng với trạng thái *không phải lỗi*.<br>Bổ sung **Bảng Đối soát loại kiểm thử (4 vòng)** và **Bảng rà soát đặc tính chất lượng ISO/IEC 25010:2023** (trước đó bộ TC chưa có bảng ISO vì sinh trước khi skill có tiêu chí 9). Thêm 1 ảnh evidence `login_form_375x700_fullpage.png`. Bản trước lưu ở `archive/test_cases_login_v3_truoc_bo_sung_4vong_20260911.md` (thư mục `archive/` đã bỏ 19-09-2026 — tra bằng `git log -- docs/testcases/login/`) | Thêm mới `CRM_LOGIN_TC_042` → `CRM_LOGIN_TC_050`<br>TC_001→TC_041 **giữ nguyên hoàn toàn** — execution report `run_1787215085` và 6 bug đang mở vẫn truy vết đúng |
| 20-08-2026 | Phản hồi người dùng — TC chứa quá nhiều kiểm chứng thuộc tính HTML | **Viết lại toàn bộ Test Steps và Expected Result theo ngôn ngữ người dùng nhìn thấy.** Mọi `document.title`, `body.className`, `checkValidity()`, `offsetParent`, `querySelector`, selector CSS, mã HTTP và tab Network được gỡ khỏi phần chính; nội dung cấp kỹ thuật tách xuống dòng `🔧 Ghi chú kỹ thuật (cần DevTools)` và TC gắn thêm tag `@TechCheck` (23 TC).<br>**TC ID giữ nguyên `001`→`041`** — không đánh lại, không cắt case nào, độ phủ vẫn 39/39 REQ. Bổ sung `ASM-09` giải thích vì sao `TC_008`/`TC_009` không có kỳ vọng nhìn thấy được. `TC_016` đổi cách chạy sang **cửa sổ ẩn danh + tắt tự điền** để tester bắt được lỗi mà không cần View Source. Bổ sung bộ chạy `@TechCheck` và mục "Cách đọc" phần 2.<br>Luật gốc đã đưa vào `.claude/skills/skills-rbt-manual-testing/SKILL.md` mục **Quy Tắc Ngôn Ngữ Kiểm Chứng** (+ tiêu chí 8 của Self-Quality Gate) nên lần sinh sau không lặp lại. Bản trước lưu ở `archive/test_cases_login_v2_GOP_domdanguage_20260820.md` (thư mục `archive/` đã bỏ 19-09-2026 — tra bằng `git log -- docs/testcases/login/`) | Toàn bộ `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_041` |
| 20-08-2026 | Yêu cầu người dùng — gộp TC nhỏ lẻ | **Sinh lại toàn bộ ở độ hạt GỘP: 82 TC → 41 TC.** Độ phủ REQ **không đổi** (39/39) và **không case nào bị bỏ**. TC ID đánh lại từ `001` — chấp nhận được vì bộ TC 82 chưa có script / execution report / RTM nào trỏ vào | Toàn bộ `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_041` |
| 20-08-2026 | `/generate-testcases-from-requirements` Mode QUICK | Khởi tạo bộ TC từ `REQUIREMENTS_LOGIN_SUMMARY.md` ở **độ hạt TÁCH** — 82 TC, 3 part. Mở 9/9 ảnh evidence, không phát hiện xung đột tài liệu ↔ ảnh | (bản đầu tiên) |
