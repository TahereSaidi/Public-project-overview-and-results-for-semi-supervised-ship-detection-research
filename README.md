# Semi-Supervised Ship Detection with Pseudo-Label Refinement and Mask-Based Consistency

This project presents a semi-supervised object detection framework for detecting ships in coastal-camera and onboard-vessel imagery. It combines an Efficient Teacher-based detector with pseudo-label refinement and mask-based consistency regularization to make use of unlabeled images when labeled data is limited.

> **Code availability:** The full implementation is not publicly available. The code may be shared upon request, subject to applicable permissions.

## Project Overview

Ship detection models typically require a substantial amount of labeled training data. In practical settings, unlabeled images may be more readily available than images annotated with bounding boxes.

This work investigates a semi-supervised object detection approach that trains with both labeled and unlabeled images. The task is single-class ship detection using bounding boxes.

## Data

The dataset is private and does not have a public dataset name. It contains imagery from coastal cameras and cameras mounted on vessels.

The experiments use four labeled-data settings:

| Labeled-data setting | Labeled training images | Unlabeled training images |
|---:|---:|---:|
| 1% | 1,000 | 117,000 |
| 2% | 2,000 | 116,000 |
| 5% | 5,000 | 113,000 |
| 10% | 10,000 | 117,000 |

The dataset contains one class: `ship`.

## Method

The framework uses a Teacher–Student training setup and combines the following components:

- **Efficient Teacher (ET):** The teacher–student object detection framework used in the experiments.
- **Teacher Refinement (TR):** Pseudo-labels are evaluated using a consistency score from two augmented views. A pseudo-label is accepted when its consistency score is greater than `0.35`.
- **Mask-Based Consistency (MBC):** For unlabeled images, the student is trained to produce consistent predictions for masked and unmasked versions of an image. Inconsistency between the two predictions is penalized.
- **Training augmentations:** The method uses augmentations, including mosaic augmentation.

The final training objective combines supervised detection loss, unsupervised loss, and mask-based consistency loss:

\[
L = L_S + L_U + L_{\text{cons-mask}}
\]

## Experimental Results

The table reports mAP@0.5 for the proposed framework and its component ablations under different labeled-data ratios.

| Model | 1% labeled | 2% labeled | 5% labeled | 10% labeled |
|---|---:|---:|---:|---:|
| YOLOv5s | 0.703 | 0.747 | 0.789 | 0.815 |
| Efficient Teacher (ET) | 0.786 | 0.805 | 0.829 | 0.841 |
| ET + Teacher Refinement (TR) | 0.820 | 0.831 | 0.848 | 0.851 |
| ET + Mask-Based Consistency (MBC) | 0.794 | 0.810 | 0.834 | 0.846 |
| **ET + TR + MBC** | **0.826** | **0.834** | **0.850** | **0.852** |

The full configuration achieves the highest reported mAP@0.5 in all four labeled-data settings. At the 1% labeled-data setting, it reaches 0.826 mAP@0.5, compared with 0.703 for YOLOv5s.

## Scope and Availability

This repository is a public project overview. It does not contain the private dataset, the full training implementation, or the model architecture figure.

The dataset is private. The full code is not publicly released and may be shared upon request, subject to applicable permissions.

## Research Status

**Paper:** *Semi-Supervised Ship Detection with Pseudo Label Refinement and Mask-Based Consistency*  
**Status:** Under review

## Notes

- The reported metric is mAP@0.5.
- The task is single-class ship detection with bounding boxes.
- The reported dataset counts and results reflect the experiments described in the project.
