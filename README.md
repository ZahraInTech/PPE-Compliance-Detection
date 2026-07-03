# PPE Compliance Detection using Deep Learning

A comparative study of three object detection architectures: YOLOv8n, YOLOv11n, and Faster R-CNN (ResNet50-FPN), for automated PPE compliance monitoring on construction sites.

## Project Overview
This project utomatically detects workers and flags missing PPE (no hardhat, no mask, no safety vest) in real-time from images and video feeds, using a 10-class detection pipeline.

## Dataset
- Source: Roboflow Construction Site Safety Dataset (sourced via Kaggle)
- 2,801 images / 38,352 annotated instances
- 10 classes: Hardhat, Mask, NO-Hardhat, NO-Mask, NO-Safety Vest, Person, Safety Cone, Safety Vest, machinery, and vehicle.

## Models & Results

| Model | mAP50 | mAP50-95 | Inference |
|---|---|---|---|
| YOLOv8n | 73.24% | 40.22% | 6.6ms |
| YOLOv11n | 73.60% | 39.52% | 8.7ms |
| Faster R-CNN (ResNet50-FPN) | 61.93% | 29.61% | — |

## Key Findings
- YOLOv11n achieves best overall mAP50, with notably stronger performance on NO-Hardhat (60.93% vs 55.36%) possibly due to its C2PSA spatial attention module.
- Faster R-CNN shows specific weakness on negative/absence classes (NO-Hardhat 42.17%, NO-Mask 44.63%) confirmed by confusion matrix analysis.
- Faster R-CNN outperforms YOLO models on Safety Cone (53.82% vs ~42%), attributed to copy-paste augmentation, targeting the rare classes.

## Live Dashboard
Built with Gradio on top of YOLOv11n:
- Image and video analysis modes.
- Per-worker COMPLIANT / SEMI-COMPLIANT / NON-COMPLIANT classification.
- Scene-level risk scoring (LOW / MEDIUM / HIGH / CRITICAL).
- PDF report export for audit documentation.

## Tech Stack
Python, PyTorch, Ultralytics YOLO, Torchvision, Faster R-CNN, Gradio, ReportLab, Google Colab.
