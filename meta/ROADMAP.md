# Roadmap (post-MVP)

## Harnesses
- opencode (needs `headroom proxy` + `OPENAI_BASE_URL`; wiring unresolved).
- copilot-cli (best-effort).
- gemini-cli (someday/maybe).

## Enhancements held for review (see DECISIONS D22)
- Prompting micro-optimization ROI (measured).
- `compute-optimization.md` (model choice, thinking mode, subagents).
- `cache-optimization.md` conceptual write-up (sourced).
- Ignore-stub opinionation (Drupal-aware, e.g. web/core vs modules/contrib).

## Tooling / packaging
- Brewfile, flake.nix (Linux/Nix path).
- Possible Pi ML augmentation if pi-headroom exposes extras (currently moot via
  PATH convergence — see DECISIONS D14).

## Provenance watch (DECISIONS D13)
- Confirm pi-caveman / pi-headroom stay maintained; re-evaluate RTK→Hypa if RTK
  ships Pi support.
