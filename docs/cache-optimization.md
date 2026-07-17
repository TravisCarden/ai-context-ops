# Cache Optimization (concept)

**Status:** scaffold. REVIEW PR — touches provider caching internals; source
carefully before asserting.

The conceptual "why": prompt caching needs a stable prefix; dynamic content in
the prefix breaks the cache key. The tooling (headroom durable hooks) is meant
to produce the right conditions automatically — this doc explains the mechanism,
`diagnose.md` handles the action.
