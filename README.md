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

3. **Ingestion:** The tool of choice for this project is purely Microsoft Excel. Utilizing the power of Power Query using the **Get Data** features, I was able to bring in the data via this whichis where both the cleaning and transfromation was done.
   
5. **Cleaning:** SIX dataset... Any issues? Of course. First we dig into the Loan table, Checked for duplicates and there is! 100 duplicates, using hthe Remove features.. It brough everything down to 20,000.
   
7. **Transformation:** [What new fields, aggregations, or structures did you create?]

9. **Analysis:** [What methods - statistical, visual, query-based, model-based?]

11. **Output:** [What form do the results take?]

---


## 6. Data Model & Schema

<!--
  Define your fields so that someone reading your analysis can follow along
  without digging through your code.

  WHAT GOOD LOOKS LIKE (one row example):
  | transaction_id | string | Unique identifier per sales transaction | TXN-00482 |
  | return_flag    | boolean | Whether the transaction included a return | TRUE |
  | region_code    | string | Two-letter identifier for store region | "NE" |

  WHAT TO AVOID:
  ❌ Skipping this section because "the field names are self-explanatory."
     They're not. Not to a reviewer. Not to you in six months.

  📌 FOR SQL PROJECTS: If you have multiple tables, create one block per table.
     Describe join keys and relationships here. Your ERD (Section 7) will
     visualise what this section describes in text.

  📌 FOR NON-SQL PROJECTS: Describe the shape of your dataset informally
     if a formal schema doesn't apply. Even one paragraph is more helpful than nothing.
-->

### Dataset / Table: `Loans_Data`

| Field Name | Data Type | Description | Example Value |
|------------|-----------|-------------|---------------|
| `LoanID` | string | Unique Laon Identifier for all Loan Collectors | LN-100012 |
| `MonthlyIncome` | int | Monthly Income of customers in Euros | 4770 |
| `CustomerID` | string | Customer Identifier for all customers |  |
| `BranchID` | string | [What this field represents] | [Non-sensitive example] |
| `OfficerID` | string | [What this field represents] | [Non-sensitive example] |
| `ProductID` | string | [What this field represents] | [Non-sensitive example] |
| `LoanAmount` | int | [What this field represents] | [Non-sensitive example] |
| `InterestRate` | decimal | [What this field represents] | [Non-sensitive example] |
| `CreditScoreAtOrganisation` | int| [What this field represents] | [Non-sensitive example] |
| `TermMonths` [ int | [What this field represents] | [Non-sensitive example] |
| `DisbursedDate` | date | [What this field represents] | [Non-sensitive example] |


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

