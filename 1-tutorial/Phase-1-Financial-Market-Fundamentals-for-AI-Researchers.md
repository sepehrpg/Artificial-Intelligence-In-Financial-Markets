# Phase 1: Financial Market Fundamentals for AI Researchers

# Table of Content
---
1. Core Idea
2. Why It Matters
3. Detailed Explanation
   - Markets and Assets
   - Basic Market Concepts
   - Market Structure
   - Order Types
   - Time and Market Data Perspective
   - Academic Research vs Trading vs Investing vs Production Systems
4. Real-World Example
5. Common Mistakes / Pitfalls
6. Extra Important Concepts
7. Summary and What Comes Next
---

## 1. Core Idea

A financial market is a place where **assets are exchanged at prices determined by supply, demand, information, and market structure**.

For an AI researcher, the key idea is this:

> You are not modeling “price” in isolation.  
> You are modeling a system of **assets, participants, information flow, and execution mechanics**.

That means you need to understand:

- what assets are
- how prices are formed
- how trades are executed
- what makes one market different from another
- why the same model may behave differently in stocks, forex, crypto, or futures

A useful mental model:

\[
\text{Market behavior} = f(\text{participants}, \text{assets}, \text{orders}, \text{liquidity}, \text{news}, \text{rules})
\]

So before building a model, you must understand the “physics” of the market.

---

# 2. Why It Matters

If you do AI on financial data without market fundamentals, you will almost certainly make mistakes such as:

- predicting the wrong target,
- using the wrong time scale,
- ignoring execution costs,
- misunderstanding price formation,
- or evaluating a model in a way that would never work in reality.

This phase matters because it connects theory to practice.

## For academic research

You need to know what problem is actually being studied:
- return prediction,
- volatility forecasting,
- regime detection,
- microstructure analysis,
- price impact modeling,
- execution optimization.

## For trading

You need to know:
- whether your signal can survive costs,
- whether the market is liquid enough,
- whether your orders can actually be filled,
- whether the strategy makes sense for that asset class.

## For investing

You need to know:
- what the asset represents,
- how it behaves over time,
- what risks dominate,
- how macro and fundamentals affect valuation.

## For production systems

You need to know:
- how the market data arrives,
- how orders are routed,
- what happens when data is delayed or missing,
- how to handle partial fills and slippage.

In short:

> Market fundamentals determine whether an AI model is merely interesting, or actually usable.

---

# 3. Detailed Explanation

## 3.1 Markets and Assets

### What is a market?

A market is a system where buyers and sellers interact to exchange assets. The exchange can happen:
- on an exchange,
- through a broker,
- through a dealer / market maker,
- or through decentralized mechanisms.

The market exists because different participants value the same asset differently at the same time.


### What is an asset?

An asset is something that has economic value and can be owned, traded, or used as a store of value or claim.

Main asset classes:
1. Stocks
2. Forex
3. Crypto
4. ETF
5. Commodities
6. Futures
7. Options


### 1. Stocks

A stock represents **ownership in a company**.

If you own a share of a company, you own a small fraction of that company’s equity.

Stock value is influenced by:
- expected profits,
- growth,
- interest rates,
- industry conditions,
- investor sentiment,
- and market expectations.

#### What matters for AI

Stock prices often combine:
- fundamental information,
- news,
- broad market movement,
- sector effects,
- and short-term trading behavior.


### 2. Forex

Forex is the market for exchanging one currency for another.

Examples:
- EUR/USD
- USD/JPY
- AUD/USD

A currency pair tells you how much of the quoted currency you need to buy one unit of the base currency.

#### What matters for AI

Forex is strongly affected by:
- interest rate differentials,
- macroeconomic data,
- central bank policy,
- geopolitical events,
- risk-on / risk-off behavior.

Forex is often very liquid, but also highly sensitive to macro conditions.


### 3. Crypto

Crypto assets are digital assets traded on cryptocurrency markets.

Examples:
- Bitcoin
- Ethereum
- altcoins
- stablecoins

#### What matters for AI

Crypto often shows:
- 24/7 trading,
- strong sentiment effects,
- high volatility,
- regime shifts,
- social-media-driven behavior,
- on-chain data availability.

That makes it attractive for research, but noisy and unstable.


### 4. ETFs

An ETF is an exchange-traded fund. It is a security that trades like a stock but typically represents a basket of assets.

Examples:
- market index ETFs,
- bond ETFs,
- sector ETFs,
- commodity ETFs.

#### Why they matter

ETFs are useful for:
- broad exposure,
- diversification,
- portfolio-level modeling,
- and studying how baskets behave versus single names.


### 5. Commodities

Commodities are physical goods traded in markets.

Examples:
- oil,
- gold,
- silver,
- wheat,
- natural gas.

#### Why they matter

Commodity prices are strongly affected by:
- supply and demand,
- storage,
- seasonality,
- global events,
- logistics,
- geopolitical risk.


### 6. Futures

A future is a contract to buy or sell an asset at a future date for a specified price.

Futures are widely used for:
- hedging,
- speculation,
- and exposure to commodities, indices, or currencies.

#### Why they matter

Futures introduce:
- expiry dates,
- rollovers,
- margin,
- leverage,
- and contract specifications.

This makes them richer, but more complex, than simple spot assets.


### 7. Options

An option gives the right, but not the obligation, to buy or sell an asset at a specified price before or at expiry.

Two basic types:
- **call option**: right to buy
- **put option**: right to sell

#### Why they matter

Options are used for:
- hedging,
- income strategies,
- volatility trading,
- directional bets,
- and risk transfer.

They are extremely important in quant finance because they are directly tied to:
- volatility,
- time decay,
- and nonlinear payoffs.


## 3.2 Basic Market Concepts

### Price

Price is the amount paid to buy an asset at a specific time.

But price is not the same as value.

- **Price** is what the market is currently willing to pay.
- **Value** is what you believe the asset is worth.

AI models often predict price movement, not intrinsic value directly.


### Return

Return measures how much an asset changes over time.


Simple return:

$$
r_t = \frac{P_t - P_{t-1}}{P_{t-1}}
$$

where:
- $P_t$ = current price
- $P_{t-1}$ = previous price

Log return:

$$
r_t = \log\left(\frac{P_t}{P_{t-1}}\right)
$$

#### Why returns matter more than raw price

Prices across assets are not directly comparable. A \$10 stock and a \$1,000 stock are not meaningful to compare by price alone.

Returns standardize the movement.

For AI, returns are often a better target than raw price because they are more stationary and more meaningful across assets.


### Volume

Volume is the amount traded over a time interval.

High volume usually means:
- more participation,
- stronger conviction,
- higher liquidity,
- or major information arrival.

Low volume often means:
- weaker participation,
- thinner liquidity,
- and potentially less reliable price signals.

For AI research, volume can be a useful feature because it often reflects market attention.


### Volatility

Volatility measures how much price fluctuates.

A simple sample volatility estimate is the standard deviation of returns:

$$
\sigma = \sqrt{\frac{1}{n-1}\sum_{i=1}^{n}(r_i - \bar{r})^2}
$$

#### Why it matters

Volatility tells you about risk and uncertainty.

- High volatility = larger swings, higher uncertainty
- Low volatility = smaller swings, more stable behavior

In markets, volatility is often as important as return. Many models are more useful for predicting volatility than price direction.


### Liquidity

Liquidity is the ease with which an asset can be bought or sold without causing a large price change.

A liquid market has:
- many buyers and sellers,
- tight spreads,
- fast execution,
- low slippage.

An illiquid market has:
- fewer participants,
- larger spreads,
- more price impact,
- harder execution.

#### AI relevance

A prediction model may look great on paper but fail in an illiquid market because actual trading costs are too high.


### Bid / Ask

The **bid** is the highest price a buyer is willing to pay.

The **ask** is the lowest price a seller is willing to accept.

So:
- if you sell immediately, you usually hit the bid,
- if you buy immediately, you usually lift the ask.

#### Mid price

A common reference is:

$$
\text{Mid Price} = \frac{\text{Bid} + \text{Ask}}{2}
$$

This is often used in analysis, but real execution usually happens at bid or ask, not at the mid.


### Spread

Spread is the difference between ask and bid:

$$
\text{Spread} = \text{Ask} - \text{Bid}
$$

It is a direct cost of trading.

A tight spread means:
- better liquidity,
- lower cost,
- easier execution.

A wide spread means:
- higher trading cost,
- less efficient market,
- potentially less liquidity.

For AI-based trading, spread is one of the first costs you must include.


### Market Depth

Market depth is the amount of buy and sell interest available at different price levels.

It tells you:
- how much you can trade,
- how the order book is structured,
- and how sensitive price may be to larger orders.

A deep market can absorb larger trades with less price movement.

A shallow market can move sharply when a large order arrives.


## 3.3 Market Structure

Market structure is how the market is organized and how trading actually happens.


### How a trade is executed

A trade occurs when a buy order and a sell order match.

Basic process:
1. A participant sends an order.
2. The order goes to the market venue or broker.
3. The matching engine tries to match it.
4. If a match exists, a trade occurs.
5. The order may be fully filled, partially filled, or not filled at all.

This process matters because your model may predict a profitable move, but if execution is bad, the edge disappears.


### Exchange

An exchange is a venue where assets are traded according to formal rules.

Examples:
- stock exchanges,
- futures exchanges,
- crypto exchanges.

Exchanges typically provide:
- order matching,
- order books,
- market data,
- transparency,
- and rules for trading.


### Broker

A broker is an intermediary that allows a trader or investor to access markets.

The broker may:
- route orders,
- provide leverage,
- supply market data,
- manage account infrastructure,
- or aggregate access to multiple venues.

For an AI system, broker APIs are often the bridge between your algorithm and actual execution.


### Market maker

A market maker provides liquidity by continuously quoting bid and ask prices.

They help:
- keep markets tradable,
- reduce friction,
- and narrow spreads.

Market makers profit from the spread and from managing inventory and risk.

#### Why this matters

Market makers strongly influence short-term price behavior and order book dynamics.

If you model microstructure data, you are indirectly modeling how liquidity is being supplied and consumed.


## 3.4 Order Types

### Market order

A market order executes immediately at the best available price.

Use it when:
- speed matters more than exact price,
- liquidity is sufficient,
- and you want guaranteed execution.

#### Risk
- slippage,
- especially in fast or thin markets.


### Limit order

A limit order sets the maximum price you are willing to pay when buying, or minimum price you are willing to accept when selling.

Use it when:
- price control matters,
- and you are willing to wait.

#### Risk
- the order may never fill.


### Stop order

A stop order becomes active when the market reaches a specified trigger price.

Use it for:
- breakout entry,
- stop-loss style behavior,
- conditional execution.

#### Risk
- can be triggered during short-lived spikes.


### Stop-limit order

A stop-limit order combines a trigger and a limit.

It activates when a stop price is reached, but only fills at the specified limit or better.

Use it when:
- you want trigger-based execution,
- but still want price protection.

#### Risk
- the trigger may happen, but the limit may not fill.


## 3.5 Time and Market Data Perspective

Financial data is time-sensitive.

A price at 9:30 AM is not the same as a price at 9:31 AM.

This creates several important ideas:
- sequence matters,
- time alignment matters,
- future information cannot be used,
- the same data point may have different meaning at different times.

For AI, this means the dataset must be built as a true time series, not as a shuffled table without temporal logic.


## 3.6 Academic Research vs Trading vs Investing vs Production Systems

### Academic research

You may study:
- price behavior,
- return predictability,
- volatility forecasting,
- order book imbalance,
- transaction cost models.

#### Goal
- explain,
- test hypotheses,
- find statistically valid effects.


### Trading

You care about:
- executable signals,
- costs,
- timing,
- liquidity,
- latency.

#### Goal
- make money after costs.


### Investing

You care about:
- asset quality,
- long-term expected return,
- risk,
- valuation,
- macro context.

#### Goal
- grow capital over time.


### Production systems

You care about:
- uptime,
- monitoring,
- order handling,
- failures,
- data quality,
- reproducibility.

#### Goal
- make the system robust enough to run safely in the real world.

---

# 4. Real-World Example

Suppose you want to build an AI system for trading Bitcoin.

## Step 1: Understand the asset

Bitcoin is a crypto asset traded 24/7. It has:
- high volatility,
- strong sentiment effects,
- and frequent regime changes.


## Step 2: Choose the data

You may use:
- OHLCV prices,
- volume,
- funding rates,
- order book data,
- news sentiment,
- on-chain metrics.


## Step 3: Define the problem

Possible tasks:
- predict next-hour return,
- predict next-hour volatility,
- detect unusual activity,
- create a risk alert.


## Step 4: Understand market structure

If you place a market order, you will cross the spread.

If the market is thin, the order may move the price.

If volatility is high, slippage may be large.

## Step 5: Evaluate properly

A model that predicts direction slightly better than chance may still be useless if:
- spread is large,
- fees are high,
- or fills are poor.

This example shows why the fundamentals matter before the model.

---

# 5. Common Mistakes / Pitfalls

## 1. Treating all markets the same

Stocks, forex, crypto, futures, and options behave very differently.


## 2. Ignoring execution mechanics

A good signal is not enough. You must know how the order actually gets filled.


## 3. Confusing price with value

The market price is not always the “true value” of an asset.


## 4. Using raw prices instead of returns

Raw prices are often less useful than returns for modeling.


## 5. Ignoring spread and slippage

These can destroy small statistical edges.


## 6. Using the wrong order type

A limit order is not the same as a market order, and each has different consequences.


## 7. Not accounting for liquidity

An edge in a thin market may disappear once you try to trade size.


## 8. Shuffling time-series data

For finance, random train-test splits usually create unrealistic results.


## 9. Overlooking market regime

A strategy that works in one environment may fail in another.


## 10. Thinking “high accuracy” means “good trading”

A high classification score does not automatically produce profit.


# 6. Extra Important Concepts

## Market efficiency

A market is more efficient when prices quickly reflect available information.

For AI, this matters because if markets are highly efficient, easy patterns are harder to exploit.


## Transaction cost

Real trading includes:
- commissions,
- spread,
- slippage,
- market impact.

Even a small edge can vanish after costs.


## Leverage

Leverage increases exposure using borrowed capital or derivative structures.

It can amplify:
- gains,
- losses,
- and liquidation risk.


## Margin

Margin is collateral required to open or maintain a leveraged position.


## Microstructure

Market microstructure studies the fine details of:
- order flow,
- bid-ask spread,
- fills,
- latency,
- and price impact.

This is especially important for intraday or high-frequency AI systems.


## Asset horizon

Different assets and strategies live on different time scales:
- milliseconds,
- minutes,
- days,
- months,
- years.

A model must be designed for the horizon it is meant to act on.


## Stationarity

Most market relationships change over time.

This is why financial ML is harder than many other ML tasks.

---

# 7. Summary and What Comes Next

Phase 1 gives you the language of the market:
- what assets are,
- how price, return, volume, volatility, liquidity, spread, bid/ask, and depth work,
- and how trades actually execute through exchanges, brokers, market makers, and order types.

The most important takeaway is:

> In financial AI, the model is only half the problem.  
> The market structure around it decides whether the idea is realistic.


## Mini Tasks

1. Pick one asset class: stock, forex, crypto, ETF, futures, or options.
2. Write one sentence explaining how it differs from the others.
3. Define price, return, volatility, spread, and liquidity in your own words.
4. Compare market order, limit order, stop order, and stop-limit order.
5. Explain why a model with good backtest accuracy might still fail live.


## What Comes Next

The next phase is **Data Literacy in Financial Markets**.

That is where we move from market concepts to the actual kinds of data you will use in AI projects:
- OHLCV,
- tick data,
- order book data,
- fundamentals,
- macro data,
- news,
- sentiment,
- and alternative data.