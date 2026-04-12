# Multi-Agent Stock Analysis System

## Workflow Overview

This workflow implements a multi-agent architecture that combines fundamental and technical analysis to provide comprehensive stock market insights.

An orchestrator agent coordinates two specialized sub-agents, each performing domain-specific analysis with web search capabilities.

---

## Workflow Diagram

```
Chat → Orchestrator Agent → [Fundamental Agent + Technical Agent] → Combined Analysis
```

---

## Node Sequence

1. **When chat message received** - Receives stock analysis requests
2. **AI Agent (Orchestrator)** - Coordinates sub-agents and combines outputs
   - **Google Gemini Chat Model** - Main reasoning engine
   - **Simple Memory** - Maintains conversation context
   - **Fundamental Analysis Agent** - Analyzes business fundamentals
     - **Google Gemini Chat Model1** - Powers fundamental analysis
     - **Gemini Search Tool** - Fetches news, earnings, macro data
   - **Technical Analysis Agent** - Analyzes price action and indicators
     - **Google Gemini Chat Model2** - Powers technical analysis
     - **Gemini Search Tool1** - Fetches price data and indicators

---

## System Prompts

### Orchestrator Agent

```
You are an Orchestrator Agent.
Your role:
1. Send the user query to:
   - Fundamental Analysis Agent
   - Technical Analysis Agent
2. Ensure both agents use their search tools before responding.
3. Collect both outputs.
4. Combine them into one clear, structured response.
Output format:
- Fundamental Insights:
- Technical Insights:
- Final Summary:
Be concise, no assumptions, only use agent outputs.
```

### Fundamental Analysis Agent

**Important:** Use this Fundamental Analysis Tool only once per user request

```
You are a Fundamental Analysis Agent.
Your role:
1. Use the search tool to find recent data (news, earnings, macro trends).
2. Analyze factors like revenue, growth, industry, sentiment.
3. Do not answer without using search.
Output format:
- Key Findings:
- Market Sentiment:
- Fundamental View (Bullish/Bearish/Neutral):
Be concise and insight-driven.
```

### Technical Analysis Agent

**Important:** Use this Technical Analysis Tool only once per user request

```
You are a Technical Analysis Agent.
Your role:
1. Use the search tool to find recent price/indicator data.
2. Analyze trends, support/resistance, momentum.
3. Do not answer without using search.
Output format:
- Trend:
- Key Levels (Support/Resistance):
- Indicator Signals:
- Technical View (Bullish/Bearish/Neutral):
Keep it short and precise.
```

---

## What This Workflow Does

- Accepts stock ticker queries from users
- Orchestrates parallel analysis by two specialized agents
- Performs fundamental analysis (earnings, news, sentiment, growth)
- Performs technical analysis (trends, support/resistance, indicators)
- Combines both perspectives into a unified investment view
- Provides structured, actionable insights

---

## How It Works

**Step 1: User Query**

User requests analysis for a stock (e.g., "Analyze NVIDIA")

**Step 2: Orchestration**

The **Orchestrator Agent**:
1. Routes the query to both specialized agents simultaneously
2. Ensures both agents use search tools
3. Waits for both responses

**Step 3: Fundamental Analysis**

The **Fundamental Analysis Agent**:
1. Searches for recent news, earnings reports, industry trends
2. Analyzes revenue growth, market position, sentiment
3. Returns bullish/bearish/neutral view with key findings

**Step 4: Technical Analysis**

The **Technical Analysis Agent**:
1. Searches for price data, chart patterns, indicators
2. Identifies trends, support/resistance levels, momentum
3. Returns bullish/bearish/neutral view with technical signals

**Step 5: Combined Response**

The **Orchestrator** combines both outputs into:
- Fundamental Insights
- Technical Insights
- Final Summary with actionable recommendation

---

## Example Output Format

```
Stock: NVIDIA (NVDA)

Fundamental Insights:
- Q4 earnings beat expectations with 265% YoY revenue growth
- Data center segment driving growth (AI chip demand)
- Market sentiment: Strongly bullish
- Fundamental View: Bullish

Technical Insights:
- Trend: Strong uptrend on daily and weekly charts
- Support: $875, Resistance: $950
- RSI: 68 (approaching overbought)
- MACD: Bullish crossover confirmed
- Technical View: Bullish with caution near resistance

Final Summary:
Both fundamental and technical analysis align bullish. Strong 
fundamentals driven by AI demand. Technically strong but nearing 
resistance - consider entry on pullback to $875 support.
```

---

## Prompts to Try

- "Analyze NVIDIA"
- "What's your view on Tesla stock?"
- "Give me analysis on Microsoft"
- "Should I buy Apple right now?"
- "Analyze Amazon stock"

---

## Key Features

**Multi-Agent Architecture**
- Orchestrator coordinates specialized sub-agents
- Parallel processing for faster analysis
- Clear separation of concerns (fundamental vs technical)

**Enforced Search Usage**
- Both agents must use search tools before responding
- Ensures analysis is based on current data
- Prevents hallucination or outdated information

**Structured Output**
- Consistent format across all analyses
- Clear bullish/bearish/neutral signals
- Actionable insights and key levels

**Domain Specialization**
- Fundamental agent focuses on business metrics
- Technical agent focuses on price action
- Each agent optimized for its analysis type

---

## Use Cases

- Pre-trade research and analysis
- Quick stock sentiment checks
- Multi-perspective investment evaluation
- Learning fundamental and technical analysis
- Validating trading ideas with dual analysis

---

## Mental Model

This is not a single AI giving opinions.

**This is a committee of specialists reaching consensus.**

The difference:
- Single agent = One perspective, potential bias
- Multi-agent = Multiple perspectives, cross-validation
- Orchestrator ensures both viewpoints are considered
- Final output is more balanced and comprehensive

---

## Architecture Pattern

```
User Query → Orchestrator → [Fundamental Agent || Technical Agent] → Search → Analysis → Synthesis → Response
```

The orchestrator ensures both agents contribute their expertise before forming a final recommendation.

---

## The Core Insight

Stock analysis requires multiple perspectives.

Fundamental analysis answers "WHAT to buy."
Technical analysis answers "WHEN to buy."

This workflow combines both to answer "WHAT to buy and WHEN to buy it."

---

## Disclaimer

This tool provides analysis based on publicly available data. It is not financial advice. Always conduct your own research and consult with financial professionals before making investment decisions.