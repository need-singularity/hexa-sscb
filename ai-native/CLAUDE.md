# sscb — AI-agent scope (CLAUDE.md)

> Repo-local AI-agent context. The canonical AI-native readme is
> [`../README.ai.md`](../README.ai.md); this file holds agent-specific
> harness notes that don't belong in the public-facing handoff.

## Identity

This repository is an **extracted standalone form** of the SSCB compute
domain from `n6-architecture`. Source-of-truth still lives at
`n6-architecture/domains/compute/sscb/sscb.md`; this repo is the public
distribution form following hive raw.mk2 arch.001 (`core/<feature>/` +
`module/<feature>/` + `README.ai.md` triplet).

## Scope guard (must not violate)

- This repo is **doc-first**. There is no Python / Rust / C code yet. Do
  not introduce code without first updating [`../README.md`](../README.md)
  and [`../README.ai.md`](../README.ai.md) module inventory.
- Do not retranslate the Korean predecessor under
  [`../doc/archive/sscb_mk1_predecessor_korean.md`](../doc/archive/sscb_mk1_predecessor_korean.md) —
  it is preserved verbatim as forensic continuity.
- Do not edit `*.md.bak` files in `doc/archive/` — these are immutable
  snapshots from the n6-architecture extraction.

## Extraction lineage

- 2026-05-06: extracted from n6-architecture
- Two source markdowns (`sscb_bak.md`, `sscb_engineering_pack_bak.md`) were
  recovered from the parent of n6-architecture commit `a489a368`
  (deletion: 2026-04-21).
- Full lineage: [`../doc/lineage/origin.md`](../doc/lineage/origin.md).

## When syncing back to n6-architecture

If a change here should propagate upstream, the upstream file mapping is:

| This repo | n6-architecture path |
|---|---|
| `core/sscb/spec.md` | `papers/sscb-mk1-2026-05-04.md` |
| `core/sscb/domain.md` | `domains/compute/sscb/sscb.md` |
| `module/engineering_pack/README.md` | (deleted; recover from history if needed) |
| `module/impact/README.md` | `domains/compute/sscb/sscb.md` §21+§22 |
