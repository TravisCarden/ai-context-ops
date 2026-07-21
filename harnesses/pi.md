# Harness: Pi

Authoritative, execution-ready steps for the Pi path. Called by `setup.md`
(Step 4) after environment detection, harness choice, and consent.

**Rules (inherited from setup.md):** idempotent — check first with each tool's
own CLI and skip if present; never overwrite an existing config file (skip and
notify). Announce nothing new here that wasn't in the Step 3 plan. Any
repo-relative path here is a local file under the repo root established in
Step 0 — read it directly, no web requests.

---

## 1. Homebrew (only if missing)
Check: `command -v brew`. If absent, install per <https://brew.sh> and ensure it
is on PATH for the current shell before continuing.

## 2. uv (only if missing)
```
uv --version || brew install uv
```

## 3. Headroom (ML compression) via uv
Shared across harnesses. Skip if `headroom` is already on PATH.
```
uv tool install --python 3.13 "headroom-ai[all]"
```
`[all]` includes neural (Kompress) compression. ~2–3 GB; Apple Silicon best,
graceful fallback on Intel. Requires Python ≥ 3.10. Verify: `headroom --version`.

## 4. Pi plugins via `pi install`
Install only the ones Step 1 reported missing:
```
pi install npm:@hypabolic/pi-hypa   # output filtering (RTK equivalent for Pi)
pi install npm:pi-lean-ctx          # LeanCTX, first-party context compression
pi install npm:pi-caveman           # output verbosity
pi install npm:pi-headroom          # Headroom integration
```

## 5. Global context file
Deploy `<repo-root>/stubs/global-context.md` → `~/.pi/agent/AGENTS.md`.
- If absent: copy it.
- If present: **do not overwrite** — notify the user and tell them to merge
  `<repo-root>/stubs/global-context.md` manually.
For per-project context, point them at `<repo-root>/stubs/project-context.md`.

## 6. Verify
```
headroom --version
pi config -l          # plugins visible
```
Report a short summary of what was installed vs. skipped.
