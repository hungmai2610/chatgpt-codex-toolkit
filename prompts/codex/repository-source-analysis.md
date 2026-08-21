# Repository Source Analysis

Prompt dùng khi cần đọc một repo hiện có, xác định feature và tạo bộ tài liệu theo cấu trúc root → architecture → feature details.

## Input

- `REPO_DIR`: folder source.
- `REPO_DESCRIPTION`: mô tả ngắn repo + phần cần quan tâm.
- `OUTPUT_DIR`: optional, mặc định `docs/source-analysis`.

## Prompt

```text
You are analyzing an existing source-code repository to understand what the system currently does.

INPUT
- REPO_DIR: {{repo_dir}}
- REPO_DESCRIPTION: {{repo_description}}
- OUTPUT_DIR: {{output_dir | default: docs/source-analysis}}

GOAL
Read the source code and build a concise, evidence-based documentation set describing:
- what the repository does;
- what features currently exist;
- how the main components and flows work;
- where each feature is implemented.

RULES
- Read-only analysis. Do not modify production source code.
- All generated documentation must be written in Vietnamese.
- Keep technical terms, code identifiers, file paths, API names, class/function names, config keys, and commands in their original form when clearer than translating them.
- Derive behavior from actual code, not assumptions.
- Use REPO_DESCRIPTION only as context; verify it against the source.
- Focus on meaningful product/system features, not individual functions/files.
- Ignore generated, build, dependency, vendor, and cache directories unless relevant.
- Cite important source paths, classes/functions, entry points, configs, or commands.
- Clearly mark anything uncertain as `Unknown` or `Needs verification`.
- Keep documentation concise. Do not repeat the same explanation across files.
- Create detail files only for meaningful features.

OUTPUT

{{output_dir}}/
├── README.md
├── ARCHITECTURE.md
└── features/
    ├── README.md
    ├── <feature-1>.md
    ├── <feature-2>.md
    └── ...

### README.md

Include only:
- Purpose
- High-level system summary
- Main entry points
- Feature map
- Links to ARCHITECTURE.md and feature documents
- Important unknowns / areas not fully verified

### ARCHITECTURE.md

Describe:
- Major components/modules
- Responsibility of each component
- Important dependencies between them
- Main data/control flow
- External systems/interfaces
- Persistent state/storage if any

Prefer a compact Mermaid diagram when useful.

### features/README.md

For each feature:
- Name
- One-line purpose
- Main implementation area
- Status: Confirmed / Partial / Unclear
- Link to detail file

### features/<feature>.md

# <Feature name>

## Purpose
What the feature does.

## Trigger / Input
How the feature starts and what data it receives.

## Flow
Short step-by-step runtime flow.

## Implementation
Important files, classes, functions, tasks, services, configs, or commands.

## State / Output
What it changes, stores, publishes, returns, or exposes.

## Dependencies
Relevant internal/external dependencies.

## Edge cases / Constraints
Only important behavior discovered in source.

## Verification
- Confirmed behavior
- Unknown / Needs verification

QUALITY BAR
A developer unfamiliar with the repository should be able to:
1. open README.md and understand the system quickly;
2. find all major features;
3. open one feature file and trace its implementation in source;
4. distinguish verified behavior from assumptions.

Before finishing:
- re-check the feature list against the repository;
- remove duplicated information;
- ensure every important claim can be traced to source code;
- report the generated files and any areas that could not be confidently analyzed.
```
