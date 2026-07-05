# Progressive Knowledge Distillation for Indian Traffic Sign Detection

Official repository for the manuscript:

> **"Progressive Knowledge Distillation for Extreme Model Compression in Traffic Sign Detection: Bridging High-Capacity Visual AI and Edge-Deployable Recognition"**
> Shiney Thomas, John Varghese, Juby Mathew
> *The Visual Computer*, Springer, 2025 (under review)

## Key Results

| Metric | Value |
|--------|-------|
| Final model (V6) | YOLOv12n, 2.6M params, 5.4 MB |
| Test mAP@0.5 (1024px) | 76.3% |
| Teacher retention | 88.9% of YOLOv12x (85.8%) |
| vs. single-step compression | +5.8pp (76.3% vs 70.5%) |
| Best edge latency | 473 ms on Raspberry Pi 5 (float16) |

## Repository Contents

```
├── Code/               # Complete training pipeline (Step 0–18)
├── model/
│   └── v6-stage3-best-model/  # V6 exports (PyTorch, TFLite, ONNX)
├── sample_dataset/     # Sample subset: 5 images/class, 300 total + XML annotations
├── sample_dataset.yaml # YAML for sample dataset
└── dataset.yaml        # Full dataset structure reference
```

## Dataset

### Sample Subset (available here)
- 5 images per class × 60 IRC-compliant classes = **300 images**
- PASCAL VOC XML bounding box annotations included
- Located in `sample_dataset/` folder
- Use `sample_dataset.yaml` for configuration

### Full Dataset (available on request)
- 30,000 images across 60 IRC-compliant classes
- Augmented for monsoon, fog, night, sign damage, and occlusion
- Train: 23,754 | Val: 2,332 | Test: 3,914
- Contact: [shiney's email] with subject "IndianTSR Dataset Request"

## 60 IRC-Compliant Classes

27 Mandatory + 33 Cautionary signs as per Indian Roads Congress standards.
See `dataset.yaml` for full class list.

## Training Pipeline

Six versions isolating each technique:

| Version | Strategy | Test mAP@0.5 |
|---------|----------|:------------:|
| V1 | Baseline (640px) | 61.6% |
| V2 | + Augmentation (800px) | 71.4% |
| V3 | + Same-arch distillation + multi-res | 75.6% |
| V4 | YOLOv12x teacher | 85.8% |
| V5 | Single-step x→n (22.8×) | 70.5% |
| V6 | **Progressive x→l→m→s→n** | **76.3%** |

## Edge Deployment

V6 exported to TFLite via Edge Impulse:

| Platform | Precision | Latency | mAP@0.5 |
|----------|-----------|---------|---------|
| Raspberry Pi 5 | float16 | 473 ms | 56.0% |
| Raspberry Pi 5 | float32 | 564 ms | 56.0% |
| Jetson Orin Nano | float32 | 2,715 ms | 56.0% |
| Raspberry Pi 4 | float32 | 10,006 ms | 56.0% |

## Requirements

```bash
pip install ultralytics torch torchvision pandas numpy
```

- Python 3.9+
- PyTorch 2.9+
- Google Colab Pro with A100 recommended for training

## Citation

```bibtex
@article{thomas2025progressive,
  title={Progressive Knowledge Distillation for Extreme Model Compression
         in Traffic Sign Detection: Bridging High-Capacity Visual AI and
         Edge-Deployable Recognition},
  author={Thomas, Shiney and Varghese, John and Mathew, Juby},
  journal={The Visual Computer},
  publisher={Springer},
  year={2025}
}
```

## License

Code: MIT License
Dataset: Available for academic research purposes only.
Contact corresponding author for full dataset access.
