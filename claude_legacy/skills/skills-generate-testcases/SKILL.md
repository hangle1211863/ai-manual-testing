---
name: skills-generate-testcases
description: Sinh bộ Test Case Manual đầy đủ (Happy path, Negative case, Edge case) từ mô tả một chức năng. Dùng khi cần viết Test Case nhanh cho một chức năng cụ thể, có thể dùng ngay sau Skill skills-requirements-analyzer.

---

Đóng vai Senior Manual QA Engineer. Sinh bộ Test Case Manual cho chức năng sau: $ARGUMENTS

Nếu trong hội thoại đã có kết quả phân tích luồng (Happy/Alternate/Exception Path) từ Skill skills-requirements-analyzer, hãy dùng chính kết quả đó làm nền — không phân tích lại từ đầu.

Yêu cầu:
- Hãy đọc file rule .claude\rules\rule-generate-testcases.md để nắm rõ các quy định khi sinh Test Case.
## Áp dụng kỹ thuật sinh test cases:
- Khi sinh test cases, phải dựa vào các tài liệu: Requirement, Design, User story, Use Case
- Dựa vào kỹ thuật:
  - EQ
  - BVA
  - Boundary Value Analysis
  - Decision Table Testing
  - State Transition Testing
  - Error Guessing