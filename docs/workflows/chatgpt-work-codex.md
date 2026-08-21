# ChatGPT Work + Codex

## Mục tiêu

Dùng ChatGPT và Codex song song nhưng tránh trùng work, giảm token/quota và vẫn giữ chất lượng output.

## Vai trò

**ChatGPT**
- Phân tích requirement.
- Thiết kế solution/architecture.
- Xác định scope và Acceptance Criteria.
- Review kết quả từ Codex.

**Codex**
- Inspect repository.
- Implement.
- Run tests.
- Inspect diff.
- Trả về Result Pack.

> Rule: **ChatGPT thinks; Codex executes. Không để hai bên làm lại cùng một việc nếu không cần.**

## Flow chuẩn

```text
User request
    ↓
ChatGPT: clarify + design
    ↓
Task Spec / Handoff Pack
    ↓
Codex: inspect / implement / test
    ↓
Result Pack
    ↓
ChatGPT: verify against requirements
    ↓
PASS → Done
FAIL → Small Fix Request → Codex
```

## Task routing

| Level | Dùng khi | Flow |
|---|---|---|
| L1 | Rename, log, typo, small bug, mechanical change | Codex → Done |
| L2 | Medium feature, vài file, ít quyết định architecture | ChatGPT → Codex |
| L3 | Architecture, security, concurrency, protocol, large refactor | ChatGPT → Codex inspect → ChatGPT → Codex → ChatGPT review |

## Handoff Pack

Chỉ truyền context ảnh hưởng trực tiếp đến task.

```text
TASK
<task>

GOAL
<desired outcome>

CURRENT BEHAVIOR
<relevant current state>

REQUIREMENTS
- ...

CONSTRAINTS
- ...

ACCEPTANCE
- ...

FIRST STEP
<inspect or implement>
```

Không copy toàn bộ conversation sang Codex.

## Codex modes

### Inspect only

```text
Inspect only. Do not modify code.

Determine:
- current flow
- relevant files/functions
- safest integration point
- risks
- proposed implementation plan
```

### Implement

```text
Implement the approved plan.

After implementation:
- run relevant tests
- inspect git diff
- report files changed
- report tests executed
- list remaining uncertainty

Do not modify unrelated code.
```

## Result Pack

Codex trả về output ngắn, có cấu trúc:

```text
STATUS: PASS | FAIL | PARTIAL

CHANGED
- ...

TESTED
- ...

BEHAVIOR
- ...

UNRESOLVED
- ...
```

Raw logs dài chỉ giữ khi cần debug; bình thường phải summarize.

## Context trong project

```text
.ai/
├── context.md       # Stable project context
├── current-task.md  # Current Task Spec
└── result.md        # Latest Result Pack
```

**TTL**
- Project context → giữ lâu dài.
- Architecture decisions → giữ lâu dài.
- Current feature → giữ tới khi Done.
- Debug logs → temporary.
- Raw Codex output → summarize rồi bỏ.

## Routing automation

Có thể tự động chấm task theo:
- repo knowledge needed;
- architecture decision;
- number of components;
- security/concurrency;
- requirement ambiguity;
- blast radius;
- expected code changes.

Ví dụ:

```text
score 0–2 → Codex directly
score 3–5 → ChatGPT → Codex
score 6+  → ChatGPT → Codex inspect → ChatGPT → Codex
```

Suggested modes:

```text
AI_ROUTER=off
AI_ROUTER=auto
AI_ROUTER=codex
AI_ROUTER=chatgpt
```

## Nguyên tắc cuối

- ChatGPT không code khi Codex có thể code tốt hơn.
- Codex không làm product/architecture reasoning nếu ChatGPT đã xử lý được.
- Không agent nào đọc context mà nó không cần.
- Mọi handoff phải ngắn, rõ, có thể verify.
