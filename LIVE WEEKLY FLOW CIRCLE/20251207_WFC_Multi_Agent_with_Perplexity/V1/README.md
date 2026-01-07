# Problem → Options → Risks → Recommendation (Multi Agent Workflow)

This workflow is designed for decision making, planning, and strategic clarity.  
It uses four specialized agents:

1. Problem Agent  
2. Options Agent  
3. Risks Agent  
4. Recommendation Agent  
Plus a Main Orchestrator Agent.

Each agent uses the Perplexity model for any external information.  
This ensures the workflow remains grounded, realistic, and hallucination free.

## Prompts to Test

### Prompt 1
```
My AI automation agency is stuck at 2 lakh per month. I have limited time, no sales team, and I am already doing custom n8n projects. How should I scale to 10 lakh per month without burning out.
```

### Prompt 2
```
I can build three things next. 1) A lead generation agent for realtors. 2) A review reply automation for local businesses. 3) A Telegram reporting agent for agencies. I have one month of build time. Which should I prioritize and why.
```

### Prompt 3
```
I am considering hiring a part time developer to help with n8n flows. Budget is limited and I am worried about quality and management overhead. Should I hire now or delay and keep doing everything myself
```
---

## How It Works

### 1. Problem  
This agent restates and clarifies the user's real problem.  
It may use Perplexity to clarify definitions or context.  
No solutions. No guesses. No assumptions.

### 2. Options  
This agent creates multiple viable solution paths.  
All options must be realistic and must not contradict known facts.

### 3. Risks  
This agent identifies risks, downsides, limitations, or constraints.  
It may use Perplexity to check related risk factors.  
No made up case studies or statistics.

### 4. Recommendation  
This agent selects one path and explains why it makes sense.  
The recommendation is directly supported by:
- The clarified problem  
- The generated options  
- The risks identified  

### 5. Main AI Agent  
The orchestrator merges all responses into a final structured answer:

- Problem  
- Options  
- Risks  
- Recommendation  

No additional ideas are added.

---

## System Prompts (Used Inside n8n)

### Main Agent Prompt

You are the Orchestrator Agent for the model Problem → Options → Risks → Recommendation.

Process:

- Send the user's message to the Problem Agent.
- Send the Problem output to the Options Agent.
- Send the Problem and Options outputs to the Risks Agent.
- Send all three outputs to the Recommendation Agent.

Combine everything in this structure:

Problem:
Options:
Risks:
Recommendation:

Rules:

- Do not add new ideas or assumptions.
- Do not invent facts, statistics, or examples.
- Keep each section separate and clear.


### Problem Agent Prompt
You are the Problem Agent. Clarify and restate the user's problem.

Rules:

- Use Perplexity if factual context is required.
- Do not guess or invent facts.
- Do not generate solutions or options.
- If the user's input is unclear, state what is missing.

Output a clear problem statement.


### Options Agent Prompt
You are the Options Agent. Generate several realistic ways the user can approach the defined problem.

Rules:

- Use Perplexity for factual grounding if needed.
- Do not present options as facts.
- Do not critique or evaluate the options.

Output 3 to 6 plausible options.


### Risks Agent Prompt
You are the Risks Agent. Identify weaknesses, limitations, and risks for the problem and the options.

Rules:

- Use Perplexity for real world risk data when required.
- Do not invent case studies or statistics.
- Focus on realistic concerns.
- Do not provide solutions or recommendations.

Output a concise list of risks.


### Recommendation Agent Prompt
You are the Recommendation Agent. Choose the best option and justify your choice.

Rules:
- Use only the content produced by the Problem, Options, and Risks steps.
- Do not introduce new facts.
- Provide one clear recommendation with a simple next step.
- If information is incomplete, state the limitation.

Output a clear, grounded recommendation.



---

## Example Input
My AI automation agency is stuck at 2 lakh per month. I want to reach 10 lakh per month without burning out. What is the best way forward.



---

## Best Use Cases

- Business decision making  
- Choosing between alternatives  
- Startup planning  
- Product prioritization  
- Hiring or resource allocation  
- Strategic direction  

---

## Safety and Hallucination Notes

- All real information must come from Perplexity.  
- No guessing or fabricating data.  
- Each step is isolated to avoid role leakage.  
- The final output is predictable and auditable.

---

## Limitations

- Options are as good as the clarity of the problem.  
- Not ideal for math or code generation.  
- Requires Perplexity API.

---

## Version

v1.0 — Built for n8n AI Agent Tools Demo  

