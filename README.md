# Operation-Threshold-Analysis-Credit-Risk-Division

[https://github.com/anuhimustee/Operation-Threshold-Analysis-Credit-Risk-Division/blob/main/visuals/VintageBank%20Snapshot.png]!

One Statement from the Head of Credit Risk set the tone for everything...
> "We're approving loans on credit score and income verification, but nobody has checked whether size-of-loan versus- income is quietly doing more damage than either. I don't want a guess. I want a number."

Debt burden — how large a loan is relative to the borrower's income — turns out to be the strongest predictor of default in this portfolio, well ahead of most of what the initial sample review suspected. A few other suspected patterns (first-payment timing, officer inconsistency) are real but weaker than they first looked, and one doesn't hold up at all. This summary says plainly which is which.

## What the Data Shows

- Debt burden predicts default. Default rate rises roughly 8x from Low DTI (**5.48%**) to Severe DTI (**43.50%**), and this number reverse-engineers correctly against the portfolio's total defaults — this is the finding to act on. (strong)
- A missed first payment rarely means default. **63%** of loans that missed their first payment went on to repay normally with no default at all (default = 3 or more missed payments). A single missed payment is a weak warning sign by itself. (strong)
- Good credit score stops protecting borrowers once debt burden is severe. At Severe DTI, Fair, Poor, and Unknown credit tiers all converge to roughly the same 43–**45%** default rate — credit score stops discriminating risk once the loan itself is oversized. (moderate)
- Defaults happen gradually more often than immediately, at every debt-burden level. **75–80%** of defaults are gradual regardless of DTI band. Higher DTI only slightly raises the chance of immediate failure — real, but a small effect, not the dramatic pattern first suspected. (moderate)
- SME loans and two branches (Manchester, Newcastle) default more than the rest of the portfolio. Worth a closer look, though a smaller effect than the DTI-band finding above. (moderate)
- The loan-officer “ranking” isn't real. Of 40 officers, only 3 have a default pattern that's statistically different from the portfolio average once sample size is accounted for. The rest of the spread you'd see in a sorted list is normal statistical noise, not a skill difference — don't act on it as a ranking. (weak — not supported)


## Recommendations

- Tighten approval criteria above ~35% DTI (the High/Severe boundary) — this is where default risk and money at risk both concentrate. _Owner: Action Required_
- Treat debt-to-income, not credit score alone, as the primary risk signal once a loan is large relative to income. **_Owner: Action Required_**
- Give SME applications and the Manchester/Newcastle branches extra review. Owner: Portfolio risk. **_Owner: Action Required_**
- Use a missed first payment as a prompt for early borrower contact, not as grounds to flag the loan as high-risk. Owner: Servicing / collections. **_Action Required_**
- Don't rank or act on individual loan officers from this data. Only 3 of 40 are statistically distinguishable from average. Owner: Risk Committee. **_Action Required_**

