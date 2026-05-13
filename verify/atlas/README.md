# verify/atlas/ — vendored atlas anchors

> **Self-contained closure**: these `.n6` shards make `verify/*.py` runnable
> without depending on the (now-relocated) `~/core/canon/atlas/` directory.
> Each anchor in this directory is **vendor-attributed** via its `anchor=`
> field (`<datasheet>/<section>/<vintage>`) — per LATTICE_POLICY.md §1.2 and
> raw#10 C3 (no lattice-fit on external entities; use vendor's own invariants).

## Files

| Shard | Provenance | Purpose |
|---|---|---|
| `atlas.append.engineering-content-mk-next-2026-05-06.n6` | absorbed shard 20260508-190356 (originally canon/atlas/) | Universal physics + standards anchors (UL 489, IEC 61810, JEDEC, ARM Cortex-M4 TRM, etc.) |
| `atlas.append.hsscb-mk1-vendor-anchors-2026-05-06.n6` | absorbed shard 20260508-190356 | 4-foundry SiP vendor anchors (YESPOWER SiC / DB HiTek BCD / SK Key Foundry Σ-Δ / ST STM32F429 commercial fallback) |
| `atlas.append.hsscb-mk1-spec-derived-2026-05-07.n6` | absorbed shard 20260508-190356 | Spec-derived BOM-class anchors (per engpack §9) |

## Why vendored

The upstream `~/core/canon/atlas/` directory was decommissioned during the
canon → meta-doc consolidation; HSSCB anchors landed in
`~/core/nexus/n6/_absorbed/20260508-190356/`. To keep this repo's verifier
surface self-contained and reproducible, the 3 shards needed by
`verify/atlas_anchors.py` are checked in here.

## Resolution order

`verify/atlas_anchors.py` looks in this order:

1. `HSSCB_ATLAS_DIR` environment variable (CI override / debug)
2. `verify/atlas/` (this directory — repo-local, default)
3. `~/core/canon/atlas/` (legacy fallback; only used if the first two miss)

## Update policy

These files are **mirrors**, not authoritative — upstream source-of-truth
is the original absorbed shard. If a vendor datasheet supersedes an anchor
(e.g. YESPOWER releases a new Vds rating), update upstream first, then
re-sync here and re-run `python3 verify/sscb_verify.py` to confirm closure.
