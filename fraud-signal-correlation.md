# Fraud Signal Correlation

> How individual signals combine into fraud typologies. Real fraud detection is not about single signals — it is about patterns across multiple dimensions.

Maintained by [Gururaj G J](https://www.linkedin.com/in/gururaj-gj-52a062b4) — Fraud Intelligence Specialist | Founder, [Zarelva](https://zarelva.com)

---

## Why Correlation Matters

A VPN alone is not fraud. A new account alone is not fraud. A high-value transaction alone is not fraud.

But **VPN + new account + high-value transaction in the first 60 minutes** is a very strong fraud cluster.

Fraud detection systems become significantly more accurate when they correlate signals across device, network, behavioral, and transaction dimensions simultaneously. This file documents known correlation patterns drawn from real fraud typologies.

---

## How to Read This File

Each pattern is structured as:
```
Signal A + Signal B + Signal C
→ Fraud Typology
→ Why this combination matters
→ Recommended response
```

Risk levels are cumulative — each additional correlated signal increases confidence that fraud is occurring.

---

## Correlation Patterns

---

### Pattern 1: Coordinated Account Farm

```
Device reused across accounts
+ VPN / proxy / datacenter IP
+ Account creation velocity (3+ accounts / hour)
→ Coordinated Account Farm
```

**Why this combination matters:**
No single signal here is conclusive. Many legitimate users share devices (family members). Some use VPNs. A business might create multiple accounts. But all three together indicate an organized operation creating accounts at scale to abuse promotions, referral bonuses, or platform access.

**What attackers are doing:**
Using a single device with rotating IPs to manufacture accounts at scale. The device fingerprint stays constant because resetting it is difficult; the IP rotates to avoid IP-level velocity rules.

**Recommended response:**
- Require step-up verification (OTP, document) for all accounts sharing the device fingerprint
- Shadow-suspend rather than hard-block (to avoid tipping off the attacker)
- Map the account cluster and investigate all accounts sharing the device
- **Risk level: Critical**

---

### Pattern 2: Mule Account Cluster

```
Device reused across accounts
+ Card testing pattern (micro-transactions)
+ Multiple cards added per account
→ Mule Account Cluster / Carding Operation
```

**Why this combination matters:**
A device associated with multiple accounts that are running card tests and cycling through multiple payment methods is a clear carding infrastructure signal. This is a coordinated operation testing stolen card validity at scale.

**What attackers are doing:**
Using a single device (or small fleet) to test batches of stolen card numbers across multiple accounts to evade per-account velocity limits. Each account tests a few cards so no single account triggers a threshold.

**Recommended response:**
- Block all card testing transactions immediately
- Freeze all accounts sharing the device fingerprint
- Report implicated card numbers to card networks (Visa/Mastercard fraud reporting)
- Escalate to law enforcement if transaction volume exceeds $10,000
- **Risk level: Critical**

---

### Pattern 3: Account Takeover (ATO)

```
IP velocity anomaly (impossible travel)
+ Profile change (email / phone / address updated)
+ Post-dormancy activation
→ Account Takeover (ATO)
```

**Why this combination matters:**
A dormant account suddenly accessed from a geographically impossible location, followed immediately by profile changes, is the textbook ATO pattern. The attacker has obtained credentials, logged in remotely, and is changing recovery information to lock out the legitimate owner.

**What attackers are doing:**
Using leaked credentials from a data breach to access dormant accounts. They prefer dormant accounts because they have lower fraud scores and higher trust levels. Profile changes lock out the legitimate user and convert the account for fraudulent use.

**Recommended response:**
- Force immediate re-authentication via registered phone number
- Freeze profile changes for 24 hours
- Notify the original registered email / phone about the suspicious access
- If profile changes already completed, initiate account recovery protocol
- **Risk level: Critical**

---

### Pattern 4: Bonus / Promotion Abuse

```
Multiple accounts per device
+ Shared IP across accounts
+ Account creation timing clusters (all created within same hour)
→ Bonus Abuse / Multi-Accounting
```

**Why this combination matters:**
When multiple accounts share a device, an IP, and a creation timestamp cluster, they were created by the same person or operation to exploit welcome bonuses, referral rewards, or promotional offers. This is widespread in fintech, gaming, and e-commerce.

**What attackers are doing:**
Creating multiple "identity" accounts using slight variations of personal information to receive welcome bonuses or referral payouts multiple times. Often automated with scripts.

**Recommended response:**
- Link all accounts in the cluster in your investigation system
- Claw back bonuses from all linked accounts
- Apply a shadow flag to all accounts sharing the cluster attributes
- Block the device fingerprint and IP range from future registrations
- **Risk level: High**

---

### Pattern 5: Synthetic Identity Fraud

```
Gibberish or slightly varied identity fields (name, address, DOB)
+ Disposable email domain
+ New account + high-value transaction within 24 hours
+ No prior platform history
→ Synthetic Identity Fraud
```

**Why this combination matters:**
Synthetic identities are constructed from fabricated or slightly modified real data. They exhibit no natural platform history (no browsing, no small transactions, no profile completion) but immediately attempt high-value actions. This pattern is common in payment fraud and digital lending abuse.

**What attackers are doing:**
Constructing plausible but fake identities specifically to pass KYC checks and immediately extract value. The rush to high-value transactions within 24 hours is because the fraudster knows the identity will eventually fail deeper verification.

**Recommended response:**
- Apply enhanced KYC / document verification before any transaction above threshold
- Flag accounts with no transaction history attempting high-value first transactions
- Check email domain against disposable email provider lists
- Impose a cooling period (48–72 hours) before first high-value transaction for new accounts
- **Risk level: High**

---

### Pattern 6: Money Laundering via Platform

```
Funds received from multiple sources
+ Rapid outbound transfers (>80% within 24 hours)
+ Round-number transaction amounts
+ Account with minimal prior activity
→ Money Mule / Layering Operation
```

**Why this combination matters:**
This is the classic layering pattern in AML. Funds arrive from multiple sources (placement), aggregate briefly in the mule account, and are rapidly transferred out (layering) in patterns designed to obscure the origin. Round numbers and minimal history are strong indicators of a purpose-built mule account.

**What attackers are doing:**
Using a network of recruited or compromised accounts to receive and forward funds from fraud proceeds, making the money trail difficult to trace. The mule account holder may not know they are participating in fraud.

**Recommended response:**
- File a Suspicious Activity Report (SAR) if transaction volume meets threshold
- Freeze outbound transfers pending investigation
- Identify upstream sending accounts and downstream receiving accounts
- Preserve all transaction records for regulatory reporting
- **Risk level: Critical**

---

### Pattern 7: Predatory Digital Lending App Abuse

```
Over-permissioned mobile app (contacts, camera, storage access)
+ PII harvested from device
+ Coercive collection behavior (mass messaging to contacts)
→ Predatory Digital Lending / PII Weaponization
```

**Why this combination matters:**
This pattern, increasingly common in India and Southeast Asia, involves apps that grant loans but use device permissions to harvest contact lists, photos, and personal data as de-facto collateral. Non-repayment triggers mass harassment of the victim's contacts using harvested data.

**What attackers are doing:**
Operating outside regulatory frameworks, using PII as leverage rather than legal collection. The device becomes a surveillance tool. Victims face reputational damage through morphed images and mass notifications to their contacts.

**Recommended response (Platform / Investigator perspective):**
- Identify and document all app permissions requested
- Preserve device artifacts before remediation
- Report to RBI / CERT-In / local cybercrime authorities
- Document contact-list access logs and any harvested data evidence
- **Risk level: Critical**

---

## Signal Correlation Matrix

Quick reference for combining signals across categories:

| Device Signal | Network Signal | Behavioral Signal | Transaction Signal | Typology |
|---------------|----------------|-------------------|--------------------|----------|
| Device shared across accounts | Datacenter IP / VPN | High account creation velocity | — | Account Farm |
| Device shared across accounts | — | — | Card testing + multiple cards | Carding Operation |
| — | IP velocity (impossible travel) | Post-dormancy activation | High-value first transaction | Account Takeover |
| Multiple accounts per device | Shared IP | Creation timestamp cluster | Refund / bonus abuse | Bonus Abuse |
| — | — | Gibberish identity fields | High-value transaction in first 24hrs | Synthetic Identity |
| — | — | Minimal prior activity | Multi-source inflows + rapid outflows | Money Mule |

---

## The Core Principle

> **No single signal is fraud. Correlation across signals is fraud.**

The stronger your correlation engine, the fewer false positives you generate and the more confident you can be when taking action.

Building fraud detection systems that correlate across device, network, behavioral, and transaction layers simultaneously is the foundation of modern fraud architecture.

---

*Part of the [Fraud Signal Library](./README.md) by Gururaj G J — Fraud Intelligence Specialist | Founder, [Zarelva](https://zarelva.com)*
