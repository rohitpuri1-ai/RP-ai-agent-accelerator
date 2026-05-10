# PORR Framework: Problem → Options → Risks → Recommendation

## Workflow Overview

This workflow implements a structured decision-making framework using a multi-agent system. An orchestrator coordinates four specialized agents that work sequentially to analyze complex business problems and deliver grounded recommendations.

Each agent builds on the previous one's output, with web search integration to ensure recommendations are based on real data, not assumptions.

---

## Step-by-Step Setup Guide

### Step 1: Add the Chat Trigger

1. Click the **+** button on the canvas
2. Search for **"When chat message received"**
3. Add it to the canvas
4. This node will receive questions from users

### Step 2: Add the Orchestrator AI Agent

1. Click **+** and search for **"AI Agent"**
2. Rename it to **"AI Agent"** (this is your orchestrator)
3. Connect it to the chat trigger

### Step 3: Add the Chat Model for Orchestrator

1. Inside the AI Agent node, click **"Chat Model"**
2. Select **"OpenRouter Chat Model"**
3. In the model field, enter: `nvidia/nemotron-3-super-120b-a12b:free`
4. Save the configuration

### Step 4: Add Simple Memory to Orchestrator

1. Inside the AI Agent node, click **"Memory"**
2. Select **"Simple Memory"**
3. This allows follow-up questions in the conversation

### Step 5: Add the Problem Agent (Tool)

1. Inside the AI Agent node, click **"Tool"**
2. Select **"Problem"** (this is a sub-agent)
3. Configure the Problem agent:

**Chat Model:**
- Click **"Chat Model"** inside Problem agent
- Select **"OpenRouter Chat Model"**
- Model: `nvidia/nemotron-3-super-120b-a12b:free`

**Tool (Gemini Search):**
- Click **"Tool"** inside Problem agent
- Select **"Gemini Search Tool"**
- This allows the agent to search for factual evidence

### Step 6: Add the Options Agent (Tool)

1. Back in the main AI Agent, click **"Tool"** again
2. Select **"Options"** (second sub-agent)
3. Configure the Options agent:

**Chat Model:**
- Click **"Chat Model"** inside Options agent
- Select **"OpenRouter Chat Model"**
- Model: `nvidia/nemotron-3-super-120b-a12b:free`

**Tool (Gemini Search):**
- Click **"Tool"** inside Options agent
- Select **"Gemini Search Tool"**
- This allows the agent to validate solution paths

### Step 7: Add the Risks Agent (Tool)

1. Back in the main AI Agent, click **"Tool"** again
2. Select **"Risks"** (third sub-agent)
3. Configure the Risks agent:

**Chat Model:**
- Click **"Chat Model"** inside Risks agent
- Select **"OpenRouter Chat Model"**
- Model: `nvidia/nemotron-3-super-120b-a12b:free`

**Tool (Gemini Search):**
- Click **"Tool"** inside Risks agent
- Select **"Gemini Search Tool"**
- This is critical for identifying real-world risks

### Step 8: Add the Recommendation Agent (Tool)

1. Back in the main AI Agent, click **"Tool"** again
2. Select **"Recommendation"** (fourth sub-agent)
3. Configure the Recommendation agent:

**Chat Model:**
- Click **"Chat Model"** inside Recommendation agent
- Select **"OpenRouter Chat Model"**
- Model: `nvidia/nemotron-3-super-120b-a12b:free`

**Tool (Gemini Search):**
- Click **"Tool"** inside Recommendation agent
- Select **"Gemini Search Tool"**
- This validates the final recommendation

### Step 9: Configure System Prompts

Now you need to add the system prompts for each agent. Copy and paste these exactly:

---

## System Prompts

### Orchestrator Agent

```
You are the Orchestrator Agent for the model Problem → Options → Risks → Recommendation.

Process:
1. Send the user's question to the Problem Agent and wait for the search-based output.
2. Send the question plus the Problem output to the Options Agent.
3. Send the question, Problem, and Options outputs to the Risks Agent.
4. Send all outputs to the Recommendation Agent.
5. Combine everything into the final structure:

Problem:
Options:
Risks:
Recommendation:

Rules:
* Do not create new content. Only merge.
* Do not assume facts or examples that were not provided by the agents.
* Clearly separate each section.
* If any output is limited, include it as is.
```

### Problem Agent

```
You are the Problem Agent. You must ALWAYS call the Gemini Search tool if available.

Process:
1. Call Gemini Search using the user's question.
2. Use ONLY search results + the user's text to define the problem clearly.

Rules:
* Do not guess or assume anything outside search evidence or user text.
* Do not produce solutions, options, risks, or recommendations.
* If search does not provide relevant context, state the limitation.

Output: A clear factual definition of the problem.
```

### Options Agent

```
You are the Options Agent. You must ALWAYS call the Gemini Search tool if available.

Process:
1. Call Gemini Search using the user's question.
2. Combine:
   * Search results
   * Problem definition
3. Generate multiple realistic and grounded solution options.

Rules:
* Do not propose options that contradict search findings.
* Do not present ideas as facts.
* No risk analysis.
* No recommendation.

Output: A list of 3 to 6 grounded options.
```

### Risks Agent

```
You are the Risks Agent. You must ALWAYS call the Gemini Search tool if available.

Process:
1. Call Gemini Search using the user's question.
2. Combine:
   * Search results
   * Problem output
   * Options output
3. Identify realistic risks, weaknesses, or limitations.

Rules:
* Do not invent case studies, numbers, or examples.
* All risks must connect to search findings or directly to the Options.
* No solutions. No recommendations.

Output: A grounded list of risks.
```

### Recommendation Agent

```
You are the Recommendation Agent. You must ALWAYS call the Gemini Search tool if available.

Process:
1. Call Gemini Search using the user's question.
2. Combine:
   * Search results
   * Problem
   * Options
   * Risks
3. Choose the most logical option and provide a simple recommendation.

Rules:
* No new facts. No assumptions. No fabrication.
* Recommendation must be explicitly based on the previous outputs.
* If the evidence is unclear, mention limitations.

Output: A grounded and practical recommendation.
```

---

## Sample Prompts to Try

### Business Scaling
```
My AI automation agency is stuck at 2 lakh per month. I have limited time, no sales team, and I am already doing custom n8n projects. How should I scale to 10 lakh per month without burning out?
```

### Product Prioritization
```
I can build three things next: 1) A lead generation agent for realtors. 2) A review reply automation for local businesses. 3) A Telegram reporting agent for agencies. I have one month of build time. Which should I prioritize and why?
```

### Hiring Decision
```
I am considering hiring a part-time developer to help with n8n flows. Budget is limited and I am worried about quality and management overhead. Should I hire now or delay and keep doing everything myself?
```

**[Prompt Library →](https://multi-agent-prompt-library.pages.dev/)** *(Additional prompts for different use cases)*

---

## What This Workflow Does

The PORR Framework breaks down complex decisions into four sequential steps:

1. **Problem** - Defines the core issue using search-based evidence
2. **Options** - Lists realistic solution paths based on the problem
3. **Risks** - Identifies weaknesses and limitations of each option
4. **Recommendation** - Suggests the best path forward with clear reasoning

This prevents rushed decisions and ensures recommendations are grounded in real data, not guesswork.

---

## Workflow Diagram

```
Chat → Orchestrator → Problem Agent → Options Agent → Risks Agent → Recommendation Agent → Final Output
```

---

## Node Sequence

1. **When chat message received** - Receives complex business questions
2. **AI Agent (Orchestrator)** - Coordinates sequential analysis
   - **OpenRouter Chat Model** - Main reasoning engine using `nvidia/nemotron-3-super-120b-a12b:free`
   - **Simple Memory** - Maintains conversation context
   - **Problem Agent** - Defines the core issue with search evidence
     - **OpenRouter Chat Model1** - Uses `nvidia/nemotron-3-super-120b-a12b:free`
     - **Gemini Search Tool** - Searches for factual problem context
   - **Options Agent** - Generates realistic solution paths
     - **OpenRouter Chat Model2** - Uses `nvidia/nemotron-3-super-120b-a12b:free`
     - **Gemini Search Tool** - Validates solution approaches
   - **Risks Agent** - Identifies weaknesses and limitations
     - **OpenRouter Chat Model3** - Uses `nvidia/nemotron-3-super-120b-a12b:free`
     - **Gemini Search Tool** - Finds real-world risk data
   - **Recommendation Agent** - Synthesizes best path forward
     - **OpenRouter Chat Model4** - Uses `nvidia/nemotron-3-super-120b-a12b:free`
     - **Gemini Search Tool1** - Validates final recommendation

---

## How It Works

**Step 1: User Asks a Question**

User submits a complex business or strategic question requiring structured analysis.

**Step 2: Problem Agent Analyzes**

The Problem Agent:
1. Searches the web using Gemini Search Tool
2. Combines search results with the user's question
3. Defines the core problem clearly and factually
4. Returns problem definition to orchestrator

**Step 3: Options Agent Generates Paths**

The Options Agent receives:
- The original question
- The problem definition

Then:
1. Searches for solution approaches
2. Lists 3-6 realistic options
3. Returns options to orchestrator

**Step 4: Risks Agent Identifies Weaknesses**

The Risks Agent receives:
- The original question
- Problem definition
- Options list

Then:
1. Searches for real-world risks and challenges
2. Identifies weaknesses in each option
3. Returns risk assessment to orchestrator

**Step 5: Recommendation Agent Decides**

The Recommendation Agent receives:
- The original question
- Problem definition
- Options list
- Risks assessment

Then:
1. Searches for validation data
2. Weighs all previous outputs
3. Recommends the best path forward
4. Returns final recommendation to orchestrator

**Step 6: Orchestrator Combines**

The orchestrator merges all outputs into a structured response:

```
Problem: [Clear problem definition]

Options:
1. [Option 1]
2. [Option 2]
3. [Option 3]

Risks:
- [Risk for option 1]
- [Risk for option 2]
- [Risk for option 3]

Recommendation: [Best option with reasoning]
```

---

## Example Output

**Input:**
"Should I niche down to real estate AI automation or stay generalist?"

**Output:**
```
Problem:
You're facing the classic generalist vs. specialist positioning dilemma 
in the AI automation market. Current data shows both real estate tech 
adoption increasing 40% YoY and general AI automation demand growing 
across sectors. The core tension is between depth of expertise and 
breadth of opportunity.

Options:
1. Full pivot to real estate vertical (6-month transition)
2. Create a real estate sub-brand while maintaining general services
3. Stay generalist but develop real estate case studies
4. Test real estate niche with 3 pilot clients before committing
5. Partner with a real estate-focused agency instead of pivoting

Risks:
- Real estate market has seasonal fluctuations affecting demand
- Vertical specialization limits addressable market size
- Competitors already established in real estate AI automation
- General positioning makes differentiation harder
- Pivoting mid-operation risks losing existing client relationships
- Testing approach may dilute brand messaging

Recommendation:
Option 4 (test with 3 pilots) is recommended. This allows validation 
of real estate demand and pricing without burning bridges. Set a 
90-day test period with clear metrics: deal size, close rate, and 
referral potential. If real estate clients show 2x better unit 
economics than general clients, commit to the niche. Otherwise, 
maintain generalist positioning with real estate as a case study 
vertical.
```

---

## Why This Framework Works

Most decision-making fails at one of three points:

1. **Unclear problem definition** - Leads to solving the wrong thing
2. **Limited options** - Leads to false dilemmas
3. **Ignored risks** - Leads to unexpected failures

The PORR Framework forces structured thinking:

- **Problem** ensures you're solving the right thing
- **Options** prevents binary thinking (this OR that)
- **Risks** surfaces blind spots before commitment
- **Recommendation** synthesizes everything into action

Each agent searches independently, preventing confirmation bias.

---

## Mental Model

This is not brainstorming.

**This is structured analysis.**

The difference:
- Brainstorming = Opinions and gut feelings
- PORR = Evidence-based reasoning
- Traditional AI = One-shot answers
- PORR Framework = Multi-perspective validation

Each agent challenges the previous one's output by searching independently.

---

## Architecture Pattern

```
Question → Problem Search → Options Search → Risks Search → Recommendation Search → Synthesis
```

Every agent must search before answering. No agent can rely solely on the previous agent's output.

This prevents the "telephone game" effect where errors compound through the chain.

---

## The Core Insight

Good decisions require:
1. Understanding the real problem
2. Seeing all options
3. Knowing the risks
4. Acting on evidence

This framework ensures you don't skip any step.

---

## Key Features

**Sequential Processing**
- Each agent builds on previous outputs
- No assumptions or skipped steps
- Complete context flows through the chain

**Search-Grounded Analysis**
- All agents use Gemini Search Tool
- Recommendations based on real data
- No fabricated examples or stats

**Structured Output**
- Consistent format every time
- Easy to scan and act on
- Clear separation of analysis phases

**No Hallucination Design**
- Agents cannot invent facts
- Must cite search evidence
- Limitations are stated explicitly

---

## Use Cases

- Business strategy decisions
- Product prioritization
- Hiring and resource allocation
- Market entry decisions
- Technology stack selection
- Pricing strategy evaluation
- Partnership opportunity assessment
- Process optimization choices

---

## Technical Notes

**Model Used:**
- All agents use `nvidia/nemotron-3-super-120b-a12b:free` via OpenRouter
- This model is free and high-quality for reasoning tasks

**Search Integration:**
- Gemini Search Tool is used by all four sub-agents
- Each agent searches independently with its own context
- Search results are combined with agent-specific analysis

**Memory:**
- Simple Memory in orchestrator enables follow-up questions
- Sub-agents are stateless (no memory)
- Context flows through orchestrator only

---

## Limitations

**Sequential Processing**
- Agents run one after another (not parallel)
- Total response time increases with complexity

**Search Dependency**
- Quality depends on search result relevance
- Some niche topics may have limited data

**Single Question Focus**
- Designed for one decision at a time
- Comparing multiple options requires separate queries

---

## Production Considerations

To make this production-ready:

1. **Add error handling** for failed search calls
2. **Implement retry logic** if an agent doesn't respond
3. **Add logging** to track which agent is processing
4. **Set timeouts** for each agent (prevent hanging)
5. **Add input validation** to ensure question quality
6. **Include cost tracking** for API usage
7. **Add output formatting** for better readability

---

You now have a repeatable system for making better business decisions backed by real data.