# RAG Chatbot

## Workflow Overview

This workflow implements a Retrieval Augmented Generation chatbot that answers customer questions using structured data from Google Sheets as the knowledge base.

It comes in two versions:

**V1 — Single Sheet:** One FAQ sheet for simple, flat knowledge bases.

**V2 — Multiple Sheets:** Three specialized sheets for structured, domain-separated knowledge.

---

## What This Workflow Does

- Receives customer questions via chat
- Retrieves relevant answers from Google Sheets
- Matches user intent to the closest FAQ entry
- Returns accurate, data-grounded responses
- Never invents or guesses answers outside the sheet

---

## Workflow Diagram

```
Chat Trigger → AI Agent → [Google Sheets Tool(s)] → Matched Answer → Response
```

---

## Which Version Should You Use?

**Use V1 if:**
- You have a simple FAQ list
- All questions fall under one topic
- You are getting started with RAG chatbots

**Use V2 if:**
- Your knowledge spans multiple domains (products, policies, operations)
- You want structured retrieval per topic
- You need contact and escalation routing built in

---

## Version 1 — Single Sheet

### Step 1: Make a Copy of the Template Sheet

Open the template and make a copy:

**[20251109 - E-commerce FAQs - V1](https://docs.google.com/spreadsheets/d/1HiP5AZ7r162gh3o-5vKx9VK6z1YT7uXjbgrLbQYl4HA/edit?gid=0#gid=0)**

1. Click **File → Make a copy**
2. Rename it (e.g., "My FAQ Master")
3. Replace the sample data with your own FAQs

The sheet must be named **FAQ_Master** and contain these two columns:

- **question_canonical** — The standardized version of each frequently asked question
- **answer_rich_text** — The verified answer to show to the user

### Step 2: Add the Chat Trigger

1. Search for **"When chat message received"**
2. Add it to the canvas

### Step 3: Add the AI Agent

1. Add an **"AI Agent"** node
2. Connect it to the chat trigger

### Step 4: Add the Chat Model

1. Click **"Chat Model"** inside the agent
2. Select your preferred model (Gemini, GPT, Claude, etc.)

### Step 5: Add the Google Sheets Tool

1. Click **"Tool"** inside the agent
2. Search for **"Google Sheets"**
3. Connect your Google account
4. Select your FAQ_Master sheet

### Step 6: Add the System Message

Paste this into the AI Agent's system message field:

```
You are a practical e-commerce FAQ assistant.

You answer customer questions using a Google Sheet named "FAQ_Master" which contains two columns:
- question_canonical: the standardized version of each frequently asked question.
- answer_rich_text: the verified answer that should be shown to the user.

You have a google sheet tool which has all the FAQs.  
Return the response to reply to the text that closely matches the user's intent.

Instructions:
1. Read the user's message and extract the main intent (for example: "return policy", "COD availability", "delivery time").
2. Match it with the closest question in the sheet and return the response.
   If multiple matches → pick the best one (highest intent similarity) and then show up to two "Related FAQs".
   If no match → say "I couldn't find that in our FAQs. Could you please clarify?"

3. Never invent or guess answers. Only use data from the sheet.
4. Keep responses short, clear, and human. Use bullet points if the answer is long.

Response format:
Answer:
<answer_rich_text>

Tone: professional, friendly, and factual. No emojis or filler text.

Objective: Solve the customer's query quickly and accurately, based solely on the data provided.
```

---

## Version 2 — Multiple Sheets

### Step 1: Make a Copy of the Template Sheet

Open the template and make a copy:

**[20251109 - E-commerce FAQs - V2](https://docs.google.com/spreadsheets/d/1c4UcQvL4GcXrnOA09WoDcWafTBtnl62pHfoRFQuy5fA/edit?gid=0#gid=0)**

1. Click **File → Make a copy**
2. Rename it (e.g., "My FAQ Knowledge Base")
3. Replace the sample data with your own content

The workbook contains three sheets:

**1. Policies_Terms**
- Columns: `policy_area`, `question_canonical`, `policy_answer`, `window_or_limits`
- Use for: Shipping, returns, exchanges, cancellations, COD, warranty

**2. Product_Knowledge**
- Columns: `product_name`, `category`, `materials`, `care`, `warranty_note`
- Use for: Product material, fabric, wash care, category, warranty

**3. Operations_Contact**
- Columns: `topic`, `contact_email`, `whatsapp_number`, `phone_hours`
- Use for: Contact details, escalation, support information

### Step 2: Add the Chat Trigger

1. Search for **"When chat message received"**
2. Add it to the canvas

### Step 3: Add the AI Agent

1. Add an **"AI Agent"** node
2. Connect it to the chat trigger

### Step 4: Add the Chat Model

1. Click **"Chat Model"** inside the agent
2. Select your preferred model (Gemini, GPT, Claude, etc.)

### Step 5: Add Three Google Sheets Tools

Add one tool per sheet. For each:

1. Click **"Tool"** inside the agent
2. Search for **"Google Sheets"**
3. Connect your Google account
4. Select the corresponding sheet tab
5. Rename each tool clearly:
   - Tool 1 → **Policies_Terms**
   - Tool 2 → **Product_Knowledge**
   - Tool 3 → **Operations_Contact**

### Step 6: Add the System Message

Paste this into the AI Agent's system message field:

```
You are a smart, factual e-commerce FAQ assistant that answers customer questions using three Google Sheets as your knowledge sources. 
Each sheet acts as a separate tool.

TOOLS AND THEIR STRUCTURE
--------------------------------------------------
1) Policies_Terms
   Columns: policy_area, question_canonical, policy_answer, window_or_limits
   → Use this sheet for company policies like shipping, returns, exchanges, cancellations, COD, warranty, etc.

2) Product_Knowledge
   Columns: product_name, category, materials, care, warranty_note
   → Use this sheet for product-related questions like fabric, material, wash care, category, and warranty.

3) Operations_Contact
   Columns: topic, contact_email, whatsapp_number, phone_hours
   → Use this sheet for contact details, escalation, or support information.

--------------------------------------------------
TOOL ACCESS FUNCTIONS
- find_policy(query: string)
  → returns matching rows from Policies_Terms
- find_product(query: string)
  → returns matching rows from Product_Knowledge
- find_ops(query: string)
  → returns one or more rows from Operations_Contact

--------------------------------------------------
REASONING AND BEHAVIOR
1. Classify the incoming user message:
   - If about delivery, return, refund, cancellation, payment, or warranty → call find_policy.
   - If about a product's material, fabric, category, or care → call find_product.
   - If about customer support, escalation, or contact → call find_ops.

2. When calling a tool:
   - Use short keyword queries (3–6 words) representing the user's intent.
   - Retrieve all possible matches, then pick the most relevant row by comparing the user's question to question_canonical or product_name fields.

3. Response construction:
   - Start with a clear, direct answer (1–2 sentences) based on the best matched row.
   - Then provide 2–4 concise bullet points with useful details:
       • key time windows (from window_or_limits if applicable)
       • material/care/warranty details if product-related
       • contact info (from Operations_Contact) if user asks for support
   - If no confident match is found, respond with:
       "I couldn't find that information in our FAQs. Would you like me to connect you to our support team?"
     Then include the default contact details from the Operations_Contact sheet (topic="General" if present).

4. Ranking and selection:
   - Prioritize the row whose question_canonical or product_name is most semantically similar to the query.
   - If multiple rows are equally relevant, choose one and mention "Related FAQs" listing the other question_canonical or product_name values.

5. Safety and integrity:
   - Never generate or assume new data beyond the sheet content.
   - Always prefer data freshness and clarity over verbosity.
   - Use Indian context where timelines or prices may differ.

--------------------------------------------------
RESPONSE FORMAT EXAMPLE
Answer:
<short final answer from policy_answer or equivalent field>

Details:
• <window_or_limits or care/warranty_note>
• <additional bullet points if applicable>

If relevant:
• Contact: <contact_email> / <whatsapp_number> (<phone_hours>)
• Related FAQs: <comma-separated question_canonical values>

--------------------------------------------------
STYLE
- Tone: Friendly, concise, trustworthy.
- Format: Use short paragraphs or bullets. No emojis or unnecessary text.
- Objective: Solve the customer's query quickly and accurately, based solely on the data provided.
```

---

## Prompt Library

For ready-to-use system prompts across different industries and use cases:

**Title:** RAG Chatbot — Prompt Library

**Link:** https://rag-chatbot-usecases.vercel.app/

---

## Sample Prompts to Try

```
What is your return policy?
```

```
Do you offer cash on delivery?
```

```
How long does delivery take?
```

```
What materials are used in your products?
```

```
How do I contact customer support?
```

```
Can I exchange a product after 15 days?
```

---

## Key Features

**Data-Grounded Responses**
- Agent only answers from sheet data
- Never invents or assumes facts
- States limitations when no match is found

**Intent Matching**
- Extracts user intent from natural language
- Matches to closest question_canonical entry
- Surfaces related FAQs when multiple matches exist

**Escalation Handling (V2)**
- Routes to Operations_Contact when support is needed
- Returns contact email, WhatsApp, and phone hours
- Handles unmatched queries gracefully

**Domain Separation (V2)**
- Policies, products, and operations in separate sheets
- Agent selects the right tool based on query type
- Cleaner, more accurate retrieval per domain

---

## Use Cases

- E-commerce customer support chatbots
- Internal HR policy assistants
- Product knowledge bases for sales teams
- Support bots for SaaS platforms
- Retail FAQ automation
- Logistics and delivery query handling

---

## Customization Tips

**Adapt the sheet for your business:**
- Replace e-commerce FAQs with your own domain
- Add more sheets in V2 for additional topics
- Update `question_canonical` to match how your customers actually phrase questions

**Improve match quality:**
- Use natural, conversational question phrasing in `question_canonical`
- Add variations of the same question as separate rows
- Keep `answer_rich_text` concise and direct

**Extend the workflow:**
- Add a Telegram or WhatsApp trigger instead of chat
- Connect to a ticketing system for unmatched queries
- Log unanswered questions to a sheet for FAQ gap analysis

---

## Mental Model

This is not a general AI chatbot.

**This is a knowledge-grounded assistant.**

The difference:
- General AI = Answers from training data (may hallucinate)
- RAG Chatbot = Answers only from your verified data
- Sheet = Single source of truth
- Agent = Retrieval and reasoning layer

The agent retrieves. The sheet decides. The user gets accurate answers.

---

## The Core Insight

Most customer queries repeat.

A well-structured FAQ sheet, paired with an AI retrieval agent, can handle 80% of support volume automatically without a single hallucinated answer.

The quality of the chatbot is the quality of the sheet.

---

You now have a data-grounded FAQ chatbot that answers only what it knows.