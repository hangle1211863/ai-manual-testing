# SRS – Module Login (Đăng nhập)

**Sản phẩm:** Perfex CRM – Anh Tester Demo
**URL màn hình:** https://crm.anhtester.com/admin/authentication
**Phiên bản tài liệu:** 1.0
**Ngày tạo:** 2026-08-27
**Người tạo:** QA Team
**Trạng thái:** Draft

---

## 1. Giới thiệu

### 1.1. Mục đích
Tài liệu đặc tả yêu cầu phần mềm (SRS) này mô tả các yêu cầu chức năng và phi chức năng cho **module Login** của hệ thống quản trị (admin) Perfex CRM. Tài liệu được dùng làm cơ sở để:
- Thiết kế và phát triển chức năng đăng nhập.
- Xây dựng test case, checklist kiểm thử.
- Nghiệm thu (UAT) chức năng.

### 1.2. Phạm vi
Module Login cho phép người dùng nội bộ (staff/admin) xác thực danh tính để truy cập khu vực quản trị `/admin`.

**Trong phạm vi:**
- Đăng nhập bằng Email + Mật khẩu.
- Tùy chọn "Remember me" (ghi nhớ đăng nhập).
- Điều hướng sang chức năng "Forgot Password".
- Kiểm tra ràng buộc dữ liệu đầu vào và thông báo lỗi.
- Bảo vệ CSRF cho form đăng nhập.

**Ngoài phạm vi:**
- Quy trình đặt lại mật khẩu chi tiết (thuộc module Forgot/Reset Password – chỉ mô tả điểm giao tiếp).
- Đăng ký tài khoản mới (hệ thống không cho tự đăng ký ở khu vực admin).
- Đăng nhập mạng xã hội / SSO / 2FA (không có trên màn hình hiện tại).
- Phân quyền sau đăng nhập (thuộc module Roles & Permissions).

### 1.3. Định nghĩa, từ viết tắt

| Thuật ngữ | Ý nghĩa |
|-----------|---------|
| SRS | Software Requirements Specification |
| CRM | Customer Relationship Management |
| CSRF | Cross-Site Request Forgery |
| Session | Phiên đăng nhập của người dùng |
| Staff/User | Người dùng nội bộ có tài khoản trong hệ thống |
| Actor | Tác nhân tương tác với hệ thống |

### 1.4. Tài liệu tham chiếu
- Màn hình Login: `GET /admin/authentication`
- Màn hình Forgot Password: `GET /admin/authentication/forgot_password`
- Endpoint submit đăng nhập: `POST /admin/authentication`

---

## 2. Mô tả tổng quan

### 2.1. Bối cảnh
Màn hình Login là điểm vào (entry point) của khu vực quản trị. Người dùng chưa xác thực khi truy cập bất kỳ URL `/admin/*` nào sẽ được chuyển hướng về màn hình này.

### 2.2. Tác nhân (Actors)

| Actor | Mô tả |
|-------|-------|
| Người dùng nội bộ (Staff/Admin) | Có tài khoản hợp lệ, cần đăng nhập để làm việc |
| Người dùng ẩn danh | Chưa đăng nhập, chỉ truy cập được màn hình Login / Forgot Password |

### 2.3. Cấu trúc màn hình Login

| # | Thành phần | Loại control | Bắt buộc | Ghi chú |
|---|------------|--------------|----------|---------|
| 1 | Logo hệ thống | Image / Link | – | Bấm vào điều hướng về trang chủ `https://crm.anhtester.com/` |
| 2 | Tiêu đề "Login" | Heading | – | |
| 3 | Email Address | Textbox (`type=email`, `name=email`, `id=email`) | Có | Nhập email tài khoản |
| 4 | Password | Textbox (`type=password`, `name=password`, `id=password`) | Có | Ký tự nhập bị che |
| 5 | Remember me | Checkbox (`name=remember`, `id=remember`) | Không | Mặc định không tích |
| 6 | Login | Button (`type=submit`) | – | Gửi form đăng nhập |
| 7 | Forgot Password? | Link | – | Điều hướng tới `/admin/authentication/forgot_password` |
| 8 | CSRF token | Hidden input (`name=csrf_token_name`) | – | Token chống CSRF, hệ thống tự sinh |

### 2.4. Ràng buộc & giả định
- Form submit bằng phương thức **HTTP POST** tới `/admin/authentication`.
- Việc kiểm tra ràng buộc bắt buộc (required) được thực hiện ở **phía server** (các trường không đặt thuộc tính `required` ở HTML; không có validate client-side chặn submit).
- Không có CAPTCHA / reCAPTCHA trên màn hình hiện tại.
- Ngôn ngữ giao diện mặc định: English (`<html lang="en">`).
- Kết nối phải chạy qua HTTPS.

---

## 3. Yêu cầu chức năng

### FR-01 – Hiển thị màn hình Login
- **FR-01.1:** Khi truy cập `GET /admin/authentication`, hệ thống hiển thị form đăng nhập với các thành phần ở mục 2.3.
- **FR-01.2:** Trường Password phải che ký tự khi nhập.
- **FR-01.3:** Checkbox "Remember me" mặc định ở trạng thái **không được chọn**.
- **FR-01.4:** Khi người dùng **đã đăng nhập** truy cập lại `/admin/authentication`, hệ thống chuyển hướng thẳng vào khu vực quản trị (`/admin`) — *cần xác nhận khi nghiệm thu*.

### FR-02 – Xác thực đăng nhập thành công
- **FR-02.1:** Khi người dùng nhập Email và Password hợp lệ, tương ứng với một tài khoản đang hoạt động, rồi bấm **Login**, hệ thống tạo session và chuyển hướng vào trang chủ khu vực quản trị (Dashboard `/admin`).
- **FR-02.2:** Sau khi đăng nhập thành công, người dùng có thể điều hướng tới các màn hình quản trị khác mà không phải đăng nhập lại (trong thời hạn session).
- **FR-02.3:** Email không phân biệt hoa/thường (case-insensitive) — *cần xác nhận khi nghiệm thu*.
- **FR-02.4:** Khoảng trắng đầu/cuối của Email được cắt bỏ (trim) trước khi so khớp — *cần xác nhận khi nghiệm thu*.

### FR-03 – Ghi nhớ đăng nhập (Remember me)
- **FR-03.1:** Nếu người dùng tích "Remember me" trước khi đăng nhập thành công, hệ thống duy trì phiên đăng nhập kể cả sau khi đóng trình duyệt, cho tới khi hết hạn cookie ghi nhớ hoặc người dùng đăng xuất.
- **FR-03.2:** Nếu **không** tích "Remember me", phiên chỉ tồn tại trong phạm vi session của trình duyệt.

### FR-04 – Kiểm tra dữ liệu đầu vào
- **FR-04.1:** Nếu **Email để trống** khi submit, hệ thống không đăng nhập và hiển thị lỗi: `The Email Address field is required.`
- **FR-04.2:** Nếu **Password để trống** khi submit, hệ thống không đăng nhập và hiển thị lỗi: `The Password field is required.`
- **FR-04.3:** Nếu **cả hai trường để trống**, hệ thống hiển thị đồng thời cả hai thông báo ở FR-04.1 và FR-04.2.
- **FR-04.4:** Trường Email là `type=email`; nếu trình duyệt bật kiểm tra định dạng, giá trị sai định dạng (ví dụ `abc`, `abc@`, `@domain.com`) sẽ bị trình duyệt cảnh báo. Server cũng phải từ chối giá trị sai định dạng email.
- **FR-04.5:** Sau khi submit lỗi, giá trị đã nhập ở ô Email nên được giữ lại; ô Password bị xóa trắng (yêu cầu bảo mật) — *cần xác nhận khi nghiệm thu*.

### FR-05 – Xác thực đăng nhập thất bại
- **FR-05.1:** Nếu Email không tồn tại **hoặc** Password sai, hệ thống hiển thị **một** thông báo chung: `Invalid email or password` và không tạo session.
- **FR-05.2:** Thông báo lỗi **không được** tiết lộ trường nào sai (không phân biệt "email không tồn tại" với "sai mật khẩu") nhằm tránh dò tài khoản (user enumeration).
- **FR-05.3:** Người dùng vẫn ở lại màn hình Login sau khi đăng nhập thất bại.
- **FR-05.4:** Với tài khoản bị vô hiệu hóa (inactive), hệ thống từ chối đăng nhập — *cần xác nhận thông báo cụ thể khi nghiệm thu*.

### FR-06 – Điều hướng Forgot Password
- **FR-06.1:** Bấm link "Forgot Password?" điều hướng tới `GET /admin/authentication/forgot_password`.
- **FR-06.2:** Màn hình Forgot Password gồm: ô **Email Address** (`type=email`) và nút **Confirm** (`type=submit`), cùng logo và CSRF token.
- **FR-06.3:** Nếu email nhập vào **không tồn tại** trong hệ thống (bao gồm cả trường hợp bỏ trống), hệ thống hiển thị lỗi: `Email not found`.
- **FR-06.4:** Nếu email tồn tại, hệ thống gửi liên kết đặt lại mật khẩu tới email đó và hiển thị thông báo xác nhận — *chi tiết thuộc module Reset Password*.

### FR-07 – Bảo vệ CSRF
- **FR-07.1:** Mỗi lần tải form Login/Forgot Password, hệ thống sinh một CSRF token và nhúng vào hidden input.
- **FR-07.2:** Request `POST` không kèm CSRF token hợp lệ phải bị từ chối.

### FR-08 – Đăng xuất (điểm giao tiếp)
- **FR-08.1:** Sau khi đăng xuất từ khu vực quản trị, người dùng được đưa về màn hình Login; truy cập lại `/admin/*` yêu cầu đăng nhập lại.

---

## 4. Yêu cầu phi chức năng

### NFR-01 – Bảo mật
- Toàn bộ giao tiếp qua HTTPS.
- Mật khẩu không hiển thị dạng plaintext trên UI, không log ra console/network response.
- Mật khẩu lưu ở dạng băm (hash) có salt phía server.
- Thông báo lỗi đăng nhập dùng chung, tránh user enumeration (xem FR-05.2).
- Có cơ chế chống brute-force (giới hạn số lần thử / khóa tạm thời / delay) — *khuyến nghị, cần xác nhận hệ thống hiện có hay không*.
- CSRF token bắt buộc cho request POST.
- Cookie session đặt cờ `HttpOnly`, `Secure`; cookie "Remember me" có thời hạn hợp lý và bị hủy khi đăng xuất.

### NFR-02 – Hiệu năng
- Màn hình Login tải xong trong ≤ 3 giây với đường truyền bình thường.
- Phản hồi kết quả đăng nhập trong ≤ 2 giây ở điều kiện tải thông thường.

### NFR-03 – Khả dụng (Usability)
- Thông báo lỗi hiển thị rõ ràng, gần khu vực form, dễ nhận biết.
- Có thể thao tác toàn bộ form bằng bàn phím (tab-order hợp lý, Enter để submit).
- Nhãn (label) gắn đúng với input tương ứng (hỗ trợ screen reader).

### NFR-04 – Tương thích
- Hoạt động đúng trên các trình duyệt phổ biến bản mới: Chrome, Firefox, Edge, Safari.
- Giao diện responsive trên desktop và mobile.

### NFR-05 – Độ tin cậy
- Khi server xử lý lỗi (5xx), hiển thị thông báo thân thiện, không lộ stack trace.

---

## 5. Quy tắc nghiệp vụ (Business Rules)

| ID | Quy tắc |
|----|---------|
| BR-01 | Chỉ tài khoản tồn tại và đang hoạt động (active) mới đăng nhập được. |
| BR-02 | Email + Password đều bắt buộc. |
| BR-03 | Thông báo đăng nhập sai luôn là thông báo chung `Invalid email or password`. |
| BR-04 | Không có chức năng tự đăng ký tài khoản ở khu vực admin. |
| BR-05 | "Remember me" chỉ ảnh hưởng thời hạn duy trì phiên, không ảnh hưởng quyền hạn. |
| BR-06 | Forgot Password chỉ gửi email đặt lại cho địa chỉ email có trong hệ thống. |
| BR-07 | Mọi truy cập `/admin/*` khi chưa xác thực đều bị chuyển về màn hình Login. |

---

## 6. Đặc tả trường dữ liệu

| Trường | Bắt buộc | Kiểu | Ràng buộc | Thông báo lỗi khi vi phạm |
|--------|----------|------|-----------|----------------------------|
| Email Address | Có | Chuỗi, định dạng email | Đúng định dạng `local@domain`; nên trim khoảng trắng | Trống: `The Email Address field is required.` / Sai định dạng: cảnh báo trình duyệt + server từ chối |
| Password | Có | Chuỗi | Không hiển thị plaintext | Trống: `The Password field is required.` |
| Remember me | Không | Boolean | Mặc định `false` | – |
| csrf_token_name | Hệ thống | Hidden | Phải khớp token phiên | Request bị từ chối nếu token sai/thiếu |

> Ghi chú: Độ dài tối đa của Email/Password chưa được quy định ở HTML (`maxlength` = không giới hạn). Cần chốt giới hạn khi triển khai (khuyến nghị Email ≤ 191 ký tự, Password theo chính sách mật khẩu).

---

## 7. Thông báo hệ thống (tổng hợp)

| Ngữ cảnh | Thông báo | Loại |
|----------|-----------|------|
| Email để trống | `The Email Address field is required.` | Lỗi validate |
| Password để trống | `The Password field is required.` | Lỗi validate |
| Sai email hoặc mật khẩu | `Invalid email or password` | Lỗi xác thực |
| Forgot Password – email không tồn tại/để trống | `Email not found` | Lỗi |
| Đăng nhập thành công | (không có message, chuyển hướng vào Dashboard) | – |

---

## 8. Luồng sử dụng (Use Cases)

### UC-01 – Đăng nhập thành công
**Tiền điều kiện:** Người dùng có tài khoản active; đang ở màn hình Login.
**Luồng chính:**
1. Người dùng nhập Email hợp lệ.
2. Người dùng nhập Password đúng.
3. (Tùy chọn) Tích "Remember me".
4. Bấm **Login**.
5. Hệ thống xác thực thành công, tạo session.
6. Hệ thống chuyển hướng vào Dashboard `/admin`.
**Hậu điều kiện:** Người dùng ở trạng thái đã đăng nhập.

### UC-02 – Đăng nhập thất bại do sai thông tin
**Luồng chính:**
1. Người dùng nhập Email và/hoặc Password không đúng.
2. Bấm **Login**.
3. Hệ thống hiển thị `Invalid email or password`.
4. Người dùng ở lại màn hình Login.

### UC-03 – Submit khi thiếu dữ liệu bắt buộc
**Luồng chính:**
1. Người dùng bỏ trống Email hoặc Password (hoặc cả hai).
2. Bấm **Login**.
3. Hệ thống hiển thị thông báo `... field is required.` tương ứng.
4. Không thực hiện xác thực.

### UC-04 – Quên mật khẩu
**Luồng chính:**
1. Tại màn hình Login, bấm "Forgot Password?".
2. Hệ thống mở màn hình Forgot Password.
3. Người dùng nhập Email và bấm **Confirm**.
4a. Nếu email tồn tại: hệ thống gửi link đặt lại mật khẩu + hiển thị xác nhận.
4b. Nếu email không tồn tại/để trống: hệ thống hiển thị `Email not found`.

### UC-05 – Truy cập trang admin khi chưa đăng nhập
**Luồng chính:**
1. Người dùng ẩn danh mở một URL `/admin/...`.
2. Hệ thống chuyển hướng về `/admin/authentication`.

---

## 9. Tiêu chí chấp nhận (Acceptance Criteria)

- [ ] AC-01: Truy cập `/admin/authentication` hiển thị đầy đủ các control ở mục 2.3.
- [ ] AC-02: Đăng nhập bằng tài khoản hợp lệ → vào Dashboard `/admin`.
- [ ] AC-03: Bỏ trống Email → hiển thị `The Email Address field is required.`
- [ ] AC-04: Bỏ trống Password → hiển thị `The Password field is required.`
- [ ] AC-05: Bỏ trống cả hai → hiển thị đồng thời cả hai thông báo.
- [ ] AC-06: Sai email/mật khẩu → hiển thị đúng một thông báo `Invalid email or password`, không lộ trường sai.
- [ ] AC-07: Password luôn hiển thị dạng che ký tự; không xuất hiện plaintext trong response.
- [ ] AC-08: "Remember me" giữ phiên sau khi đóng/mở lại trình duyệt; không tích thì phiên kết thúc theo session trình duyệt.
- [ ] AC-09: Link "Forgot Password?" mở đúng `/admin/authentication/forgot_password`.
- [ ] AC-10: Forgot Password với email không tồn tại → `Email not found`.
- [ ] AC-11: Form Login có CSRF token; POST thiếu/sai token bị từ chối.
- [ ] AC-12: Truy cập `/admin/*` khi chưa đăng nhập → chuyển hướng về màn hình Login.
- [ ] AC-13: Toàn bộ giao tiếp qua HTTPS.
- [ ] AC-14: Thao tác form bằng bàn phím và nhấn Enter để submit hoạt động đúng.

---

## 10. Vấn đề mở / Cần làm rõ (Open Issues)

| # | Nội dung cần xác nhận |
|---|------------------------|
| 1 | Hệ thống có cơ chế chống brute-force / khóa tài khoản sau N lần sai không? Ngưỡng bao nhiêu? |
| 2 | Thời hạn cụ thể của session thường và cookie "Remember me". |
| 3 | Thông báo khi đăng nhập bằng tài khoản bị vô hiệu hóa. |
| 4 | Email có được trim / so khớp không phân biệt hoa thường không? |
| 5 | Sau submit lỗi, ô Email có giữ lại giá trị đã nhập không? |
| 6 | Giới hạn độ dài tối đa cho Email và Password. |
| 7 | Có hỗ trợ đa ngôn ngữ cho thông báo lỗi không? |
| 8 | Hành vi khi người dùng đã đăng nhập mở lại `/admin/authentication`. |

---

## Phụ lục A – Ghi chú khảo sát thực tế (2026-08-27)

Quan sát trực tiếp trên `https://crm.anhtester.com/admin/authentication`:

- Form: `method=post`, `action=https://crm.anhtester.com/admin/authentication`.
- Các input: `email` (`type=email`), `password` (`type=password`), `remember` (`checkbox`), `csrf_token_name` (`hidden`). Không input nào đặt thuộc tính `required` ở HTML → validate bắt buộc chạy ở server.
- Không có reCAPTCHA / CAPTCHA.
- `<html lang="en">`.
- Submit rỗng → trả về: `The Password field is required.` và `The Email Address field is required.`
- Submit sai (email `wrong@example.com`, mật khẩu sai) → `Invalid email or password`.
- Màn hình Forgot Password: input `email` + nút `Confirm`; submit không có email hợp lệ → `Email not found`.
- Luồng đăng nhập thành công (FR-02) và hành vi "Remember me" (FR-03) **chưa được kiểm chứng bằng phiên đăng nhập thực tế** trong đợt khảo sát này; cần đội QA xác nhận khi thực thi test.
