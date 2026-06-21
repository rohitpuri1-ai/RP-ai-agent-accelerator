# Prompt for the Data Analysis 
```
Read the 2 CSV files inside the input folder.

The files are:

1. inventory_costs_may_2026.csv
Expected columns:
SKU, Product Name, Category, Unit Cost

2. sales_may_2026.csv
Expected columns:
Order ID, Customer Name, Product SKU, Order Date, Revenue

Your task:
- Open both CSV files.
- Read only the headers/column names.
- Compare the actual columns with the expected columns listed above.
- Show the actual columns found for each file.
- Mention whether any column is missing or extra.
- Do not clean, transform, merge, or analyze the data yet.
- End with a short acknowledgement that the files were read successfully.

Output format:

inventory_costs_may_2026.csv
Actual columns found:
- ...

Column check:
- Missing columns: None / list missing columns
- Extra columns: None / list extra columns

sales_may_2026.csv
Actual columns found:
- ...

Column check:
- Missing columns: None / list missing columns
- Extra columns: None / list extra columns

Acknowledgement:
Both files have been read and the column check is complete.
```