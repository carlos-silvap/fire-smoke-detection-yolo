# Fire and Smoke Detection using YOLO

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)

**Group project** developed at [Mid Sweden University](https://www.miun.se/en) by Andrea Faccioli, Sopio Shankulashvili, and [Carlos Silva](https://github.com/carlos-silvap).

---

## Overview

A real-time, vision-based fire and smoke detection system using transfer learning on lightweight anchor-free YOLO architectures. The system supports inference on images, video files, and live webcam streams. Multiple YOLO variants and training strategies are benchmarked against a Faster R-CNN baseline.

---

## Results

Best model: **YOLO11n with all layers unfrozen, trained on the full dataset.**

| Model | Frozen Layers | Data | mAP@50 | mAP@50-95 |
|-------|--------------|------|--------|-----------|
| YOLO11n | 0 | 100% | **~0.75** (test) | **0.574** (val) |
| YOLO11n | 0 | 20% | 0.699 | 0.412 |
| YOLOv8n | 0 | 20% | 0.695 | 0.397 |
| YOLOv10n | 10 | 20% | 0.639 | 0.350 |
| Faster R-CNN (baseline) | - | 20% | 0.45 | - |

Precision on test set (full training): **0.893 (fire), 0.900 (smoke)**

Live webcam pipeline latency: **up to 40ms**

---

## Key Findings

- Full fine-tuning (all layers unfrozen) consistently outperforms partial freezing when adapting COCO-pretrained weights to fire and smoke
- YOLO11n achieves the best accuracy-efficiency trade-off with 2.59M parameters, fewer than YOLOv8n (3.01M)
- Architectural extensions (GAM, ECA attention modules) did not improve performance over the YOLO11n baseline, likely due to redundancy with the built-in C2PSA attention mechanism
- The anchor-free design of YOLO11n is better suited to fire and smoke, which have irregular and non-rigid shapes

---

## Approach

- **Dataset:** 35,000+ annotated fire/smoke images (Kerby, Kaggle 2024), split into train/val/test
- **Transfer learning:** Pre-trained YOLO nano weights (COCO), fully fine-tuned on fire/smoke domain
- **Architectures compared:** YOLOv8n, YOLOv10n, YOLO11n (anchor-free variants)
- **Baseline:** Faster R-CNN with ResNet-50 + FPN backbone (Detectron2)
- **Inference modes:** static images, video files, live webcam stream

---

## Report

[Read the full paper](report.pdf)

---

## References

- He et al., *DCGC-YOLO*, IEEE Access 2024
- Cao et al., *Efficient Forest Fire Detection*, Visual Intelligence 2024
- Wang et al., *ECA-Net*, CVPR 2020
