# ChatGPT Codex Toolkit

Bộ tài liệu và công cụ để dùng ChatGPT + Codex như một workflow thống nhất, ưu tiên chất lượng output và tiết kiệm token/quota.

## Nguyên tắc

- Tài liệu chính viết bằng tiếng Việt.
- Thuật ngữ kỹ thuật, tên file/folder và prompt có thể giữ English.
- Nội dung phải ngắn, không lặp ý, nhưng đủ để áp dụng ngay.
- **Progressive Disclosure — scan first, drill down later.** Root docs chỉ là map/index; detail nằm ở file con.
- Ưu tiên checklist, bullet, table, diagram và link thay vì paragraph dài.
- Mỗi bullet ở summary/index nên gói trong 1 dòng; tối đa 2 dòng nếu thật sự cần.
- Nếu một ý cần giải thích dài hơn, giữ ý ngắn ở level hiện tại và link xuống detail; không xóa detail cần thiết.
- ChatGPT tập trung vào reasoning, architecture, requirement và review.
- Codex tập trung vào inspect repo, implement, test và diff.
- Không để hai bên làm lại cùng một việc nếu không cần thiết.

## Nội dung dự kiến

```text
chatgpt-codex-toolkit/
├── docs/          # Concepts, workflows, best practices
├── prompts/       # Prompt cho ChatGPT, Codex và handoff
├── templates/     # Task Spec, Result Pack, review format
├── automation/    # Router và helper scripts
├── examples/      # Ví dụ thực tế
└── ROADMAP.md     # Hướng phát triển tiếp theo
```

Mục tiêu là chuẩn hóa manual workflow trước, sau đó mới tự động hóa những phần lặp lại và an toàn để automate.
