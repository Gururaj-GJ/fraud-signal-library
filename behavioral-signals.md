# Behavioral Signals

> Fraud detection signals based on user session patterns, interaction velocity, timing anomalies, and navigation behavior.

---

## Overview

Behavioral signals are difficult for fraudsters to fake at scale because they require mimicking human interaction patterns across time, context, and platform. These signals capture the *how* of platform usage, revealing bot activity, account takeovers, and coordinated abuse.

---

## Signals

### 1. Account Creation Velocity
- **What it is:** Multiple accounts created in rapid succession from the same IP, device, or email domain pattern.
- **Why it matters:** Legitimate users create one account. Account farms create dozens or hundreds. High-velocity registration is a primary multi-accounting signal.
- **Detection approach:** Track account creation rate per IP, device fingerprint, and email domain in rolling 1-hour and 24-hour windows. Flag when rate exceeds 3 accounts per hour from a single source.
- **Risk level:** High

---

### 2. Session Duration Anomalies
- **What it is:** Sessions are either extremely short (automated scripts completing actions instantly) or abnormally long without interaction.
- **Why it matters:** Human users take time to read, navigate, and interact. Bots complete transactions in milliseconds. Either extreme deviates from expected human behavior.
- **Detection approach:** Establish baseline session duration distributions. Flag sessions completing checkout in <10 seconds or >4 hours without interaction. Combine with page interaction heatmaps.
- **Risk level:** Medium–High

---

### 3. Credential Stuffing Patterns
- **What it is:** High-volume login attempts across many accounts from a single IP or coordinated IP range, with a typical success rate of 1–5%.
- **Why it matters:** Credential stuffing uses leaked username/password combinations to take over accounts. The attack pattern is distinctive: many failures, few successes.
- **Detection approach:** Monitor login failure rates per IP per hour. Flag IPs with >50 failures/hour. Track the ratio of failed-to-successful logins. Alert when success rate is in the 0.5–5% range (below random guessing, above zero).
- **Risk level:** Critical

---

### 4. Abnormal Navigation Patterns
- **What it is:** User navigates directly to high-value pages (checkout, withdrawal, profile edit) without normal browsing flow.
- **Why it matters:** Human users typically browse, search, and explore before transacting. Bots and fraudsters using automation go directly to target pages. This creates abnormal page transition sequences.
- **Detection approach:** Build page transition models from legitimate user journeys. Flag sessions where users access sensitive pages without traversing the expected preceding pages.
- **Risk level:** Medium

---

### 5. Copy-Paste vs. Typed Input
- **What it is:** Form fields (card numbers, addresses, names) are filled via paste operations rather than manual typing, especially at high speed.
- **Why it matters:** Fraudsters using stolen credentials or automated tools copy-paste stolen information. Keyboard dynamics for pasted vs. typed content differ measurably.
- **Detection approach:** Track input event types (keyboard events vs. paste events) on sensitive form fields. Flag registrations where all fields were pasted in under 3 seconds total.
- **Risk level:** Medium

---

### 6. Post-Dormancy Activation
- **What it is:** A previously inactive account (dormant for 90+ days) suddenly becomes active with high-value transactions.
- **Why it matters:** Dormant accounts are valuable targets for account takeover. Fraudsters prefer aged, verified accounts with positive history. Sudden reactivation after long dormancy is a strong ATO signal.
- **Detection approach:** Track dormancy periods per account. Flag accounts dormant for 90+ days that suddenly initiate high-value transactions, especially if combined with profile changes (email, phone, address).
- **Risk level:** High

---

*Part of the [Fraud Signal Library](../README.md) by Gururaj G J — Fraud Intelligence Specialist*
