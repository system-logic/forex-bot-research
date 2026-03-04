# VIDEO 1 — PROJECT STATE

## 1. Market Selection

Chosen market: Forex

Reasoning:
- Relative pricing between economies
- Cyclical and regime-based structure
- Moderate volatility
- Leverage availability
- High liquidity on major pairs
- Lower transaction costs compared to many markets


## 2. Structural Characteristics (Constant)

- 24/5 trading cycle
- Session-based activity differences (Asia / London / New York)
- Decentralized market (broker-dependent execution)
- High liquidity on major pairs
- Leverage impact
- Low transaction costs (spread + swap)
- Sensitivity to macroeconomic news


## 3. Dynamic Characteristics (Real-Time Context)

- Market regime (trend / range / chop)
- Current volatility
- Session activity intensity
- Strength of price movement (momentum)
- Inter-pair correlation
- Proximity of high-impact news
- Carry / swap impact for multi-day holding


## 4. Trading Scope

Type: Medium-term trading  
Holding period: Several hours to several days  

Pairs:
- EUR/USD
- GBP/USD
- USD/JPY
- AUD/USD
- USD/CAD

Sessions:
- London
- New York
- Overlap preferred
- Asian session excluded (initial stage)


## 5. Core Filters (Minimum Context for Trade)

A trade is allowed only if:

1. Active session
2. Regime matches strategy type
3. Volatility within acceptable range
4. No high-impact news nearby
5. No abnormal market state
6. Acceptable spread / liquidity
7. Risk per trade within 0.5–2%


## 6. Architectural Direction

Two possible architectures:

1. Static profit-seeking system
2. Adaptive regime-aware system

Chosen direction: Adaptive architecture

Structure:
- Strategy layer
- Meta-layer (regime detection, activation control, parameter adjustment)

Critical node:
Market regime detection


## 7. Edge Hypothesis Directions (Draft)

Hypothesis 1:
Session-based impulse continuation.

Hypothesis 2:
Mean deviation reversion under volatility-adjusted extremes.

Hypothesis 3:
Liquidity capture behavior near structural highs/lows.


---

Status after Video 1:
Environment defined.
Constraints defined.
Architectural direction chosen.
Edge hypotheses outlined.
Implementation not started.
