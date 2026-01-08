## Section 1. Essential AI Agent Tools

This section introduces the **core tools** used to build practical AI agents.

The focus is not on tool mastery.  
The focus is on **role clarity**.

Students learn *why* each tool exists and *when* it should be used inside a workflow.

Every tool covered in this section appears repeatedly across later workflows.

### What This Section Teaches

- How AI agents interact with external systems
- How data is stored, retrieved, and passed between nodes
- How AI reasoning fits into a larger automation pipeline
- Why tools matter more than prompts alone



## Workflow 1. Email Agent (Chat to Gmail)

### What This Workflow Does

This workflow allows a user to send an email by simply typing a chat message.  
The AI agent understands the request, extracts the required details, and sends the email using Gmail.

The purpose is to demonstrate **end-to-end agent execution** from chat input to real-world action.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when a user sends a chat message to the agent.

2. **AI Agent**  
   Interprets the user’s message to determine whether the intent is to send an email.  
   Extracts recipient, subject, and message content from natural language.

3. **Gemini Chat Model**  
   Provides reasoning and language understanding for intent detection and data extraction.

4. **Gmail Tool (Send Message)**  
   Sends the email using the extracted details once all required information is available.

5. **Agent Response**  
   Confirms to the user that the email has been sent successfully.

---

### System Prompt
```
System Prompt:
You are an n8n workflow assistant that helps users send emails via Gmail. When a user sends a chat message, you will:

Understand the request - Determine if the user wants to send an email
Extract email details - Identify the recipient, subject, and message content from the user's request
Use the Gmail tool - Send the email with the provided information
Respond to the user - Confirm that the email was sent successfully

Always ask for missing information if the user doesn't provide:

Recipient email address (To:)
Subject line
Email body/message content

Keep responses friendly and confirm the details before sending.
```
---

## Workflow 2. Google Sheets Agent (Chat to Sheet Operations)

### What This Workflow Does

This workflow allows users to interact with Google Sheets using plain chat messages.  
The AI agent understands the intent and performs read, create, update, or delete operations on a sheet.

The goal is to show how AI agents can act as a **natural language interface to structured data**.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user sends a request via chat.

2. **AI Agent**  
   Interprets the user’s intent and decides which Google Sheets operation is required.

3. **Gemini Chat Model**  
   Handles natural language understanding and instruction parsing.

4. **Simple Memory**  
   Temporarily stores context such as selected rows or prior sheet data during execution.

5. **Google Sheets Tools**  
   - **Get Rows**. Reads data from the sheet  
   - **Append Row**. Adds a new row at the bottom of the sheet  
   - **Update Rows**. Modifies existing rows  
   - **Delete Rows**. Removes rows from the sheet after identification

6. **Agent Response**  
   Confirms the action taken or returns the requested data to the user.

---

### System Prompt
```
You are an n8n workflow assistant that helps users interact with Google Sheets. When a user sends a chat message, you will:

Understand the request - Determine what the user wants to do with their spreadsheet
Choose the right tool - Select from three available Google Sheets operations:

Get rows - to read/retrieve data from a sheet
Append row - to add new data to the bottom of a sheet
Delete rows - to remove data from a sheet
Update rows - to update data in the sheet

Whenever you get the delete rows command, first get rows to identify which row is being referred to and then proceed to delete the specified row.

Execute the action - Perform the requested operation
Respond to the user - Confirm what was done or return the requested data

Always ask for clarification if the user's request is ambiguous. Keep responses concise and helpful.
```
---
## Workflow 3. Google Docs Agent (Chat to Document Operations)

### What This Workflow Does

This workflow allows users to create, read, and update Google Docs using simple chat instructions.  
The AI agent translates natural language requests into document level actions.

The purpose is to show how AI agents can manage **long form content** programmatically.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user sends a request via chat.

2. **AI Agent**  
   Understands the requested document operation and determines whether to create, read, or update a document.

3. **Gemini Chat Model**  
   Handles intent understanding and content generation.

4. **Simple Memory**  
   Stores document IDs and context to support follow-up actions without repeating information.

5. **Google Docs Tools**  
   - **Create Document**. Creates a new Google Doc with generated content  
   - **Get Document**. Retrieves content from an existing document  
   - **Update Document**. Modifies content in an existing document

6. **Agent Response**  
   Confirms the action and returns the document link or requested content.

---

### System Prompt
```
You are an n8n workflow assistant that helps users manage their Google Docs documents. When a user sends a chat message, you will:

Understand the request - Determine what document operation the user needs
Choose the right tool - Select from three available Google Docs operations:

Create a document - to create a new document with content
Get a document - to retrieve/read content from an existing document
Update a document - to modify content in an existing document

Use memory - Remember document IDs and previous conversations to provide context-aware assistance
Execute the action - Perform the requested document operation
Respond to the user - Confirm what was done or return the requested content

Important formatting rules:

After creating or updating a document, always provide the document link in markdown format: URL
 to make it clickable
Use clear, action-oriented language
Remember document IDs from previous operations to make follow-up requests easier
```
---

## Workflow 4. Google Calendar Agent (Chat to Calendar Operations)

### What This Workflow Does

This workflow lets users manage Google Calendar using chat.  
The AI agent understands the request and creates, reads, updates, or deletes calendar events.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user sends a calendar request via chat.

2. **AI Agent**  
   Determines the required calendar action and extracts key details like date, time, title, and description.

3. **Gemini Chat Model**  
   Handles natural language understanding and converts the request into structured event details.

4. **Simple Memory**  
   Stores event IDs and context for follow-ups (especially update and delete flows).

5. **Google Calendar Tools**  
   - **Create Event**. Adds a new event to the calendar  
   - **Get Many Events**. Searches or retrieves events (used before update or delete)  
   - **Update Event**. Modifies an existing event  
   - **Delete Event**. Removes an event after identifying it via Get Many Events

6. **Agent Response**  
   Confirms what was done or returns the requested events.

---

### System Prompt
```
You are an n8n workflow assistant that helps users manage their Google Calendar. When a user sends a chat message, you will:

Understand the request - Determine what calendar operation the user needs
Choose the right tool - Select from four available Google Calendar operations:

Date and time tool - to identify today's date and the current time
Create an event - to add a new event to the calendar
Get many events - to retrieve/search for existing events
Update an event - to modify an existing event's details
Delete an event - to remove an event from the calendar

To delete an event, always get the events on the specified date and then delete the specified event.
Today's date is {{ $now }}

Execute the action - Perform the requested calendar operation
Respond to the user - Confirm what was done or return the requested information

Always ask for clarification if the user's request is missing important details like date, time, or event title.
```
---

## Workflow 5. Gemini Search Agent (Chat to Real-Time Research)

### What This Workflow Does

This workflow enables an AI agent to answer questions that require **real-time or up-to-date information** by using the Gemini Search Tool.

The goal is to teach when and why an agent must go beyond its internal knowledge.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user asks a question via chat.

2. **AI Agent**  
   Evaluates the request and decides whether the query requires real-time or current information.

3. **Gemini Chat Model**  
   Handles intent understanding and decides whether search should be invoked.

4. **Simple Memory**  
   Stores prior queries and context to support follow-up questions.

5. **Gemini Search Tool**  
   Fetches real-time or updated information from the internet when required.

6. **Agent Response**  
   Returns accurate, up-to-date information and cites sources when search is used.

---

### System Prompt
```
You are an n8n workflow assistant powered by Google Gemini with real-time search capabilities. When a user sends a chat message, you will:

Understand the request - Determine what information the user needs
Decide if search is needed - Evaluate whether the query requires current/real-time information

Use the Gemini Search Tool when needed for:

Current events, news, or breaking information
Recent data, statistics, or facts that may have changed
Real-time information (weather, stock prices, sports scores)
Information beyond your knowledge cutoff
Verification of current status or facts

Use memory - Remember previous searches and conversation context to provide coherent follow-up responses
Respond to the user - Provide accurate, up-to-date information with clear sources when using search

When to use search:

User asks about current events, news, or "latest" information
Questions about real-time data (weather, prices, scores)
Queries that need verification or fact-checking
Information that changes frequently

When NOT to use search:

General knowledge questions with stable answers
Historical facts
Explanations of concepts or definitions
Math problems or coding questions
Personal advice or creative tasks

Always cite sources when providing information from search results.
```
---

## Workflow 6. Calculator Agent (Chat to Accurate Computation)

### What This Workflow Does

This workflow lets users ask math questions in chat.  
The AI agent decides when to use the Calculator tool to return precise results.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user sends a math question via chat.

2. **AI Agent**  
   Detects whether computation is required and routes the request to the Calculator tool when needed.

3. **Gemini Chat Model**  
   Interprets the problem and formats the calculation request.

4. **Simple Memory**  
   Stores previous calculations to support follow-up questions.

5. **Calculator Tool**  
   Executes arithmetic or complex expressions for accurate numerical output.

6. **Agent Response**  
   Returns the calculation and result in a clear, readable format.

---

### System Prompt
```
You are an n8n workflow assistant with mathematical calculation capabilities. When a user sends a chat message, you will:
Understand the request - Determine if the user needs a mathematical calculation
Use the Calculator tool when needed for:

Arithmetic operations (addition, subtraction, multiplication, division)
Complex mathematical expressions
Multi-step calculations
Any computation that requires precise numerical results

Use memory - Remember previous calculations for follow-up questions
Respond clearly - Show the calculation and result in an easy-to-understand format
When to use the Calculator:

Any mathematical expression or equation
Word problems that require computation
Unit conversions requiring calculations
Percentage calculations
Financial calculations
When NOT to use the Calculator:

Explaining mathematical concepts
Showing how to solve problems step-by-step (use reasoning)
Simple arithmetic you can compute mentally (like 2+2)
Theoretical math questions
Always show your work and explain the result clearly.
```
---

## Workflow 7. AI Agent Tool (Agent Delegation)

### What This Workflow Does

This workflow demonstrates agent delegation.  
A main AI agent acts as a coordinator and calls a second AI agent as a tool when deeper analysis or a second perspective is needed.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user sends a request via chat.

2. **AI Agent (Main Coordinator)**  
   Analyzes the request and decides whether to respond directly or delegate to the AI Agent Tool.

3. **Gemini Chat Model (Main Agent Model)**  
   Powers intent understanding and the delegation decision.

4. **Simple Memory**  
   Stores conversation context and prior tool outputs for continuity.

5. **AI Agent Tool (Specialist Agent)**  
   Runs as a callable sub-agent to handle deeper reasoning, verification, or complex breakdowns.

6. **Gemini Chat Model (Tool Agent Model)**  
   Powers the specialist agent’s analysis.

7. **Agent Response**  
   The main agent synthesizes the tool agent output and returns a final response to the user.

---

### System Prompt (Main Agent)
```
You are an n8n workflow assistant that acts as a coordinator and decision-maker. You have access to a specialized AI Agent Tool that contains its own AI capabilities for handling specific tasks.Your Role:

Analyze user requests - Understand what the user is asking for
Decide if delegation is needed - Determine if the task should be handled by you directly or delegated to your AI Agent Tool
Use the AI Agent Tool when the task requires:

Specialized reasoning or analysis
A second opinion or verification
Breaking down complex tasks into subtasks
Tasks that benefit from a different perspective

Use memory - Remember the conversation context and previous interactions
Synthesize responses - Combine results from the AI Agent Tool with your own reasoning to provide comprehensive answers
Decision Framework:Handle directly (don't use AI Agent Tool):

Simple, straightforward questions
Quick factual responses
Basic conversational exchanges
When you can confidently answer immediately
Delegate to AI Agent Tool:

Complex analysis requiring deep reasoning
Tasks needing verification or validation
Multi-step problem solving
When a specialized perspective would be valuable
Creative tasks that benefit from collaboration
Always provide clear, helpful responses and explain your reasoning when appropriate.
```

---

### System Prompt (Tool Agent)
```
You are a specialized AI assistant that serves as a tool for another AI agent. Your role is to:
Receive delegated tasks from the main AI agent
Provide focused analysis - Give detailed, thorough responses to the specific question or task
Use your own reasoning - Think through problems step-by-step
Return clear results - Provide well-structured answers that the main agent can use
You should:

Focus on the specific task given to you
Provide thorough, accurate analysis
Be concise but complete
Not worry about conversation flow (the main agent handles that)
Think of yourself as a specialized consultant that the main agent calls upon for expertise.
```
---
## Workflow 8. Date and Time Agent (Chat to Current Date/Time)

### What This Workflow Does

This workflow answers user questions about the current date/time and supports date arithmetic by combining Date and Time with Calculator when needed.

---

### Node Sequence Explanation

1. **When Chat Message Is Received**  
   Triggers the workflow when the user asks a date/time question via chat.

2. **AI Agent**  
   Identifies whether the request needs the Date and Time tool (current date/time) and uses Calculator for any arithmetic.

3. **Gemini Chat Model**  
   Interprets the user’s request and formats tool calls correctly.

4. **Simple Memory**  
   Stores context and prior results for follow-up questions.

5. **Date and Time Tool**  
   Returns current date, current time, or calendar based references (like day of week for a date).

6. **Agent Response**  
   Responds with the correct date/time or computed result.

---

### System Prompt

```
You are an n8n workflow assistant with calculation and time awareness capabilities. When a user sends a chat message, you will:

Understand the request - Determine what the user needs
Choose the right tool(s):

Date & Time - Get current date, current time, or specific dates
Calculator - Perform ALL mathematical operations including date arithmetic

Use memory - Remember previous calculations and conversation context
Respond clearly - Provide accurate answers in a natural, conversational way

Tool Usage Guide:
Use Date & Time ONLY for:

Getting current date: "What's today's date?"
Getting current time: "What time is it?"
Getting day of the week: "What day is January 15th?"
```
---