---
description: One-line-per-finding code review of a file or diff.
---

## User Input

```text
$ARGUMENTS
```

## Instructions

Review the code specified in `$ARGUMENTS` (file path, glob, or "staged"). If empty, review the current staged diff.

Steps:
1. Read the target code/diff.
2. Identify all findings. Each finding gets exactly one line.
3. Output findings using the format below, then stop.
4. If no issues found, output `LGTM` and stop.

### Finding format

```
L<line>: <severity> <problem>. <fix>.
```

Severity markers:
- `bug:` — incorrect behaviour, will cause failures.
- `risk:` — potential issue under certain conditions.
- `nit:` — style, naming, minor inconsistency.
- `q:` — question about intent or approach.

Rules:
- Reference exact line numbers and symbol names.
- Explain "why" only when reasoning isn't obvious from the code.
- No praise ("Great work!", "Nice job").
- No restating what the code does — only what is wrong and how to fix it.
- Exception: security vulnerabilities and architectural disputes warrant a full paragraph.

Example output:
```
L42: bug: null check missing before dereferencing user.id. Add `if (!user) return`.
L87: risk: regex not anchored — can match substrings. Use `^...$`.
L103: nit: `getUserData` → `fetchUser` matches repo naming convention.
```
