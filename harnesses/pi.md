# Harness: Pi

**Status:** scaffold. MVP.

Idiomatic `pi install npm:…` — Pi's native package system. No brew/uv/rtk needed
for Pi itself (Node is already present).
- `pi install npm:@hypabolic/pi-hypa`  (output filtering — RTK's job, native)
- `pi install npm:pi-lean-ctx`         (LeanCTX, first-party)
- `pi install npm:pi-caveman`          (caveman — third-party; see DECISIONS)
- `pi install npm:pi-headroom`         (low adoption; see DECISIONS)
- Global context file: `~/.pi/agent/AGENTS.md` (skip-and-notify if present).
- Requires Python >= 3.10 for pi-headroom's venv; recommend `brew install
  python` only if missing.
- Full ML compression: install `headroom-ai[all]` via uv so pi-headroom's
  PATH-first detection uses it (no `[proxy]`-only venv).
