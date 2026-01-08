## Section 4. Flow Control

Flow control decides **where data should go next**.

This section teaches students how to route data based on conditions instead of processing everything the same way.

The focus is decision logic, not AI reasoning.

---

## Workflow 401. If Node (Binary Decision Routing)

### What This Workflow Does

This workflow checks whether a lead is **hot** and routes it to a different Google Sheet.

It demonstrates simple yes/no decision making.

---

### Node Sequence Explanation

1. **Manual Trigger**  
   Starts the workflow for testing.

2. **Google Sheets (Get Rows)**  
   Reads lead data from the source sheet.

3. **If Node**  
   Evaluates a condition (example. Lead Status equals Hot).  
   - **True path**. Lead is hot  
   - **False path**. Lead is not hot (warm or cold)

4. **Google Sheets (Append Row)**  
   Stores hot leads in a separate sheet.

---

### Key Concept

The If node is used when:
- Only two outcomes are required
- Simple true/false decisions are enough
- You want clean, readable logic

---

## Workflow 402. Switch Node (Multi-Condition Routing)

### What This Workflow Does

This workflow divides leads into **hot, warm, and cold** and sends each type to a separate sheet.

It demonstrates multi-path decision routing.

---

### Node Sequence Explanation

1. **Manual Trigger**  
   Starts the workflow.

2. **Google Sheets (Get Rows)**  
   Reads all lead data.

3. **Switch Node**  
   Evaluates multiple conditions based on Lead Status:
   - Hot
   - Warm
   - Cold

4. **Google Sheets (Append Row)**  
   Appends each lead to its respective sheet.

---

### Key Concept

The Switch node is used when:
- More than two outcomes exist
- Data must be split across multiple paths
- Logic needs to scale cleanly

---

### When to Use What

- **If Node**. Two outcomes only  
- **Switch Node**. Three or more outcomes

Section 4 is now complete.

Next section is **Deploy Agents**.
