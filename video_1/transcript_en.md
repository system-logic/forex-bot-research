# Video 1 — Full Transcript (English)

Hi everyone! Today, we’re going to work on developing a bot for algorithmic trading. We’ll start from scratch—beginning with defining the task itself. As for where exactly this particular video will end, I’m not sure yet.

For this research, I’m setting myself a tentative deadline of three weeks. Whatever I manage to accomplish within that time frame will be included in the video. After that, I’ll spend one additional week summarizing the results, refining the key points, and so on.

If you’re new to the channel and not sure what it’s about, I recommend checking out this link.

For this playlist, I’ve created a GitHub repository, where you’ll be able to find materials for each video in separate folders. Sometimes these will be the main points, and other times there will be code attached, for example.

So, we’ve decided to create a bot. Where do we start?

Since I have an engineering and scientific background, it has shaped my methodology and the way I think. I believe the first step should be a complete understanding of the object of research. Of course, I’ve studied this before—it isn’t something you can grasp in five minutes or even five days—and I’ll be building on the knowledge I already have.

I’m going to pose the questions that must be answered before we can move forward—and I'll answer them right away.

Here’s the first question: Which market am I interested in?

It could be crypto, stocks, Forex, and so on. This is purely a subjective opinion based on my own research. For me, the stock market—especially when limited to a specific region—is relatively low in volatility. I generally see it as a long-term investment vehicle, so for now, I’m ruling it out.

Crypto is too volatile and heavily driven by news. Given the current global instability, I personally find it difficult to forecast its movements. If you buy Bitcoin now and hold it for the next 10 years, why not? But when it comes to speculation, I’ll pass for now.

I’m more inclined toward the Forex market—currency trading. Forex is a market of relative values. A currency is not an asset by itself, but a relationship between two economies. This creates cyclicality and trending phases. In other words, the market is not completely chaotic; it is structurally driven by macroeconomic balances. In addition, compared to crypto, volatility here is more structurally bounded, though major macroeconomic events can temporarily generate extreme spikes. Leverage is available—which suits me.

Based on this choice, let’s look at the characteristics we need to consider for the bot to operate effectively. This is important not necessarily to find a profitable strategy immediately, but to understand how the system works and which assumptions influence its behavior.

So the question becomes: What characteristics are key for this type of market?

This is one of the most fundamental questions in trading. When we don’t tie ourselves to a specific market, we are forced to look for universal characteristics that determine which strategy, indicator, or mechanism actually makes sense to apply. This task is extremely complex, especially at the initial stage.

For the Forex market, there is indeed a set of characteristics that strongly influence which strategies, indicators, and filters will work—and which ones will quickly fail. I’ve identified the most important ones to analyze when building a bot. They can be divided into constant (structural) characteristics and dynamic characteristics (those that change day by day and must be monitored by the bot in real time).

Let’s briefly touch on the system-level analysis of the environment.

It’s important to clarify one thing: we are not studying the market just for the sake of trading. We are studying the process of designing a system in a non-stationary environment. Forex is simply a convenient laboratory: sufficiently complex, dynamic, and sensitive to regime changes. If a method proves robust here, it can potentially be transferred to other domains as well.

Before talking about strategies, it’s important to take a step back. In any complex system, you first analyze the environment — and only then the behavior within it. We’re not looking for an entry point right now; we’re building a map of the environment.

Let’s start with the structural characteristics — the ones that must always be considered during the design phase.

First, the bot operates 24 hours a day, five days a week — without fixed opening and closing times like in stock exchanges. Data flows continuously, but activity varies significantly depending on the time of day. Therefore, the bot must filter by trading sessions: the Asian session is usually quiet, while the London and New York sessions are the most active — especially during their overlap. We should also keep rollover in mind — holding positions overnight — as it comes with its own specifics and surprises.

Second, the market has enormous liquidity — one of the highest in global finance, around $10 trillion in daily volume. On major pairs like the euro against the dollar, slippage is minimal and spreads are tight, which even allows for aggressive scalping. However, on cross pairs — and especially exotic pairs — liquidity is lower, slippage becomes more noticeable, and caution is required.

Third, the market is decentralized — there is no single central exchange. Quotes may differ slightly between brokers, spreads vary, and sometimes requotes occur. That means you press the “buy” button, but the broker does not execute the order immediately — instead, you receive a notification that the price has changed and are offered a new one.

A logical question might arise: why does this matter to me? I trade with one broker, and all my executions are based on their data anyway. If we were doing arbitrage, then yes — discrepancies would be critical. Indeed, differences in quotes do not directly affect our trades. We open, hold, and close positions within one ecosystem — at the prices provided by our broker.

However, there are nuances.

The broker itself is essentially a mini-market. Since there is no central exchange, each broker forms its own quotes based on its liquidity sources — major banks, other brokers, aggregators. A good broker aggregates quotes from a large number of sources (sometimes 20 or more), which brings the price closer to the “real” market value, keeps spreads tight, and minimizes slippage. Some brokers may display prices slightly shifted in their favor, or during high volatility decide whether to execute an order at the requested price.

Impact on order execution. When we place a market order or a stop-loss, the broker attempts to execute it at the current quote. But if the market moves sharply — for example, due to news — the broker may apply slippage, issue a requote, partially fill the order, or even reject it. This happens because quote synchronization is not perfect, not necessarily because someone is trying to cheat us. The thinner the liquidity pool, the worse this effect can be.

Backtesting vs. reality. When we develop a bot and test it on historical data, that data usually comes from an aggregated feed — a single data stream compiled and processed from multiple internal or external sources, then delivered as one unified flow. In reality, our broker may provide slightly different highs and lows, spreads, and even ticks. If a strategy shows a 70% win rate in testing, it may decline on a live account due to these micro-differences. Ideally, we should pull tick data directly from our own broker rather than using a generic feed, so that the simulation is as close to reality as possible.

Indirect influence of other brokers. If our broker is a market maker and sees that many clients are opening positions in the same direction, it may “adjust” quotes or widen the spread to balance its book. This is part of the broader market picture. During news releases or thin market conditions — such as the Asian session or rollover — differences between brokers can become significant. One broker might have a 1-pip spread, while another has 15. The bot may assume everything is calm, while in reality we’re already facing substantial slippage.

In practice, if we use a well-regulated broker with a solid reputation, these issues are minimized and almost unnoticeable under normal market conditions. However, in many cases — like mine — I’m limited to just four or five brokers within my country, especially if I want to avoid complications with fund transfers, legality, and so on.

I don’t intend to deeply analyze and compare them with the giants of the industry. That would require significant effort and, in the end, might only yield a potential advantage of 10 pips — and even that is not guaranteed. Will this information prove useful in designing the bot? I’m not sure yet. But it’s worth keeping in mind — I suspect it may become relevant at some point in the future.

Fourth, leverage is high here — typically up to 1:30 with regulated brokers and up to 1:500 with offshore ones. Even small price movements can turn into huge gains or losses on the account. That means strict risk management is absolutely essential. The risk per trade is usually kept within 0.5–2%; otherwise, one strong news impulse — and hello, margin call. Of course, there are more advanced risk models, but we’ll start with something simpler and more robust.

Fifth, transaction costs are very low. The main components are the spread and swap; commissions are minimal or nonexistent in many cases. Because of this, strategies with a large number of trades per day become viable — especially on pairs with tight spreads. This opens the door to scalping and even high-frequency approaches.

Sixth, Forex is heavily tied to macroeconomics and news — though not to the same extent as crypto. Major economic releases can trigger sharp impulses, spikes, and sometimes even gaps. Volatility during these moments can increase by 3 to 10 times. Therefore, almost any serious bot should include a built-in news filter: 5–30 minutes before and after major releases, trading should either be paused or entry rules should be significantly tightened. Otherwise, the system may repeatedly fall into false breakouts.

These six factors form the structural foundation of the Forex market — they don’t fundamentally change from year to year. If we ignore this foundation when designing the bot, the system will operate randomly rather than systematically.

Preliminarily, we can say that session filters, news filters, strict risk control, and liquidity checks should be built in first — and only after that should we layer indicators or other strategy components on top.

Now let’s move on to the dynamic characteristics. These are the factors that change every day, every hour, or even every few minutes. They create the market context. The bot must be able to detect them in real time, analyze them, and based on that decide: should it trade now, or is it better to wait? Should it use a trend-following indicator or switch to a different approach? Tighten stops or loosen them?

This is essentially the pulse of the market. If the bot ignores it, it will blindly follow indicator signals even when the conditions are no longer suitable.

First and most important — the current market regime: trend, range, sideways movement with many false breakouts (so-called chop), or some transitional phase. There are many ways to measure this — for example, using ADX, Bollinger Bands, or simple structural analysis. But for now, the specifics don’t matter; at this stage, we are formalizing the tasks. The regime can shift within hours or even minutes due to participants’ behavior, news, or accumulated imbalance. A strategy that works in one regime is usually unprofitable or random in another. Without regime detection, the system operates blindly.

Second — current volatility. This reflects the magnitude and speed of price fluctuations relative to the recent past. It spikes during news and strong impulses and drops during low-activity periods. Volatility affects expected movement size, stop reliability, the frequency of false breakouts, and even the type of tactical approach that makes sense.

Third — session activity and intraday cyclicality. Depending on the time of day and session overlaps (Asia, London, New York), trading intensity varies. Each day follows a repeating cycle with predictable peaks and troughs in activity. Most false signals and losing periods occur during low-activity windows. Many of the strongest movements are often observed during session overlaps, though this is an empirical tendency rather than a structural law and must be validated statistically.

Fourth — the strength of the movement itself. How confident and sustainable is it? Is the price continuing in the same direction, or is it exhausting and preparing to reverse? This changes in real time — momentum can build, weaken, or disappear entirely. When movement is weak, continuation signals quickly turn into traps.

Fifth — correlations between pairs. Are they moving together right now, or diverging? This is not fixed. Sometimes most pairs move in sync following the dollar; other times they diverge in different directions. High synchronization increases the risk of duplicating essentially the same position multiple times. Divergence may create hedging opportunities — or signal that something unusual is happening.

Sixth — proximity of news and events. The calendar changes every day: interest rate decisions, inflation data, employment reports, central bank speeches, geopolitics. When such events are near, the market can explode at any second, and familiar patterns may simply stop working. Ignoring this means exposing yourself to the most frustrating losses.

Seventh and finally — carry trade dynamics. That is, how interest rate differentials affect the cost of holding a position. Central bank rates change, market expectations shift, and swaps are adjusted daily. For short-term trades this is almost negligible, but if positions are held for days or weeks, the swap can become either a pleasant bonus or a silent expense that slowly eats into profits.

All these factors are dynamic because they depend on what is happening right now: trader behavior, external triggers, time of day, sentiment. They are not written into the fixed rules of the market. They are critical because they create the real context — the conditions under which a strategy has an edge, and those in which it becomes random or harmful.

Without continuous monitoring of these parameters, the bot cannot adapt — it simply executes trades mechanically while the market has already shifted into a different state. But when the bot senses these changes and filters conditions accordingly, it stops being a dumb machine and starts to at least partially understand what is going on.

And once we’ve gone through the general questions, a more fundamental one emerges: Am I building a system that simply makes money, or a system that adapts to changing market regimes?

These are different architectures. And this question goes beyond trading. It applies to any model operating in a changing environment. Static system vs. adaptive architecture — this is a fundamental fork in the road.

Let’s look at a system designed purely to generate profit. The classic path. The logic is straightforward: find a pattern, formalize it, optimize the parameters, achieve positive expectancy, and control risk. In this case, the main goal is positive mathematical expectancy on historical data with an acceptable drawdown.

The problem is that markets change. What used to work eventually stops working. At that point, the system either keeps trading and slowly dies — or it gets manually switched off. This static architecture can be profitable, sometimes even very profitable. But it doesn’t know when it has become irrelevant.

With a system that adapts to market regimes, the situation is different. Here, the goal is not just to make money, but to determine: What regime is the market currently in? Does my strategy fit this regime? Should the parameters be adjusted? Should the strategy be disabled? Or should it switch to another one?

In this case, the strategy becomes two-layered: the trading strategy itself and a meta-system that incorporates regime detection, adaptation, and switching logic. In these two approaches, we are asking two fundamentally different questions. In the first case: “Where do I enter and where do I exit?” In the second: “When is my model applicable at all?”

In a non-stationary environment, an architecture that continuously evaluates the applicability of its own model is structurally more robust than one that assumes stability by default. Personally, I’m looking for architecture, systemic thinking, robustness, and the ability to deal with uncertainty — not curve fitting, parameter optimization, or debates about win rate. That’s noise.

From an engineering theory perspective, this brings us into the domain of regime analysis, distributions, stability, model degradation, and related concepts. That’s really what this project is about. Of course, an adaptive system is much more complex. I need to define the regimes themselves, detect regime shifts, establish criteria for disabling or switching strategies, and avoid overfitting to those regimes. In essence, this becomes a research project.

But we must understand that a fully adaptive system is impossible. Any adaptation is also based on assumptions. The question is not about building a perfect model — it’s about how quickly the system can recognize that it no longer understands the market.

At the same time, research requires a simple reference object to start from. So first, we’ll set the task of building a simple system that makes money — even a little. We’ll observe its limitations, its degradation, and where it breaks. Only after that will we decide whether we can design a system capable of overcoming those weaknesses.

There is also a subtle danger here. When we analyze the environment too deeply, we may construct an almost perfect protective framework — but end up with an overly narrow signal component. As a result, the bot will open trades very rarely. This is a classic engineering bias: maximum safety leads to minimal activity, which in turn leads to insufficient statistical significance.

In the next video, I’ll demonstrate a clear example of such a project.

We’ve now covered the key characteristics. That allows us to raise the next set of questions.

What type of trading are we considering? We will focus on medium-term trading — positions held from several hours to several days. This format aligns well with the structural characteristics of the Forex market. It allows us to work with cyclicality and regime behavior without falling into extreme dependence on micro-execution, as in HFT or aggressive scalping, where ticks, latency, and fractional spreads become critical.

At the same time, medium-term trading maintains a sufficient trade frequency for statistical relevance, while avoiding a shift into long-term investing — where fundamental shifts and macro cycles dominate. This time horizon offers a balance between cost control, manageable risk, and the ability to exploit volatility during active sessions without becoming overly sensitive to market noise.

As mentioned earlier, slippage is minimal on major pairs, but on crosses and exotic pairs, it becomes noticeable. This naturally leads to another question.

Which currency pairs are we interested in?

Primarily, we will focus on major currency pairs such as EUR/USD, GBP/USD, USD/JPY, and possibly AUD/USD and USD/CAD. These pairs offer the highest liquidity, tight and relatively stable spreads, minimal slippage, and more predictable reactions to macroeconomic factors.

For medium-term trading, this is especially important: we reduce the impact of micro-execution issues, lower transaction costs, and operate within a cleaner market structure — without the sharp anomalies often seen in exotic pairs. At this stage, it makes sense to exclude crosses and exotic pairs in order to avoid unnecessary complexity and additional sources of instability in the model.

Which trading sessions should the bot operate in? When should it be active?

For a medium-term model, it makes sense to activate the bot during the London and New York sessions — especially during their overlap, when liquidity is at its peak, spreads are more stable, and price movements tend to be more structured. Empirically, many significant impulses tend to form during these hours, and the daily direction is often established there — though this tendency must be statistically verified within our own dataset.

At the initial stage, the Asian session can logically be either excluded entirely or used only for managing already open positions. This session is often characterized by compressed volatility and a higher frequency of false moves. Such an approach allows us to focus on periods where the probability of meaningful movement is higher and relative market noise is lower compared to the active phases.

Should the bot treat news as a hard filter?

Yes. At the design stage of a medium-term bot, news should be implemented as a strict filter rather than as an additional indicator. In a market sensitive to macroeconomics, a single release can completely alter the regime, volatility, and price structure within minutes. If the bot ignores this factor, it may enter the market at a moment of maximum uncertainty.

How do we account for news?

At the initial stage, news can be incorporated in the following way: First, an importance filter — considering only high-impact events like interest rates, inflation, or central bank speeches. Second, a time-based blocking window — prohibiting new positions 15 to 30 minutes before and after the release. Third, stricter position management — if a position is already open, we temporarily reduce risk by tightening the stop or partially locking in profits. And fourth, a volatility stabilization filter — waiting until spreads normalize and volatility returns to its typical range after the release.

In this way, news becomes a risk management tool. At this stage, our goal is not to capture news-driven moves, but to avoid destructive entries during chaotic market restructuring.

Should the bot determine the market regime?

Again, yes. The bot must detect the market regime in real time. For a medium-term strategy, it is critical to understand whether the market is trending, ranging, or stuck in chaotic chop, since signal effectiveness and the appropriate tactical approach depend heavily on the regime.

Without such recognition, the bot will mechanically follow indicators even when the market does not provide conditions for sustained movement, leading to a large number of false trades. Regime detection allows us to adapt position size, stop levels, and profit targets to the current market structure — increasing the probability that the strategy is operating where its edge actually exists.

How should we account for current volatility?

Current volatility must be treated as a key dynamic parameter influencing every aspect of a position. For a medium-term bot, this can be implemented as follows: We use volatility measurement — like ATR or standard deviation — to estimate the current movement range. We use adaptive stop-loss and take-profit levels — the higher the volatility, the wider the stops should be to avoid being shaken out by random fluctuations. We adjust position size — when volatility increases, it’s prudent to reduce trade size to keep risk under control. And finally, signal filtering — during extreme volatility, signals may be ignored or stricter entry criteria activated.

Volatility should become a contextual measure of risk and signal quality, not just a statistical number. The bot must adjust the strategy to the current market state in order to preserve its edge and minimize random losses. These approaches are not dogma — they must be tested and calibrated.

Should trading be limited under abnormal conditions?

Yes. Limiting trading during abnormal conditions is extremely important. On Forex, abnormal situations are those in which the normal market structure breaks down: sharp volatility spikes, extreme spreads, unexpected gaps, or technical failures at the broker. During these moments, standard signals almost always become false, the risk of drawdowns rises sharply, and the strategy’s edge temporarily disappears.

Is there a concept of an abnormal market state?

The bot must have the notion of an “abnormal state” and act accordingly: either refrain from trading or reduce risk. This could mean fully blocking new positions, reducing the size of trades, tightening stops, or partially closing existing positions if necessary. Such measures make the system more resilient and protect capital — which is especially important in medium-term trading, where positions are held for hours or days and are sensitive to sudden impulses.

Should the bot operate all the time or only when market conditions align?

The bot should not trade continuously. Its purpose is to operate only when key environmental conditions align and the probability of an edge is highest. In other words, every trade should pass through a set of filters: market regime, session, volatility, news, liquidity, and per-trade risk. Without these restrictions, the bot would “poke” the market during chaotic periods, turning an edge into randomness.

How many filters are acceptable without killing activity?

There should be enough filters to protect capital, but not so many that trades become too rare and statistical significance is lost. Practically, this means four to six core filters at the initial stage: session, news, market regime, volatility, liquidity, and risk management. Additional filters can be introduced gradually after testing, evaluating their effect on trade frequency and the balance between safety and sufficient activity.

It is also important to recognize that some of these filters may be interdependent. For example, volatility affects regime detection, news influences volatility, and session structure impacts liquidity. Later, we will need to distinguish between primary state variables and derived conditions to avoid redundant or circular logic within the architecture.

What is the minimum context required to allow a trade?

The minimum context for allowing a trade is the alignment of key environmental conditions that critically impact the bot’s edge. For a medium-term bot, this means: First, the trading session is active. Second, the market regime matches the chosen tactic. Third, volatility is within an acceptable range. Fourth, no high-impact news is imminent and the market is not in an abnormal state. And fifth, liquidity is sufficient and spreads are not abnormally wide.

Only when all these conditions are simultaneously met is a trade allowed.

Will the system be adaptive or static?

The system will be adaptive. The bot continuously evaluates the current values of these parameters and adjusts its actions in real time — position size, stops, take-profits, and even the decision to enter a trade. This allows it to respond to market changes and maintain its edge, rather than mechanically following signals without regard to the current context.

Now, having described the space of permissible trading, we can talk about finding an edge within that space — the key point. Without this framework, an edge becomes an abstraction. I’ve mentioned this term several times before, but now we can expand on it.

An edge is a statistically verified, repeatable trading advantage in which the expected return of a single trade is positive even after accounting for all costs — spread, swap, and risks. We build it through careful analysis of market patterns, rigorous testing, filtering out false signals, and continuous verification that the advantage persists under real conditions.

At this stage, all we can do is formulate hypotheses, perhaps several, which will be refined or discarded at each step of further research. Only after we use these hypotheses and the framework we’ve built to define a final edge can we talk about a working strategy.

First, we need to understand the basis for formulating edge hypotheses. They arise from a combination of three components: Static characteristics (the structural properties of the market), Dynamic characteristics (real-time parameters like regime and volatility), and simple empirical verification (the search for recurring statistical tendencies). An edge is not just an idea; it is a recurring statistical bias that can potentially be formalized.

Should we select edge hypotheses now? At this stage, it’s better not to fixate on specific ones. Instead, we’ll outline three directions of thought that will later serve as the foundation for real, testable edge hypotheses.

First: Using session context and momentum. As we discussed, Forex is structurally divided into trading sessions. London and New York generate the main volumes and impulses. This is not random — it’s when large participants, banks, and hedge funds enter the market. Hypothesis: If a directional impulse forms at the beginning of an active session during high activity, there is an increased probability that the movement will continue within the same phase. The edge here is derived from the behavioral structure at the start of the session.

Second: Market reaction to deviations from the mean. The market constantly fluctuates around a balance point between supply and demand. Hypothesis: When the price deviates significantly from its recent equilibrium — more than typical for the current volatility — the probability of returning toward balance increases. When movement exceeds normal amplitude, participants take profits, new players hesitate to enter, and liquidity dries up. The edge here comes from the asymmetric market reaction to extremes.

At this point, we must acknowledge a conceptual tension: the first hypothesis assumes momentum continuation, while the second assumes mean reversion. These are structurally opposite behaviors. This makes regime detection a critical architectural node of the entire system. Without correctly identifying when continuation dominates and when reversion prevails, the system would attempt to apply mutually incompatible logics simultaneously.

Third: Influence of key liquidity levels and structural points. Forex is a market where a huge number of stop orders cluster around obvious highs and lows. These zones are liquidity pools. Hypothesis: Price tends to either accelerate after capturing liquidity — a breakout — or reverse sharply after stops are cleared — a false breakout. This works because large participants need liquidity to enter positions. Crowd stops act as ready counter-orders. The potential edge is based on price behavior near structural extremes, not merely on the fact that a level was broken.

These ideas are not new — I’m not revealing secret knowledge here. They are classic trading concepts based on behavioral economics, order flow, and market statistics. Nevertheless, they remain highly relevant, especially in the context of AI and algorithmic trading, which only amplify these patterns.

With these three ideas, I’ll end this video.

We’ve studied the object of research, posed key questions, provided answers, and outlined three directions for potential strategies. This represents the essential work for this stage.

Remember: any model is just a hypothesis that lives only as long as the environment validates it. The real question isn’t “Does the system work?” — it’s “Will it know when it stops working?”
