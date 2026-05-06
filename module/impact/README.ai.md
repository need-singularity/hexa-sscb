# module/impact — AI-native handoff

> AI agents: this module is the **Mk-ladder roadmap** for HEXA-SSCB.
> Conformance: hive raw 271 (readme-ai-native-mandate).

## Purpose

[`README.md`](README.md) holds §21 IMPACT (per-Mk what-changes ladder) and
§22 (reduction to two practical questions), extracted verbatim from
[`../../core/sscb/domain.md`](../../core/sscb/domain.md).

The Mk ladder runs reverse-chronologically (Mk.V at the top, Mk.I at the
bottom) so the latest plan is the first thing a reader sees.

## Mk inventory

| Mk | Date | Status | Capability delta |
|---|---|---|---|
| Mk.V | 2030-06-01 | PLANNED | GaN complement (1.2 kV) + AI cutoff prediction |
| Mk.IV | 2029-06-01 | PLANNED | 100% domestic sourcing (SiC die domesticated) |
| Mk.III | 2028-06-01 | PLANNED | HVDC data-center (±400 V class) |
| Mk.II | 2027-06-01 | PLANNED | Auto-reclosure + bidirectional |
| Mk.I  | 2026-04-18 | RELEASED (paper) | 48 V / 100 A DC unidirectional, manual re-close |

## Edit invariants

1. **Monotonicity** — capability and date must increase Mk.I → Mk.V. A new
   Mk.X cannot regress capability below an earlier Mk without a documented
   "fork" (which then becomes a separate ladder).
2. **§21 / §22 sync** — this module's `README.md` is regenerated from
   `core/sscb/domain.md` §21–§22. Do not edit here without propagating
   to `domain.md` first.
3. **Three-tier structure** in each Mk entry must remain: ① what changes,
   ② why now, ③ acceptance / falsifier. The `<details>` collapse structure
   was added in commit `068e84da` and is part of the contract.
