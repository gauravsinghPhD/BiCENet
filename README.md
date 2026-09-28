<div align="center">

# BiCENet

### A Dual-Branch Chebyshev Tone-Curve Network for Low-Contrast Image Enhancement

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
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

The model uses **997,749 parameters** and **282.71 GFLOPs** at 400x600.

## Installation

```bash
git clone https://github.com/gauravsinghPhD/BiCENet.git
cd BiCENet
pip install -r requirements.txt
```

## Quick start

Enhance a single image with the released weights:

```python
from BiCENet import load_checkpoint, enhance_image_file

model, _ = load_checkpoint('model.pth', local_dc=False)
enhance_image_file(model, 'input.png', 'output.png')
```

## Training

```python
from torch.utils.data import DataLoader
from BiCENet import ContrastDataset, build_model, train_model, set_seed

set_seed(0)

dataset = ContrastDataset('data/train/low', 'data/train/gt',
                          crop_size=320, augment=True)
loader  = DataLoader(dataset, batch_size=8, shuffle=True)

model, device = build_model(local_dc=False)
train_model(model, loader, num_epochs=240, lr=1e-4, device=device,
            checkpoint_dir='checkpoints')
```

Place paired images with matching filenames in the two folders:

```
data/
  train/low/    train/gt/
  test/low/     test/gt/
```

## Evaluation

```python
from torch.utils.data import DataLoader
from BiCENet import ContrastDataset, evaluate_dataset

loader = DataLoader(ContrastDataset('data/test/low', 'data/test/gt',
                                    crop_size=320), batch_size=1)
evaluate_dataset(model, loader, device)
```

## Configurations

| Configuration | Call | Parameters |
| :--- | :--- | ---: |
| BiCENet | `build_model(local_dc=False)` | 997,749 |
| w/o KAN | `build_model(local_dc=False, use_kan=False)` | 982,401 |
| w/o Chebyshev | `build_model(local_dc=False, basis_family='monomial')` | 997,749 |

## Training settings

| Setting | Value |
| :--- | :--- |
| Optimizer | Adam, lr 1e-4, cosine annealing |
| Epochs | 240 |
| Batch size | 8 |
| Training crop | 320 x 320 |
| Gradient clipping | L2 norm 1.0 |

## Pretrained model

`model.pth` in this repository is the checkpoint used for all results reported
in the paper.

## Dataset

**LCDataset** contains 533 ground-truth images, each degraded at three
severity levels, giving 1,599 paired training images and a held-out test set
of 90 images.

The dataset is available for academic use on request. Please contact
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
