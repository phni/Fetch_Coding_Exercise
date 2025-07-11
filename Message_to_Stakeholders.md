# 🧾 Data Quality Review: Fetch Rewards Analytics Engineering Exercise : Slack/eMail Message to Stakeholders

## TL;DR

The datasets are valuable, but currently have data quality issues that could affect reporting and downstream analysis. Most are fixable with better validation and some business context. Below are the top highlights and open questions.

---

## ✅ Top Highlights

- **Users:** 70 duplicate users, missing records in `lastLogin` `state` `sign up source` 
  - _Impact: Skewed engagement and retention metrics._
- **Brands:** 429 test brand names, duplicate barcodes across different brand IDs, brandcode has null values
  - _Impact: Product attribution and brand-level insights may be inaccurate._
- **Receipts:** 400–600 records missing key fields like `purchaseDate`, `totalSpent`, and `pointsEarned`
    - _Impact: Incomplete receipts can break metrics like average spend, reward value, and purchase behavior._
- **Items:** Over 7,000 items missing in critical fields `userFlaggedPrice` `brandcode` 
  - _Impact: Prevents accurate SKU- or category-level spend analysis._

---

## 🔍 How I Found the Issues

- Ran profiling and null checks on each dataset  
- Used deduplication logic to identify repeated records  
- Flagged outliers (e.g., test brand names) and data conflicts (e.g., barcodes mapping to multiple brands)

---

## 🤔 Key Questions

To move forward effectively, here’s what I need help clarifying:

- Are test users/brands intentionally mixed with live data, or can they be filtered out?
- Should barcodes map 1:1 with brands, or are overlaps valid? We need more clarity on brands esp. relation between brand_id, barcode and brand_code
- Are nulls expected in fields like `totalSpent`, `bonusPointsEarned`, or `itemPrice`?
- What defines a “Accepted” receipt scan, in the data we see Finished, Submitted, Rejected, Pending and Flagged?

---

## 🧠 What I Need to Move Forward

- Engineering context on how/when fields are populated (especially receipt scanning)
- More details on missing records, possible a indicator if missing in the receipt or failed to scan
- Access to detailed logs on backend will help to fill some of the gaps in the data
- Decision on whether and how to filter test/incomplete data

---

## 📌 Additional Context to Improve Optimization

- Clear business rules around reward logic and completeness
- Field-level documentation or expectations (nullable, default values, etc.)

---

## 🚀 Scaling Considerations

- **Performance:** Incomplete data adds processing complexity
- **Data trust:** Misleading or dirty data could impact key KPIs
- **Plan:** Add ingestion validation, deduplication logic, and quality flags to filter low-confidence records

---

Let me know if you'd like to walk through any of this or align on what to prioritize.

— *Phani*

## 📊 Full Findings

### Users
- 70 duplicate records  
- 62 users missing `lastLogin`  
- 56 missing `state`  
- 48 missing `sign_up_source`

### Brands
- Duplicate barcodes tied to different brand + name combos  
- Inconsistent `brandCode` vs. `brand_id` mapping  
- 429 test brand names present

### Receipts
- 448 missing `purchaseDate`  
- 435 missing `totalSpent`  
- 510 missing `pointsEarned`  
- 575 missing both `bonusPointsEarned` and `bonusPointsEarnedReason`  
- 551 missing `finishedDate`  
- 484 missing `purchasedItemCount`  
- 582 missing `pointsAwardedDate`

### Receipt Items
- 440 missing `brandCode`  
- 614 missing `itemPrice` and `finalPrice`  
- 7,082 missing `userFlaggedPrice`

_For detailed outputs and code, see `notebook.ipynb`._

---


