## 4. 读论文：建立坐标系，不是被动读完

读论文的目的不是“看完”，而是回答：这篇论文在我的项目里有什么用？它是 benchmark、baseline、方法来源、诊断视角，还是一个需要被检验的 claim？

### 4.0 先补基础：论文不是教材

论文通常只呈现一个增量贡献，不负责系统教学。新手如果只刷最新论文，很容易得到一堆碎片术语，却不知道哪些是基本定义，哪些是社区共识，哪些只是某篇论文的局部技巧。

进入一个新方向时，先建立最小基础包：

- 一份核心教材、课程讲义或高质量 lecture notes；
- 一篇 survey 或综述；
- 三到五篇 anchor papers；
- 一个可信 benchmark 或 toolkit；
- 一组领域常用 baseline 和 metric。

读教材或讲义的目标不是“从头学完”，而是建立坐标系：核心定义是什么，标准任务是什么，基本方法族有哪些，常见评测怎么做，哪些失败模式已经被反复观察到。教材给结构，论文给前沿增量。两者缺一不可。

### 4.1 读之前先确定目的

| 阅读目的 | 重点看什么 | 产出 |
|---|---|---|
| 入门 | abstract、introduction、conclusion、related work | 领域地图 |
| 找 baseline | method、experiment setup、metric、code | baseline 列表 |
| 复现 | implementation detail、hyperparameter、data split | 复现 checklist |
| 找问题 | limitation、assumption、error case、ablation | gap list |
| 做对照 | main result、ablation、setting difference | 对照实验设计 |
| 解释失败 | case study、analysis、appendix | failure taxonomy |

如果你不知道自己为什么读这篇论文，就先不要读深。先判断它在项目里的角色。

### 4.2 三遍读法

**第一遍：5 到 10 分钟，判断是否值得继续读。**

只看 title、abstract、introduction、figures、main tables、conclusion。结束后写五行：

```md
- Category：方法、数据集、评测、系统、理论、survey？
- Problem：它解决什么问题？
- Claim：最核心 claim 是什么？
- Evidence：主要证据是什么？
- Relevance：和我当前问题有什么关系？
```

关系弱的论文放进 bibliography，不要立刻深读。

**第二遍：30 到 60 分钟，理解结构和证据。**

重点看：

- problem formulation；
- method overview；
- experiment setup；
- main results；
- ablation；
- limitations；
- appendix 中的实现细节。

结束后写一张 paper card。

**第三遍：半天到数天，只给关键论文。**

只有以下论文值得深读：

- 你要复现的论文；
- 你的核心 baseline；
- 定义数据集、metric 或 protocol 的论文；
- 你要挑战其 claim 的论文；
- 你的方法或实验直接依赖的论文。

第三遍要做：

- 对照论文和代码；
- 重画 pipeline；
- 复现关键表格或图；
- 列出 hidden assumptions；
- 写出你会质疑的地方；
- 标注每个 claim 依赖哪些证据。

### 4.3 判断一篇论文是否重要

不要只看 venue、citation 或榜单分数。更实用的判断方式是看它在项目中的角色。

| 角色 | 判断标准 | 是否需要深读 |
|---|---|---|
| Anchor paper | 定义问题或任务 | 是 |
| Benchmark paper | 提供 dataset、metric、protocol | 是 |
| Baseline paper | 需要在实验中比较 | 是 |
| Diagnostic paper | 提供错误类型或评测视角 | 通常需要 |
| Method paper | 提供可借鉴机制 | 看相关性 |
| Survey / SoK | 帮你建立地图 | 第一阶段有用 |
| Incremental SOTA | 主要是小幅涨分 | 不一定优先 |
| Engineering report | 实现细节丰富 | 复现时有用 |

判断论文重要性时，问四个问题：

1. 它是否定义了我现在的问题？
2. 它是否决定了我需要比较的 baseline？
3. 它是否提供了我可以验证或挑战的 claim？
4. 它是否改变了我下一步实验设计？

如果四个答案都是否，这篇论文当前不是优先级。

### 4.4 AI 辅助读文献的边界

AI 工具可以帮你快速整理背景、解释术语、列出相关工作，也可以把一篇非核心论文压缩成初步摘要。但核心论文不能只靠 AI 摘要。

对于以下论文，要自己读原文和附录：

- 你要复现的 baseline；
- 你的实验 protocol 依赖的 benchmark；
- 你要挑战其 claim 的论文；
- 你的方法直接借用的论文；
- 你发现其 bad cases 或 limitations 可能成为切入点的论文。

AI 摘要通常会把论文中最有价值的“毛刺”抹平：作者没有强调的限制、附录里的妥协、实验设置中的隐含前提、失败 case 的尴尬细节。新手真正的机会，常常就在这些不顺滑的地方。

使用 AI 读论文时，产出不要是“这篇论文讲了什么”，而要是：

```text
它的核心 claim 是什么？
claim 依赖哪些证据？
它固定了哪些 setting？
它没有证明什么？
我当前项目能从中得到哪个 action item？
```

如果回答不了最后一题，说明这篇论文暂时不是你当前主线的输入。

### 4.5 Paper Card

```md
# Paper Card

## Citation
论文题目、作者、年份、链接。

## Problem
这篇论文解决什么问题？

## Core Idea
核心机制是什么？用三句话以内说明。

## Assumptions
它默认了哪些条件？数据、模型、metric、资源、任务边界。

## Evidence
它用哪些实验支持 claim？主结果、ablation、case study。

## Weakness / Boundary
在哪些场景下可能失效？作者有没有展示 failure case？

## Relation to My Project
它在我项目里是什么角色：baseline、benchmark、方法来源、反例、诊断视角？

## Action Item
读完后我下一步要做什么？复现、加入对照、做切片、暂存。
```

### 4.6 Literature Map

读 10 篇论文后，不要只留下 10 个摘要。要把它们放进地图。

```md
# Literature Map

## 当前问题
一句话描述。

## 论文分组
### 定义问题的论文
- Paper A：定义了什么？

### Baseline 论文
- Paper B：我需要比较什么？

### 方法论文
- Paper C：提供什么机制？

### 评测与 benchmark 论文
- Paper D：定义了什么数据、metric 或 protocol？

### 诊断与 failure analysis 论文
- Paper E：提供什么错误类型或切片视角？

## 已知结论
这个方向目前可靠的结论是什么？

## 共同假设
这些论文共同默认了什么？

## 共同盲点
它们没有测试什么？

## 对我当前项目的启发
下一步应该验证什么？
```

---

