# AI Manual Testing – Khóa học Kiểm thử thủ công với sự hỗ trợ của AI

Repo lưu trữ tài liệu và sản phẩm thực hành của khóa học **Manual Testing** có ứng dụng AI để hỗ trợ khảo sát hệ thống, viết đặc tả và thiết kế test case.

## Hệ thống được kiểm thử (SUT)

| Mục | Giá trị |
|-----|---------|
| Ứng dụng | Perfex CRM – Anh Tester Demo |
| URL | https://crm.anhtester.com/admin/authentication |
| Tài khoản demo | `admin@example.com` / `123456` |
| Module đang tập trung | Login / Authentication |

## Cấu trúc thư mục

```
ai-manual-testing/
├── README.md                       # Tài liệu này
├── LICENSE                         # Giấy phép MIT
├── .gitignore
├── Reqs/                           # Yêu cầu gốc (không đưa lên git – chỉ dùng cục bộ)
│   └── Requirement_Login.md
└── docs/
    ├── SRS_Login_Module.md         # Đặc tả yêu cầu phần mềm (SRS) cho module Login
    └── Test cases/
        ├── TC_Login.txt            # Test case cho màn hình Login
        └── TC_Dasboard.txt         # Test case cho màn hình Dashboard
```

> Thư mục `Reqs/` và file `.env` được liệt kê trong `.gitignore` nên không được commit.

## Nội dung chính

### `docs/SRS_Login_Module.md`
Đặc tả yêu cầu phần mềm cho module Login của Perfex CRM demo, được xây dựng từ kết quả khảo sát trực tiếp trên giao diện. Bao gồm:

- Giới thiệu, phạm vi, thuật ngữ, tài liệu tham chiếu
- Mô tả tổng quan: tác nhân, cấu trúc màn hình, ràng buộc & giả định
- Yêu cầu chức năng `FR-01` … `FR-08` (hiển thị, xác thực, "Remember me", validate đầu vào, xử lý lỗi, Forgot Password, CSRF, đăng xuất)
- Yêu cầu phi chức năng `NFR-01` … `NFR-05` (bảo mật, hiệu năng, khả dụng, tương thích, độ tin cậy)
- Quy tắc nghiệp vụ, đặc tả trường dữ liệu, bảng thông báo hệ thống
- Use case `UC-01` … `UC-05`
- 14 tiêu chí chấp nhận (`AC-01` … `AC-14`)
- Danh sách vấn đề cần làm rõ + Phụ lục ghi chú khảo sát thực tế

Các thông báo đã kiểm chứng trên hệ thống:

| Ngữ cảnh | Thông báo |
|----------|-----------|
| Email trống | `The Email Address field is required.` |
| Password trống | `The Password field is required.` |
| Sai email / mật khẩu | `Invalid email or password` |
| Forgot Password – email không tồn tại | `Email not found` |

### `docs/Test cases/`
Bộ test case dạng văn bản cho từng màn hình. Hiện là bản nháp, sẽ được hoàn thiện dựa trên SRS.

## Quy ước làm việc

- Tài liệu viết bằng tiếng Việt, định dạng Markdown.
- Mỗi module có một file SRS trong `docs/` và một file test case tương ứng trong `docs/Test cases/`.
- Yêu cầu chức năng đánh mã `FR-xx`, phi chức năng `NFR-xx`, use case `UC-xx`, tiêu chí chấp nhận `AC-xx` để truy vết.
- Những điểm chưa kiểm chứng được đánh dấu *cần xác nhận khi nghiệm thu* trong tài liệu.

## Giấy phép

Mã nguồn và tài liệu trong repo được phát hành theo giấy phép [MIT](LICENSE).
