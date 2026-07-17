# diagnose.md — measurement & debugging prompt

> An agent reads this and follows it: measures effectiveness, interprets the
> numbers, recommends/executes fixes.

**Status:** scaffold. Core in MVP.

## Intended behavior (to implement)
- Run and interpret: `rtk gain`, `headroom stats`, `/caveman-stats`.
- Read cache hit rate; diagnose a broken/dynamic prefix.
- Recommend/trigger `headroom learn --apply` after significant debugging.
- Surface the biggest current token/cost leak and the single next fix.
