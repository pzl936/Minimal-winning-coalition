# Minimal Winning Coalitions for Cognitive Probing of HRRP Recognition Networks

A sufficiency-oriented way of probing what a radar HRRP recogniser actually needs in order
to commit to a decision.

## What is in here

```
figures/     vector PDFs of the results
README.md    this file
```

Every figure is a vector PDF with embedded Times New Roman, sized to the IEEE two-column
grid: **3.5 in** for a single column and **7.16 in** for a full-width figure.

## The idea in one paragraph

Deep recognisers for radar high-resolution range profiles (HRRP) are accurate but opaque.
Most attribution methods ask a *necessity* question — which features, if removed, degrade
the prediction. This work asks the complementary *sufficiency* question: what is the
smallest subset of range bins whose amplitudes alone drive the softmax confidence of the
correct class above a threshold? Borrowing from cooperative game theory, that subset is a
**minimal winning coalition (MWC)** and each range bin is a player. The exact MWC is
NP-hard, so the analysis uses an integrated-gradients (IG) guided forward derivation
(fMWC) and a per-bin backward derivation (bMWC), quantifies how far the cheap search sits
from a true greedy optimum, and reports every coalition size as a certified **upper bound**
rather than a minimum.

Four read-outs follow:

- **Orphan effect** — spatially isolated bins cause non-monotonic confidence drops as a
  coalition is built, reflecting how much an architecture relies on local pattern
  continuity.
- **Cognitive asymmetry** — a cross-model transfer matrix shows that some recognisers
  export coalitions they would not themselves accept.
- **Sufficiency–maintenance gap** — creating a decision takes markedly fewer modulated bins
  than defending against one.
- **Physical bound** — re-synthesising a coalition through a DRFM and a real matched filter
  costs a measurable, quantified share of its ideal success rate.

## Recognition architectures

Four architecturally diverse recognisers, probed on 11 aircraft classes, all exceeding
99.7% test accuracy:

| Model | Type | First-layer extent | Receptive field |
|---|---|---|---|
| ResNet-18 | 1-D residual CNN | kernel 7 | ≈ 32 |
| ViT | vision transformer | patch 16 | 400 (global attention) |
| MediumCNN | 5-layer CNN | kernel 7 | ≈ 63 |
| BiLSTM | 2-layer bidirectional LSTM | 1 bin | 400 (full recurrence) |

## Figures

| File | What it shows |
|---|---|
| `figures/figR_framework.pdf` | Framework flowchart: the recognition problem, exact vs. approximate coalition search, the probing read-outs, and validation. |
| `figures/figR_scalability.pdf` | (a) Per-stage per-sample cost of the probing protocol. (b) Mean coalition size against the number of samples evaluated. |
| `figures/figR_orphan_sensitivity.pdf` | (a) Orphan rate as a function of the spatial-isolation threshold. (b) Observed rate against a spatially permuted null model. |
| `figures/fig3a_orphan_hrrp.pdf` | An HRRP with the active coalition and a newly added orphan bin. |
| `figures/fig3b_orphan_curve.pdf` | The resulting confidence drop, against a measured adjacent-bin counterfactual. |
| `figures/fig3_transfer_matrix.pdf` | The 4 × 4 cross-model transfer matrix. |
| `figures/figC_asymmetry_bridge.pdf` | Coalition structure behind the asymmetry: MWC bin patterns and the corresponding export/import rates. |
| `figures/figR_redundancy.pdf` | The sufficiency–maintenance gap: distribution of *R*, and forward vs. backward confidence at matched coalition size. |
| `figures/figR_drfm.pdf` | (a) Attack success rate under ideal gating, DRFM synthesis, and full-profile synthesis. (b) Peak-to-valley ratio of two compressed impulses against their separation. |
| `figures/figR_robustness.pdf` | Degradation under five impairments: additive noise, clutter in the unmodulated bins, per-bin calibration error, global gain error, and aspect uncertainty. |

## Reproducing the figures

The figures are regenerated from stored experiment outputs by a single entry-point script
that applies one shared style module: exact column widths, Times New Roman at 9/8 pt,
embedded TrueType fonts, no in-figure titles, and panel labels placed outside the top-left
corner of each subplot.

```bash
python make_ieee_figures.py
```

## License

Released under the MIT License. See `LICENSE`.
