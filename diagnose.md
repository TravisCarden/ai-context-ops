# diagnose.md — measurement & debugging prompt

> Your AI coding agent reads this and follows it. Run it in a fresh session
> after setup, and periodically thereafter.

---

## What this does

Measures how much the stack is actually saving, surfaces the biggest current
waste source, and fixes common configuration problems. Work through the steps
in order.

---

## Step 1 — Collect stats

Run these commands and capture their output:

```
headroom stats
rtk gain          # Claude Code only — skip if not present
```

Also ask the agent to run `/caveman-stats` (Claude Code) or the equivalent
caveman report for Pi. If the command isn't recognized, note that caveman stats
aren't available.

---

## Step 2 — Interpret the numbers

For each tool that reported data, assess:

- **Headroom:** What is the compression ratio on tool results? What is the
  cache hit rate? A hit rate below ~60% in a session with repeated reads
  suggests an unstable prefix.
- **RTK / pi-hypa:** What is the token savings percentage on filtered output?
  A very low number suggests few CLI calls are being made, or the hook isn't
  firing.
- **caveman:** Is verbosity suppression active? If `/caveman-stats` returned
  nothing, confirm the plugin is installed with `claude plugin list | grep -i caveman`
  (Claude Code) or `pi config -l | grep -i caveman` (Pi).

Report a brief plain-English summary: how much is each layer saving, and
which layer is contributing the most?

---

## Step 3 — Diagnose a broken cache prefix

If the Headroom cache hit rate is low (under 40%), or sessions feel slower and
costlier than expected, run:

```
headroom debug prefix
```

Look for:
- Dynamic timestamps, random IDs, or session tokens appearing in the prefix
- Tool descriptions that change between turns
- Caveman-shrink not registered (check `claude mcp list` or equivalent)

If a dynamic element is found: identify its source (which tool, which config
file) and either remove it or move it after the stable prefix region. After
fixing, re-run `headroom stats` in the next session to confirm improvement.

---

## Step 4 — Check all components are wired

Verify each tool responds:

**Claude Code:**
```
headroom --version
rtk --version
claude plugin list        # should include: caveman
claude mcp list           # should include: headroom/serena
```

**Pi:**
```
headroom --version
pi config -l              # should include: pi-hypa, pi-lean-ctx, pi-caveman, pi-headroom
```

For any missing component, re-run the relevant section of the setup harness
([`harnesses/claude-code.md`](harnesses/claude-code.md) or [`harnesses/pi.md`](harnesses/pi.md)).

---

## Step 5 — Update the compression model (if warranted)

If this session involved significant debugging, error messages, or large tool
outputs:

```
headroom learn --apply
```

This updates Headroom's neural compression model based on the patterns it saw.
Run it at the end of substantial work sessions, not routine ones.

---

## Step 6 — Report and recommend

Summarize findings in this format:

```
## Diagnostics — [date]

### Savings by layer
- Headroom:         [compression ratio, cache hit rate]
- RTK / pi-hypa:    [token savings %]
- caveman:          [verbosity suppression active / not active]

### Biggest current waste source
[one sentence]

### Recommended next action
[one concrete step, or "no action needed"]
```

If anything was fixed during this session, note what changed.
