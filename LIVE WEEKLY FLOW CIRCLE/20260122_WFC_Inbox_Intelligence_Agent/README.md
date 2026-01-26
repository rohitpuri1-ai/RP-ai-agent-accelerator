# Inbox Intelligence Agent V1 - Using Google Sheets as a Tool

## Workflow Overview
This workflow uses an AI Agent with Google Sheets tool access to automatically categorize and log all incoming emails directly to a spreadsheet.

---

## Workflow Diagram

---
Gmail Trigger → AI Agent → [Tools: Model, Memory, Google Sheets, Output Parser]
---

**Node Sequence:**
1. **Gmail Trigger** - Monitors inbox for new emails
2. **AI Agent** - Analyzes and categorizes emails
   - **Google Gemini Chat Model** - Provides AI intelligence
   - **Simple Memory** - Maintains context
   - **Append row in sheet in Google Sheets** - Writes to spreadsheet
   - **Structured Output Parser** - Formats response data

---

## System Prompt

Copy and paste this into your AI Agent node:

```
You are an email sorting assistant with access to Google Sheets. Analyze every email and log it directly to the spreadsheet.

Analyze this email and categorize it:

Email From: {{ $json.From }}
Email Subject: {{ $json.Subject }}
Email Body: {{ $json.snippet }}
Thread ID: {{ $json.threadId }}

For each email, determine:
- Type: Choose one (Support, Sales, General Inquiry, Newsletter, Spam, Other)
- Category: Choose one (Urgent, Important, Normal, Low Priority)
- Requires Response: Answer Yes or No
- Suggested Response: Write 1-2 sentences suggesting how to reply, or write "No response needed"

After analyzing the email, use the Google Sheets tool to add a new row with all this information. Make sure every email gets logged.

Today's date is {{ $now }}
```

---

## Structured Output Parser

Copy and paste this into your Structured Output Parser node:

```json
{
  "type": "type of email",
  "category": "category of the email",
  "requires_response": "Yes/No",
  "suggested_response": "Suggested response by the AI Agent"
}
```

---

## Google Sheets Setup

### Required Headers

Please make a copy of this sheet - [Email Sorter](https://docs.google.com/spreadsheets/d/12DGj6emdR8Mjaf_ZGJBEHWeSXaKCNaBFKPxvZhdqQ5M/edit?usp=sharing)

---

## Configuration Requirements

### Required Credentials

1. **Gmail API Access**
   - OAuth credentials for Gmail Trigger
   - Permissions: Read emails

2. **Google Sheets API Access**
   - OAuth credentials for Sheets access
   - Permissions: Write to spreadsheet

3. **AI Model API Key**
   - Google Gemini API key
   - Or compatible AI model credentials

---

## How It Works

### Step-by-Step Process

1. **Email Arrives**
   - Gmail Trigger detects new email in inbox
   - Extracts: From, Subject, snippet, threadId

2. **AI Analysis**
   - AI Agent receives email data through system prompt
   - Analyzes content and context
   - Determines type, category, response need, and suggestion

3. **Automatic Logging**
   - AI Agent uses Google Sheets tool directly
   - Appends new row with all analyzed data
   - Structured Output Parser ensures consistent format

4. **Completion**
   - Email is logged in spreadsheet
   - Workflow ready for next email

---

## Example Output

### Input Email
```
From: john.customer@example.com
Subject: Help with order #12345
Body: Hi, I placed an order yesterday but haven't received confirmation. Can you help?
```

### AI Agent Response (Logged to Sheet)

| Date | Sender Email | Type | Category | Requires Response | Suggested Response |
|------|--------------|------|----------|-------------------|-------------------|
| 2026-01-23 | john.customer@example.com | Support | Urgent | Yes | Thank you for reaching out. We'll look into your order #12345 right away and send you the confirmation within the hour. |

---

## Troubleshooting

### Common Issues

**Problem**: Emails not being logged
- Check Gmail Trigger is active
- Verify Google Sheets credentials
- Ensure AI Agent has Google Sheets tool enabled

**Problem**: Incorrect categorization
- Refine system prompt with specific examples
- Add more detailed instructions for edge cases
- Review and adjust category definitions

**Problem**: Missing data in spreadsheet
- Verify column headers match exactly
- Check that all variables are correctly mapped
- Ensure structured output parser schema is valid

---

## Advantages of V1 Approach

✅ **Fully Autonomous**: AI Agent handles everything
✅ **Tool-Based Learning**: Great for teaching AI tool usage
✅ **Logs Everything**: Complete audit trail of all emails
✅ **Simple Setup**: Fewer nodes to configure
✅ **Direct Control**: AI makes decisions and executes actions

---

## Version

**v1.0** – Inbox Intelligence Agent using Google Sheets as an AI Agent Tool

---





---

# Inbox Intelligence Agent V2 - Only Logging the Responses that Need a Response

## Workflow Overview
This workflow uses an AI Agent to analyze incoming emails and only logs emails that require a response to a Google Sheet using conditional logic.

---

## Workflow Diagram

---
Gmail Trigger → AI Agent → If (Conditional) → Append row in sheet / No Operation
---

**Node Sequence:**
1. **Gmail Trigger** - Monitors inbox for new emails
2. **AI Agent** - Analyzes and categorizes emails
   - **Google Gemini Chat Model** - Provides AI intelligence
   - **Simple Memory** - Maintains context
   - **Structured Output Parser** - Formats response data
3. **If** - Checks if email requires response
4. **Append row in sheet** - Logs emails that need response (True branch)
5. **No Operation, do nothing** - Skips emails that don't need response (False branch)

---

## System Prompt

Copy and paste this into your AI Agent node:

```
Analyze this email and categorize it:

Email From: {{ $json.From }}
Email Subject: {{ $json.Subject }}
Email Body: {{ $json.snippet }}
Thread ID: {{ $json.threadId }}

Classify the email and provide:
1. Type: (Support, Sales, General Inquiry, Newsletter, Spam, or Other)
2. Category: (Urgent, Important, Normal, or Low Priority)
3. Requires Response: (Yes or No)
4. Suggested Response: (Brief 2-3 sentence suggestion for replying, or "No response needed")

Be concise and clear in your classifications.

Today's date is {{ $now }}
```

---

## Structured Output Parser

Copy and paste this into your Structured Output Parser node:

```json
{
  "date_received": "date of receiving the email",
  "sender_email": "name@email.com",
  "sender_name": "John Doe",
  "email_snippet": "email snippet",
  "thread_id": "abc123xyz",
  "type": "type of email",
  "category": "category of email",
  "requires_response": "Yes/No",
  "suggested_response": "response by AI Agent"
}
```

---

## Google Sheets Setup

### Required Headers

Please make a copy of this sheet - [Email Sorter](https://docs.google.com/spreadsheets/d/12DGj6emdR8Mjaf_ZGJBEHWeSXaKCNaBFKPxvZhdqQ5M/edit?usp=sharing)

---

## Configuration Requirements

### Required Credentials

1. **Gmail API Access**
   - OAuth credentials for Gmail Trigger
   - Permissions: Read emails

2. **Google Sheets API Access**
   - OAuth credentials for Sheets access
   - Permissions: Write to spreadsheet

3. **AI Model API Key**
   - Google Gemini API key
   - Or compatible AI model credentials

### If Node Configuration

- **Condition**: `{{ $json.requires_response }}` equals `Yes`
- **True branch**: Connects to "Append row in sheet"
- **False branch**: Connects to "No Operation, do nothing"

---

## How It Works

### Step-by-Step Process

1. **Email Arrives**
   - Gmail Trigger detects new email in inbox
   - Extracts: From, Subject, snippet, threadId

2. **AI Analysis**
   - AI Agent receives email data through system prompt
   - Analyzes content and context
   - Determines type, category, response need, and suggestion
   - Outputs structured data with all email information

3. **Conditional Check**
   - If node evaluates `requires_response` field
   - If "Yes" → proceeds to Google Sheets
   - If "No" → skips to No Operation node

4. **Selective Logging**
   - Only emails requiring responses are logged
   - Google Sheet stays clean and actionable
   - No Operation node ends workflow for other emails

---

## Example Output

### Input Email (Requires Response)
```
From: john.customer@example.com
Subject: Help with order #12345
Body: Hi, I placed an order yesterday but haven't received confirmation. Can you help?
```

### AI Agent Response → Logged to Sheet

| Date | Sender Email | Type | Category | Requires Response | Suggested Response |
|------|--------------|------|----------|-------------------|-------------------|
| 2026-01-23 | john.customer@example.com | Support | Urgent | Yes | Thank you for reaching out. We'll look into your order #12345 right away and send you the confirmation within the hour. |

### Input Email (No Response Needed)
```
From: newsletter@company.com
Subject: Weekly Newsletter - January 2026
Body: Check out this week's top stories...
```

### AI Agent Response → Not Logged (Skipped)
- Type: Newsletter
- Requires Response: No
- Action: Workflow ends at "No Operation" node

---

## Troubleshooting

### Common Issues

**Problem**: All emails being logged (or none being logged)
- Check If node condition: `{{ $json.requires_response }}` equals `Yes`
- Verify AI Agent is outputting "Yes" or "No" (case-sensitive)
- Test with a known support email

**Problem**: If node not working
- Ensure Structured Output Parser is before If node
- Check that `requires_response` field exists in output
- Verify connections: True → Append row, False → No Operation

**Problem**: Missing data in spreadsheet
- Verify all fields are mapped from AI Agent output
- Check Google Sheets node is using correct field names
- Ensure date_received, sender_email, etc. are available

---

## Advantages of V2 Approach

✅ **Selective Logging**: Only actionable emails are saved
✅ **Cleaner Data**: Spreadsheet contains only emails needing attention
✅ **Workflow Control**: Logic handled by If node, not AI
✅ **Better for Teams**: Focus on emails that matter
✅ **Reduced Clutter**: No newsletters, spam, or automated emails logged

---

## Version

**v2.0** – Inbox Intelligence Agent with Conditional Logging

---