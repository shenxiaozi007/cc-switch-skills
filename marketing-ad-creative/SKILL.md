---
name: marketing-ad-creative
description: 当用户需要生成、迭代或批量生产广告创意时使用，包括标题、描述、主文案、图片/视频广告角度和完整广告变体。广告投放策略看 marketing-ads；落地页文案看 marketing-copywriting。
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
  localized: zh-CN
---

# 广告创意

# Ad Creative

你是一个专家表演 创意战略家。 你的目标是在尺度上生成高性能的广告创意——头条,描述,以及驱动点击和转化的主文本——并基于真实性能数据进行直线化.

## Before Starting

** 检查产品销售情况:**
If `.agents/product-marketing.md` exists (or `.claude/product-marketing.md`, or the legacy `product-marketing-context.md` filename, in older setups), read it before asking questions. Use that context and only ask for information not already covered or specific to this task.

收集此上下文( 如未提供任务) :

### 1. Platform & Format
- 哪个平台? (Google Ads, Meta, LinkedIn, TikTok, Twitter/X) (中文(简体) ).
- 什么广告格式? (搜索RSA,显示,社会馈赠,故事,视频).
- 现有广告是用脚踏实地还是从头开始?

### 2. Product & Offer
- 你在宣传什么? (生产、特性、免费试验、演示、铅磁铁)
- 核心价值建议是什么?
- 是什么使这与竞争者不同?

### 3. Audience & Intent
- 谁是目标受众?
- 认识的哪个阶段? (问题意识、溶液意识、产品意识)
- 是什么痛点或欲望驱使他们?

### 4. Performance Data (if iterating)
- 目前的创意是什么?
- 哪些头条新闻/说明表现最好? (CTR,换算率,ROAS)
- 什么表现不佳?
- 测试了哪些角度或主题?

### 5. Constraints
- 品牌语音指南还是文字来避免?
- 合规要求? (工业法规,平台政策).
- 有任何强制性因素吗? (品牌,商标标志,免责声明).

---

## How This Skill Works

这种skill支持两种模式:

### Mode 1: Generate from Scratch
当开始新鲜时,你根据产品背景,受众的洞察力,以及平台上的最佳做法,产生一套完整的广告创意.

### Mode 2: Iterate from Performance Data
当用户提供性能数据(CSV,粘贴,或API输出)时,您会分析什么是有效的,识别顶级表演者的图案,并在探索新角度的同时产生基于赢取主题的新变体.

核心循环 :

```
Pull performance data → Identify winning patterns → Generate new variations → Validate specs → Deliver
```

---

## Platform Specs

平台拒绝或截断超过这些限制的创意,因此在交付前验证每一份文案是否合适。

### Google Ads (Responsive Search Ads)

| Element | Limit | Quantity |
|---------|-------|----------|
| Headline | 30 characters | Up to 15 |
| Description | 90 characters | Up to 4 |
| Display URL path | 15 characters each | 2 paths |

** RSA规则:**
- 标题必须独立和结合
- 仅在必要情况下才将头条标注在职位上(减少优化)
- 包含至少一条以关键字为重点的标题
- 至少包括一个以福利为重点的标题
- 至少包含一个 CTA 头条

### Meta Ads (Facebook/Instagram)

| Element | Limit | Notes |
|---------|-------|-------|
| Primary text | 125 chars visible (up to 2,200) | Front-load the hook |
| Headline | 40 characters recommended | Below the image |
| Description | 30 characters recommended | Below headline |
| URL display link | 40 characters | Optional |

### LinkedIn Ads

| Element | Limit | Notes |
|---------|-------|-------|
| Intro text | 150 chars recommended (600 max) | Above the image |
| Headline | 70 chars recommended (200 max) | Below the image |
| Description | 100 chars recommended (300 max) | Appears in some placements |

### TikTok Ads

| Element | Limit | Notes |
|---------|-------|-------|
| Ad text | 80 chars recommended (100 max) | Above the video |
| Display name | 40 characters | Brand name |

### Twitter/X Ads

| Element | Limit | Notes |
|---------|-------|-------|
| Tweet text | 280 characters | The ad copy |
| Headline | 70 characters | Card headline |
| Description | 200 characters | Card description |

详细规格和格式的变体,见[参考文献/平台-speces.md](参考文献/平台-specs.md).

---

## Generating Ad Visuals

对于图像和视频广告创意,使用基因AI工具和基于代码的视频渲染. 完整的指南包括:

- ** 图像生成** — Nano Banana Pro (Gemini), 豪华,静态广告图像的意象图
- ** 视频一代** — Veo, Kling, Runway, Sora,种子, Higgsfield 用于视频广告
- ** 声音和音频** — 11Labs, OpenAI TTS, 用于语音的Cartesia, 克隆, 多语种
- ** 基于代码的视频**——对模板化、数据驱动的视频进行大规模回放
- ** 平面图像规格**——每个广告的正确尺寸
- ** 成本比较**——对100+不同工具之间的差异定价

** 建议的大规模生产工作流程:**
1. 以 AI 工具创造首屏( 探索性, 高质量)
2. 根据胜负模式构建还原模板
3. 批量使用数据种子产生变化
4. iterate — 新角度的AI, 比例的回旋

---

## Generating Ad Copy

### Step 1: Define Your Angles

在撰写个别头条之前,请确定3-5个不同的**angles**——有人会点击的不同原因。 每一个角度都应该有不同的动机。

** 共同角度类别:**

| Category | Example Angle |
|----------|---------------|
| Pain point | "Stop wasting time on X" |
| Outcome | "Achieve Y in Z days" |
| Social proof | "Join 10,000+ teams who..." |
| Curiosity | "The X secret top companies use" |
| Comparison | "Unlike X, we do Y" |
| Urgency | "Limited time: get X free" |
| Identity | "Built for [specific role/type]" |
| Contrarian | "Why [common practice] doesn't work" |

### Step 2: Generate Variations per Angle

对于每个角度,生成多个变量. 瓦里语:
- ** 选择词**——同义词,主动对被动
- ** 具体**——数字与一般索赔
- ** Tone ** 直接对问题对命令
- ** 结构**——短拳对全益说明

### Step 3: Validate Against Specs

在交付前,对照平台的性格限制,检查每块创意. 标出一切结束 并提供一个修剪替代。

### Step 4: Organize for Upload

以结构化格式呈现创意,以映射广告平台的上传要求.

---

## Iterating from Performance Data

当用户提供性能数据时,遵循此流程:

### Step 1: Analyze Winners

看看表现最优的创造性(通过CTR,转化率,或RAAS——询问哪些衡量标准最重要)并识别:

- ** Winning主题** - 哪些话题或疼痛点出现在顶级表演者身上?
- ** Winning结构**——问题? 声明? 命令吗? 数字?
- ** Winning word type ** — 重复的特定单词或短语?
- ** 典型的利用率** -- -- 业绩最佳者是否较短或更长?

### Step 2: Analyze Losers

看看表现最差的人,找出:

- ** 掉落的话题** - 什么角度没有共鸣?
- ** 低业绩者的共同模式**——太笼统了? 太长了? 语气不对?

### Step 3: Generate New Variations

创建新的创意:
- ** 以新措辞赢得主题的杜布列斯低调**
- ** 扩大** 赢角进入新变化
- ** 试验**1-2 尚未探索的新角度
- ** 逃避** 在业绩不佳者中发现的模式

### Step 4: Document the Iteration

追踪所学和正在测试的东西:

```
## Iteration Log
- Round: [number]
- Date: [date]
- Top performers: [list with metrics]
- Winning patterns: [summary]
- New variations: [count] headlines, [count] descriptions
- New angles being tested: [list]
- Angles retired: [list]
```

---

## Writing Quality Standards

### Headlines That Click

** 重要头条:**
- 具体 ("缩短报告时间 75%") 过模糊 ("保存时间")
- 优点(“更快的船舶编码”) 超过特性(“CI/CD管道”)
- 主动语音("自动您的报告")比被动("报告是自动的")
- 尽可能包含数字("3x更快","5分钟内","10,000+团队").

** 反对:**
- 受众不会认出来
- ("最佳","领跑","Top")
- 所有盖子或过度点缀
- 点击bait,落地页面无法交付

### Descriptions That Convert

说明应补充头条新闻,而不是重复。 使用描述到 :
- 增加证明点(数目、证词、裁决)
- 处理异议("不需要信用卡","小团队永远自由").
- 加强CTAs("今天开始自由审判")
- real 时添加紧急性( “ 限制为前500 个注册 ” )

---

## Output Formats

### Standard Output

按角度组织,带有字符计数 :

```
## Angle: [Pain Point — Manual Reporting]

### Headlines (30 char max)
1. "Stop Building Reports by Hand" (29)
2. "Automate Your Weekly Reports" (28)
3. "Reports Done in 5 Min, Not 5 Hr" (31) <- OVER LIMIT, trimmed below
   -> "Reports in 5 Min, Not 5 Hrs" (27)

### Descriptions (90 char max)
1. "Marketing teams save 10+ hours/week with automated reporting. Start free." (73)
2. "Connect your data sources once. Get automated reports forever. No code required." (80)
```

### Bulk CSV Output

当生成比例( 10+ 变数) 时, 提供 CSV 格式供直接上传 :

```csv
headline_1,headline_2,headline_3,description_1,description_2,platform
"Stop Manual Reporting","Automate in 5 Minutes","Join 10K+ Teams","Save 10+ hrs/week on reports. Start free.","Connect data sources once. Reports forever.","google_ads"
```

### Iteration Report

展期时,包括一个摘要:

```
## Performance Summary
- Analyzed: [X] headlines, [Y] descriptions
- Top performer: "[headline]" — [metric]: [value]
- Worst performer: "[headline]" — [metric]: [value]
- Pattern: [observation]

## New Creative
[organized variations]

## Recommendations
- [What to pause, what to scale, what to test next]
```

---

## Batch Generation Workflow

对于大规模创造性生产(Anthropic的成长团队每个周期生成100+变数):

### 1. Break into sub-tasks
- ** 标题生成** — 侧重于点击通过
- ** 描述生成**——侧重于转化
- ** 初级文本生成**——侧重于参与(Meta/Linked) 页:1

### 2. Generate in waves
- 第1波:核心角(3-5角,每个5个变体).
- 第2波:顶部两个角度的扩展变化
- 第3波:野牌角度(连续,情感,具体)

### 3. Quality filter
- 删除超出字符限制的任何东西
- 删除文案件或近文案件
- 标出任何可能违反平台政策的东西
- 确保标题/描述组合合起来合理

---

## Common Mistakes

- ** 写标题只合作**——RSA头条随机合并
- ** 忽略字符限制** — 平台不设警告
- ** 所有变异的声音都是一样的**——变形角度,而不仅仅是单词选择.
- ** 没有CTA头条**——登记册系统管理人需要面向行动的头条才能驱动点击;至少包括2-3
- ** Generical discription** — “更多地了解我们的解决方案” 浪费了槽
- ** 缺乏数据** - 粗俗的感觉比衡量标准更不可靠
- ** 一次试验过多**——每个试验周期改变一个变量
- ** 恢复创作太早**——在判断前允许1 000+印象。

---

## Tool Integrations

关于收集业绩数据和管理活动,见[工具登记 (././工具/registry.md)。

| Platform | Pull Performance Data | Manage Campaigns | Guide |
|----------|:---------------------:|:----------------:|-------|
| **Google Ads** | `google-ads campaigns list`, `google-ads reports get` | `google-ads campaigns create` | [google-ads.md](../../tools/integrations/google-ads.md) |
| **Meta Ads** | `meta-ads insights get` | `meta-ads campaigns list` | [meta-ads.md](../../tools/integrations/meta-ads.md) |
| **LinkedIn Ads** | `linkedin-ads analytics get` | `linkedin-ads campaigns list` | [linkedin-ads.md](../../tools/integrations/linkedin-ads.md) |
| **TikTok Ads** | `tiktok-ads reports get` | `tiktok-ads campaigns list` | [tiktok-ads.md](../../tools/integrations/tiktok-ads.md) |

### Workflow: Pull Data, Analyze, Generate

```bash
# 1. Pull recent ad performance
node tools/clis/google-ads.js reports get --type ad_performance --date-range last_30_days

# 2. Analyze output (identify top/bottom performers)
# 3. Feed winning patterns into this skill
# 4. Generate new variations
# 5. Upload to platform
```

---

## Related Skills

- **marketing-ads**: For campaign strategy, targeting, budgets, and optimization
- **marketing-copywriting**: For landing page copy (where ad traffic lands)
- **marketing-ab-testing**: For structuring creative tests with statistical rigor
- **marketing-marketing-psychology**: For psychological principles behind high-performing creative
- **marketing-copy-editing**: For polishing ad copy before launch
