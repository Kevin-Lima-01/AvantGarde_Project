# Dynamic Factor Allocation: Regime Identification and Prediction in the Brazilian Equity Market

This repository contains the research project developed in collaboration between **FGV Quant** and **AvantGarde Capital**. The study presents a replication and adaptation, for the Brazilian equity market, of a hybrid framework for identifying and predicting **allocation-focused regimes** applied to dynamic factor investing. My individual contribution focused on the **macroeconomic block of the XGBoost classifier**, responsible for predicting regimes out-of-sample using financial and macro features.

## Institutional Context

This project was conducted within **FGV Quant**, the quantitative finance research entity at Fundação Getulio Vargas, in partnership with **AvantGarde Capital**. The collaboration bridges academic rigor and industry application, aiming to develop implementable factor-based allocation strategies tailored to the Brazilian market.

## Core Concept

The central idea is to define regimes not as broad macroeconomic states, but as **persistent periods in which one investment strategy (or factor) systematically outperforms another**, making the regime directly actionable for allocation decisions. This contrasts with traditional macro-regime approaches by focusing on **relative factor performance** rather than aggregate economic conditions.

## Methodology

The framework decouples the problem into two distinct stages:

| Stage | Task | Method |
|-------|------|--------|
| **Stage 1: In-Sample Labeling** | Identify persistent outperformance regimes in historical data | Statistical model with an explicit **jump penalty** that penalizes frequent regime switches, ensuring stable and economically meaningful regime assignments |
| **Stage 2: Out-of-Sample Prediction** | Forecast regimes for forward-looking allocation | **XGBoost gradient-boosted tree classifier** trained on an enriched set of financial and macroeconomic features |

The link between stages is achieved through **performance-driven hyperparameter selection**: the jump penalty that maximizes risk-adjusted returns in validation windows is chosen, and the resulting labeler–classifier pair is applied to subsequent windows in a rolling procedure.

### My Contribution: Macroeconomic Block of the XGBoost Classifier

My primary responsibility was the **macroeconomic feature engineering and modeling block** of the XGBoost classifier. Given the constraints of the Brazilian market, the macro feature set was restricted to:

| Variable | Rationale |
|----------|-----------|
| **Exchange Rate (BRL/USD)** | Captures external sector dynamics and risk appetite toward Brazil |
| **Oil Price** | Proxies global commodity cycles and terms-of-trade shocks |
| **3-Month Interest Rate** | Reflects monetary policy stance and short-term funding conditions |

We acknowledge that the absence of desirable proxies — such as a long-dated volatility index or long-term interest rates — reduces the predictive efficiency of the macro block. Nonetheless, the parsimonious specification was deliberately chosen to avoid overfitting given data limitations.

## Data and Implementation

The Brazilian implementation required substantive adaptations:

| Challenge | Adaptation |
|-----------|------------|
| **Data Availability** | Sample restricted to **2000–2024** due to unavailability, low granularity, and heterogeneous reliability of historical Brazilian data |
| **Factor Portfolio Construction** | Long-only factor portfolios with monthly or annual rebalancing, market-cap-weighted, filtered by liquidity and price/volume consistency criteria |
| **Macro Feature Constraints** | Limited to exchange rate, oil price, and 3-month interest rate; lack of volatility indices and long-term rates reduces predictive efficiency |

## Key Results

The out-of-sample analysis evaluates both **active strategies** (single factor vs. equal-weighted benchmark) and **complementary strategies** (pairwise factor rotation). Notable findings include:

- 🔹 **Stability of the optimal jump penalty (λ\*)** across rolling windows, suggesting the regime structure is persistent and not an artifact of overfitting.
- 🔹 **Relationship between classification accuracy and economic performance:** higher predictive accuracy does not always translate into superior risk-adjusted returns, highlighting the importance of economic evaluation metrics over pure statistical measures.

## Limitations and Forward Bias Discussion

A critical methodological discussion addresses potential **forward bias** and its mechanisms:

- **Investable Universe Construction:** Risk of incorporating information not available at the time of decision-making.
- **Temporal Alignment of Fundamentals:** Ensuring macroeconomic data releases are synchronized with the investor's information set.
- **Reuse of Statistics with Future Information:** Artificial inflation of out-of-sample performance when statistics calculated with hindsight are used as features.

> *These considerations are essential for assessing the real-time robustness of the signal and avoiding inflated expectations when deploying the strategy in live trading environments.*
