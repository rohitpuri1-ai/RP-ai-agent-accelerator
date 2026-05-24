# Automated Lead Nurturing Email System

## Workflow Overview

This workflow implements an automated lead nurturing system that reads customer data from Google Sheets, sends personalized emails based on lead temperature (Hot/Warm/Cold), and automatically updates the tracking status.

The system runs on a schedule, processes leads intelligently, and ensures no duplicate emails are sent by tracking completion status.

---

## Step-by-Step Setup Guide

### Step 1: Prepare Your Google Sheet

**Option 1: Use the Template (Recommended)**

1. Open the template sheet: [Lead Nurturing Template](https://docs.google.com/spreadsheets/d/1LmZP5OGl21332_dmg2f3lN-JwXo1ffuPsS7Ezex2gQQ/edit?gid=0#gid=0)
2. Click **File → Make a copy**
3. Rename your copy (e.g., "My Lead Nurturing Database")
4. The template includes sample data to help you understand the structure

**Option 2: Create Your Own Sheet**

Create a Google Sheet with these exact column names:
   - **Customer Name** - Full name of the lead
   - **Phone** - Contact phone number
   - **Email** - Email address
   - **Location** - City or region
   - **Lead Status** - Must be: Hot, Warm, or Cold
   - **Email Status** - Leave blank initially (will be marked "Complete" after sending)

**Important:** Column names must match exactly for the workflow to work.

### Step 2: Add the Schedule Trigger

1. Click the **+** button on the n8n canvas
2. Search for **"Schedule Trigger"**
3. Configure how often to run (e.g., every day at 9 AM)
4. This controls when the workflow executes automatically

### Step 3: Add the AI Agent

1. Click **+** and search for **"AI Agent"**
2. Add it to the canvas
3. Connect it to the Schedule Trigger

### Step 4: Add the Chat Model

1. Inside the AI Agent node, click **"Chat Model"**
2. Select **"Google Gemini Chat Model"**
3. Choose your preferred Gemini model (e.g., gemini-1.5-flash)
4. This powers the email personalization logic

### Step 5: Add Gmail Tool

1. Inside the AI Agent node, click **"Tool"**
2. Search for **"Send a message in Gmail"**
3. Connect your Gmail account
4. Set Email Type to **"HTML"** (optional, for formatted emails)
5. The agent will use this to send emails

### Step 7: Add Get Rows Tool

1. Click **"Tool"** again in the AI Agent
2. Search for **"Get row(s) in sheet in Google Sheets"**
3. Select **"read: sheet"** operation
4. Connect your Google account
5. Select your spreadsheet and sheet name
6. The agent will use this to fetch leads

### Step 8: Add Update Row Tool

1. Click **"Tool"** again in the AI Agent
2. Search for **"Update row in sheet in Google Sheets"**
3. Select **"update: sheet"** operation
4. Connect your Google account
5. Select the same spreadsheet and sheet
6. The agent will use this to mark emails as complete

### Step 9: Configure the User Message

Paste this into the AI Agent's **Chat Input** or **User Message** field:

```
You are an automated lead nurturing agent. Your task is to send personalized emails based on lead temperature.

Process:
1. Use the Get rows tool to fetch customers from Google Sheets
2. Filter for customers where "Email Status" is blank
3. Send only 2 emails per execution, then stop
4. For each customer, personalize the email based on their "Lead Status":

Email Personalization Rules:
- Hot Leads: Request a demo meeting
  Subject: "Ready to see [Product Name] in action?"
  Body: Express excitement, ask for their availability for a demo this week
  
- Warm Leads: Gauge interest level
  Subject: "How does [Product Name] fit your needs?"
  Body: Ask what aspects of the product interest them most
  
- Cold Leads: Gather feedback
  Subject: "What would make [Product Name] perfect for you?"
  Body: Ask what additional features they'd like to see

Output Format:
For each email, provide:
- Subject: [email subject]
- Body: [email body]

After sending each email:
1. Use the Gmail tool to send the message
2. Use the Update row tool to set "Email Status" to "Complete"

Stop after sending exactly 2 emails.
```

**Important:** Replace `[Product Name]` with your actual product name before running.

---

## What This Workflow Does

- Runs automatically on your defined schedule
- Reads lead data from Google Sheets
- Identifies leads that haven't been emailed yet (blank Email Status)
- Sends personalized emails based on lead temperature:
  - **Hot** leads get demo requests
  - **Warm** leads get interest qualification
  - **Cold** leads get feedback requests
- Updates Google Sheets to mark emails as sent
- Sends only 2 emails per execution (prevents spam and rate limits)

---

## Workflow Diagram

```
Schedule Trigger → AI Agent → [Gmail + Google Sheets Read + Google Sheets Update] → Email Sent + Status Updated
```

---

## Node Sequence

1. **Schedule Trigger** - Executes workflow at specified intervals
2. **AI Agent** - Orchestrates the email nurturing process
   - **Google Gemini Chat Model** - Powers personalization logic
   - **Send a message in Gmail** - Sends personalized emails
   - **Get row(s) in sheet** - Reads lead data from Google Sheets
   - **Update row in sheet** - Marks emails as complete

---

## How It Works

**Step 1: Scheduled Execution**

The workflow triggers automatically based on your schedule (e.g., daily at 9 AM).

**Step 2: Fetch Leads**

The AI Agent:
1. Calls the "Get rows" tool
2. Retrieves all customer records from Google Sheets
3. Filters for rows where "Email Status" is blank

**Step 3: Personalize Emails**

For each lead (max 2 per execution):
1. Reads the "Lead Status" column (Hot/Warm/Cold)
2. Generates appropriate subject line and email body
3. Personalizes with customer name and context

**Step 4: Send Email**

The agent:
1. Uses the Gmail tool to send the personalized email
2. Includes the generated subject and body

**Step 5: Update Tracking**

After successful send:
1. Uses the "Update row" tool
2. Sets "Email Status" to "Complete" for that customer
3. Prevents duplicate emails on next execution

**Step 6: Repeat**

Process repeats for the second lead, then stops until next scheduled execution.

---

## Email Templates by Lead Type

### Hot Leads (Demo Request)

**Subject:** "Ready to see [Product Name] in action, [Customer Name]?"

**Body:**
```
Hi [Customer Name],

I'm excited to connect with you about [Product Name]! Based on your 
interest, I'd love to show you how we can help [solve their problem].

Are you available for a quick 15-minute demo this week? I have slots 
open on [Day 1] or [Day 2].

Looking forward to showing you what we've built!

Best regards,
[Your Name]
```

### Warm Leads (Interest Qualification)

**Subject:** "How does [Product Name] fit your needs, [Customer Name]?"

**Body:**
```
Hi [Customer Name],

Thanks for your interest in [Product Name]! I'd love to understand 
what aspects of our solution resonate most with you.

What specific features or capabilities are you most interested in?

This will help me share the most relevant information with you.

Best regards,
[Your Name]
```

### Cold Leads (Feedback Request)

**Subject:** "What would make [Product Name] perfect for you, [Customer Name]?"

**Body:**
```
Hi [Customer Name],

I noticed you checked out [Product Name]. We're always improving based 
on customer feedback.

What additional features or capabilities would you like to see that 
would make this a must-have solution for you?

Your input helps shape our roadmap!

Best regards,
[Your Name]
```

---

## Prompt Library

For industry-specific prompts and ready-to-use templates across 8 different use cases:

**Title:** Lead Nurturing Automation - Prompt Library

**Link:** https://agentic-emailer.vercel.app/

---

## Sample Prompts to Try

While this workflow uses a fixed user message, here are variations you can customize:

### Aggressive Follow-Up (Shorter Timeline)
```
Send 5 emails per execution instead of 2. For Hot leads, request same-day 
or next-day demos with urgency.
```

### Multi-Touch Campaign
```
Add a "Follow-Up Count" column. Send different emails based on:
- First touch: Introduction
- Second touch: Case study or testimonial
- Third touch: Special offer or discount
```

### Product-Specific Campaigns
```
Segment by product interest. Read a "Product Interest" column and customize 
emails with specific product benefits and use cases.
```

### Geographic Personalization
```
Use the Location column to add region-specific references, local case studies, 
or timezone-appropriate meeting suggestions.
```

---

## Use Cases

### Sales Team Automation
- Automate first-touch outreach for new leads
- Qualify leads before passing to sales reps
- Schedule demos automatically
- Reduce manual email writing time

### Marketing Campaigns
- Nurture leads based on engagement level
- Segment messaging by customer temperature
- Track email completion rates
- Run drip campaigns automatically

### Customer Success
- Re-engage cold leads with feedback requests
- Identify hot leads for priority follow-up
- Automate onboarding email sequences
- Collect product improvement ideas

### Real Estate Agents
- Follow up with property inquiries
- Schedule viewing appointments
- Gauge buyer interest levels
- Collect property preferences

### SaaS Product Launches
- Invite beta users to demos
- Collect feature requests from interested users
- Prioritize hot leads for early access
- Build waitlist engagement

### Consulting Services
- Schedule discovery calls
- Qualify project fit
- Send service proposals
- Gather requirements from prospects

---

## Key Features

**Intelligent Lead Segmentation**
- Personalizes messaging based on lead temperature
- Different CTAs for different engagement levels
- Prevents generic, one-size-fits-all emails

**Automatic Tracking**
- Marks emails as sent to prevent duplicates
- Creates audit trail in Google Sheets
- Easy to see nurturing progress at a glance

**Rate Limiting**
- Sends only 2 emails per execution
- Prevents spam flags and Gmail limits
- Spreads outreach over time naturally

**Scheduled Automation**
- Set-and-forget daily/weekly execution
- Consistent touchpoints without manual work
- Scales to hundreds of leads effortlessly

---

## Why This Works

Traditional lead nurturing fails because:

1. **Manual emails don't scale** - Sales reps can't personalize hundreds of emails
2. **Generic blasts get ignored** - One-size-fits-all emails have low response rates
3. **Follow-up gets forgotten** - Leads fall through the cracks without tracking

This workflow solves all three:

- **Scales automatically** - Runs on schedule, processes entire lead list
- **Personalizes intelligently** - Different messages for Hot/Warm/Cold
- **Tracks completion** - Never emails the same lead twice

---

## Mental Model

This is not a mass email blast.

**This is intelligent lead nurturing.**

The difference:
- Email blasts = Same message to everyone
- Lead nurturing = Right message to right person at right time
- Manual nurturing = Doesn't scale
- Automated nurturing = Scales infinitely

The AI agent reads context (lead temperature), personalizes appropriately, and tracks completion automatically.

---

## Configuration Tips

### Adjust Email Volume

Change `Send only 2 emails` to higher numbers if you want more aggressive outreach:
- **2 emails** = Safe, gradual nurturing
- **5 emails** = Moderate pace
- **10+ emails** = Aggressive campaign (watch Gmail limits)

### Customize Templates

Edit the email rules in the user message to match your:
- Product name
- Brand voice
- Industry terminology
- Call-to-action preferences

### Schedule Frequency

**Daily (9 AM)** - Steady nurturing, 2 emails per day = 60/month
**3x per week** - Moderate pace, 6 emails per week = 24/month
**Weekly** - Conservative approach, 2 emails per week = 8/month

### Add More Lead Statuses

Expand beyond Hot/Warm/Cold:
- **Super Hot** - Immediate demo requests
- **Interested** - Send case studies
- **Inactive** - Re-engagement campaigns
- **Unqualified** - Polite decline with resources

---

## Limitations

**Gmail Sending Limits**
- Free Gmail: ~500 emails/day
- Google Workspace: ~2,000 emails/day
- Adjust frequency and volume accordingly

**No Email Validation**
- Workflow assumes valid email addresses
- Bounced emails won't be tracked automatically

**Single Product Focus**
- Default setup assumes one product
- Multi-product requires additional logic

**No A/B Testing**
- Sends one version of each email type
- No built-in split testing for optimization

---

## Production Considerations

To make this production-ready:

1. **Add email validation** before sending
2. **Track bounce rates** and update sheet accordingly
3. **Implement unsubscribe handling** for compliance
4. **Add logging** for sent emails and errors
5. **Set up error notifications** if sending fails
6. **Monitor Gmail quota** to prevent hitting limits
7. **Add personalization tokens** (company name, industry, etc.)
8. **Include email signature** with contact info
9. **Test with small batches** before full deployment

---

## Compliance Notes

**CAN-SPAM / GDPR Compliance:**
- Ensure leads have opted in to receive emails
- Include unsubscribe link in email templates
- Add physical mailing address in signature
- Honor opt-out requests immediately
- Only email leads who have given consent

This workflow handles automation, not compliance. You must ensure legal requirements are met.

---

## The Core Insight

Lead nurturing is not about sending more emails.

It's about sending **the right email** to **the right person** at **the right temperature**.

Hot leads need meetings.
Warm leads need qualification.
Cold leads need engagement.

This workflow ensures each lead gets the message that matches their journey stage.

---

You now have an automated system that nurtures leads intelligently without manual effort.