# STMH-Net: A Heterogeneous Dual-Head Network for SAR Ship Detection

> 🎉 **Accepted by ICONIP 2026**
> 📖 To appear in **Springer Communications in Computer and Information Science (CCIS)**

## Introduction

**STMH-Net** is a lightweight heterogeneous detection framework designed for accurate and efficient ship detection in Synthetic Aperture Radar (SAR) imagery.

SAR ship detection remains challenging due to weak scattering responses, small target sizes, elongated target morphology, large orientation variations, and strong background interference in complex coastal scenes.

Instead of employing homogeneous prediction structures for different feature resolutions, STMH-Net introduces a **branch-specialized detection paradigm**, where different prediction branches are explicitly designed to model different SAR target characteristics.

The framework mainly consists of:

* **Spatial-Texture Perception Head (STP Head)**
* **Morphology-Aware Head (MA Head)**
* **Head-aligned Scale-Morphology Box Loss (HSM-Box Loss)**

These components jointly improve weak-target perception, morphology representation, and high-quality localization while maintaining a lightweight architecture.

---

## News

* **Aug. 2026** — STMH-Net was accepted by **ICONIP 2026**.
* The paper will be published in the **Springer CCIS proceedings of ICONIP 2026**.
* Source code, trained weights, and detailed configurations will be released in this repository.

---

## Motivation

SAR images exhibit fundamentally different visual characteristics from natural optical images.

Typical difficulties include:

* **Weak scattering responses** of small ships;
* **Limited spatial information** for tiny targets;
* **Elongated and orientation-sensitive morphology**;
* Strong interference from **coastlines, ports, buildings, and sea clutter**;
* Different target representation requirements at different feature resolutions.

A homogeneous detection head may therefore be insufficient for simultaneously modeling weak local responses and global ship morphology.

STMH-Net addresses this issue by introducing **heterogeneous prediction branches with branch-aligned supervision**.

---

## Method

### 1. Spatial-Texture Perception Head

The **Spatial-Texture Perception (STP) Head** is designed for the high-resolution prediction branch.

It focuses on preserving and enhancing weak responses of small SAR ships by jointly modeling local spatial information and scattering-related feature responses.

A **Dual-Pooling Channel Attention (DPCA)** mechanism further emphasizes informative channels while suppressing redundant background responses.

The STP Head mainly contributes to:

* weak-target perception;
* small-object representation;
* local spatial-detail preservation;
* clutter-resistant feature enhancement.

---

### 2. Morphology-Aware Head

Ships in SAR imagery usually exhibit highly elongated geometric structures and strong directional characteristics.

The **Morphology-Aware (MA) Head** explicitly enhances morphology-related information by modeling horizontal and vertical structural dependencies.

This enables the prediction branch to better capture:

* elongated ship structures;
* directional responses;
* shape-related contextual information;
* medium-scale target morphology.

---

### 3. HSM-Box Loss

Different prediction branches are responsible for different target characteristics.

Therefore, applying completely homogeneous localization supervision to every branch may not fully exploit their specialized representations.

We introduce the **Head-aligned Scale-Morphology Box Loss (HSM-Box Loss)** to provide branch-specific regression supervision.

The loss incorporates geometric information related to:

* target center;
* object scale;
* aspect ratio;
* bounding-box localization.

By aligning optimization objectives with branch responsibilities, HSM-Box Loss further improves high-quality localization.

---

## Framework

```text
                       SAR Image
                           │
                           ▼
                  Feature Extraction
                           │
                           ▼
                 Multi-scale Features
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
        High-resolution  Mid-resolution  Deep Feature
           Feature          Feature        Branch
             │             │
             ▼             ▼
         STP Head        MA Head
             │             │
             └──────┬──────┘
                    │
                    ▼
          Heterogeneous Prediction
                    │
                    ▼
              HSM-Box Loss
                    │
                    ▼
              Detection Results
```

The core philosophy of STMH-Net is:

> **Different prediction branches should learn target representations that match their specific responsibilities, while localization supervision should be aligned with these heterogeneous representations.**

---

## Experimental Results

### HRSID

STMH-Net achieves strong detection performance on the HRSID benchmark.

| Metric       |  STMH-Net |
| ------------ | --------: |
| Precision    | **95.1%** |
| Recall       | **89.0%** |
| mAP@0.5      | **93.4%** |
| mAP@0.5:0.95 | **73.9%** |

The model particularly improves high-quality localization and the detection of small and weak SAR ships.

---

### SSDD

STMH-Net also demonstrates strong adaptability on the SSDD benchmark.

| Metric       |  STMH-Net |
| ------------ | --------: |
| Precision    | **97.0%** |
| Recall       | **97.1%** |
| mAP@0.5      | **98.6%** |
| mAP@0.5:0.95 | **73.0%** |

The results demonstrate that the proposed heterogeneous design remains effective across different SAR ship detection scenarios.

---

## Performance on Different Target Scales

STMH-Net provides significant improvements for tiny and small targets, which are particularly challenging in SAR imagery.

| Target Scale |        AP |
| ------------ | --------: |
| Tiny         | **54.7%** |
| Small        | **69.9%** |
| Medium       | **81.3%** |
| Large        | **60.4%** |

The results indicate that the proposed high-resolution branch is particularly effective for tiny and small ship targets.

---

## Model Efficiency

STMH-Net maintains a lightweight architecture while achieving competitive detection accuracy.

| Model        | Parameters |       FLOPs |       FPS |
| ------------ | ---------: | ----------: | --------: |
| **STMH-Net** |  **7.5 M** | **21.69 G** | **91.41** |

This makes STMH-Net suitable for scenarios requiring a favorable trade-off between detection accuracy and computational efficiency.

---

## Analysis

To better understand the behavior of the proposed framework, the paper includes extensive analyses covering:

* component ablation studies;
* channel-attention comparison;
* scale-wise detection performance;
* feature representation analysis;
* rotation perturbation experiments;
* complex-background false-positive analysis;
* qualitative detection visualization;
* computational efficiency evaluation.

These experiments demonstrate that the performance improvement does not originate from a single component, but from the cooperation between **heterogeneous representation learning and branch-aligned localization supervision**.

---

## Datasets

Experiments are conducted on two widely used SAR ship detection benchmarks:

* **HRSID**
* **SSDD**

Please download the corresponding datasets from their official repositories and configure the dataset paths according to the provided configuration files.

---

## Installation

```bash
git clone <repository-url>
cd STMH-Net

pip install -r requirements.txt
```

Detailed environment information will be provided together with the released code.

---

## Training

Example training command:

```bash
python train.py \
    --data <dataset_config> \
    --cfg <model_config> \
    --weights <pretrained_weights>
```

Detailed training hyperparameters and configuration files will be released with the source code.

---

## Evaluation

```bash
python val.py \
    --data <dataset_config> \
    --weights <checkpoint>
```

---

## Inference

```bash
python detect.py \
    --weights <checkpoint> \
    --source <image_or_directory>
```

---

## Repository Structure

```text
STMH-Net/
│
├── models/
│   ├── STP/
│   ├── MA/
│   └── STMH-Net/
│
├── utils/
│   └── losses/
│
├── data/
│
├── configs/
│
├── weights/
│
├── train.py
├── val.py
├── detect.py
├── requirements.txt
└── README.md
```

The repository structure will be updated together with the official code release.

---

## Paper

**STMH-Net: A Heterogeneous Dual-Head Network for SAR Ship Detection**

**Xinyang Zhang, Hui Luo, Zhen Ding**

International Conference on Neural Information Processing (**ICONIP 2026**)

Accepted for publication in the **Springer Communications in Computer and Information Science (CCIS)** proceedings.

---

## Citation

If you find STMH-Net useful in your research, please consider citing our work.

```bibtex
@inproceedings{zhang2026stmh,
  title     = {STMH-Net: A Heterogeneous Dual-Head Network for SAR Ship Detection},
  author    = {Zhang, Xinyang and Luo, Hui and Ding, Zhen},
  booktitle = {International Conference on Neural Information Processing},
  year      = {2026},
  publisher = {Springer}
}
```

The BibTeX entry will be updated after the official Springer volume, page numbers, and DOI become available.

---

## Acknowledgement

We sincerely thank the researchers and maintainers of the HRSID and SSDD datasets for providing valuable SAR ship detection benchmarks.

We also appreciate the open-source community for providing excellent tools and frameworks that support SAR object detection research.

---

## Contact

For questions regarding STMH-Net, please open an issue in this repository.

---

## License

The license information will be provided together with the official source-code release.
