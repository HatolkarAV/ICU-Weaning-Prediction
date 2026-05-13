# Predicting Successful Weaning from Mechanical Ventilation Using Time-Series Deep Learning

## Overview

Mechanical ventilation is essential in Intensive Care Units (ICUs), but determining the optimal timing for extubation remains a major clinical challenge. Premature extubation can lead to reintubation and complications, while delayed weaning increases ICU stay duration and healthcare burden.

This capstone project investigates whether time-series deep learning models can improve prediction of successful weaning from mechanical ventilation using ICU physiological data from AmsterdamUMCdb.

The project focuses on sequential modelling of physiological and ventilator-related variables preceding extubation using:

- Long Short-Term Memory (LSTM)
- Gated Recurrent Unit (GRU)
- Temporal Convolutional Network (TCN)

Successful weaning is defined as the absence of reintubation within 48 hours of extubation.

---

## Objectives

- Explore and understand AmsterdamUMCdb OMOP-CDM structure
- Identify mechanically ventilated ICU patients
- Detect extubation and reintubation events
- Extract physiological time-series preceding extubation
- Build deep learning models for weaning prediction
- Compare LSTM, GRU, and TCN performance using:
  - ROC-AUC
  - Precision
  - Recall
  - F1-score

---

## Dataset

- AmsterdamUMCdb
- ICU electronic health record database
- OMOP Common Data Model (CDM) format
- Accessed through Google BigQuery

---

## Current Progress

- Repository initialized and project structure created
- Connected AmsterdamUMCdb using Google BigQuery
- Explored OMOP-CDM database schema
- Identified important tables for analysis:
  - measurement
  - device_exposure
  - procedure_occurrence
  - visit_occurrence
  - concept
- Started exploratory data analysis (EDA) on the `measurement` table
- Investigating respiratory and ventilation-related concepts
- Exploring timestamp structure and time-series density

---

## Repository Structure

```text
icu-weaning-prediction/
│
├── notebooks/          # Jupyter notebooks for exploration and experiments
├── src/                # Reusable preprocessing and modeling scripts
├── docs/               # Notes and documentation
├── models/             # Saved model artifacts
├── results/            # Figures, metrics, and outputs
├── data/               # Local ignored data folders
├── README.md
├── requirements.txt
└── .gitignore