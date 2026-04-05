# n8n MCP Server Workflow
## AI-Powered CRM Agent — CRUD on Google Sheets

> **Type a message. Let the AI Agent create, read, update, or delete your CRM data.**

![Platform](https://img.shields.io/badge/Platform-n8n-orange) ![Trigger](https://img.shields.io/badge/Trigger-Chat%20Message-blue) ![Model](https://img.shields.io/badge/Model-Gemini%20Free-green) ![Pattern](https://img.shields.io/badge/Pattern-MCP%20Server-purple)

---

## Overview

This system uses **two separate n8n workflows** that work together. The first workflow is the user-facing AI Agent. The second workflow is the MCP Server that executes Google Sheets operations. The AI Agent decides which CRUD operation to run based on natural language — the user never needs to know which tool is being called.

**CRM Sheet Columns:** `Customer Name · Phone · Email · Location · Lead Status`

---

## Architecture

```
WORKFLOW 1 — AGENT SIDE
────────────────────────────────────────────
Chat Trigger → AI Agent (Gemini + Memory + MCP Client)

WORKFLOW 2 — MCP SERVER SIDE
────────────────────────────────────────────
MCP Server Trigger
    ├── Append Row in Google Sheets     (CREATE)
    ├── Get Row(s) in Google Sheets     (READ)
    ├── Update Row in Google Sheets     (UPDATE)
    └── Delete Rows/Columns from Sheet  (DELETE)
```

Workflow 1 uses MCP Client as a tool node attached to the AI Agent.  
Workflow 2 is triggered automatically when the AI Agent calls a tool.

---

## Node Configuration

### Workflow 1 — Agent Side

| Node | Type | Notes |
|------|------|-------|
| When chat message received | n8n Chat Trigger | Entry point |
| AI Agent | AI Agent | Orchestrates all tool calls |
| Google Gemini Chat Model | Chat Model | Attach to AI Agent |
| Simple Memory | Memory | Attach to AI Agent |
| MCP Client | Tool | Attach to AI Agent — connects to Workflow 2 |

### Workflow 2 — MCP Server Side

| Node | Type | Operation |
|------|------|-----------|
| MCP Server Trigger | MCP Server Trigger | Entry point for tool calls |
| Append row in sheet | Google Sheets | CREATE |
| Get row(s) in sheet | Google Sheets | READ |
| Update row in sheet | Google Sheets | UPDATE |
| Delete rows or columns | Google Sheets | DELETE |

---

## System Prompt

Paste this into the **System Prompt** field of the AI Agent node.

```
You are a CRM Analyst.
You have access to MCP Server which has 4 tools.

1/ Leads Database
When the user requests any information about leads, use this tool to read 
the sheet and provide insights.

There are four tools:
1. Create or append rows
2. Get rows
3. Update rows
4. Delete rows

For the update and delete rows option, always make sure you do the get rows 
first to understand which row to update or delete.

In the update row option, the unique identifier that we have is the email 
itself. So you can ask the user to give the email ID whenever they ask to 
update any row. Ask them for the email ID so that you can identify which 
row to update.
```

---

## Test Prompts

### CREATE — Add a new contact

```
Add a new contact: Name: Kunaal Naik, Phone: 9999999999, Email: kunaal@me.com, Location: BLR, Lead Status: Hot
```

```
Add a new lead — James Smith, phone 555-1212, email james.smith@email.com, based in New Jersey, status Hot
```

---

### READ — Query the CRM

```
Show me all contacts with a Hot lead status
```

```
Find the contact with email daniel.harris@email.com
```

```
Show me all contacts located in New York
```

```
Give me a list of all Cold leads
```

---

### UPDATE — Modify an existing record

```
Update the lead status for james.wilson@email.com to Hot
```

```
Change the location for sarah.johnson@email.com to Chicago
```

> The agent will ask for the email ID to confirm which record to update before making any change.

---

### DELETE — Remove a contact

```
Delete the record for john.doe@email.com
```

```
Remove the contact with email kun@me.com from the CRM
```

> ⚠️ Delete is irreversible in Google Sheets. For production use, add a confirmation step to the system prompt (see Implementation Notes below).

---

## Implementation Notes

### Update & Delete — Why Email Is the Unique Identifier

Names can have duplicates in a CRM. Email is always unique. The system prompt instructs the agent to ask for the email before any update or delete operation. This prevents accidental edits to the wrong row.

The agent flow for UPDATE/DELETE:
1. Agent asks user for the email of the contact to modify
2. Agent calls **Get Rows** first to confirm the record exists
3. Agent calls **Update Row** or **Delete Row** on the confirmed record

### Production Additions (Optional)

Add these lines to the system prompt to make the agent production-safe:

```
Before calling the delete tool, always ask the user:
"Are you sure you want to permanently delete [Name]? This cannot be undone."
Only proceed after the user confirms.

Before calling the create tool, always call get rows first to check if a 
contact with the same email already exists. If a duplicate is found, inform 
the user and ask whether to proceed.
```

### Extending This Workflow

- Swap Google Sheets for **Airtable, Notion, or a SQL database** via MCP — no agent changes required
- Add a **fifth tool** for bulk export or reporting
- Connect the MCP Client to a **remote n8n instance** to separate agent and data infrastructure

---

*Data Science Masterminds · AI Agent Accelerator Program*  
*Built for cohort use. Do not redistribute without permission.*