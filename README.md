# Fraud Signal Library

> A practitioner-built reference library of fraud detection signals used in fraud intelligence, risk architecture, and investigation workflows.

Maintained by [Gururaj G J](https://www.linkedin.com/in/gururaj-gj-52a062b4) — Fraud Intelligence Specialist | Founder, [Zarelva](https://zarelva.com)

---

## Overview

Fraud rarely operates within a single dimension. Attackers correlate activity across devices, networks, identities, and timing patterns to evade detection.

This library documents the **signals** fraud and risk teams should monitor to identify coordinated abuse, multi-account fraud, payment fraud, and identity manipulation.

---

## Fraud Detection Architecture

This library is organized to mirror how a layered fraud detection system actually works — signals flow upward from raw data through correlation into actionable investigation:

```
┌────────────────────┐
│   Device Signals    │  ← Emulator detection, device sharing, ID cycling
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│  Network Signals   │  ← VPN/proxy, datacenter IPs, IP velocity
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│ Behavioral Signals │  ← Session anomalies, credential stuffing, ATO patterns
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│Transaction Signals │  ← Card testing, chargeback abuse, mule patterns
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│  Signal Correlation │  ← Patterns that combine signals into typologies
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│  Risk Scoring Engine│  ← Weighted signal scoring + rule engine
└─────────┬─────────┘
          │
          ▼
┌────────────────────┐
│Investigation Workflow│ ← Case management, evidence, escalation
└────────────────────┘
```

> **Key Principle:** No single signal is fraud. Correlation across multiple layers is how real fraud detection systems separate genuine abuse from noise.

---

## Signal Categories

| File | Description |
|------|-------------|
| [device-signals.md](./device-signals.md) | Device fingerprint anomalies, emulator signals, hardware inconsistencies |
| [network-signals.md](./network-signals.md) | IP, proxy, VPN, and ASN-based risk indicators |
| [behavioral-signals.md](./behavioral-signals.md) | Session patterns, velocity, timing, and interaction anomalies |
| [transaction-signals.md](./transaction-signals.md) | Payment fraud patterns, card abuse, chargeback signals |
| [fraud-signal-correlation.md](./fraud-signal-correlation.md) | How signals combine into fraud typologies: account farms, ATO, mule clusters, synthetic identity |

---

## How to Use This Library

Each signal file contains:
- **Signal name** — what it is
- **Why it matters** — fraud context and attacker behavior
- **Detection approach** — how to surface it in your systems
- **Risk level** — Low / Medium / High / Critical

The correlation file documents how signals **combine** into recognizable fraud typologies, with a quick-reference matrix and 7 fully documented attack patterns.

This library is intended for:
- Fraud analysts building detection rules
- Risk architects designing fraud frameworks
- Trust & Safety teams reviewing platform abuse
- Fintech teams scaling faster than their risk controls

---

## About the Author

6+ years investigating financial crime across Amazon, Google, Flipkart, and G2 Risk Solutions. Founder of Zarelva, a fraud intelligence and risk architecture consulting initiative.

**Connect:** [LinkedIn](https://www.linkedin.com/in/gururaj-gj-52a062b4) | [Portfolio](https://gururaj-gj.github.io/profile-hub/) | [Zarelva](https://zarelva.com)

---

*This library is a living document. Signals are updated based on real investigation patterns.*
