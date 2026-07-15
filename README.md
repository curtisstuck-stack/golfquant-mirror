# golfquant-mirror

Public mirror of [dispersion.golf](https://dispersion.golf) model output. Pushed by the pipeline host on each cycle.

- `feed/site-feed.json` — current model feed (model probabilities vs live Kalshi prices)
- `ledger/calls.jsonl` — append-only, hash-chained ledger of published calls
- `ledger/calls.head` — current chain head
- `ledger/anchors/` — RFC 3161 + OpenTimestamps anchoring evidence

To verify the ledger independently, see `docs/verifying-the-ledger.md` in the main project.
