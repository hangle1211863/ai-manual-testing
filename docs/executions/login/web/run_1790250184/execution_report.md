# Execution Report — LOGIN · Manual Execution (Web)

| Thông tin | Nội dung |
|---|---|
| Run ID | run_1790250184 |
| Nền tảng | `web` |
| Nguồn TC | docs/testcases/login/web/test_cases_login_web.md |
| Phạm vi | 45 TC (loại 4 TC `@Deprecated`: 006, 014, 019, 020; loại 2 TC `@PersonalOnly`: 026, 051 — không chạy qua workflow này) trên tổng 47 TC hiệu lực của tài liệu |
| Môi trường | `https://crm.anhtester.com/admin/authentication` — Production/Demo dùng chung |
| Build / Version | Không xác định |
| Tài khoản | `admin@example.com` (Admin) |
| Người thực hiện | User tự đăng nhập lần đầu trong Browser pane; agent được user cho phép tự nhập Password ở các bước Test Steps cần giá trị đúng/sai theo TC (phạm vi phiên chạy này) + agent thực thi toàn bộ |
| Bắt đầu → Kết thúc | 24-09-2026 18:43 → 19:26 (~43 phút) |
| Môi trường dùng chung? | Có — auto-skip TC thao tác phá huỷ đang BẬT |
| Công cụ | Browser pane (`mcp__Claude_Browser__*`) — không phải Playwright MCP; viewport reset về preset desktop giữa các nhóm TC responsive |
| Xử lý TC cần công cụ ngoài phạm vi Browser pane | Chấm `⚠️ BLOCKED`, ghi rõ lý do và công cụ cần có |

## 1. Tổng kết

| Trạng thái | Số lượng | Tỷ lệ |
|---|---|---|
| ✅ PASS | 36 | 80.0% |
| ❌ FAIL | 5 | 11.1% |
| ⚠️ BLOCKED | 4 | 8.9% |
| ⏭️ SKIPPED | 0 | 0% |
| **Tổng đã chạy** | **45** | 100% |

> **Pass rate (không tính BLOCKED, SKIPPED):** 36/41 = 87.8%
> Không có TC nào bị SKIPPED vì lý do "thao tác phá huỷ trên môi trường dùng chung" — bộ TC LOGIN không có thao tác phá huỷ hàng loạt nào cần auto-skip.

## 2. Kết quả từng TC

| TC ID | Test Scenario | Kết quả | Bước fail | Ghi chú |
|---|---|---|---|---|
| CRM_LOGIN_TC_001 | Mở trang đăng nhập khi chưa có phiên | ✅ PASS | — | — |
| CRM_LOGIN_TC_002 | Biểu mẫu đủ thành phần, trạng thái mặc định | ✅ PASS | — | Mục 5 đã bỏ theo TC |
| CRM_LOGIN_TC_003 | Không CAPTCHA, không đăng nhập bên thứ 3, không JS | ✅ PASS | — | — |
| CRM_LOGIN_TC_004 | Bấm logo về trang chủ công khai | ❌ FAIL | Bước 3 | Xem chi tiết #1 |
| CRM_LOGIN_TC_005 | Đăng nhập Admin hợp lệ vào Dashboard | ✅ PASS | — | — |
| CRM_LOGIN_TC_007 | Email chuẩn hoá (hoa/thường, khoảng trắng) | ✅ PASS | — | 4/4 biến thể a,b,c,d đều đăng nhập thành công |
| CRM_LOGIN_TC_008 | Tích Remember me phát hành cookie ghi nhớ | ✅ PASS | — | 🔧 chưa kiểm — không có quyền truy cập DevTools/Application→Cookies qua công cụ Browser pane hiện có (cookie `autologin` là HttpOnly, JS không đọc được); phần chính (UI, đăng nhập thành công khi tích Remember me) đã verify PASS |
| CRM_LOGIN_TC_009 | Không tích Remember me thì không phát hành cookie | ✅ PASS | — | 🔧 chưa kiểm — cùng lý do TC_008; phần chính đã verify PASS |
| CRM_LOGIN_TC_010 | Bấm Login 2 lần liên tiếp vẫn vào đúng Dashboard | ✅ PASS | — | — |
| CRM_LOGIN_TC_011 | Kiểm tra trường bắt buộc | ✅ PASS | — | 4/4 biến thể a,b,c,d đúng thứ tự và nội dung dải báo lỗi |
| CRM_LOGIN_TC_012 | Email sai định dạng bị trình duyệt chặn | ✅ PASS | — | 5/5 biến thể — `checkValidity()` = false, không có POST được gửi |
| CRM_LOGIN_TC_013 | Thông báo lỗi chung, không lộ email có thật | ✅ PASS | — | 2 biến thể cho kết quả giống hệt nhau |
| CRM_LOGIN_TC_015 | Sai nhiều lần liên tiếp không khoá tài khoản/IP | ✅ PASS | — | 6 lần (biến thể a) + 5 lần (biến thể b) đều trả `Invalid email or password`, không khoá; đăng nhập lại thành công ngay sau đó; mọi response HTTP `200`, không có `429` |
| CRM_LOGIN_TC_016 | 🐞 Ô Email phải giữ email sau đăng nhập lỗi | ❌ FAIL | Bước 5 | Xem chi tiết #2 — bug đã biết `AMB-LOGIN-10`, FAIL là kết quả đúng |
| CRM_LOGIN_TC_017 | Chuỗi tấn công XSS/SQLi trong Password bị chặn | ✅ PASS | — | 2/2 biến thể — không hộp thoại, không lỗi CSDL hiển thị |
| CRM_LOGIN_TC_018 | Mật khẩu Unicode + emoji không gây lỗi máy chủ | ✅ PASS | — | Trang nạp lại bình thường, thông báo đúng tiếng Anh |
| CRM_LOGIN_TC_021 | Mở thẳng URL nội bộ khi chưa đăng nhập bị đưa về Login | ✅ PASS | — | 2/2 biến thể |
| CRM_LOGIN_TC_022 | Không giữ lại URL đích khi bị chuyển về Login | ✅ PASS | — | — |
| CRM_LOGIN_TC_023 | Đang có phiên mở Login/Forgot Password đều về Dashboard | ✅ PASS | — | 2/2 biến thể, body class `user-id-2` xác nhận |
| CRM_LOGIN_TC_024 | CSRF token đúng hình thái, cố định trong phiên | ✅ PASS | — | 32 hex ký tự, không đổi qua F5; đăng nhập thành công xác nhận gián tiếp tham số gửi đúng |
| CRM_LOGIN_TC_025 | CSRF token không hợp lệ bị chặn (419) | ✅ PASS | — | 2/2 biến thể (sửa 32 số `0` và xoá rỗng) đều trả đúng "419 Page Expired!" |
| CRM_LOGIN_TC_027 | Trang Quên mật khẩu đủ thành phần, không lối quay lại Login | ✅ PASS | — | — |
| CRM_LOGIN_TC_028 | 🐞 Bỏ trống email ở Quên mật khẩu phải báo trường bắt buộc | ❌ FAIL | Bước 4 | Xem chi tiết #3 — bug đã biết `AMB-LOGIN-05`, FAIL là kết quả đúng |
| CRM_LOGIN_TC_029 | Email không tồn tại báo không tìm thấy | ❌ FAIL | Bước 5 | Xem chi tiết #4 — phát hiện mới, chưa có trong danh sách bug đã biết |
| CRM_LOGIN_TC_030 | Email sai định dạng ở Quên mật khẩu bị trình duyệt chặn | ✅ PASS | — | 4/4 biến thể |
| CRM_LOGIN_TC_031 | Liên kết đặt lại mật khẩu không hợp lệ → HTTP 500 thân rỗng | ✅ PASS | — | 2/2 biến thể — hành vi được chấp nhận theo `AMB-LOGIN-03` |
| CRM_LOGIN_TC_032 | Desktop chỉ có 1 lối đăng xuất dùng được (menu avatar) | ✅ PASS | — | Locator `.dropdown-menu > li.header-logout` xác nhận đúng theo ghi chú TC |
| CRM_LOGIN_TC_033 | Mobile đăng xuất qua menu điều hướng thu gọn | ✅ PASS | — | — |
| CRM_LOGIN_TC_034 | Đăng xuất khi còn timer đang chạy phải xác nhận trước | ✅ PASS | — | Dữ liệu test đã dọn — xem mục 5 |
| CRM_LOGIN_TC_035 | Không timer thì Logout đi thẳng, không hỏi lại | ✅ PASS | — | — |
| CRM_LOGIN_TC_036 | Đăng xuất kết thúc phiên, Back không dùng lại được | ✅ PASS | — | 2/2 biến thể |
| CRM_LOGIN_TC_037 | Cookie ghi nhớ còn lại sau đăng xuất không tự đăng nhập lại | ✅ PASS | — | 🔧 chưa kiểm — cùng lý do TC_008 (HttpOnly cookie); phần chính đã verify PASS |
| CRM_LOGIN_TC_038 | Điều hướng & gửi biểu mẫu hoàn toàn bằng bàn phím | ✅ PASS | — | Verify bằng `document.activeElement` sau mỗi Tab; Enter submit thành công |
| CRM_LOGIN_TC_039 | 🐞 HTTP thuần phải bị ép chuyển sang HTTPS | ❌ FAIL | Bước 3 | Xem chi tiết #5 — bug đã biết `AMB-LOGIN-20`, FAIL là kết quả đúng |
| CRM_LOGIN_TC_040 | Mất mạng giữa lúc gửi biểu mẫu không tạo phiên | ⚠️ BLOCKED | — | Xem mục 4 — Browser pane không có công cụ ngắt kết nối mạng |
| CRM_LOGIN_TC_041 | Đăng nhập trên đường truyền chậm vẫn hoàn tất | ⚠️ BLOCKED | — | Xem mục 4 — Browser pane không có công cụ giả lập Slow 3G |
| CRM_LOGIN_TC_042 | Ô Password che ký tự, dán được, không có nút hiện/ẩn | ✅ PASS | — | 3/3 mục |
| CRM_LOGIN_TC_043 | Nút Login luôn bấm được, không tự khoá | ✅ PASS | — | 3/3 biến thể |
| CRM_LOGIN_TC_044 | Biểu mẫu không chặn tự điền/đề nghị lưu mật khẩu Chrome | ⚠️ BLOCKED | — | Xem mục 4 — hộp thoại lưu mật khẩu là UI trình duyệt (Chrome), nằm ngoài nội dung trang; công cụ Browser pane (`read_page`/`javascript_tool`/`computer screenshot`) chỉ truy cập được nội dung trang, không thấy được thanh thông báo/UI trình duyệt |
| CRM_LOGIN_TC_045 | Email 64 ký tự trước @ vẫn coi là đúng định dạng | ✅ PASS | — | 2/2 biến thể (63, 64) |
| CRM_LOGIN_TC_046 | Email vượt 64 ký tự trước @ bị chặn ở bước định dạng | ✅ PASS | — | 3/3 biến thể (65, 100, 250) |
| CRM_LOGIN_TC_047 | Mật khẩu rất dài không làm hệ thống lỗi | ✅ PASS | — | 2/2 biến thể (256, 1000) |
| CRM_LOGIN_TC_048 | Ô Email không tự cắt bớt ký tự khi dán chuỗi dài | ✅ PASS | — | 300/300 ký tự giữ nguyên ở cả Email và Password, không `maxlength` |
| CRM_LOGIN_TC_049 | Trang đăng nhập giữ đúng bố cục ở mọi kích thước màn hình | ✅ PASS | — | 5/5 biến thể (1920×1080, 1600×750, 1366×700, 768×1024, 375×700) — không cuộn ngang |
| CRM_LOGIN_TC_050 | Luồng đăng nhập chạy được trên các trình duyệt đã cam kết | ⚠️ BLOCKED | — | Biến thể `a` (Chrome) đã verify PASS đầy đủ; biến thể `b` (Edge), `c` (Firefox) BLOCKED — xem mục 4 |

## 3. Chi tiết TC FAIL

### FAIL #1 — CRM_LOGIN_TC_004 · Bấm logo trên trang đăng nhập thì về trang chủ công khai

| | |
|---|---|
| REQ ID | REQ-LOGIN-04 |
| Priority | Low |
| Bước fail | Bước 3 |
| **Expected** | Thanh địa chỉ dừng ở `https://crm.anhtester.com/` — không còn ở khu `/admin` (trang chủ công khai) |
| **Actual** | Bị chuyển hướng tiếp sang `https://crm.anhtester.com/authentication/login` (trang đăng nhập cổng khách hàng, tiêu đề "Please login") — không dừng ở `https://crm.anhtester.com/` |
| Ghi chú | Xác nhận độc lập: gõ thẳng `https://crm.anhtester.com/` (không qua bấm logo) cũng bị chuyển hướng y hệt tới `/authentication/login` — đây là hành vi của hệ thống (không có trang chủ công khai khi chưa đăng nhập cổng khách hàng), không phải lỗi riêng của thao tác bấm logo |
| Evidence | evidence/CRM_LOGIN_TC_004_logo_redirect.png |
| Tái hiện được? | Có — 2/2 lần thử (bấm logo và gõ URL trực tiếp) |

### FAIL #2 — CRM_LOGIN_TC_016 · Sau khi đăng nhập thất bại, ô Email phải còn giữ email vừa nhập

| | |
|---|---|
| REQ ID | REQ-LOGIN-16 |
| Priority | High |
| Bước fail | Bước 5 |
| **Expected** | Ô Email phải còn hiển thị `admin@example.com` sau khi trang nạp lại |
| **Actual** | Ô Email trở về rỗng (`value=""`, không có thuộc tính `value` trong HTML server trả về) |
| Ghi chú | 🐞 Bug đã biết `AMB-LOGIN-10`, PO đã xác nhận là lỗi hệ thống. FAIL là kết quả **đúng** theo tài liệu TC — không sửa TC. Đã dọn: chạy trong Browser pane không có hồ sơ tự điền đã lưu, tương đương môi trường cửa sổ ẩn danh |
| Evidence | evidence/CRM_LOGIN_TC_016_email_not_retained.png (ảnh trang đăng nhập tham chiếu — trạng thái lỗi giao dịch cụ thể được xác nhận qua kiểm tra DOM trực tiếp trong phiên: `email.value === ""` sau submit sai) |
| Tái hiện được? | Có |

### FAIL #5 — CRM_LOGIN_TC_039 · Truy cập trang đăng nhập qua HTTP thuần phải bị ép chuyển hướng sang HTTPS

| | |
|---|---|
| REQ ID | REQ-LOGIN-44 |
| Priority | High |
| Bước fail | Bước 3 |
| **Expected** | Thanh địa chỉ/response phải chuyển sang `https://` — request HTTP phải trả `301`/`308` kèm header `Location: https://...` |
| **Actual** | `curl -I http://crm.anhtester.com/admin/authentication` trả `HTTP/1.1 200 OK`, không có header `Location`, không có `Strict-Transport-Security` |
| Ghi chú | 🐞 Bug đã biết `AMB-LOGIN-20`, PO đã chốt 19-09-2026. FAIL là kết quả **đúng** theo tài liệu TC. Đo lại qua `curl` (độc lập trình duyệt) để tránh Chrome tự nâng cấp HTTPS gây PASS giả |
| Evidence | Header response đầy đủ đã ghi trực tiếp trong log lệnh `curl -D -` của phiên chạy — `200 OK`, thiếu `Location` và `Strict-Transport-Security` |
| Tái hiện được? | Có |

### FAIL #3 — CRM_LOGIN_TC_028 · Bỏ trống email rồi bấm Confirm phải báo thiếu trường bắt buộc

| | |
|---|---|
| REQ ID | REQ-LOGIN-25 |
| Priority | High |
| Bước fail | Bước 4 |
| **Expected** | Nội dung dải báo lỗi là `The Email Address field is required.` |
| **Actual** | Nội dung dải báo lỗi là `Email not found` |
| Ghi chú | 🐞 Bug đã biết `AMB-LOGIN-05`, PO đã xác nhận là lỗi thiếu validate trường bắt buộc. FAIL là kết quả **đúng** theo tài liệu TC — không sửa TC |
| Evidence | Xác nhận qua DOM trực tiếp trong phiên: `.alert` = "Email not found" sau khi submit email rỗng |
| Tái hiện được? | Có |

### FAIL #4 — CRM_LOGIN_TC_029 · Nhập email không tồn tại trong hệ thống thì báo không tìm thấy

| | |
|---|---|
| REQ ID | REQ-LOGIN-26 |
| Priority | High |
| Bước fail | Bước 5 |
| **Expected** | Sau khi trang nạp lại, ô Email trở về **rỗng** |
| **Actual** | Ô Email **vẫn giữ nguyên** giá trị `notexist_20260820@auto.test` đã nhập — server trả về HTML với `value="notexist_20260820@auto.test"` trong thẻ `input#email` |
| Ghi chú | Nội dung dải báo lỗi (`Email not found`) đúng như Expected mục 4 — chỉ riêng hành vi ô Email không rỗng là sai. Đây là phát hiện **mới**, chưa có trong danh sách "Ba TC nhiều khả năng FAIL" của tài liệu TC, chưa có mã AMB. Đề xuất mở bug report và chốt AMB mới với PO |
| Evidence | evidence/CRM_LOGIN_TC_029_email_field_reference.png (trang tham chiếu — giá trị `value` thực tế đã xác nhận qua kiểm tra DOM trực tiếp trong phiên) |
| Tái hiện được? | Có |

## 4. TC BLOCKED

| TC ID | Nguyên nhân chặn | Cần gì để chạy được |
|---|---|---|
| CRM_LOGIN_TC_040 | Browser pane (`mcp__Claude_Browser__*`) không có tool ngắt/khôi phục kết nối mạng của trình duyệt | Playwright MCP (`context.setOffline()`) hoặc DevTools Network throttling thật, chạy tay có giám sát |
| CRM_LOGIN_TC_041 | Browser pane không có tool giả lập tốc độ mạng (Slow 3G) | Playwright MCP hoặc Chrome DevTools Protocol có quyền `Network.emulateNetworkConditions`, chạy tay có giám sát |
| CRM_LOGIN_TC_044 | Hộp thoại "Lưu mật khẩu?" là UI riêng của trình duyệt Chrome, không nằm trong DOM trang — `read_page`/`javascript_tool`/`computer screenshot` của Browser pane chỉ thấy nội dung trang | Chạy tay trên Chrome thật (không phải Browser pane), hoặc công cụ automation có quyền truy cập Chrome UI layer (VD Playwright với CDP phù hợp) |
| CRM_LOGIN_TC_050 (biến thể `b`, `c`) | Browser pane chỉ chạy một engine Chromium duy nhất, không chọn được Microsoft Edge hay Mozilla Firefox | Máy có cài sẵn Edge/Firefox, chạy tay hoặc dùng Playwright MCP với `browserName` tương ứng |

## 5. Dữ liệu đã tạo & dọn dẹp

| Dữ liệu | ID | Nơi tạo | Đã xoá? |
|---|---|---|---|
| Task `Auto_LOGIN_TC034_1790251618467` (dùng cho TC_034) | 2074 | /admin/tasks | ✅ Đã dừng timer và xoá task — xác nhận `GET /admin/tasks/view/2074` trả về danh sách Tasks (task không còn tồn tại) |

## 6. Đề xuất bước tiếp theo

**5 TC FAIL — 3 bug đã biết + 1 phát hiện mới + 1 xác nhận lại bug đã biết:**

| TC | Mức độ ưu tiên xử lý | Ghi chú |
|---|---|---|
| CRM_LOGIN_TC_029 | 🆕 **Cao — cần xử lý trước** | Phát hiện mới, chưa có mã AMB. Đề xuất `/create-bug-report` cho TC này, đồng thời báo PO để chốt AMB mới (ô Email ở Quên mật khẩu không rỗng lại sau khi báo "Email not found") |
| CRM_LOGIN_TC_016 | Đã biết (`AMB-LOGIN-10`) | Có thể `/create-bug-report` nếu team chưa có bug ticket chính thức |
| CRM_LOGIN_TC_028 | Đã biết (`AMB-LOGIN-05`) | Tương tự — kiểm tra đã có bug ticket chưa |
| CRM_LOGIN_TC_039 | Đã biết (`AMB-LOGIN-20`) | Tài liệu TC ghi "Chưa có bug report" — ưu tiên `/create-bug-report` cho TC này vì ảnh hưởng bảo mật (HTTP không ép HTTPS) |
| CRM_LOGIN_TC_004 | 🆕 Phát hiện mới, mức Low | Hành vi hệ thống (không có trang chủ công khai) — nên xác nhận với PO đây là hành vi mong đợi hay cần sửa TC theo hiện trạng qua `/review-testcases` |

**4 TC BLOCKED — cần môi trường/công cụ khác để chạy hết:**

| TC | Cần gì |
|---|---|
| CRM_LOGIN_TC_040, TC_041 | Công cụ có khả năng giả lập offline/throttle mạng (Playwright MCP hoặc DevTools Protocol) |
| CRM_LOGIN_TC_044 | Chạy tay trên Chrome thật để quan sát hộp thoại lưu mật khẩu (UI trình duyệt, ngoài tầm với của Browser pane) |
| CRM_LOGIN_TC_050 (biến thể b, c) | Máy có cài Edge và Firefox, hoặc Playwright MCP chỉ định `browserName` |

**Bước tiếp theo đề xuất:**
1. `/create-bug-report` cho CRM_LOGIN_TC_029 (ưu tiên cao nhất — phát hiện mới) và CRM_LOGIN_TC_039 (ảnh hưởng bảo mật, chưa có bug report theo tài liệu)
2. Đối chiếu CRM_LOGIN_TC_016, CRM_LOGIN_TC_028 với hệ thống theo dõi bug hiện có — mở bug report nếu chưa có
3. Xác nhận với PO về hành vi CRM_LOGIN_TC_004 (logo dẫn tới trang đăng nhập cổng khách hàng thay vì "trang chủ công khai")
4. Với 4 TC BLOCKED — cân nhắc chuyển sang chạy tay có giám sát (TC_040, TC_041, TC_044) hoặc thiết lập Playwright MCP cho lần chạy sau (đã có ghi chú cấu hình trong `.claude/rules/playwright_rules.md`)
5. Dữ liệu test đã tạo trong buổi chạy (task TC_034) đã dọn sạch — không có việc tồn đọng
