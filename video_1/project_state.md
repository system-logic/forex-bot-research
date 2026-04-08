# Video 1 — Environment Analysis and Edge Hypotheses

**Part of:** Forex Bot Research  
**Focus:** Understanding the trading environment before writing any code

## Goal of the Video
To thoroughly analyze the Forex market as a non-stationary environment and lay the foundation for building an adaptive mid-term trading bot.  
No code or specific strategy in this episode — only deep environment mapping and architectural decisions.

## Key Decisions Made

### 1. Market Choice
- **Selected:** Forex (currency pairs)
- **Rejected:** Stocks (too low volatility for short-term speculation) and Crypto (excessively news-driven and chaotic)

**Reason:** Forex is a market of relative values with structural cyclicality, macroeconomic drivers, bounded volatility, and available leverage.

### 2. Structural (Constant) Market Characteristics
These define the foundation the bot must respect:

1. 24/5 market with no fixed open/close — requires session filtering
2. Extremely high liquidity on major pairs — tight spreads, minimal slippage
3. Decentralized market — quotes and execution depend on the broker
4. High leverage (1:30 to 1:500) — demands strict risk management (0.5–2% per trade)
5. Very low transaction costs — enables higher trade frequency
6. Strong sensitivity to macroeconomic news — requires built-in news filter

### 3. Dynamic Characteristics (Monitored in Real Time)
The bot must continuously track these to understand current market context:

1. Current market regime (trend / range / chop)
2. Volatility level
3. Session activity and intraday cyclicality
4. Momentum strength and sustainability
5. Correlations between currency pairs
6. Proximity of news/events
7. Carry trade dynamics (swaps)

### 4. System Architecture Choice
- **Chosen:** Adaptive (regime-aware + context-aware)
- Goal: Not just to make money, but to know **when the model stops working**
- First, a simple reference bot will be built to observe its degradation in real conditions

### 5. Trading System Parameters (Mid-term)
- **Style:** Medium-term (positions held from several hours to several days)
- **Pairs:** Only major pairs — EUR/USD, GBP/USD, USD/JPY, AUD/USD, USD/CAD
- **Sessions:** London + New York (especially overlap). Asian session — only for managing open positions
- **News:** Hard filter (high-impact events + 15–30 min blocking window)
- **Regime Detection:** Mandatory real-time detection
- **Volatility:** Adaptive stop-loss, take-profit, and position sizing
- **Abnormal Conditions:** Block trading or reduce risk when market structure breaks
- **Minimum Context for Trade:** All key conditions must align (session + regime + volatility + no news + sufficient liquidity)

### 6. Three Directions for Future Edge Hypotheses
These will be explored and tested in later videos:

1. **Session Momentum Continuation**  
   Strong directional impulse at the start of an active session tends to continue.

2. **Mean Reversion from Extremes**  
   When price deviates significantly from its recent balance (given current volatility), probability of return increases.

3. **Liquidity Levels & Structural Breaks**  
   Price behavior around key highs/lows: true breakout vs false breakout (liquidity grab).

## What's Next (Video 2)
Analysis of an **unstructured bot** — a simple strategy without regime detection or environmental filters — to clearly see its weaknesses and degradation in a non-stationary market.

---

**Important Disclaimer**  
This is a research project for educational purposes only.  
No trading strategy or profitable bot is provided.  
Any use of this material in live trading is at your own risk.
