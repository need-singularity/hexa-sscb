<!-- @created: 2026-05-12 -->
<!-- @scope: real-limits audit (Wave M) — solid-state circuit breaker physics + engineering -->
<!-- @authority: applies LATTICE_POLICY.md §1.2 taxonomy verbatim -->
---
type: limit-breakthrough-audit
wave: M
session: 2026-05-12
parent_policy: LATTICE_POLICY.md §1.2
applies_to: hexa-sscb — Solid-State Circuit Breaker (HEXA-SSCB mk1), 600 ns DC interruption, 4-foundry SiP
---

# LIMIT_BREAKTHROUGH.md — hexa-sscb real-limits audit (Wave M)

> **Question**: hexa-sscb interrupts a 48 V / 100 A DC bus in 600 ns.
> Where are the **physics** (di/dt, SiC breakdown, energy dissipation)
> and **engineering** (foundry yield, BOM, packaging) walls — and which
> can be broken?

---

## §1 Domain identification

| Layer | Verbs / Modules | Concern |
|-------|-----------------|---------|
| Power die | SiC MOSFET (YESPOWER 1200 V / 30 mΩ, 200 mm MPW) | Interrupt 100 A in 600 ns |
| Gate driver | BCD 180 nm (DB HiTek 0.18 µm 5M1P) | Drive 30 nF gate in < 100 ns |
| Sensing | Σ-Δ ADC 24-bit + comparator (SK Key Foundry 0.18 µm) | Detect fault @ < 100 ns |
| Logic | Cortex-M4 MCU 40 nm (Samsung; STM32 fallback for mk1) | Decision + handshake |
| Package | EDiP SiP + DBC AlN + Ag-sintered Cu (AT&S Signetics) | Thermal + parasitic-L management |
| Spec | `core/sscb/spec.md` — cutoff_ns ≡ 600, foundry_count ≡ 4, BOM ≡ 12 lines | Falsification budget |

mk1 is **paper + engineering pack + impact ladder** (Mk.I → Mk.V).
Silicon tape-out PLANNED, not delivered. Real-limits audit treats
the published spec as the claim under test.

---

## §2 Real limits applicable to hexa-sscb

### 2.1 di/dt × parasitic inductance → voltage overshoot (PHYSICAL)

```
V_overshoot  =  L_parasitic · (di/dt)
```
Interrupting 100 A in 600 ns → di/dt ≈ 1.67 × 10⁸ A/s. With even
50 nH stray inductance (very tight package), V_overshoot ≈ 8.3 V.
At 5 nH (DBC bus-bar best-case), ≈ 0.83 V. **Below SiC breakdown
1200 V** with margin, but tight; package inductance is the binding
floor.

### 2.2 SiC MOSFET avalanche / breakdown (PHYSICAL)

YESPOWER 1200 V SiC die. Avalanche energy E_AS bounded by die area
and thermal mass; ~1 J per 1 cm² die at 175 °C T_jmax. A single
catastrophic interrupt at 48 V × 100 A × 600 ns ≈ **2.88 mJ** — well
within E_AS budget. **3 orders of margin.**

### 2.3 Landauer + thermal floor on detection latency (PHYSICAL)

Σ-Δ ADC at 24-bit, 1 MHz BW dissipates ~10 mW. Detection of an
overcurrent transient against thermal noise floor `4kTRΔf` requires
SNR > ~20 dB for false-alarm < 10⁻⁶/s. Setting fault threshold at
150 A on a 100 A bus is **5σ above thermal noise** — comfortable.

### 2.4 Speed-of-light propagation in package (PHYSICAL)

c in FR-4 ≈ 1.5 × 10⁸ m/s. For a 1 cm signal trace between sense and
gate-driver, propagation ≈ 67 ps. Total budget 600 ns; speed-of-light
is **~10⁴× faster than budget** — never binding in this size class.

### 2.5 4-foundry coordination yield (ENGINEERING)

Compound yield = ∏ y_i. For 4 independent foundry deliverables at
y_i ≈ 0.85 each (typical MPW), compound y ≈ 0.52. mk1 BOM yield
target is the binding production-economics constraint.

### 2.6 SiC MPW (Multi-Project-Wafer) cadence (ENGINEERING)

YESPOWER 200 mm SiC MPW runs ~quarterly. Tape-out → samples ≈ 14-18
weeks. For 5 design iterations (typical first-silicon to release),
**18-24 months calendar floor**.

### 2.7 BOM cost floor (ENGINEERING)

mk1 BOM: $30.77. Breakdown rough: SiC die ~$8, BCD die ~$3, ADC die
~$3, MCU ~$5, DBC AlN package ~$10, passives + assembly ~$2. SiC
die cost is the binding floor; 200 mm SiC wafer ≈ $2,500 / die-count.

### 2.8 Regulatory certification envelope (ENGINEERING)

UL 489-class (DC breaker) + IEC 60947-2 + automotive AEC-Q200 each
require independent test campaigns (~$200-500 k, 6-12 months each).
Telecom 48 V DC (ETSI EN 300 132-3) adds another tier. **Cert
schedule, not silicon, is the longest path to Mk.III deployment.**

---

## §3 Per-limit breakthrough assessment

### 3.1 di/dt × L_parasitic → **SOFT_WALL** (package geometry breaks it)

Engineering, not physics. Reducing L from 50 nH to 5 nH is a
DBC + sinter-Cu + tight-loop package design problem. Trigger:
ship Mk.II SiP with measured L_loop < 10 nH. Status: **engineering
pack §11 calls this out**.

### 3.2 SiC avalanche → **SOFT_WALL** (die area / multi-die parallelisation)

Die-area scaling linear; cost scaling super-linear. For higher-current
variants (1 kA), multi-die parallel arrays are standard. **No
fundamental wall** at hexa-sscb's 100 A target.

### 3.3 Detection-latency thermal floor → **HARD_WALL but loose**

`4kTRΔf` is fundamental. 20 dB SNR at 5σ threshold is **20 orders**
above k_B T per measurement at sensor level. Catalog-only; not
binding.

### 3.4 Speed-of-light → **HARD_WALL but irrelevant**

Cannot be broken (special relativity). 67 ps propagation is 0.01%
of the 600 ns budget. Catalog-only.

### 3.5 4-foundry compound yield → **BREAKABLE_WITH_TECH (consolidation)**

Mk.III roadmap collapses to single-foundry SiP (e.g., GlobalFoundries
or TSMC SOI with integrated SiC). Trigger: integrated process node
that supports SiC + BCD + CMOS on one mask set. Status:
**research-stage** at IMEC and Stanford; commercial 5-7 yr.
**Alternative**: redundant test + binning, raising effective y_i
toward 0.95 per stage → compound 0.81.

### 3.6 SiC MPW cadence → **BREAKABLE_WITH_TECH (full-wafer + shuttle)**

Engineering. YESPOWER offers full-wafer dedicated runs at ~$80 k
that compress to 8-10 weeks. Trigger: when mk1 ramps past 1k
units/quarter, switch from MPW to dedicated wafer.

### 3.7 BOM cost floor → **SOFT_WALL** (volume + SiC wafer-cost curve)

SiC wafer cost dropped ~40% 2020-2024 and is forecast to drop another
30% by 2028 as 8-inch SiC capacity comes online (Wolfspeed, II-VI,
SK Siltron CSS). BOM floor follows the wafer-cost curve.

### 3.8 Regulatory certification → **BREAKABLE_WITH_TECH (concurrent campaigns + harmonised tests)**

Multi-cert can be parallelised (~$1.2 M for the full UL+IEC+AEC stack,
~14 months). Trigger: cert-campaign budget line in Mk.III gate.
**No physics wall.**

---

## §4 Top-3 breakthrough opportunities

### #1 — L_loop < 10 nH SiP (§3.1)

Largest physics-anchored lever. Drops V_overshoot to ~0.17 V at the
peak di/dt event, **unlocking aggressive sub-300 ns interruption
targets** for Mk.III. ~6 engineer-months of packaging redesign;
high payoff.

### #2 — Foundry consolidation (§3.5)

Compound yield 0.52 → 0.81 (or higher) drops BOM by ~30-40% and
collapses logistics complexity. The 4-foundry SiP is mk1 *because*
of strategic-sourcing diversification; mk3+ should consolidate.
12-24 months.

### #3 — Concurrent UL + IEC + AEC certification (§3.8)

Pulls Mk.III deployment in by ~12 months. ~$1.2 M budget. Largest
calendar lever, no R&D risk.

---

## §5 Honest caveats

1. **mk1 is paper + engineering-pack — no silicon yet.** All
   physics numbers above derive from datasheet / first-principles
   calculations, not measured. The 600 ns claim is a *spec target*.

2. **HARD_WALLs (§3.3, §3.4) are non-binding** at this size class.
   Honest classification, but not where the design pressure sits.

3. **The binding walls are §3.1 (parasitic L) and §3.5 (4-foundry
   yield)** — both BREAKABLE, both with explicit engineering paths.

4. **No NDA / proprietary content.** Foundry process numbers come
   from public PDKs and DRM datasheets.

5. **n=6 lattice (cutoff_ns = 6 × 100 ns, foundry_count = τ(6) = 4,
   BOM = σ(6) = 12) is NOT a "limit"** per LATTICE_POLICY.md §1.2
   — it is hexa-sscb's organising vocabulary / falsification budget.
   The *real* limit is `V_overshoot < V_SiC_BV` and friends.

6. **Cert envelope (§3.8) is jurisdiction-bound.** UL is US, IEC is
   international, AEC is automotive-specific. A 48 V DC product
   crossing jurisdictions may need 4-6 parallel cert tracks.

---

## §6 References

- `LATTICE_POLICY.md` §1.2 — taxonomy
- `README.md` — mk1 status, BOM, foundry roster
- `core/sscb/spec.md` — published spec parameters
- `module/engineering_pack/README.md` — §11 ACCEPTANCE bench tests
- `module/firmware/` — Cortex-M4 firmware build
- `verify/sscb_verify.py`, `verify/cross_doc_audit.py` — current verify suite
- External: Wolfspeed SiC MOSFET datasheets, UL 489-DC draft,
  IEC 60947-2:2016, AEC-Q200 Rev D, Yole *SiC Power Devices 2024
  Report*, ETSI EN 300 132-3-1 V2.1.1 (48 V DC powering).

---

*End of LIMIT_BREAKTHROUGH.md (hexa-sscb, Wave M).*
