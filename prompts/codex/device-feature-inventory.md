# Device Feature Inventory

Prompt dùng để quét source/context và tạo Excel inventory **đầy đủ nhưng ở mức capability của thiết bị**, không đi sâu thành implementation inventory.

## Input

- `SOURCE_DIR`: folder source, optional.
- `CONTEXT_DIR`: folder context/doc, optional.
- `OUTPUT_FILE`: file `.xlsx` output.
- Cần ít nhất một trong `SOURCE_DIR` hoặc `CONTEXT_DIR`.

## Prompt

```text
You are analyzing an existing device/software project to build a COMPLETE high-level feature inventory.

INPUT
- SOURCE_DIR: {{source_dir | optional}}
- CONTEXT_DIR: {{context_dir | optional}}
- OUTPUT_FILE: {{output_file | default: device-feature-inventory.xlsx}}

At least one of SOURCE_DIR or CONTEXT_DIR must be provided.

GOAL
Read all relevant source/context and create an Excel file listing ALL meaningful capabilities of the device.

Prefer listing an uncertain/minor feature over silently missing it.
The user will review and remove unnecessary items later.

GRANULARITY
Inventory at PRODUCT / DEVICE capability level.

Think:
"What can this device do?"

NOT:
"How is this implemented internally?"

Use only 2 levels:
Tính năng chính
└── Tính năng phụ (only when needed)

A sub-feature should exist only if it can reasonably be understood, tested, migrated, or reviewed independently.

Do NOT create separate rows for helper functions, individual GPIO operations, ISR/timer steps, parsing steps, low-level read/write calls, or individual source files.
Implementation details belong in `Mô tả` or `Bằng chứng`.

SOURCE OF TRUTH
Priority:
1. Source code
2. Hardware/configuration evidence
3. Context/documentation
4. Inference

Do not convert inference into fact.
If SOURCE_DIR and CONTEXT_DIR disagree, prefer actual implementation found in source and record the discrepancy in `Note`.

OUTPUT
Create an Excel workbook with sheet `Features` and columns:
1. STT
2. Tính năng chính
3. Tính năng phụ
4. Liên quan
5. Mô tả
6. Bằng chứng
7. Xác minh
8. Note

COLUMN RULES

### Tính năng chính
Large device capability. Keep the name short.
Examples: OTA, Đo điện, Relay, LED, Button, Network, Storage, Security.

### Tính năng phụ
Use only when the main feature genuinely contains separate meaningful behaviors. Do not over-split.

### Liên quan
Hardware/platform/component involved.
Examples: PIC, Linux, RTL8196E, ESP32-S3, EEPROM, W5500, Wi-Fi, Ethernet.
Multiple values are allowed.

### Mô tả
Short description of actual behavior.
Prefer checklist/bullet style, one idea per line. Avoid paragraphs.

### Bằng chứng
Enough information to trace deeper: file, function, task/thread, command, config, GPIO, script, protocol handler.
Keep concise.

### Xác minh
Use:
- Confirmed
- Partial
- Unknown

Confirmed = direct evidence exists.
Partial = feature exists but some behavior is unclear.
Unknown = context suggests it, but evidence is insufficient.

### Note
Only important notes: limitation, risk, legacy behavior, discrepancy, hardware dependency, needs real-device test, or needs server-side verification.
Do not duplicate `Mô tả`.

COMPLETENESS
Before finishing, perform a second repository/context scan specifically to find missed capabilities.

Check relevant areas such as:
- entry points
- command handlers
- tasks/services/daemons
- drivers / GPIO
- protocols / network
- storage
- OTA / boot / reset
- LED / button / buzzer
- factory / provisioning
- security
- watchdog / recovery
- scripts
- config / build flags
- legacy or alternate hardware paths

Do not omit a feature because it is small, legacy, factory-only, diagnostic, rarely used, or hardware-specific.

STYLE
Follow SCAN FIRST, DRILL DOWN LATER.
The spreadsheet is an inventory, not detailed documentation.

- Keep rows compact.
- Prefer one capability per row.
- Do not merge cells.
- Repeat `Tính năng chính` when multiple sub-features exist.
- Keep enough evidence to trace into source/context.

LANGUAGE
All human-readable spreadsheet content must be Vietnamese.
Keep technical identifiers in their original form: function names, file names, commands, protocols, config keys, and hardware names.

EXCEL FORMAT
`Features` sheet:
- freeze header
- enable filters
- wrap text
- top-align cells
- sensible column widths

`Lists` sheet:
- dropdown values for `Xác minh`
- reusable values for `Liên quan` when practical

FINAL CHECK
Verify:
- all major device capabilities are represented
- granularity is not implementation-level
- uncertain features are preserved as Partial/Unknown
- duplicates are consolidated
- every important row can be traced deeper

FINAL RESPONSE
Report only:
- output file path
- number of main features
- total number of rows
- Confirmed / Partial / Unknown counts
- important areas that could not be fully analyzed
```
