# Requirements — Module Login (Đăng nhập)

> ← [Danh mục requirements](../README.md) · Bản đồ khám phá: [`_discovery/modules/module_01_dang_nhap.md`](../_discovery/modules/module_01_dang_nhap.md)

| Mục | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — bản demo Anh Tester, phân hệ Admin |
| **Module · Prefix** | Login · `LOGIN` (cấp bởi `/discover-system`, 2026-09-14) |
| **Route** | `/admin/authentication` · `/admin/authentication/forgot_password` · `/admin/authentication/logout` |
| **Phiên bản tài liệu** | 1.0 — khởi tạo 2026-09-14 |
| **Nhánh phân tích** | UI Recon + tài liệu bán phần (mục 3.3.1) — mode HYBRID |
| **Dải mã đã dùng** | `REQ-LOGIN-01` → `REQ-LOGIN-49` · `AMB-01` → `AMB-23` · `RISK-01` → `RISK-09` · `STORY-LOGIN-01` → `STORY-LOGIN-07` |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-LOGIN-50` · `AMB-24` · `RISK-10` · `STORY-LOGIN-08` — **KHÔNG đánh lại từ 01** |
| **Trình duyệt khảo sát** | Browser pane của Claude desktop app (Chromium, UA `Chrome/152.0.7977.76`), viewport giả lập **`1600×750`**. Mọi AC dựa trên thông báo mặc định của trình duyệt **chỉ đúng với trình duyệt này**. ⚠️ Không dùng Playwright MCP (dự án chưa có `.mcp.json`) — lệch quy định `CLAUDE.md`, xem RISK-07 |
| **Tài khoản** | 1 tài khoản Admin trong `.env` — **user tự đăng nhập**, agent không nhập mật khẩu thật |
| **Môi trường dùng chung** | ✅ Có — chỉ submit dữ liệu giả (email `auto_login_20260914_<mô tả>@auto.test`, mật khẩu giả 16 ký tự), tổng 6 lần submit lỗi; không thử brute-force, không gửi email đặt lại thật |
| **Tổng REQ** | **49** (🟢 46 · ⚪ 3) — thuộc ngưỡng 25–80 → 1 file + mục Phân rã Epic/Story (6.8) |

---

## 1. Tổng quan

Module Login là cổng xác thực của phân hệ Admin. Người dùng nội bộ nhập Email + Password để tạo phiên làm việc. Mọi URL `/admin/*` khi chưa có phiên đều bị đưa về màn hình Login. Module còn có hai điểm giao tiếp: màn hình **Forgot Password** và hành động **Đăng xuất**.

**Trong phạm vi:**
- Màn hình Login: hiển thị, kiểm tra dữ liệu đầu vào, xác thực thành công / thất bại
- Ghi nhớ đăng nhập (Remember me)
- Bảo vệ khu vực `/admin/*` khi chưa đăng nhập; hành vi khi đã đăng nhập mở lại trang Login
- Đăng xuất — **kết quả** của hành động (phiên kết thúc, chuyển về Login)
- Màn hình Forgot Password — hiển thị và thông báo lỗi
- CSRF token trên form; giao thức truyền tải

**Ngoài phạm vi:**
- Quy trình đặt lại mật khẩu chi tiết (màn hình Reset Password) — chỉ ghi điểm giao tiếp REQ-LOGIN-42, 43
- **Vị trí và giao diện** nút Logout trong menu hồ sơ ở thanh đầu trang → module `HDR`. Module này chỉ ghi hành vi đăng xuất
- Đăng ký tài khoản, SSO, 2FA — không có trên UI
- Phân quyền sau khi đăng nhập (module Roles — `SETUP`, đang ⏸️ Hoãn)
- Client portal

## 2. Bản đồ phủ tài liệu (mục 6.5.1)

**Tài liệu có sẵn** (QA cung cấp, nằm ở `Reqs/` — ngoài `docs/`):

| Ký hiệu | Tệp | Mô tả |
|---|---|---|
| **SRS** | `Reqs/SRS_Login_Module.md` | SRS v1.0 · Draft · 2026-08-27 · QA Team. Có Phụ lục A khảo sát UI. Nhiều mục tự gắn nhãn *"cần xác nhận khi nghiệm thu"* |
| **PT** | `Reqs/Phan_tich_Requirement_Login_Module.md` | Phân tích từ SRS (2026-09-08) — 27 luồng + 17 câu hỏi Q1–Q17. Không phải nguồn sự thật mới, chỉ dùng làm danh sách câu hỏi |

| Vùng chức năng | Tài liệu phủ | Mức phủ | Chiến lược recon | REQ liên quan |
|---|---|---|---|---|
| Hiển thị form Login | SRS §2.3, FR-01, Phụ lục A | 🟩 Đầy đủ | Đối chiếu từng control bằng DOM | REQ-LOGIN-01 → 09 |
| Validation bắt buộc & định dạng | SRS FR-04, §6, §7 | 🟨 Một phần — FR-04.5 gắn *"cần xác nhận"* | Bổ khuyết: trigger từng validation, đọc response | REQ-LOGIN-10 → 17 |
| Xác thực thất bại / thành công | SRS FR-02, FR-05 | 🟨 Một phần — FR-02.3, 02.4, 05.4 *"cần xác nhận"*, Phụ lục A ghi FR-02 chưa kiểm chứng | Bổ khuyết: email giả + user tự đăng nhập | REQ-LOGIN-18 → 27 |
| Remember me | SRS FR-03 | 🟨 Một phần — không có thời hạn, chưa kiểm chứng | Không kiểm được trong Browser pane (cần đóng/mở trình duyệt) → giữ theo tài liệu + AMB | REQ-LOGIN-28 → 30 |
| Bảo vệ `/admin/*` & đăng xuất | SRS BR-07, FR-08, UC-05 | 🟨 Một phần — không nói gì về timer chặn đăng xuất | Recon đầy đủ | REQ-LOGIN-31 → 35 |
| Forgot Password | SRS FR-06 | 🟨 Một phần | Bổ khuyết: submit rỗng + email giả; **không** gửi email thật | REQ-LOGIN-36 → 43 |
| CSRF & HTTPS | SRS FR-07, NFR-01 | 🟨 Một phần — không kiểm được FR-07.2 bằng UI | Đối chiếu phần quan sát được; phần cần tự dựng request → AMB | REQ-LOGIN-44 → 49 |
| Phân quyền theo role | — | ⬜ Trắng | Chỉ có tài khoản Admin (mục 3.1.2) | Ma trận mục 6 |

**Quy tắc phân xử khi SRS lệch thực tế** (áp thống nhất cho toàn tài liệu):

| Loại mục trong SRS | REQ ghi theo | Lý do |
|---|---|---|
| Mục SRS **tự gắn** *"cần xác nhận"* / *"khuyến nghị"* | **Thực tế** + `AMB` 🔴 | Tác giả SRS đã coi đó là giả định |
| Mục SRS **khẳng định chắc chắn** | **SRS** — AC ghi rõ thực tế đang lệch + `AMB` 🔴 | TC sinh ra sẽ FAIL → lộ lỗi tiềm ẩn thay vì hợp thức hoá nó |

### 2.1. Đối chiếu SRS ↔ thực tế

| Mục SRS | SRS nói | Thực tế 2026-09-14 | Kết luận |
|---|---|---|---|
| §2.3, FR-01.1 | 8 thành phần màn hình Login | Khớp đủ (DOM) | ✅ Khớp → REQ-01 |
| FR-01.4 *(cần xác nhận)* | Đã đăng nhập mở lại Login → vào `/admin` | Chuyển về `/admin/` | ✅ Khớp → REQ-26 |
| FR-02.1 | Đăng nhập đúng → Dashboard `/admin` | `/admin/`, tiêu đề chứa `Dashboard` | ✅ Khớp → REQ-24 |
| FR-04.1 → 04.3 | 2 thông báo required | Khớp nguyên văn. Thứ tự khi trống cả hai: **Password trước, Email sau** | ✅ Khớp → REQ-10 → 12 |
| FR-04.4 | Trình duyệt cảnh báo + server từ chối sai định dạng | Trình duyệt chặn, không gửi request. Server: không kiểm được qua UI | ⚠️ Một nửa → REQ-14 ✅, REQ-15 → AMB-08 |
| FR-04.5 *(cần xác nhận)* | Giữ lại Email sau submit lỗi | **Không** giữ lại — HTML trả về không có thuộc tính `value` | ❌ Lệch → REQ-16, 20 theo thực tế · AMB-01 |
| FR-05.1 | `Invalid email or password` | Khớp (sau khi cắt khoảng trắng) | ✅ Khớp → REQ-18 |
| FR-06.3 | `Email not found` cho email không tồn tại / trống | Khớp cả hai trường hợp | ✅ Khớp → REQ-39, 40 |
| FR-07.1 | *"Mỗi lần tải form"* sinh CSRF token | Token **giữ nguyên** giữa các lần tải, giữa Login ↔ Forgot, và **cả sau khi đăng nhập** | ❌ Lệch → REQ-46 theo SRS · AMB-02 |
| FR-08.1 | Đăng xuất → về Login | Khớp — nhưng có timer đang chạy thì phải xác nhận qua popup (SRS không nói) | ✅ Khớp + bổ sung → REQ-32 → 35 |
| NFR-01, AC-13 | Toàn bộ giao tiếp qua HTTPS | Mở `http://` → trang Login tải bằng HTTP, **không** chuyển sang HTTPS. Form vẫn gửi tới `https://` | ❌ Lệch → REQ-49 theo SRS · REQ-48 thực tế · AMB-03 |

## 3. Yêu cầu chức năng

**Bảng mã trạng thái:** 🟢 Active · 🟡 Changed · 🔴 Deprecated · ⚪ Chưa implement.
> ⚪ trong tài liệu này dùng theo **mục 4.3.8** của skill: REQ về vế *"tác tạo dùng được"* chưa kiểm chứng được — **không** khẳng định hệ thống chưa build. TC viết trước, đánh dấu `skip` tới khi AMB tương ứng được trả lời.

**Quy ước viết AC:**
- `<base>` = giá trị `BASE_URL` trong `.env` (bỏ phần đường dẫn)
- Kích thước phần tử đo ở viewport `1600×750`
- `hiển thị được` = `offsetParent !== null` **và** kích thước > 0

### 3.1. STORY-LOGIN-01 — Hiển thị màn hình Login

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | Form Login hiển thị đủ thành phần khi truy cập ẩn danh | Người dùng chưa đăng nhập mở `/admin/authentication` thấy form đăng nhập | Trang có đúng 1 `form` `method=post`, `action=<base>/admin/authentication`, gồm các phần tử hiển thị được:<br>• `input#email` `type=email` — 336×34<br>• `input#password` `type=password` — 336×34<br>• `input#remember` `type=checkbox` — 13×13<br>• `button[type=submit]` text `Login` — 336×33<br>• `a[href$="/admin/authentication/forgot_password"]` text `Forgot Password?` — 119×17 | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS §2.3, FR-01.1 |
| REQ-LOGIN-02 | Trang Login mang tiêu đề "Login" | Tiêu đề hiển thị và tiêu đề tab nhận diện được màn hình | • `h1` duy nhất có text `Login`<br>• `document.title` **chứa** `Login`. Giá trị quan sát: `<tên công ty> - Login` — phần tên công ty lấy từ cấu hình, **không** assert khớp tuyệt đối (4.3.7) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-03 | Ô Password che ký tự khi nhập | Mật khẩu không hiển thị dạng rõ | `input#password` có `type="password"`. Đã gõ 16 ký tự giả — màn hình hiện ký tự che | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-01.2 |
| REQ-LOGIN-04 | Remember me mặc định không được tích | Lần tải trang đầu, checkbox ở trạng thái chưa chọn | Tải mới trang → `#remember.checked === false`, `disabled === false` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-01.3 |
| REQ-LOGIN-05 | Nhãn gắn đúng với ô nhập tương ứng | Hỗ trợ screen reader và bấm nhãn để focus | `#email.labels[0]` = `Email Address` · `#password.labels[0]` = `Password` · `#remember.labels[0]` = `Remember me` (so sánh sau khi cắt khoảng trắng) | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS NFR-03 |
| REQ-LOGIN-06 | Logo liên kết về trang chủ site | Logo phía trên form là liên kết | `a.logo` có `href=<base>/` · chứa `img` · hiển thị được, 448×90. Chưa bấm thử | 🟢 | — | UI thực tế · SRS §2.3 #1 |
| REQ-LOGIN-07 | Trang Login không có CAPTCHA khi tải lần đầu | Người dùng đăng nhập không cần giải CAPTCHA | (1) Mở tab chưa có phiên → (2) xác nhận chưa submit lần nào trong tab → (3) tải `/admin/authentication` → (4) số phần tử `.g-recaptcha, iframe[src*=recaptcha]` = 0. Chưa kiểm sau nhiều lần đăng nhập sai — xem AMB-06 | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS §2.4 |
| REQ-LOGIN-08 | Thứ tự phần tử tương tác theo DOM là Logo → Email → Password → Remember me → Login → Forgot Password | Làm căn cứ cho thứ tự Tab bằng bàn phím | Thứ tự DOM của `a, input:not([type=hidden]), button` là: `a.logo`, `#email`, `#password`, `#remember`, `button Login`, `a Forgot Password?`. Không phần tử nào có `tabIndex > 0`. ⚠️ Chưa bấm Tab thật để kiểm focus | 🟢 | — | UI thực tế · SRS NFR-03 |
| REQ-LOGIN-09 | Nhấn Enter trong ô Password gửi form đăng nhập | Thao tác được toàn bộ form bằng bàn phím | Nhập Email + Password, focus `#password`, nhấn Enter → phát sinh `POST /admin/authentication`. ⚠️ Chưa kiểm chứng — xem AMB-17 | 🟢 | — | Tài liệu · SRS NFR-03, AC-14 |

### 3.2. STORY-LOGIN-02 — Kiểm tra dữ liệu đầu vào

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-10 | Để trống Email thì báo lỗi bắt buộc của Email | Server kiểm tra Email bắt buộc | Email trống, Password = chuỗi giả → bấm Login → vẫn ở `/admin/authentication`, có đúng 1 `div.alert.alert-danger` text `The Email Address field is required.` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-04.1 · API · POST /admin/authentication · 200 |
| REQ-LOGIN-11 | Để trống Password thì báo lỗi bắt buộc của Password | Server kiểm tra Password bắt buộc | Email = `auto_login_<ts>_pwdempty@auto.test`, Password trống → bấm Login → vẫn ở `/admin/authentication`, có đúng 1 `div.alert.alert-danger` text `The Password field is required.` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-04.2 · API · POST /admin/authentication · 200 |
| REQ-LOGIN-12 | Để trống cả hai ô thì hiển thị cả hai thông báo | Hai lỗi bắt buộc hiện đồng thời | Cả hai trống → bấm Login → có đúng 2 `div.alert.alert-danger`, text là 2 thông báo của REQ-10 và REQ-11. Thứ tự quan sát: Password trước, Email sau — **không assert thứ tự** cho tới khi AMB-21 được trả lời | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-04.3 · API · POST /admin/authentication · 200 |
| REQ-LOGIN-13 | Thông báo lỗi validation nằm trong form, phía trên ô Email | Vị trí hiển thị lỗi — căn cứ cho locator | Mỗi `div.alert.alert-danger` là **con trực tiếp** của `form` và đứng **trước** khối `.form-group` chứa `#email` trong DOM | 🟢 | — | Kiểm chứng thực tế · SRS NFR-03 |
| REQ-LOGIN-14 | Email sai định dạng bị trình duyệt chặn, không gửi request | Ô `type=email` kiểm tra định dạng phía client | Nhập `abc` vào Email → bấm Login:<br>• `#email.validity.typeMismatch === true`, `checkValidity() === false`<br>• **không** phát sinh `POST /admin/authentication` mới<br>🚫 Chuỗi cảnh báo quan sát trên Chrome 152: `Please include an '@' in the email address. 'abc' is missing an '@'.` — chuỗi của trình duyệt, **CẤM dùng làm assertion** (4.3.6) | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-04.4 (vế 1) |
| REQ-LOGIN-15 | Server từ chối Email sai định dạng | Không phụ thuộc kiểm tra của trình duyệt | Gửi Email sai định dạng khi bỏ qua kiểm tra client → không tạo phiên, hiển thị lỗi. ⚠️ Không kiểm được qua UI vì trình duyệt chặn trước — xem AMB-08 | 🟢 | — | Tài liệu · SRS FR-04.4 (vế 2) |
| REQ-LOGIN-16 | Máy chủ không trả lại giá trị Email sau khi submit lỗi validation | Trang trả về sau lỗi bắt buộc không điền sẵn Email | (1) Tải mới trang → (2) xác nhận `#email.getAttribute('value') === null` → (3) nhập Email giả, để trống Password, bấm Login → (4) trên trang trả về `#email.getAttribute('value') === null`.<br>🚫 **Không** assert ô Email trống trên màn hình — trình duyệt có thể tự điền (4.3.1, RISK-09). ⚠️ Lệch SRS FR-04.5 — AMB-01 | 🟢 | — | Kiểm chứng thực tế · API · POST /admin/authentication · 200 |
| REQ-LOGIN-17 | Response sau submit lỗi không chứa mật khẩu đã nhập | Mật khẩu không bị trả ngược về trình duyệt | (1) Tải mới trang → (2) xác nhận `#password` không có thuộc tính `value` → (3) nhập Password giả, để trống Email, bấm Login → (4) body HTML của response `POST /admin/authentication`: `#password` không có thuộc tính `value` **và** không chứa chuỗi mật khẩu đã nhập | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-04.5, NFR-01 · API · POST /admin/authentication · 200 |

### 3.3. STORY-LOGIN-03 — Xác thực đăng nhập

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-18 | Email không tồn tại thì báo lỗi chung và ở lại trang Login | Không tạo phiên với thông tin sai | Email `auto_login_<ts>_notexist@auto.test` + mật khẩu giả → bấm Login:<br>• URL cuối là `/admin/authentication`, form Login hiển thị<br>• có đúng 1 `div.alert.alert-danger` với `textContent.trim() === "Invalid email or password"` (không có dấu chấm cuối; text gốc có khoảng trắng bao quanh)<br>• mở `/admin/` → bị đưa về `/admin/authentication` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-05.1, FR-05.3 |
| REQ-LOGIN-19 | Email tồn tại nhưng sai mật khẩu thì báo cùng thông báo chung | Chống dò tài khoản (user enumeration) | Email của tài khoản có thật + mật khẩu sai → thông báo **giống hệt** REQ-18, không có dấu hiệu nào phân biệt với trường hợp email không tồn tại. ⚠️ Chưa kiểm chứng — AMB-04 | 🟢 | — | Tài liệu · SRS FR-05.2, BR-03 |
| REQ-LOGIN-20 | Máy chủ không trả lại giá trị Email sau khi đăng nhập sai | Trang Login hiển thị lại sau xác thực thất bại không điền sẵn Email | (1) Tải mới trang → (2) xác nhận `#email.getAttribute('value') === null` → (3) thực hiện REQ-18 → (4) trên trang hiển thị lỗi, `#email.getAttribute('value') === null`. 🚫 Không assert ô trống trên màn hình (RISK-09). ⚠️ Lệch SRS FR-04.5 — AMB-01 | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-21 | Tài khoản bị vô hiệu hoá không đăng nhập được | Chỉ tài khoản active mới vào được hệ thống | Email + mật khẩu đúng của tài khoản inactive → không tạo phiên, ở lại Login. Nội dung thông báo chưa xác định — AMB-04 | 🟢 | — | Tài liệu · SRS FR-05.4, BR-01 |
| REQ-LOGIN-22 | Email không phân biệt chữ hoa chữ thường | So khớp email case-insensitive | Nhập email tài khoản thật đổi hoa/thường (không thêm khoảng trắng) + mật khẩu đúng → đăng nhập thành công. ⚠️ Chưa kiểm chứng — AMB-04 | 🟢 | — | Tài liệu · SRS FR-02.3 |
| REQ-LOGIN-23 | Khoảng trắng đầu/cuối Email được bỏ qua | Email được cắt khoảng trắng trước khi so khớp | Nhập email tài khoản thật **đúng hoa/thường**, thêm khoảng trắng đầu và cuối + mật khẩu đúng → đăng nhập thành công. ⚠️ Chưa kiểm chứng. Lưu ý: ô `type=email` của trình duyệt tự cắt khoảng trắng trước khi gửi — kết quả qua UI **không** chứng minh được server có cắt — AMB-04 | 🟢 | — | Tài liệu · SRS FR-02.4 |
| REQ-LOGIN-24 | Đăng nhập hợp lệ thì vào Dashboard | Tạo phiên và chuyển vào khu vực quản trị | Tài khoản Admin hợp lệ, **không** tích Remember me → bấm Login → URL `/admin/`, `document.title` **chứa** `Dashboard`, không còn `#password` trên trang. Kiểm chứng 1 lần do user tự đăng nhập. Xem AMB-16 về việc không quay lại URL gốc | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-02.1, UC-01 |
| REQ-LOGIN-25 | Đã đăng nhập thì mở được trang quản trị khác mà không phải đăng nhập lại | Phiên dùng được cho mọi trang `/admin/*` | Sau REQ-24, mở `/admin/clients` → URL giữ nguyên `/admin/clients`, `document.title` **chứa** `Customers`, không có `#password` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-02.2 |
| REQ-LOGIN-26 | Đã đăng nhập mà mở lại trang Login thì được chuyển vào Dashboard | Không hiển thị lại form Login cho người đã có phiên | Đang có phiên → mở `/admin/authentication` → URL cuối `/admin/`, không có `#password` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-01.4 |
| REQ-LOGIN-27 | Cookie của phiên đăng nhập không đọc được từ JavaScript | Cookie phiên được bảo vệ khỏi script trên trang | Đang có phiên (xác nhận bằng REQ-25) → `document.cookie === ""`. Cờ `Secure` không đọc được bằng công cụ khảo sát — xem RISK-04 | 🟢 | — | Kiểm chứng thực tế · SRS NFR-01 |

### 3.4. STORY-LOGIN-04 — Ghi nhớ đăng nhập (Remember me)

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-28 | Tích Remember me khi đăng nhập thì hệ thống cấp cookie ghi nhớ | Vế *"tác tạo được tạo ra"* (4.3.8) | (1) Xoá sạch cookie của site → (2) xác nhận không còn cookie → (3) tích Remember me, đăng nhập hợp lệ → (4) có cookie ghi nhớ mới với thời hạn cố định (không phải cookie phiên). Tên cookie và thời hạn chưa biết — AMB-07. ⚠️ Chưa kiểm chứng | 🟢 | — | Tài liệu · SRS FR-03.1, NFR-01 |
| REQ-LOGIN-29 | Cookie ghi nhớ khôi phục phiên sau khi đóng trình duyệt | Vế *"tác tạo dùng được"* (4.3.8) | Sau REQ-28, đóng hẳn trình duyệt, mở lại, vào `/admin/` → vào thẳng Dashboard, không qua Login. Không kiểm được trong Browser pane — AMB-07 | ⚪ | 2026-09-14 · UI recon | Tài liệu · SRS FR-03.1, AC-08 |
| REQ-LOGIN-30 | Không tích Remember me thì phiên kết thúc khi đóng trình duyệt | Phiên thường chỉ sống trong session trình duyệt | Đăng nhập **không** tích Remember me → đóng hẳn trình duyệt, mở lại, vào `/admin/` → bị đưa về `/admin/authentication`. ⚠️ Chưa kiểm chứng — AMB-07 | 🟢 | — | Tài liệu · SRS FR-03.2, AC-08 |

### 3.5. STORY-LOGIN-05 — Bảo vệ khu vực admin & đăng xuất

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-31 | Chưa đăng nhập mà mở trang quản trị thì bị đưa về Login | Mọi `/admin/*` yêu cầu phiên | Không có phiên → mở `/admin/clients` → URL cuối `/admin/authentication`, có `#password`, `h1` = `Login`, không có nội dung trang Customers. Kiểm chứng 2 lần: trước khi đăng nhập và sau khi đăng xuất. Mã chuyển hướng **không** ghi nhận được — không assert mã | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS BR-07, UC-05, AC-12 |
| REQ-LOGIN-32 | Có timer công việc đang chạy thì bấm Logout hiện popup cảnh báo, chưa đăng xuất | Nhắc người dùng dừng timer trước khi rời hệ thống | Điều kiện: `.started-timers-top li.timer` ≥ 1 (quan sát: 1).<br>Mở menu hồ sơ → bấm mục Logout **hiển thị được** (`li.header-user-profile ul.dropdown-menu > li.header-logout > a`, 160×32 — dùng `>` vì menu có 5 `li` trực tiếp nhưng 32 `li` lồng) → sau 3 giây:<br>• URL **không đổi**<br>• popup `.popup-content` có liên kết `Logout` hiển thị được (70×33), `href=<base>/admin/authentication/logout`<br>Nội dung chữ lấy từ template `#timers-logout-template-warning`: `Started tasks timers found!` · `Are you sure you want to logout without stopping the timers?` — **chưa đo được hiển thị**, không assert text | 🟢 | — | Kiểm chứng thực tế · UI thực tế · mã JS `logout()` |
| REQ-LOGIN-33 | Xác nhận Logout trong popup thì về trang Login | Kết thúc phiên khi người dùng xác nhận | Từ trạng thái REQ-32, bấm `Logout` trong popup → URL cuối `/admin/authentication`, có `#password`, không có `div.alert` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-08.1 |
| REQ-LOGIN-34 | Không có timer đang chạy thì bấm Logout đăng xuất ngay | Không hiện popup khi không có timer | Điều kiện: `.started-timers-top li.timer` = 0 → bấm Logout → chuyển thẳng tới `/admin/authentication/logout` rồi về Login, không hiện popup. ⚠️ Chỉ suy từ mã JS `logout()` đọc trên trang; tài khoản dùng chung đang có timer nên chưa kiểm chứng — AMB-18 | 🟢 | — | UI thực tế · mã JS `logout()` (chưa kiểm chứng) |
| REQ-LOGIN-35 | Sau khi đăng xuất, mở trang quản trị thì bị đưa về Login | Phiên cũ không còn hiệu lực | Ngay sau REQ-33 → mở `/admin/` rồi `/admin/clients` → cả hai đều về `/admin/authentication`, có `#password` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-08.1 |

### 3.6. STORY-LOGIN-06 — Forgot Password (điểm giao tiếp)

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-36 | Link Forgot Password? mở màn hình Forgot Password | Điều hướng từ Login | Bấm `Forgot Password?` → URL `/admin/authentication/forgot_password`, `h1` = `Forgot Password` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-06.1, AC-09 |
| REQ-LOGIN-37 | Mở thẳng URL Forgot Password khi chưa đăng nhập thì thấy form | Màn hình cho phép truy cập ẩn danh | Không có phiên → mở `/admin/authentication/forgot_password` → URL giữ nguyên, `h1` = `Forgot Password`, không bị đưa về Login | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS §2.2 |
| REQ-LOGIN-38 | Màn hình Forgot Password hiển thị đủ thành phần | Form yêu cầu gửi link đặt lại | Có đúng 1 `form` `method=post`, `action=<base>/admin/authentication/forgot_password`, gồm:<br>• `input#email` `type=email`, nhãn `Email Address` — 336×34<br>• `button[type=submit]` text `Confirm` — 336×33<br>• `a.logo` `href=<base>/`<br>Không có `required`/`maxlength` trên ô Email | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-06.2 |
| REQ-LOGIN-39 | Forgot Password với email không tồn tại báo "Email not found" | Không gửi email cho địa chỉ lạ | Nhập `auto_login_<ts>_forgot@auto.test` → bấm Confirm → URL giữ nguyên, có đúng 1 `div.alert.alert-danger` text `Email not found` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-06.3, AC-10 · API · POST /admin/authentication/forgot_password · 200 |
| REQ-LOGIN-40 | Forgot Password để trống email cũng báo "Email not found" | Không có thông báo bắt buộc riêng | Email trống → bấm Confirm → có đúng 1 `div.alert.alert-danger` text `Email not found` (không phải thông báo `... field is required.`) — AMB-11 | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-06.3 · API · POST /admin/authentication/forgot_password · 200 |
| REQ-LOGIN-41 | Sau khi báo "Email not found", máy chủ trả lại email đã nhập | Người dùng sửa được email mà không phải gõ lại | Sau REQ-39 → `#email.getAttribute('value')` **bằng đúng** chuỗi vừa nhập. Ngược với trang Login (REQ-16) — AMB-12 | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-42 | Forgot Password với email tồn tại thì gửi link đặt lại và báo xác nhận | Vế *"tác tạo được tạo ra"* (4.3.8) | Nhập email của tài khoản có thật → bấm Confirm → hiển thị thông báo xác nhận (nội dung chưa biết) và gửi email chứa link đặt lại. ⚠️ Không thử — sẽ gửi email thật trên môi trường dùng chung — AMB-20 | 🟢 | — | Tài liệu · SRS FR-06.4, BR-06 |
| REQ-LOGIN-43 | Link đặt lại mật khẩu trong email mở được màn hình đặt lại | Vế *"tác tạo dùng được"* (4.3.8). Chi tiết thuộc module Reset Password | Mở link trong email của REQ-42 → hiển thị màn hình đặt lại mật khẩu. Chưa kiểm chứng — AMB-20 | ⚪ | 2026-09-14 · UI recon | Tài liệu · SRS FR-06.4 |

### 3.7. STORY-LOGIN-07 — CSRF & giao thức truyền tải

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-44 | Form Login chứa CSRF token ẩn | Mọi lần tải form có token | Form Login có đúng 1 `input[type=hidden][name=csrf_token_name]`, giá trị có hình thái `<32 ký tự hex thường>` (`/^[0-9a-f]{32}$/`). 🔒 Không chép giá trị vào TC | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-07.1, §2.3 #8 |
| REQ-LOGIN-45 | Form Forgot Password chứa CSRF token ẩn | Như REQ-44 cho màn hình Forgot | Form Forgot Password có đúng 1 `input[type=hidden][name=csrf_token_name]`, hình thái `<32 ký tự hex thường>` | 🟢 | — | Tài liệu + kiểm chứng thực tế · SRS FR-06.2, FR-07.1 |
| REQ-LOGIN-46 | Mỗi lần tải form, CSRF token được sinh mới | Token không dùng lại giữa các lần tải | Tải trang Login 2 lần liên tiếp trong cùng phiên trình duyệt → token lần 2 **khác** lần 1.<br>⚠️ **Thực tế 2026-09-14 LỆCH:** token giống nhau sau khi tải lại · giống nhau giữa Login ↔ Forgot Password · và **trùng** token dùng sau khi đăng nhập (thấy trong query string request của Dashboard). TC theo REQ này dự kiến FAIL — AMB-02 | 🟢 | — | Tài liệu · SRS FR-07.1 — ⚠️ thực tế lệch (AMB-02) |
| REQ-LOGIN-47 | POST thiếu hoặc sai CSRF token bị từ chối | Vế *"token có tác dụng"* (4.3.8) | `POST /admin/authentication` không kèm token hợp lệ → không xác thực, không tạo phiên. Không kiểm được qua UI — phải tự dựng request, bị cấm khi recon — AMB-09 | ⚪ | 2026-09-14 · UI recon | Tài liệu · SRS FR-07.2, AC-11 |
| REQ-LOGIN-48 | Form Login gửi dữ liệu tới địa chỉ HTTPS kể cả khi trang mở bằng HTTP | Thông tin đăng nhập không đi qua kênh không mã hoá | Mở `http://<host>/admin/authentication` → `form.getAttribute('action')` bắt đầu bằng `https://`. Link `Forgot Password?` và logo cũng là `https://` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-49 | Truy cập trang Login bằng HTTP được chuyển sang HTTPS | Toàn bộ giao tiếp qua HTTPS | Mở `http://<host>/admin/authentication` → URL cuối có `location.protocol === "https:"`.<br>⚠️ **Thực tế 2026-09-14 LỆCH:** trang tải bằng `http:`, request `GET 200`, không chuyển hướng. TC theo REQ này dự kiến FAIL — AMB-03 | 🟢 | — | Tài liệu · SRS NFR-01, AC-13 — ⚠️ thực tế lệch (AMB-03) |

## 4. Đặc tả trường dữ liệu

### 4.1. Màn hình Login — `/admin/authentication`

| Field (Label) | Loại UI | Required | Ràng buộc (min/max/format/default) | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Email Address | `input#email` · `type=email` · `name=email` | Có — **chỉ ở server** (HTML không có `required`) | Không `maxlength`/`minlength`/`pattern` · định dạng email do trình duyệt kiểm · có thuộc tính `autofocus="1"` · server **không** trả lại giá trị sau lỗi | 01, 05, 10, 12, 14–16, 20, 22, 23 | Độ dài tối đa chưa quy định — AMB-22. Hành vi autofocus khi tải chưa đo được (pane ẩn lúc tải) |
| Password | `input#password` · `type=password` · `name=password` | Có — chỉ ở server | Không `maxlength` · không trả lại giá trị | 01, 03, 05, 11, 12, 17 | — |
| Remember me | `input#remember` · `type=checkbox` · `name=remember` | Không | Mặc định `checked=false` · `value="estimate"` | 01, 04, 05, 28–30 | `value` không khớp ý nghĩa field — AMB-15 |
| Login | `button[type=submit]` · class `btn btn-primary btn-block` | — | Text `Login` | 01, 09–12, 18, 24 | — |
| Forgot Password? | `a[href$="/admin/authentication/forgot_password"]` | — | — | 01, 36 | — |
| Logo | `a.logo` > `img` | — | `href=<base>/` · `alt` = tên công ty từ cấu hình | 06 | — |
| csrf_token_name | `input[type=hidden]` | Hệ thống | `<32 ký tự hex thường>` · không đổi giữa các lần tải (thực tế) | 44, 46, 47 | 🔒 Không ghi giá trị |

### 4.2. Màn hình Forgot Password — `/admin/authentication/forgot_password`

| Field (Label) | Loại UI | Required | Ràng buộc | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Email Address | `input#email` · `type=email` · `name=email` | Không có `required` — để trống vẫn gửi được | Không `maxlength` · **không** có `autofocus` (khác trang Login) · server **trả lại** giá trị sau lỗi | 38–41 | — |
| Confirm | `button[type=submit]` | — | Text `Confirm` | 38–40 | — |
| csrf_token_name | `input[type=hidden]` | Hệ thống | `<32 ký tự hex thường>` — trùng token trang Login trong cùng phiên | 45, 46 | 🔒 Không ghi giá trị |

> `body.className` trang Login chứa `login_admin`; trang Forgot chứa `forgot-password`. Cả hai chứa class Tailwind có thể đổi theo bản build — chỉ dùng phần định danh nếu cần.

## 5. Business Rules & Validation Messages

| REQ ID | Rule / Trigger | Thông báo mong đợi (nguyên văn) | Phần tử |
|---|---|---|---|
| REQ-LOGIN-10 | Login · Email trống | `The Email Address field is required.` | `div.alert.alert-danger` con trực tiếp của `form` |
| REQ-LOGIN-11 | Login · Password trống | `The Password field is required.` | như trên |
| REQ-LOGIN-12 | Login · cả hai trống | Hai thông báo trên; quan sát: Password đứng trước | 2 `div.alert.alert-danger` |
| REQ-LOGIN-14 | Login · Email sai định dạng (`abc`) | *(chuỗi của trình duyệt — CẤM assert)* Chrome 152: `Please include an '@' in the email address. 'abc' is missing an '@'.` | Tooltip của trình duyệt, không có trong DOM |
| REQ-LOGIN-18 | Login · Email không tồn tại / sai thông tin | `Invalid email or password` — **không** dấu chấm; `textContent` có khoảng trắng bao quanh → so sánh sau `trim()` | `div.text-center.alert.alert-danger` — thứ tự class khác lỗi validation, gợi ý là flash message hiển thị sau chuyển hướng |
| REQ-LOGIN-39 | Forgot · email không tồn tại | `Email not found` | `div.alert.alert-danger` |
| REQ-LOGIN-40 | Forgot · email trống | `Email not found` | `div.alert.alert-danger` |
| REQ-LOGIN-32 | Logout · có timer đang chạy | `Started tasks timers found!` · `Are you sure you want to logout without stopping the timers?` · nút `Logout` — *(từ template, chưa đo hiển thị chữ)* | popup `.popup-content` |
| REQ-LOGIN-42 | Forgot · email tồn tại | ❔ Chưa biết — AMB-20 | — |
| REQ-LOGIN-21 | Login · tài khoản inactive | ❔ Chưa biết — AMB-04 | — |

⚠️ Thông báo có thể đổi theo ngôn ngữ hệ thống (menu hồ sơ có 25 ngôn ngữ) — AMB-23, RISK-06.

## 6. Ma trận Phân quyền

Hệ thống chưa lộ danh sách role (màn hình Roles bị cấm — xem `SETUP`). Cột **Role khác** gộp mọi role chưa biết tên.

| Hành động | Ẩn danh (chưa đăng nhập) | Admin (tài khoản `.env`) | Role khác |
|---|---|---|---|
| Xem form Login | ✅ (REQ-01) | ❌ — bị chuyển về `/admin/` (REQ-26) | ❔ |
| Đăng nhập bằng thông tin hợp lệ | — | ✅ (REQ-24) | ❔ |
| Truy cập `/admin/*` | ❌ — bị đưa về Login (REQ-31) | ✅ `/admin/`, `/admin/clients` (REQ-25) | ❔ |
| Đăng xuất | — | ✅ (REQ-33) | ❔ |
| Mở màn hình Forgot Password | ✅ (REQ-37) | ❔ Chưa thử khi đang có phiên | ❔ |

```
Tổng 15 ô = Đã kiểm chứng 7 · Suy diễn 0 · Chưa rõ 6 · Không áp dụng 2
Ô "không áp dụng" là [Đăng nhập hợp lệ × Ẩn danh] — ẩn danh theo định nghĩa chưa có tài khoản để đăng nhập;
[Đăng xuất × Ẩn danh] — chưa có phiên để kết thúc.
Cột "Role khác": chưa có danh sách role và chưa có account (AMB-05).
Ô [Forgot Password × Admin]: có account nhưng chưa thử khi đang có phiên.
```

## 7. Ma trận Trạng thái

**Không áp dụng** — module không có entity mang status flow. Hai trạng thái phiên (ẩn danh ↔ đã đăng nhập) và các chuyển đổi được mô tả trực tiếp ở REQ-LOGIN-24 → 35 và luồng mục 9.

## 8. Điểm Mơ Hồ & Rủi Ro

### 8.1. Ambiguities

| Mã | Câu hỏi | Nguy cơ | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| AMB-01 | SRS FR-04.5 ghi *"giữ lại Email sau submit lỗi"*, thực tế Login **không** trả lại Email (cả lỗi validation lẫn sai thông tin). Hành vi nào đúng? | TC viết theo SRS sẽ FAIL; nếu dev "sửa" theo SRS thì TC theo thực tế FAIL | 🔴 | Theo thực tế (REQ-16, 20) — SRS tự gắn nhãn *"cần xác nhận"* | ❓ Chờ trả lời | — |
| AMB-02 | SRS FR-07.1 ghi token sinh mỗi lần tải form; thực tế token giữ nguyên suốt phiên trình duyệt và **không đổi sau khi đăng nhập**. Là cấu hình chủ đích hay lỗi? | Token không xoay qua ranh giới xác thực là rủi ro bảo mật (RISK-05) | 🔴 | Theo SRS (REQ-46) — TC dự kiến FAIL | ❓ Chờ trả lời | — |
| AMB-03 | SRS NFR-01 yêu cầu HTTPS toàn bộ; thực tế `http://` vẫn phục vụ trang Login, không chuyển hướng. Có bắt buộc redirect/HSTS không? | Trang HTTP có thể bị sửa trên đường truyền (RISK-04) | 🔴 | Theo SRS (REQ-49) — TC dự kiến FAIL | ❓ Chờ trả lời | — |
| AMB-04 | Xin **môi trường hoặc tài khoản test riêng** để kiểm: sai mật khẩu với email thật (REQ-19), tài khoản inactive + nội dung thông báo (REQ-21), hoa/thường (REQ-22), khoảng trắng phía server (REQ-23) | 4 REQ không kiểm chứng được; thử trên account dùng chung có thể gây khoá | 🔴 | Theo SRS; TC gắn nhãn `assumption-based`, chưa chạy trên môi trường dùng chung | ❓ Chờ trả lời | — |
| AMB-05 | Hệ thống có những role nào? Xin account từng role để kiểm ma trận mục 6 | 6 ô ❔ không kiểm được; không biết role nào bị chặn đăng nhập | 🔴 | Mọi staff active đăng nhập như Admin — **chưa** kiểm chứng | ❓ Chờ trả lời | — |
| AMB-06 | Có cơ chế chống brute-force / khoá tài khoản / CAPTCHA xuất hiện sau N lần sai không? Ngưỡng, thời gian khoá, tính theo tài khoản hay IP? | Không test được trên môi trường dùng chung; REQ-07 chỉ đúng cho lần tải đầu | 🟡 | Không có cơ chế (6 lần submit lỗi không thấy CAPTCHA) — **chưa** kết luận | ❓ Chờ trả lời | — |
| AMB-07 | Tên + thời hạn cookie Remember me? Thời hạn phiên thường, có idle-timeout không? | REQ-28 → 30 không viết được bước kiểm chính xác | 🟡 | Theo SRS FR-03 | ❓ Chờ trả lời | — |
| AMB-08 | Server có tự kiểm định dạng Email không (khi bỏ qua kiểm tra của trình duyệt)? | REQ-15 không kiểm được qua UI | 🟡 | Có kiểm (theo SRS) | ❓ Chờ trả lời | — |
| AMB-09 | POST thiếu/sai CSRF token phản hồi thế nào (trang lỗi, mã, thông báo)? | REQ-47 không kiểm được khi recon; cần dev xác nhận hoặc test ở môi trường riêng | 🟡 | Bị từ chối, không tạo phiên | ❓ Chờ trả lời | — |
| AMB-10 | Forgot Password báo `Email not found` làm lộ email nào tồn tại — mâu thuẫn nguyên tắc chống dò tài khoản của Login (REQ-18/19). Có chấp nhận không? | Rủi ro user enumeration | 🟡 | Chấp nhận như hiện tại (REQ-39) | ❓ Chờ trả lời | — |
| AMB-11 | Forgot Password để trống email báo `Email not found` thay vì thông báo bắt buộc. Chủ đích? | TC kỳ vọng sai thông báo | 🟢 | Theo thực tế (REQ-40) | ❓ Chờ trả lời | — |
| AMB-12 | Forgot Password trả lại email sau lỗi, Login thì không. Hai màn hình có cần thống nhất? | Trải nghiệm không nhất quán | 🟢 | Theo thực tế từng màn hình (REQ-16, 41) | ❓ Chờ trả lời | — |
| AMB-13 | Tab trình duyệt ở màn hình Forgot Password vẫn mang tiêu đề `... - Login`. Lỗi hay chủ đích? | Assert tiêu đề sai màn hình | 🟢 | Không assert tiêu đề ở Forgot Password | ❓ Chờ trả lời | — |
| AMB-14 | Màn hình Forgot Password **không có** liên kết quay lại Login (chỉ có logo về trang chủ site). Có cần không? | Người dùng phải dùng nút Back | 🟢 | Không có — ghi nhận, không sinh REQ | ❓ Chờ trả lời | — |
| AMB-15 | Checkbox Remember me có `value="estimate"`. Server chỉ kiểm có/không gửi, hay đọc giá trị? | Nếu server đọc giá trị thì Remember me có thể không hoạt động | 🟢 | Server chỉ kiểm có gửi field | ❓ Chờ trả lời | — |
| AMB-16 | Chưa đăng nhập mở `/admin/clients` → đăng nhập → vào `/admin/`, **không** quay lại `/admin/clients`. Chủ đích? (Quan sát 1 lần, điều kiện chưa sạch — giữa chừng có mở trang HTTP và đóng/mở lại pane) | TC về "quay lại URL gốc" có kỳ vọng sai | 🟢 | Luôn vào `/admin/` — không sinh REQ tới khi quan sát lại ở điều kiện sạch | ❓ Chờ trả lời | — |
| AMB-17 | Enter trong ô Password có gửi form không? Công cụ khảo sát gửi phím Enter tới trang (trang nhận `keydown` Enter trên `#password`) nhưng không phát sinh POST — chưa loại trừ được lỗi của công cụ | REQ-09 chưa kiểm chứng | 🟢 | Có gửi (hành vi chuẩn của form HTML có nút submit) — cần tester kiểm tay | ❓ Chờ trả lời | — |
| AMB-18 | Tài khoản dùng chung đang có timer chạy nên chưa thấy nhánh đăng xuất không có timer. Cần tài khoản không có timer để kiểm REQ-34 | REQ-34 chỉ dựa trên mã JS | 🟢 | Theo mã JS `logout()` | ❓ Chờ trả lời | — |
| AMB-19 | Đăng xuất qua popup **không** dừng timer. Timer có tiếp tục tính giờ sau khi đăng xuất không — có chấp nhận về nghiệp vụ không? | Timesheet sai nếu người dùng quên | 🟡 | Timer tiếp tục chạy — thuộc phạm vi `TASK`, không sinh REQ ở đây | ❓ Chờ trả lời | — |
| AMB-20 | Forgot Password với email tồn tại hiển thị thông báo gì? Link đặt lại hết hạn sau bao lâu? Không thử vì sẽ gửi email thật | REQ-42, 43 không kiểm chứng được | 🟡 | Có thông báo xác nhận (theo SRS) | ❓ Chờ trả lời | — |
| AMB-21 | Để trống cả hai ô: thông báo Password luôn đứng trước Email? Có cần assert thứ tự không? | Assert thứ tự dễ đỏ khi dev đổi thứ tự rule | 🟢 | Không assert thứ tự | ❓ Chờ trả lời | — |
| AMB-22 | Độ dài tối đa Email/Password? HTML không giới hạn | Không có biên để viết boundary test | 🟡 | Không giới hạn phía client; server chưa rõ | ❓ Chờ trả lời | — |
| AMB-23 | Thông báo lỗi Login có đổi theo ngôn ngữ hệ thống không (trang Login ẩn danh luôn `lang="en"`)? | Assert nguyên văn tiếng Anh sẽ đỏ nếu đổi ngôn ngữ mặc định | 🟡 | Luôn tiếng Anh trên trang ẩn danh | ❓ Chờ trả lời | — |

### 8.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| RISK-01 | Thiếu môi trường & tài khoản cho luồng phá huỷ | Chỉ 1 tài khoản Admin dùng chung; không thể test khoá tài khoản, inactive, sai mật khẩu với email thật | Xin môi trường riêng (AMB-04); tới khi có, TC tương ứng để `blocked` |
| RISK-02 | Test đăng nhập sai có thể ảnh hưởng người dùng khác | Nếu có cơ chế khoá theo IP/tài khoản, chạy suite trên môi trường dùng chung có thể khoá người khác | Chỉ dùng email giả không tồn tại; giới hạn số lần; không chạy song song các TC đăng nhập sai trên môi trường dùng chung |
| RISK-03 | Timer đang chạy chặn đăng xuất một bước | Trạng thái timer do người khác trên tài khoản dùng chung tạo ra → luồng Logout rẽ nhánh không đoán trước | TC/automation đăng xuất phải xử lý **cả hai** nhánh (có/không popup); không dừng timer của người khác |
| RISK-04 | Trang Login phục vụ qua HTTP | Form gửi tới HTTPS (REQ-48) nhưng trang HTTP có thể bị chèn mã trước khi người dùng nhập. Cờ `Secure` của cookie không đo được | Báo AMB-03; bổ sung kiểm tra header (HSTS, `Set-Cookie`) bằng công cụ có quyền đọc header ở môi trường riêng |
| RISK-05 | CSRF token không đổi qua ranh giới đăng nhập | Token lấy được lúc ẩn danh vẫn dùng được sau khi đăng nhập | Báo AMB-02; đưa vào phạm vi security test |
| RISK-06 | Assertion nguyên văn dễ đỏ | Thông báo có khoảng trắng bao quanh, class khác nhau giữa hai loại lỗi, có thể đổi theo ngôn ngữ | So sánh sau `trim()`; locator dựa `div.alert-danger` thay vì thứ tự class; theo dõi AMB-23 |
| RISK-07 | Evidence không có ảnh lưu đĩa; recon không dùng Playwright MCP | Browser pane không trả tệp ảnh; máy không có Node/Playwright. Bằng chứng là số liệu DOM ghi trong AC | Khi chạy `/execute-test-cases`, chụp lại ảnh vào `executions/login/run_*/evidence/`; bổ sung `.mcp.json` để lần recon sau đúng quy định |
| RISK-08 | Có 3 phần tử "Logout" trong DOM | Navbar mobile (ẩn ở desktop), dropdown hồ sơ (ẩn tới khi mở menu), link `/admin/authentication/logout` trong `div.hide` | Locator bắt buộc: `li.header-user-profile ul.dropdown-menu > li.header-logout > a`, kiểm hiển thị trước khi bấm |
| RISK-09 | Trình duyệt tự điền Email | Ô Email có thể hiện giá trị do autofill dù server không trả lại → TC assert "ô trống" sẽ đỏ ngẫu nhiên | TC chỉ assert thuộc tính `value` của HTML (REQ-16, 20); chạy automation với profile trình duyệt sạch |

## 9. Luồng xử lý (User Flows)

**UF-01 — Đăng nhập thành công** (REQ-01, 24, 25)
1. Mở `/admin/authentication` → form Login hiển thị
2. Nhập Email + Password hợp lệ, không tích Remember me
3. Bấm Login → vào `/admin/` (Dashboard)
4. Mở trang quản trị khác → không bị hỏi đăng nhập lại

**UF-02 — Submit thiếu dữ liệu** (REQ-10 → 13, 16, 17)
1. Bỏ trống Email và/hoặc Password → bấm Login
2. Server trả về trang Login với thông báo bắt buộc tương ứng, đặt trong form phía trên ô Email
3. Email và Password **không** được điền lại

**UF-03 — Sai thông tin đăng nhập** (REQ-18 → 20)
1. Nhập Email không tồn tại (hoặc sai mật khẩu) → bấm Login
2. Ở lại Login, hiện `Invalid email or password`; Email không được điền lại

**UF-04 — Quên mật khẩu** (REQ-36 → 43)
1. Bấm `Forgot Password?` → màn hình Forgot Password
2. Nhập email → bấm Confirm
3a. Email không tồn tại / trống → `Email not found`, email đã nhập được giữ lại
3b. Email tồn tại → thông báo xác nhận + email chứa link đặt lại *(chưa kiểm chứng)*
4. Không có liên kết quay lại Login trên màn hình này

**UF-05 — Đăng xuất** (REQ-32 → 35)
1. Mở menu hồ sơ ở thanh đầu trang → bấm Logout
2a. Có timer đang chạy → popup cảnh báo → bấm `Logout` trong popup → về Login
2b. Không có timer → về Login ngay *(suy từ mã JS, chưa kiểm chứng)*
3. Mở `/admin/*` → bị đưa về Login

**UF-06 — Truy cập trái phép** (REQ-26, 31)
- Chưa đăng nhập mở `/admin/*` → về Login
- Đã đăng nhập mở `/admin/authentication` → về `/admin/`

## 10. Yêu cầu phi chức năng

| Hạng mục | Yêu cầu (SRS) | Quan sát 2026-09-14 | REQ / AMB |
|---|---|---|---|
| Bảo mật — HTTPS | Toàn bộ qua HTTPS | ❌ HTTP không chuyển hướng; form vẫn gửi tới HTTPS | REQ-48, 49 · AMB-03 |
| Bảo mật — mật khẩu | Không hiển thị rõ, không có trong response | ✅ `type=password`; response lỗi không chứa mật khẩu | REQ-03, 17 |
| Bảo mật — cookie | `HttpOnly`, `Secure` | ✅ Không đọc được từ JS · `Secure` chưa đo được | REQ-27 · RISK-04 |
| Bảo mật — chống dò tài khoản | Thông báo chung | ✅ ở Login (email giả) · ❌ Forgot Password lộ email | REQ-18, 19, 39 · AMB-10 |
| Bảo mật — brute-force | *Khuyến nghị* | Chưa kiểm — môi trường dùng chung | AMB-06 |
| Hiệu năng | Tải trang ≤ 3 giây · phản hồi đăng nhập ≤ 2 giây | **Chưa đo** — không có công cụ đo thời gian chính xác trong phiên khảo sát | — |
| Khả dụng | Label đúng · thao tác bằng bàn phím · lỗi gần form | ✅ Label · ✅ lỗi trong form · ❔ Enter chưa kiểm chứng | REQ-05, 08, 09, 13 · AMB-17 |
| Tương thích | Chrome, Firefox, Edge, Safari · responsive mobile | Chỉ khảo sát Chromium 152 ở `1600×750`. Có navbar mobile riêng (ẩn ở desktop) — cần recon riêng ở viewport mobile | — |
| Độ tin cậy | Lỗi 5xx không lộ stack trace | Không quan sát được — không gặp lỗi 5xx | — |
| Ngôn ngữ | Mặc định English | ✅ `<html lang="en">` trên trang ẩn danh | AMB-23 |

## 11. Quan sát tầng network (mục 3.1.1)

> Chỉ quan sát thụ động request do UI phát sinh. Không gọi API trực tiếp.

| Request | Phát sinh khi | Kết quả quan sát | Ghi chú |
|---|---|---|---|
| `GET /admin/authentication` | Tải trang Login | `200` | — |
| `POST /admin/authentication` | Submit thiếu dữ liệu (3 lần) | `200` — server render lại trang kèm lỗi, **không** chuyển hướng | Body response không chứa giá trị Email/Password đã gửi (REQ-16, 17) |
| `POST /admin/authentication` | Submit email không tồn tại | Công cụ **không** ghi nhận dòng POST; ngay sau đó có `GET /admin/authentication 200` và thông báo dạng flash | Gợi ý mô hình POST → chuyển hướng → GET. **Không** assert mã — chưa loại trừ công cụ ghi thiếu (4.3.9) |
| `POST /admin/authentication` | User đăng nhập hợp lệ | Không ghi nhận dòng POST; sau đó `GET /admin/ 200` | Cùng hiện tượng với dòng trên |
| `GET /admin/clients` (ẩn danh) | Mở URL khi chưa đăng nhập | Kết thúc ở `GET /admin/authentication 200` | Mã chuyển hướng không ghi nhận được |
| `GET http://<host>/admin/authentication` | Mở bằng HTTP | `200 OK` — không chuyển hướng | REQ-49 · AMB-03 |
| `GET /admin/authentication/forgot_password` | Mở màn hình Forgot | `200` | — |
| `POST /admin/authentication/forgot_password` | Submit rỗng / email giả | `200` — render lại kèm lỗi | — |
| `GET /admin/utilities/get_calendar_data?csrf_token_name=<32 hex>&…` | Tải Dashboard sau đăng nhập | `200` | Token trong query string **trùng** token trang Login trước khi đăng nhập (AMB-02). Hiện tượng token trong URL đã ghi ở `system_map.md` mục 4.2 |

## 12. Danh mục Evidence

**Không có tệp ảnh.** Browser pane không trả tệp ảnh ra đĩa, máy không có Node/Playwright để chụp thay (RISK-07). Ảnh chụp chỉ dùng để quan sát trong phiên, không lưu lại. Thư mục `evidence/` vì vậy **không** được tạo.

Bằng chứng thay thế là **số liệu DOM và network chép trực tiếp vào Acceptance Criteria**, theo mục 7.2.1:

| Nhóm REQ có nguồn `Kiểm chứng thực tế` / `UI thực tế` | Số liệu chống lưng |
|---|---|
| 01 → 08, 13, 38 | Thuộc tính, nhãn, thứ tự DOM, kích thước + `offsetParent` ở `1600×750` — ghi trong AC |
| 10 → 12, 14, 16, 17, 18, 20, 39 → 41 | Nguyên văn `div.alert`, thuộc tính `value`, `validity`, body response `POST` — ghi trong AC + mục 5, 11 |
| 24 → 27, 31 → 35 | URL cuối, tiêu đề chứa, `document.cookie`, số timer, kích thước phần tử Logout/popup — ghi trong AC |
| 44 → 46, 48, 49 | Hình thái token, quan hệ giống/khác giữa các lần tải, `form.action`, `location.protocol` — ghi trong AC + mục 11 |

## 13. Phân rã Epic / Story (Backlog View)

**Epic:** `LOGIN` — Xác thực phân hệ Admin

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-LOGIN-01 | Hiển thị màn hình Login | REQ-LOGIN-01 → 09 | 9 | AMB-17 | Chỉ trạng thái tải trang, chưa submit |
| STORY-LOGIN-02 | Kiểm tra dữ liệu đầu vào | REQ-LOGIN-10 → 17 | 8 | AMB-01, 08, 21, 22, 23 · RISK-06, 09 | Validation client + server, không xác thực |
| STORY-LOGIN-03 | Xác thực đăng nhập | REQ-LOGIN-18 → 27 | 10 | AMB-04, 06, 16 · RISK-01, 02 | Thành công / thất bại, phiên |
| STORY-LOGIN-04 | Ghi nhớ đăng nhập | REQ-LOGIN-28 → 30 | 3 | AMB-07, 15 | Cần đóng/mở trình duyệt thật |
| STORY-LOGIN-05 | Bảo vệ khu vực admin & đăng xuất | REQ-LOGIN-31 → 35 | 5 | AMB-18, 19 · RISK-03, 08 | Giao diện menu hồ sơ thuộc `HDR` |
| STORY-LOGIN-06 | Forgot Password | REQ-LOGIN-36 → 43 | 8 | AMB-10, 11, 12, 13, 14, 20 | Không gồm màn hình Reset Password |
| STORY-LOGIN-07 | CSRF & giao thức truyền tải | REQ-LOGIN-44 → 49 | 6 | AMB-02, 03, 09 · RISK-04, 05 | Phần kiểm qua UI; phần tự dựng request để ngoài |

**Tổng: 7 Story / 49 REQ — mọi REQ thuộc đúng một Story, không mồ côi, không trùng.** `9 + 8 + 10 + 3 + 5 + 8 + 6 = 49 ✔`

**Đối chiếu AMB / RISK:**

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | AMB-01 → 04, 06 → 23 | Phân bổ ở bảng Story (22 mã) |
| AMB cấp Epic | AMB-05 | Ma trận Phân quyền (mục 6) |
| RISK thuộc Story | RISK-01 → 06, 08, 09 | Phân bổ ở bảng Story (8 mã) |
| RISK cấp Epic | RISK-07 | Công cụ & evidence của cả đợt khảo sát |

`AMB: 22 + 1 = 23 ✔ · RISK: 8 + 1 = 9 ✔`

**Hạng mục cấp Epic** (cố ý không gán Story):

| Hạng mục | Lý do |
|---|---|
| Ma trận Phân quyền (mục 6) + AMB-05 | Cắt ngang Story 01, 03, 05, 06 |
| Ma trận Trạng thái (mục 7) | Không áp dụng — ghi nhận cho đủ |
| Yêu cầu phi chức năng (mục 10) | Áp cho cả module |
| RISK-07 | Giới hạn công cụ của cả đợt recon, không thuộc chức năng nào |

**Thứ tự triển khai đề xuất** (theo phụ thuộc và rủi ro):

| Thứ tự | Story | Lý do | Trạng thái |
|---|---|---|---|
| 1 | STORY-LOGIN-01 | Nền cho mọi Story khác | Sẵn sàng (REQ-09 chờ AMB-17) |
| 2 | STORY-LOGIN-02 | Chạy được bằng dữ liệu giả, không cần tài khoản | Sẵn sàng |
| 3 | STORY-LOGIN-05 | Rủi ro cao (quyền truy cập); cần xử lý nhánh timer | Sẵn sàng (REQ-34 chờ AMB-18) |
| 4 | STORY-LOGIN-03 | Cần tài khoản thật; 4 REQ cần môi trường riêng | ⛔ **BLOCKED một phần** — REQ-19, 21, 22, 23 bởi AMB-04 |
| 5 | STORY-LOGIN-07 | Hai REQ dự kiến FAIL — cần PO/dev phân xử trước khi báo bug | ⛔ **BLOCKED một phần** — REQ-46 bởi AMB-02, REQ-49 bởi AMB-03, REQ-47 bởi AMB-09 |
| 6 | STORY-LOGIN-06 | Phần ẩn danh chạy được | ⛔ **BLOCKED một phần** — REQ-42, 43 bởi AMB-20 |
| 7 | STORY-LOGIN-04 | Cần trình duyệt thật đóng/mở được + thông số cookie | ⛔ **BLOCKED** — AMB-07 |

## 14. Nhật ký thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 2026-09-14 | UI recon | REQ-LOGIN-29, 43, 47 | ⚪ Chưa implement | Vế *"tác tạo dùng được"* của cookie ghi nhớ, link đặt lại mật khẩu, CSRF token chưa kiểm chứng được (mục 4.3.8) — chờ AMB-07, 20, 09 | — (viết TC trước, đánh dấu `skip`) |
| 2026-09-14 | UI recon + SRS v1.0 (`/generate-requirements-from-website`) | REQ-LOGIN-01 → 49 | 🟢 Thêm | Khởi tạo tài liệu: HYBRID — đối chiếu `Reqs/SRS_Login_Module.md` với UI thực tế; 7 Story, 23 AMB, 9 RISK | — (viết TC mới) |
