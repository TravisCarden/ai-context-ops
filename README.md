# AI Context Ops

**Cut your AI coding costs and keep your context window focused** — a curated
tool stack and agent-driven setup for Claude Code and Pi on macOS.

Every token an AI agent reads or writes fills your context window and costs
money. Left unchecked, sessions bloat with noisy tool output, verbose replies,
redundant file reads, and repeated symbol discovery — each one crowding out the
work that matters. This project installs and wires a set of lightweight tools
that intercept those waste streams at the layer where they originate.

---

## Quick setup

Paste this into your agent (Claude Code or Pi):

```
Read https://raw.githubusercontent.com/TravisCarden/ai-context-ops/main/setup.md and follow it.
```

Or from a local clone, open [`setup.md`](setup.md) and prompt your agent to read and follow it.

The setup prompt detects what you already have, shows you the full install plan, asks once for consent, and skips anything already present. It is safe to re-run.

---

## How it works

```
╔════════════════════════════════════════════════════════════╗
║  LAYER 1 · INPUT                                           ║
╟------------------------------------------------------------╢
║  LeanCTX — AST-maps source files instead of dumping raw    ║
║  stubs   — concise, stable context files (global/project)  ║
╠════════════════════════════════════════════════════════════╣
║  LAYER 2 · TOOL RESULTS                                    ║
║  (MCP / CLI output returning to the agent)                 ║
╟------------------------------------------------------------╢
║  Headroom       — compresses results via MCP proxy         ║
║  RTK / pi-hypa  — filters CLI output noise before it lands ║
╠════════════════════════════════════════════════════════════╣
║  LAYER 3 · AGENT REPLIES                                   ║
╟------------------------------------------------------------╢
║  caveman — suppresses over-verbose agent responses         ║
╠════════════════════════════════════════════════════════════╣
║  LAYER 4 · CACHE                                           ║
║  (provider-side prompt cache)                              ║
╟------------------------------------------------------------╢
║  Headroom durable hooks keep the prefix stable so the      ║
║  cache key doesn't break between turns                     ║
╚════════════════════════════════════════════════════════════╝
```

### Components

| Tool | Layer | What it does |
|---|---|---|
| [**Headroom**](https://headroom-docs.vercel.app) | Tool results + cache | Compresses MCP tool results via proxy; durable hook placement keeps the prompt prefix stable for provider caching. Neural (Kompress) compression via `[all]` install. |
| [**RTK**](https://www.rtk-ai.app/) *(Claude Code)* | Tool results | Filters noisy CLI/tool output before it enters the context window. Wired via `rtk init --global` + PreToolUse hook. |
| [**pi-hypa**](https://github.com/Hypabolic/Hypa#readme) *(Pi)* | Tool results | Pi-native equivalent of RTK. Installed as a Pi plugin. |
| [**LeanCTX**](https://leanctx.com) / [**pi-lean-ctx**](https://leanctx.com) | Input | AST-maps source files — agent reads a compact symbol map instead of raw file bytes. |
| [**caveman**](https://github.com/JuliusBrussee/caveman) | Agent replies | Suppresses over-verbose agent responses. |
| **Serena** | Input | LSP-aware symbol search — bundled via Headroom. Lets the agent navigate code without `grep`/`cat` file dumps. |

Most tools are **passive** — once installed, they intercept automatically. One
requires deliberate use: `headroom learn --apply` (run after significant
debugging sessions to update the compression model).

---

## Measuring effectiveness

After you've been using the stack for a while, run this in a fresh session to
see how much each layer is saving and surface any configuration issues:

```
Read https://raw.githubusercontent.com/TravisCarden/ai-context-ops/main/diagnose.md and follow it.
```

---

## Repo layout

| Path | What it is |
|---|---|
| [`setup.md`](setup.md) | **Start here** — the setup prompt you give your agent |
| [`diagnose.md`](diagnose.md) | Measures token savings; helps debug the stack post-setup |
| [`harnesses/`](harnesses/) | Per-agent setup steps called by [`setup.md`](setup.md) |
| [`stubs/`](stubs/) | Starter templates for global and per-project context files |
