<div align="center">

# ThyroidXL

### Advancing Thyroid Nodule Diagnosis with an Expert-Labeled, Pathology-Validated Dataset

**MICCAI 2025**

[![Project Page](https://img.shields.io/badge/Project-Page-1D4F73)](https://vnpt-ai-official.github.io/ThyroidXL/)
[![Dataset](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Dataset-yellow)](https://huggingface.co/datasets/VNPT-AI/ThyroidXL)
[![MICCAI 2025](https://img.shields.io/badge/MICCAI-2025-2F7F86)](#citation)

Hung Duong Viet<sup>1,3</sup>, Huan Vu<sup>1,4,✉</sup>, Duong Phan Huong<sup>2</sup>, Quyen Nguyen Duc<sup>1</sup>, Hao Pham Duc<sup>1,3</sup>, Toan Le Quang<sup>2</sup>,
Sy Nguyen Ba<sup>2</sup>, Dung Do Tien<sup>2</sup>, Sang Dinh Viet<sup>3</sup>, Cuong Nguyen Tien<sup>1,4</sup>, Hoang Pham Huy<sup>1</sup>, Hy Ngo Dien<sup>1</sup>

<sup>1</sup>VNPT AI, VNPT Group &nbsp;·&nbsp; <sup>2</sup>National Hospital of Endocrinology &nbsp;·&nbsp; <sup>3</sup>Hanoi University of Science and Technology &nbsp;·&nbsp; <sup>4</sup>National Economics University

<sup>✉</sup> Corresponding author

</div>

---

## Overview

**ThyroidXL** (XL = e**X**pert-**L**abeled) is an open benchmark dataset for thyroid nodule **malignancy classification**, **detection** and **segmentation** on B-mode ultrasound. It contains **11,635 images from 4,093 patients**, collected and annotated by expert radiologists at the Vietnam National Hospital of Endocrinology. It is the largest publicly available thyroid nodule ultrasound dataset in terms of both patient count and image volume.

### Highlights

- **Pathology-validated labels.** Benign/malignant labels come directly from fine-needle aspiration cytology (Bethesda system) and surgical pathology, with no inter-annotator variability.
- **Expert segmentation masks.** Each image is annotated independently by two board-certified radiologists (≥ 5 years of experience). The 8% of conflicting cases are resolved by a senior radiologist (≥ 30 years).
- **Clean images.** There are no hand-written calipers or diameter marks on the pixels, unlike DDTI and TN3K.
- **Clinical context.** Every image is scored with ACR TI-RADS and comes with patient demographic data.
- **Ready-made benchmark.** Baselines are provided for three tasks, reported at both image level and patient level.

## Dataset

| | ThyroidXL | DDTI | TN3K | TG3K | TN-SCUI |
|---|:---:|:---:|:---:|:---:|:---:|
| Images | **11,635** | 347 | 3,493 | 3,585 | 3,644 |
| Patients | **4,093** | 299 | 2,421 | – | 3,644 |

**Split**

| Split | Images | Patients | Benign | Malignant |
|---|:---:|:---:|:---:|:---:|
| Train | 9,541 | 3,354 | 2,477 | 877 |
| Test | 2,094 | 739 | 386 | 353 |

Benign and malignant counts are per patient. The cohort includes 3,650 female and 443 male patients, and most patients are aged 30–60.

**Acquisition**

- **Site and period:** Vietnam National Hospital of Endocrinology, collected over two years starting February 2023.
- **Device:** Hitachi Aloka Arietta V70. Each image contains one nodule; when a patient has several, the most suspicious one is selected.
- **Inclusion criteria:** normal thyroid function, nodule diameter > 5 mm, and no prior intervention (alcohol injection, radiofrequency ablation or repeated aspiration).
- **Ethics and privacy:** the study was approved by the Research Ethics Committee and all patients gave written informed consent. Images are de-identified: patient-information regions are cropped and IDs are remapped.

### Download

```bash
# Hugging Face CLI
huggingface-cli download VNPT-AI/ThyroidXL --repo-type dataset --local-dir ./ThyroidXL
```

```python
# Python
from huggingface_hub import snapshot_download

snapshot_download(repo_id="VNPT-AI/ThyroidXL", repo_type="dataset", local_dir="./ThyroidXL")
```

Please check the [dataset card](https://huggingface.co/datasets/VNPT-AI/ThyroidXL) for the licence and terms of use before downloading.

## Benchmark

Sensitivity and specificity are reported at both **image level** and **patient level**. Patient-level predictions use Weighted Majority Voting over all images of a patient. The tables below show the best models; full results are on the [project page](https://vnpt-ai-official.github.io/ThyroidXL/#benchmark).

**Malignancy classification** (input 224×224, main metric: F1)

| Model | Accuracy | F1 | Sens. (img) | Spec. (img) | Sens. (pt) | Spec. (pt) |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| VGG-13 BN | 0.830 | **0.824** | 0.855 | 0.809 | 0.853 | 0.824 |
| EfficientNet-B7 | **0.831** | 0.820 | 0.833 | 0.829 | 0.839 | **0.878** |

**Nodule detection** (COCO-style metrics)

| Model | mAP | mAP@50 | mAP@75 | F1 | Sens. (pt) | Spec. (pt) |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| CO-DETR (Swin-L) | **0.613** | **0.904** | **0.702** | 0.833 | 0.802 | **0.935** |
| YOLOX-M | 0.580 | **0.904** | 0.649 | **0.866** | **0.873** | 0.930 |

**Nodule segmentation** (input 384×480, main metric: IoU)

| Model | Backbone | IoU | Dice | F1 | Sens. (pt) | Spec. (pt) |
|---|---|:---:|:---:|:---:|:---:|:---:|
| UNet++ | EfficientNet-B5 | **0.648** | **0.725** | **0.840** | **0.759** | **0.984** |
| U-Net | EfficientNet-B6 | 0.640 | 0.716 | 0.835 | 0.745 | **0.984** |

## Citation

If you find ThyroidXL useful in your research, please cite:

```bibtex
@inproceedings{viet2025thyroidxl,
  title     = {ThyroidXL: Advancing Thyroid Nodule Diagnosis with an
               Expert-Labeled, Pathology-Validated Dataset},
  author    = {Duong Viet, Hung and Vu, Huan and Phan Huong, Duong and
               Nguyen Duc, Quyen and Pham Duc, Hao and Le Quang, Toan and
               Nguyen Ba, Sy and Do Tien, Dung and Dinh Viet, Sang and
               Nguyen Tien, Cuong and Pham Huy, Hoang and Ngo Dien, Hy},
  booktitle = {Medical Image Computing and Computer Assisted Intervention -- MICCAI 2025},
  publisher = {Springer},
  year      = {2025}
}
```

## Acknowledgments

This study was funded by the Vietnam Ministry of Science and Technology (grant number KC-4.0-43/19-25). We thank the physicians and staff of the Vietnam National Hospital of Endocrinology for data collection and annotation.

## Contact

- Hung Duong Viet: hungdv@vnpt.vn
- Huan Vu (corresponding author): huanv@neu.edu.vn
