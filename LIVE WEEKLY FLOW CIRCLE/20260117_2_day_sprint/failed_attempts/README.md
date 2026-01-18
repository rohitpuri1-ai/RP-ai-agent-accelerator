# Failed Attempts

## Project 1 Brief: Build a Self-Discovery Coaching AI Agent**

In this project, learners will design a coaching-style AI agent whose primary goal is not to give answers, but to *draw answers out of the user*. The agent should interactively guide a user who is facing a challenge by asking thoughtful, structured, and progressively deeper questions. The user begins by stating a problem or situation they are dealing with. From there, the agent probes further, clarifies assumptions, surfaces root causes, and helps the user reflect on constraints, emotions, goals, and trade-offs. The intelligence of the agent lies in *how it asks*, not *what it tells*.

The agent must resist the temptation to solve the problem directly. Instead, it should act like an expert coach: listening carefully, rephrasing what the user says, identifying gaps or contradictions, and asking follow-up questions that help the user reach their own clarity. At the end of the conversation, the agent should guide the user to articulate their own solution or next steps in their own words. Finally, the agent summarizes the journey: the core problem, key insights discovered, and the solution the user has chosen to act on.

The learner’s task is to create a **single, reusable AI agent prompt** that consistently produces this behavior across different problem domains (career, business, productivity, relationships, decision-making, etc.).

---

### Success Criteria

* The agent asks **open-ended, non-leading questions**
* The agent **does not give direct advice or solutions**
* Each question logically builds on the previous answer
* The agent reflects and reframes the user’s responses before probing deeper
* The final output includes:

  * A clear articulation of the core problem
  * The user-derived solution or decision
  * A concise summary written in the user’s own language

---

### Sample Success Flow (Illustrative)

**User:**
“I feel stuck in my career and I’m not sure what to do next.”

**Agent:**

1. “When you say *stuck*, what specifically feels blocked right now: growth, learning, money, recognition, or something else?”
2. “Can you describe a recent moment that made this feeling of being stuck very clear to you?”
3. “What have you already tried to change this situation, and what happened as a result?”
4. “If this situation stayed exactly the same for the next 12 months, what would concern you the most?”
5. “What does an ideal outcome look like for you if this problem were resolved?”

**Agent (Closing):**
“Based on what you shared, here is how I understand your situation…”
“Can you summarize, in your own words, the action or decision you want to take next?”

**Final Summary (Agent):**

* Core problem (as stated by the user)
* Key realizations uncovered during the conversation
* The solution or next step chosen by the user

This project evaluates your ability to design *thinking behavior*, not just generate responses.
