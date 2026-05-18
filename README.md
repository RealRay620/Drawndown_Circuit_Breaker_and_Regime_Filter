# Circuit Breaker & Regime Filter Trading System

## Overview

This project develops a systematic trend-following trading framework designed to improve risk-adjusted performance and reduce capital erosion during market stress periods. The strategy integrates a broad market regime filter with a portfolio-level drawdown circuit breaker to create a combined proactive and reactive risk management system.

The framework was implemented using QuantConnect LEAN in Python and tested across multiple normal and crisis market environments to evaluate robustness, drawdown control, and risk-adjusted return stability.

The strategy was specifically designed to address weaknesses identified during earlier backtests:

- excessive exposure during unfavorable macro trends
- concentration-driven drawdowns during crisis periods

To mitigate these issues, the project introduced:

- a 200-day SPY trend regime filter
- a portfolio-level drawdown circuit breaker

---

# Research Objective

The primary objective of this project was to:

- reduce maximum drawdown during market crises
- improve risk-adjusted returns
- suppress counter-trend trading behavior
- preserve portfolio capital during severe market stress
- evaluate the interaction between trend filtering and portfolio risk controls

The project combines concepts from:

- systematic trading
- trend following
- regime detection
- portfolio risk management
- volatility targeting
- algorithmic trading

---

# Strategy Hypothesis

The combined hypothesis of the framework was:

> Integrating broad market trend filtering with portfolio capital failsafes creates a proactive/reactive risk shield capable of reducing drawdowns and improving risk-adjusted returns.

The project specifically targeted improvements in:

- Sharpe Ratio
- Sortino Ratio
- Calmar Ratio
- Maximum Drawdown

---

# Core Framework

The strategy combines two major components:

## Regime Filter

The regime filter uses the SPY 200-day simple moving average (SMA) to suppress trades that conflict with the broader market trend.

### Regime Logic

```math
SPY_t > SMA_{200} \Rightarrow \text{Bull Regime}
```

```math
SPY_t < SMA_{200} \Rightarrow \text{Bear Regime}
```

### Signal Suppression Rules

- Long signals are suppressed during bear regimes
- Short signals are suppressed during bull regimes

This prevents the strategy from trading against dominant macro momentum.

---

# Drawdown Circuit Breaker

The circuit breaker dynamically reduces portfolio exposure when portfolio drawdowns exceed a specified threshold.

## Drawdown Calculation

```math
Drawdown_t = \frac{NAV_t - PeakNAV_t}{PeakNAV_t}
```

## Circuit Breaker Rule

```math
Drawdown_t \leq -10\% \Rightarrow \text{Flatten Portfolio}
```

When triggered:

- all portfolio weights are scaled to zero
- exposure is eliminated
- capital preservation becomes the priority

The system automatically resets after portfolio stabilization.

---

# Alpha Model Framework

The strategy used a composite trend-following alpha model built across multiple time horizons.

## Signal Horizons

| Horizon | Window |
|---|---|
| Short-Term | 20 Days |
| Medium-Term | 63 Days |
| Long-Term | 252 Days |

Signals were normalized using ATR-based scaling and combined into a composite trend score.

---

# Portfolio Construction

The portfolio construction engine incorporated:

- volatility targeting
- exposure scaling
- gross exposure limits
- net exposure limits
- position size controls

## Portfolio Constraints

| Constraint | Value |
|---|---|
| Target Annual Volatility | 10% |
| Maximum Gross Exposure | 1.50 |
| Maximum Net Exposure | 0.50 |
| Maximum Position Weight | 10% |

---

# Methodology

The workflow consisted of:

1. Market data collection and preprocessing  
2. Trend signal generation  
3. SPY regime detection  
4. Volatility-adjusted portfolio construction  
5. Drawdown monitoring and exposure scaling  
6. Rolling backtesting across multiple market regimes  
7. Performance attribution and robustness analysis  

---

# Backtesting Framework

The framework was evaluated across both normal and crisis market environments.

## Market Regimes Tested

| Regime | Time Period |
|---|---|
| N1 | 2012–2014 |
| N2 | 2016–2018 |
| C1 | 2007–2009 |
| C2 | 2020–2022 |

---

# Performance Metrics

The strategy was evaluated using:

- Total Return
- Sharpe Ratio
- Sortino Ratio
- Calmar Ratio
- Maximum Drawdown

---

# Key Results

## Crisis Market Improvement

### 2020–2022 Regime

| Metric | Baseline | Modified |
|---|---|---|
| Total Return | -5.51% | 6.57% |
| Sharpe Ratio | -0.201 | 0.278 |
| Max Drawdown | 15.9% | 12.1% |

The combined overlays significantly improved crisis-period performance while reducing drawdown severity.

---

# Key Findings

## Regime Filter

- reduced counter-trend exposure
- prevented long exposure during major crashes
- reduced short exposure during recovery rallies

## Drawdown Circuit Breaker

- reduced concentration-driven losses
- dynamically scaled exposure during severe drawdowns
- improved capital preservation

---

# Key Insights

- Trend filtering materially improves crisis-period stability
- Portfolio-level risk controls complement directional filters
- Drawdown protection improved risk-adjusted returns
- Risk overlays remained mostly inactive during normal markets
- Combined overlays produced more stable Sharpe and Calmar ratios across regimes

---

# Limitations

Several limitations were identified:

- the 200-day SMA signal is inherently lagging
- the DOW 30 universe introduces survivorship bias
- limited historical regime windows reduce statistical significance
- additional overlays increased trading frequency and transaction costs

---

# Technology Stack

- Python
- QuantConnect LEAN
- Pandas
- NumPy
- Yahoo Finance API
- Algorithmic Trading Frameworks
- Portfolio Analytics
- File I/O

---

# References

- Moskowitz, Tobias, Yao Hua Ooi, and Lasse Pedersen. *Time Series Momentum.*

- Chan, Ernest. *Algorithmic Trading: Winning Strategies and Their Rationale.*

- López de Prado, Marcos. *Advances in Financial Machine Learning.*

- Faber, Mebane. *A Quantitative Approach to Tactical Asset Allocation.*

- Grinold, Richard, and Ronald Kahn. *Active Portfolio Management.*

- QuantConnect LEAN Documentation.
