# Harness: Claude Code

**Status:** scaffold. MVP.

Uses generic binaries + each tool's own installer:
- `brew install rtk repomix`
- `uv tool install "headroom-ai[all]"`  (ML compression; shared with Pi)
- `rtk init --global`                    (auto-places `~/.claude/RTK.md` + hook)
- `headroom init claude`                 (durable hooks + provider routing)
- `claude plugin install caveman@caveman`
- `claude mcp add caveman-shrink --scope user -- npx caveman-shrink`
- Serena: left to headroom (no separate registration).

Idempotency checks use each tool's own CLI where possible
(`claude plugin list`, `claude mcp list`, etc.).
