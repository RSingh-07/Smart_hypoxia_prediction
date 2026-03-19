# 🫁 Smart Hypoxia Prediction System for Critical Care

> An early warning system that uses machine learning to predict hypoxic events in ICU and peri-operative settings — giving clinical staff advance notice before oxygen saturation reaches dangerous levels.

---

## 📌 Overview

Traditional bedside monitors are reactive — they only alert staff *after* a patient's SpO₂ has already crossed a threshold. This project flips that paradigm by predicting hypoxia risk *before* it occurs, using continuous physiological time-series data and machine learning.

The system outputs a real-time, colour-coded risk score across three tiers:

| State | Colour | Meaning |
|-------|--------|---------|
| 🟢 Stable | Green | Low risk — no immediate action required |
| 🟡 Warning | Amber | Elevated risk — increased vigilance recommended |
| 🔴 Critical | Red | High risk — immediate clinical review advised |

---

## ✨ Key Features

- **Predictive, not reactive** — generates risk signals minutes before a hypoxic event
- **Multi-signal analysis** — uses SpO₂, heart rate, blood pressure, and respiratory rate together
- **Sliding window feature extraction** — captures trends, not just snapshot values
- **Two-model pipeline** — Logistic Regression baseline + Random Forest as primary classifier
- **Class imbalance handling** — evaluated with AUROC and AUPRC; trained with balanced class weights
- **Gradio dashboard** — lightweight, real-time, interpretable UI for bedside use
- **Three-tier alert system** — reduces alarm fatigue compared to binary threshold alarms

---

## 🏗️ System Architecture

```
Physiological Signals (SpO₂, HR, BP, RR)
        ↓
  Data Preprocessing
  (artifact removal, interpolation, per-patient min-max normalization)
        ↓
  Temporal Segmentation
  (overlapping fixed-length sliding windows)
        ↓
  Feature Extraction
  (mean, median, std, IQR, slope, rate of change, skewness, kurtosis)
        ↓
  ML Classifier (Random Forest)
        ↓
  Risk Score → [Stable | Warning | Critical]
        ↓
  Colour-coded Gradio Dashboard
```

---

## 📊 Model Performance

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| Logistic Regression | 0.82 | 0.80 | 0.78 | 0.79 |
| **Random Forest** | **0.89** | **0.87** | **0.86** | **0.86** |

- **AUROC:** 0.88 – 0.90 (Random Forest, cross-validated)
- Random Forest correctly anticipated the majority of hypoxic events with a meaningful lead time before SpO₂ breached dangerous levels

---

## 🗂️ Dataset

- **Source:** VitalDB — multi-channel physiological recordings from ICU and peri-operative patients
- **Size:** ~300 patient cases
- **Signals:** SpO₂, Heart Rate, Systolic BP, Diastolic BP, Respiratory Rate
- **Labelling:** Binary — whether a hypoxic event occurs within a look-ahead window
- **Class imbalance:** Hypoxic windows are a small minority of total samples (handled via class weighting)

---

## 🛠️ Tech Stack

| Component | Tools |
|-----------|-------|
| Language | Python 3 |
| Environment | Google Colab |
| Data Handling | NumPy, Pandas |
| ML | Scikit-learn (Logistic Regression, Random Forest) |
| Imbalanced Learning | imbalanced-learn |
| Deep Learning (optional) | TensorFlow / Keras |
| Data Source | VitalDB (`vitaldb`), WFDB (`wfdb`) |
| Visualization | Matplotlib, Seaborn |
| Dashboard | Gradio |

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/smart-hypoxia-prediction.git
cd smart-hypoxia-prediction
```

### 2. Install dependencies

```bash
pip install vitaldb scikit-learn imbalanced-learn tensorflow pandas numpy matplotlib seaborn gradio wfdb
```

### 3. Run the notebook

Open `smart_hypoxia.ipynb` in Google Colab or Jupyter and run all cells sequentially.

### 4. Launch the dashboard

```bash
python app_gradio.py
```

The dashboard will be available at `http://127.0.0.1:7860` or via a public Gradio share link.

---

## 📁 Project Structure

```
smart-hypoxia-prediction/
│
├── smart_hypoxia.ipynb       # Main notebook (data loading, training, evaluation)
├── app_gradio.py             # Gradio dashboard application
├── README.md                 # You're here
└── requirements.txt          # (optional) dependency list
```

---

## 🔬 Methodology Summary

1. **Preprocessing** — Remove artefacts, forward-fill/interpolate short gaps, apply per-patient min-max normalization
2. **Segmentation** — Divide signals into overlapping windows to capture physiological transitions
3. **Feature Extraction** — Compute 8 statistical descriptors per channel per window
4. **Feature Selection** — Retain variables with strongest predictive signal
5. **Training** — Stratified k-fold cross-validation; class-weight balancing; grid search for hyperparameters
6. **Evaluation** — AUROC, AUPRC, event-level detection rate, mean warning lead time

---

## ⚠️ Limitations

- Dataset size (~300 patients) is sufficient for exploratory validation but smaller than some comparable studies
- Evaluation is offline (cross-validation); prospective live-environment validation is a next step
- Patient-specific context (comorbidities, medications, surgical history) not yet incorporated

---

## 🔭 Future Work

- Integrate deep learning architectures (LSTMs, Transformers) for long-range temporal dependencies
- Add SHAP-based explanations so each risk score is transparent to clinicians
- Incorporate additional clinical variables (blood pressure, respiratory rate, patient history)
- Deploy within a live hospital monitoring platform for real-world prospective validation

---

## 👩‍💻 Team

Developed as a capstone project at the **Department of Computer Science & Engineering, SOET, CMR University, Bengaluru**.

| Name | Email |
|------|-------|
| Shalini Kumari | shalini.hajipur@gmail.com |
| Rakshitha Poleneni | poleneni.rakshitha@gmail.com |
| Shaik Misba | misbashaik0320@gmail.com |
| Riya Singh | 537riya@gmail.com |
| Sindhu MS | sindhusrinivas902@gmail.com |

---

## 📄 Research Paper

An accompanying research paper — *"Smart Hypoxia Prediction System for Critical Care"* — is currently under review.

---

## 📜 License

This project is intended for academic and research purposes. Please contact the team before any clinical deployment or commercial use.
