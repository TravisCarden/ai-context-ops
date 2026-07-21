# Stubs

Starter templates to copy into place. The setup harnesses deploy
`global-context.md` automatically (skip-and-notify if the target exists).
`project-context.md` and `ignore` are for manual use.

| Stub | Deploy to | Purpose |
|---|---|---|
| [`global-context.md`](global-context.md) | `~/.claude/CLAUDE.md`, `~/.pi/agent/AGENTS.md`, etc. | User-level instructions applied to every session across all projects |
| [`project-context.md`](project-context.md) | `./CLAUDE.md`, `./AGENTS.md`, etc. (repo root) | Per-project context: paths, conventions, notes specific to one repo |
| [`ignore`](ignore) | `.claudeignore`, etc. (repo root) | Tells the agent which files/dirs to skip when reading the codebase |

**After deploying `global-context.md`**, add any environment-specific notes that
are true for you personally. For example, if you have `gh` installed:
```
- **Env:** `gh` available
```
