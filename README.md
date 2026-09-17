# **Operation-Threshold-Analysis-Credit-Risk-Division**

> Disclaimer: Vantage Trust Bank does not exist. Every borrower, branch, and loan officer named in this piece is a synthetic name attached to synthetic data that simulated a real-li work scenario for a personal analytics case study project. Every figure in this article is real in the sense that I calculated it correctly from data. Nothing else about it is real. Think of it as a flight simulator for credit risk analytics: the instruments work, the sky doesn't exist.

![VintageBank Snapshot](visuals/VintageBank%20Snapshot.png)

One Statement from the Head of Credit Risk set the tone for everything...
> "We're approving loans on credit score and income verification, but nobody has checked whether size-of-loan versus- income is quietly doing more damage than either. I don't want a guess. I want a number."

Debt burden — how large a loan is relative to the borrower's income — turns out to be the strongest predictor of default in this portfolio, well ahead of most of what the initial sample review suspected. A few other suspected patterns (first-payment timing, officer inconsistency) are real but weaker than they first looked, and one doesn't hold up at all. This summary says plainly which is which.

---

## 1. Project Overview

**Contextual Background:** Vantage Trust Bank operates 10 branches, offering Personal, SME, Auto, and Mortgage loans to retail and small business customers through a network of 40 loan officers. The credit team approves loans primarily on credit score and income verification, but has never systematically checked whether loan size relative to income predicts
repayment failure.

**The Incident:** The Head of Credit Risk pulled a routine sample of defaulted accounts for a write-off review and noticed something informally: a disproportionate number of the defaults looked "over-borrowed" relative to what the customer earned. No one has confirmed this with actual data. The Risk Committee wants to know, before the next lending cycle's approval criteria are set, whether debt burden actually predicts default — and whether high-burden borrowers fail immediately or gradually.

One statement from the Head of Credit Risk set the tone for everything...
> "We're approving loans on credit score and income verification, but nobody has checked whether size-of-loan versus- income is quietly doing more damage than either. I don't want a guess. I want a number."

**Approach:** Vintage Trust Bank has handed me the raw unclean loan and repayment extracts covering the full 2025 portfolio. My approach is to use Excel — Power Query, Power Pivot, and DAX — to clean and model the data, calculate each loan's debt burden, and determine whether high-burden loans default more, and whether they fail immediately at first payment or gradually over time.

**Outcome:** Debt burden predicts default. Default rate rises roughly 8x from Low DTI (**5.48%**) to Severe DTI (**43.50%**), and this number reverse-engineers correctly against the portfolio's total defaults — this is the finding to act on. ***More findings can be found at the Key & Insights section***

---

## 2. Objectives
The Head of Credit Risk Management had mentioned that they are approving loans on credit score and income verification, but nobody has checked whether size of loan version income is quietly doing more damage than either. They don't want a guess.. they need a data driven fact. My goal for this project is to verified the flagged signals that has been pointed out by the Credit Risk Team and to critically evaluate if these are the signal for debt burden, loan rate and repayment shortfalls

These Flagged Signals includes;
-❗**Debt-To-Icome Skew**: Some loans may be sized far beyond what the customer's income supports

-❗**Immediate vs. Gradual Failure**: Defaults may cluster at first payment (underwriting failure) or later (reparyment stress)

-❗**Product Concentration**: Certain loan types may carry disproportionately high debt burden ratios

-❗**Branch/Officer Inconsistency**: Some branches or officers may be approving higher-burden loan than others

> 💡 *Every analysis decision in this project traces back to one of these objectives.*

---

## 3. Project Scope & Tools

### Scope

The scope of this project involves a transactio-level data that captures different entities such as the Region, Customers data, Officers, Repayments etc. The dataset is a full year loan porfolio transactions with active loands of approximately 20,000. All analysis done in this project was done using the provided data and no extract or external data was used or borrow to supported analysis. Basic customer behaviour, loan allocation principles and finance knowledge was utilized where possible during the execution of this project.
 

### Tools & Technologies

<!--
  List only what you actually used on this project.
  This is not your skills section - it's the project's technical context.
-->

| Category | Tool(s) Used |
|----------|--------------|
| Data Storage | CSV files |
| Data Processing | Microsoft Excel |
| Analysis | Power Pivot, DAX Measures, Pivot Tables |
| Visualization | Microsoft Excel |
| Documentation | Github & Medium(*For Full Report*) |

---

## 4. Data Workflow

<!--
  Show how data moved through your project - from source to output.
  Every transformation decision should be traceable here.

  WHAT GOOD LOOKS LIKE:
  1. Source: "Monthly CSV exports pulled from the internal POS system.
              Five files, one per region, covering Jan 2023–Jun 2024."
  2. Ingestion: "Loaded into Python using pandas. Files concatenated into
                 a single dataframe (approx. 340,000 rows)."
  3. Cleaning: "Removed 1.2% of rows with null transaction IDs.
                Standardised date formats across regional files.
                Resolved product category naming inconsistencies (3 variants → 1)."
  4. Transformation: "Created a returns_rate field at product-category level.
                      Aggregated to weekly and regional grain for trend analysis."
  5. Analysis: "Descriptive statistics, regional comparison, return rate
                segmentation by product category."
  6. Output: "Summary report (PDF), annotated notebook, processed CSV."

  WHAT TO AVOID:
  ❌ "Data was cleaned and analysed." (No chain. No decisions. No trust.)
-->

```
[Data Source(s)]
      ↓
[Ingestion / Collection Method]
      ↓
[Cleaning & Transformation]
      ↓
[Analysis / Modelling / Querying]
      ↓
[Output / Visualisation / Reporting]
```

1. **Source:** The data need sits on 6 solid CSV files each having its own unique stroies, attributes etc. but together tells a unified and cohorent story of what we are looking for.

2. **Ingestion:** The tool of choice for this project is purely Microsoft Excel. Utilizing the power of Power Query using the **Get Data** features, I was able to bring in the data via this whichis where both the cleaning and transfromation was done.
   
5. **Cleaning:** SIX dataset... Any issues? Of course. First we dig into the Loan table, Checked for duplicates and there is! 100 duplicates, using hthe Remove features.. It brough everything down to 20,000.
   
7. **Transformation:** [What new fields, aggregations, or structures did you create?]

9. **Analysis:** [What methods - statistical, visual, query-based, model-based?]

11. **Output:** [What form do the results take?]

---


## 6. Data Model & Schema

> A field-level reference for the six source files in this loan-portfolio dataset. Reference (`_ref`) and dimension (`Dim_`) files are clean lookup tables; raw (`_raw`) files carry the transactional data in its original, un-cleaned state and contain deliberate quality issues (mixed formats, inconsistent casing) noted below.

### Dataset / Table: `Branches_ref.csv`
Lookup table of bank branches and the region each belongs to.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `BranchID` | string | Unique identifier for each branch; primary key linking to `Loans_raw` and `Officers_ref`. | `BR006` |
| `BranchName` | string | Human-readable name of the branch. Casing is inconsistent in the source (e.g. a leading-space, lowercase entry). | `Bristol Branch` |
| `Region` | string | Geographic region / city the branch serves. | `Bristol` |

> **Row count (approx.):** [X rows]
> **Date range:** [Jan 2025] – [Dec 2025]
> **Key join / relationship:** [e.g., `branches.branch_id` → `loan.branch_Id`]

---

### Dataset / Table: `Officers_ref.csv`
Lookup table of loan officers, the branch they work at, and their tenure.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `OfficerID` | string | Unique identifier for each loan officer; primary key linking to `Loans_raw`. | `OF007` |
| `OfficerName` | string | Full name of the officer. Casing is inconsistent in the source (some entries fully uppercase). | `Declan Owusu` |
| `BranchID` | string | Branch the officer is assigned to; foreign key to `Branches_ref.BranchID`. | `BR007` |
| `YearsExperience` | int | Number of years of experience the officer holds. | `12` |

---

### Dataset / Table: `Products_ref.csv`
Lookup table describing each loan product and its typical terms.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `ProductType` | string | Category of loan product; joins to `Loans_raw.ProductType`. | `SME` |
| `TermRangeMonths` | string | Typical range of loan terms in months, stored as a hyphenated range (text, not numeric). | `24-60` |
| `TypicalRateRange` | string | Typical interest-rate band for the product, stored as a percentage range (text, not numeric). | `6-12%` |

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

---

### Dataset / Table: `epayments_raw.csv`
Raw repayment schedule — one row per scheduled monthly payment per loan. Joins back to `Loans_raw` on `LoanID`.

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `LoanID` | string | Loan the payment belongs to; foreign key to `Loans_raw.LoanID`. | `LN-100000` |
| `PaymentMonth` | int | Sequence number of the instalment within the loan's schedule (1 = first month). | `1` |
| `ScheduledAmount` | float | Instalment amount the borrower was due to pay that month. | `148.65` |
| `AmountPaid` | float | Amount the borrower actually paid that month. | `146.96` |
| `PaymentStatus` | string | Status of the payment relative to its due date. | `On-Time` |


> **Row count (approx.):** [X rows]
> **Date range:** [Start] – [End]
> **Key join / relationship:** [e.g., `orders.customer_id` → `customers.id`]

*Add additional table blocks as needed for multi-table projects.*

---






## What the Data Shows

- Debt burden predicts default. Default rate rises roughly 8x from Low DTI (**5.48%**) to Severe DTI (**43.50%**), and this number reverse-engineers correctly against the portfolio's total defaults — this is the finding to act on. (strong)

- A missed first payment rarely means default. **63%** of loans that missed their first payment went on to repay normally with no default at all (default = 3 or more missed payments). A single missed payment is a weak warning sign by itself. (strong)

- Good credit score stops protecting borrowers once debt burden is severe. At Severe DTI, Fair, Poor, and Unknown credit tiers all converge to roughly the same 43–**45%** default rate — credit score stops discriminating risk once the loan itself is oversized. (moderate)

- Defaults happen gradually more often than immediately, at every debt-burden level. **75–80%** of defaults are gradual regardless of DTI band. Higher DTI only slightly raises the chance of immediate failure — real, but a small effect, not the dramatic pattern first suspected. (moderate)

- SME loans and two branches (Manchester, Newcastle) default more than the rest of the portfolio. Worth a closer look, though a smaller effect than the DTI-band finding above. (moderate)

- The loan-officer “ranking” isn't real. Of 40 officers, only 3 have a default pattern that's statistically different from the portfolio average once sample size is accounted for. The rest of the spread you'd see in a sorted list is normal statistical noise, not a skill difference — don't act on it as a ranking. (weak — not supported)


## Recommendations

## Recommendations

- Tighten approval criteria above ~35% DTI (the High/Severe boundary) — this is where default risk and money at risk both concentrate. _Owner: Action Required_

- Treat debt-to-income, not credit score alone, as the primary risk signal once a loan is large relative to income. **_Owner: Action Required_**

- Give SME applications and the Manchester/Newcastle branches extra review. **_Owner: Action Required_**

- Use a missed first payment as a prompt for early borrower contact, not as grounds to flag the loan as high-risk. **_Owner: Action Required_**

- Don't rank or act on individual loan officers from this data. Only 3 of 40 are statistically distinguishable from average. **_Owner: Action Required_**

