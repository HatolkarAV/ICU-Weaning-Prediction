# Forecasting ICU Vital Signs Around Ventilator Weaning
MSc Data Analytics dissertation — **University of Galway**
Supervised by **Prof. Michael Madden** · in progress, expected August 2026

---

## Overview
Patients in intensive care are often kept alive by a mechanical ventilator. Deciding *when* a patient is ready to come off the ventilator (weaning) is a hard clinical call. This project looks at a supporting question: **how well can modern time-series models forecast a patient's vital signs in the hours around that decision?**

Instead of predicting a single yes/no "ready to wean" answer, the project forecasts the **near-future trajectory of 13 vital signs** — heart rate, respiratory rate, SpO₂, blood pressures, FiO₂, PEEP, tidal volume and related measures — a few hours ahead, and compares several models on that task.

## Models compared
Three kinds of model, one per "tier":

| Tier | Model | How it is used |
| --- | --- | --- |
| 1 | iTransformer | trained from scratch on the ICU data |
| 2 | Chronos-2 | a large pretrained model, used as-is with no training (zero-shot) |
| 3 | GraFITi | a graph model that reads the measurements at their true, irregular times |

GraFITi reads the raw, irregularly sampled data directly. The Tier 1 and Tier 2 models read a tidy hourly grid, together with an **observation mask** that marks which values were really measured and which were filled in.

## Data
- **AmsterdamUMCdb** — a large, freely available intensive-care database
- Converted to the **OMOP Common Data Model (CDM)**
- Queried at scale with **Google BigQuery**
- Cohort: **about 2,600 ventilation stays** (~1,900 usable after windowing)

## Two windowing designs
The project builds the prediction windows in two ways and compares them:

- **Around extubation (imv_end):** windows anchored to the moment the breathing tube is removed. Simple to set up, but it needs to know the extubation time in advance and quietly drops patients who die on the ventilator.
- **From ventilation start (imv_start):** windows anchored to when ventilation begins, then slid forward through the stay. This could run in real time and keeps every patient — the more realistic, deployable design.

## Pipeline
The preprocessing is built to be **leakage-safe**, so no information from the future or from other patients leaks into training:

- Remove impossible values and duplicate measurements
- Fill gaps but keep an **observation mask** (the model always knows measured vs. filled)
- Split the data **by patient** (no patient appears in more than one split)
- Build forecast windows at **+1h, +4h and +8h** ahead

## Evaluation
Models are compared with:

- **MAE** and **RMSE** — in real clinical units
- **MASE** — a scale-free score measured against a **persistence baseline** ("assume the last value stays the same"), so results are comparable across very different signals

Every score is broken down **by forecast horizon and by signal**, for a fair per-variable comparison.

## Result (preliminary)
> **GraFITi** gave the best accuracy: the lowest error on **all 13 signals**, with a mean **MASE of about 0.79** — ahead of **iTransformer (0.82)** and **Chronos-2 (0.84)**. All models beat the **persistence baseline (1.00)**, and the same ranking held under **both** windowing designs.

Results are preliminary and may change before final submission (August 2026).

## Tech stack
`Python` · `PyTorch` · `Hugging Face Transformers` · `Google BigQuery` · `pandas` · `NumPy` · `Jupyter`

## Repository structure
```
ICU-Weaning-Prediction/
├── notebooks/        # exploration, preprocessing, model training & evaluation
├── img/              # figures and result plots
├── Backups/          # notebook backups
├── requirements.txt
└── README.md
```

## Author
**Atharva Hatolkar** — MSc Computer Science (Data Analytics), University of Galway
Portfolio: [hatolkarav.github.io](https://hatolkarav.github.io) · LinkedIn: [atharva-hatolkar](https://www.linkedin.com/in/atharva-hatolkar-b235541a5/)

---
*This repository accompanies an MSc dissertation and is shared for portfolio and reference. The AmsterdamUMCdb data is not redistributed here and must be requested through its official access process.*
