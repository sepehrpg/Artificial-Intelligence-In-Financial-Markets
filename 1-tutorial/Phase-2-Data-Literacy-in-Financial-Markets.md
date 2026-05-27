# Phase 2: Data Literacy in Financial Markets

# Table of Content
---
0. Quick Reference Tables
1. Core Idea
2. Why It Matters
3. Detailed Explanation
   - What does data mean in markets?
   - Data types in finance
   - Data structure: structured, semi-structured, unstructured
   - How each data type is used differently
   - A practical example of a data hierarchy
   - small Python example
4. Real-World Example
5. Common Mistakes / Pitfalls
6. Extra Important Concepts
7. Summary and What Comes Next
---

# 0. Quick Reference Tables

| Data Type | Structure | Typical Examples | Main Use | Best Horizon | Common Pitfalls |
|---|---|---|---|---|---|
| **Price & Volume** | Structured | OHLCV, tick data, quote data, order book, trade data | Trend analysis, volatility, liquidity, short-term prediction | Intraday to short-term | Ignoring spread, slippage, and time alignment |
| **Fundamental Data** | Structured / Semi-structured | Revenue, EPS, profit, balance sheet, filings | Valuation, stock selection, long-term analysis | Medium to long-term | Using revised data, ignoring reporting delays |
| **Macroeconomic Data** | Structured | Interest rate, inflation, GDP, unemployment, PMI | Regime analysis, asset allocation, macro forecasting | Medium to long-term | Ignoring release dates and revisions |
| **News & Text Data** | Unstructured | News articles, reports, filings, analyst notes, transcripts | Event detection, NLP, surprise analysis, sentiment extraction | Short to medium-term | Treating raw text as signal without proper preprocessing |
| **Sentiment Data** | Derived from text / social data | Positive/negative tone, fear-greed, social mood | Crowd reaction, event impact, behavioral signals | Short-term | Noisy signals, overfitting, keyword shortcuts |
| **Alternative Data** | Structured / Semi-structured / Unstructured | On-chain data, web traffic, search trends, app usage, satellite data | Early behavioral signals, hidden activity, cross-signal research | Varies by source | Expensive noise, hard cleaning, weak interpretability |

## Data Structure Quick Guide

| Structure Type | What It Means | Examples | Best Tools / Methods |
|---|---|---|---|
| **Structured** | Fixed schema, tabular data | OHLCV, ratios, macro time series | SQL, pandas, scikit-learn, classical ML |
| **Semi-structured** | Some organization, but nested or flexible | JSON API responses, filings with metadata, event feeds | Parsing, ETL pipelines, Python, JSON tools |
| **Unstructured** | Free-form content | News text, transcripts, social posts, audio, images | NLP, embeddings, transformers, CV models |

## Practical Interpretation Table

| Question | Best Data Type |
|---|---|
| What is the market doing right now? | Price & volume |
| Is the asset cheap or expensive? | Fundamental data |
| Is the economy changing? | Macroeconomic data |
| Did an important event happen? | News & text data |
| How are people reacting emotionally? | Sentiment data |
| Is there hidden real-world activity? | Alternative data |

## Core Takeaways

| Idea | Summary |
|---|---|
| Data is not only price | Financial decisions can use many signal types |
| Structure matters | Data format determines storage, cleaning, and modeling |
| Timing matters | Release time and latency are critical in finance |
| Horizon matters | Different data is useful at different time scales |
| Leakage matters | Using future information creates fake performance |
| More data is not always better | Relevance and quality matter more than quantity |




# 1. Core Idea

Phase 2 is about learning to think like a **data person** in finance. The core idea is:

> In financial markets, “data” means **anything that can improve a financial decision**.

That includes:
- direct market data like OHLCV and order books,
- company fundamentals,
- macro indicators,
- news and text,
- sentiment,
- and alternative data such as on-chain, web traffic, or search trends.

A stronger way to think about it is:

$$
\text{Market Data} = \text{Structured Data} + \text{Semi-Structured Data} + \text{Unstructured Data}
$$

Where:
- **structured data** has a fixed schema like sql.
- **semi-structured data** has some structure but not a rigid table shape like json or xml.
- **unstructured data** is free-form text, images, or audio.

This phase teaches you how to recognize each type, what it contains, and why it matters.

---

# 2. Why It Matters

Without data literacy, even a strong ML model becomes fragile.

Why this phase matters for AI and financial markets:

### For research
You must know:
- what the data represents,
- how it was generated,
- whether it is complete,
- and whether it can support the hypothesis you want to test.

### For trading
The key question is not “Do I have data?” but:
- Is the data timely?
- Is it tradable?
- Is it usable before the market moves?
- Does it contain leakage?

### For investing
Different data supports different horizons:
- fundamentals for longer-term valuation,
- macro data for regime and cycle analysis,
- sentiment for near-term reaction,
- price and volume for market timing.

### For production systems
You need to know:
- how data arrives,
- whether it can be parsed reliably,
- how often it changes,
- and how to monitor quality over time.

A model is only as good as the data semantics behind it. A beautiful neural network trained on misunderstood data is still a misunderstanding machine.

---

# 3. Detailed Explanation

## 3.1 What does data mean in markets?

Financial data is any recorded signal that can help estimate **return, risk, volatility, direction, valuation, liquidity, regime, or event impact**.

That means the “market” is not just a chart. It is a multi-layered information system:

- market prices tell you what the crowd is doing now,
- fundamentals tell you what the business is doing,
- macro tells you what the economic environment is doing,
- text tells you what people are saying,
- sentiment tells you how people feel,
- alternative data tells you what is happening outside the exchange.


## 3.2 Data types in finance
I divide financial data into six main groups: 
1. price/volume
2. fundamental data
3. macroeconomic data
4. news/text
5. sentiment
6. and alternative data

### 1. Price and volume data

This is the most familiar category and usually the first one people learn.

### A) OHLCV
OHLCV stands for:
- **Open**
- **High**
- **Low**
- **Close**
- **Volume**

It is typically recorded at a chosen time bar:
- 1 minute,
- 5 minutes,
- 1 hour,
- 1 day,
- etc.

#### What it means
For a given bar:
- **open** = first traded price in the interval,
- **high** = maximum traded price,
- **low** = minimum traded price,
- **close** = last traded price,
- **volume** = total traded quantity.

#### Why it matters
OHLCV is the classic starting point for:
- trend analysis,
- volatility analysis,
- return prediction,
- momentum features,
- pattern recognition.

#### Structured or unstructured?
OHLCV is **structured data** because it fits a clear table schema.

#### Example
Scenario: A 5-Minute Time Bar (10:00 - 10:05)
- **OHLC:** Price starts at \\$60k (Open), peaks at \$61k (High), drops to \\$59k (Low), and finishes at \$60.5k (Close).
- **Volume:** A total of 150 units were traded during this specific 5-minute interval.
- **Result:** Thousands of chaotic trades are compressed into one structured row of data for analysis.

### B) Tick data
Tick data records each trade or price change event as it happens.

A tick may include:
- timestamp,
- price,
- size,
- side,
- trade conditions.

#### Why it matters
Tick data gives higher resolution than OHLCV. It is useful for:
- microstructure research,
- intraday behavior,
- order-flow analysis,
- high-frequency features.

#### Challenge
Tick data is noisy, huge, and easy to misuse.

#### Example
Scenario: A Single Trade Event (Tick Data)
- **The Event:** At exactly 10:00:01.234, a single trade occurs where a buyer purchases 0.5 BTC at $60,000.50.
- **Data Recorded:** The database instantly logs the exact timestamp (in milliseconds), price, size, and trade side (Buy).
- **Result:** It captures the raw, atomic level of the market, preserving the micro-behavior before it gets aggregated into an OHLCV bar.

### C) Quote data
Quote data captures the best bid and ask available at a moment.

It often includes:
- bid price,
- ask price,
- bid size,
- ask size,
- timestamp.

#### Why it matters
Quote data helps you estimate:
- spread,
- liquidity,
- short-term execution cost,
- supply-demand imbalance.


### D) Order book data
Order book data shows many levels of buy and sell interest.

It may contain:
- price levels,
- sizes at each level,
- changes over time,
- order arrival and cancellation events.

#### Why it matters
This is central in microstructure and short-horizon trading.

Example questions:
- Is buying pressure stronger than selling pressure?
- Is liquidity thinning out?
- Is a large order likely to move the price?

#### Structured or semi-structured?
Order book data is usually **structured**, but depending on source format it can feel **semi-structured** because it is often nested, event-based, or delivered as snapshots and updates.


### E) Trade data
Trade data logs executed transactions.

It may include:
- timestamp,
- executed price,
- size,
- buyer/seller aggressor side,
- exchange or venue.

#### Why it matters
Trade data tells you what actually happened, not just what was offered.

### Market Data Hierarchy: From Intent to Aggregation
* **Order Book Data:** The full queue of buyers and sellers across multiple price levels; represents the overall intent of the market. What traders want to do (Intent).
* **Quote Data:** The front line of the Order Book; captures only the best available bid and ask prices (the top intentions).
* **Trade Data:** The moment an intention turns into reality; logs actual, executed transactions (What traders actually did (Reality)).
* **Tick Data:** The raw, event-driven stream of the market; records every single microscopic change (every executed trade and every quote update) with millisecond timestamps.
* **OHLCV Data:** The time-based compression; aggregates all individual trade data points within a fixed interval (e.g., 5 minutes) into 5 structured numbers The summary of what they did over time (Aggregation)..

### 2. Fundamental data

Fundamental data describes the underlying business or issuer.

#### Examples
- revenue
- net income
- earnings per share (EPS)
- assets and liabilities
- cash flow
- debt levels
- margins
- valuation ratios
- official annual or quarterly reports

#### Why it matters
Fundamentals are especially useful for:
- investing,
- long-horizon stock selection,
- valuation models,
- factor models,
- credit and risk analysis.

#### Research angle
Researchers may test whether:
- earnings surprises predict returns,
- valuation ratios relate to future performance,
- profitability improves ranking,
- fundamentals behave differently across regimes.

#### Structured or semi-structured?
Financial statements are usually **structured data**, though filings themselves are often **semi-structured** or partially **unstructured** because they include tables plus narrative text.


### 3. Macroeconomic data

Macro data describes the broad economic environment.

#### Examples
- central bank policy rates
- CPI inflation
- unemployment rate
- GDP growth
- PMI
- consumer confidence
- industrial production

#### Why it matters
Macro data often drives:
- asset class returns,
- risk appetite,
- sector rotation,
- yield curves,
- currency strength,
- equity valuation regimes.

#### Important time nuance
Macro data is often released **with delay** and later revised. That creates an important modeling issue:

> The value you see today may not be the value the market had at the time.

That matters a lot for backtesting and research.

### Structured or semi-structured?
Usually **structured data**, sometimes with revision history and release calendars that make it more complex.


### 4. News and text data

News, reports, company announcements, and analyst reports are one of the most important categories for modern AI.

#### Examples
- earnings press releases
- news articles
- regulatory announcements
- analyst notes
- CEO interviews
- transcripts
- social posts

#### Why it matters
Text often contains information before it is reflected in price.

AI can use NLP to:
- extract events,
- classify sentiment,
- summarize reports,
- detect topics,
- identify surprises.

#### Structured, semi-structured, or unstructured?
Mostly **unstructured data**.

Some sources are semi-structured because they contain:
- a headline,
- metadata,
- time,
- source,
- body text,
- tags.

But the main payload is free-form language.


### 5. Market sentiment data

Sentiment is not the same thing as text. Text is the raw input; sentiment is an extracted feature or latent signal.

#### Examples
- positive / negative / neutral news tone
- social-media mood
- fear / greed indicators
- event reaction strength
- surprise response to announcements

#### Why it matters
Markets are not purely rational. Sentiment can:
- accelerate moves,
- create overshooting,
- amplify volatility,
- or signal crowd positioning.

#### Important caution
Sentiment is often noisy and easy to overfit. A model that “understands sentiment” on paper may just be learning keyword shortcuts.


### 6. Alternative data

Alternative data means non-traditional data sources outside standard prices, fundamentals, and macro.

#### Examples
- blockchain transfers and wallet activity
- Google search trends
- app downloads
- website visits
- foot traffic
- shipment data
- satellite data
- payment data
- social engagement

#### Why it matters
Alternative data may reveal behavior earlier than traditional market data.

#### Practical examples
- rising website traffic may precede sales growth,
- increasing on-chain activity may precede crypto interest,
- search spikes may precede retail attention.

#### Challenge
Alternative data can be:
- expensive,
- hard to clean,
- delayed,
- incomplete,
- or hard to interpret.

#### Structured, semi-structured, or unstructured?
It depends:
- some are structured time series,
- some are semi-structured event feeds,
- some are derived from unstructured signals like images or text.


## 3.3 Data structure: structured, semi-structured, unstructured

This distinction helps you choose storage, tools, and modeling methods.

### Structured data
Data with a fixed schema and tabular shape.

#### Examples
- OHLCV table
- financial ratios table
- macro time series
- order book snapshot table

#### Traits
- easy to store in SQL or Parquet
- easy to filter and join
- easy for classical ML models


### Semi-structured data
Data with some organization, but not a rigid table.

#### Examples
- JSON API responses
- XML filings
- nested order book messages
- event feeds
- news objects with metadata and body text

#### Traits
- contains keys and hierarchy
- needs parsing and normalization
- common in APIs and financial feeds


### Unstructured data
Data without a fixed schema.

#### Examples
- news articles
- earnings call transcripts
- social media posts
- analyst comments
- images
- audio

#### Traits
- needs NLP or computer vision
- harder to clean and standardize
- often converted into features before modeling


### Why this distinction matters

A stock price table and a news article need totally different tools.

- Structured data → statistical models, time-series methods, tree models
- Semi-structured data → parsing, ETL, normalization
- Unstructured data → NLP, embeddings, transformers, extraction pipelines

This is a big part of data literacy: not just collecting data, but understanding its shape.


## 3.4 How each data type is used differently

### In academic research
Researchers often ask:
- Does this data contain predictive information?
- Does it add explanatory power?
- Does it behave differently by regime or horizon?

Example:
- Does news sentiment improve next-day return prediction?
- Do order book imbalances predict short-term movement?
- Do macro surprises explain sector performance?

### In trading
The question is:
- Can I trade this signal before it fades?
- Is the signal strong enough after costs?
- Is the data timely enough to act on?

Example:
- Tick or quote data is more relevant for fast trading.
- News sentiment may matter for event-driven strategies.

### In investing
The question is often:
- Does this data improve valuation or risk assessment over weeks or months?

Example:
- fundamentals and macro dominate longer-horizon investing.

### In production systems
The question becomes:
- Can we ingest this data reliably?
- Can we detect missing or delayed records?
- Can we keep schemas stable across vendors?


## 3.5 A practical example of a data hierarchy

Suppose you are analyzing a company stock:

- **price/volume** tells you how the market reacts,
- **fundamentals** tell you how the business is doing,
- **macro** tells you the economic background,
- **news** tells you what happened recently,
- **sentiment** tells you how people interpret the news,
- **alternative data** tells you what real-world activity may be ahead.

**This is why strong financial models are usually multi-source systems, not single-source systems.**


## 3.6 A small Python example

```python
import pandas as pd

# Structured data: OHLCV
ohlcv = pd.DataFrame({
    "date": pd.to_datetime(["2026-01-01", "2026-01-02", "2026-01-03"]),
    "open": [100, 102, 101],
    "high": [103, 104, 105],
    "low": [99, 100, 100],
    "close": [102, 101, 104],
    "volume": [1500000, 1800000, 1700000]
})

# Semi-structured data: API-like records
news_records = [
    {
        "timestamp": "2026-01-03T09:00:00Z",
        "source": "Reuters",
        "headline": "Company X reports stronger-than-expected earnings",
        "body": "Company X posted revenue growth..."
    },
    {
        "timestamp": "2026-01-03T11:00:00Z",
        "source": "Bloomberg",
        "headline": "Market reacts to central bank comments",
        "body": "Traders shifted expectations..."
    }
]

# Unstructured data: text content extracted from news
texts = [item["body"] for item in news_records]

print(ohlcv)
print(texts)
```

This example shows three levels:
- a table for structured data,
- a list of dictionaries for semi-structured data,
- raw text for unstructured data.

# 4. Real-World Example

Imagine you are building a model for **short-term stock movement after earnings**, but instead of relying only on market data, you also bring in **social media data** as an alternative signal.

Now think about platforms like **X** and **Instagram**. They generate huge amounts of content every day, and that content can help explain how fast information, attention, and sentiment spread around a stock, sector, or event.

- X’s own engineering blog says the platform processes **roughly 500 million Tweets per day**. 
- Instagram publicly reported that users share **more than 95 million photos and videos per day**. That is the clearest public daily-content figure available, even though the platform’s current exact daily volume is not officially published. Reuters also reported in 2025 that Instagram had reached **3 billion monthly active users**, which shows how massive the platform’s reach has become. 

## Data sources
- OHLCV data for price behavior
- trading volume for attention
- company filings for financial fundamentals
- news article text for event context
- sentiment score from headlines and social posts
- social media content from X and Instagram as alternative data

## What each source contributes
- **OHLCV**: how the market was moving before and after the event
- **volume**: whether the market cared
- **fundamentals**: whether the company was strong or weak
- **news text**: what actually happened
- **sentiment**: how the market interpreted the event
- **X / Instagram content**: how fast public attention and crowd reaction are building around the event

## Why this is powerful
No single data source gives the full picture.

- Price alone tells you reaction, not reason.  
- Text alone tells you narrative, not market response.  
- Fundamentals alone tell you business quality, not immediate trading pressure.  
- And social media alone tells you what people are saying, not necessarily what the market will do.

This is why data literacy matters: it helps you combine signals from different layers of reality. It also helps you understand the **scale** of each source. For example, X produces roughly **500 million Tweets per day**, while Instagram has historically reported **95 million photos and videos shared per day**. That means both platforms are massive streams of unstructured or semi-structured alternative data, not just “social apps.” 



# 5. Common Mistakes / Pitfalls

## 1. Thinking price data is enough
Price is important, but it is only one part of the market state.

## 2. Mixing data types without understanding them
Joining sentiment, macro, and order book data without time alignment can create nonsense.

## 3. Confusing raw text with sentiment
Sentiment is a derived signal, not the same as the article itself.

## 4. Ignoring release times
Macro and fundamental data must be aligned to when the market actually had access to them.

## 5. Using future revisions unknowingly
Some macro and fundamental data are revised later. If you use the revised version in backtests, you may be leaking future knowledge.

## 6. Overvaluing alternative data
Alternative data is not automatically better. Sometimes it is just expensive noise.

## 7. Treating structured and unstructured data the same
They require different preprocessing, storage, and modeling strategies.

## 8. Forgetting the time horizon
A data source useful for long-term investing may be useless for intraday trading.

## 9. Ignoring missingness
Missing data is often informative or harmful, depending on the source.

## 10. Not checking source reliability
Two providers may report the same metric differently.


# 6. Extra Important Concepts

## 6.1 Data frequency
Data can be:
- tick-level,
- intraday,
- daily,
- weekly,
- monthly,
- quarterly.

The frequency must match the problem.

## 6.2 Data latency
Latency is the delay between data generation and data availability.

This matters a lot in trading and event-driven systems.

## 6.3 Data revision
Some financial and macro data are updated after initial release.

For research, you often need the **as-of** version, not the final revised version.

## 6.4 Data granularity
Granularity means how detailed the data is.

Example:
- daily close is low granularity,
- tick or order book data is high granularity.

## 6.5 Labels are data too
In supervised learning, the target label is also data.

If you define the wrong label, the whole model becomes misaligned.

## 6.6 Derived features are not raw data
A volatility score, sentiment score, or momentum indicator is created from data.  
That distinction matters for interpretability and leakage checks.


# 7. Summary and What Comes Next

Phase 2 teaches a crucial lesson:

> Financial data is not just market price.  
> It is a multi-layer information system made of structured, semi-structured, and unstructured signals.

The most important takeaways are:

- **structured data**: OHLCV, ratios, macro series, order book tables
- **semi-structured data**: JSON feeds, filing objects, event records
- **unstructured data**: news, transcripts, social posts, reports
- different data types support different horizons and tasks
- the usefulness of data depends on timing, quality, and alignment
- research, trading, investing, and production each use the same data differently

## Mini tasks
1. Pick one asset and list at least one example from each data category.
2. Classify each of these as structured, semi-structured, or unstructured: OHLCV, earnings press release, JSON API response, analyst report.
3. Explain which data type you would use for:
   - intraday trading,
   - long-term investing,
   - event-driven research.
4. Write down one example of a data leakage risk for macro data.
5. Think of one alternative data source and explain what financial behavior it might reveal.

## What comes next
The next phase is **Data Sources and Access Methods**, where the focus shifts from “what data exists” to **where to get it, how APIs work, how to store it, and how to build a clean ingestion pipeline**.