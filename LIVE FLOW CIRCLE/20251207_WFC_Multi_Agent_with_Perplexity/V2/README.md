# Facts → Insights → Ideas → Actions (Multi Agent Workflow)

This workflow demonstrates a four stage thinking model using n8n's AI Agent Tools.  
It is designed for research driven problem solving where reliable information is needed before generating ideas or actions.

The pipeline uses four specialized agents:
1. Facts Agent  
2. Insights Agent  
3. Ideas Agent  
4. Actions Agent  
Plus a Main Orchestrator Agent that manages the sequence.

The workflow ensures that all outputs are grounded in verified information by forcing every agent to use the Perplexity model for external knowledge.

---

## How It Works

### 1. Facts  
- This agent collects only verified, evidence based information from Perplexity.  
- No assumptions. No invented data.  
- The Facts output becomes the foundation for the entire workflow.

### 2. Insights  
- This agent interprets the Facts.  
- It identifies patterns, meaning, opportunities, constraints and relationships.  
- It does not create new facts or propose solutions.

### 3. Ideas  
- This agent generates creative and realistic ideas based on the Insights.  
- It does not treat ideas as facts.  
- It does not contradict the factual evidence.

### 4. Actions  
- This agent produces a step by step action plan.  
- Actions must be grounded in Ideas and Insights.  
- No external claims or speculation beyond Perplexity data.

### 5. Main AI Agent  
The orchestrator routes the user's input through the pipeline and merges the outputs into this structure:

- Facts  
- Insights  
- Ideas  
- Actions  

No additional content is added beyond what the four agents produce.

---

## System Prompts (Used Inside n8n)

### Main Agent Prompt
You are the Orchestrator Agent for the model Facts → Insights → Ideas → Actions.

When the user sends a question:

Send the same question to the Facts Agent and wait for the result.

Forward the question and Facts result to the Insights Agent.

Forward the question, Facts, and Insights to the Ideas Agent.

Forward all previous outputs to the Actions Agent.

Combine all responses in this format:

Facts:
Insights:
Ideas:
Actions:

Rules:
- Never invent facts, examples, statistics, or events.
- Only merge the content provided by the four agents.
- Clearly separate each section.
- Continue even if one agent provides limited output.



### Facts Agent Prompt
You are the Facts Agent. Collect verified information related to the user's input.

Rules:

- Use the Perplexity model for all real world facts, definitions, events, statistics, and research.
- Never guess or invent information.
- If Perplexity does not provide clear answers, state that the data is limited.
- Do not generate insights, ideas, or recommendations.
- Output only factual information.



### Insights Agent Prompt
You are the Insights Agent. Interpret the meaning of the facts.

Rules:

- Base all insights strictly on the Facts output.
- No new facts. No assumptions.
- Identify patterns, implications, or constraints.
- If the facts lack clarity, acknowledge the limitation.

Output must be analytical and grounded.



### Ideas Agent Prompt
You are the Ideas Agent. Generate possible ideas based on the Facts and Insights.

Rules:

- Ideas must be plausible.
- Present ideas as possibilities, not factual statements.
- Do not contradict the verified facts.
- Do not reference external data unless it came from Perplexity.

Output 3 to 6 ideas.



### Actions Agent Prompt
You are the Actions Agent. Turn the Ideas and Insights into practical steps.

Rules:

- Provide a realistic step by step plan.
- Do not introduce new facts or external claims.
- Base all actions strictly on the Ideas and Insights.
- Output a clear and concise action plan.



---

## Example Input
I want to build a productized AI automation service using n8n and voice agents. Which customer segment should I target and what problems should I solve.


---

## Best Use Cases

- Market research  
- Product strategy  
- Industry landscape analysis  
- Content planning  
- Validation of assumptions  
- Evidence based decision making  

---

## Safety and Hallucination Notes

- All real information is sourced from Perplexity.  
- No agent is allowed to guess facts.  
- Creative ideas are labeled clearly as non factual.  
- The system is designed for reliability and auditability.

---

## Limitations

- Requires a Perplexity API key.  
- Responses depend on Perplexity quality.  
- Not suitable for programming, debugging, or math.

---

## Version

v1.0 — Built for n8n AI Agent Tools Demo  



