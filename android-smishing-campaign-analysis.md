# MALWARE INCIDENT ANALYSIS REPORT  
## Smishing-based Android Malware Campaign (foto.apk)

## Disclaimer

This repository is created strictly for **educational and threat intelligence purposes**.

- No malware binaries, exploits, or weaponized code are included.
- Indicators of Compromise (IOCs) are shared solely to support **defensive detection and mitigation**.
- This content must not be used for malicious activity.
- Author attribution is intentionally omitted for operational security reasons.

---

## 1. Executive Summary

This report documents a **smishing-based Android malware campaign** observed targeting users in **Türkiye**.  
The campaign leverages **SMS-based social engineering**, **URL shorteners**, and **trusted infrastructure abuse (GitHub Raw)** to distribute a malicious Android APK (`foto.apk`).

> **Note:**  
> This repository does **not** include in-depth malware reverse engineering.  
> Static and dynamic malware analysis will be conducted and published in a **separate follow-up report**.

---

## 2. Incident Overview

- **Initial Vector:** SMS (Smishing)
- **Target Platform:** Android
- **Distribution Method:** URL shortener → redirect → APK download
- **Primary Goal:** Unauthorized APK installation & SMS-based propagation
- **Geographical Focus:** Türkiye

### Example Smishing Message
Zararlı bağlantıdır, tıklarken dikkatli olunuz.
Bu fotoğraftaki sen misin?
hxxps://jpeg[.]ly/O7OLs


---

## 3. Attack Chain / Campaign Flow

User interaction with the malicious link initiates the infection chain.

1. Victim receives an SMS containing a shortened URL.
2. The URL shortener resolves and performs **platform-based redirection**:
   - **Desktop / Non-Android devices:** Benign content (e.g., social media pages)
   - **Android devices:** Malicious APK delivery
3. The user is socially engineered to:
   - Enable installation from **unknown sources**
   - Approve requested permissions
4. The malicious APK (`foto.apk`) is installed.
5. The malware initiates **SMS-based propagation**.

---

## 4. Indicators of Compromise (IOC)

Only **high-confidence malicious indicators** are listed.  
Benign or sandbox-related noise has been intentionally excluded.

### 🔴 Malicious URLs

- `hxxps://jpeg[.]ly/O7OLs`
- `hxxps://guncelspor[.]shop/861398`
- `hxxps://raw[.]githubusercontent[.]com/.../foto.apk` *(inactive)*

### 🔴 Domains

- `jpeg[.]ly`
- `guncelspor[.]shop`
- `raw[.]githubusercontent[.]com` *(abused for payload hosting)*

### 🔴 IP Addresses

- `193.111.77[.]77` *(guncelspor[.]shop)*

### 🔴 Malicious File Hashes (APK Payload)

- **SHA256:** `e42373663f31fdcc9d30d2faec63a8d18bac92e25b21487f8a6c15de7354444e`
- **SHA256:** `7f2ea7272cc19c6f05553a55752a3a4739d78cc7a8b68fa7fecc92cafc4d2000`

---

## 5. Payload Hosting Analysis

The secondary payload (`foto.apk`) was hosted on GitHub, abusing raw content delivery mechanisms.

### Key Observations

- Abuse of trusted infrastructure significantly increases delivery success
- Helps bypass naive reputation-based filtering mechanisms
- Enables rapid re-hosting and infrastructure redundancy
- Requires **platform-level takedown**, not only domain or IP blocking

---

## 6. Dynamic Behavior Summary

### ANY.RUN Sandbox Findings

#### Dropped Files

- `base.4299.tmp` → **SHA256:** `e42373663f31fdcc...`
- `base.4373.tmp` → **SHA256:** `7f2ea7272cc19c6f...`

#### DNS Requests

- `guncelspor[.]shop`
- `jpeg[.]ly`
- `raw[.]githubusercontent[.]com`

#### HTTP / HTTPS Requests

- `hxxps://guncelspor[.]shop/861398`
- `hxxps://raw[.]githubusercontent[.]com/.../foto.apk`

> Google-related endpoints observed during analysis were identified as **benign sandbox artifacts**.

#### Network Connections

- `193.111.77[.]77` – **Malicious**
- `188.114.96[.]3` – Cloudflare (**Benign**)
- Various Google IP ranges (**Benign**)

---

## 7. Observed Malware Capabilities (High-Level)

> Detailed reverse engineering is **out of scope** for this report.

Based on dynamic analysis and behavioral observation:

- Android-targeted APK payload
- Social engineering-driven permission acquisition
- SMS sending and **SMS-based propagation**
- Likely abuse of the Android permission model

### Suspected Permissions

- `SEND_SMS`
- `RECEIVE_SMS`
- `READ_PHONE_STATE`
- `WRITE_EXTERNAL_STORAGE`

---

## 8. Attribution & Infrastructure Notes

- **Targeting:** Türkiye
- **Initial Vector:** SMS (Smishing)
- **Redirection Logic:** Platform-aware
- **Payload Hosting:** GitHub Raw *(abused)*
- **WHOIS Information:** Privacy protected

No direct threat actor attribution is possible at this stage.

---

## 9. Impact & Risk Assessment

| Impact Area          | Risk Level |
|----------------------|------------|
| User privacy         | High       |
| SMS spam propagation | Medium     |
| Financial fraud      | Potential  |
| Device compromise    | Medium     |

**Overall Risk Level:**  
**High – Active Android-targeted smishing campaign**

---

## 10. Recommended Mitigations

### Telecom Operators / National CERT (e.g., USOM)

- Block listed URLs, domains, IP addresses, and file hashes
- Initiate takedown requests for GitHub-hosted payloads
- Monitor for similar SMS-based infection patterns

### Enterprise / SOC Teams

- Update MDM / EDR detection and prevention rules
- Alert on APK sideloading attempts
- Educate users about the risks of shortened URLs

### End Users

- Do not click links received via unsolicited SMS messages
- Disable APK installation from unknown sources
- Remove suspicious applications immediately

---

## 11. Future Work

The following activities are **planned but not included** in this repository:

- Full static analysis of `foto.apk`
- `AndroidManifest.xml` permission analysis
- Native library (`.so`) reverse engineering
- Behavioral analysis using **Frida**
- C2 communication and command analysis

These findings will be published in a **separate malware analysis report**.

---

## 12. References & Appendices

### A. ANY.RUN Report

- `hxxps://app.any.run/tasks/8fd0f124-1518-496c-a329-b4506729a710`

