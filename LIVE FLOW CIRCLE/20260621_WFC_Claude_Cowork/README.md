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

# Prompt for dealing with the inconsistencies in the data
```
There is an input folder that contains 2 CSV files:

1. inventory_costs_may_2026.csv
2. sales_may_2026.csv

Please read both files, identify data inconsistencies, fix them where possible, and clearly report what was changed.

File details:

1. inventory_costs_may_2026.csv
Expected columns:
- SKU
- Product Name
- Category
- Unit Cost

2. sales_may_2026.csv
Expected columns:
- Order ID
- Customer Name
- Product SKU
- Order Date
- Revenue

Your task:

1. Read both CSV files from the input folder.
2. Validate that the expected columns are present in each file.
3. Check for inconsistencies in both files, including:
   - Missing values
   - Duplicate rows
   - Duplicate Order IDs
   - Duplicate SKUs
   - Invalid or inconsistent date formats
   - Non-numeric values in Unit Cost or Revenue
   - Negative or zero values in Unit Cost or Revenue
   - Extra spaces before or after text values
   - Inconsistent capitalization in names, categories, or SKUs
   - Sales Product SKUs that do not exist in the inventory_costs_may_2026.csv file
   - Inventory SKUs that have no matching sales records

4. Fix issues where the correction is clear and safe, such as:
   - Trim extra spaces
   - Standardize SKU formatting
   - Standardize text capitalization where appropriate
   - Convert Unit Cost and Revenue into numeric format
   - Convert Order Date into a consistent date format
   - Remove exact duplicate rows

5. Do not guess missing values.
6. Do not delete rows unless they are exact duplicates.
7. Do not modify unclear or ambiguous values without reporting them.
8. Save the cleaned versions as new files, without overwriting the originals.

Output files to create:

1. cleaned_inventory_costs_may_2026.csv
2. cleaned_sales_may_2026.csv

Final response format:

Summary:
- Files read:
- Issues found:
- Issues fixed:
- Issues not fixed because they require manual review:

Detailed report:

inventory_costs_may_2026.csv
- Columns validated:
- Issues found:
- Fixes applied:
- Rows affected:
- Manual review needed:

sales_may_2026.csv
- Columns validated:
- Issues found:
- Fixes applied:
- Rows affected:
- Manual review needed:

Cross-file checks:
- Product SKUs in sales but missing from inventory:
- Inventory SKUs with no sales records:

Saved files:
- cleaned_inventory_costs_may_2026.csv
- cleaned_sales_may_2026.csv

Acknowledgement:
Confirm that the files were cleaned, saved as new CSV files, and that the original files were not overwritten.
```