# PPE Compliance Detection System (Using Deep Learning for Industrial Safety)

An automated Personal Protective Equipment (PPE) compliance monitoring system built for real-world industrial environments: construction sites, petrochemical facilities, and heavy manufacturing operations where HSE compliance is critical.

Comparative study of three object detection architectures: YOLOv8n, YOLOv11n, and Faster R-CNN (ResNet50-FPN), with a live Gradio dashboard for real-time compliance monitoring.

## Why This Matters for Industrial Safety

Manual PPE checks are slow, inconsistent, and impossible to scale across large industrial facilities with hundreds of workers across multiple zones. This system enables:
- Continuous, automated compliance monitoring.
- Real-time alerts for HSE violations before incidents occur.
- Audit-ready PDF compliance reports for regulatory documentation.
- Per-worker compliance tracking: COMPLIANT / SEMI-COMPLIANT / NON-COMPLIANT with weighted severity scoring.

## Dataset

- Source: Roboflow Construction Site Safety Dataset (sourced through Kaggle).
- 2,801 images / 38,352 annotated instances, across 3 splits.
- 10 Classes: Hardhat, Mask, NO-Hardhat, NO-Mask, NO-Safety Vest, Person, Safety Cone, Safety Vest, machinery, and vehicle.

## Models & Results

| Model | mAP50 | mAP50-95 | Speed |
|---|---|---|---|
| YOLOv8n | 73.24% | 40.22% | 6.6ms/img |
| YOLOv11n | 73.60% | 39.52% | 8.7ms/img |
| Faster R-CNN (ResNet50-FPN) | 61.93% | 29.61% | — |

## Key Technical Findings

- YOLOv11n achieves best overall mAP50 via C2PSA spatial  attention, notably stronger on safety-critical violation class (NO-Hardhat: 60.93% v 55.36% for YOLOv8n)
- Faster R-CNN shows confirmed weakness on negative/absence classes (NO-Hardhat 42.17%, NO-Mask 44.63%).
- Class imbalance addressed via copy-paste augmentation + WeightedRandomSampler for Faster R-CNN while, standard Ultralytics 
  augmentation pipeline for YOLO models.
- All models trained from pretrained weights (transfer learning) on Tesla T4 GPU.

## Live Compliance Dashboard

Built with Gradio on top of YOLOv11n, deployable on any server connected to facility CCTV feeds:
- Image and video analysis (1–10 FPS sampling).
- IoU-based worker-to-PPE spatial matching.
- Weighted violation scoring: NO-Hardhat (highest) > NO-Safety Vest > NO-Mask.
- Scene-level risk tiers: LOW / MEDIUM / HIGH / CRITICAL.
- Includes machinery/vehicle hazard bonus and Safety Cone mitigation discount in risk formula.
- One-click PDF report export for HSE audit documentation.

## Tech Stack

Python · PyTorch · Ultralytics YOLO · Torchvision · Faster R-CNN · ResNet50-FPN · Gradio · ReportLab · Google Colab · Tesla T4 GPU

## HSE & Industrial Relevance

Designed with large-scale industrial deployment in mind:
- Architecture separates perception (YOLO model) from compliance logic (rule engine) thus, safety rules can be updated without retraining the model.
- Sub-10ms inference enables real-time monitoring across multiple concurrent camera feeds.
- Weighted risk scoring mirrors real HSE severity prioritization frameworks.
- Exportable audit reports support regulatory compliance documentation workflows.
