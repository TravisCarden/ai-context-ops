# ai-context-ops — setup

You are an AI coding agent setting up an optimized context/token stack on this
**macOS** machine. Follow these steps in order. Do not improvise installs the
user hasn't seen. Work idempotently and get explicit consent before installing
anything.

Base URL for files this prompt references:
`https://raw.githubusercontent.com/TravisCarden/ai-context-ops/main/`
(If you are running from a local clone, read the sibling files directly.)

---

## Step 1 — Detect the environment (no changes yet)

Report a short table of what is already present, so nothing is reinstalled:

- `brew --version`, `uv --version`, `python3 --version` (need ≥ 3.10 for Pi)
- Which harnesses exist: `claude --version`, `pi --version`
- Which tools exist: `rtk --version`, `repomix --version`, `headroom --version`

Do not install anything in this step.

## Step 2 — Ask which harness(es) to set up

Ask the user to choose. Nothing is set up automatically:

- **Claude Code**
- **Pi**
- **Both**

Only proceed with the chosen harness path(s).

## Step 3 — Announce the plan and get one consent

Before installing anything, show the user the full list of what you will
install for their chosen harness(es), and call out global software explicitly:

- **Homebrew** — installed only if missing (`https://brew.sh`).
- **uv** — Python tool manager (`brew install uv`), used to install Headroom in
  isolation.
- **Headroom `[all]`** — includes neural ML compression. **~2–3 GB download**,
  best on Apple Silicon (falls back gracefully on Intel). Recommended: it is the
  core of the compression gain, especially on large, complex codebases like
  Drupal. This install is shared by both harnesses.
- **caveman** — installs via its standard `curl | bash` (Claude Code path) or
  `pi install` (Pi path). The `curl | bash` is caveman's normal, documented
  install method.

Skip anything Step 1 found already present. Then ask once: **Proceed? [Y/n]**.
If the user declines, stop cleanly. Re-running later is safe (idempotent).

## Step 4 — Run the chosen harness path(s)

- **Claude Code** → follow `harnesses/claude-code.md`.
- **Pi** → follow `harnesses/pi.md`.

Apply these rules throughout every path:
- **Idempotent:** check first (prefer each tool's own CLI), skip if present.
- **Existing config files:** never overwrite. If a target file already exists
  (e.g. `~/.claude/CLAUDE.md`, `~/.pi/agent/AGENTS.md`), **skip and notify** the
  user, pointing them at the stub to merge manually.
- **Global context files** are placed by the harness path, not here.

## Step 5 — Verify and hand off

After setup, confirm each installed piece responds (e.g. `headroom --version`,
`rtk --version`, `claude mcp list`, `pi config -l`). Then tell the user:

- To measure effectiveness or debug savings later:
  `Read <base-url>diagnose.md and follow it`.
- Per-project setup (ignore files, project context): see
  `playbooks/new-project.md`.
- Ongoing hygiene (learn cadence, updates): see `playbooks/maintenance.md`.
