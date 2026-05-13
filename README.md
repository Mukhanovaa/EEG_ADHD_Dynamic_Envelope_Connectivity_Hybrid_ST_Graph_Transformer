# EEG-Based ADHD Detection Using Dynamic Envelope Connectivity and a Hybrid Spatio-Temporal Graph Transformer

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Figures-11557C?style=for-the-badge)
![Subject-Level Validation](https://img.shields.io/badge/Validation-Subject--Level-2E8B57?style=for-the-badge)
![Test Accuracy](https://img.shields.io/badge/Test%20Accuracy-0.63-4C78A8?style=for-the-badge)
![Test AUC](https://img.shields.io/badge/Test%20AUC--ROC-0.74-2E8B57?style=for-the-badge)

## Overview

This repository contains a reproducible scientific analysis for EEG-based ADHD detection using dynamic functional connectivity. The workflow focuses on band-limited theta and beta amplitude-envelope connectivity and evaluates several network-index models, including the proposed **Hybrid Multihead Spatio-Temporal Graph Transformer Index**.

The analysis is designed for publication-oriented verification rather than exploratory prototyping. It emphasizes subject-level data splitting, leakage prevention, reproducibility checks, conservative interpretation of classification metrics, and clear separation between model-based connectivity results and descriptive raw-amplitude analyses.

## Main Analysis Notebook

The central notebook is:

**`EEG_ADHD_Dynamic_Envelope_Connectivity_Hybrid_ST_Graph_Transformer.ipynb`**

The notebook title is:

**EEG-Based ADHD Detection Using Dynamic Envelope Connectivity and a Hybrid Spatio-Temporal Graph Transformer**

This title reflects the scientific scope of the study: dynamic EEG connectivity, amplitude-envelope modeling, subject-level ADHD detection, and the proposed hybrid spatio-temporal graph-transformer architecture.

## Dataset

The analysis uses an ADHD EEG dataset provided in **CSV tabular format**. The dataset includes EEG channel measurements, subject identifiers, and class labels required for subject-level ADHD/control modeling.

The repository does not include personal participant information. All analysis is intended for secondary computational research using anonymized EEG data. The original dataset source should be cited, and the dataset provider's usage terms should be followed.

## Scientific Workflow

```mermaid
flowchart LR
    A[CSV EEG Dataset] --> B[Integrity Audit]
    B --> C[Subject-Level Split]
    C --> D[Preprocessing]
    D --> E[Theta and Beta Filtering]
    E --> F[Hilbert Amplitude Envelopes]
    F --> G[Dynamic Connectivity Sequences]
    G --> H[Network Index Models]
    H --> I[Subject-Level Evaluation]
    I --> J[Reviewer-Ready Tables and Figures]
```

## Core Methodological Components

| Component | Purpose |
|---|---|
| Subject-level split | Prevents data leakage between train, validation, and test partitions. |
| Demeaning and detrending | Reduces subject-specific offsets and slow drifts before connectivity analysis. |
| Robust amplitude clipping | Limits the influence of extreme amplitude artifacts while preserving signal structure. |
| Theta and beta filtering | Extracts band-limited activity used for final envelope-connectivity modeling. |
| Hilbert envelopes | Converts filtered signals into amplitude-envelope representations. |
| Dynamic connectivity windows | Builds time-resolved inter-channel connectivity matrices. |
| Hybrid graph-transformer model | Combines transformer, recurrent, and graph-informed representations for scalar network-index prediction. |
| Subject-level aggregation | Produces clinically more relevant evaluation than window-level reporting alone. |

## Evaluated Model Families

The notebook evaluates the original model family without replacing the proposed architecture:

- Linear Network Index
- Sparse Oscillatory / Sparse Linear Index
- Global Graph-Metric Network Index
- GRU Dynamic Connectivity Index
- Static GCN Brain Network Index
- Spatio-Temporal GCN Brain Network Index
- Proposed Hybrid Multihead Spatio-Temporal Graph Transformer Index
- Proposed-model ablation, where applicable

## Validation Principles

The analysis follows conservative validation rules:

- train, validation, and test partitions are separated at the subject level;
- leakage checks are explicitly computed;
- classification metrics are reported separately from distribution-level separability metrics;
- bootstrap confidence intervals are included for subject-level test performance;
- repeated subject-level splits and lightweight leave-one-subject-out validation are used as additional stability checks;
- raw-amplitude correlations are treated as descriptive analyses only;
- final model results are based on theta/beta amplitude-envelope connectivity.

## What Was Implemented

| Area | What was completed | Why it matters |
|---|---|---|
| Dataset audit | Verified rows, subjects, EEG channels, labels, missing values, class balance, and subject identifiers. | Ensures the analysis starts from a transparent and reproducible data description. |
| Subject-level validation | Recreated subject-level train/validation/test partitions and verified zero subject overlap. | Prevents window-level leakage and avoids inflated performance estimates. |
| Signal processing | Applied demeaning, detrending, robust clipping, theta/beta band-pass filtering, and Hilbert envelope extraction. | Aligns final modeling with band-limited amplitude-envelope connectivity rather than raw-amplitude correlations. |
| Dynamic connectivity | Built overlapping window-level and sequence-level connectivity representations. | Captures time-varying functional connectivity patterns used by the dynamic models. |
| Model benchmarking | Evaluated linear, sparse, graph-metric, GRU, GCN, spatio-temporal GCN, and hybrid graph-transformer index models. | Provides a controlled comparison between simpler baselines and the proposed model family. |
| Reviewer-oriented validation | Added bootstrap confidence intervals, repeated subject-level splits, lightweight LOSO validation, proposed-model deep LOSO validation, 128 Hz sensitivity rerun, sampling-frequency audit, and metric-category separation. | Makes the reported results more transparent while separating computationally resolved checks from remaining dataset-documentation caveats. |

## Proposed Model

The main model is the **Hybrid Multihead Spatio-Temporal Graph Transformer Index**. It preserves the original architecture used in the scientific pipeline and combines:

- an edge-vector representation of theta/beta amplitude-envelope connectivity;
- a transformer branch with multi-head self-attention;
- positional embedding for dynamic sequence structure;
- a GRU branch for temporal connectivity dynamics;
- feature fusion with normalization and dropout;
- a scalar network-index output for subject-level aggregation.

| Model setting | Value |
|---|---:|
| Input dimension | 342 |
| Output dimension | 1 |
| Hidden size | 64 |
| Transformer heads | 4 |
| Transformer encoder layers | 1 |
| GRU hidden size | 64 |
| Dropout | 0.25 |
| Optimizer | AdamW |
| Learning rate | 0.0004 |
| Weight decay | 0.0003 |
| Batch size | 64 |
| Maximum epochs | 16 |
| Early stopping | patience = 5 |

The sparse oscillatory baseline uses an L1-regularized logistic model with `C = 0.25` and `max_iter = 5000`, matching the executed code configuration.

## Main Subject-Level Results

The independent test set contains **19 subjects**. The proposed hybrid model achieves the strongest overall ranking among the evaluated models, but the result should be interpreted as moderate subject-level discrimination rather than diagnostic-level performance.

| Model | Accuracy | Balanced Accuracy | Sensitivity | Specificity | F1-score | AUC-ROC |
|---|---:|---:|---:|---:|---:|---:|
| Proposed Hybrid ST Graph Transformer | **0.63** | **0.62** | 0.80 | 0.44 | **0.70** | **0.74** |
| Proposed model ablation | 0.63 | 0.62 | 0.80 | 0.44 | 0.70 | 0.73 |
| GRU dynamic index | 0.58 | 0.57 | 0.70 | 0.44 | 0.64 | 0.66 |
| Global graph-metric index | 0.58 | 0.57 | 0.80 | 0.33 | 0.67 | 0.63 |
| Linear network index | 0.53 | 0.52 | 0.70 | 0.33 | 0.61 | 0.47 |
| Spatio-temporal GCN index | 0.53 | 0.50 | 1.00 | 0.00 | 0.69 | 0.48 |
| Static GCN index | 0.53 | 0.50 | 1.00 | 0.00 | 0.69 | 0.44 |
| Sparse oscillatory index | 0.47 | 0.47 | 0.60 | 0.33 | 0.55 | 0.47 |

### Uncertainty of the Proposed Model

| Metric | Estimate | 95% bootstrap confidence interval |
|---|---:|---:|
| Accuracy | 0.63 | 0.42–0.84 |
| Balanced accuracy | 0.62 | 0.42–0.83 |
| Sensitivity | 0.80 | 0.50–1.00 |
| Specificity | 0.44 | 0.11–0.78 |
| F1-score | 0.70 | 0.50–0.86 |
| AUC-ROC | 0.74 | 0.49–0.94 |

The wide confidence intervals reflect the small independent test set. Therefore, the result supports a proof-of-concept and screening-oriented interpretation, not a standalone clinical diagnostic claim.

## Separability Results

Distribution-level index separability is reported separately from classification performance. These values describe how the learned scalar index differs between control and ADHD groups, but they are not classification metrics.

| Model | Cohen's d | Pearson r | Distribution overlap | Silhouette |
|---|---:|---:|---:|---:|
| Proposed Hybrid ST Graph Transformer | **0.73** | **0.36** | **0.72** | **0.03** |
| Proposed model ablation | 0.66 | 0.33 | 0.74 | 0.01 |
| GRU dynamic index | 0.66 | 0.33 | 0.74 | -0.02 |
| Global graph-metric index | 0.55 | 0.28 | 0.78 | -0.04 |
| Linear network index | 0.04 | 0.02 | 0.98 | -0.04 |
| Sparse oscillatory index | 0.09 | 0.05 | 0.96 | -0.04 |
| Static GCN index | -0.34 | -0.18 | 0.87 | -0.04 |
| Spatio-temporal GCN index | -0.23 | -0.12 | 0.91 | -0.07 |

Historical separability values such as mean shift `6.21`, Cohen's `d = 1.51`, Pearson `r = 0.60`, and overlap `0.45` should not be reported as current results of this pipeline unless they are independently reproduced under the same subject-level split and model-output aggregation. The current analysis supports the values shown above for the proposed model: `delta mean = 1.16`, `Cohen's d = 0.73`, `Pearson r = 0.36`, and `overlap = 0.72`.

## Additional Validation

| Validation analysis | Key result | Interpretation |
|---|---|---|
| Leakage matrix | No subject overlap between train, validation, and test partitions. | Supports subject-level separation and reduces leakage risk. |
| Repeated subject-level splits | Lightweight models showed variable performance across repeated splits. | Confirms that split uncertainty is non-negligible in this dataset. |
| Lightweight LOSO validation | Sparse oscillatory index reached accuracy 0.74 and AUC-ROC 0.79 across 121 subjects. | Provides an additional baseline stability check, but does not replace deep LOSO or external validation of the proposed hybrid model. |
| Proposed-model deep LOSO validation | The proposed hybrid model reached accuracy 0.72, balanced accuracy 0.72, sensitivity 0.82, specificity 0.62, F1-score 0.75, and AUC-ROC 0.78 across 121 held-out subjects. | Provides direct leave-one-subject-out evidence for the proposed deep architecture while still requiring external validation before clinical claims. |
| 128 Hz sensitivity rerun | The proposed model reached test accuracy 0.68, balanced accuracy 0.68, sensitivity 0.80, specificity 0.56, F1-score 0.73, and AUC-ROC 0.81 under the 128 Hz rerun. | Addresses the 500/128 Hz concern computationally; reporting should still disclose that the metadata sources are inconsistent. |
| Sampling-frequency audit | Computational analysis used 500 Hz and was accompanied by a full fixed-split 128 Hz rerun. | The manuscript should report both analyses and avoid unsupported claims about raw acquisition frequency unless provenance is confirmed. |

## Remaining Technical Caveats

These items are intentionally reported conservatively:

- **Sampling frequency.** The primary computation uses 500 Hz, while external documentation may indicate 128 Hz. The analysis now includes both the timing-impact table and a full fixed-split 128 Hz rerun of the proposed model. The metadata discrepancy should still be disclosed rather than hidden.
- **Deep LOSO.** Leave-one-subject-out validation is now reported for the proposed hybrid deep model and for lightweight models. This strengthens subject-level validation, but it is still internal validation on a single dataset rather than independent external validation.
- **Historical separability values.** Earlier values such as `6.21`, `d = 1.51`, `r = 0.60`, and `overlap = 0.45` are not used as current results of this pipeline.
- **Volume leakage.** Sensor-space Pearson/envelope connectivity may reflect field spread or shared sources. The analysis therefore avoids claiming direct cortical coordination.
- **Event-locked and behavioral validation.** Event markers and behavioral outcomes are not included, so event-specific mechanisms and behavioral correlates remain future work.

## Output Types

The notebook generates publication-oriented scientific outputs:

- dataset integrity and sampling-frequency audit tables;
- subject-level split and leakage-prevention summaries;
- preprocessing and connectivity validation figures;
- classification and separability metric tables;
- bootstrap confidence intervals;
- repeated-split and lightweight LOSO validation summaries;
- model architecture and hyperparameter audit tables;
- reviewer-oriented methodological caveat tables;
- publication-quality Matplotlib figures.

## Interpretation

The model should be interpreted conservatively. Subject-level performance is evidence for a preliminary screening-oriented decision-support approach, not a standalone diagnostic system. Distribution-level index separability is reported separately from classification performance and should not be treated as equivalent to clinical diagnostic accuracy.

Important limitations include the small independent test set, single-dataset evaluation, possible sensor-space volume leakage, lack of event-locked or behavioral validation, and remaining metadata uncertainty when dataset documentation and computational sampling-frequency assumptions differ. The added deep LOSO and 128 Hz rerun reduce reviewer-facing methodological uncertainty but do not convert the model into an externally validated diagnostic system.

## Reproducibility Notes

The notebook is intended to run sequentially from the first cell to the last cell in an existing Python/Jupyter environment with the scientific Python stack installed. The analysis uses fixed random seeds and Matplotlib-only visualization to improve reproducibility and consistency of exported figures.

The proposed architecture, preprocessing logic, feature construction, and subject-level split strategy are preserved from the completed scientific pipeline. The notebook reorganizes and validates the work for transparent review, but does not introduce a replacement model.

## Recommended Citation

If this repository is used, cite the associated manuscript and the original ADHD EEG dataset source. Exact bibliographic information should be added after manuscript acceptance or public dataset citation confirmation.
