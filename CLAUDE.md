# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spec Kit is a Python CLI toolkit (`specify`) for Spec-Driven Development (SDD) — a workflow where AI coding agents follow structured specifications before writing code. It ships coordinated prompt templates, scripts, and integrations for 32+ AI coding agents (Claude, Copilot, Cursor, Gemini, etc.).

## Commands

```bash
# Install dependencies (including test extras)
uv sync --extra test

# Install CLI from working tree
uv pip install -e .

# Run all tests
uv run pytest

# Run specific test suite
uv run python -m pytest tests/test_agent_config_consistency.py -q

# Lint
uvx ruff check src/

# Verify CLI works
uv run specify --help
```

Run `test_agent_config_consistency.py` whenever you change agent metadata, context update scripts, or integration wiring.

## Architecture

### Source: `src/specify_cli/`

- **`__init__.py`** — CLI entry point (~5000 lines). Defines all Typer commands: `specify init`, `specify check`, `specify version`, `specify extension`, `specify preset`. Handles TOML/Markdown command format generation for each agent.
- **`agents.py`** — Metadata and wiring for all 32+ supported AI agents.
- **`extensions.py`** — Extension system for custom commands and workflows.
- **`presets.py`** — Preset configurations for different project types.
- **`integrations/`** — One subdirectory per AI agent (claude, copilot, cursor, gemini, etc.), each with its own assets and integration logic.

### Templates and scripts

- **`templates/commands/`** — Markdown prompt templates that define slash commands. All `.md` files here are auto-discovered and installed as skills — no registration required.
- **`templates/*-template.md`** — Core workflow artifacts (spec, plan, tasks, checklist, constitution).
- **`scripts/bash/`** and **`scripts/powershell/`** — Cross-platform scripts invoked by the workflow commands. `common.sh` is sourced by several other scripts — changes to it affect multiple commands.
- **`workflows/speckit/`** — Core workflow definitions.

### Extensions and presets

- **`extensions/`** — Bundled extensions (git, selftest, template) plus catalog.
- **`presets/`** — Bundled presets (lean, scaffold, self-test) plus catalog.

### Caveman integration (Claude Code)

Caveman token-compression is bundled natively. Key files:

- **`src/specify_cli/integrations/claude/__init__.py`** — `ClaudeIntegration._build_context_section()` overrides the base class to append caveman "full" mode rules into the `<!-- SPECKIT START/END -->` block in `CLAUDE.md`. `ARGUMENT_HINTS` includes entries for all four caveman commands.
- **`templates/commands/caveman.md`** — Mode switcher skill (lite/full/ultra/off).
- **`templates/commands/caveman-commit.md`** — Conventional Commits generator from staged diff.
- **`templates/commands/caveman-review.md`** — One-line-per-finding code review.
- **`templates/commands/caveman-compress.md`** — Context file compressor with `.original.md` backup.

The four caveman templates are auto-discovered by `list_command_templates()` and installed alongside all other skills. The always-on rules are injected by `upsert_context_section()` which is called during `specify init` and on plan updates.

## Manual testing

Changes to slash commands require manual testing through an AI agent. To set up a test project:

```bash
uv run specify init <temp-dir>/speckit-test --ai <agent> --offline
cd <temp-dir>/speckit-test
# Open in your agent and run affected commands
```

Command dependency order: `/speckit.specify` → `/speckit.plan` → `/speckit.tasks`.

When changing a script in `scripts/`, check `templates/commands/` to find which commands invoke it (including transitive `source` dependencies).

## Key docs

- `DEVELOPMENT.md` — Repo orientation and component map.
- `CONTRIBUTING.md` — Full testing and PR process, including the manual test reporting template.
- `spec-driven.md` — End-to-end SDD workflow explanation.
- `.github/workflows/RELEASE-PROCESS.md` — Release and versioning rules.
