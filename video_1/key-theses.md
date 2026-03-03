# Forex Bot Research — Core Takeaways from Video 1

## Project Goal
Not to build a profitable bot.  
To study the process of designing a robust decision-making system in a non-stationary environment.  
Forex is just a convenient lab: complex, dynamic, mode-sensitive.

## Core Question
Static system that makes money  
or  
Adaptive system that detects when it no longer works?

Choice: **adaptive architecture** (regime analysis, degradation detection, meta-system layer).

## Key Environment Characteristics (Forex)
### Structural (fixed)
- 24/5 no fixed close → session filtering mandatory  
- High liquidity on majors → minimal slippage, but crosses/exotics are risky  
- Decentralized market → broker differences (spreads, requotes, execution)  
- Leverage → strict risk management (0.5–2% per trade)  
- Low costs → scalping/HFT viable  
- Macro/news dependency → news filter mandatory

### Dynamic (real-time monitoring)
- Market regime (trend / range / chop)  
- Current volatility  
- Session activity / intraday cycles  
- Momentum strength (impulse vs exhaustion)  
- Pair correlations  
- Proximity to news/events  
- Carry trade (swap impact)

## Minimum Trade Filters (required context)
- Active session (London / New York, especially overlap)  
- Regime matches chosen tactic  
- Volatility within acceptable range  
- No imminent high-impact news / anomalies  
- Liquidity sufficient, spread normal

## Edge Definition & Approach
Edge = statistically confirmed, repeatable advantage after all costs + ongoing validation.  
Not an idea — a persistent statistical skew.

Initial hypotheses (drafts only, not fixed strategy):
1. Session context + impulse at active session open  
2. Mean-reversion on extreme deviation from recent equilibrium  
3. Price behavior at structural liquidity levels (stop hunts, fakeouts / true breaks)

## Main Risks & Traps
- Over-filtering → too few trades → no meaningful statistics  
- Ignoring dynamics → blind signal following in wrong regime  
- Static optimization → death on regime shift  
- No degradation awareness → system keeps trading after edge vanishes

## Bottom Line
Any model is a hypothesis that lives only as long as the environment validates it.  
The real question is not “does it work?”, but  
“will the system recognize when it stops working?”

Next steps: build simple static baseline → expose its breaking points → layer adaptive mechanisms.
