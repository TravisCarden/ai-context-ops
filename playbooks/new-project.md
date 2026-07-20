# Playbook: New Project

**Status:** scaffold. REVIEW PR — includes Drupal-adjacent domain judgment.

Checklist for setting up a fresh repo with an optimized agent context stack.

## 1. Add an ignore file

Copy `stubs/ignore` to the ignore file for your agent (e.g. `.claudeignore`).
Extend it with any project-specific paths (build artifacts, fixtures, vendored
code) that are large and irrelevant to the agent's work.

## 2. Add a project context file

Copy `stubs/project-context.md` to the context file for your agent (e.g.
`CLAUDE.md` or `AGENTS.md`). Then fill in each section. Keep entries terse and behavioral — the agent acts
on what's here, so every token should change its behavior or save it work.

**`## Key paths`** — where things live that the agent can't easily infer from
the repo structure:
```
- Source: `src/`
- Config: `config/`
- Tests: `tests/Feature/`
```

**`## Patterns`** — conventions the agent couldn't reliably infer from reading
the code:
```
- Feature flags live in `config/flags.php`; never hardcode env checks
- Use the query builder; never write raw SQL
```

**`## Out of scope`** — paths or concerns to avoid or defer on:
```
- `legacy/` — do not touch
- Deployment config — lives outside this repo
```

Remove any section you have nothing to put in it. An empty heading costs tokens
on every session.

## 3. Prime the cache early

Have the agent read the context file and key paths at the start of the first
session, before diving into work. This front-loads the cost once rather than
paying for repeated re-discovery.
