# FL-IDS: Federated Learning Intrusion Detection System

**241UC24160 — Network Anomaly Prediction, Detection, and Mitigation using Machine Learning**
Loke Mun Chun, supervised by Dr. Pee Chih Yang, MMU Faculty of Computing & Informatics

## Project Overview

This project implements a Federated Learning-based Intrusion Detection System (FL-IDS) using the CSE-CIC-IDS2018 dataset. A lightweight 1D-CNN model (67,335 parameters, ~263KB) is trained under the FedAvg protocol across IID, Non-IID (70/20/10 label skew), and Extreme Non-IID partitioning scenarios, then compared against centralized XGBoost and 1D-CNN baselines.

Key results (on the real CSE-CIC-IDS2018 dataset, ~15.7M flows):
- Centralized 1D-CNN baseline: 98.24% test accuracy
- FL-IID (5 clients): 98.24% — matches centralized exactly
- FL-Non-IID (5 clients): 98.22% — only 0.02% degradation
- FL-Extreme Non-IID (10 clients): 89.36% — FedAvg fails under extreme heterogeneity

## Notebooks

The pipeline is split across four Jupyter notebooks, meant to be run in this order:

| Notebook | Purpose |
|---|---|
| `A combine and spliting.ipynb` | Stage 1: Combine 10 daily CSE-CIC-IDS2018 CSVs, clean (remove duplicates, corrupted timestamps, stray headers), map 15 raw labels to 7 Attack_Type categories, chronological 70/15/15 train/val/test split |
| `New Preprocessing-Train-Val-Test-setb.ipynb` | Stage 2-3: Clean numeric columns, handle NaN/Inf/negatives, stratified 80/20 re-split of train+val pool, Min-Max normalization, Boruta and CorrMI feature selection (Top-10/20/30 subsets) |
| `Model Training-Boruta-setc.ipynb` | Stage 4: XGBoost grid search (27 combinations × 6 variants: T10/T20/T30/Downsampling/SMOTE1/SMOTE2) + centralized 1D-CNN training and evaluation |
| `FL_improved_v3.ipynb` | Stage 5: Federated Learning — IID/Non-IID/Extreme Non-IID client partitioning, FedAvg training loop (R=20 rounds, E=5 local epochs, batch=256), local epoch sensitivity analysis (E=1/5/10) |

## Dataset

The CSE-CIC-IDS2018 dataset (~13M+ flow records) is **not included** in this repository — it is too large and not this project's data to redistribute. Download it from the Canadian Institute for Cybersecurity's official distribution, then point the first notebook's `datapath` variable at the folder containing the 10 daily CSV files.

## Environment

Developed and tested on:
- Windows 11, Intel i7 / AMD Ryzen 7, 32GB RAM (MMU FCI Lab)
- Python 3.x (Anaconda), Jupyter Notebook
- See `requirements.txt` for package dependencies

Install dependencies:
```bash
pip install -r requirements.txt
```

## 1D-CNN Architecture (Table 4.1 in report)

| Layer | Output Shape | Parameters |
|---|---|---|
| Conv1D (64 filters, kernel=3) + BatchNorm + MaxPool | (4, 64) | 256 + 256 |
| Conv1D (128 filters, kernel=3) + BatchNorm + MaxPool | (1, 128) | 24,704 + 512 |
| Flatten → Dense(128) → Dropout(0.3) | (128) | 16,512 |
| Dense(64) → Dropout(0.3) | (64) | 8,256 |
| Dense(7, softmax) | (7) | 455 |
| **Total** | | **67,335** (~263KB) |

## Federated Learning Configuration

- Algorithm: FedAvg (McMahan et al., 2017)
- Communication rounds: R = 20
- Local epochs: E = 5 (default), sensitivity tested with E = 1 and E = 10
- Batch size: 256
- Clients tested: K = 5, 10, 15
- Boruta Top-10 features used for all FL experiments

## Files

- `mappings.json` — label-to-integer encoding mappings used during preprocessing
- `requirements.txt` — Python package dependencies
- `.gitignore` — excludes data files, trained models, and Python cache from version control
