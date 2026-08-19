# AI Market Analysis Platform

AI Market Analysis Platform is an academic project developed as part of the **Artificial Intelligence and Autonomous Learning** course at **Escuela Politécnica Nacional (EPN)**.

The project combines financial-market visualization, Smart Money Concepts (SMC), statistical analysis, temporal feature engineering, and machine learning techniques to analyze market behavior and model future traces after detected market events.

The system integrates an interactive TradingView-inspired visualization environment developed primarily in **Perl/Tk** with a machine learning pipeline based on:

**t-SNE → Hidden Markov Model → Gaussian Mixture Model**

> This project was developed for academic and educational purposes. It is not intended to provide financial advice, investment recommendations, or production trading signals.

---

## Project Overview

The project was developed as an extension of an interactive financial-market visualization platform.

The application processes OHLCV market data and combines graphical market analysis with statistical and machine learning techniques.

The platform includes concepts and indicators such as:

* Market structure.
* Average True Range (ATR).
* Volume analysis.
* Liquidity levels.
* Break of Structure (BOS).
* Change of Character (CHoCH).
* Equal Highs and Equal Lows.
* Fair Value Gaps (FVG).
* Order Blocks.
* Buy-Side Liquidity (BSL).
* Sell-Side Liquidity (SSL).
* Fibonacci levels.
* VWAP.
* Volume Profile.
* Support and resistance.
* Multi-timeframe context.
* Supply and demand zones.
* Trend and channel analysis.

These elements are also used to generate causal information for the machine learning pipeline.

---

## System Architecture

The project can be divided into three main areas:

### Market Visualization

An interactive graphical environment inspired by TradingView for displaying and analyzing financial-market information.

### Market Analysis

Technical and structural market-analysis components based on indicators, liquidity, market structure, and Smart Money Concepts.

### Machine Learning

A temporal machine learning pipeline that combines dimensionality reduction, sequential modeling, and probabilistic clustering.

The final pipeline follows the sequence:

```text
Market Data
    |
    v
Causal Feature Engineering
    |
    v
Feature Scaling
    |
    v
t-SNE
    |
    v
Hidden Markov Model
    |
    v
Gaussian Mixture Model
    |
    v
Target Estimation
    |
    v
Temporal TEST Evaluation
```

---

# Market Visualization

The project includes an interactive graphical interface developed primarily using **Perl and Tk**.

The visualization environment provides functionality including:

* Candlestick charts.
* Multiple timeframes.
* Zoom.
* Horizontal scrolling.
* Crosshair interaction.
* Market replay.
* ATR visualization.
* Market structure overlays.
* Liquidity visualization.
* Smart Money Concepts.
* Trend and channel visualization.
* Support and resistance.
* Multi-timeframe analysis.

The graphical component was designed to provide an environment similar to financial charting platforms while allowing custom indicators and machine learning components to be integrated into the same application.

---

# Market Structure and SMC

The system implements several Smart Money Concepts and structural market-analysis components.

These include:

* Higher High (HH).
* Higher Low (HL).
* Lower High (LH).
* Lower Low (LL).
* Break of Structure (BOS).
* Change of Character (CHoCH).
* Buy-Side Liquidity.
* Sell-Side Liquidity.
* Equal Highs.
* Equal Lows.
* Fair Value Gaps.
* Order Blocks.
* Internal and external market structure.
* ZigZag structures.
* Fibonacci-based analysis.
* Trendlines.
* Parallel channels.

These structures are calculated using historical information available at each point in time to preserve temporal causality.

---

# Machine Learning Pipeline

The final machine learning model combines three principal techniques:

1. t-SNE.
2. Hidden Markov Model.
3. Gaussian Mixture Model.

The complete pipeline is designed around chronological separation between training and testing information.

---

## 1. Feature Engineering

Market information is transformed into numerical causal features before entering the machine learning pipeline.

The final experiment uses **132 causal input features**.

Examples of information represented in the feature space include:

* ATR expressed in PIPs.
* Volume.
* Volume EMA.
* VWAP.
* Volume Profile POC.
* Volume Profile VAH.
* Volume Profile VAL.
* Fibonacci 0.382.
* Fibonacci 0.500.
* Fibonacci 0.618.
* Support levels.
* Resistance levels.
* Trend information.
* Channel information.
* Multi-timeframe support and resistance.
* Supply zones.
* Demand zones.
* Distance to liquidity structures.
* Distance to Fair Value Gaps.
* Distance to Order Blocks.
* Distance to Equal Highs and Equal Lows.
* Distance to BOS and CHoCH structures.

Only information available at the corresponding point in time is used to construct the machine learning input.

---

## 2. t-SNE

**t-Distributed Stochastic Neighbor Embedding (t-SNE)** is used to reduce the scaled causal feature space to a two-dimensional representation.

The dimensionality-reduction stage is trained exclusively using TRAIN information.

### Final configuration

* Input features: **132**
* TRAIN anchors: **90**
* Output dimensions: **2**
* Perplexity: **25**
* Final KL divergence: **0.3277**

Because t-SNE does not provide a native transformation function for unseen observations, non-anchor events and TEST samples are projected using **k-nearest-neighbor interpolation based exclusively on TRAIN anchors**.

This allows the TEST dataset to remain isolated from the t-SNE training process.

---

## 3. Hidden Markov Model

A **Hidden Markov Model (HMM)** is applied to the t-SNE representation to model temporal market behavior.

The final model contains three latent market states:

* `QUIET`
* `ACTIVE`
* `INTENSE`

These states represent different behavioral regimes detected from sequential market information.

The HMM is trained exclusively with chronological TRAIN data.

### Final HMM configuration

* Hidden states: **3**
* TRAIN log-likelihood: **-1713.1207**

The HMM provides temporal-state information and posterior probabilities that are subsequently incorporated into the next stage of the pipeline.

---

## 4. Gaussian Mixture Model

The final probabilistic stage uses a **Gaussian Mixture Model (GMM)**.

The GMM receives information derived from:

* t-SNE coordinates.
* HMM posterior probabilities.
* Selected causal market features.

The number of Gaussian components is selected using the **Bayesian Information Criterion (BIC)**.

### Final GMM configuration

* Gaussian components: **7**
* Selected causal features: **39**
* BIC: **-47712.1738**
* Convergence: **successful**

The resulting clusters represent probabilistic groupings of market events with similar characteristics.

---

# Temporal Experimental Design

A fundamental objective of the project was to maintain chronological separation between model development and final evaluation.

The dataset was divided into two temporal periods.

## TRAIN

Training information corresponds to:

**April – June 2026**

Number of detected TRAIN events:

**433**

TRAIN data is used for:

* Feature scaling.
* t-SNE construction.
* HMM training.
* GMM training.
* Cluster analysis.
* Target mapping.

---

## TEST

Final evaluation information corresponds to:

**July 1 – July 24, 2026**

Number of detected TEST events:

**107**

TEST information does **not** participate in:

* Feature scaling.
* t-SNE training.
* HMM training.
* GMM training.
* Target mapping.

The TEST period is used only after the training process has been completed.

This separation was implemented to reduce temporal information leakage and provide a more realistic evaluation of unseen market events.

---

# Prediction Targets

For each detected event, the system generates four numerical targets:

```text
target_trace_3
target_trace_5
target_trace_10
target_trace_15
```

These represent cumulative traces observed after an event at horizons of:

* 3 minutes.
* 5 minutes.
* 10 minutes.
* 15 minutes.

The objective is to estimate the subsequent market trace associated with each detected event.

---

# Final TEST Results

The final pipeline was evaluated using the independent July TEST period.

| Horizon | Model MAE | Model RMSE | Exact Accuracy | Accuracy ±1 | Baseline MAE | MAE Improvement |
| ------- | --------: | ---------: | -------------: | ----------: | -----------: | --------------: |
| 3 min   |    0.9182 |     1.0752 |         27.10% |      85.05% |       0.9260 |           0.84% |
| 5 min   |    1.1427 |     1.4193 |         27.10% |      69.16% |       1.1698 |           2.32% |
| 10 min  |    1.5758 |     1.9381 |         20.56% |      53.27% |       1.6479 |           4.38% |
| 15 min  |    2.1674 |     2.7665 |         13.08% |      44.86% |       2.2697 |           4.51% |

The final model achieved a modest MAE improvement over the baseline across all evaluated horizons.

These results are reported transparently as part of the academic experimentation process rather than being presented as evidence of a production-ready predictive trading system.

---

# Model Verification

The repository includes several verification and audit artifacts used to validate the final pipeline.

These include:

* Data audit.
* Model evidence.
* Final verification.
* Demonstration samples.
* Final machine learning report.
* t-SNE/HMM/GMM visualization.

The verification process checks aspects such as:

* Correct TRAIN/TEST separation.
* Feature consistency.
* Model availability.
* Pipeline execution.
* Final model configuration.
* Evaluation outputs.

---

# Technologies

## Programming

* Perl

## User Interface

* Perl/Tk

## Machine Learning

* t-SNE
* Hidden Markov Models
* Gaussian Mixture Models
* k-NN interpolation
* Probabilistic clustering

## Statistical and Data Analysis

* Mean.
* Variance.
* Covariance.
* Correlation.
* Feature scaling.
* Temporal feature engineering.
* Baseline comparison.
* MAE.
* RMSE.
* Accuracy-based evaluation.

## Financial Market Analysis

* OHLCV data.
* Smart Money Concepts.
* Market structure.
* Liquidity analysis.
* ATR.
* VWAP.
* Volume Profile.
* Fibonacci analysis.
* Multi-timeframe analysis.

## Development Environment

* Linux
* Git
* GitHub

---

# Project Structure

```text
AI-Market-Analysis-Platform/
|
|-- src/
|   |-- market.pl
|   |-- final_ml_pipeline.pl
|   |-- final_ml_demo.pl
|   |-- verify_final_ml.pl
|   |
|   `-- Market/
|       |-- ML/
|       |-- Indicators/
|       |-- Overlays/
|       |-- Panels/
|       |-- Debug/
|       |-- ChartEngine.pm
|       |-- IndicatorManager.pm
|       `-- MarketData.pm
|
|-- models/
|   `-- ghost_tsne_hmm_gmm.bin
|
|-- reports/
|   |-- FINAL_ML_REPORT.md
|   |-- DATA_AUDIT.txt
|   |-- MODEL_EVIDENCE.txt
|   |-- DEMO_SAMPLE.txt
|   |-- VERIFICATION_PASS.txt
|   `-- tsne_hmm_gmm.svg
|
|-- docs/
|   |-- README_FINAL_ML.md
|   `-- FINAL_CHECKLIST.md
|
|-- assets/
|   `-- images/
|
|-- .gitignore
`-- README.md
```

---

# Running the Project

The repository contains the final trained model and verification scripts.

## Verify the Final Model

From the project directory:

```bash
cd src
perl verify_final_ml.pl
```

This executes the verification process associated with the final machine learning model.

---

## Run the Final Demonstration

From the `src` directory:

```bash
perl final_ml_demo.pl --n 12
```

This produces a sample demonstration using the final trained model.

---

## Rebuild the Machine Learning Pipeline

If the required original datasets and dependencies are available, the complete machine learning pipeline can be rebuilt using:

```bash
perl final_ml_pipeline.pl --rebuild
```

The raw datasets used during academic development are not included in this portfolio repository.

---

## Run the Graphical Application

The graphical market-analysis environment can be started from the `src` directory using:

```bash
perl market.pl
```

The application requires the corresponding Perl modules and Tk graphical dependency.

---

# My Contribution

This was a collaborative academic project, although I was responsible for most of its design, implementation, integration, and experimentation.

My main contributions included:

* Development of the TradingView-inspired graphical interface.
* Implementation of candlestick visualization and market interaction.
* Implementation and integration of Smart Money Concepts.
* Development of market-structure visualization.
* Development of liquidity analysis and visualization.
* Implementation of indicators and market-analysis overlays.
* Implementation of multi-timeframe functionality.
* Development of replay functionality using historical information.
* Preparation and labeling of temporal market data.
* Statistical analysis and feature engineering.
* Development and integration of the t-SNE stage.
* Development and integration of the Hidden Markov Model stage.
* Development and integration of the Gaussian Mixture Model stage.
* Implementation of chronological TRAIN/TEST separation.
* Model evaluation and baseline comparison.
* Verification and auditing of the final machine learning pipeline.
* Integration of machine learning components with the overall market-analysis project.
* Organization and coordination of development activities.

Some components and development activities received support from other team members as part of the academic group project.

---

# What I Learned

This project provided practical experience combining several areas that are normally studied independently.

The development process involved:

* Designing a larger modular software project.
* Processing temporal financial data.
* Applying statistical concepts to real datasets.
* Building causal features.
* Working with dimensionality-reduction techniques.
* Modeling sequential information.
* Applying probabilistic clustering.
* Evaluating models on chronologically unseen data.
* Comparing model performance against baselines.
* Integrating machine learning with an interactive graphical application.
* Debugging and validating a multi-stage machine learning pipeline.
* Using Git and GitHub for project development and version control.

The project also reinforced the importance of separating model experimentation from final evaluation and avoiding information leakage when working with temporal datasets.

---

# Documentation and Evidence

Additional technical information is available inside the repository.

## Final Machine Learning Documentation

[`docs/README_FINAL_ML.md`](docs/README_FINAL_ML.md)

Detailed information about the final machine learning implementation.

## Final ML Report

[`reports/FINAL_ML_REPORT.md`](reports/FINAL_ML_REPORT.md)

Contains the final model configuration, evaluation results, and experiment summary.

## Data Audit

[`reports/DATA_AUDIT.txt`](reports/DATA_AUDIT.txt)

Contains information related to dataset and feature verification.

## Model Evidence

[`reports/MODEL_EVIDENCE.txt`](reports/MODEL_EVIDENCE.txt)

Contains evidence associated with the trained model and final configuration.

## Verification

[`reports/VERIFICATION_PASS.txt`](reports/VERIFICATION_PASS.txt)

Contains the result of the final verification process.

## Model Visualization

[`reports/tsne_hmm_gmm.svg`](reports/tsne_hmm_gmm.svg)

Contains the visualization associated with the final t-SNE, HMM, and GMM pipeline.

---

# Academic Context

**Institution:** Escuela Politécnica Nacional
**Degree:** Software Engineering
**Course:** Artificial Intelligence and Autonomous Learning
**Academic Period:** 2026-A
**Project Type:** Collaborative academic project

---

# Repository Purpose

This repository is maintained as part of my personal software-development portfolio.

Its purpose is to document the architecture, implementation, experimentation, and evaluation of the AI Market Analysis Platform while presenting the technical concepts applied during its development.

The repository contains selected source code, model artifacts, reports, and verification evidence from the final academic version of the project.

Raw datasets, intermediate experiments, temporary files, and academic delivery artifacts have intentionally been excluded to maintain a cleaner portfolio-oriented repository.

---

# Disclaimer

This software was developed exclusively for **academic and educational purposes**.

The project is an experimental application of statistical analysis and machine learning techniques to historical financial-market data.

It does **not** provide financial advice, investment recommendations, guaranteed predictions, or production trading signals. Past experimental results should not be interpreted as evidence of future financial-market performance.
