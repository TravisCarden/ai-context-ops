# Architecture Decision Log

Decisions from the initial design interview (2026-07-16). Format: decision →
rationale. "REVIEW" tags mark choices held back for colleague/domain review.

---

## D1 — Audience: personal *and* publicly shareable
Portable install path, cloneable structure, generic stub names. Drives clarity
requirements throughout.

## D2 — Platform: macOS only
Linux is low-likelihood someday/maybe. No cross-platform conditionals.

## D3 — Prompt-primary architecture
The primary artifact is a prompt (`setup.md`) an agent reads and follows, not a
deterministic `install.sh`. Deterministic commands live *inside* the prompt as
referenced snippets.
- **Why:** most setup work is adaptive judgment (detect environment, handle
  existing files, wire config) — exactly what agents do well and what makes
  rigid scripts fragile. A prompt is equally reviewable, self-updating via URL,
  and needs no clone.
- **Boundary:** prompts win except where determinism/offline/zero-token truly
  help. Evaluation & debugging (`diagnose.md`) are inherently judgment-heavy and
  became prompts too.

## D4 — Entry point: URL-first, clone-optional
`Read https://raw.githubusercontent.com/.../setup.md and follow it`. BC-free
updates, instant reach, no clone. Prompt works identically from a local clone.

## D5 — Cold-start handled in README, no bootstrap script
A user needs one working agent to run the prompt. README states the prerequisite
and links Claude Code / Pi directly, plus caveman's per-agent matrix for others.
A bootstrap script would reintroduce exactly the artifact D3 removed.

## D6 — Install behavior (expressed as prompt instructions)
Announce everything upfront (esp. global software: Homebrew, uv, caveman's
standard `curl|bash` — framed as the normal method, not scary); single opt-out
before anything runs; idempotent (skip if present); skip-and-notify on existing
config files.

## D7 — Python tools via `uv tool install`
Headroom (and LeanCTX where relevant) need isolation from macOS's externally
managed Python. `uv` (brew-installable) gives clean per-tool isolation with no
venv management. Offered with opt-out like any dependency.

## D8 — Install `headroom-ai[all]` (ML compression) universally
Max compression is the point of the project. `[all]` enables neural (Kompress)
compression, high-value for large, complex codebases like Drupal.
- Prompt defaults to installing it; discloses ~2–3GB download and Apple-Silicon
  requirement (graceful fallback on Intel).
- `[vector]` excluded (needs C++ toolchain; unrelated to compression).

## D9 — Harness install is the user's choice
`setup.md` supports both MVP harnesses but asks which to install (Claude Code,
Pi, or both). Nothing is set up automatically. Harness selection drives installs.

## D10 — MVP harnesses: Claude Code + Pi
Both must be fully supported. opencode + copilot-cli = best-effort/later;
gemini-cli = someday/maybe. Every shipped harness fully working beats several
half-working. opencode's unresolved proxy wiring stays deferred.

## D11 — Wiring hierarchy
(1) official/reputable harness-native integration → (2) the tool's own installer
→ (3) direct file edits (last resort, unless documented best practice). Third-
party integrations must be de-facto-standard / reputable / safe.

## D12 — Claude Code wiring
Official tool installers: `brew` (rtk), `uv` (headroom[all]),
`rtk init --global`, `headroom init claude`, `claude plugin install
caveman@caveman`, `claude mcp add caveman-shrink`. Idempotency via each tool's
CLI. Serena left to headroom.

## D13 — Pi wiring: native npm packages
Idiomatic `pi install npm:…` (Pi's only install path; `pi.dev/packages` is a web
gallery over npm). No brew/uv/rtk for Pi itself.
- **REVIEW — RTK → Hypa substitution:** RTK has no Pi path (no package, no
  `init`, undocumented). `@hypabolic/pi-hypa` does RTK's job natively (highest
  adoption ~5.8k/wk, authored by Hypa's team, local/deterministic). Different
  tool; quality-vs-RTK unverified.
- **REVIEW — pi-caveman provenance:** authored by jonjonrankin, *not* the
  caveman author. ~1k/wk adoption clears a reasonable bar; flagged.
- **REVIEW — pi-headroom low adoption:** ~42/wk. Works; maintenance risk noted.
- `pi-lean-ctx`: first-party (LeanCTX author). Strong.

## D14 — Both harnesses converge on one ML-equipped headroom (key finding)
`pi-headroom` checks the system PATH *first* (`ensureInstalled` step 1) before
creating its `[proxy]`-only venv. Installing `headroom-ai[all]` via uv puts an
ML-equipped `headroom` on PATH, which pi-headroom then uses transparently — Pi
inherits full compression with no venv, no `[proxy]` fallback, no file edits.
This is why the uv/headroom[all] install is universal, not Claude-Code-only.

## D15 — Pi's lone platform dependency: Python >= 3.10
`pi-headroom` needs a Python >=3.10 interpreter (manages its own venv). Prompt
checks and recommends `brew install python` only if missing. Other Pi packages
(hypa, lean-ctx, caveman) are self-contained Node.

## D16 — Ignore stub: minimal & safe only
Exclude unambiguous noise (node_modules, vendor, dist, build, *.lock, .DS_Store).
No aggressive excludes where dependency internals could matter. Opinionated /
Drupal-aware patterns → ROADMAP.

## D17 — Context files: no duplication
- Global RTK rules are NOT hand-written: `rtk init` auto-places `~/.claude/
  RTK.md` + hook for Claude Code; Pi uses Hypa. No generic global RTK stub.
- `stubs/project-context.md` is a terse skeleton; omits anything the tools/harness
  already provide. Duplication conflicts with the project's optimization goal.
- Human process (headroom-learn cadence) lives in playbooks,
  not agent instructions.

## D18 — Serena left to headroom
`headroom init claude` owns headroom's hooks/MCP; serena bundled via headroom.
No explicit serena registration.

## D19 — Docs describe; playbooks prescribe; prompts execute
- `diagnose.md` (execution) and consolidated `maintenance.md` (recurring
  behavior). Behavioral cadence still taught (D20).
- `docs/cache-optimization.md` is *concept only* (why misses happen).

## D20 — Recurring hygiene → one lean `maintenance.md`
`headroom learn` cadence, `pi update`, periodic `diagnose.md`. A prompt is
reactive; the *habit* of invoking it must be taught. One playbook, not many.

## D21 — `mcp-servers.json` (if kept) is reference-only
setup uses CLIs (`claude mcp add`), not a file the prompt applies — avoids
drift. (Under D3, largely superseded; revisit if needed.)

## D22 — Delivery: self-contained functional MVP + isolated review PRs
- **MVP core:** `README.md`, `AGENTS.md`, `setup.md` (both harnesses, user-chosen),
  `diagnose.md` core, stubs, `harnesses/*.md`, `docs/{stack,benchmarks}`,
  `playbooks/{maintenance, prompting[accepted practices], new-project}`, meta.
- **REVIEW PRs:** tool-substitution/provenance flags (D13), prompting micro-opt
  ROI, `compute-optimization.md`, `cache-optimization.md` concept, ignore-stub
  opinionation. MVP does not depend on any of these.

## D23 — `AGENTS.md` is a living example of the principles
Well-compressed, structured, terse — dogfooding at the repo root.

---

## Research inputs (fold in during write-up; verify, don't assume)
- humanlayer 12-factor-agents — factor-03 "own your context window".
- abhishekray07/claude-md-templates.
- Seek more reputable sources: agents.md convention, Anthropic context-
  engineering guidance, each tool's official docs.
