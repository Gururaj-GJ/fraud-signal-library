# Transaction Signals

> Fraud detection signals based on payment patterns, card abuse, chargeback behavior, and financial transaction anomalies.

---

## Overview

Transaction signals sit at the intersection of behavioral and financial risk. They capture how money moves, who benefits, and whether the pattern matches the account's legitimate profile. These are the signals that directly connect to fraud losses.

---

## Signals

### 1. Card Testing / BIN Attack
- **What it is:** Multiple small-value transactions ($0.01–1.00) attempted rapidly across many card numbers to verify which are valid and active.
- **Why it matters:** Card testing is the first step before large fraudulent charges. Attackers validate stolen card numbers in bulk. The signal is high-volume micro-transactions from a single device or IP.
- **Detection approach:** Flag accounts or IPs with >5 transactions under $1.00 within 30 minutes. Monitor for sequential BIN (first 6 digits) patterns indicating bulk card testing. Implement velocity checks at the payment processor level.
- **Risk level:** Critical

---

### 2. Chargeback Rate Anomalies
- **What it is:** Account, merchant, or payment method has a chargeback rate significantly above baseline (typically >1% is a concern; >2% triggers card network scrutiny).
- **Why it matters:** High chargeback rates indicate either fraud on the account or merchant-side abuse. First-party fraud (friendly fraud) and third-party fraud both surface through chargeback patterns.
- **Detection approach:** Track chargeback-to-transaction ratios per account, per merchant, and per payment method. Segment by dispute reason code (unauthorized vs. item not received vs. not as described). Reason code 10.4 (EMV liability shift) and 4853 are highest priority.
- **Risk level:** High

---

### 3. Unusual Transaction Value Distribution
- **What it is:** Transaction amounts cluster at threshold boundaries (just below $10,000 in AML contexts; just below fraud screening limits in payments).
- **Why it matters:** Structuring is the practice of keeping transactions just below reporting or screening thresholds. It indicates deliberate evasion of controls.
- **Detection approach:** Analyze transaction value distributions. Flag accounts with abnormal clustering of amounts just below defined thresholds ($9,500–9,999 in AML; $499–499 if platform screens at $500). Correlate with account age and transaction frequency.
- **Risk level:** High

---

### 4. Multiple Cards on a Single Account
- **What it is:** A single account uses an abnormally large number of distinct payment methods (cards) over a short period.
- **Why it matters:** Fraudsters cycle through stolen card numbers. Legitimate users typically have 1–3 cards. An account using 5+ distinct cards in a week indicates card abuse or testing.
- **Detection approach:** Track unique payment methods per account per rolling 7-day period. Flag accounts adding >3 new payment methods per week. Combine with transaction success rates per card.
- **Risk level:** High

---

### 5. Refund / Reversal Abuse
- **What it is:** Account repeatedly requests refunds, chargebacks, or reversals, especially after receiving goods or services.
- **Why it matters:** Refund abuse (also called friendly fraud or first-party fraud) generates financial losses without triggering traditional fraud signals. It's especially prevalent in e-commerce and digital goods.
- **Detection approach:** Track refund request rates per account lifetime. Flag accounts where refund-to-purchase ratio exceeds 20%. Correlate with delivery confirmation data, product type (digital goods are higher risk), and account age.
- **Risk level:** Medium–High

---

### 6. Mule Account Transaction Patterns
- **What it is:** Account receives money from multiple sources and rapidly transfers out to other accounts, often in round numbers, with minimal transaction history.
- **Why it matters:** Money mule accounts are used to layer and obscure fraud proceeds. The pattern is: receive → aggregate → transfer out, with little time spent holding funds.
- **Detection approach:** Track fund flow patterns: accounts receiving from >3 sources and transferring out >80% of received funds within 24 hours. Flag round-number transfers (exact ₹10,000, $5,000) especially from new accounts with no prior history.
- **Risk level:** Critical

---

*Part of the [Fraud Signal Library](../README.md) by Gururaj G J — Fraud Intelligence Specialist*
