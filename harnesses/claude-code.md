# Harness: Claude Code

Authoritative, execution-ready steps for the Claude Code path. Called by
`setup.md` (Step 4) after environment detection, harness choice, and consent.

**Rules (inherited from setup.md):** idempotent — check first with each tool's
own CLI and skip if present; never overwrite an existing config file (skip and
notify). Announce nothing new here that wasn't in the Step 3 plan. Any
repo-relative path here (e.g. `stubs/project-context.md`) is a local file under
the repo root established in Step 0 — read it directly, no web requests.

---

## 1. Homebrew (only if missing)
Check: `command -v brew`. If absent, install per <https://brew.sh> and ensure it
is on PATH for the current shell before continuing.

## 2. CLI tools via Homebrew
Install only the ones Step 1 reported missing. Check before each:
```
rtk --version     || brew install rtk        # output filtering (drops ~/.claude/RTK.md + hook)
uv --version      || brew install uv         # Python tool manager (for Headroom)
```

## 3. Headroom (ML compression) via uv
Shared across harnesses. Skip if `headroom` is already on PATH.
```
uv tool install --python 3.13 "headroom-ai[all]"
```
`[all]` includes neural (Kompress) compression. ~2–3 GB; Apple Silicon best,
graceful fallback on Intel. Verify: `headroom --version`.

## 4. Headroom → Claude Code hooks
Registers durable hooks + provider routing (this also covers Serena — do not
register Serena separately).
```
headroom init claude
```
Idempotent by design; safe to re-run.

## 5. RTK → Claude Code
`brew install rtk` already dropped `~/.claude/RTK.md` and a PreToolUse hook.
Ensure global wiring (idempotent):
```
rtk init --global
```
**Do not** hand-write RTK substitution rules into any context file — the hook handles that.

## 6. caveman (output verbosity)
Install the plugin via caveman's Claude Code flow. Check first:
`claude plugin list | grep -qi caveman`.
```
claude plugin marketplace add JuliusBrussee/caveman
claude plugin install caveman@caveman
```

## 7. caveman-shrink (MCP tool-description compression)
Register as a user-scoped MCP server. Check first:
`claude mcp list | grep -qi caveman-shrink`.
```
claude mcp add caveman-shrink -s user -- npx caveman-shrink
```

## 8. Global context file
Deploy `<repo-root>/stubs/global-context.md` → `~/.claude/CLAUDE.md`.
- If absent: copy it.
- If present: **do not overwrite** — notify the user and tell them to merge
  `<repo-root>/stubs/global-context.md` manually.
For per-project context, point them at `<repo-root>/stubs/project-context.md`.

## 9. Verify
```
headroom --version
rtk --version
claude plugin list        # caveman present
claude mcp list           # caveman-shrink (and headroom/serena) present
```
Report a short summary of what was installed vs. skipped.
