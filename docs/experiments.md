# Detailed Experiments

[Back to the project overview](../README.md)

All values on this page are transcribed from [TempoFit, arXiv:2603.07647v1](https://arxiv.org/pdf/2603.07647v1), submitted on March 8, 2026. They are paper-reported results, not fresh evaluations. Success rates are percentages; changes in success rate are **percentage points (pp)**. Different backbones, checkpoints, training regimes, and evaluation settings should not be treated as matched controls.

## Evaluation Setup

| Evaluation | Setup reported in the paper |
| :--- | :--- |
| LIBERO-LONG | 10 tasks, 50 trials per task, 500 trials per paired backbone/configuration |
| CALVIN D-D | Policies trained on environment D, evaluated on held-out instruction sequences in D |
| CALVIN ABC-D | Policies trained on A/B/C, evaluated in unseen environment D |
| Simulation observations | Primary and wrist cameras |
| Standard history capacity | 8 frames; varied separately in efficiency and capacity ablations |
| Backbones | RLinf pi0.5 and StarVLA QwenGR00T; additional comparison checkpoints as listed below |
| Efficiency hardware | NVIDIA RTX 5090 |
| Real robot | Realman RM-65B; Orbbec 336L third-person camera and a USB wrist camera |
| Real-world data and baseline | 100 demonstrations per task for three tasks; task-specific pi0.5 baseline training is reported |

The temporal retrofit itself requires no additional training. The paper does not establish a released software API, installation procedure, or reproducible command line for this repository; those remain part of the upcoming code release.

## LIBERO-LONG

### Paired Backbone Comparison

Source: Table I. The following four configurations are the within-backbone comparisons (50 trials per task).

| Task | QwenGR00T | + TempoFit | pi0.5 | + TempoFit |
| :--- | ---: | ---: | ---: | ---: |
| Put soup and box in basket | 88.0 | 100.0 | 100.0 | 100.0 |
| Put box and butter in basket | 94.0 | 98.0 | 96.0 | 100.0 |
| Turn on stove and put pot | 100.0 | 100.0 | 98.0 | 98.0 |
| Put bowl in drawer and close | 98.0 | 100.0 | 96.0 | 98.0 |
| Put mugs on left and right plates | 96.0 | 100.0 | 96.0 | 100.0 |
| Pick book and place it in back | 100.0 | 98.0 | 100.0 | 100.0 |
| Put mug on plate, pudding right | 68.0 | 80.0 | 96.0 | 96.0 |
| Put soup and sauce in basket | 100.0 | 92.0 | 90.0 | 96.0 |
| Put both pots on stove | 66.0 | 88.0 | 58.0 | 84.0 |
| Put mug in microwave and close | 98.0 | 88.0 | 96.0 | 96.0 |
| **Paper-reported average (%)** | **90.8** | **94.4** | **92.6** | **96.6** |

Paper-reported average gains are **+3.6 pp** for QwenGR00T and **+4.0 pp** for pi0.5. Some QwenGR00T task scores decrease; the aggregate improvement is not a claim of improvement on every task.

**Source-table note:** The ten printed pi0.5 + TempoFit task scores average to **96.8%**, while Table I and the paper's headline report **96.6%**. Both are transcribed as published; the README uses the reported 96.6% rather than silently replacing the headline or altering a task score. Resolving this difference requires the underlying evaluation records.

### Broader Comparison

The following average results are reproduced from Table I for context. Rows from other methods are not controlled ablations of TempoFit: their architectures, checkpoints, and training procedures differ.

| Method | Average success (%) |
| :--- | ---: |
| Seer (scratch) | 78.7 |
| Seer | 87.7 |
| UniVLA | 90.0 |
| OpenVLA-OFT | 94.0 |
| MemoryVLA | 93.4 |
| HiF-VLA | 96.4 |
| QwenGR00T | 90.8 |
| QwenGR00T + TempoFit | 94.4 |
| pi0.5 | 92.6 |
| pi0.5 + TempoFit | 96.6 |

## CALVIN

Source: Table II. Columns 1-5 report success (%) for completing consecutive instruction prefixes of the specified length. Average length is the paper-reported average number of successfully completed tasks, out of five. The paper does not specify the number of evaluation sequences in this table or the accompanying experimental setup.

| Setting | Method | 1 | 2 | 3 | 4 | 5 | Avg. length |
| :--- | :--- | ---: | ---: | ---: | ---: | ---: | ---: |
| D-D | pi0 | 84.8 | 70.4 | 55.9 | 46.6 | 37.7 | 2.95 |
| D-D | QwenPI | 90.9 | 79.5 | 69.6 | 62.2 | 55.4 | 3.58 |
| D-D | QwenGR00T | 92.5 | 83.9 | 74.4 | 67.9 | 59.8 | 3.78 |
| D-D | QwenGR00T + TempoFit | 92.0 | 83.8 | 75.7 | 70.3 | 62.3 | 3.84 |
| ABC-D | pi0.5 | 93.2 | 84.6 | 76.7 | 68.8 | 61.4 | 3.83 |
| ABC-D | pi0.5 + TempoFit | 93.0 | 84.8 | 77.3 | 69.4 | 62.0 | 3.87 |

The average-length gains are **+0.06** (D-D) and **+0.04** (ABC-D), as reported. Five-instruction success increases by **+2.5 pp** and **+0.6 pp**, respectively. Early-instruction scores do not uniformly improve.

**Source-table note:** For the ABC-D pi0.5 baseline, summing the printed prefix-success probabilities gives 3.847 completed tasks, whereas the reported average length is 3.83. We retain the reported average and do not infer a corrected value without the evaluation records.

## Inference Efficiency

Source: Table III. Values use pi0.5 on LIBERO-LONG with an NVIDIA RTX 5090. Latency and peak memory are measured per timestep and averaged within the episode. The table below preserves absolute measurements rather than recomputing the source's rounded relative factors.

| Method | History length | Latency (ms) | Peak memory (MB) |
| :--- | ---: | ---: | ---: |
| Single-frame pi0.5 | 1 | 71.2 | 6,396 |
| Multi-frame input | 4 | 94.8 | 22,640 |
| TempoFit | 4 | 73.4 | 6,498 |
| Multi-frame input | 8 | 176.3 | 45,980 |
| TempoFit | 8 | 74.4 | 6,600 |
| TempoFit | 16 | 81.4 | 6,761 |
| TempoFit | 32 | 86.8 | 7,030 |

At history length 8, the difference from the single-frame baseline is **3.2 ms** and **204 MB**. These measurements do not describe training compute, separate retrieval-only timing, or sensor-to-actuator latency on the real robot.

## Ablations

Source: Table IV. pi0.5 on LIBERO-LONG, average success over 500 trials per configuration. Rows belong to separate ablation groups; they are not a sequence of cumulative modifications.

| Group | Configuration | Average success (%) |
| :--- | :--- | ---: |
| Baseline | No memory | 92.6 |
| Components | KV memory only | 93.8 |
| Components | KV memory + FGTB | 96.6 |
| Retrieval | Q-to-K retrieval | 93.3 |
| Retrieval | K-to-K retrieval | 96.6 |
| Injection | Concatenation | 0.8 |
| Injection | Residual loading without norm preservation | 90.2 |
| Injection | Residual loading with norm preservation | 96.6 |
| Layers | All layers (0-17) | 74.2 |
| Layers | Layers 9-17 | 59.8 |
| Layers | Layers 0-8 | 89.4 |
| Layers | Selected intermediate layers | 96.6 |
| Capacity | 4 frames | 95.2 |
| Capacity | 8 frames | 96.6 |
| Capacity | 16 frames | 96.2 |
| Capacity | 32 frames | 95.2 |

The paper does not enumerate the exact selected intermediate layer IDs or give a complete numerical inference configuration. We therefore do not supply guessed defaults for layer selection or the FGTB decay strength. The layer-range labels above use explicit indices instead of the paper's potentially ambiguous "top" and "bottom" terminology.

## Real-World Manipulation

Source: Figure 3 and Section IV-D. The paper reports 100 demonstrations per task and task-specific pi0.5 baseline training, but does not fully specify the real-world training and retrofit protocol. The table preserves **Figure 3's printed stage-wise percentages**, including its rounding.

| Task | Policy | Stage 1 (%) | Stage 2 (%) | Stage 3 (%) |
| :--- | :--- | ---: | ---: | ---: |
| Place objects in order | pi0.5 | 95.2 | 76.2 | 57.1 |
| Place objects in order | + TempoFit | 95.2 | 85.7 | 66.7 |
| Clean desk | pi0.5 | 100.0 | 80.9 | N/A |
| Clean desk | + TempoFit | 95.2 | 85.7 | N/A |
| Organize bowls | pi0.5 | 80.9 | 80.9 | 66.7 |
| Organize bowls | + TempoFit | 85.7 | 80.9 | 80.9 |

**Task sequences:** place three vegetables into a tray in order; dispose of tissue and then place a pepper into a box; put both green bowls into a drawer and close it. Stage 2 is the final stage of the two-stage desk-cleaning task; "N/A" is not a failure or a zero.

### Reporting Notes

- Section IV-D states **20 evaluation trials per task**, but printed values such as 95.2% and 57.1% are not integer-success proportions out of 20. Without raw episode records, the denominator cannot be reconciled. We retain the published percentages and do not infer counts or uncertainty intervals.
- Figure 3 prints **80.9%** where the prose sometimes uses **81.0%**. Prose also reports gains of +9.5, +4.8, and +14.3 pp and an average gain of +9.5 pp. We use the figure's endpoints without substituting reconstructed counts or presenting rounded differences as exact.
- Figure 1 includes a "Real World" aggregate bar, but its aggregation is not defined well enough to equate it with final-stage success across the three tasks. The README therefore highlights the explicitly labeled task-level results instead.
- All paper figures are preserved unchanged. These notes explain source-level reporting differences; they are not corrections to the original measurements.

## Reproducibility Boundary

The paper reports results for existing checkpoints and a training-free temporal addition. This documentation does not constitute an independent reproduction. Exact memory-layer indices, full hyperparameters, environment setup, seed/episode manifests, code, and raw evaluation logs should accompany a future reproducible release. No statistical significance claim is made from the reported point estimates alone.
