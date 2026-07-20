# Stubs

Starter templates to copy into place. The setup harnesses deploy these
automatically; you can also grab them manually for reference or to merge with
an existing file.

| Stub | Deploy to | Purpose |
|---|---|---|
| [`global-context.md`](global-context.md) | `~/.claude/CLAUDE.md`, `~/.pi/agent/AGENTS.md`, etc. | User-level instructions applied to every session across all projects |
| [`project-context.md`](project-context.md) | `./CLAUDE.md`, `./AGENTS.md`, etc. (repo root) | Per-project context: paths, conventions, notes specific to one repo |
| [`ignore`](ignore) | `.claudeignore`, `.geminiignore`, etc. (repo root) | Tells the agent which files/dirs to skip when reading the codebase |

**Rules the harnesses follow (and you should too):**
- Never overwrite an existing file — merge manually instead.
- Keep context files lean; don't repeat instructions that already live elsewhere.
