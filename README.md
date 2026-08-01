# data-quality-pipelines
Demonstration of data validation using great expectations to maintain data integrity and consistency

# Enterprise Data Quality & Anomaly Detection Pipeline

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.2.2-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-2.0.2-013243?logo=numpy&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Latest-F7931E?logo=scikit-learn&logoColor=white)
![Great Expectations](https://img.shields.io/badge/Great_Expectations-1.19.1-FD4F00?logo=great-expectations&logoColor=white)

## Overview

In production Machine Learning (ML) environments, data quality is infrastructure, not just a cleanup step. When schemas evolve silently or business behaviors change, automated quality checks are the only defense against silent model degradation and pipeline failures. 

This repository implements a systematic data quality pipeline acting as a rigorous "quality gate" between raw ingestion and downstream ML model consumption. It establishes clear data contracts, handles complex missing data topologies, and enforces multi-layered anomaly detection to ensure all ingested data is statistically and structurally sound.

## Dataset

This pipeline is designed to process and validate the dataset named cars24-car-price-cleaned-new.csv. 

## Core Architecture

### 1. Data Validation Gate (Great Expectations)
Instead of assuming data correctness, this module explicitly validates incoming batches against a predefined Expectation Suite (our Data Contract).
* **Structural Checks:** Validates schema integrity, column presence, and expected data types.
* **Content Checks:** Assesses missing values, valid ranges, and allowed categorical limits (e.g., ensuring `km_driven` falls within specific operational boundaries).
* **Outputs:** Generates human-readable Data Docs and pass/fail metrics used to programmatically halt or warn the CI/CD or orchestration layer.

### 2. Intelligent Imputation Engine
Missing data is handled based on its underlying mechanism: Missing Completely At Random (MCAR), Missing At Random (MAR), or Missing Not At Random (MNAR). 
* **Numerical Imputation:** Utilizes median-based `SimpleImputer` to prevent distribution skewing.
* **Categorical Imputation:** Falls back to `most_frequent` class strategies for categorical vectors.
* **Note:** Imputation is treated as a business-logic decision, as improper strategies can introduce bias or break model fairness.

### 3. Multi-Layer Anomaly Detection
Anomalies represent data points that are technically valid but violate business logic or expected distributions. This pipeline flags anomalies across three distinct layers:
* **Rule-Based (Business Constraints):** Hard thresholds dictated by domain experts (e.g., selling prices outside of ₹50,000 – ₹50,00,000 bounds or impossible mileage metrics).
* **Statistical (Z-Score / IQR):** Outlier detection using Interquartile Range (IQR) bounds to catch distribution drift.
* **Model-Based (Isolation Forest):** An unsupervised `IsolationForest` model trained to learn normal feature behavior and detect complex, multi-variate pattern deviations (configured for a 2% contamination rate).

## Production Telemetry: Fail vs. Warn

A critical component of this MLOps architecture is deciding pipeline execution flow based on validation results:
* **FAIL (Block downstream usage):** Triggered when compliance is violated, business-critical metrics are fundamentally wrong, or model inputs are corrupted.
* **WARN (Alert and proceed):** Triggered for minor statistical drift or missing non-critical fields.

## Getting Started

### Prerequisites
Ensure your virtual environment is active and install the required dependencies:

```bash
pip install great_expectations pandas numpy scikit-learn
```
---

## 🤝 Contributing

Contributions to improve the examples, add new functions or methods, or fix typos are always welcome. Please feel free to open an issue or submit a pull request!

---

## Connect with me
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/abhay-kumar-sharma-a22a94171)
