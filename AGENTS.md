# Working on ai-context-ops

Instructions for AI agents contributing to *this repo*. This file is also a
living example of the project's own principles: terse, structured, no waste.

## Principles
- **Prompt-primary.** `setup.md` and `diagnose.md` are the primary artifacts.
  Deterministic commands live inside prompts as referenced snippets — there is
  no `install.sh`.
- **Docs describe. Playbooks prescribe. Prompts execute.**
- **Never duplicate an instruction** — duplication costs tokens on every run.
- **Wiring hierarchy:** (1) official/reputable harness-native integration →
  (2) the tool's own installer → (3) direct file edits (last resort).
- **Idempotent + consented:** announce before installing, offer a single
  opt-out, skip what's already present, skip-and-notify on existing files.

## Adding a harness
Add `harnesses/<name>.md` and a referenced section in `setup.md`. No per-harness
scripts. See `harnesses/claude-code.md` and `harnesses/pi.md` for the pattern.

## Where things live
See `meta/STATUS.md` for current state and `meta/DECISIONS.md` for why.
