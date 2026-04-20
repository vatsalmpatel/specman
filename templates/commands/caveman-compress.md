---
description: Compress a Markdown or text context file to reduce input token count.
---

## User Input

```text
$ARGUMENTS
```

## Instructions

Compress the file at the path given in `$ARGUMENTS` to reduce input token usage.

Steps:
1. **Validate target**: The file must have extension `.md`, `.txt`, or no extension. Never modify code files (`.py`, `.js`, `.ts`, `.sh`, etc.). If the target fails this check, say `Skipped: only .md/.txt/extensionless files allowed.` and stop.
2. **Backup**: Copy the original file to `<filepath>.original.md` before making any changes. Confirm: `Backup: <filepath>.original.md`
3. **Compress**: Rewrite the file content applying caveman compression rules:
   - Remove articles (a/an/the), filler words (just, really, basically, actually, simply, very, quite), pleasantries, and hedging.
   - Convert verbose prose to fragments and bullet points.
   - Collapse redundant explanations into one line.
   - Use shorter synonyms (however→but, in order to→to, additional→more).
   - **Preserve exactly**: code blocks (``` fences), inline code (` backtick`), URLs, file paths, shell commands, version numbers, dates, proper nouns, technical terms.
   - Never remove headings, never alter semantic meaning.
4. **Report**: After writing the compressed file, estimate and report token savings:
   ```
   Compressed: <filepath>
   Original: ~<N> tokens | Compressed: ~<M> tokens | Saved: ~<pct>%
   ```
   Estimate tokens as `characters / 4` (rough approximation).

Do not print the compressed content — only write it to disk and report the summary.
