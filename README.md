# CyberSentinel AI: Intrusion Detection & Threat Intelligence Platform

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/)
[![Dataset: UNSW-NB15](https://img.shields.io/badge/dataset-UNSW--NB15-red.svg)](https://research.unsw.edu.au/projects/unsw-nb15-dataset)
[![Accuracy](https://img.shields.io/badge/binary--acc-96.83%25-brightgreen.svg)]()
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)]()

**CyberSentinel AI** is an end-to-end **Cyber Attack Detection, Threat Intelligence, and Security Operations Center (SOC) Analytics Platform** engineered using the benchmark **UNSW-NB15 dataset** (82,332 real and synthetic network flows, 45 features, 9 cyber attack vectors + normal traffic).

The system integrates high-performance supervised Machine Learning models with an interactive, dark-mode SOC Command Dashboard for real-time telemetry inspection, attack simulation, MITRE ATT&CK taxonomy exploration, and automated incident response mitigation.

---

## Key Features

- 🛡️ **Dual-Stage Machine Learning Engine**:
  - **Binary Intrusion Detector**: Classifies normal vs malicious flows with **96.83% accuracy** and **97.07% F1-score** using Random Forest.
  - **Multi-Class Attack Classifier**: Accurately categorizes flows into 9 distinct cyber attack types (DoS, Exploits, Fuzzers, Reconnaissance, Generic, Analysis, Backdoors, Shellcode, Worms) with **86.59% accuracy**.
  - **Feature Importance Analytics**: Identifies key network attack signatures (e.g. `smean`, `sbytes`, `service`, `ct_dst_sport_ltm`, `sload`, `rate`, `sttl`).
- ⚡ **Interactive SOC Command Center (`web/`)**:
  - **Executive HUD Dashboard**: Real-time traffic KPIs, transport protocol distribution, target application service breakdown, and live simulated telemetry stream.
  - **Cyber Attack Taxonomy & MITRE ATT&CK Matrix**: Deep dive into all 9 attack categories with mapped tactics, techniques, severity ratings, and typical packet profiles.
  - **Live Network Flow Threat Simulator**: Interactive test workbench with one-click attack presets (`DoS`, `Exploit`, `Reconnaissance`, `Fuzzing`, `Normal`) that calculates anomaly scores, threat severity levels (0–100), and auto-generates defensive firewall rules.
  - **Dataset Telemetry Explorer**: Filterable, searchable table displaying real UNSW-NB15 flow records with full packet inspection modals.
  - **ML Benchmark & Explainability Center**: Comparative metrics across algorithms and Gini feature importance progress indicators.
  - **Defensive Mitigation & Incident Playbooks**: Exportable incident response playbooks conforming to NIST SP 800-61 guidelines.
- 💻 **Cross-Platform CLI & Prediction Tool**:
  - Standalone inference CLI (`src/predict_flow.py`) for automated security pipeline integration and SIEM alerting.

---

## Dataset Overview (UNSW-NB15)

The dataset is located in `Datasets/UNSW_NB15_training-set.csv` containing **82,332 records** and **45 features**.

| Attack Category | Record Count | Percentage | MITRE ATT&CK Tactic | MITRE ID |
| :--- | :---: | :---: | :--- | :--- |
| **Normal** | 37,000 | 44.94% | Baseline Enterprise Traffic | N/A |
| **Generic** | 18,871 | 22.92% | Defense Evasion / Initial Access | T1110 |
| **Exploits** | 11,132 | 13.52% | Execution / Privilege Escalation | T1203 |
| **Fuzzers** | 6,062 | 7.36% | Discovery / Execution | T1595.002 |
| **DoS** | 4,089 | 4.97% | Impact | T1498 / T1499 |
| **Reconnaissance** | 3,496 | 4.25% | Reconnaissance (Port Scanning) | T1595.001 |
| **Analysis** | 677 | 0.82% | Reconnaissance / Web App Probing | T1190 |
| **Backdoor** | 583 | 0.71% | Persistence / Command & Control | T1059 / T1071 |
| **Shellcode** | 378 | 0.46% | Process Injection / Execution | T1055 |
| **Worms** | 44 | 0.05% | Lateral Movement | T1080 |

---

## Machine Learning Benchmark Results

Trained and evaluated on 40,000 stratified samples with an 80/20 train/test split:

### 1. Binary Intrusion Detection (Attack vs Normal)
| Model Architecture | Accuracy | Precision | Recall | F1-Score | Training Time |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Random Forest (Binary)** | **96.83%** | **97.84%** | **96.32%** | **97.07%** | **0.59s** |
| Decision Tree (Binary) | 95.91% | 97.11% | 95.36% | 96.23% | 0.23s |
| Logistic Regression | 88.67% | 88.93% | 90.55% | 89.73% | 0.48s |

### 2. Multi-Class 10-Class Attack Categorizer
- **Overall Accuracy**: **86.59%**
- **Weighted Precision**: **85.84%**
- **Weighted Recall**: **86.59%**
- **Weighted F1-Score**: **85.99%**

### 3. Top 10 Predictive Features (Gini Importance)
1. `smean` (Mean Source Packet Size): **8.44%**
2. `sbytes` (Source to Destination Transaction Bytes): **8.05%**
3. `service` (Application Layer Service): **8.05%**
4. `ct_dst_sport_ltm` (Connections from Same Dest to Src Port): **6.57%**
5. `sload` (Source Bits per Second): **6.02%**
6. `ct_dst_src_ltm` (Connections Between Same Source and Dest): **5.59%**
7. `ct_srv_dst` (Connections with Same Service and Dest): **5.51%**
8. `rate` (Flow Packet Rate): **4.68%**
9. `dur` (Flow Duration): **3.56%**
10. `sttl` (Source Time to Live): **3.22%**

---

## Project Directory Structure

```
cyber attacks/
├── Datasets/
│   └── UNSW_NB15_training-set.csv     # Raw dataset (82,332 records)
├── src/
│   ├── eda_analyzer.py                 # Dataset analysis & telemetry extractor
│   ├── train_models.py                 # ML training & benchmark evaluation engine
│   ├── predict_flow.py                 # Real-time CLI flow threat predictor
│   └── batch_predict.py                # Batch dataset scanner for arbitrary CSV files
├── models/                             # Serialized trained model artifacts (.joblib)
│   ├── best_binary_model.joblib
│   ├── multiclass_attack_model.joblib
│   ├── encoders.joblib
│   ├── scaler.joblib
│   └── feature_cols.joblib
├── data/
│   ├── dataset_summary.json            # Structured dataset telemetry
│   ├── benchmark_results.json          # Empirical ML benchmark metrics
│   ├── flow_presets.json               # Verified test presets
│   └── predictions_output.csv          # Exported batch scan results
├── web/                                # Interactive SOC Web Platform
│   ├── index.html                      # Semantic HUD dashboard
│   ├── style.css                       # Cyber SOC dark-mode aesthetic styling
│   ├── app.js                          # Canvas charts, simulator, & batch scanner logic
│   └── dashboard_data.js               # Embedded telemetry & benchmark data
├── python_env/                         # Local portable Python 3.11 environment
├── requirements.txt                    # Python dependencies
└── README.md                           # Documentation
```

---

## Quickstart Guide

### 1. Scan Any CSV Dataset for Cyber Attacks

You can scan any dataset using either the **Web Dashboard** or the **Python CLI**:

#### A. In the Web Browser (Zero Installation):
1. Open `http://localhost:8080` (or double-click `web/index.html`).
2. Click the **"Upload & Scan Dataset"** tab in the top navigation bar.
3. Drag & drop your `.csv` file (or click to browse).
4. Click **"Scan Dataset for Threats"** (or click **"Load Sample Test Batch (500 Flows)"** for an instant demo).
5. Review the detected attacks, threat scores, breakdown charts, and click **"Export Predictions (.CSV)"** to download the annotated dataset!

#### B. Via Python Command Line:
```powershell
# Scan an entire dataset and output predictions to CSV:
.\python_env\python.exe src/batch_predict.py --input "Datasets/UNSW_NB15_training-set.csv" --output "data/predictions_output.csv"

# Scan the first 5,000 records of any dataset:
.\python_env\python.exe src/batch_predict.py --input "path/to/your_dataset.csv" --limit 5000
```

### 2. Run the Interactive SOC Web Platform
Open the web platform in any browser:
- **Option A (Direct File Open)**: Open `web/index.html` directly in Google Chrome, Edge, or Firefox.
- **Option B (Local Web Server)**:
  ```bash
  .\python_env\python.exe -m http.server 8080 --directory web
  ```
  Then navigate to: `http://localhost:8080`

### 3. Test Real-time Single Flow Threat Prediction via CLI
Run threat assessments using built-in presets:
```bash
# Test a Denial of Service attack flow
.\python_env\python.exe src/predict_flow.py --preset dos

# Test an Exploit flow
.\python_env\python.exe src/predict_flow.py --preset exploits

# Test benign enterprise traffic
.\python_env\python.exe src/predict_flow.py --preset normal

# Test custom flow parameters
.\python_env\python.exe src/predict_flow.py --proto tcp --service http --dur 0.02 --sbytes 95000 --sttl 254 --rate 50000
```

### 3. Re-train Models or Run EDA Analysis
To re-run the exploratory analysis or re-train models:
```bash
# Run Exploratory Data Analysis
.\python_env\python.exe src/eda_analyzer.py

# Re-train Machine Learning Classifiers
.\python_env\python.exe src/train_models.py
```

---

## Automated Incident Response & Playbooks

When a cyber attack flow is detected, CyberSentinel AI dynamically generates defensive countermeasures:
- **Linux Iptables Rule**: Immediate stateful packet dropping or rate-limiting for offending IPs/protocols.
- **Suricata / Snort Rule**: Intrusion Prevention System (IPS) signature for edge perimeter blocking.
- **NIST SP 800-61 Checklist**: Triage actions covering Containment, Host Isolation, PCAP Logging, and Forensics.

---

## References & Citation
- Moustafa, Nour, and Jill Slay. *"UNSW-NB15: a comprehensive data set for network intrusion detection systems (UNSW-NB15 network data set)."* 2015 Military Communications and Information Systems Conference (MilCIS). IEEE, 2015.
- Australian Centre for Cyber Security (ACCS), UNSW Canberra.
- MITRE ATT&CK Framework Enterprise Matrix.
