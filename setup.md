# setup.md — primary setup prompt

> An agent reads this and follows it. URL-first, clone-optional.

**Status:** scaffold. Build in MVP (Claude Code path, then Pi path).

## Intended behavior (to implement)
1. Detect environment; report what's present vs. missing.
2. Ask which harness(es) to set up: Claude Code, Pi, or both. Nothing is
   installed automatically.
3. Announce everything to be installed (incl. global software: Homebrew, uv,
   caveman's standard `curl|bash`). Offer a single opt-out. Then proceed.
4. Idempotent: skip anything already present. Skip-and-notify on existing
   config files (e.g. `~/.claude/CLAUDE.md`, `~/.pi/agent/AGENTS.md`).
5. Install `headroom-ai[all]` via `uv tool install` whenever Pi or Claude Code
   is chosen (ML compression; Pi inherits it via PATH-first detection).

See `harnesses/claude-code.md` and `harnesses/pi.md` for the per-harness steps.
