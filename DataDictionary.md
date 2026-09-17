# **Data Dictionary**

> A field-level reference for the six source files in this loan-portfolio dataset. Reference (`_ref`) and dimension (`Dim_`) files are clean lookup tables; raw (`_raw`) files carry the transactional data in its original, un-cleaned state and contain deliberate quality issues (mixed formats, inconsistent casing) noted below.

### Dataset / Table: `Branches_ref.csv`
Lookup table of bank branches and the region each belongs to.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `BranchID` | string | Unique identifier for each branch; primary key linking to `Loans_raw` and `Officers_ref`. | `BR006` |
| `BranchName` | string | Human-readable name of the branch. Casing is inconsistent in the source (e.g. a leading-space, lowercase entry). | `Bristol Branch` |
| `Region` | string | Geographic region / city the branch serves. | `Bristol` |

> **Row count (approx.):** 10 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:** [e.g., `Branches.BranchID` → `Loan.BranchID`]

---

### Dataset / Table: `Officers_ref.csv`
Lookup table of loan officers, the branch they work at, and their tenure.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `OfficerID` | string | Unique identifier for each loan officer; primary key linking to `Loans_raw`. | `OF007` |
| `OfficerName` | string | Full name of the officer. Casing is inconsistent in the source (some entries fully uppercase). | `Declan Owusu` |
| `BranchID` | string | Branch the officer is assigned to; foreign key to `Branches_ref.BranchID`. | `BR007` |
| `YearsExperience` | int | Number of years of experience the officer holds. | `12` |

> **Row count (approx.):** 40 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:** [e.g., `Officers.OfficerID` → `Loan.OfficerID`]
---

### Dataset / Table: `Products_ref.csv`
Lookup table describing each loan product and its typical terms.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `ProductType` | string | Category of loan product; joins to `Loans_raw.ProductType`. | `SME` |
| `TermRangeMonths` | string | Typical range of loan terms in months, stored as a hyphenated range (text, not numeric). | `24-60` |
| `TypicalRateRange` | string | Typical interest-rate band for the product, stored as a percentage range (text, not numeric). | `6-12%` |

> **Row count (approx.):** 4 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:** [e.g., `Products.ProductID` → `Loan.ProductID`]
---

### Dataset / Table: `Dim_Calendar.csv`
Date dimension table providing calendar attributes for time-based analysis (one row per calendar day).

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `Date` | date | Calendar date; grain of the table and key for joining to date fields in fact tables. | `2023-01-01` |
| `Year` | int | Four-digit calendar year. | `2023` |
| `MonthNumber` | int | Month of the year as a number (1–12). | `1` |
| `MonthName` | string | Full name of the month. | `January` |
| `MonthYear` | string | Abbreviated month-and-year label, useful for sorting/labelling charts. | `Jan-2023` |
| `Quarter` | string | Calendar quarter the date falls in. | `Q1` |
| `Day` | int | Day of the month (1–31). | `1` |
| `DayName` | string | Full name of the weekday. | `Sunday` |
| `IsWeekend` | boolean | Flag indicating whether the date falls on a Saturday or Sunday. | `True` |

> **Row count (approx.):** 1461 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:** [e.g., `Calendar.Date` → `Loan.DisbursedDate`]
---

### Dataset / Table: `Loans_raw.csv`
Raw loan-origination records — one row per disbursed loan. Contains customer, officer, branch, and product details. This is a **raw** file: several columns hold mixed formats that require cleaning before analysis (flagged below).

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `LoanID` | string | Unique identifier for each loan; primary key linking to `Repayments_raw`. | `LN-111866` |
| `CustomerName` | string | Full name of the borrower. | `A. Customer` |
| `Age` | int | Age of the borrower in years. | `46` |
| `Gender` | string | Borrower's gender, coded as a single letter. | `M` |
| `Region` | string | Region / city associated with the borrower. | `Bristol` |
| `EmploymentSector` | string | Borrower's employment category. | `Self-Employed` |
| `MonthlyIncome` | float | Borrower's stated monthly income. | `1334.0` |
| `BranchID` | string | Branch that originated the loan; foreign key to `Branches_ref.BranchID`. | `BR006` |
| `OfficerID` | string | Officer who originated the loan; foreign key to `Officers_ref.OfficerID`. | `OF007` |
| `ProductType` | string | Loan product category; foreign key to `Products_ref.ProductType`. | `SME` |
| `LoanAmount` | string *(needs cleaning → float)* | Principal amount disbursed. Stored inconsistently — some values are plain numbers, others carry a currency symbol and thousands separator (e.g. `£10,900`), so it reads as text until cleaned. | `5000.0` |
| `InterestRate` | float | Annual interest rate applied to the loan, as a percentage. | `8.68` |
| `CreditScoreAtOrigination` | int | Borrower's credit score at the time the loan was granted. | `653` |
| `TermMonths` | int | Length of the loan in months. | `35` |
| `DisbursedDate` | string *(needs cleaning → date)* | Date the loan was paid out. Stored in mixed formats (`30/10/2024`, `2023-12-26`, `12 January 2024`), so it reads as text until standardised. | `2023-12-26` |

> **Row count (approx.):** 20,000 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:**  `LoanID is the Primary Key that connects to keys in the Branch, Officers, Product tables`
---

### Dataset / Table: `Repayments_raw.csv`
Raw repayment schedule — one row per scheduled monthly payment per loan. Joins back to `Loans_raw` on `LoanID`.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `LoanID` | string | Loan the payment belongs to; foreign key to `Loans_raw.LoanID`. | `LN-100000` |
| `PaymentMonth` | int | Sequence number of the instalment within the loan's schedule (1 = first month). | `1` |
| `ScheduledAmount` | float | Instalment amount the borrower was due to pay that month. | `148.65` |
| `AmountPaid` | float | Amount the borrower actually paid that month. | `146.96` |
| `PaymentStatus` | string | Status of the payment relative to its due date. | `On-Time` |


> **Row count (approx.):** 240,000 Rows
> **Date range:** Jan 2025 – Dec 2025
> **Key join / relationship:** [e.g., `Repayment.LoanID` → `Loan.ProductID`]

*Add additional table blocks as needed for multi-table projects.*

---
