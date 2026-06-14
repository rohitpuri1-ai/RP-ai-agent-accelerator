# Database Assistant Agent

## Workflow Overview

This workflow creates an AI-powered database assistant that executes SQL queries on PostgreSQL databases through natural language requests.

It demonstrates how AI agents can translate conversational queries into structured SQL operations with schema awareness.

---

## Workflow Diagram

```
Chat Trigger → AI Agent → [Tools: Model, Memory, SQL Query Executor] → Query Results
```

---

## Node Sequence

1. **When chat message received** - Receives natural language database queries from users
2. **AI Agent** - Interprets requests and generates SQL queries
   - **Google Gemini Chat Model** - Provides language understanding and SQL generation
   - **Simple Memory** - Maintains conversation context for follow-up queries
   - **Run SQL Query (executeQuery)** - Executes SQL against PostgreSQL database

---

## System Prompt

Copy and paste this into your AI Agent node:

```
You are DB assistant. You need to run queries in DB aligned with user requests. Whenever you use column names in the query enclose with double quotes. Example instead of SUM(Units) write SUM("Units")
Run custom SQL query to aggregate data and response to user.
Fetch all data to analyze it for response if needed.
Here is the schema and table definition before you create the query. 
| table_schema | table_name                     |
| ------------ | ------------------------------ |
| public       | tbl_transactions               |
| public       | YKA_Stock                      |
| public       | n8n_chat_histories_agentic_rag |
| public       | documents_google               |
| public       | n8n_chat_histories_hfn_bot1    |
| public       | tbl_customers                  |
| public       | n8n_chat_histories_l202        |
Table Definition of tbl_customers
| column_name     | data_type | is_nullable | column_default | constraint_type | referenced_table | referenced_column |
| --------------- | --------- | ----------- | -------------- | --------------- | ---------------- | ----------------- |
| Customer_ID     | bigint    | NO          | null           | null            | null             | null              |
| Customer_Name   | text      | YES         | null           | null            | null             | null              |
| Customer_Email  | text      | YES         | null           | null            | null             | null              |
| Customer_Number | text      | YES         | null           | null            | null             | null              |
| Age             | bigint    | YES         | null           | null            | null             | null              |
| Gender          | text      | YES         | null           | null            | null             | null              |
| Location        | text      | YES         | null           | null            | null             | null              |
Table Definition of tbl_transactions
| column_name      | data_type        | is_nullable | column_default | constraint_type | referenced_table | referenced_column |
| ---------------- | ---------------- | ----------- | -------------- | --------------- | ---------------- | ----------------- |
| Transaction_ID   | bigint           | NO          | null           | null            | null             | null              |
| Date_of_Purchase | text             | YES         | null           | null            | null             | null              |
| Customer_ID      | bigint           | YES         | null           | null            | null             | null              |
| Product_Category | text             | YES         | null           | null            | null             | null              |
| Product_Name     | text             | YES         | null           | null            | null             | null              |
| Units            | bigint           | YES         | null           | null            | null             | null              |
| Price            | double precision | YES         | null           | null            | null             | null              |
| Discounts        | double precision | YES         | null           | null            | null             | null              |
| Returned         | boolean          | YES         | null           | null            | null             | null              |
| Mode_of_Payment  | text             | YES         | null           | null            | null             | null              |
| Purchase_Channel | text             | YES         | null           | null            | null             | null              |
```

### Description
```
Run custom SQL queries using knowledge about Output structure to provide needed response for user request.
Use ->> operator to extract JSON data.
```

### Query from AI
```
{{ $fromAI("query","SQL query for PostgreSQL DB in Supabase") }}
```

---

## What This Workflow Does

- Translates natural language questions into SQL queries
- Executes queries against PostgreSQL database
- Understands database schema and table relationships
- Handles column name formatting (double quotes for PostgreSQL)
- Analyzes query results and provides human-readable responses
- Maintains conversation context for follow-up questions

---

## Key Features

**Schema-Aware Query Generation**
- Agent has full knowledge of available tables and columns
- Automatically applies correct column name formatting
- Understands data types and nullable constraints

**Natural Language Interface**
- Ask questions in plain English
- No SQL knowledge required from users
- Agent translates intent into optimized queries

**Data Analysis Capability**
- Fetches and analyzes results
- Provides aggregations and summaries
- Answers follow-up questions about retrieved data

---

## How It Works

**Step 1: User Request**

A user asks a database question in natural language:
- "How many customers do we have from each location?"
- "What's the total revenue from electronics in 2024?"
- "Show me all returned transactions above $100"

**Step 2: Query Generation**

The AI Agent:
1. Reviews the schema and table definitions
2. Identifies relevant tables and columns
3. Generates proper SQL with double-quoted column names
4. Optimizes query for performance

**Step 3: Query Execution**

The **Run SQL Query** tool:
- Executes the generated SQL against PostgreSQL
- Returns result set to the agent

**Step 4: Response Formatting**

The agent analyzes results and provides:
- Human-readable answer to the original question
- Data insights and patterns
- Suggestions for follow-up queries

---

## Example Interactions

**Customer Analysis:**
```
User: How many customers do we have by gender?
Agent: [Generates] SELECT "Gender", COUNT(*) FROM tbl_customers GROUP BY "Gender"
       [Returns] "We have 450 male customers and 520 female customers."
```

**Revenue Query:**
```
User: What's the total revenue from Electronics category?
Agent: [Generates] SELECT SUM("Price" * "Units") FROM tbl_transactions 
       WHERE "Product_Category" = 'Electronics'
       [Returns] "Total revenue from Electronics is $125,430.50"
```

**Follow-up Context:**
```
User: What about returned items?
Agent: [Understands context from previous query]
       [Generates] SELECT SUM("Price" * "Units") FROM tbl_transactions 
       WHERE "Product_Category" = 'Electronics' AND "Returned" = true
       [Returns] "Returned Electronics total $3,240.25"
```

---

## Critical SQL Formatting Rule

**Always Use Double Quotes for Column Names**

PostgreSQL requires proper column name formatting:

✅ Correct:
```sql
SELECT SUM("Units"), "Product_Category" 
FROM tbl_transactions 
GROUP BY "Product_Category"
```

❌ Incorrect:
```sql
SELECT SUM(Units), Product_Category 
FROM tbl_transactions 
GROUP BY Product_Category
```

The agent automatically applies this formatting rule.

---

## Database Schema

**Available Tables:**
- `tbl_customers` - Customer demographic and contact information
- `tbl_transactions` - Transaction records with products, pricing, and returns
- `YKA_Stock` - Stock/inventory data
- Chat history tables (for system use)

**Main Relationships:**
- `tbl_customers.Customer_ID` links to `tbl_transactions.Customer_ID`
- Enables customer purchase history analysis

---

## Use Cases

- Business intelligence queries without SQL knowledge
- Quick data exploration and analysis
- Customer segmentation and profiling
- Revenue and sales analytics
- Return rate analysis
- Product performance tracking
- Ad-hoc reporting for non-technical users

---

## Limitations

**Memory is Ephemeral**
- Query context only persists during the session
- For production, implement persistent conversation memory

**Schema Must Be Provided**
- Agent needs schema definition in system prompt
- Schema changes require prompt updates

**No Write Operations**
- Read-only queries for safety
- No INSERT, UPDATE, or DELETE operations

**Single Database Connection**
- Configured for one PostgreSQL database
- Multi-database queries not supported

---

## Production Considerations

To make this production-ready:

1. **Add query validation** to prevent unsafe operations
2. **Implement query caching** for common requests
3. **Set up query logging** for audit and debugging
4. **Add rate limiting** to prevent database overload
5. **Configure read replicas** for query distribution
6. **Include error handling** for connection failures and invalid queries
7. **Add user authentication** to control database access
8. **Implement row-level security** for sensitive data

---

## Security Notes

**Current Implementation:**
- Queries are read-only (SELECT statements)
- No data modification allowed
- Schema is exposed to agent

**For Production:**
- Implement query whitelisting or validation
- Add user-level permissions
- Sanitize and validate all generated SQL
- Use database views to restrict data access
- Log all queries for audit compliance

---

## Mental Model

This is not a database GUI.

**This is a conversational data analyst.**

The difference:
- SQL tools require query knowledge → Agent understands questions
- Dashboards show pre-built views → Agent answers any question
- Reports are static → Agent provides dynamic analysis

The agent translates business questions into database operations.

---

## Architecture Pattern

```
Natural Language → Schema Understanding → SQL Generation → Query Execution → Result Analysis → Human Response
```

The agent acts as an intelligent intermediary between non-technical users and structured database systems.

---

## The Core Insight

Databases speak SQL.

Humans speak questions.

This workflow translates between the two, making data accessible to everyone regardless of technical skill.

---

You now understand how to build conversational interfaces for database operations.
