# module/engineering_pack — AI-native handoff

> AI agents: this module is the **build hand-off** for HEXA-SSCB mk1.
> Conformance: hive raw 271 (readme-ai-native-mandate).

## Purpose

A receiving engineer should be able to start a tape-out / PCB rev-A / firmware
bring-up directly from [`README.md`](README.md). Every numeric claim is
derivable; the §12 appendix re-derives every dimensioned number using only
the Python `stdlib`.

## Source of truth

Numbers in this module MUST agree with [`../../core/sscb/domain.md`](../../core/sscb/domain.md).
If they diverge, `domain.md` wins and this module is updated.

## Sections (per `README.md`)

- §0 EXEC SUMMARY — one-page summary table
- §1 SYSTEM REQUIREMENTS — quantitative requirements (electrical / thermal / EMC / safety)
- §2–§7 ARCHITECTURE / CIRCUIT / PCB / FIRMWARE / MECHANICAL / MANUFACTURING
- §8 TEST — bench plan
- §9 BOM — σ(6)=12 line-item lattice with $31.50 / 1k target
- §10 VENDOR — 4-foundry domestic stack assignment
- §11 ACCEPTANCE — 10-item PASS/FAIL sign-off
- §12 APPENDIX — Python `stdlib` re-derivation of every number

## Edit invariants

1. σ(6) = 12 BOM lattice must remain **exactly 12 lines**. Adding a 13th
   line forces splitting an existing module or violates the n=6 contract.
2. The 4-foundry domestic-stack assignment is contractual. Do not silently
   add a 5th foundry or substitute a foreign foundry in the assignment table.
3. Every $-figure must reconcile with the §12 appendix derivation.
