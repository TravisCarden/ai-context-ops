# Stack Reference

**Status:** scaffold. MVP (factual).

Tier map + per-component reference: what each tool does, its data flow, and how
it fits. Describes; does not prescribe.

| Tool | Layer | Role |
|---|---|---|
| headroom | Compression | Compresses tool results/responses (MCP + proxy) |
| RTK / pi-hypa | Output filter | Filters CLI/tool output noise |
| caveman | Output verbosity | Compresses agent reply verbosity |
| LeanCTX | Input code | AST-maps source files before context |
| repomix | Repo packing | Packs scoped repo subsets for consumption |
| serena | Code nav | LSP-aware symbol search (bundled via headroom) |
| caveman-shrink | MCP middleware | Compresses MCP tool descriptions |

<!-- TODO(mvp): expand each row; data-flow diagram; per-harness variance. -->
