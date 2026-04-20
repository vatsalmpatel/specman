---
description: Generate a terse Conventional Commits message for staged changes.
---

## User Input

```text
$ARGUMENTS
```

## Instructions

Generate a commit message for the current staged changes.

Steps:
1. Run `git diff --staged` to read what is staged. If nothing is staged, say `Nothing staged.` and stop.
2. Analyse the diff to determine the primary change type and scope.
3. Write the commit message following the rules below.
4. Output the message only — no explanation, no git commands.

### Commit message rules

**Subject line** (required):
- Format: `<type>(<scope>): <description>`
- Scope is optional; omit parentheses when not used.
- Imperative mood: "add", "fix", "remove", "update" — not "added" or "adds".
- ≤72 characters. No period at end.
- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`.

**Body** (optional):
- Add only when the *why* is not obvious from the subject.
- Wrap at 72 chars. Separate from subject with a blank line.
- No filler, no restating what the diff already shows.

**Breaking changes**: append `!` after type/scope and add `BREAKING CHANGE:` footer.

Output format — message text only, no markdown fences:

```
<type>(<scope>): <description>

<optional body>
```
