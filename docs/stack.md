# Stack

**Status:** scaffold. MVP (factual). Describes; does not prescribe.

---

## The problem

Every token an AI agent reads or writes costs money and fills the context
window. Left unchecked, sessions bloat with noisy tool output, verbose replies,
redundant file reads, and repeated symbol discovery — each one crowding out the
work that matters. This stack intercepts those waste streams at the layer where
each originates.

---

## How the layers fit

A session has four points where tokens accumulate. One or more tools targets
each:

```
┌─────────────────────────────────────────────────────────────┐
│  PROMPT / INPUT                                             │
│  LeanCTX — AST-maps source files instead of dumping raw     │
│  repomix — packs scoped repo subsets on demand              │
│  stubs   — concise, stable context files (global/project)   │
├─────────────────────────────────────────────────────────────┤
│  TOOL RESULTS (MCP / CLI output coming back to the agent)   │
│  Headroom       — compresses results via MCP proxy          │
│  RTK / pi-hypa  — filters CLI output noise before it lands  │
│  caveman-shrink — shrinks MCP tool descriptions             │
├─────────────────────────────────────────────────────────────┤
│  AGENT REPLIES                                              │
│  caveman — suppresses over-verbose agent responses          │
├─────────────────────────────────────────────────────────────┤
│  CACHE (provider-side prompt cache)                         │
│  Headroom durable hooks keep the prefix stable so the       │
│  cache key doesn't break between turns                      │
└─────────────────────────────────────────────────────────────┘
```

Data flows: you write a prompt → agent reads context files and source
(LeanCTX/repomix reduce this) → agent calls tools → tool results return through
Headroom/RTK (compressed/filtered) → agent replies through caveman (compressed)
→ provider caches the stable prefix (Headroom hooks maintain stability).

---

## Component reference

| Tool | Layer | What it does |
|---|---|---|
| **Headroom** | Tool results + cache | Compresses MCP tool results via proxy; durable hook placement keeps the prompt prefix stable for provider caching. Neural (Kompress) compression enabled via `[all]` install. |
| **RTK** *(Claude Code)* | Tool results | Filters noisy CLI/tool output before it enters the context window. Wired via `rtk init --global` + PreToolUse hook. |
| **pi-hypa** *(Pi)* | Tool results | Pi-native equivalent of RTK. Installed as a Pi plugin. |
| **LeanCTX / pi-lean-ctx** | Input | AST-maps source files — agent reads a compact symbol map instead of raw file bytes. First-party (LeanCTX author). |
| **caveman** | Agent replies | Suppresses over-verbose agent responses. Installed as a plugin on both harnesses. |
| **caveman-shrink** | Tool results | Compresses MCP tool *descriptions* (the schema overhead each tool carries), not just results. Registered as a user-scoped MCP server. |
| **repomix** | Input | Packs scoped repo subsets into a single consumable artifact. Use with `--include` to scope; never dump the whole repo. |
| **Serena** | Input | LSP-aware symbol search — bundled via Headroom. Lets the agent navigate code without `grep`/`cat` file dumps. Registered automatically by `headroom init`. |
| **Context stubs** | Input | Concise, stable context files (`global-context.md`, `project-context.md`) deployed to the agent's config directory. Stable content = better cache hits. |

---

## Per-harness variance

Most of the stack is shared. The differences are plugin/hook wiring only:

| Component | Claude Code | Pi |
|---|---|---|
| Headroom | `uv tool install "headroom-ai[all]"` + `headroom init claude` | Same `uv` install; `pi install npm:pi-headroom` wires it |
| Output filter | RTK (`brew install rtk` + `rtk init --global`) | pi-hypa (`pi install npm:@hypabolic/pi-hypa`) |
| LeanCTX | — | `pi install npm:pi-lean-ctx` |
| caveman | `claude plugin install caveman@caveman` | `pi install npm:pi-caveman` |
| caveman-shrink | `claude mcp add caveman-shrink -s user -- npx caveman-shrink` | — |
| Serena | Registered by `headroom init claude` | Registered by pi-headroom |
| repomix | `brew install repomix` | `brew install repomix` |

---

## Active vs. passive

Some tools run automatically once installed; others require deliberate use.

**Passive — runs automatically after setup:**
- Headroom (MCP proxy + hooks fire on every tool call)
- RTK / pi-hypa (PreToolUse hook intercepts output)
- caveman (plugin intercepts agent replies)
- caveman-shrink (MCP middleware wraps tool descriptions)
- Serena (available as a tool; agent calls it instead of grep)

**Active — you drive these:**
- `repomix --include <path>` — scope and pack before a large read
- `headroom learn --apply` — run after significant debugging sessions to update the compression model
- Context stubs — you fill in `project-context.md` per repo; you extend `global-context.md` with your environment
- `diagnose.md` prompt — run periodically to measure savings and catch regressions
