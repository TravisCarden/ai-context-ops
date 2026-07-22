# Working on ai-context-ops

Instructions for AI agents contributing to *this repo*. This file is also a
living example of the project's own principles: terse, structured, no waste.

## Principles
- **Prompt-primary.** [`setup.md`](setup.md) and [`diagnose.md`](diagnose.md) are the primary artifacts.
  Deterministic commands live inside prompts as referenced snippets — there is
  no `install.sh`.
- **Never duplicate an instruction** — duplication costs tokens on every run.
- **Wiring hierarchy:** (1) official/reputable harness-native integration →
  (2) the tool's own installer → (3) direct file edits (last resort).
- **Idempotent + consented:** announce before installing, offer a single
  opt-out, skip what's already present, skip-and-notify on existing files.

## Supported agents
The only currently supported agents are **Claude Code** and **Pi**. Do not
add references to any other specific agent (Gemini, Copilot, Codex, opencode,
etc.) in docs, stubs, or prompts until a harness for it exists in
[`harnesses/`](harnesses/).

## Adding a harness
Add `harnesses/<name>.md` and a referenced section in [`setup.md`](setup.md). No per-harness
scripts. See [`harnesses/claude-code.md`](harnesses/claude-code.md) and [`harnesses/pi.md`](harnesses/pi.md) for the pattern.

## Writing docs
- **Repo file references:** always use Markdown links — e.g. [`setup.md`](setup.md), not bare `setup.md`. Applies in all `.md` files in the repo.

## Where things live
See [`README.md`](README.md) for project overview. Harness files live in [`harnesses/`](harnesses/). Stubs in [`stubs/`](stubs/).
