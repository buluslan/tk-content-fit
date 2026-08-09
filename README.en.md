English | [简体中文](README.md)

<div align="center">

<img src="assets/banner.png" alt="tk-content-fit" width="100%">

# 🎯 tk-content-fit

**Translate an Amazon hit-product Listing into a TikTok content strategy — validate whether it's worth doing across four dimensions**

**For the latest AI industry trends, AI × e-commerce / advertising playbooks, and thinking on human-AI collaboration, follow the WeChat Official Account: 【新西楼.AI】**

![qrcode_for_gh_e3b954bd3859_258](https://github.com/user-attachments/assets/d8f068d9-c4f8-46c7-914c-fbcab5d52f2a)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-SKILL-blueviolet.svg)](https://docs.anthropic.com/en/docs/claude-code)
[![Version](https://img.shields.io/badge/version-0.5.1-black.svg)]()

**Zero-Dependency · 4-Dimension Validation · Layered Decisions · User at the Wheel**

**Created By Buluu@新西楼.AI**

</div>

## Overview

A general-purpose Agent Skill: feed it an Amazon hit-product Listing, get back a feasibility verdict on TikTok content promotion — plus content directions, a creator profile, and a minimum test plan.

- **Agent-native**: works with any agent (Claude Code / Codex / Cursor, etc.) — install and run the 6-step flow
- **4-dimension validation**: product demand / content demonstrability / content competition / creator accessibility, judged layer by layer from this search's results
- **Zero-dependency**: runs fully on manual input with no MCP installed; auto-fetches when KOLSprite / SellerSprite MCP is configured (both paths are first-class)
- **User at the wheel**: every layer emits a "layer summary card" so you decide stop / go — the skill only gives evidence and recommendations, never decides for you

## ✨ What it does

Turns "is this Amazon hit worth taking to TikTok?" into an actionable 4-dimension verdict + plan:

| Capability | Description |
|---|---|
| Listing → TK translation | Translate Amazon title keywords into localized search terms (6 word types + a recall-quality check loop) |
| 4-dimension validation | Product demand / content demonstrability / content competition / creator accessibility, sampled across L1/L2/L3 |
| Verdict tiering | Worth testing / Conditionally worth testing / Insufficient evidence (distinguishes "weak signal after testing" from "not yet tested enough") |
| Opportunity typing | Category dividend / Differentiation / Content-driven / Creator-leverage — main + secondary hooks |
| Content directions | ≥3 priority directions + not-recommended ones (+ script skeleton if L3 runs) |
| Creator profile | Reference group + first-round collaboration group (hard constraint: only list creators with contact info) |
| Minimum test plan | Auto-proportioned by opportunity type (angle × creator × cycle + scale-up / stop criteria) |

**6-step flow**: Pre-screen → Demand anchoring → **🚪 Confirm gate** → Translation → 3-layer validation → Verdict & typing → Plan customization (3-tier file output)

## 🚫 What it does NOT do

- ❌ No cost / margin / commission / inventory / ads / conversion diagnostics (only validates content-promotion feasibility)
- ❌ No go/no-go decisions (you decide — the skill only gives evidence and advice)
- ❌ No whole-market claims (numbers are based on this search's results, not the entire TK market)
- ❌ Does not generate the video itself (output is content direction + creators + test plan, not a video file)

## 🚀 Quick Start

**Install** (Claude Code skills path; Codex / Cursor users: feed SKILL.md to your agent as instructions):

```bash
# User-level (available across all projects)
git clone https://github.com/buluslan/tk-content-fit.git ~/.claude/skills/tk-content-fit
```

**Minimal input example**:

```
Validate this Amazon hit for TikTok: https://www.amazon.com/dp/B0XXXXXXXX
```

The agent will ask follow-ups (target market / competitors / known TK info), or you can give everything at once per the "Input" section of SKILL.md to skip the back-and-forth. After each layer you get a "layer summary card" — stop or continue, your call.

## Three Iron Rules

1. **Data is replaceable** — every module has two paths (MCP auto-fetch / manual fill); paid data sources are optional enhancements, no membership required to run
2. **Layered unfolding** — shallow → deep, each layer is a decision point; the priciest step (L3 caption teardown) stays off by default, buried deepest
3. **User at the wheel** — key decisions (early-stop / continue, how deep, go/no-go) give advice but the call is yours

## Workflow skeleton (steps 0–5)

| Step | What | Details |
|------|------|---------|
| 0 Pre-screen | Category traits + shootability first-pass | [`references/listing-frontload.md`](references/listing-frontload.md) §0 |
| 1 Demand anchoring | Listing → product understanding card | same §1 |
| **🚪 Confirm gate** | Show product card + target market; waits for your confirm before spending TK quota (hard gate) | same §1.5 |
| 2 Translation | Amazon keywords → TK localized search terms (with recall check) | same §2 |
| 3 3-layer validation | L1 existence → L2 competition & creators → L3 script (optional) | [`references/tk-validation.md`](references/tk-validation.md) |
| 4 Verdict & typing | 4-dimension read + tier + opportunity type | same §4 |
| 5 Plan customization + dump | Conclusion tier (content + creators + test plan + call cost) + analysis tier + fact-tier xlsx | [`references/output-blueprint.md`](references/output-blueprint.md) |

## Data sources

**Recommended: connect the [KOLSprite](https://kolsprite.com) + [SellerSprite](https://sellersprite.com) MCP** — fuller, more accurate data; the skill ships with a complete fetch SOP. **Fully runnable without them** (zero-dependency, all manual).

| Side | MCP auto-fetch (recommended) | Manual (zero-dependency) |
|------|------------------------------|--------------------------|
| **Amazon** | SellerSprite: Listing details + sales/BSR forecast + competitors + reviews (UGC clustering) | Listing URL / title+bullets / screenshot (**required input**) + your own data |
| **TikTok** | KOLSprite: products / videos / creators / shops / captions | TK similar links + your own captions |

> The Amazon Listing is the skill's **primary input** — you can use other ERP data sources, or your agent's built-in web access / browser automation to fetch the Listing content.

## 🎁 Sign-up Perks

Use these buluslan (WeChat OA: 新西楼.AI) exclusive discount codes when subscribing to the two MCP data sources (paste the code in the "coupon" field at checkout):

| Tool | Code | Discount | Link |
|------|------|----------|------|
| SellerSprite · MCP | `XXL` | 10% off | [open.sellersprite.com/pricing/mcp](https://open.sellersprite.com/pricing/mcp) |
| KOLSprite · Membership + MCP | `XXL` | 10% off | [kolsprite.com/price](https://www.kolsprite.com/price) |
| SellerSprite · Membership | `XXL90` (monthly) / `XXL72` (individual annual) / `XXL78` (standard/advanced/VIP annual) | See site per plan | [sellersprite.com/cn/price](https://www.sellersprite.com/cn/price) |

> KOLSprite also offers a **7-day free trial**: [kolsprite.com/?utm_source=XXL](https://www.kolsprite.com/?utm_source=XXL)

<div align="center">
<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/c46f6725-4ebd-49a0-a6be-a4fc910465be" alt="SellerSprite discount" width="280"></td>
    <td><img src="https://github.com/user-attachments/assets/f691e661-315c-439b-bff1-3d38ae223c66" alt="KOLSprite discount" width="280"></td>
  </tr>
  <tr>
    <td align="center"><b>SellerSprite</b></td>
    <td align="center"><b>KOLSprite</b></td>
  </tr>
</table>
</div>

## 🏠 Community

<div align="center">

🎯 **More AI playbooks and exclusive perks in our "MBG 跨境AI实战圈" — 50+ cross-border top sellers and AI experts already inside.**

—— Cross-border practitioners welcome. Let's explore the best practices and real boundaries of AI + business together, take cross-border AI from 0 to 1, outpace your peers, and outgrow your boss.

**Community intro: [mp.weixin.qq.com/s/dOz4fLmRnaFR7sD_TQm00Q](https://mp.weixin.qq.com/s/dOz4fLmRnaFR7sD_TQm00Q)**

<img width="1125" height="618" alt="image" src="https://github.com/user-attachments/assets/20f47cd6-e33c-4f3e-9362-3846c11135fd" />

</div>

## 📁 Structure

```
tk-content-fit/
├── SKILL.md                      # Heart: routing + general rules + 0-5 step skeleton
├── LICENSE                       # MIT
├── README.md                     # Chinese readme
├── assets/
│   └── banner.png                # README banner
└── references/
    ├── listing-frontload.md      # Steps 0-2: pre-screen / demand anchoring / 🚪confirm gate / translation
    ├── tk-validation.md          # Steps 3-4: 3-layer validation + verdict
    ├── output-blueprint.md       # Step 5: the three deliverables + 3-tier dump
    └── mcp-reference.md          # MCP tool call manual (read only if MCP is configured)
```

## 📜 License

[MIT](LICENSE) · Copyright (c) 2026 Buluu@新西楼.AI

## 📖 One more thing

<div align="center">

**If this tool helped you, a ⭐ Star means a lot. For more AI × cross-border e-commerce playbooks, follow the WeChat Official Account 「新西楼.AI」.**

</div>
