# Product-analysis
End-to-end retail analytics project featuring a SQL star schema, data-quality checks, and an interactive Power BI dashboard (DAX/Power Query) tracking sales, inventory, and targets. Includes a grounded Microsoft Copilot Studio AI agent that answers stakeholder queries with source citations.

# Product Performance Analytics — starter dataset
Synthetic product retail data built for practice dataset so SQL, Power BI and the agent all work from the same numbers.
# Files
- Product information; dim_product.csv — 48 products, 6 categories: Jeans, Active, Tops, Dresses & Skirts, Outerwear, Shorts & Swim. Columns: product_id, product_name, category, price_usd, launch_date.
  
- Sales transaction; fact_sales.csv — 8,840 rows: daily sales, 2026-08-01 to 2026-10-31, by product × channel (Online / Store). Columns: sale_date, product_id, channel, units_sold, sales_amount, returns_units.

- Sales targets; monthly_targets.csv — 17 rows: monthly sales target by category, Aug–Oct 2026.

- Inventory performance; inventory.csv — 48 rows: stock_on_hand and reorder_point per product (as of Oct 31, 2026).

- Important business milestones; milestones.csv — 8 rows: drops, launches and promotions to explain performance moves.

## schema.sql — Databricks volume + load statements for the CSVs in this folder.
Load the CSVs with schema.sql and check the row counts above.
- In Databricks, upload the 5 CSVs to a Volume — Catalog Explorer > your catalog + schema > Create Volume, then upload dim_product.csv, fact_sales.csv, monthly_targets.csv, inventory.csv, milestones.csv
- In that new file, replace <catalog, <schema, and <volume with yours, then run it
Run Q0 at the bottom. You should still see 48 / 8,840 / 17 / 48 / 8

The 5 business questions that would be ask from the datasets
- Which categories missed target last month (Oct 2026), and by how much?
- Which products are driving growth vs. dragging it — top 10 and bottom 10 by sales, Aug vs. Oct?
- Is Online or Store driving the trend, and does the mix differ by category?
- Which products are selling through fast enough to risk a stockout? (Use inventory vs. recent daily sales.)
- Did the Sep 4 outerwear launch and the Sep 10 jean event move sales — and did the lift last?
  
# Data-quality traps — find this later
Read this list only when you've run your own DQ checks. The planted issues are:
- Monthly target is missing for Outerwear, 2026-09 (17 target rows, not 18).
- 5 rows in fact_sales have negative units_sold / sales_amount.
- 3 exact duplicate rows in fact_sales.
- Products 1002 and 1025 sold 0 units Sep 10–16 (stockout window) — check whether that's a data gap or a real stockout before you explain it.
# Notes
- Dates are Aug–Oct 2026 so YoY / time-intelligence work in Power BI has a real trend to show (fall outerwear rising, swim falling). There is no 2025 data in this set, so a true YoY comparison will need a note, not an invented prior year.
- Totals for a sanity check after loading: 8,840 sales rows; total sales in the file are about $6.52m (synthetic scale, not a real store total).
