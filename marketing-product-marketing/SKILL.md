---
name: marketing-product-marketing
description: 当用户需要创建或更新产品营销上下文文档时使用，包括产品定位、目标受众、ICP、价值主张、消息框架，以及希望让其他 marketing-* skills 复用基础信息的场景。
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
  localized: zh-CN
---

# 产品营销上下文

# Product Marketing Context

您帮助用户创建和维护产品营销背景文档 。 这捕捉到其他营销skill参考的基础定位和消息信息,因此用户不会重复.

The document is stored at `.agents/product-marketing.md`.

## Workflow

### Step 1: Check for Existing Context

First, check if `.agents/product-marketing.md` already exists. Also check `.claude/product-marketing.md` and the legacy filename `product-marketing-context.md` (in either `.agents/` or `.claude/`) for older setups — if found anywhere other than `.agents/product-marketing.md`, offer to move it to the canonical location.

** 如果存在:**
- 读一下,总结一下捕捉到的东西
- 询问要更新的章节
- 只为这些章节收集信息

** 如果不存在,则提供两种选择:**

1. ** 代码库的自动草稿**(建议): 你会研究Repo-README,落地页,营销文案,package.json等,并起草上下文文件的V1. 然后用户审查、纠正和填补空白。 这比从头开始快

2. ** 从零开始**: 逐条逐条地走过每个区段,一次收集一个区段的信息.

大多数用户更喜欢备选案文1。 在提出草案后,问:"什么需要纠正? 少了什么东西?"

### Step 2: Gather Information

** 如果自动起草:**
1. 读取代码库:README,落地页,营销文案,关于页面,元描述,软件包. json,任何现有的docs
2. 根据您发现的情况起草所有章节
3. 提出草案并询问需要纠正或缺少什么
4. 直到用户满意为止

** 如果从零开始:**
逐段逐段地通过对话 不要马上放弃所有的问题。

每节:
1. 简单解释一下你在捕捉什么
2. 询问相关问题
3. 确认准确性
4. 移动到下一个

推向逐字记录客户语言——准确的短语比抛光描述更有价值,因为它们反映了客户实际思考和说话的方式,这使得拷贝更具共鸣性.

---

## Sections to Capture

### 1. Product Overview
- 一行说明
- 它所做的(2-3句)
- 产品类别(您所坐的“ shelf ” —— 顾客如何搜索您)
- 产品类型(SaaS、市场、电子商务、服务等)
- 商业模式和定价

### 2. Target Audience
- 目标公司类型(工业、规模、舞台)
- 目标决策者(作用、部门)
- 主使用大小写( 您解决的主要问题)
- 工作要完成(2 -3件事 客户"雇"你)
- 具体用途案例或设想

### 3. Personas (B2B only)
如果有多个利益攸关方参与购买,则收集每个利益攸关方:
- 用户、冠军、决策者、金融买家、技术影响者
- 每个人关心什么, 他们的挑战,和你向他们保证的价值

### 4. Problems & Pain Points
- 客户在找到你之前面临核心挑战
- 目前的解决办法为何不足
- 他们付出的代价(时间、金钱、机会)
- 情绪紧张(压力、恐惧、怀疑)

### 5. Competitive Landscape
- ** 直接竞争者**: 同样的解决方案,同样的问题(例如,Calendly vs SavvyCal).
- ** 二级竞争者**: 不同的解决方案,同样的问题(如卡伦德利对超人排程)
- ** 间接竞争者**: 冲突方法(如卡伦德利与个人助理)
- 对顾客来说,每个都有什么缺点

### 6. Differentiation
- 关键差异(缺乏替代能力)
- 如何用不同方法解决
- 为什么这样更好(好处)
- 为什么顾客选择你 而不是其他选择

### 7. Objections & Anti-Personas
- 在销售中听取了前3名的反对意见以及如何解决这些问题
- 谁是不合适的(反人)

### 8. Switching Dynamics
JTBD四军:
- ** 推动**: 是什么挫败感驱使他们远离目前的解决方案
- ** 页:1 什么吸引他们到你
- ** 哈比特**: 是什么让他们坚持目前的方法
- ** 焦虑**: 为何他们担心换衣服

### 9. Customer Language
- 顾客如何描述问题(verbatim)
- 他们如何描述你的解决方案(verbatim)
- 要使用的单词/词句
- 避免的言语
- 具体产品术语词汇表

### 10. Brand Voice
- 通音(专业、随意、游戏等)
- 交流风格(直接、对话、技术)
- 品牌人格(3-5个形容词).

### 11. Proof Points
- 可引用的关键衡量标准或结果
- 知名客户/日志
- 证词片断
- 主要价值主题和佐证

### 12. Goals
- 主要商业目标
- 密钥转化动作( 您想要人们做什么 )
- 当前衡量标准(如果已知)

---

## Step 3: Create the Document

After gathering information, create `.agents/product-marketing.md` with this structure:

```markdown
# Product Marketing Context

*Last updated: [date]*

## Product Overview
**One-liner:**
**What it does:**
**Product category:**
**Product type:**
**Business model:**

## Target Audience
**Target companies:**
**Decision-makers:**
**Primary use case:**
**Jobs to be done:**
-
**Use cases:**
-

## Personas
| Persona | Cares about | Challenge | Value we promise |
|---------|-------------|-----------|------------------|
| | | | |

## Problems & Pain Points
**Core problem:**
**Why alternatives fall short:**
-
**What it costs them:**
**Emotional tension:**

## Competitive Landscape
**Direct:** [Competitor] — falls short because...
**Secondary:** [Approach] — falls short because...
**Indirect:** [Alternative] — falls short because...

## Differentiation
**Key differentiators:**
-
**How we do it differently:**
**Why that's better:**
**Why customers choose us:**

## Objections
| Objection | Response |
|-----------|----------|
| | |

**Anti-persona:**

## Switching Dynamics
**Push:**
**Pull:**
**Habit:**
**Anxiety:**

## Customer Language
**How they describe the problem:**
- "[verbatim]"
**How they describe us:**
- "[verbatim]"
**Words to use:**
**Words to avoid:**
**Glossary:**
| Term | Meaning |
|------|---------|
| | |

## Brand Voice
**Tone:**
**Style:**
**Personality:**

## Proof Points
**Metrics:**
**Customers:**
**Testimonials:**
> "[quote]" — [who]
**Value themes:**
| Theme | Proof |
|-------|-------|
| | |

## Goals
**Business goal:**
**Conversion action:**
**Current metrics:**
```

---

## Step 4: Confirm and Save

- 显示已完成的文档
- 询问是否需要调整
- Save to `.agents/product-marketing.md`
- Tell them: "Other marketing skills will now use this context automatically. Run `/marketing-product-marketing` anytime to update it."

---

## Tips

- ** 具体一点**:问:什么是#1 挫折带给你的? 不是说,他们解决了什么问题?
- ** 精确词**: 客户语言胜过抛光描述
- ** 举个例子**: "你能举个例子吗?"解锁更好的答案
- 时间轴: 将每一节进行总结,并在继续前确认
- ** 滑动不适用**: 并非所有产品都需要所有部分(例如,B2C的Personas)
