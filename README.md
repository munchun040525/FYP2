# Network Intrusion Detection System (CIC-IDS2018)

## Overview
This repository contains the source code and documentation for a Network Intrusion Detection System (NIDS), developed as a Final Year Project. The project focuses on building robust machine learning models to analyze network traffic and detect malicious activities using the **CIC-IDS2018 dataset**.

## Dataset
The project utilizes the CSE-CIC-IDS2018 dataset, a comprehensive collection of network traffic data containing both benign and attack flows.
- **Size:** Approximately 16 million records.
- **Classes:** Benign, SQL Injection, Brute Force, DoS, Bot, etc.
- **Challenge:** Severe class imbalance (e.g., ~13 million Benign flows vs. 87 SQL Injection flows).
- **Strategy:** Applied stratified splitting by label and undersampling techniques to ensure balanced detection capabilities.

## Tech Stack
- **Language:** Python
- **Data Manipulation:** `pandas`, `numpy`
- **Machine Learning:** `scikit-learn`
- **Data Visualization:** `seaborn`, `matplotlib`

## Project Structure
```text
├── data/               # Raw and processed datasets (e.g., combined_data1.csv)
├── notebooks/          # Jupyter notebooks for EDA and Model Training
│   ├── 01_EDA.ipynb
│   ├── 02_Data_Cleaning.ipynb
│   └── 03_Model_Evaluation.ipynb
├── src/                # Source code for data preprocessing and modeling
├── requirements.txt    # Python dependencies
└── README.md           # Project documentation
```

## Methodology
1. **Data Cleaning:** 
   - Removed redundant headers (e.g., `Label == 'Label'`).
   - Handled missing values (NaN) and infinite values (Inf) resulting from traffic calculations.
   - Dropped identifying and highly-missing features (e.g., `Flow ID`, `Src IP`, `Src Port`, `Dst IP`, `Timestamp`) to prevent overfitting.
2. **Preprocessing:** Stratified data splitting to preserve the distribution of rare attack classes during training and testing.
3. **Modeling:** Training and evaluating classification models to detect anomalous network behavior.

## Installation & Usage
1. Clone this repository:
   ```bash
   git clone [https://github.com/munchun040525/FYP2.git](https://github.com/munchun040525/FYP2.git)
   ```
2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the data processing scripts in the `src/` directory or follow the step-by-step Jupyter notebooks.
