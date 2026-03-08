# WHR - Prompt

## Prompt 1
```
## INPUTS 
(paste your filled answers below)

You are an expert AI Solutions Architect and API Integration Specialist.

I will give you 9 inputs in a W.H.R. (What–How–Rules) framework. Your job is to convert these inputs into a practical, step-by-step implementation plan to build this AI Agent using APIs (no-code or low-code friendly), and output everything in a clean, structured format that I can paste into Claude.



---

## YOUR TASK

Using ONLY the above inputs, produce a step-by-step “Build Plan” for an API-based AI Agent. Make it simple enough for a beginner but detailed enough to implement.

## OUTPUT FORMAT (follow exactly)

### 1) Agent Summary (1 paragraph)

* Summarize the agent in plain English using the WHAT inputs.

### 2) Agent Requirements (bullet list)

* List functional requirements (what it must do)
* List non-functional requirements (speed, reliability, privacy, safety)

### 3) System Architecture (simple diagram in text)

Use this format:
User → (Channel/UI) → (Backend/Orchestrator) → (LLM) → (Tools/APIs) → Output → User

### 4) Step-by-Step Build Plan (10–18 steps)

Each step must include:

* Step name
* What you build/do
* What input/output looks like
* What could go wrong + how to handle it

### 5) APIs Required (with purpose + example fields)

Provide a table with columns:

* API Category
* Specific API Options (2–4 options)
* Why needed
* Inputs
* Outputs

Include (where relevant):

* LLM API (OpenAI / Anthropic / etc.)
* Messaging/Channel API (Telegram/WhatsApp/Web chat/Email)
* Data storage (Airtable/Google Sheets/Postgres/Supabase)
* Auth (API keys, OAuth if needed)
* Retrieval (search/vector DB) ONLY if needed
* Observability/logging (basic logging)

### 6) Prompt Pack (ready-to-use)

Create 3 prompts:
A) System Prompt (role + rules + tone + limits)
B) Developer Prompt (workflow + format + constraints)
C) User Prompt Template (what the user will type)

### 7) Safety & Control Checks (must include)

* Guardrails based on Limits
* Escalation rules based on Decision Owner
* “Uncertainty behavior” (what agent does when it lacks info)
* Data privacy notes (what not to store)

### 8) Test Plan (minimum 8 test cases)

Make a table:

* Test Case
* Input
* Expected Output
* Edge Condition

### 9) MVP vs V2 Enhancements

* MVP: what to build in the first version
* V2: what to add later (memory, RAG, dashboards, analytics, etc.)

## IMPORTANT RULES FOR YOU

* Do not assume tools I didn’t mention unless necessary; if you must assume, clearly label it as an assumption.
* Keep it implementation-oriented, not theory.
* Use clear headings, lists, and tables.
* No fluff.

```

## Prompt 2
```
You are a world-class positioning and offer-creation expert. 
Using the W.H.R. (What–How–Rules) inputs provided earlier, create a compelling one-paragraph offer tailored specifically 
to the target audience identified. The offer should clearly state who this is for, the painful problem it removes, 
the tangible outcome they will get, and why this AI Agent–based solution is different from generic AI or ChatGPT usage. 
Keep it benefit-driven, simple, non-technical, and focused on speed, clarity, and real-world impact. Avoid jargon. 
Write it in a confident, credible tone that makes the reader feel, “This is exactly for me.” 
The output should be a single paragraph suitable for a landing page, sales slide, or webinar pitch.
```