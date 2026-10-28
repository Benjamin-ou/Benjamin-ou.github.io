[README.md](https://github.com/user-attachments/files/34077112/README.md)
# Inventory health analysis: where is cash tied up?

An end-to-end analysis of 46 products from an inventory record: data checks, ABC classification, sell-through, a review list, and an interactive dashboard.

**Tools:** Python (pandas, matplotlib), SQL (SQLite), HTML and JavaScript (dashboard)
**Data:** `data/Inventory-Records-Sample-Data.xlsx`, a sample dataset with opening stock, purchases, units sold, closing stock and unit cost for 46 products.
**Live dashboard:** https://benjamin-ou.github.io/inventory-analysis/ (filter by category or review list)

![Dashboard](charts/dashboard.png)

## Business question

Inventory is worth $359,760 at cost. Which products hold most of that money, which are selling too slowly, and can the records be trusted?

## Key findings

1. **Stock is building up.** Inventory value rose 20.3% (from $298,970 to $359,760). Every one of the 46 products received more stock than it sold: 707 units bought against 314 sold, about 2.3 times as many.
2. **Sell-through is low.** Only 13.3% of available stock was sold, from 7.5% to 20% by product. Cost of units sold ($62,339) was about 19% of average inventory value, so stock turned over 0.19 times in the period.
3. **Value is concentrated.** Laptops and smartphones hold $72,000 each (40% of the total together). Five products hold 65.8%, and 11 products (the A class) hold 80.9%. Computers and displays make up 61% of value.
4. **Ten products need review.** These are A or B class products with above-average stock cover. The biggest is the Monitor: $25,000 in stock, 50 units on hand, only 5 sold.
5. **The records do not fully reconcile.** For 12 of 46 products (26%), closing stock is lower than opening stock plus purchases minus sales: 44 units, $2,200 at cost. The USB Hub is the largest gap at 12 units.

## Recommendations

- Pause or reduce reorders on the review list, starting with the Monitor, and base purchase quantities on recent sales rather than past orders.
- Protect stock of the best seller, the Smartphone (20 units sold, 20% sell-through), while cutting back on slower, high-value lines on the review list, such as the Monitor and Gaming Monitor.
- Count the 12 products that do not reconcile and find out why stock is missing before relying on these records.

## How it was done

1. `analysis.py` loads the sheet (the table starts on row 7), checks for gaps and duplicate IDs (none), and confirms that value equals closing stock times unit cost for all 46 rows.
2. It adds derived metrics: sell-through, stock cover, cost of units sold, reconciliation difference, category, ABC class and the review flag.
3. `queries.sql` holds six SQL queries, and the script checks that the SQL review list matches the pandas one.
4. It saves four charts and a dashboard image to `charts/` and the cleaned data to `data/inventory_clean.csv`.
5. `index.html` is an interactive dashboard (HTML and JavaScript).

To reproduce: `pip install pandas matplotlib openpyxl`, then `python analysis.py`.

## Definitions and assumptions

- **Sell-through** = units sold ÷ (opening stock + purchases).
- **Stock cover** = closing stock ÷ units sold, in periods of sales. The data does not say how long the period is.
- **ABC class:** A = products making up the first 80% of value, B = the next 15%, C = the last 5%.
- **Category** is my own grouping, based on product names.
- **Review list** = A or B class products whose cover is above the average of all products.

## Limitations

- This is a small sample with no dates, so there are no trends or seasonality, and the period length is unknown.
- There are no selling prices, so profit and margin cannot be calculated. All values are at cost.
- Findings show the method on sample data, not facts about a real business.

## Next steps

- Power BI version: import `data/inventory_clean.csv` and rebuild the dashboard with category and review-list slicers.
