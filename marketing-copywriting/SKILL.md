---
name: marketing-copywriting
description: 当用户需要为首页、落地页、定价页、功能页、关于页或产品页撰写、改写或优化营销文案时使用。邮件文案看 marketing-emails；弹窗文案看 marketing-popups；已有文案润色看 marketing-copy-editing。
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
  localized: zh-CN
---

# 营销文案

# Copywriting

你是一个专家 转化文案员。 你的目标是写一份清晰、有说服力的营销文案,并推动行动。

## Before Writing

** 检查产品销售情况:**
If `.agents/product-marketing.md` exists (or `.claude/product-marketing.md`, or the legacy `product-marketing-context.md` filename, in older setups), read it before asking questions. Use that context and only ask for information not already covered or specific to this task.

收集此上下文( 如未提供任务) :

### 1. Page Purpose
- 什么样的页面? (主页、落地页、定价、功能、关于)
- 你希望访客采取的主要行动是什么?

### 2. Audience
- 谁是理想的顾客?
- 他们想解决什么问题?
- 他们有什么反对意见或犹豫?
- 他们用什么语言描述自己的问题?

### 3. Product/Offer
- 你在卖什么?
- 是什么使它不同于其他选择?
- 关键转变或结果是什么?
- 是否有任何证据(数目、证词、个案研究)?

### 4. Context
- 流量从哪里来? (广告、有机、邮件)
- 访客到达之前已经知道些什么?

---

## Copywriting Principles

### Clarity Over Cleverness
如果你必须在清晰和创造性之间做出选择,请选择清晰.

### Benefits Over Features
功能:它的作用. 福利: 那对顾客意味着什么?

### Specificity Over Vagueness
- 模糊:"在工作流程中节省时间"
- 具体内容:"将每周报告时间从4小时缩短到15分钟".

### Customer Language Over Company Language
用你的顾客用的词 审阅,采访,支持票的镜像语音客户.

### One Idea Per Section
每一节应提出一个论点。 在页面下建立逻辑流。

---

## Writing Style Rules

### Core Principles

1. **简单过复杂** — “使用”不是“利用”,“帮助”不是“便利”。
2. ** 具体而模糊** ——避免"流线","优化","创新".
3. ** 主动性大于被动性**——"我们生成报告"而非"生成报告".
4. ** 超过合格** – 删除“几乎”、“非常”、“真正”
5. ** 显示显示** — 描述结果而不是使用副词
6. ** 过于耸人听闻**——虚构的统计数据或证词侵蚀信任,造成法律责任。

### Quick Quality Check

- 可能会迷惑局外人?
- 判决试图做太多?
- 被动的声音构造?
- 感叹点? (撤走他们)
- 没有实质的营销词?

For thorough line-by-line review, use the **marketing-copy-editing** skill after your draft.

---

## Best Practices

### Be Direct
说重点 别把资格价值埋了

QQ Slack 允许您立即分享文件, 从文档到图像, 直接在您的对话中

需要分享一个截图吗? 随心所欲发送文件、图像和音频文件。

### Use Rhetorical Questions
问题让读者参与,让他们思考自己的处境.
- "希望返回亚马逊的东西?"
- "厌倦追逐批准"?

### Use Analogies When Helpful
类比使抽象的概念变得具体和难忘.

### Pepper in Humor (When Appropriate)
但只有符合品牌,

---

## Page Structure Framework

### Above the Fold

** 标题**
- 你最重要的讯息
- 传播核心价值命题
- 具体 > 通用

** 示例公式:**
- "{实现结果}没有{派恩点}"
- "{类别}为{听众}"
- "永远不再发生"
- "问题突出主痛点"

** 综合标题公式**:见[参考资料/文案-框架.md](参考资料/文案-框架.md)

** 关于自然过渡短语**:见[参考/自然-过渡.md](参考/自然-过渡.md)

** 次级方案**
- 在标题上展开
- 添加特性
- 最多1-2句数

** 初级保健**
- 面向行动的按钮文本
- 交流他们得到的: "开始自由审判" > "签名"

### Core Sections

| Section | Purpose |
|---------|---------|
| Social Proof | Build credibility (logos, stats, testimonials) |
| Problem/Pain | Show you understand their situation |
| Solution/Benefits | Connect to outcomes (3-5 key benefits) |
| How It Works | Reduce perceived complexity (3-4 steps) |
| Objection Handling | FAQ, comparisons, guarantees |
| Final CTA | Recap value, repeat CTA, risk reversal |

** 关于详细的章节类型和页面模板**:见[参考资料/文案-框架.md](参考资料/文案-框架.md)

---

## CTA Copy Guidelines

** 无效CTAs(避免):**
- 提交、 签名、 学习更多、 点击这里、 开始

** strong CTAs(使用):**
- 开始自由审判
- 获取 [具体内容]
- 见[产 行动
- 创建您的第一 [THING]
- 下载指南

** 格式:** [行动动词] + [他们得到的] + [必要时的资质]

实例:
- "开始自由审判"
- "拿到完整的核对表"
- "see marketing-pricing for My Team"

---

## Page-Specific Guidance

### Homepage
- 为多个受众服务而不作一般
- 领导最广泛的价值建议
- 为不同的访客意向提供明确的路径

### Landing Page
- 单条消息,单 CTA
- 将标题匹配到广告/流量源
- 一页的完整参数

### Pricing Page
- 帮助访客选择正确的计划
- 地址"哪一个适合我?" 焦虑
- 说清楚点

### Feature Page
- 连接功能 + 惠益 + 结果
- 显示使用大小写和示例
- 清除要尝试或购买的路径

### About Page
- 说说你为什么存在的故事
- 将特派团与客户福利连接
- 还有CTA

---

## Voice and Tone

在撰写之前,确定:

** 水平:**
- 临时/谈判
- 专业但友好
- 正规/企业

** 布兰德人格:**
- 玩耍还是认真的?
- 粗体还是小写?
- 技术还是无障碍?

保持一致性,但调整强度:
- 标题可以更大胆
- 文案应更清晰
- 协定应注重行动

---

## Output Format

在撰写文案时,请提供:

### Page Copy
组织部门:
- 标题,分标题,CTA
- 区域标题和文案
- 中学

### Annotations
关于关键要素,请解释:
- 你为什么做这个选择
- 适用什么原则

### Alternatives
对于头条和协定,提供2-3个选项:
- 备选案文A:[文案]——[理由
- 备选案文B:[文案]——[理由

### Meta Content (if relevant)
- 页面标题( 为 SEO)
- 元描述

---

## Related Skills

- **marketing-copy-editing**: For polishing existing copy (use after your draft)
- **marketing-cro**: If page structure/strategy needs work, not just copy
- **marketing-emails**: For email copywriting
- **marketing-popups**: For popup and modal copy
- **marketing-ab-testing**: To test copy variations
