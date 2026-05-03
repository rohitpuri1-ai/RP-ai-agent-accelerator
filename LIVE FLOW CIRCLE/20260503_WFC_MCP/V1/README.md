# 🔍 Web Research AI Agent — n8n + Exa MCP

A conversational AI agent built in n8n that uses **Exa's MCP server** to perform live web search and page fetching. Demonstrates external MCP integration with an n8n AI Agent node using HTTP Streamable transport.

---

## 📌 What This Workflow Does

- Accepts a user message via the **Chat Trigger**
- Routes it to an **AI Agent** (Anthropic Claude) with memory
- The agent autonomously decides when to call Exa's MCP tools:
  - `web_search_exa` — searches the live web for any query
  - `web_fetch_exa` — fetches and reads a full webpage as markdown
- Returns a structured, cited response back to the user

---

## 🧱 Workflow Architecture

```
[Chat Trigger]
      │
      ▼
[AI Agent]
  ├── Model     → Anthropic Chat Model (Claude)
  ├── Memory    → Simple Memory (conversation context)
  └── Tool      → MCP Client (Exa MCP Server)
                    ├── web_search_exa
                    └── web_fetch_exa
```

---

## 🛠️ Tech Stack

| Component         | Tool / Service              |
|-------------------|-----------------------------|
| Workflow Engine   | n8n                         |
| AI Model          | Anthropic Claude (via n8n)  |
| MCP Server        | Exa (`https://mcp.exa.ai/mcp`) |
| Transport         | HTTP Streamable             |
| Auth              | Header Auth (`x-api-key`)   |
| Memory            | Simple Memory (in-session)  |

---

## ⚙️ Setup Instructions

### 1. Prerequisites

- n8n instance (cloud or self-hosted)
- Anthropic API key
- Exa API key → [Get it here](https://dashboard.exa.ai/api-keys)

> **Free tier note:** Exa offers a free plan. Students can test without a paid account but will hit rate limits under heavy usage.

---

### 2. Node Configuration

#### Chat Trigger
- Default settings — no changes needed

#### Anthropic Chat Model
- Connect your Anthropic API credential
- Recommended model: `claude-3-5-sonnet` or `claude-3-haiku` (faster for demos)

#### Simple Memory
- Default settings — stores conversation history within a session

#### MCP Client (Exa)

| Parameter          | Value                        |
|--------------------|------------------------------|
| Endpoint           | `https://mcp.exa.ai/mcp`    |
| Server Transport   | HTTP Streamable              |
| Authentication     | Header Auth                  |
| Tools to Include   | All                          |

**Header Auth Credential setup:**

| Field         | Value               |
|---------------|---------------------|
| Header Name   | `x-api-key`         |
| Header Value  | `your_exa_api_key`  |

---

### 3. AI Agent — System Message

Paste this into the AI Agent node's **System Message** field:

```
You are a research assistant with access to Exa's web search tools.

When the user asks a question:
- Use web_search_exa for finding current information, news, or general research
- Use web_fetch_exa when the user provides a specific URL and wants its content
- Always cite where the information came from
- Keep responses concise and structured — use bullet points for lists, bold for key takeaways
- If a search returns irrelevant results, refine and search again before responding

Do not make up information. If you cannot find something, say so clearly.
```

---

## 🧪 Test Prompts

Use these to verify the workflow after setup.

### 🟢 Basic — Confirm tool is working
```
What are the top 3 AI tools that launched this week?
```
```
What is n8n used for? Search the web and explain it simply.
```

### 🟡 Intermediate — Agent reasoning + tool selection
```
Search for "best free MCP servers for AI agents" and list them with their use cases.
```
```
Fetch the content of https://n8n.io and summarize what the product does in 5 bullet points.
```

### 🔴 Advanced — Real-world simulation
```
I'm preparing a training session on AI automation for corporate teams. Search for 3 recent case studies of companies using AI agents and summarize each one.
```
```
Search for "n8n vs Zapier 2025" and give me a comparison table with pros and cons of each.
```
```
Find the Exa documentation page for web search, fetch its content, and explain the difference between neural search and keyword search based on what you find.
```

---

## 🔑 Key Concepts Demonstrated

| Concept | Where it shows up |
|---|---|
| External MCP over HTTP | MCP Client node using SSE/Streamable HTTP (not stdio) |
| Agent tool autonomy | Agent decides when to call `web_search_exa` vs `web_fetch_exa` |
| Multi-tool chaining | Advanced prompts trigger both tools in a single agent run |
| Conversation memory | Simple Memory node retains context across turns |

---

## 📂 Available Exa MCP Tools

| Tool | Description |
|---|---|
| `web_search_exa` | Search the live web for any topic |
| `web_fetch_exa` | Read full page content from a URL as clean markdown |
| `web_search_advanced_exa` | Advanced search with filters, date ranges, domain restrictions *(optional)* |

To enable advanced search, update the endpoint:
```
https://mcp.exa.ai/mcp?tools=web_search_exa,web_fetch_exa,web_search_advanced_exa
```

---

## 🚨 Troubleshooting

| Issue | Fix |
|---|---|
| Tools not appearing after Execute Step | Check that Header Auth credential is correctly saved with `x-api-key` |
| 429 Rate Limit error | Add your Exa API key — free plan has low limits |
| Agent not using MCP tools | Ensure the MCP Client is connected to the **Tool** port (not the main input) |
| Empty search results | Try rephrasing the query — Exa uses neural search, so natural language works better than keywords |

---

## 📎 Resources

- [Exa MCP Documentation](https://exa.ai/docs/reference/exa-mcp)
- [Exa API Dashboard](https://dashboard.exa.ai/api-keys)
- [n8n MCP Client Node Docs](https://docs.n8n.io)
- [Exa GitHub (open-source MCP server)](https://github.com/exa-labs/exa-mcp-server)

---

*Built as part of the DSM Applied AI curriculum — n8n + MCP Integration Module.*