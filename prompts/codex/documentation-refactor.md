# Documentation Refactor

Prompt dùng khi cần tổ chức lại tài liệu mà không làm thay đổi nội dung kỹ thuật hiện có.

## Modification modes

- `STRUCTURE-ONLY` — chỉ tổ chức lại; không đổi content.
- `CONTENT UPDATE` — được cập nhật kiến thức/content.
- `FULL REWRITE` — được thay cả structure + content.

Prompt dưới đây dành cho `STRUCTURE-ONLY`.

## Prompt

```text
You are reorganizing existing documentation.

INPUT
- DOC_PATH: {{doc_path}}
- TARGET_STYLE: Progressive Disclosure — scan first, drill down later.

GOAL
Reorganize the documentation so it is easier to scan and navigate.

CRITICAL RULE
This is a STRUCTURE-ONLY refactor.

The existing content is the source of truth.

DO NOT:
- change technical meaning;
- add new conclusions;
- remove existing information;
- correct or reinterpret existing content;
- summarize away details;
- rewrite content just to make it sound better;
- change confirmed/unknown status;
- merge separate facts into a new conclusion.

ALLOWED:
- move content between sections/files;
- split long sections into detail files;
- create indexes and links;
- convert suitable content into bullets/checklists/tables;
- shorten root-level text ONLY by moving the original detail into a child file;
- adjust headings and navigation;
- remove duplicated text only when the same information remains available in one canonical location.

STYLE
Follow Progressive Disclosure:

- Root/index docs = map/navigation only.
- Prefer 1 line per item; max 2 lines when necessary.
- Put implementation details, evidence, reasoning, ports, commands, paths, edge cases, etc. into detail files.
- Never delete detail just to make a parent document shorter.
- Detail must remain traceable from the parent document.

IMPORTANT
When moving content:
- preserve the original meaning and level of certainty;
- preserve important technical wording;
- preserve source references, paths, commands, values, and evidence;
- prefer moving existing text over rewriting it.

If restructuring would require changing meaning, leave that content unchanged.

OUTPUT
1. Reorganize the documentation.
2. Do not modify source code.
3. At the end, report only:
   - files created;
   - files modified;
   - content moved;
   - any content intentionally left unchanged because restructuring could alter its meaning.

Final check:
- No information lost.
- No new information introduced.
- No technical meaning changed.
- Parent docs are easy to scan.
- All removed parent-level detail still exists in a traceable child document.
```
