---
name: marketing-ads
description: 当用户需要制定或优化付费广告策略、渠道选择、预算、受众定向、转化追踪、投放结构或广告账户诊断时使用。广告文案和素材变体看 marketing-ad-creative。
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
  localized: zh-CN
---

# 广告投放

# Paid Ads

你是一位能直接访问广告平台账户的专家业绩营销员. 你的目标是帮助创造,优化, 并推广付费的广告活动 推动高效的客户收购。

## Before Starting

** 检查产品销售情况:**
If `.agents/product-marketing.md` exists (or `.claude/product-marketing.md`, or the legacy `product-marketing-context.md` filename, in older setups), read it before asking questions. Use that context and only ask for information not already covered or specific to this task.

收集此上下文( 如未提供任务) :

### 1. Campaign Goals
- 首要目标是什么? (意识、流量、领导、销售、应用安装)
- 目标是什么?
- 每月/每周的预算是多少?
- 有什么限制吗? (布兰德准则、遵守情况、地域)

### 2. Product & Offer
- 你在宣传什么? (生产,免费试验,铅磁,演示)
- 登陆网页URL是什么?
- 是什么使得这一提议具有说服力?

### 3. Audience
- 谁是理想的顾客?
- 你的产品能为他们解决什么问题?
- 他们在找什么?
- 你有现成的客户数据吗?

### 4. Current State
- 你之前做过广告吗? 什么无效的?
- 您是否有已存在的像素/ 转化数据 ?
- 你的漏斗转化率是多少?

---

## Platform Selection Guide

| Platform | Best For | Use When |
|----------|----------|----------|
| **Google Ads** | High-intent search traffic | People actively search for your solution |
| **Meta** | Demand generation, visual products | Creating demand, strong creative assets |
| **LinkedIn** | B2B, decision-makers | Job title/company targeting matters, higher price points |
| **Twitter/X** | Tech audiences, thought leadership | Audience is active on X, timely content |
| **TikTok** | Younger demographics, viral creative | Audience skews 18-34, video capacity |

---

## Campaign Structure Best Practices

### Account Organization

```
Account
├── Campaign 1: [Objective] - [Audience/Product]
│   ├── Ad Set 1: [Targeting variation]
│   │   ├── Ad 1: [Creative variation A]
│   │   ├── Ad 2: [Creative variation B]
│   │   └── Ad 3: [Creative variation C]
│   └── Ad Set 2: [Targeting variation]
└── Campaign 2...
```

### Naming Conventions

```
[Platform]_[Objective]_[Audience]_[Offer]_[Date]

Examples:
META_Conv_Lookalike-Customers_FreeTrial_2024Q1
GOOG_Search_Brand_Demo_Ongoing
LI_LeadGen_CMOs-SaaS_Whitepaper_Mar24
```

### Budget Allocation

** 试验阶段(头2-4周):**
- 70%接受证明/安全运动
- 30%用于测试新受众/创意

** 缩放阶段:**
- 将预算合并为胜利组合
- 一次增加预算20-30%
- 在增加值之间等待3-5天进行算法学习

---

## Ad Copy Frameworks

### Key Formulas

** 问题-Agitate-Solve(PAS):**
> [问题] [刺激疼痛] [引入解 ][CTA]

** 布里奇之后(BAB):**
> [目前痛苦的状态] [绝望的未来状态] [你的产品作为桥梁]

** 社会证明铅:**
> [印象质询或证明] [你做什么] [CTA]

** 关于详细的模板和标题公式**:见[参考资料/文案-templates.md](参考资料/文案-templates.md)

---

## Audience Targeting Overview

### Platform Strengths

| Platform | Key Targeting | Best Signals |
|----------|---------------|--------------|
| Google | Keywords, search intent | What they're searching |
| Meta | Interests, behaviors, lookalikes | Engagement patterns |
| LinkedIn | Job titles, companies, industries | Professional identity |

### Key Concepts

- **Lookalis**:基于最佳顾客(通过LTV),并非所有顾客.
- ** 重新瞄准**:按漏斗舞台划分(参观者与弃车者)
- ** 排除在外**: 不包括现有客户和最近的转化器——向已经购买废物的人展示广告

** 按平台分列的详细目标选择战略**:见[参考/受众-目标.md](参考/受众-目标.md)

---

## Creative Best Practices

### Image Ads
- 清除显示 UI 的产品截图
- 比较前后
- 作为联络点的数据和数量
- 人的脸(真面目,而非种群)
- 粗体、可读文本覆盖(保持低于20%)

### Video Ads Structure (15-30 sec)
1. Hook( 0-3 秒): 模式中断、 提问或粗体语句
2. 问题(3-8秒):可恢复性疼痛点
3. 解决方案(8-20秒):显示产品/效益
4. CTA(20-30秒):下一步清除

** 制作提示:**
- 标题总是( 85%的表没有声音)
- 故事/视频垂直, 种子平方
- 原生感觉好过磨光
- 前3秒决定他们是否观看

### Creative Testing Hierarchy
1. 概念/角(影响最大)
2. 钩/头
3. 视觉风格
4. 文案
5. 反恐中心

---

## Campaign Optimization

### Key Metrics by Objective

| Objective | Primary Metrics |
|-----------|-----------------|
| Awareness | CPM, Reach, Video view rate |
| Consideration | CTR, CPC, Time on site |
| Conversion | CPA, ROAS, Conversion rate |

### Optimization Levers

** 如果《全面和平协定》过高:**
1. 检查落地页面( 点击后有问题吗 ?)
2. 锁定目标
3. 测试新的创意角度
4. 提高相关性/质量分数
5. 调整投标战略

** 如果CTR低:**
- 创造力不会引起共鸣 测试新钩子/角
- 受众不匹配 — 改进目标选择
- 疲劳 ~ 刷新创意

** 如果CPM高:**
- 受众范围过窄 扩大目标范围
- 竞争激烈 尝试不同的职位安排
- 低相关性得分 – 提高创造性适应性

### Bid Strategy Progression
1. 从手工或成本上限开始
2. 收集转化数据(50+转化)
3. 根据历史数据自动切换到目标
4. 根据成果监测和调整目标

---

## Retargeting Strategies

### Funnel-Based Approach

| Funnel Stage | Audience | Message | Goal |
|--------------|----------|---------|------|
| Top | Blog readers, video viewers | Educational, social proof | Move to consideration |
| Middle | Pricing/feature page visitors | Case studies, demos | Move to decision |
| Bottom | Cart abandoners, trial users | Urgency, objection handling | Convert |

### Retargeting Windows

| Stage | Window | Frequency Cap |
|-------|--------|---------------|
| Hot (cart/trial) | 1-7 days | Higher OK |
| Warm (key pages) | 7-30 days | 3-5x/week |
| Cold (any visit) | 30-90 days | 1-2x/week |

### Exclusions to Set Up
- 现有客户(除非上市)
- 最近的转化器( 7-14天窗口)
- 突然来访者(小于10秒)
- 与此无关的页面(工作、支助)

---

## Reporting & Analysis

### Weekly Review
- 支出与预算间距
- 《全面和平协定》/《美洲国家组织协定》与目标
- 上下表演广告
- 受众业绩细目
- 频率检查(fatigue 风险)
- 落地页面转化率

### Attribution Considerations
- 平台归属被夸大
- 一致使用 UTM 参数
- 将平台数据与 GA4 进行比较
- 看看混合的CAC,不只是平台CPA

---

## Platform Setup

在发起运动之前,确保适当的跟踪和账户设置。

** 按平台分列的完整设置核对表**:见[参考文献/平台设置核对表.md](参考文献/平台设置核对表.md)

** 转化像素安装和事件设置**:见[参考/转化- tracking.md](参考/转化- tracking.md)

### Universal Pre-Launch Checklist
- [ ] 用实际转化测试转化跟踪
- [ ] 落地页面载荷快速( < 3 秒)
- [ ]落地页面方便移动
- [ ] UTM 参数有效
- [ ] 预算设置正确
- [ ] 瞄准预定受众

---

## Google RSA Output Spec (mandatory when generating RSAs)

当用户请求Google Ads RSAs(响应搜索Ads)时,输出MUST符合这些平台限制和结构要求. 不要输出任何违反它们的RSA.

### Hard limits per RSA (enforce before responding)

- **Headlines:** exactly **15** per RSA, each **≤ 30 characters** (count characters, including spaces). Render as `1. ... (NN chars)` so the reader can verify.
- ** 说明:** 每个登记册系统管理人准确的**4**,每个字母== 90个字符**。
- **Paths:**最多2个路径字段,每个字段为QQ15字符**.
- ** 最后URL:** 现时,https.
- ** Pinning:** 说明任何固定位置。 默认 = 未绑定, 除非用户询问 。
- ** 账户警卫:** Google 执行 **3 RSAs 最大每个广告组**. 当用户要求 >3时,按广告分组.

### Required sidecar artifacts (always include with RSA request)

1. **Ad group structure**, labeled `Ad group structure:` — list each ad group with its theme, target keywords (match types), and which RSAs map to it.
2. **Negative keyword list**, labeled `Negative keywords:` — minimum **8** entries, group-level vs campaign-level called out.
3. ** 链路**(≥4),** 呼叫**(≥4 ≤25字节),** 断片段**(如果相关的话)。

### Medical / CFM compliance (when product context indicates pt-BR medical practice)

If `.agents/product-marketing.md` indicates a Brazilian medical practice (CFM-regulated), the following terms are **forbidden** in headlines, descriptions, sitelinks, and callouts:

- Superlatives: `#1`, `melhor`, `o melhor`, `melhor do brasil`, `top`, `referência`
- Outcome promises: `garantido`, `garantia`, `cura`, `cura definitiva`, `100%`, `resultado garantido`, `livre da dor`
- 比较索赔与其他医生/诊所

Use neutral framing: `atendimento`, `consulta`, `avaliação`, `segunda opinião`, `agende sua consulta`, `tire suas dúvidas`. Geo modifier (`Porto Alegre`, `POA`, `Zona Sul POA`) required where the prompt specifies a region.

### Output ORDER (mandatory — emit in this order to avoid truncation)

1. ** 阿德集团结构**(短)
2. ** 无关键字** (==8, Mandatory ——在登记册系统管理人之前发出,所以如果输出长则不会丢弃)
3. ** 链路** (≥4)
4. ** 呼声** (≥4)
5. **RSA1,RSA2,RSA3**(最大区段,最后一个区段——安全可优雅地缩短)

### Output template (mandatory shape)

```
Ad group structure:
- AG1 [theme]: keywords (match types) → RSA1, RSA2
- AG2 [theme]: ...

Negative keywords:
  Campaign-level:
    - <kw>
    - <kw>
    (≥4 here)
  Ad-group level:
    - AG1: <kw>, <kw>
    - AG2: <kw>, <kw>
    (≥4 more here — TOTAL ≥8 entries)

Sitelinks (≥4):
  - <title (≤25)> | <desc1 (≤35)> | <desc2 (≤35)> | URL

Callouts (≥4, each ≤25 chars):
  - <callout>

RSA1 — [ad group name]
  Final URL: https://...
  Path1: ...   Path2: ...
  Headlines (15, each ≤30 chars):
    1. <headline> (NN chars)
    ...
    15. <headline> (NN chars)
  Descriptions (4, each ≤90 chars):
    1. <description> (NN chars)
    ...
    4. <description> (NN chars)
  Pinning: H1=none; H2=none; ...   (or explicit pins)

RSA2 — ...
RSA3 — ...
```

### Self-check before responding

在发送输出之前, 以心力运行此清单 :

- 每个登记册系统管理人都有15条新闻头条 4条新闻
- [ ]每条头条都是 ≤ 30 个字符;每条描述都是 ≤ 90 个字符. 字符计数 。
- [] 标注了负面关键词列表和\\\8条目.
- [ ] Ad group structure 标签化.
- [ ] 如果医疗(CFM):没有禁止的超音速/结果单词;必要时有地理修饰剂;语言为pt-BR。

如果检查失败, 请在回复前重写 。 不运送部分登记册系统管理人。

---

## Common Mistakes to Avoid

### Strategy
- 不跟踪转化而启动
- 竞选活动过多(预算分散)
- 没有给算法足够的学习时间
- 优化错误的衡量标准

### Targeting
- 受众范围太窄或太广
- 不排除现有客户
- 重叠的受众竞争

### Creative
- 每套广告只有一个广告
- 不刷新创意( fatigue)
- 广告和落地页面的错配

### Budget
- 在各种运动中分散过多
- 进行重大预算改革(干扰学习)
- 在学习阶段停止运动

---

## Task-Specific Questions

1. 您正在运行还是想要从哪个平台开始 ?
2. 你的每月广告预算是多少?
3. 成功的转化是什么样子(和它的价值)?
4. 您是否拥有已有的创意资产或需要创建这些资产?
5. 广告会指什么?
6. 您是否设置了像素/ 转化跟踪 ?

---

## Tool Integrations

关于执行情况,见[工具登记册](././工具/登记册)。 关键广告平台:

| Platform | Best For | MCP | Guide |
|----------|----------|:---:|-------|
| **Google Ads** | Search intent, high-intent traffic | ✓ | [google-ads.md](../../tools/integrations/google-ads.md) |
| **Meta Ads** | Demand gen, visual products, B2C | - | [meta-ads.md](../../tools/integrations/meta-ads.md) |
| **LinkedIn Ads** | B2B, job title targeting | - | [linkedin-ads.md](../../tools/integrations/linkedin-ads.md) |
| **TikTok Ads** | Younger demographics, video | - | [tiktok-ads.md](../../tools/integrations/tiktok-ads.md) |

关于跟踪设置,见[参考文献/转化-跟踪.md](参考文献/转化-跟踪.md),[ga4.md](././工具/整合/ga4.md),[section.md](././tools/整合/组件.md)

---

## Related Skills

- **marketing-ad-creative**: For generating and iterating ad headlines, descriptions, and creative at scale
- **marketing-copywriting**: For landing page copy that converts ad traffic
- **marketing-analytics**: For proper conversion tracking setup
- **marketing-ab-testing**: For landing page testing to improve ROAS
- **marketing-cro**: For optimizing post-click conversion rates
