# REPORT — Service Ticket Data Pipeline

**Student:** Mohamed Jlidi (a22601339)  
**Course:** Applied Programming for AI — Mini-project 1

## Diagnosis
The unchanged synthetic input contains **84 rows and 6 columns**. Each row is one service-ticket observation. The initial examination inspected the column types, missing values, exact copies, and repeated ticket identifiers. A ticket ID should identify one unique ticket, but repeated IDs can have conflicting data.

## Rejection policy
I removed **3 extra exact copies**. I then rejected **4 rows** whose ID was missing or malformed, whose ID conflicted with another remaining record, or whose date was missing, invalid, or outside 1–31 October 2026. When two rows shared a conflicting ID, I rejected both. Every rejected source row was preserved with all applicable reasons in `rejected_tickets.csv`.

## Cleaning and features
I kept **77 trustworthy ticket rows**. Category values were stripped and lowercased, while unrecognized or missing categories became `unknown` with flags (2 channel, 2 priority). Duration values may be zero or fractional, but cannot be negative, infinite, or unparseable. I replaced **3 invalid/missing durations** with the median **59.00 minutes** of valid retained durations. The extra source flags distinguish missing values from present-but-invalid values. The **3 unavailable satisfaction ratings** stayed missing because filling them would invent customer opinions. I created a duration z-score with population standard deviation and four one-hot channel indicators.

## Checks, interpretation, limitations
All assertions passed: **84 = 3 + 4 + 77**. IDs are unique, dates and durations are valid, each feature row matches the clean table, and exactly one channel indicator is active. Tests also covered constant durations, duplicates, conflicting IDs, date errors, fractional inputs, and the missing-median error.

**Limitation 1:** This small synthetic dataset does not represent real support-service patterns. **Limitation 2:** Replacing invalid durations with a median reduces variation and can hide unusual cases. For a future predictive model, normalization statistics must be learned only from the training set to avoid data leakage.
