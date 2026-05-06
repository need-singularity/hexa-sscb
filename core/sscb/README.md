# core/sscb — canonical specification

This directory holds the **two canonical specifications** for HEXA-SSCB mk1.

| File | Form | Lines | Audience |
|---|---|---|---|
| [`spec.md`](spec.md) | Short paper — n=6 lattice, COMPARE matrix, ASCII bars, §1–§22 | 387 | reviewers, external readers |
| [`domain.md`](domain.md) | Long domain doc — full §0 EXEC SUMMARY through §22, with circuit / PCB / firmware / mechanical / manufacturing / test / BOM / vendor / acceptance / appendix / IMPACT | 1320 | implementers, agents |

Both files are **canonical** — they describe the same artifact at two
different resolutions. They MUST stay numerically consistent: any number
that appears in both must agree to within the documented tolerance.

Modules under [`../../module/`](../../module/) extract specific cross-sections
(engineering pack / impact ladder) for downstream readers and stay in sync
with `domain.md` as the source of truth.

## When to read which

- **Just want to understand what this is?** → [`spec.md`](spec.md) §1 WHY + §2 COMPARE.
- **Building it?** → [`domain.md`](domain.md) §0–§20, then [`../../module/engineering_pack/`](../../module/engineering_pack/README.md).
- **Pitching it / planning roadmap?** → [`../../module/impact/`](../../module/impact/README.md).

## Doc subfolder

[`doc/`](doc/) is reserved for per-feature deep dives that do not belong in
either canonical spec (e.g. derivation walkthroughs, vendor evaluation memos).
Currently empty — populate as needed.
