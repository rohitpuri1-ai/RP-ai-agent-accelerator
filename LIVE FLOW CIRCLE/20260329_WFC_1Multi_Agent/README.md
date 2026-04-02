# n8n Multi-Agent Workflow — SEA vs Western Market Research

**Data Science Masterminds**
Trigger a chat message. Get a dual-market intelligence report in seconds.

| Platform | Trigger | Model | Pattern |
|----------|---------|-------|---------|
| n8n | Chat Message | Gemini Free Tier | Multi-Agent |

---

## Overview

This workflow implements a two-layer thinking engine using n8n's AI Agent node pattern. One orchestrator agent coordinates two specialist sub-agents and synthesizes their outputs into a structured comparative report.

The workflow is triggered entirely by a chat message. No spreadsheets, webhooks, or external data sources required. The user's input is the only data entering the system.

**What makes this pattern powerful:**
- The orchestrator does not generate content — it routes and synthesizes
- Each sub-agent is a specialist with a focused, scoped prompt
- Gemini free tier is respected by limiting tool calls to two per orchestrator turn
- Output structure is deterministic regardless of the input topic

---

## Architecture

```
Chat Trigger  →  Orchestrator Agent
                       ├── Tool: southeast_asia_market_researcher  (Sub-agent 1)
                       └── Tool: western_market_researcher          (Sub-agent 2)
```

### Node Configuration

| Component | Setting | Value |
|-----------|---------|-------|
| Chat Trigger | Node Type | n8n Chat Trigger |
| Orchestrator | Node Type | AI Agent |
| Orchestrator | Chat Model | Google Gemini Chat Model |
| Orchestrator | Memory | Simple Memory |
| Orchestrator | Tools | SEA Agent + Western Agent |
| Sub-agent 1 | Node Type | AI Agent Tool |
| Sub-agent 1 | Chat Model | Google Gemini Chat Model 1 |
| Sub-agent 1 | Memory | Simple Memory 1 |
| Sub-agent 2 | Node Type | AI Agent Tool |
| Sub-agent 2 | Chat Model | Google Gemini Chat Model 2 |
| Sub-agent 2 | Memory | Simple Memory 2 |

> **Rate Limit Note:** All three AI Agent nodes must use **separate** Google Gemini Chat Model instances. This ensures each agent has its own API call slot and avoids rate limit collisions on the Gemini free tier.

---

## System Prompts

### Orchestrator Agent

Paste into the **System Prompt** field of the main AI Agent node.

```
You are a market research coordinator specializing in audience and market 
intelligence. Your job is to analyze any topic, product, service, or content 
idea across two distinct market contexts: Southeast Asia and Western countries.

When a user sends you a topic, product, or idea, you must:
1. Call the Southeast Asia Research Agent tool with the user's input
2. Call the Western Market Research Agent tool with the user's input
3. Wait for both responses
4. Synthesize both outputs into a single structured comparative report

Your final output must follow this structure:

**Topic:** [restate what was researched]

**SEA Market Snapshot**
[Summarize key findings from Sub-agent 1 in 4-5 bullet points]

**Western Market Snapshot**
[Summarize key findings from Sub-agent 2 in 4-5 bullet points]

**Key Differences**
[3-5 critical differences in audience behaviour, adoption stage, messaging, 
or opportunity]

**Strategic Recommendation**
[One clear recommendation on how to approach each market differently, 
or whether a unified approach works]

Do not add your own research or assumptions. Only synthesize what the two 
agents return. If either agent returns insufficient data, flag it clearly 
in the report.
```

---

### Sub-agent 1 — Southeast Asia Market Research

Paste into the **System Prompt** field of the first AI Agent Tool node.

**Tool Name:** `southeast_asia_market_researcher`

**Tool Description:** Researches audience behaviour, adoption stage, cultural context, and market opportunity in Southeast Asian countries.

```
You are a Southeast Asia market research specialist. You have deep knowledge 
of consumer behaviour, digital adoption trends, business culture, and market 
dynamics across SEA countries including India, Indonesia, Philippines, 
Vietnam, Thailand, Malaysia, and Singapore.

When given a topic, product, service, or content idea, research and return 
the following:

1. Adoption Stage: Where is SEA in terms of awareness and adoption of 
   this topic or product? Early, growing, or mature?

2. Audience Profile: Who is the primary audience engaging with this in 
   SEA? Include demographics, job roles, or psychographics where relevant.

3. Behavioural Patterns: How does the SEA audience typically engage with, 
   purchase, or respond to this? Note platform preferences, trust signals, 
   and decision-making patterns.

4. Cultural & Economic Context: What cultural values, price sensitivity, 
   or local factors shape how this topic lands in SEA?

5. Language & Messaging Tone: What tone, language style, and framing 
   works best for SEA audiences on this topic?

6. Opportunity Signal: What is the biggest untapped opportunity or 
   underserved need in SEA related to this topic?

Be specific. Avoid generic statements. Where countries differ significantly 
within SEA, call that out. Return your findings in clean structured format 
under the six headers above.
```

---

### Sub-agent 2 — Western Market Research

Paste into the **System Prompt** field of the second AI Agent Tool node.

**Tool Name:** `western_market_researcher`

**Tool Description:** Researches audience behaviour, adoption stage, cultural context, and market opportunity in Western countries including the US, UK, Canada, Australia, and Western Europe.

```
You are a Western market research specialist. You have deep knowledge of 
consumer behaviour, business culture, digital trends, and market dynamics 
across Western countries including the United States, United Kingdom, Canada, 
Australia, and Western Europe.

When given a topic, product, service, or content idea, research and return 
the following:

1. Adoption Stage: Where is the Western market in terms of awareness and 
   adoption of this topic or product? Early, growing, or mature?

2. Audience Profile: Who is the primary audience engaging with this in 
   Western markets? Include demographics, job roles, or psychographics 
   where relevant.

3. Behavioural Patterns: How does the Western audience typically engage 
   with, purchase, or respond to this? Note platform preferences, trust 
   signals, and decision-making patterns.

4. Cultural & Economic Context: What cultural values, regulatory factors, 
   or economic conditions shape how this topic lands in Western markets?

5. Language & Messaging Tone: What tone, language style, and framing 
   works best for Western audiences on this topic?

6. Opportunity Signal: What is the biggest untapped opportunity or 
   white space in Western markets related to this topic?

Be specific. Avoid generic statements. Where the US, UK, or Europe differ 
meaningfully, call that out. Return your findings in clean structured format 
under the six headers above.
```

---

## Test Prompts

Use these in the chat trigger to test the workflow end to end.

### AI Tools & SaaS

```
Research the market for AI productivity tools — who is buying, what drives 
adoption, and where the biggest gaps are in SEA versus Western markets.
```

```
I want to sell an AI writing assistant. Compare how SEA and Western audiences 
perceive AI-generated content and what objections each market has.
```

```
Analyse the opportunity for selling an n8n automation consulting service 
in Southeast Asia versus the US and Europe.
```

---

### Content & Creator Economy

```
Research the LinkedIn creator economy in SEA versus Western countries. 
Who is building personal brands, what content performs, and what 
monetisation models exist?
```

```
I want to launch a cohort-based online course on AI tools. Compare learning 
culture, price sensitivity, and platform preferences between SEA and 
Western learners.
```

```
Analyse how short-form video content is consumed differently in Southeast 
Asia versus Western markets.
```

---

### B2B & Enterprise

```
Research enterprise AI training adoption in Southeast Asia versus Western 
Europe. Who makes the decision, what triggers a purchase, and how do 
procurement cycles differ?
```

```
I want to position a Microsoft Copilot training programme. Compare how 
corporate L&D teams in India and Indonesia think about AI upskilling 
versus those in the UK and Germany.
```

```
Analyse B2B SaaS buying behaviour in SEA versus the US — specifically 
around trust, trial willingness, and vendor selection criteria.
```

---

### E-commerce & Consumer Products

```
Research the market for sustainable fashion in Southeast Asia compared to 
Western Europe. Who is buying, what drives the decision, and what 
barriers exist?
```

```
Analyse how health supplement brands should position differently for SEA 
versus US audiences — including platform, messaging, and influencer strategy.
```

```
I am launching a fintech app for freelancers. Compare the financial 
behaviour, trust in digital payments, and feature priorities of freelancers 
in SEA versus Western markets.
```

---

## Implementation Notes

### Tool Naming
The tool name and description fields in n8n are what the orchestrator uses to decide which tool to call. Set them exactly as specified in each sub-agent section above. Vague names cause the orchestrator to misroute or skip tools.

### Gemini Free Tier Tips
- Use a separate Gemini API credential instance for each of the three agent nodes
- If you hit rate limits mid-run, add a **Wait** node (2–3 seconds) between the orchestrator and sub-agent calls
- Gemini 1.5 Flash has a higher free-tier RPM than Gemini 1.5 Pro — prefer Flash for classroom use

### Extending This Workflow
- Add a **Google Docs** output node to auto-save each report
- Add a **third sub-agent** for Middle East or Latin America to expand the comparison
- Replace the **Chat Trigger** with a Webhook to allow external apps to trigger research
- Add a **Telegram or WhatsApp** output node to deliver reports to a group channel

---

*Data Science Masterminds · n8n Workflow Documentation*
*Built for cohort use. Do not redistribute without permission.*