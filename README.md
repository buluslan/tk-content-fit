[English](README.en.md) | 简体中文

<div align="center">

<img src="assets/banner.png" alt="tk-content-fit" width="100%">

# 🎯 tk-content-fit

**把亚马逊爆品 Listing,翻译成 TikTok 内容策略,四维验证值不值得做**

**想了解更多最新AI行业动态,AI+电商/广告的行业实践方法,人与AI如何协作共生的思考,请关注公众号:【新西楼.AI】**

![qrcode_for_gh_e3b954bd3859_258](https://github.com/user-attachments/assets/d8f068d9-c4f8-46c7-914c-fbcab5d52f2a)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-SKILL-blueviolet.svg)](https://docs.anthropic.com/en/docs/claude-code)
[![Version](https://img.shields.io/badge/version-0.4.0-black.svg)]()

**零依赖 · 四维验证 · 分层决策 · 用户掌舵**

**Created By Buluu@新西楼.AI**

</div>

## 项目简介

一个 **Agent 通用**的 Skill:输入一个亚马逊爆品 Listing,输出 TikTok 内容推广的可行性判断 + 内容方向 + 达人画像 + 最小测试计划。

- **Agent 通用**:适配任意 Agent(Claude Code / Codex / Cursor 等),装上即按 6 步流程验证
- **四维验证**:商品需求 / 内容可演示性 / 内容竞争 / 达人可获取性,基于本次搜索结果分层判断
- **零依赖**:不装任何 MCP 全程手填也能跑;配了达人精灵/卖家精灵则自动取数(两条通路平等,手填是 first-class 不是兜底)
- **用户掌舵**:每层出「层小结卡」让你决定停/进,skill 只给证据和建议,不替你拍板

## ✨ 它做什么

把"这个亚马逊爆品转 TikTok 值不值得做"转成可落地的四维判断 + 行动方案:

| 能力 | 说明 |
|---|---|
| Listing → TK 翻译 | 亚马逊标题词翻译成本地化搜索词(6 类词 + 召回质量校验闭环) |
| 四维验证 | 商品需求 / 内容可演示 / 内容竞争 / 达人可获取,分层(L1/L2/L3)采信号 |
| 档位判定 | 值得测试 / 有条件测试 / 证据不足(区分"验了信号弱"与"没验全") |
| 机会类型定调 | 品类红利 / 差异化 / 内容驱动 / 达人撬动,定主+辅抓手 |
| 内容方向 | ≥3 个优先方向 + 不建议方向(+ 进 L3 则补脚本骨架) |
| 达人画像 | 内容参考组 + 首轮合作组(硬约束:只列有联系方式的) |
| 最小测试计划 | 按机会类型自适应配比(角度×达人×周期 + 放大/停止判据) |

**6 步流程**:预筛 → 需求锚定 → 翻译 → 三层验证 → 判断定调 → 方案定制

## 🚫 它不做什么

- ❌ 不做成本/毛利/佣金/库存/广告/转化诊断(只验证内容推广可行性)
- ❌ 不替你拍板(go/no-go 由你定,skill 只给证据和建议)
- ❌ 不做全市场断言(数字基于本次搜索结果,非 TK 全市场口径)
- ❌ 不生成视频本身(输出是内容方向 + 达人 + 测试计划,不是视频文件)

## 🚀 快速开始

**安装**(Claude Code skills 路径;Codex / Cursor 用户把 SKILL.md 当指令喂给 agent 即可):

```bash
# 个人级(所有项目可用)
git clone https://github.com/buluslan/tk-content-fit.git ~/.claude/skills/tk-content-fit
```

**最小输入示例**:

```
验证下这个亚马逊爆品转 TK:https://www.amazon.com/dp/B0XXXXXXXX
```

Agent 会反问补全(目标市场 / 竞品 / 已知 TK 情况),或你照 SKILL.md「输入」一次给全跳过反问。每层跑完出「层小结卡」,停 / 进你定。

## 三条铁律

1. **数据可替换** —— 每个模块两条通路(MCP 自动取 / 用户手填),付费数据源是可选增强,没会员也能跑
2. **分层展开** —— 浅→深,每层是决策点;最贵的字幕拆解(L3)默认不触发,放最深层
3. **用户掌舵** —— 关键决策(早停/继续、深不深、go/no-go)给建议,拍板权在你

## 工作流骨架(0–5 步)

| 步 | 做什么 | 详细文档 |
|----|--------|---------|
| 0 预筛 | 品类属性 + 可拍性初判 | [`references/listing-frontload.md`](references/listing-frontload.md) §0 |
| 1 需求锚定 | Listing → 商品理解卡 | 同上 §1 |
| 2 翻译 | 亚马逊词 → TK 本地化搜索词(含召回校验) | 同上 §2 |
| 3 三层验证 | L1 存在 → L2 竞争达人 → L3 脚本(可选) | [`references/tk-validation.md`](references/tk-validation.md) |
| 4 判断定调 | 四维判读 + 档位 + 机会类型 | 同上 §4 |
| 5 方案定制 | 内容方向 + 达人画像 + 测试计划 | [`references/output-blueprint.md`](references/output-blueprint.md) |

## 数据源

**推荐接入 [达人精灵 KOLSprite](https://kolsprite.com) + [卖家精灵 SellerSprite](https://sellersprite.com) MCP** —— 数据更全更准,skill 内置完整调取 SOP。**不接入也完全能跑**(零依赖,全程手填)。

| 数据侧 | 接入 MCP 自动取(推荐) | 手填(零依赖也能跑) |
|--------|------------------------|----------------------------|
| **亚马逊侧** | 卖家精灵:Listing 详情 + 销量/BSR 预测 + 竞品 + 评论(UGC 聚类) | Listing 链接/标题+五点/截图(**必给输入**)+ 自带数据 |
| **TK 侧** | 达人精灵:商品/视频/达人/店铺/字幕 | TK 同类链接 + 自贴字幕 |

> 亚马逊 Listing 是 skill 的**主输入**,你可以使用其他ERP的数据源,或者使用你 Agent 内置的联网功能、浏览器自动化来获取 Listing 内容。

## 🎁 接入福利

接入这两个 MCP 数据源时,用以下 buluslan(公众号:新西楼.AI)专属优惠码享折扣(下单时在「折扣券」处粘贴对应码):

| 工具 | 优惠码 | 折扣 | 购买链接 |
|------|--------|------|----------|
| 卖家精灵 · MCP | `XXL` | 9 折 | [open.sellersprite.com/pricing/mcp](https://open.sellersprite.com/pricing/mcp) |
| 达人精灵 · 会员 + MCP | `XXL` | 9 折 | [kolsprite.com/price](https://www.kolsprite.com/price) |
| 卖家精灵 · 会员 | `XXL90`(包月) / `XXL72`(单人包年) / `XXL78`(标准/高级/VIP 包年) | 见官网对应套餐 | [sellersprite.com/cn/price](https://www.sellersprite.com/cn/price) |

> 达人精灵还有 **7 天免费试用**:[kolsprite.com/?utm_source=XXL](https://www.kolsprite.com/?utm_source=XXL)

<div align="center">
<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/c46f6725-4ebd-49a0-a6be-a4fc910465be" alt="卖家精灵优惠" width="280"></td>
    <td><img src="https://github.com/user-attachments/assets/f691e661-315c-439b-bff1-3d38ae223c66" alt="达人精灵优惠" width="280"></td>
  </tr>
  <tr>
    <td align="center"><b>卖家精灵</b></td>
    <td align="center"><b>达人精灵</b></td>
  </tr>
</table>
</div>

## 🏠 交流社区

<div align="center">

🎯 **更多 AI 实战教程和专属福利尽在我们「MBG 跨境AI实战圈」,已有 50+ 跨境大卖、AI 专家热聊中**

—— 欢迎跨境电商从业者加入我们,一起探索 AI+商业的最佳实践和真实边界,跑通【跨境AI】的从 0 到 1,打败你的同事,干掉你的老板。

**社区介绍:[mp.weixin.qq.com/s/dOz4fLmRnaFR7sD_TQm00Q](https://mp.weixin.qq.com/s/dOz4fLmRnaFR7sD_TQm00Q)**

<img width="1125" height="618" alt="image" src="https://github.com/user-attachments/assets/20f47cd6-e33c-4f3e-9362-3846c11135fd" />

</div>

## 📁 结构

```
tk-content-fit/
├── SKILL.md                      # 心脏:路由 + 通用规则 + 0-5 步骨架
├── LICENSE                       # MIT
├── README.md                     # 本文件
├── assets/
│   └── banner.png                # README 横幅
└── references/
    ├── listing-frontload.md      # 第 0-2 步:预筛/需求锚定/翻译
    ├── tk-validation.md          # 第 3-4 步:三层验证 + 判断定调
    ├── output-blueprint.md       # 第 5 步:方案三件套
    └── mcp-reference.md          # MCP 工具调用手册(配了 MCP 才读)
```

## 📜 License

[MIT](LICENSE) · Copyright (c) 2026 Buluu@新西楼.AI

## 📖 写在最后

<div align="center">

**如果这个工具帮到了你,欢迎 ⭐ Star 支持。更多 AI × 跨境电商实操内容,关注公众号「新西楼.AI」。**

</div>
