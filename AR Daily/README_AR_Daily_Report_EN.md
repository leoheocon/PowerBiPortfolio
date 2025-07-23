
# Power BI Project – Daily Accounts Receivable Report (AR_Daily)

## Purpose of the Report

This report is built to monitor and analyze daily Accounts Receivable (AR). The main objectives include:

- Track AR balances by customer, company, region, customer group... in real time.
- Help finance and sales teams quickly identify receivables status and fluctuations.
- Ensure data consistency between the ERP system and BI reports.
- Serve as a basis for evaluating receivables collection performance and credit risk.

## Power BI Model Architecture

- **Number of data tables**: 52
- **Number of measures**: 227
- **Number of calculated columns**: 115
- **Number of calculated tables**: 0
- **Main data source**: SQL Server, dynamically generated via M Code
- **Technologies used**: M code (Power Query), DAX, Dynamic SQL

---

## Technical Highlights

### 1. Dynamic SQL processing via Power Query (M Code)

The `StrSQL_Dynamic_AR_Daily` query is the heart of this report. It is a complex M code script capable of:

- Automatically generating different SQL queries per month.
- Dynamically splitting time by Posting Date for accurate data filtering.
- Creating nested CTEs (Common Table Expressions) including:
  - Debit and credit calculation
  - Combining opening balances with monthly transactions
  - FIFO allocation logic
  - Customer join with exception filtering
  - Building monthly results and unioning them together

The complexity of this logic is equivalent to a specialized business logic module in an ERP system.

The dynamic SQL is embedded directly in Power Query, allowing:
- Avoiding hardcoded logic for each month
- Extending or adjusting logic without modifying the original report
- Full control over the data pipeline

---

### 2. Multi-dimensional DAX logic with strong context control

The report uses 227 measures ranging from basic, intermediate, to complex aggregations. Techniques include:

- `VAR`, `RETURN`, `CALCULATE`, `REMOVEFILTERS`, `ISINSCOPE`, `FILTER`, `SUMX`, `ADDCOLUMNS`, `RANKX`, `SWITCH`, `DIVIDE`...
- Deep conditional logic by time, month, and slicer selection
- Measures organized into clear groups: AR Opening, AR Change, AR Daily, Check Data, Delta, Actual, Expected, Variance...

---

### 3. Star schema optimized data structure

- Tables like `Date`, `Customer`, `Company`, `Territory` are used as clear dimension tables.
- `AR_Facts` and dynamic data tables serve as fact tables.
- Many calculated columns are used for joins and dynamic classifications.
- No calculated tables used, reducing model size and improving performance.

---

### 4. Data Security and Masking

- Sensitive information such as customer names and company codes has been encrypted or replaced.
- Original connection strings are retained in comments for logic review but do not expose system credentials.
- High flexibility for demo/test environments.

---

### 5.Integration with Power Automate

In addition to reporting and analysis, this Power BI model also serves as a data backend for Power Automate workflows.

Usage Highlights:

- Scheduled Email Automation: Key metrics and daily AR balances are extracted and sent automatically to customers, sales, and finance teams.

- Dynamic Filtering: Data is filtered by company, customer group, and other criteria in Power BI, allowing Power Automate to send tailored messages.

- Stable & Centralized Source: Instead of querying ERP directly, workflows use structured output from Power BI, reducing risk of performance issues or data mismatches.

Use Cases:

- Daily AR summaries to internal teams.

- Reminder emails to customers for overdue balances.

- Real-time alert triggers based on balance thresholds.

This design enables the AR_Daily report to function not just as a visual dashboard, but as a central operational node in the receivables communication workflow.

## Value and Potential for Expansion

- Can be used as a standard framework for AR reporting in large enterprises.
- Can be expanded to include metrics like aging reports, credit limits, payment behavior...
- Can be extended to cover Accounts Payable (AP), Inventory, Cash Flow reporting...

