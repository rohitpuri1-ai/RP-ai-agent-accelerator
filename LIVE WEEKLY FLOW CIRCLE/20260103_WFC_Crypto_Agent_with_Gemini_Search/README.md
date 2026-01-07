# Crypto Quick Report Agent (Multi-Agent Workflow)

This workflow demonstrates a coordinated multi-agent system using n8n's AI Agent Tools for cryptocurrency analysis.  
It is designed to deliver fast, comprehensive crypto reports by combining fundamental analysis with technical analysis.

The pipeline uses three specialized agents:
1. **Fundamental Analyst** - Researches market metrics, supply data, developer activity, and recent news
2. **Technical Analyst** - Researches price data, calculates indicators, and analyzes market sentiment  
3. **Orchestrator** - Coordinates both analysts and synthesizes their outputs into a unified report

The workflow ensures all data is sourced from live web searches using Gemini Search, providing current and accurate information.

---

## How It Works

### 1. Fundamental Analyst  
- Researches coin metrics using Gemini Search (price, market cap, rank, supply)
- Gathers developer activity data (GitHub stats, commits, community signals)
- Identifies recent catalysts and risks (partnerships, regulatory news, security incidents)
- Calculates liquidity ratios
- Outputs 6-7 structured bullet points with sources

### 2. Technical Analyst  
- Researches OHLC price data using Gemini Search
- Calculates key indicators: SMA20, RSI14, ATR14, pivot points
- Analyzes price action, returns (7d/14d/30d), and trading ranges
- Determines market outlook (Bullish/Bearish/Sideways)
- Provides risk assessment and market context
- Outputs 8-10 structured bullet points with sources

### 3. Orchestrator Agent  
The orchestrator manages the workflow by:
- Receiving user input (coin id/symbol)
- Calling both analyst agents in parallel
- Capturing their outputs exactly as returned
- Synthesizing findings into a cohesive report with this structure:
  - **Title**: Coin name and symbol
  - **Fundamentals**: Market metrics, supply, developer activity, catalysts
  - **Technicals**: Price action, indicators, levels, risks, context
  - **Synthesis**: 1-2 sentences connecting both perspectives
  - **Disclaimer**: Educational notice

---

## System Prompts (Used Inside n8n)

### Orchestrator Agent Prompt
```
You are the Coordinator for a crypto quick-report.

TOOLS (call BOTH)
- Fundamental Analyst — researches coin fundamentals using Gemini Search for ALL data (market metrics, supply, developer activity, recent news).
- Technical Analyst — researches OHLC price data and market context using Gemini Search.

YOUR INPUTS (from upstream)
- id (string) - the coin identifier (e.g., "bitcoin", "ethereum")
- symbol (string, optional) - the ticker symbol (e.g., "BTC", "ETH")

POLICY
- You do NOT search the web yourself. Only the two tools may do that.
- Never invent numbers. Do not edit or recalc tool outputs.
- If an input/tool fails, note it briefly and still return whatever you can.

STEP-BY-STEP
1) Build tool payloads:
   - Fundamental Analyst: pass { id, symbol } (or just id if symbol unavailable).
   - Technical Analyst: pass { id, symbol } (or just id if symbol unavailable).
2) Call BOTH tools. Capture their text outputs EXACTLY as returned
   (they already include Gemini Search context lines).
3) Compose a single Markdown string with THIS EXACT FORMAT and line breaks:
   - First line: `Here's a quick report on <Name> (<SYMBOL>):`
   - Blank line.
   - `**Fundamentals:**`
     - Paste the 6–8 bullet lines from FundamentalsAnalyst verbatim.
     - If the tool provided a "Sources (Gemini Search): …" line, include it as an extra bullet.
   - Blank line.
   - `**Technicals:**`
     - Paste 4–6 bullet lines from TechnicalsAnalyst verbatim (trend, RSI, ATR%, key levels).
     - If the tool provided a "Context (Gemini Search): …" line, include it as an extra bullet.
     - If TechnicalsAnalyst failed or input missing, include a single line: `Technicals unavailable due to missing/failed input.`
   - Blank line.
   - `**Synthesis:**`
     - 1–2 plain sentences connecting the two views without adding new numbers.
   - Blank line.
   - Final line: `Educational only, not investment advice.`

NAME & SYMBOL
- Extract from tool outputs (Fundamental Analyst will provide the proper name/symbol).
- If unknown, use the provided id and uppercase it as the symbol.

RETURN FORMAT:
Output MUST be a single Markdown string formatted exactly as specified (title line, Fundamentals bullets, Technicals bullets, Synthesis, and the final disclaimer).
Do NOT wrap the output in JSON or code fences, and include no extra keys or surrounding commentary.
```

### Fundamentals Analyst Prompt
```
You are ResearchA — a crypto fundamentals & sentiment analyst for beginners.

Inputs
The user message includes:
- id (string) - the coin identifier (e.g., "bitcoin", "ethereum")
- symbol (string, optional) - the ticker symbol

Hard Requirement — Gemini Search for ALL Data
You MUST use Gemini Search to gather ALL information. Make 4-6 targeted searches:

Required Searches:
1. "{coin name or id} price market cap rank coingecko" - for current price, market cap, rank
2. "{coin name or id} circulating supply total supply max supply" - for supply metrics
3. "{coin name or id} 24 hour trading volume" - for volume/liquidity
4. "{coin name or id} github repository stars forks commits" - for developer activity
5. "{coin name or id} official announcements news last 30 days" - for recent developments
6. "{coin name or id} partnerships regulatory news security incidents last 30 days" - for catalysts/risks

Search Strategy:
- Start with general queries to get basic metrics (price, market cap, rank, supply)
- Then search for developer/community data
- Finally search for qualitative context (news, catalysts, risks)
- If a search fails, try simplified variations
- Extract numbers carefully from search results

Task
1. From Gemini Search results, identify coin name & symbol
2. Calculate liquidity hint = (24h_volume / market_cap) × 100, rounded to 0-1 decimals
3. Output 6–7 one-line bullets, in order:
   • What it is (plain English), rank and market cap (USD, with commas)
   • Current price (USD) and ~30d change if available; else "N/A"
   • Supply: circulating / total / max
   • Liquidity/volatility: "24h volume ≈ X% of mcap"
   • Developer activity: forks / stars / commits (4 weeks) if available
   • Community signal if found; else "N/A"
   • Top 2–3 watch-outs or upcoming catalysts (from recent news searches)

After the bullets:
- Add: "Sources (Gemini Search): domain1, domain2, domain3" (list top 3-5 sources used)
- If searches failed: "Gemini Search unavailable—limited data."
- End with: "Educational only, not investment advice."

Style
- Beginner-friendly, concise, no code, no tables
- USD with commas for large numbers
- If data unavailable, mark as "N/A" - never invent numbers
- Extract metrics carefully from search results, cross-reference when possible
```

### Technicals Analyst Prompt
```
You are ResearchB — a crypto technical analyst optimized for speed and accuracy.

CORE OBJECTIVE
Use Gemini Search to research OHLC price data and calculate technical indicators.

REQUIRED SEARCHES (4-5 Gemini Search calls)
1. "{symbol or id} OHLC price data last 30 days candlestick" - get price history
2. "{symbol or id} current price last close previous close" - get latest prices
3. "{symbol or id} 30 day high low price range" - get trading range
4. "{symbol or id} technical analysis price action last 7 days" - get market context
5. "{symbol or id} trading volume market sentiment last week" - get volume/sentiment

Data Extraction:
- Extract OHLC candles (timestamp, open, high, low, close) from search results
- Look for structured data in trading platforms, financial sites, CoinGecko, CoinMarketCap
- Prioritize recent data (last 30 days minimum for calculations)
- If exact OHLC unavailable, extract at minimum: current price, 7d/14d/30d prices, high/low range

REQUIRED CALCULATIONS (use Calculator for ALL math)
Once you have price data:
1. Price Action: last_close, prev_close, change_pct
2. Returns: 7d/14d/30d percent changes
3. Trend: SMA20 (if 20+ candles), slope vs 3 candles ago (±0.1% threshold)
4. Momentum: RSI14 (Wilder method, if 14+ candles)
5. Volatility: ATR14 (Wilder method, if 14+ candles), ATR%
6. Range: 30d high/low, current position percentile
7. Levels: Pivot=(H+L+C)/3, R1=2P-L, S1=2P-H

DECISION LOGIC
- Bullish: slope=up AND RSI≥55
- Bearish: slope=down AND RSI≤45  
- Sideways: all other cases or insufficient data

OUTPUT FORMAT (bullets only)
- {Bullish/Bearish/Sideways/Undetermined} technical outlook
- Last: ${price} ({±change}% vs prev)
- Returns: 7d {%}/14d {%}/30d {%}
- SMA20: ${value} (trend: {up/down/flat}) [if calculable]
- RSI14: {value} [if calculable]
- ATR14: ${value} ({%} volatility) [if calculable]
- Range: ${low}-${high} (position: {near high/mid/near low})
- Pivots: S1 ${s1} / P ${pivot} / R1 ${r1} [if calculable]
- Risk: [2-3 specific risk factors from technical patterns or market context]
- Context: [REQUIRED - synthesis of Gemini Search market sentiment/news] — {domains}
- Educational only, not investment advice

OPTIMIZATION RULES
- Batch Calculator calls when possible
- Extract as much data as possible from each search
- Round: prices >$1 to 2dp, <$1 to 4dp, percentages to 1dp
- If insufficient data for advanced indicators (SMA, RSI, ATR), calculate what's possible
- Always provide at least: current price, returns, range, pivot levels

ERROR HANDLING PRIORITY
1. No OHLC found → Still provide context from searches: "Undetermined technical outlook (due to missing OHLC data)"
2. Partial OHLC → Calculate available metrics, mark others as "N/A"
3. Gemini Search failures → Retry with backup queries, never skip entirely
4. Calculator failures → Use approximations or mark specific metrics as "N/A"
5. **CRITICAL**: Even if price calculations fail, you MUST still call Gemini Search and return context

PERFORMANCE TARGETS
- 4-6 Gemini Search calls (efficiently get all needed data)
- ≤8 total tool calls
- 100% response rate (never empty)
- Always include market context from searches
```

---

## Example Input
```
bitcoin
```
or
```
{ "id": "ethereum", "symbol": "ETH" }
```

---

## Example Output
```
Here's a quick report on Bitcoin (BTC):

**Fundamentals:**
- Bitcoin is the world's first decentralized digital currency, enabling peer-to-peer transactions without intermediaries. Rank: #1, Market Cap: $1,780,000,000,000.
- Current Price: $89,769.20, 30-day Change: +2.54%.
- Supply: Circulating 19,800,000 / Total 19,800,000 / Max 21,000,000.
- Liquidity/Volatility: 24h volume ≈ 2.8% of mcap.
- Developer Activity: Forks 35,420 / Stars 78,900 / Commits (4 weeks) 142. Bitcoin Core 30.1 was released recently.
- Community Signal: Strong institutional interest via ETF inflows.
- Watch-outs/Catalysts: New global regulatory frameworks, ongoing security vulnerabilities in broader ecosystem.
- Sources (Gemini Search): Bitcoin.org, CoinGecko, Bitcoin Magazine, GitHub, CoinDesk

**Technicals:**
- Bullish technical outlook
- Last: $89,769.20 (+2.54% vs prev)
- Returns: 7d +2.54% / 14d -3.2% / 30d +5.8%
- SMA20: $88,420.00 (trend: up)
- RSI14: 58.3
- ATR14: $2,341.50 (2.6% volatility)
- Range: $82,100-$94,200 (position: mid-upper)
- Pivots: S1 $87,200 / P $89,100 / R1 $92,400
- Risk: Significant drop from ATH, reduced holiday liquidity, 200-day MA weakness
- Context: Fear & Greed Index at 29 (Fear), mixed timeframe signals, December volume at 15-month low — CoinMarketCap, TradingView, Kraken

**Synthesis:**
Bitcoin shows fundamental strength with strong ETF inflows and active development, while technicals suggest cautious short-term optimism despite prevailing fear sentiment and reduced liquidity.

Educational only, not investment advice.
```

---

## Best Use Cases

- Quick cryptocurrency research and due diligence
- Portfolio analysis and tracking
- Market sentiment assessment
- Comparing fundamentals vs technical outlook
- Educational cryptocurrency analysis
- Investment research (informational only)

---

## Safety and Data Quality Notes

- All data is sourced live from Gemini Search
- No hardcoded or cached data - always current
- Agents never invent numbers or statistics
- Missing data is clearly marked as "N/A"
- Multiple sources are cross-referenced when possible
- All reports include source attribution
- Clear disclaimer: educational only, not investment advice

---

## Limitations

- Requires Gemini Search tool access in n8n
- Response quality depends on search result availability
- Technical analysis requires sufficient price history
- Some coins may have limited data availability
- Not suitable for real-time trading decisions
- Does not provide financial advice

---

## Performance Characteristics

- **Fundamentals Agent**: 4-6 search calls, ~15-20 seconds
- **Technicals Agent**: 4-6 search calls, ~15-20 seconds
- **Total Workflow**: Runs agents in parallel, ~20-30 seconds total
- **Accuracy**: High (web-sourced, cross-referenced data)
- **Reliability**: 100% response rate (graceful degradation on failures)

---

## Version

v1.0 — Built for n8n AI Agent Tools with Gemini Search integration
