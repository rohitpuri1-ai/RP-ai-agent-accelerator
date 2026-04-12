# Scheduled Stock Analysis Email

## Workflow Overview

This workflow implements a scheduled multi-agent stock analysis system that automatically emails comprehensive market insights at specified intervals.

An orchestrator agent coordinates two specialized sub-agents, synthesizes their analysis, and delivers formatted HTML email reports directly to your inbox.

---

## Workflow Diagram

```
Schedule Trigger → Stock Input → Orchestrator → [Fundamental + Technical Agents] → HTML Email → Gmail
```

---

## Node Sequence

1. **Schedule Trigger** - Executes workflow at specified intervals (daily, weekly, etc.)
2. **Enter Stock (manual)** - Defines which stock ticker to analyze
3. **AI Agent (Orchestrator)** - Coordinates analysis and generates email
   - **Google Gemini Chat Model** - Main reasoning and HTML generation
   - **Fundamental Analysis Agent** - Analyzes business fundamentals
     - **Google Gemini Chat Model1** - Powers fundamental analysis
     - **Gemini Search Tool** - Fetches news, earnings, macro data
   - **Technical Analysis Agent** - Analyzes price action and indicators
     - **Google Gemini Chat Model2** - Powers technical analysis
     - **Gemini Search Tool1** - Fetches price data and indicators
4. **Send a message in Gmail** - Delivers HTML email report

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
4. Combine them into one clear response.
5. Create a clean HTML email version of the final response.
6. Pass that HTML to the Gmail send node.
Output format:
- Fundamental Insights:
- Technical Insights:
- Final Summary:
- Email Subject:
- Email HTML:
Rules:
- Be concise.
- Do not make assumptions.
- Only use agent outputs.
- Email HTML must be simple, clean, and readable using basic HTML tags only.
```

### Fundamental Analysis Agent

**Description:**
```
Use this Fundamental Analysis Tool only once per user request
```

**System Message:**
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

**Description:**
```
Use this Technical Analysis Tool only once per user request
```

**System Message:**
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

- Runs automatically on a schedule (daily, weekly, or custom interval)
- Analyzes a predefined stock ticker using multi-agent system
- Performs fundamental analysis (earnings, news, sentiment, growth)
- Performs technical analysis (trends, support/resistance, indicators)
- Combines both perspectives into a unified investment view
- Generates clean HTML-formatted email
- Sends analysis report directly to Gmail inbox

---

## How It Works

**Step 1: Scheduled Trigger**

The workflow executes automatically based on your schedule configuration (e.g., every weekday at 8 AM)

**Step 2: Stock Input**

The **Enter Stock** node provides the ticker symbol to analyze (configured manually)

**Step 3: Orchestration**

The **Orchestrator Agent**:
1. Routes the stock query to both specialized agents
2. Ensures both agents use search tools
3. Waits for both responses
4. Combines outputs into structured analysis
5. Generates HTML email format

**Step 4: Fundamental Analysis**

The **Fundamental Analysis Agent**:
1. Searches for recent news, earnings reports, industry trends
2. Analyzes revenue growth, market position, sentiment
3. Returns bullish/bearish/neutral view with key findings

**Step 5: Technical Analysis**

The **Technical Analysis Agent**:
1. Searches for price data, chart patterns, indicators
2. Identifies trends, support/resistance levels, momentum
3. Returns bullish/bearish/neutral view with technical signals

**Step 6: Email Delivery**

The orchestrator generates:
- **Subject**: Defined automatically by the model
- **Message**: HTML content defined automatically by the model

These are passed to the **Gmail** node which sends the email

---

## Email Output Format

The HTML email includes:

```
Subject: Stock Analysis: [TICKER] - [Date]

Body:
📊 Fundamental Insights:
- Key findings from business analysis
- Market sentiment assessment
- Fundamental view (Bullish/Bearish/Neutral)

📈 Technical Insights:
- Current trend direction
- Support and resistance levels
- Indicator signals (RSI, MACD, etc.)
- Technical view (Bullish/Bearish/Neutral)

💡 Final Summary:
- Combined perspective
- Actionable recommendation
- Key levels to watch
```

---

## Configuration Steps

1. **Set Schedule**: Configure Schedule Trigger for desired frequency
2. **Enter Stock Ticker**: Set the stock symbol in the "Enter Stock" node
3. **Configure Gmail**: 
   - Connect Gmail account
   - Email Type is set to HTML
   - Subject and Message are defined automatically by the orchestrator model
4. **Test Workflow**: Run manually before activating schedule
5. **Activate**: Enable workflow to run on schedule

---

## Key Features

**Automated Delivery**
- Set-and-forget scheduled execution
- No manual intervention required
- Consistent daily/weekly reports

**Multi-Agent Analysis**
- Parallel fundamental and technical analysis
- Cross-validated insights
- Balanced perspective on stocks

**Clean HTML Emails**
- Professional formatting
- Easy to read on mobile and desktop
- Structured layout for quick scanning

**Enforced Search Usage**
- Both agents must use search tools
- Ensures current, accurate data
- Prevents hallucination

---

## Use Cases

- Daily morning stock briefings before market open
- Weekly portfolio review emails
- Pre-earnings analysis automation
- Watchlist monitoring and alerts
- Educational tool for learning market analysis

---

## Schedule Examples

**Daily Morning Briefing:**
- Trigger: Every weekday at 7:00 AM
- Use: Pre-market analysis before trading

**Weekly Review:**
- Trigger: Every Sunday at 6:00 PM
- Use: Weekend portfolio planning

**Pre-Earnings:**
- Trigger: One day before earnings date
- Use: Prepare for earnings announcements

---

## Limitations

**Single Stock Per Execution**
- Analyzes one ticker at a time
- For multiple stocks, duplicate workflow or use loops

**Schedule Trigger Dependency**
- Requires workflow to be active
- n8n instance must be running

**Email Formatting**
- Basic HTML only (no complex CSS)
- Optimized for email client compatibility

**Search Tool Dependency**
- Analysis quality depends on search results
- May have slight delays in data freshness

---


## Mental Model

This is not a manual research workflow.

**This is your automated investment analyst.**

The difference:
- Manual research = Time-consuming, inconsistent
- Scheduled automation = Reliable, regular insights
- Email delivery = Analysis comes to you
- Multi-agent = Comprehensive perspective

Set it once, receive insights forever.

---

## Architecture Pattern

```
Schedule → Input → Orchestrator → [Fundamental || Technical] → HTML Generation → Email Delivery
```

The workflow runs autonomously, delivering investment insights directly to your inbox without any manual intervention.

---

## The Core Insight

Markets move daily.

Your analysis should too.

This workflow ensures you wake up to fresh, multi-perspective market analysis every day, without lifting a finger.

---

## Disclaimer

This tool provides automated analysis based on publicly available data. It is not financial advice. Always conduct your own research and consult with financial professionals before making investment decisions. Past performance does not guarantee future results.