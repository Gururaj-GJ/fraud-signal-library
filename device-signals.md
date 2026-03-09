# Device Signals

> Fraud detection signals based on device fingerprinting, hardware identifiers, and software environment anomalies.

---

## Overview

Device signals are among the most reliable fraud indicators because legitimate users typically operate from consistent, stable device environments. Fraudsters often use emulators, virtual machines, device spoofing tools, or shared infrastructure that creates detectable anomalies.

---

## Signals

### 1. Emulator / Virtual Machine Detection
- **What it is:** Device reports as an Android emulator (e.g., Genymotion, BlueStacks) or virtual machine rather than a physical device.
- **Why it matters:** Legitimate users rarely use emulators. Fraudsters use them to create unlimited accounts, bypass device bans, and run automated abuse at scale.
- **Detection approach:** Check for emulator-specific device model strings (e.g., `sdk_gphone`, `generic`), missing hardware sensors (accelerometer, gyroscope), and emulator build fingerprints.
- **Risk level:** High

---

### 2. Device ID Cycling / Reuse
- **What it is:** The same physical device reports multiple different device IDs in a short timeframe, or a previously banned device ID reappears with minor variation.
- **Why it matters:** Fraudsters reset device IDs or use device ID spoofing tools to evade bans and create fresh accounts.
- **Detection approach:** Track device ID consistency across sessions. Flag devices where hardware fingerprint matches a known banned device but the ID has changed.
- **Risk level:** Critical

---

### 3. Root / Jailbreak Detection
- **What it is:** Device has been rooted (Android) or jailbroken (iOS), allowing modification of system-level behavior.
- **Why it matters:** Rooted/jailbroken devices can bypass fraud controls, modify app behavior, intercept SSL certificates, and automate actions through scripts.
- **Detection approach:** Check for root indicators: presence of `su` binary, Magisk installations, abnormal system file permissions, SafetyNet/Play Integrity API failures.
- **Risk level:** Medium–High

---

### 4. Device Sharing Across Multiple Accounts
- **What it is:** A single device fingerprint is associated with an abnormally high number of different user accounts.
- **Why it matters:** A legitimate user typically has 1–2 accounts per device. Multiple accounts per device indicates account farming, bonus abuse, or multi-accounting fraud.
- **Detection approach:** Link device fingerprint to account registry. Flag when one device is associated with 3+ accounts within a rolling 30-day window.
- **Risk level:** High

---

### 5. Mismatched Device Metadata
- **What it is:** Device-reported metadata (OS version, screen resolution, timezone, language) is internally inconsistent or inconsistent with the user's claimed location.
- **Why it matters:** Fraudsters using spoofing tools often fail to synchronize all device parameters, creating detectable mismatches.
- **Detection approach:** Cross-check device timezone with IP geolocation. Flag devices where OS version is impossible (e.g., Android 15 on a device model released in 2018).
- **Risk level:** Medium

---

### 6. Factory Reset Patterns
- **What it is:** Device shows evidence of recent factory reset, particularly following account suspension or fraud action.
- **Why it matters:** Fraudsters factory reset devices to clear device bans and generate new device fingerprints. High-frequency resets on the same hardware are a strong abuse signal.
- **Detection approach:** Track hardware identifiers (IMEI, MAC address where accessible) across resets. Flag devices where the hardware ID is consistent but the device fingerprint changed post-action.
- **Risk level:** High

---

*Part of the [Fraud Signal Library](../README.md) by Gururaj G J — Fraud Intelligence Specialist*
