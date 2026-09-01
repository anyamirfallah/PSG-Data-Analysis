# PSG Data Exploration and Analysis

## Overview

This project focuses on exploring and analyzing **Polysomnography (PSG)** data from EDF recordings and corresponding CSV-based annotations/features.

The goal of this work is to understand the structure of PSG signals from an engineering perspective, including:

* Signal channel identification
* Raw physiological signal inspection
* Epoch-based data organization
* Sleep stage analysis
* Respiratory event analysis
* Preparation of PSG data for machine learning applications

---

# Dataset Description

The dataset contains:

* A raw PSG recording in **EDF format**
* An extracted feature/annotation file in **CSV format**

The EDF file contains continuous physiological signals, while the CSV file contains 30-second epoch-level annotations and extracted features.

---

# EDF File Exploration

The EDF recording was loaded using **MNE-Python**.

```python
import mne

raw = mne.io.read_raw_edf(
    "file.edf",
    preload=True
)
```

## Recording Information

| Property           |          Value |
| ------------------ | -------------: |
| Number of channels |             43 |
| Sampling frequency |         256 Hz |
| Recording duration | ~20578 seconds |
| Duration           |     ~5.7 hours |
| Total samples      |      5,267,968 |

---

# PSG Channels

The recording contains multiple physiological signal groups:

## EEG Channels

Used for brain electrical activity and sleep staging:

* F4:A1
* F3:A2
* C4:A1
* C3:A2
* O2:A1
* O1:A2

Additional electrode channels:

* F3
* F4
* C3
* C4
* O1
* O2
* A1
* A2

---

## EOG Channels

Used for eye movement detection:

* EOG1:A2
* EOG2:A2
* EOG1:A1
* EOG2:A1
* EOG1
* EOG2

Applications:

* REM detection
* Eye movement analysis

---

## EMG Channels

Used for muscle activity:

* EMG
* EMG+
* EMG-
* PLMl
* PLMr

Applications:

* Muscle tone analysis
* Periodic limb movement detection
* REM-related muscle suppression analysis

---

## ECG and Pulse

Channels:

* ECG 2
* Pulse

Applications:

* Heart rate analysis
* Cardiac activity monitoring

---

## Respiratory Channels

Channels:

* Flow DVH
* Pressure Flow
* Thorax
* RIP Thorax
* RIP Abdomen
* Sum RIPs
* Leak DVH

Applications:

* Airflow analysis
* Respiratory effort estimation
* Apnea/Hypopnea detection

---

## Oxygen Saturation

Channel:

* SPO2

Application:

* Oxygen desaturation analysis

---

# EDF Annotation Analysis

The EDF annotations mainly contained CPAP/BiPAP titration events.

Examples:

```
5.0 cmH2O
6.0 cmH2O
...
25.0 cmH2O
```

The recording represents a **PAP titration PSG study**, where pressure settings were gradually adjusted.

The annotations also included calibration events:

* Eyes closed
* Eyes open
* Blink
* Horizontal eye movement
* Vertical eye movement
* Deep breaths
* Fast shallow breaths
* Stop breathing

No direct sleep stage annotations were found inside the EDF file.

---

# CSV Dataset Exploration

The CSV file contains epoch-level PSG analysis.

## Dataset Shape

```
685 rows × 219 columns
```

Each row represents:

```
1 epoch = 30 seconds
```

Total duration:

```
685 × 30 seconds ≈ 20550 seconds
```

which matches the EDF duration.

---

# Sleep Stage Distribution

The dataset contains the following sleep stage labels:

| Sleep Stage | Epoch Count |
| ----------- | ----------: |
| N3          |         209 |
| N2          |         198 |
| Wake        |         165 |
| REM         |          82 |
| N1          |          30 |
| A           |           1 |

Sleep stages available:

* Wake
* N1
* N2
* N3
* REM

The dataset is imbalanced, especially for N1 sleep.

For machine learning evaluation, metrics such as:

* Macro F1-score
* Balanced Accuracy
* Confusion Matrix

should be considered instead of accuracy alone.

---

# Respiratory Event Analysis

The CSV contains respiratory event labels:

| Event                             | Count |
| --------------------------------- | ----: |
| Normal                            |   291 |
| Hypopnea                          |   229 |
| Hypopnea, Hypopnea                |   129 |
| Obstructive Apnea                 |    15 |
| Obstructive Apnea + Hypopnea      |     9 |
| Hypopnea + Obstructive Apnea      |     6 |
| Multiple obstructive apnea events |     4 |

The dataset can be used for:

* Apnea detection
* Hypopnea classification
* Respiratory event analysis

---

# Extracted Features

The CSV contains 219 extracted features, including:

## Frequency Domain Features

Examples:

* Sigma FFT
* Delta FFT
* Alpha + Beta FFT
* Average frequency value

Applications:

* Sleep stage classification
* EEG spectral analysis

---

## Sleep-Specific Features

Examples:

* REM indicator
* Spindle and K-complex features
* CAP features

Applications:

* Sleep architecture analysis

---

## EMG Features

Examples:

* Integral EMG
* PLM events

Applications:

* Muscle activity analysis

---

# Possible Machine Learning Tasks

## 1. Sleep Stage Classification

Pipeline:

```
PSG Features
      |
      ↓
Machine Learning Model
      |
      ↓
Wake / N1 / N2 / N3 / REM
```

Possible models:

* Random Forest
* XGBoost
* SVM
* Neural Networks

---

## 2. Apnea Detection

Pipeline:

```
Respiratory Signals
        |
        ↓
Feature Extraction
        |
        ↓
Apnea / Normal Classification
```

Relevant signals:

* Airflow
* RIP Thorax
* RIP Abdomen
* SpO2
* Pulse

---

# Tools Used

* Python
* MNE-Python
* Pandas
* NumPy
* SciPy
* Scikit-learn

---

# Future Work

Possible next steps:

* Signal preprocessing and filtering
* Artifact detection
* EEG spectral analysis
* Epoch visualization
* Feature selection
* Sleep stage classification models
* Apnea detection models
* Deep learning approaches using raw PSG signals

---

# Conclusion

The dataset contains a complete PSG recording with multimodal physiological signals and corresponding epoch-level annotations.

The combination of EDF raw signals and CSV extracted features provides a suitable foundation for biomedical signal processing and machine learning experiments.
