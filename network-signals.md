# Network Signals

> Fraud detection signals based on IP address intelligence, proxy usage, VPN detection, and network infrastructure anomalies.

---

## Overview

Network signals are critical for identifying fraud at the connection level. Fraudsters frequently obscure their true location using VPNs, proxies, Tor, and hosting provider IPs. These signals help distinguish genuine users from coordinated abuse actors.

---

## Signals

### 1. VPN / Proxy / Tor Usage
- **What it is:** User's IP address is associated with a known VPN provider, open proxy, or Tor exit node.
- **Why it matters:** Legitimate users occasionally use VPNs, but high-risk transactions from anonymizing services are a strong fraud indicator. Fraudsters use these to bypass geo-restrictions, location-based rules, and IP reputation systems.
- **Detection approach:** Cross-reference IP against commercial threat intelligence feeds (MaxMind, IPQualityScore, Spur). Score based on type: Tor > residential proxy > datacenter proxy > commercial VPN.
- **Risk level:** Medium–High (context-dependent)

---

### 2. Datacenter / Hosting Provider IP
- **What it is:** IP address belongs to a cloud hosting provider (AWS, GCP, Azure, DigitalOcean) rather than a residential or mobile ISP.
- **Why it matters:** Real users access platforms from ISP-assigned IPs. Datacenter IPs indicate automated scripts, bots, or server-based fraud operations.
- **Detection approach:** Use ASN (Autonomous System Number) classification. Flag IPs where ASN owner is a known hosting provider. Combine with user-agent analysis to confirm automation.
- **Risk level:** High

---

### 3. IP Geolocation vs. Billing Address Mismatch
- **What it is:** The IP geolocation does not match the billing address provided during a transaction.
- **Why it matters:** Fraudsters use stolen card details with billing addresses in one country while operating from another. Geographic inconsistency is a strong carding signal.
- **Detection approach:** Compare IP-derived country/city with billing address country. Flag transactions where distance exceeds 500km and no travel pattern has been established.
- **Risk level:** High

---

### 4. IP Address Shared Across Multiple Accounts
- **What it is:** Multiple distinct user accounts accessing the platform from the same IP address in a short timeframe.
- **Why it matters:** Account farms often operate from shared infrastructure. A single IP creating or logging into many accounts indicates coordinated fraud.
- **Detection approach:** Track IP-to-account associations in a rolling 24-hour window. Flag IPs with 5+ distinct account logins unless the IP is classified as a shared network (university, corporate, ISP CGNAT).
- **Risk level:** High

---

### 5. IP Velocity (Rapid IP Changes)
- **What it is:** A single user account is accessed from multiple geographically distant IP addresses in an impossibly short timeframe.
- **Why it matters:** Impossible travel patterns indicate either account takeover (ATO) or credential sharing across multiple actors.
- **Detection approach:** Calculate time between logins and distance between IP geolocations. Flag cases where the implied travel speed exceeds 900 km/h (faster than commercial aviation).
- **Risk level:** Critical

---

### 6. IP Reputation Score
- **What it is:** IP address has a history of prior abuse, fraud, or spam activity documented in threat intelligence databases.
- **Why it matters:** Fraud infrastructure is often reused. An IP used in previous fraud activity is statistically more likely to be involved in future abuse.
- **Detection approach:** Integrate with IP reputation APIs. Create a risk scoring model: clean IP (0–30), suspicious (31–60), high risk (61–80), block (81+). Adjust thresholds by transaction value.
- **Risk level:** Medium–Critical (score-dependent)

---

*Part of the [Fraud Signal Library](../README.md) by Gururaj G J — Fraud Intelligence Specialist*
