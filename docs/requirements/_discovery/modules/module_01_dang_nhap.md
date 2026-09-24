# 01 — Đăng nhập

> ← [Về bản đồ hệ thống](../system_map.md) · Trạng thái recon: xem [danh mục](../../README.md)

## Login — `LOGIN`

| Mục | Ghi nhận | Nguồn |
|---|---|---|
| Tên trên UI | `Login` (tiêu đề trang: `Perfex CRM \| Anh Tester Demo - Login`) | UI thực tế |
| Route | `/admin/authentication` | UI thực tế |
| Loại màn hình | Form | UI thực tế |
| Thành phần quan sát được | `Email Address` · `Password` · checkbox `Remember me` · nút `Login` · link `Forgot Password?` | UI thực tế |
| CRUD | Không áp dụng | — |
| Status flow | Không quan sát được | — |
| Ước lượng độ lớn | 2 field + 1 checkbox + luồng quên mật khẩu · đăng xuất (menu hồ sơ ở header) | UI thực tế |
| Risk | 🔴 — cổng xác thực của mọi module, liên quan quyền truy cập | — |

### Network

Chưa ghi nhận — trang login chỉ được mở trước khi bật theo dõi network.

### Vùng chưa xác minh

- Luồng đăng nhập thành công / thất bại, thông báo lỗi — agent **không** nhập mật khẩu; user tự đăng nhập
- Trang `Forgot Password?` chưa mở
- Cơ chế `Remember me` (cookie ghi nhớ) chưa kiểm
- Khoá tài khoản sau nhiều lần sai, CAPTCHA — chưa quan sát được
- ⚠️ Recon cấp module cần phương án đăng nhập không để agent nhập mật khẩu (user tự đăng nhập, hoặc dùng phiên có sẵn)

### Evidence

Không có ảnh lưu ra đĩa — xem lý do ở [system_map.md mục 1](../system_map.md#1-bối-cảnh-khảo-sát).
