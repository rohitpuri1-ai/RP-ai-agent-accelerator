## Section 2. Essential Triggers

Triggers define **when** an AI agent should run.

Without triggers, workflows do nothing.  
This section introduces the most common ways AI agents are activated in real systems.

The focus here is **control and timing**, not intelligence.

---

### What This Section Teaches

- How workflows start execution
- The difference between manual, conversational, and automated triggers
- When to use each trigger type
- How trigger choice affects agent behavior

---

## Workflow 201. Manual Trigger (Click to Execute)

### What This Workflow Does

This workflow shows how to start a workflow **manually** using the Execute Workflow / Manual Trigger.

It is primarily used for testing and controlled execution.

---

### Node Sequence Explanation

1. **When Clicking “Execute Workflow”**  
   Manually starts the workflow from the editor.

2. **Gmail (Send Message)**  
   Sends a predefined message once the workflow is executed.

---

### Key Concept

Manual triggers are best for:
- Testing workflows
- Debugging logic
- One-time executions
- Learning how data flows without automation

---

## Workflow 202. Chat Trigger (Conversational Start)

### What This Workflow Does

This workflow starts when a user sends a **chat message**.  
It introduces conversational entry points for AI agents.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when a user sends a message.

2. **Downstream Nodes**  
   Process the message or route it to an AI agent or tool.

---

### Key Concept

Chat triggers enable:
- Interactive AI agents
- Copilots and assistants
- Human-in-the-loop workflows

---

## Workflow 203. Schedule Trigger (Time-Based Execution)

### What This Workflow Does

This workflow runs automatically at a **scheduled time** without user input.

It demonstrates background automation using AI.

---

### Node Sequence Explanation

1. **Schedule Trigger**  
   Starts the workflow at a defined time or interval.

2. **AI Agent**  
   Acts as a Crypto Analyst and processes the task.

3. **Gemini Search Tool**  
   Fetches current market data.

4. **Gmail (Send Message)**  
   Emails the generated HTML report.

---

### System Prompt

```
You are a Crypto Analyst. Use the Google Search Tool to extract the information.
Generate the report in HTML format for emailing.
```

---

### Key Concept

Schedule triggers are used for:
- Reports
- Monitoring
- Alerts
- Fully automated agents

---

## Workflow 204. Form Trigger (On Form Submission)

### What This Workflow Does

This workflow starts when a **form is submitted**.

It shows how user input from forms can initiate automation.

---

### Node Sequence Explanation

1. **On Form Submission**  
   Triggers the workflow when a form is submitted.

2. **Google Sheets (Append Row)**  
   Stores the submitted data in a spreadsheet.

---

### Key Concept

Form triggers are useful for:
- Lead capture
- Data collection
- User driven workflows
- Structured input pipelines
