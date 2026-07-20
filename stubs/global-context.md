# Global Agent Instructions

<!-- Terse skeleton for user-level context. Deployed to ~/.claude/CLAUDE.md,
     ~/.pi/agent/AGENTS.md, or equivalent. Keep it lean — this loads on every
     session across every project. Only put things that should ALWAYS apply. -->

- **Env:** `gh` available; use non-interactive flags (`-y`, `--yes`)
- **Paths:** Use relative `cd` (avoids prompts); never commit absolute paths
- **Safety:** Ask before push, PR, git rewrite, or destructive operations
- **Critique:** Challenge my ideas/code if flawed or a simpler path exists
- **Style:** Write minimal, idiomatic code matching existing project patterns
- **Output:** NO placeholders (`// ...`); write full code replacements
- **Search:** Use Serena/RTK instead of `grep`/`cat`/`find` to save tokens
- **Scope:** Respect `context-mode` boundaries to keep session lean
