# Signal Definitions

This file documents key fraud detection signals from the Fraud Signal Library.
Each signal includes its category, description, risk rationale, detection example, and severity.

---

## action_velocity_high

**Category:** Behavioral

**Description:**
Agent or user performs actions at an unusually high frequency relative to normal workflows.

**Risk rationale:**
High action velocity may indicate automation abuse, scripted agents, or compromised sessions executing rapid operations to maximize impact before detection.

**Detection example:**
`actions_per_minute > 60` over a 5–10 minute window, adjusted for product baseline.

**Severity:** Medium

---

## identity_age_new

**Category:** Identity

**Description:**
Account, agent identity, or credential set was created very recently relative to the time of risky activity.

**Risk rationale:**
Fresh identities are often used to test stolen cards, abuse free trials, or perform high-risk actions before reputation can be established.

**Detection example:**
`account_age_days < 3` AND `risky_event == true`

**Severity:** Medium

---

## identity_revoked

**Category:** Identity

**Description:**
The identity or credential being used has been previously flagged, suspended, or revoked.

**Risk rationale:**
Revoked identities attempting re-entry are a strong indicator of persistent bad actors cycling through accounts.

**Detection example:**
`identity_status == revoked` AND `login_attempt == true`

**Severity:** High

---

## delegation_depth_high

**Category:** Delegation

**Description:**
Tasks or permissions are passed through an unusually long chain of agents, tools, or services.

**Risk rationale:**
Deep delegation chains can indicate attempts to obscure origin, bypass controls, or fragment responsibility across multiple agents.

**Detection example:**
`delegation_depth >= 4` for a single critical workflow.

**Severity:** High

---

## privilege_escalation

**Category:** Delegation

**Description:**
An agent or user attempts to acquire permissions beyond those initially granted.

**Risk rationale:**
Privilege escalation is a core attacker technique used to move from limited access to full control of resources or accounts.

**Detection example:**
`permission_requested > permission_granted_at_onboarding`

**Severity:** Critical

---

## shared_credentials

**Category:** Coordination

**Description:**
Multiple agents or accounts operate using the same credential set, token, or secret.

**Risk rationale:**
Shared credentials are common in account farms, call-center style abuse, and internal policy violations.

**Detection example:**
`distinct_identities_using_same_credential >= 3` within 24 hours.

**Severity:** High

---

## coordinated_agent_activity

**Category:** Coordination

**Description:**
Multiple agents perform similar actions against related entities in a tight time window.

**Risk rationale:**
Coordinated behavior often signals organized abuse: scripted attacks, farm operations, or collusion between entities.

**Detection example:**
`similar_action_fingerprint` across `>= 5` agents within 10 minutes against related accounts or resources.

**Severity:** High

---

## tor_exit_node

**Category:** Network

**Description:**
Request originates from a known Tor exit node IP address.

**Risk rationale:**
Tor usage is commonly associated with anonymization of fraudulent or abusive activity, including account takeover and payment fraud.

**Detection example:**
`ip_address IN tor_exit_node_list`

**Severity:** High

---

## vpn_proxy_detected

**Category:** Network

**Description:**
Request originates from a known VPN, proxy, or anonymization service.

**Risk rationale:**
VPN/proxy usage can indicate identity concealment, geo-restriction bypass, or coordination of multi-account abuse.

**Detection example:**
`ip_type IN [vpn, proxy, datacenter]` AND `user_declared_location != ip_geolocated_location`

**Severity:** Medium

---

## financial_action_no_approval

**Category:** Financial

**Description:**
A financial action (payment, transfer, withdrawal) is initiated without the expected approval or authorization step.

**Risk rationale:**
Skipping approval steps is a common pattern in internal fraud, agent manipulation, and compromised workflow abuse.

**Detection example:**
`transaction_type == financial` AND `approval_status == missing`

**Severity:** Critical

---

## abnormal_payment_frequency

**Category:** Financial

**Description:**
Payment attempts occur at a frequency that is statistically anomalous relative to the account's historical baseline.

**Risk rationale:**
High payment frequency can indicate card testing, automated payout abuse, or mule account activity.

**Detection example:**
`payment_attempts_per_hour > baseline_mean + (3 * baseline_stddev)`

**Severity:** High

---

*Signal definitions are updated based on real investigation patterns. See individual category files for full signal catalogs.*

[Back to README](./README.md)
