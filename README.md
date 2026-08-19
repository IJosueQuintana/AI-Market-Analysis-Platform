# AI Market Analysis Platform

Academic machine learning project developed as part of the Artificial Intelligence and Autonomous Learning course at Escuela Politécnica Nacional (EPN).

The project combines financial-market visualization, Smart Money Concepts (SMC), statistical analysis, temporal feature engineering, and machine learning techniques to analyze market behavior and model future traces after detected market events.

The system includes an interactive market-analysis interface developed in Perl, together with a machine learning pipeline based on:

**t-SNE -> Hidden Markov Model -> Gaussian Mixture Model**

---

## Project Overview

The project was developed as an extension of a TradingView-inspired market visualization platform.

The application processes OHLCV market data and integrates technical and structural market information, including:

- Market structure.
- ATR.
- Volume.
- Liquidity levels.
- BOS and CHoCH.
- Equal Highs and Equal Lows.
- Fair Value Gaps.
- Order Blocks.
- Buy-Side and Sell-Side Liquidity.
- Fibonacci levels.
- VWAP.
- Volume profile.
- Support and resistance.
- Multi-timeframe context.
- Supply and demand zones.

These features are used to generate causal information for the machine learning pipeline.

---

## Machine Learning Pipeline

The final model follows the sequence:

### 1. t-SNE

t-SNE is used to reduce the scaled causal feature space to a two-dimensional representation.

The model is trained exclusively using TRAIN information.

Since t-SNE does not provide a native transform operation, non-anchor events and TEST samples are projected using k-NN interpolation based only on TRAIN anchors.

Configuration of the final experiment:

- 132 causal input features.
- 90 TRAIN anchors.
- Perplexity: 25.
- Final KL divergence: 0.3277.

---

### 2. Hidden Markov Model

A Hidden Markov Model is used to represent temporal market behavior over the t-SNE embedding.

The final model uses three latent states:

- QUIET
- ACTIVE
- INTENSE

The HMM models sequential transitions between these market states using only TRAIN data.

Final TRAIN log-likelihood:

`-1713.1207`

---

### 3. Gaussian Mixture Model

The final stage applies a Gaussian Mixture Model using information derived from:

- t-SNE coordinates.
- HMM posterior probabilities.
- Selected causal features.

The number of components is selected using the Bayesian Information Criterion (BIC).

Final configuration:

- 7 Gaussian components.
- 39 selected features plus embedding and HMM information.
- BIC: `-47712.1738`.
- Convergence: successful.

---

## Experimental Design

The dataset was separated chronologically to avoid information leakage.

### Training period

April, May and June 2026.

- 433 detected events.

### Test period

July 1–24, 2026.

- 107 detected events.

The TEST dataset does not participate in:

- Feature scaling.
- t-SNE training.
- HMM training.
- GMM training.
- Target mapping.

This separation was designed to preserve temporal causality during evaluation.

---

## Prediction Targets

For each detected event, the system generates four numerical targets:

- `target_trace_3`
- `target_trace_5`
- `target_trace_10`
- `target_trace_15`

These targets represent cumulative traces observed after the event at horizons of:

- 3 minutes.
- 5 minutes.
- 10 minutes.
- 15 minutes.

---

## Test Results

The final model was evaluated on the July TEST dataset.

| Horizon | Model MAE | Model RMSE | Exact Accuracy | Accuracy ±1 | Baseline MAE | MAE Improvement |
|---|---:|---:|---:|---:|---:|---:|
| 3 min | 0.9182 | 1.0752 | 27.10% | 85.05% | 0.9260 | 0.84% |
| 5 min | 1.1427 | 1.4193 | 27.10% | 69.16% | 1.1698 | 2.32% |
| 10 min | 1.5758 | 1.9381 | 20.56% | 53.27% | 1.6479 | 4.38% |
| 15 min | 2.1674 | 2.7665 | 13.08% | 44.86% | 2.2697 | 4.51% |

The results show a modest improvement over the baseline across all evaluated horizons.

This project is presented as an academic experimentation and learning exercise rather than a production-ready trading model.

---

## Data and Feature Engineering

The machine learning pipeline uses causal information derived from market data and multiple timeframes.

Examples include:

- ATR expressed in PIPs.
- Volume.
- EMA of volume.
- VWAP.
- Volume-profile POC, VAH and VAL.
- Fibonacci 0.382, 0.500 and 0.618 levels.
- Support and resistance.
- Trend and channel information.
- Multi-timeframe support and resistance.
- Supply and demand zones.
- Distance to liquidity structures.
- Distance to FVGs.
- Distance to Order Blocks.
- Distance to EQH/EQL.
- Distance to BOS/CHoCH.

---

## Market Visualization

The project also includes an interactive graphical interface inspired by TradingView.

The visualization system supports:

- Candlestick charts.
- Multiple timeframes.
- Zoom and scrolling.
- Replay functionality.
- ATR visualization.
- Market structure overlays.
- Liquidity visualization.
- SMC structures.
- Trend and channel visualization.

The interface was developed primarily using Perl and Tk.

---

## Project Structure

```text
AI-Market-Analysis-Platform/
|
|-- src/
|   |-- market.pl
|   |-- final_ml_pipeline.pl
|   |-- final_ml_demo.pl
|   |-- verify_final_ml.pl
|   `-- Market/
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
