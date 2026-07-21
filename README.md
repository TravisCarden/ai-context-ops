# ai-context-ops

Operational discipline for AI-agent context and token cost on macOS.

**Status:** scaffold — see [`meta/STATUS.md`](meta/STATUS.md).

> [!NOTE]
> This is not a product and comes with no support or warranty. It's my own
> opinionated collection of best practices for wringing the most out of AI
> coding agents, shared in case it's useful. It wires together third-party
> tools I don't maintain; review what the setup prompt does before running it,
> and adapt it to your own judgment.

## Quick start (60 seconds)

You need one supported AI coding agent. If you don't have one:

- **Claude Code** — <https://docs.anthropic.com/en/docs/claude-code>
- **Pi** — <https://pi.dev>
- **Any other agent** — <https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md#per-agent-install>

Before running setup, read [`docs/stack.md`](docs/stack.md) for a 5-minute orientation — what each tool does, where it fits in the stack, and what runs automatically vs. what you drive. It makes the setup choices meaningful rather than mechanical.

Then point your agent at the setup prompt:

```
Read https://raw.githubusercontent.com/TravisCarden/ai-context-ops/main/setup.md and follow it
```

It will ask which harness(es) to set up (Claude Code, Pi, or both) and walk the
rest, with an option to accept the plan or abort before making changes.

## Repo layout

| Path | What it is |
|---|---|
| [`setup.md`](setup.md) | Entry point — the prompt you give your agent |
| [`diagnose.md`](diagnose.md) | Measures token savings and helps debug the stack post-setup |
| [`harnesses/`](harnesses/) | Per-agent setup steps called by [`setup.md`](setup.md) |
| [`stubs/`](stubs/) | Starter templates for context and ignore files — **read these to understand what gets deployed** |
| [`playbooks/`](playbooks/) | Recipes for recurring tasks (new project, maintenance, …) |
| [`docs/stack.md`](docs/stack.md) | How the stack works — layers, components, per-harness variance, active vs. passive |
| [`meta/`](meta/) | Project decisions and status |


