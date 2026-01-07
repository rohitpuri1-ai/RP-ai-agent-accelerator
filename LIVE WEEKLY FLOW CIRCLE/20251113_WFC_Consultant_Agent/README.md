# 🚀 **Build the “Consultant AI Agent” n8n Flow — Step-By-Step Guide**

A complete Markdown guide your users can follow to recreate the flow.

---

## #️⃣ **Overview of the Automation**

This n8n workflow does three things:

1. **Collects business details via a FormTrigger**
2. **Sends the answers to an AI Agent** that generates personalised automation recommendations
3. **Transforms the output into a polished HTML email**
4. **Emails the final formatted message** to the user

---

# 🧩 **FLOW STRUCTURE**

```
Form Trigger ➜ AI Agent (LangChain Agent) ➜ Code Node (HTML Email Builder) ➜ Gmail Send
```

---

# ✅ **1. Create the Form Trigger Node**

### Node Name

**On form submission**

### Form Title

`Discover Where AI Agents Can Help Your Business`

### Form Description

`Fill this form to know how can AI Agents help you`

---

## 📌 **Form Fields (Copy-Paste Friendly List)**

Create these fields in exact order:

### **1. What is your name?**

* Type: Text
* Required: Yes

### **2. What is your business/company name?**

* Type: Text
* Required: Yes

### **3. Please enter your email address**

* Type: Email
* Required: Yes

### **4. Select the area where you want AI Agents to make the biggest impact**

* Type: Radio
* Options:

  * Marketing
  * Sales
  * Business Operations
  * HR / Recruitment
  * Customer Support
  * Finance and Accounts

### **5. Briefly describe your current process or challenge in this area**

* Type: Long text

### **6. What outcome would you like AI Agents to achieve for you?**

* Type: Long text

---

# 🤖 **2. AI Agent Node Setup**

Node Type: **LangChain Agent**

### **Prompt Input (Copy-Paste Ready)**

In **“Text / Define”**, paste:

```
={{ $json["What is your name?" ] }}, {{ $json['Select the area where you want AI Agents to make the biggest impact'] }}, {{ $json['Briefly describe your current process or challenge in this area'] }}, {{ $json['What outcome would you like AI Agents to achieve for you?'] }}
```

---

## 📌 **SYSTEM PROMPT (Copy-Paste Entire Block)**

Paste this inside **System Message**:

```
You are an AI Business Strategist helping small and medium businesses, students, and MSMEs understand *how AI agents can improve their specific area of work.*

You will receive input data in JSON format with the following fields:
{
  "area": "<marketing/sales/operations/hr/customer_support/finance/others>",
  "department": "<optional>",
  "challenge": "<text>",
  "desired_outcome": "<text>"
}

Your task:
1. Interpret their business area and challenge.
2. Identify 3–4 high-impact AI agent ideas specific to that area.
3. For each idea, use EXACTLY this format:
   - **What it automates**: [description]
   - **Tangible business benefit**: [description]
4. End with a short summary paragraph titled "Next Step" that explains how they can start with one quick-win agent and scale later.
5. Write the entire output as an email-ready message (subject line + body), friendly yet professional in tone, suitable for MSME owners or professionals.

CRITICAL FORMATTING RULES:
- Each AI agent MUST follow this exact structure:
  1. **Agent Name**
     - **What it automates**: [Single clear description]
     - **Tangible business benefit**: [Single clear description]

- DO NOT use variations like "What it simplifies", "Role", etc.
- Use ONLY "What it automates" and "Tangible business benefit"
- Keep each field on a single line.

Always sign off with:
Best regards,

Output Format - plain text without markdown notations
```

---

# 🧪 **3. Code Node (Convert AI Output → Beautiful HTML Email)**

### Node Name

**Code in JavaScript**

### Paste this Entire Code Block:

```
// n8n Code Node - Convert AI Agent Email to Beautiful HTML
const inputData = $input.all();
const emailContent = inputData[0].json.output;
const fullText = emailContent;

// Extract subject
const subjectMatch = fullText.match(/Subject:\s*(.+?)[\n\r]/);
const subject = subjectMatch ? subjectMatch[1].trim() : 'AI Agent Recommendations';

// Extract recipient name
const recipientMatch = fullText.match(/Dear\s+(.+?),/);
const recipientName = recipientMatch ? recipientMatch[1].trim() : 'Valued Client';

// Extract AI agents
const agents = [];
let agentSections = fullText.split(/(?=\d+\.\s+\*\*[^*]+?\*\*)/);
if (agentSections.length <= 1) {
  agentSections = fullText.split(/(?=^-\s+\*\*[^*]+?\*\*)/m);
}

for (const section of agentSections) {
  if (!section.match(/^(\d+\.|-)\s+\*\*/)) continue;
  
  const nameMatch = section.match(/^(?:\d+\.|-)\s+\*\*(.+?)\*\*/);
  if (!nameMatch) continue;
  
  const name = nameMatch[1].trim();
  let automation = '';
  let benefit = '';
  
  const autoMatch = section.match(/\*\*What it automates\*\*:\s*(.+?)(?=\n\s*-\s*\*\*|$)/is);
  if (autoMatch) automation = autoMatch[1].trim();
  
  const benefitMatch = section.match(/\*\*Tangible business benefit\*\*:\s*(.+?)(?=\n\n|-\s+\*\*|\d+\.\s+\*\*|Next Step|$)/is);
  if (benefitMatch) benefit = benefitMatch[1].trim();
  
  if (automation || benefit) {
    agents.push({ name, automation, benefit });
  }
}

// Extract next step
const nextStepMatch = fullText.match(/Next Step:\s*(.+?)(?=\n\nBest regards|$)/is);
const nextStep = nextStepMatch ? nextStepMatch[1].trim() : '';

// Build agent cards HTML
let agentCardsHTML = '';
for (let i = 0; i < agents.length; i++) {
  const agent = agents[i];
  agentCardsHTML += '<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 12px; padding: 24px; margin-bottom: 20px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">';
  agentCardsHTML += '<table width="100%" cellpadding="0" cellspacing="0"><tr>';
  agentCardsHTML += '<td style="vertical-align: middle; width: 48px; padding-right: 16px;"><div style="background: rgba(255,255,255,0.2); border-radius: 50%; width: 48px; height: 48px; text-align: center; line-height: 48px; font-size: 24px; font-weight: bold; color: white;">' + (i + 1) + '</div></td>';
  agentCardsHTML += '<td style="vertical-align: middle;"><h3 style="margin: 0; color: white; font-size: 22px; font-weight: 600;">' + agent.name + '</h3></td>';
  agentCardsHTML += '</tr></table><div style="height: 16px;"></div>';
  
  if (agent.automation) {
    agentCardsHTML += '<div style="background: rgba(255,255,255,0.15); border-radius: 8px; padding: 16px; margin-bottom: 12px;">';
    agentCardsHTML += '<div style="color: rgba(255,255,255,0.9); font-weight: 600; margin-bottom: 6px; font-size: 14px; text-transform: uppercase;">⚙️ WHAT IT AUTOMATES</div>';
    agentCardsHTML += '<div style="color: white; font-size: 15px; line-height: 1.6;">' + agent.automation + '</div></div>';
  }
  
  if (agent.benefit) {
    agentCardsHTML += '<div style="background: rgba(255,255,255,0.25); border-radius: 8px; padding: 16px; border-left: 4px solid #fbbf24;">';
    agentCardsHTML += '<div style="color: white; font-weight: 600; margin-bottom: 6px; font-size: 14px; text-transform: uppercase;">💎 TANGIBLE BENEFIT</div>';
    agentCardsHTML += '<div style="color: white; font-size: 15px; line-height: 1.6; font-weight: 500;">' + agent.benefit + '</div></div>';
  }
  
  agentCardsHTML += '</div>';
}

// Build complete HTML
const html = '<!DOCTYPE html><html><head><meta charset="UTF-8"><title>' + subject + '</title></head>' +
'<body style="margin: 0; padding: 0; font-family: -apple-system, sans-serif; background-color: #f3f4f6;">' +
'<table width="100%" cellpadding="0" cellspacing="0" style="background-color: #f3f4f6; padding: 40px 20px;"><tr><td align="center">' +
'<table width="600" cellpadding="0" cellspacing="0" style="background-color: white; border-radius: 16px; overflow: hidden; box-shadow: 0 10px 25px rgba(0,0,0,0.1);">' +
'<tr><td style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 40px 30px; text-align: center;">' +
'<h1 style="margin: 0; color: white; font-size: 28px; font-weight: 700;">' + subject + '</h1>' +
'<p style="margin: 10px 0 0 0; color: rgba(255,255,255,0.9); font-size: 16px;">Transforming Your Business with Intelligent Automation</p></td></tr>' +
'<tr><td style="padding: 30px 30px 20px 30px;"><p style="margin: 0 0 10px 0; color: #1f2937; font-size: 16px;">Dear <strong>' + recipientName + '</strong>,</p>' +
'<p style="margin: 0; color: #4b5563; font-size: 15px;">I hope this message finds you well!</p></td></tr>' +
'<tr><td style="padding: 0 30px 30px 30px;"><p style="margin: 0; color: #4b5563; font-size: 15px; line-height: 1.7;">Based on the information I received, here are some focused AI agent ideas.</p></td></tr>' +
'<tr><td style="padding: 0 30px;">' + agentCardsHTML + '</td></tr>' +
(nextStep ? '<tr><td style="padding: 30px; background-color: #fef3c7; border-top: 3px solid #fbbf24;"><h3 style="margin: 0 0 16px 0; color: #92400e; font-size: 20px;">🚀 Next Step</h3><p style="margin: 0; color: #78350f; font-size: 15px;">' + nextStep + '</p></td></tr>' : '') +
'<tr><td style="padding: 30px;"><p style="margin: 0 0 20px 0; color: #4b5563; font-size: 15px;">Looking forward to hearing your thoughts!</p>' +
'<p style="margin: 0 0 8px 0; color: #1f2937; font-size: 15px; font-weight: 600;">Best regards,</p>' +
'<p style="margin: 0; color: #667eea; font-size: 16px; font-weight: 700;">Consulting Agent KAI</p></td></tr>' +
'<tr><td style="background-color: #f9fafb; padding: 20px 30px; text-align: center;"><p style="margin: 0; color: #9ca3af; font-size: 13px;">This email was sent by your AI Consulting Agent.</p></td></tr>' +
'</table></td></tr></table></body></html>';

return [{ json: { subject: subject, html: html, recipient: recipientName, agentCount: agents.length } }];
```

(Your full JS code block is already perfect and very long, so you keep it as is.)

---

# 📧 **4. Gmail Send Node**

### Node Name

**Send a message**

### Values

**Send To:**

```
={{ $('On form submission').item.json['Please enter your email address'] }}
```

**Subject:**

```
={{ $json.subject }}
```

**Message:**

```
={{ $json.html }}
```

**Sender Name:**
`Consulting Agent Kai`

Connect your Gmail OAuth credentials.

---

# 🔗 **5. Connect the Nodes**

```
On form submission → AI Agent → Code Node → Gmail Node
```

Also connect:

```
Google Gemini Chat Model → AI Agent (via ai_languageModel)
```

---

# 🎉 **DONE — Your Consultant AI Agent Flow Is Ready**