# Cafe Sales - Data Cleaning

Cleaning a messy transactional dataset (10,000 rows) from a fictional cafe, using pandas and numpy.

## The problem

The raw dataset (`dirty_cafe_sales.csv`) had several issues:

- Missing values across almost every column
- `"ERROR"` and `"UNKNOWN"` used as placeholder text instead of real missing values
- Numeric columns (Quantity, Price Per Unit, Total Spent) and the date column stored as text
- Inconsistent column names

## Approach

1. **Fixed data types** - converted Quantity, Price Per Unit, and Total Spent to numeric, and Transaction Date to datetime. Invalid text values became `NaN` automatically during this step.
2. **Standardized column names** and treated `"ERROR"` / `"UNKNOWN"` as missing values across the relevant columns.
3. **Recovered missing values using relationships between columns**, since each menu item has a fixed price:
   - Missing `Item` filled from `Price Per Unit`
   - Missing `Price Per Unit` filled from `Item`, then from `Total Spent / Quantity`
   - Missing `Quantity` filled from `Total Spent / Price Per Unit`
   - Missing `Total Spent` filled from `Price Per Unit * Quantity`
4. **Checked `Transaction ID` for duplicates** (none found).
5. **Payment Method and Location** had ~30% of values missing with no way to infer them from other columns. Rather than filling them with the most common value (which would badly distort the real distribution), they were labeled `"Unknown"` to keep the missingness honest.
6. **Dropped rows** that were still missing `Item`, `Quantity`, `Price Per Unit`, or `Total Spent` after all recovery steps - these couldn't be used for any revenue analysis. This affected 26 rows (0.26% of the dataset).
7. **Transaction Date** was left as missing (`NaT`) for rows where it couldn't be parsed, since the rest of the row is still valid for non-date-based analysis.

## Result

| | Before | After |
|---|---|---|
| Rows | 10,000 | 9,974 |
| Missing values (excl. Transaction Date) | thousands across all columns | 0 |
| Transaction Date missing | 460 | 460 (kept as NaT, excluded from date-based analysis) |

## Files

- `dirty_cafe_sales.csv` - original raw data
- `cleaned_cafe_sales.csv` - cleaned output
- `Data_Cleaning_Cafe_Sales.ipynb` - full cleaning process

## Tools

Python, pandas, numpy, Jupyter Notebook

## Author

Youssef Elfaramawy - [LinkedIn](https://www.linkedin.com/in/youssef-elfaramawy1/en/)
