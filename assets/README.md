# Paper Figures

These assets come from the author-provided `TempoFit_IROS_2026 (1).zip` source archive. The selected files are those referenced by `ieeeconf/arxiv.tex` and correspond to Figures 1-3 of [arXiv:2603.07647v1](https://arxiv.org/pdf/2603.07647v1). Alternate draft figures and unused source files are not included.

| Paper figure | Description | Web preview | Original PDF | Archive source |
| :--- | :--- | :--- | :--- | :--- |
| Figure 1 | Motivation and overview | [PNG](figures/overview.png) | [PDF](pdf/overview.pdf) | `ieeeconf/Image/overview.pdf` |
| Figure 2 | Layer-wise temporal KV memory pipeline | [PNG](figures/method.png) | [PDF](pdf/method.pdf) | `ieeeconf/Image/methodnnnfinal.pdf` |
| Figure 3 | Real-robot setup and stage-wise evaluation | [PNG](figures/real-world.png) | [PDF](pdf/real-world.pdf) | `ieeeconf/Image/experiment.pdf` |

The PDFs are unchanged copies. PNGs are full-page renders with Poppler, without cropping, redrawing, or changing the reported numbers. The overview uses a 2,400-pixel maximum dimension; the method and real-world figures use 2,800 pixels.

To regenerate the previews from the repository root with Poppler installed:

```bash
pdftoppm -png -singlefile -scale-to 2400 assets/pdf/overview.pdf assets/figures/overview
pdftoppm -png -singlefile -scale-to 2800 assets/pdf/method.pdf assets/figures/method
pdftoppm -png -singlefile -scale-to 2800 assets/pdf/real-world.pdf assets/figures/real-world
```

Figures retain the original paper's labels and rounding. For precise metric definitions, use the [experimental tables and reporting notes](../docs/experiments.md), rather than comparing unlabeled summary bars across different benchmarks.
