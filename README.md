# VEGFA viewers

Interactive 3D structure viewers comparing **Evo2-7B** (Arc Institute), **Carbon-3B / Carbon-8B** (HuggingFaceBio), and **ProtGPT2** (Ferruz et al.) on VEGFA generation, with ESMFold v1 (Meta) for folding and `tmtools` for TM-alignment.

## 🌐 Live site

**https://pphouse.github.io/vegfa-viewers/**

| Page | URL |
|---|---|
| Index (hub) | https://pphouse.github.io/vegfa-viewers/ |
| Compare (2-pane) | https://pphouse.github.io/vegfa-viewers/compare.html |
| Experiment 1 — Evo2 upstream ORF | https://pphouse.github.io/vegfa-viewers/upstream-orf.html |
| Experiment 2 — Evo2 CDS continuation | https://pphouse.github.io/vegfa-viewers/cds.html |
| Experiment 3 — Carbon-8B + ProtGPT2 | https://pphouse.github.io/vegfa-viewers/carbon-protgpt2.html |

Each viewer page accepts `?compact=1` (used by `compare.html` to hide non-essential sidebar cards for side-by-side viewing).

## Files

- `index.html` — landing page with experiment cards
- `compare.html` — left/right iframe layout with experiment dropdowns + swap button
- `upstream-orf.html` — Experiment 1: 7 models (Evo2 ORFs + VEGFA P15692 PRED + 1VPF crystal)
- `cds.html` — Experiment 2: 5 models (Evo2 CDS continuations + VEGFA real + 1VPF crystal)
- `carbon-protgpt2.html` — Experiment 3: 14 models (Carbon-8B upstream ORF · Carbon-8B CDS · ProtGPT2 + 2 references)
- `.nojekyll` — bypass Jekyll on GitHub Pages so raw HTML serves verbatim

Every viewer embeds its PDB structures inline (pre-aligned via TM-align), so the pages are self-contained — 1–3 MB per viewer with no runtime fetches beyond the 3Dmol.js CDN.

## Quick read of the viewers

- **pLDDT** (0–1): ESMFold per-residue confidence. **> 0.7** well-folded, **< 0.5** disordered.
- **TM-score**: **> 0.5** same fold · **0.3–0.5** partial · **< 0.3** unrelated.
- **VEGFA reference ceiling**: 1VPF crystal (RBD) vs ESMFold of the 412-aa precursor gives TM = **0.226**. Anything below that is structurally **not** VEGFA.

## Bottom line

No genomic/protein foundation model recreated VEGFA (all generated TM-score ≲ 0.31 vs crystal). ProtGPT2 produces the most plausible *generic* protein structures (pLDDT 0.48), but not specifically VEGFA. Carbon-3B/8B is ~6× faster than Evo2-7B at this scale but is strand-sensitive — naive reverse-complement sampling collapses to ~98% GC and needs the **FNS branch** or **metadata + repetition penalty** to remain clean.

## Citations

- **Brixi et al., 2025** — *Genome modeling and design across all domains of life with Evo 2* (Arc Institute)
- **Ben Allal et al., 2026** — *Carbon: Decoding the Language of Life* (Hugging Face + TIGEM + Zhongguancun Academy)
- **Ferruz et al., 2022** — *ProtGPT2 is a deep unsupervised language model for protein design* (Nat. Commun.)
- **Lin et al., 2023** — *Evolutionary-scale prediction of atomic-level protein structure (ESMFold)* (Meta AI)

VEGFA reference: UniProt P15692 · RefSeq NM_001025366.3 · GRCh38 `chr6:43,770,209–43,786,487` · RCSB 1VPF chain A.

Built on a single NVIDIA A100 80 GB.
