# Forecasting ICU Vital Signs with Time-Series Foundation Models

MSc Data Analytics dissertation — **University of Galway**
Supervised by **Prof. Michael Madden** · in progress, expected August 2026

---

## Overview

Mechanical ventilation is a core life-support technology in the Intensive Care Unit (ICU), but deciding *when* a patient is ready to be weaned from the ventilator remains a difficult clinical judgement. This project studies a supporting question: **how accurately can modern time-series models forecast a patient's physiological signals in the hours around extubation?**

Rather than predicting weaning as a single yes/no outcome, this work forecasts the **future trajectory of 13 physiological signals** (heart rate, respiratory rate, SpO₂, blood pressures, FiO₂, PEEP, tidal volume, and related measures) at multiple horizons, and benchmarks a range of transformer and pretrained foundation models on that task.

## Models Benchmarked

| Category | Models |
| --- | --- |
| Transformers (trained from scratch) | iTransformer, GraFITi |
| Time-series foundation models (zero-shot) | Chronos-2, TimesFM, Moirai-2 |

GraFITi and the foundation models are evaluated on their ability to handle **irregularly sampled, partially observed** clinical time series without hand-built resampling.

## Dataset

- **AmsterdamUMCdb** — a large freely-available intensive-care database
- Standardised to the **OMOP Common Data Model (CDM)**
- Queried at scale through **Google BigQuery**
- Cohort: **1,600+ ICU patients** undergoing mechanical ventilation

## Pipeline

The preprocessing pipeline is designed to be **leakage-safe** so that no information from the future or from other patients contaminates training:

- Impossible-value filtering and de-duplication of raw measurements
- Imputation with an explicit **observation mask** (the model always knows which values were measured vs. filled)
- **Patient-level** train / validation / test splits (no patient appears in more than one split)
- Windowing at **+1h, +4h, and +8h** forecast horizons

## Evaluation

Models are compared with:

- **MAE** and **RMSE** — in native clinical units
- **MASE** — scale-free, computed against a **persistence baseline**, so results are comparable across signals

All metrics are **stratified by forecast horizon and by physiological signal** for a fair, per-variable comparison.

## Result (preliminary)

> **GraFITi** achieved the strongest overall accuracy, leading on **11 of 13 signals** with a mean **MASE of 0.79** — ahead of **iTransformer (0.82)** and **Chronos-2 (0.84)**.

Results are preliminary and subject to change ahead of final dissertation submission (August 2026).

## Tech Stack

`Python` · `PyTorch` · `Hugging Face Transformers` · `Google BigQuery` · `pandas` · `NumPy` · `Jupyter`

## Repository Structure

```
ICU-Weaning-Prediction/
├── notebooks/        # Exploration, preprocessing, model training & evaluation
├── img/              # Figures and result plots
├── Backups/          # Notebook backups
├── requirements.txt
└── README.md
```

## Author

**Atharva Hatolkar** — MSc Computer Science (Data Analytics), University of Galway
Portfolio: [hatolkarav.github.io](https://hatolkarav.github.io) · LinkedIn: [atharva-hatolkar](https://www.linkedin.com/in/atharva-hatolkar-b235541a5/)

---

*This repository accompanies an MSc dissertation and is shared for portfolio and reference purposes. The AmsterdamUMCdb data is not redistributed here and must be requested through its official access process.*
