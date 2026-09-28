<div align="center">

# BiCENet

### A Dual-Branch Chebyshev Tone-Curve Network for Low-Contrast Image Enhancement

[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c.svg)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

</div>

---

## Overview

Low contrast is a degradation distinct from low light, and methods built for
illumination attenuation do not correct it directly. **BiCENet** estimates
global and local tone-curve coefficients over a shared Chebyshev basis and
fuses them through a per-pixel gate.

- **Global branch** — a Kolmogorov-Arnold (KAN) head regresses one tone curve
  per colour channel for the whole image.
- **Local branch** — a multi-scale encoder with CBAM attention predicts a
  tone curve at every pixel.
- **Adaptive fusion** — a learned per-pixel gate blends the two corrections.
- **CDCF loss** — supervises luminance contrast, chrominance fidelity and
  gradient direction in the YCbCr domain.

## Model

| Property | Value |
| :--- | :--- |
| Parameters | 997,749 |
| GFLOPs | 282.71 (at 400 x 600) |
| Framework | PyTorch |

## Training settings

| Setting | Value |
| :--- | :--- |
| Optimizer | Adam, lr 1e-4, cosine annealing |
| Epochs | 240 |
| Batch size | 8 |
| Training crop | 320 x 320 |
| Gradient clipping | L2 norm 1.0 |

## Pretrained model

`model.pth` is the checkpoint used for every result reported in the paper.

## Availability

This repository currently hosts the pretrained model. The training and
inference code will be released here once the paper is accepted.

**LCDataset** contains 533 ground-truth images, each degraded at three
severity levels, giving 1,599 paired training images and a held-out test set
of 90 images. Both the dataset and the source code are available for academic
use on request. Please write to
[202491003@iiitvadodara.ac.in](mailto:202491003@iiitvadodara.ac.in) with your
name, affiliation and intended use.

## Citation

```bibtex
@article{singh2026bicenet,
  author  = {Singh, Gaurav and Paul, Abhisek and Singh, Dharmendra},
  title   = {BiCENet: A Dual-Branch Chebyshev Tone-Curve Network for
             Low-Contrast Image Enhancement},
  journal = {},
  year    = {2026}
}
```

## Contact

Gaurav Singh — [202491003@iiitvadodara.ac.in](mailto:202491003@iiitvadodara.ac.in)
Indian Institute of Information Technology Vadodara

## License

MIT. See [LICENSE](LICENSE).
