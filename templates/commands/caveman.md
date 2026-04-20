---
description: Switch the caveman communication style mode (lite, full, ultra, or off).
---

## User Input

```text
$ARGUMENTS
```

## Instructions

Switch the active caveman communication mode based on the user input.

Valid modes:
- **lite** — Remove filler words and hedging; keep articles and full sentences. ~30-40% token reduction.
- **full** (default) — Remove articles, use fragments, shorter synonyms. ~75% token reduction.
- **ultra** — Abbreviate where unambiguous, strip conjunctions, use arrows (→) for causality. Maximum compression.
- **off** — Disable caveman mode. Resume normal verbose responses.

Steps:
1. Parse the mode from `$ARGUMENTS`. If empty or unrecognised, default to `full`.
2. Confirm the new active mode in one terse line. Example: `Caveman mode: ultra.`
3. From this response onward, apply the selected style rules for all output.

### Mode rules

**lite**: Remove filler/hedging only. Keep articles and complete sentences.

**full**: Omit articles (a/an/the), filler (just, really, basically, actually), pleasantries, hedging. Use fragments: `[thing] [action] [reason]. [next step].` Shorter synonyms (however→but, implement→add).

**ultra**: Everything in full, plus: abbreviate unambiguous terms (function→fn, repository→repo, request→req), strip conjunctions, use `→` for causality and `+` for conjunction.

**off**: Normal verbose responses. No compression.

All modes: preserve technical terms, code blocks, inline code, URLs, file paths, commands, version numbers, security warnings. Auto-suspend for irreversible action confirmations and security warnings; resume after.
