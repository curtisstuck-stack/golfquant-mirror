# Errata

## 2026-07-16 — in-play calls retracted (pre-anchor beta reset)

Between `2026-07-16T05:45:34Z` and `2026-07-16T21:30:56Z`, **82 calls** were published
to this ledger on The Open (`espn:401811957`) make-cut markets by an automated
5-minute in-play loop.

**These calls were invalid.** The model that generated them does not condition on live
round-1 scores. Its probabilities stayed frozen at pre-tournament values — drifting only
by Monte Carlo sampling noise (~±0.005) — while market prices repriced on actual play.
Example, same market, ~40 hours apart:

| Player | Pre-tournament | In-play (round 1 complete) |
|---|---|---|
| Justin Rose (`JROS`) | model 0.6442 · market 72¢ | model 0.6492 · market 13¢ |
| MJ Daffue (`MDAF`) | model 0.3381 · market 30¢ | model 0.3404 · market 84¢ |

The resulting "edges" were artifacts of a stale model, not measured inefficiencies.

### What was removed

All 82 in-play calls. The ledger was truncated to the **27 calls published while the
tournament state was `pre`** (through `2026-07-16T05:30:34Z`) — generated with a valid
pre-tournament information set, before any shot was struck.

- Head before retraction (109 entries): `da3595b152b4ce3eebda5d2cbdf2bb94abc9c5b780fd07e26f2de48c839cb99a`
- Head after retraction (27 entries): `6e0eafc41f24fde69ce526aa88c8a2cb80bb9cea3c04d66fad9f261659ce5ae6`

### Basis for removal

Removal was applied to the **entire contiguous block** of in-play calls, selected on the
defect — publication while `tournament.state == "in"` — and **not on outcome**. No call was
removed because of how it was performing. The block boundary is the feed's own state
transition, which occurred at `2026-07-16T05:45:03Z`.

**No anchor had been taken.** The removed calls were never externally timestamped
(RFC 3161 / OpenTimestamps). This repository's git history retains the full record of the
affected commits; nothing has been force-pushed.

In-play publishing is disabled until the model conditions on live tournament state.
