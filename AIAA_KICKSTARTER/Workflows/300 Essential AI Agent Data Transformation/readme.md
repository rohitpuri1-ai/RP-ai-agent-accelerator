## Section 3. Essential Data Transformation

Data transformation controls **what data flows forward** and **in what shape**.

This section teaches students how to clean, rename, filter, and limit data before it reaches tools or AI agents.

The focus is precision, not intelligence.

---

## Workflow 302. Edit Fields (Rename and Restructure Data)

### What This Workflow Does

This workflow shows how to rename and restructure incoming data fields before storing or using them.

---

### Node Sequence Explanation

1. **On Form Submission**  
   Receives raw form data with default field names.

2. **Edit Fields**  
   Renames, removes, or restructures fields to match the required schema.

3. **Google Sheets (Append Row)**  
   Stores the cleaned and renamed data in the sheet.

---

### Key Concept

Edit Fields is used to:
- Standardize field names
- Remove unwanted fields
- Prepare clean inputs for downstream nodes

---

## Workflow 303. Filter (Conditional Data Passing)

### What This Workflow Does

This workflow filters data based on defined conditions and only allows matching items to continue.

---

### Node Sequence Explanation

1. **Manual Trigger**  
   Starts the workflow for testing.

2. **Google Sheets (Get Rows)**  
   Reads rows from a spreadsheet.

3. **Filter**  
   Applies conditions and passes only rows that satisfy them.

4. **Google Sheets (Append Row)**  
   Stores only the filtered results.

---

### Key Concept

Filter nodes are used to:
- Enforce rules
- Remove unwanted data
- Control decision paths

---

## Workflow 304. Limit (Restrict Number of Outputs)

### What This Workflow Does

This workflow limits the number of items passed forward after filtering.

---

### Node Sequence Explanation

1. **Manual Trigger**  
   Starts the workflow.

2. **Google Sheets (Get Rows)**  
   Reads data from the sheet.

3. **Filter**  
   Applies selection criteria.

4. **Limit**  
   Restricts the number of output items.

5. **Gmail (Send Message)**  
   Sends only the limited results.

---

### Key Concept

Limit nodes are used to:
- Prevent overload
- Control batch size
- Send top-N results
- Optimize performance
