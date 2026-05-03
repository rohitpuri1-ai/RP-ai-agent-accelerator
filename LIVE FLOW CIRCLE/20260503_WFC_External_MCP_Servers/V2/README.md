# 📬 Agent KAI — Research + Email Workflow | n8n + Exa MCP + Gmail


An AI agent that researches any topic using **Exa MCP**, generates structured content ideas, and automatically sends the output via **Gmail** — all from a single chat message. No manual steps. No copy-pasting.

---

## 🧠 What is Exa? (Simple Explanation)

Think of **Google** as a search engine built for humans — it shows you links and snippets so *you* can read and decide.

**Exa** is a search engine built for **AI agents** — it returns clean, structured content that an AI can directly read, understand, and use. No ads. No noise. Just the actual information.

| | Google | Exa |
|---|---|---|
| Built for | Humans | AI agents |
| Returns | Links + snippets | Full readable content |
| How it searches | Keyword matching | Meaning-based (neural search) |
| Best used by | You, in a browser | Your AI agent, inside a workflow |

### When should your agent use Exa?

| Situation | Use Exa? |
|---|---|
| Agent needs to find current news or recent events | ✅ Yes |
| Agent needs to read the full content of a webpage | ✅ Yes |
| Agent needs to research a topic before generating output | ✅ Yes |
| Agent already has all the information in its context | ❌ No |
| You need real-time stock prices or live database queries | ❌ No — use a dedicated API |

> **One-line rule for individuals:** If your agent needs to *know something it wasn't trained on*, give it Exa.

---

## 📌 What This Workflow Does

1. User sends a topic or URL via the **Chat Trigger**
2. **Agent KAI** researches it using Exa's live web search
3. Generates a structured output:
   - Research Summary
   - 10 Tweet Topic Ideas
   - 10 LinkedIn Post Topic Ideas
   - Top Content Idea
4. Formats everything as an **HTML email**
5. Sends the email via the **Gmail tool** — automatically
6. Confirms with: `Email sent. — Agent KAI`

---

## 🧱 Workflow Architecture

```
[Chat Trigger]
      │
      ▼
[AI Agent — Agent KAI]
  ├── Model     → OpenRouter Chat Model
  ├── Memory    → Simple Memory (conversation context)
  └── Tools
        ├── MCP Client (Exa)
        │     ├── web_search_exa
        │     └── web_fetch_exa
        └── Send a message in Gmail
```

---

## 🛠️ Tech Stack

| Component         | Tool / Service                      |
|-------------------|-------------------------------------|
| Workflow Engine   | n8n                                 |
| AI Model          | OpenRouter (any model via API)      |
| MCP Server        | Exa (`https://mcp.exa.ai/mcp`)     |
| Transport         | HTTP Streamable                     |
| MCP Auth          | Header Auth (`x-api-key`)           |
| Email Tool        | Gmail (n8n native tool node)        |
| Memory            | Simple Memory (in-session)          |

---

## ⚙️ Setup Instructions

### 1. Prerequisites

- n8n instance (cloud or self-hosted)
- OpenRouter API key → <a href="https://openrouter.ai" target="_blank">openrouter.ai</a>
- Exa API key → <a href="https://dashboard.exa.ai/api-keys" target="_blank">dashboard.exa.ai/api-keys</a>
- Gmail account connected to n8n via OAuth2

---

### 2. Node Configuration

#### Chat Trigger
- Default settings — no changes needed

#### OpenRouter Chat Model
- Add your OpenRouter API credential
- Recommended model: `anthropic/claude-3.5-sonnet` or `google/gemini-flash-1.5`

#### Simple Memory
- Default settings

#### MCP Client (Exa)

| Parameter          | Value                        |
|--------------------|------------------------------|
| Endpoint           | `https://mcp.exa.ai/mcp`    |
| Server Transport   | HTTP Streamable              |
| Authentication     | Header Auth                  |
| Tools to Include   | All                          |

**Header Auth Credential:**

| Field         | Value               |
|---------------|---------------------|
| Header Name   | `x-api-key`         |
| Header Value  | `your_exa_api_key`  |

#### Send a message in Gmail
- Connect via Gmail OAuth2 credential
- No additional configuration needed — the agent controls all parameters (recipient, subject, body) at runtime

---

### 3. AI Agent — System Message

```
You are Agent KAI, a research assistant with access to Exa tools and Gmail send message tool.
Your job is to research the user's topic, create content ideas, and send the final result by email.

IMPORTANT EXECUTION RULES:
1. You must use web_search_exa for research.
2. If the user gives a URL, use web_fetch_exa.
3. You must use the Gmail send message tool to send the email.
4. Never say "Email sent" unless the Gmail send message tool has successfully executed.
5. If the Gmail tool fails, say: "Email was not sent. Reason: [tool error]"
6. Do not pretend that an email was sent.
7. Do not only generate an email draft in chat.
8. The final chat response must only happen after the Gmail tool runs.

Workflow:
Step 1: Research the topic using Exa.
Step 2: Generate:
- Research Summary
- 10 Tweet Topic Ideas
- 10 LinkedIn Post Topic Ideas
- Top Content Idea

Step 3: Create an HTML email body.
Step 4: Send the email using Gmail send message tool.
Step 5: After successful sending, reply only:
Email sent.
- Agent KAI

Email must include:
- Subject line
- HTML body
- Source links
- Clear next step
- Sign off as Agent KAI

Tone:
Professional, clear, useful, and insight-driven.
```

---

## 🧪 Test Prompts

### 🟢 Basic — Single topic research + send
```
Research "AI agents in 2025" and send the output to myemail@gmail.com
```

### 🟡 Intermediate — URL-based research
```
Fetch https://n8n.io and create content ideas based on what the product does. Send to myemail@gmail.com
```

### 🔴 Advanced — Industry-specific content pipeline
```
Research "how Indian retail companies are using AI automation" and generate content ideas for LinkedIn and Twitter. Send the full report to myemail@gmail.com
```

```
Research "n8n vs Make vs Zapier 2025" — create a comparison summary and 10 LinkedIn post ideas around it. Send to myemail@gmail.com
```

---

## 🔑 Key Concepts Demonstrated

| Concept | Where it shows up |
|---|---|
| External MCP over HTTP | Exa MCP via HTTP Streamable — not stdio |
| Native n8n tool inside agent | Gmail tool runs as an agent action, not a separate workflow node |
| Agent execution order enforcement | System message rules prevent hallucinated "Email sent" responses |
| Multi-tool orchestration | Agent chains: search → generate → format → send |
| Guardrail prompting | Rules 4–8 prevent the agent from skipping tool calls |

---

## 📂 Agent KAI Output Structure

Every email sent by Agent KAI includes:

```
Subject: [Topic] — Research Report by Agent KAI

[Research Summary]

[10 Tweet Topic Ideas]

[10 LinkedIn Post Topic Ideas]

[Top Content Idea]

Sources:
- [link 1]
- [link 2]

Next Step: [suggested action]

— Agent KAI
```

---

## ⚠️ Critical Prompting Notes

**Why the execution rules matter:**

LLMs will hallucinate tool execution if not constrained. Without rules 4–8, the agent may:
- Generate the email draft in chat and claim it was sent
- Skip the Gmail tool entirely
- Respond before the tool finishes

The system message forces the agent to treat the Gmail tool call as a hard dependency before producing any final output. This is a pattern individuals should replicate in every agentic workflow where a side-effect (email, API call, database write) must be confirmed before the agent responds.

---

## 🚨 Troubleshooting

| Issue | Fix |
|---|---|
| Agent says "Email sent" without sending | Check Gmail OAuth credential is active and has Send permission |
| Gmail tool not appearing in agent | Ensure it is connected to the **Tool** port of the AI Agent node |
| Exa tools not loading | Verify `x-api-key` header credential is saved correctly |
| Agent only generates draft in chat | System message rules 6–8 are missing or overridden — repaste the full system message |
| 429 from Exa | Add your Exa API key — free plan rate limit hit |
| OpenRouter model not responding | Check API key and confirm selected model is active on OpenRouter |

---

## 🔄 v1 vs v2 Comparison

| Feature | v1 (Exa Only) | v2 (Agent KAI) |
|---|---|---|
| Web Search | ✅ | ✅ |
| Page Fetching | ✅ | ✅ |
| Content Generation | ❌ | ✅ (tweets + LinkedIn + summary) |
| Email Delivery | ❌ | ✅ (via Gmail tool) |
| Execution Guardrails | ❌ | ✅ (rules 4–8) |
| Agent Identity | Generic | Agent KAI |
| Model | Anthropic (direct) | OpenRouter (model-agnostic) |

---

## 📎 Resources

- <a href="https://exa.ai/docs/reference/exa-mcp" target="_blank">Exa MCP Documentation</a>
- <a href="https://dashboard.exa.ai/api-keys" target="_blank">Exa API Dashboard</a>
- <a href="https://openrouter.ai" target="_blank">OpenRouter</a>
- <a href="https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.gmail/" target="_blank">n8n Gmail Tool Docs</a>
- <a href="https://github.com/exa-labs/exa-mcp-server" target="_blank">Exa GitHub</a>

---

*Built as part of the DSM Applied AI curriculum — n8n + MCP Integration Module | v2: Multi-Tool Agent.*