# 科研新手做事手册：把问题往前推进

> 版本：v1.5，学生版 / 实验室内部传承版  
> 读者：第一次进入科研训练的本科生、硕士生、博士一年级学生  
> 适用方向：ML / NLP / LLM / Recommender Systems / AI Safety 等实验型计算机研究  
> 使用方式：这是一份做事手册。它只讲如何读论文、看数据、复现、读代码、分析错误、设计实验、记录结果和沟通推进；不讨论论文写作、投稿策略或故事组织。

---

## 0. 这份手册解决什么问题

刚开始做研究时，很多人会卡在相同的位置：

- 读了不少论文，但没有形成判断；
- 跑通了代码，但不知道代码到底做了什么；
- 复现了一个分数，但不知道这个分数说明什么；
- 只看总体指标，不看样本、不看中间结果、不看错误类型；
- 把模型、数据、metric 和代码都当成黑盒；
- 实验像随机试错，没有 hypothesis、control、metric 和 stop condition；
- 看到负结果，只能说“无效”，无法解释原因；
- 做了几周，没有留下可复盘的记录、图表、分析或判断；
- 卡住后长时间沉默，最后发现问题出在很早的步骤。

这份手册要求你建立一个最小研究闭环：

```text
明确问题 → 定位关键论文 → 建立可信 baseline → 看数据和代码
→ 做 error analysis → 形成假设 → 做最小实验 → 解释结果 → 决定下一步
```

公开论文、排行榜和组会报告通常呈现的是整理后的结果，不会完整展示中间的混乱、返工、低级 bug 和负结果。刚进入研究时觉得系统失控、代码难懂、结果反常，是正常状态。要训练的不是“永远不出错”，而是尽快把错误暴露出来，把混乱拆成可以验证的小问题。

新手阶段的核心目标不是立刻提出完整创新点，而是训练判断。手册里的规则不是为了制造额外负担，而是为了减少不可见损耗：让错误尽早暴露，让决定有迹可循，让每一步都能改变下一步。

需要训练的判断包括：

- 什么问题重要；
- 什么结果可信；
- 什么实验值得做；
- 什么现象需要追查；
- 什么时候继续；
- 什么时候转向；
- 什么时候停止。

每天结束时，只问一个问题：

> 今天的工作是否改变了我对问题的理解，或者改变了下一步决策？

如果没有，说明你可能只是在运行流程，而不是推进问题。

手册里的 card、log、table 和 checklist 不是思考的替代品。它们的作用只有一个：把你的判断外化，让别人和未来的自己能检查你的证据链。填满模板本身不算进展；只有当它帮助你减少不确定性、排除替代解释、发现瓶颈或改变下一步时，它才有价值。

---

## 1. 从“完成任务”切换到“推进问题”

课程作业通常已经给好题目、数据、评价标准和正确答案。研究不同。问题边界可能不清楚，数据可能有噪声，代码可能有隐性 bug，评价指标可能只反映一部分能力，SOTA 也可能只是在某个 benchmark 上表现好。

### 1.1 接受“未被安排好”的问题空间

在课程和考试里，题目通常被精心设计过：条件充分，答案存在，反馈快速，老师知道标准解法。研究不是这样。

在研究中：

- 导师通常能判断哪些线索更可能有价值，但不一定知道答案；
- 数据、代码、metric、baseline 都可能有瑕疵；
- 一个想法可能失败，也可能只是因为某个早期环节有 bug；
- 努力和产出不是线性关系。方向错时，投入更多时间不一定带来进展；关键假设一旦被验证，几小时的分析也可能改变整个方向。

面对这种不确定性，不要用“多跑几个实验”来缓解焦虑。先把系统退回到你能观察、能解释、能验证的状态。研究推进的第一步，常常不是加复杂度，而是减少未验证的复杂度。

### 1.2 慢下来，是为了更快定位问题

结果异常时，先停下来问四个问题：

1. 我现在看到的是事实，还是猜测？
2. 哪个环节可以用最小例子验证？
3. 我是否一次只改变了一个变量？
4. 下一次实验出来后，我是否知道它会支持或否定哪个假设？

“慢”不是拖延，而是把每一步变成可解释的判断。随机调参、反复重跑、临时改代码，看起来更快，通常只会把问题空间搅得更乱。

### 1.3 完成任务 vs 推进问题

| 完成任务的习惯 | 推进研究的习惯 |
|---|---|
| 代码跑通，就算完成 | 代码跑通只是开始，还要检查数据流、loss、metric 和样本输出 |
| 只看最后一个分数 | 总体分数只是入口，还要看切片、样本、置信度和错误类型 |
| 遇到负结果就换方向 | 先判断负结果排除了哪个假设，是否暴露了更重要的问题 |
| 读论文就是从头看到尾 | 读论文是为了定位 baseline、assumption、claim、evidence 和 boundary |
| 复现就是下载代码跑一次 | 复现是建立可信参照系，并理解方法何时有效、何时失败 |
| 想到新点子就马上做 | 先放入 parking lot，等当前核心问题有结论后再排序 |
| 独立就是自己憋到做出来 | 独立包括及时暴露问题，带着证据求助 |
| 忙就是有进展 | 只有留下 artifact、改变判断或缩小不确定性，才算推进 |

研究不是一直“想”，也不是一直“跑”。它是一套持续缩小不确定性的工作方法。

---

## 2. 每周工作系统

### 2.1 一周只抓一个核心问题

新手最容易同时做太多事：读很多论文、跑很多脚本、改很多模块、记很多想法，但最后没有一个清楚结论。

每周开始前，先写一句话：

```text
本周我只回答这个问题：____。
```

这个问题要足够具体。

| 不合格 | 合格 |
|---|---|
| 继续研究 knowledge editing | 确认 MEMIT 在低频实体上的 locality failure 是否显著高于高频实体 |
| 优化推荐模型 | 检查 LightGCN 的总体提升是否主要来自 head items |
| 看看 jailbreak defense 怎么样 | 比较同一 defense 在原始英文攻击、中文翻译、模板改写下的 attack success 和 over-refusal |
| 复现这篇论文 | 对齐论文 Table 2 的主结果，并确认差异来自数据 split、超参还是评测脚本 |

每周只允许有一个主问题。其他想法写进 `parking_lot.md`，不要马上切过去。

### 2.2 进展要物化

每周结束前，至少交付一个 artifact。artifact 可以很小，但要能被别人查看和复盘。

常见 artifact：

- 一页 problem brief；
- 一张结果表；
- 一张数据切片图；
- 一份复现记录；
- 一份代码阅读笔记；
- 一份 error analysis；
- 一个被排除的假设及证据；
- 一个 Go / Pivot / Stop 判断。

没有 artifact，很难判断你是否真的推进了问题。

### 2.3 每日 5 行日志

每天结束前写 5 行：

```md
# Daily Log

- 今天回答的问题：
- 今天做了什么：
- 今天看到的证据：
- 今天最大的阻塞：
- 明天第一步：
```

日志不需要长，但要可追溯。两周后你应该能回答：你为什么做这些实验？哪些判断改变了？哪些支线已经排除？

### 2.4 四周一次 Go / Pivot / Stop

每四周做一次阶段判断：

- **Go**：主问题仍然重要，证据支持继续投入；
- **Pivot**：问题仍然重要，但当前路径信息增益低，需要换机制、换切片、换评测或换 baseline；
- **Stop**：当前方向的收益、风险或成本不匹配，停止投入，并留下清楚记录。

Stop 不是失败。干净地停止低价值方向，是研究能力的一部分。

---

## 3. 拿到一个方向后，先写清边界

很多空转来自边界不清。你以为自己在“做一个方向”，实际同时面对新任务、新数据、新模型、新评测、新工程系统和新理论框架。第一步不是开跑，而是写清楚：当前阶段究竟固定什么，观察什么，暂时不处理什么。

### 3.1 Boundary Card

```md
# Boundary Card

## 当前方向
一句话描述方向。

## 当前阶段只回答的问题
本阶段只回答一个问题：

## 已固定的东西
- Dataset / Benchmark：
- Baseline：
- Metric：
- Codebase：
- Model：

## 当前只观察或改变的变量
例如：实体频率、输入长度、检索文档数量、prompt 模板、negative sampling 策略。

## 暂时不处理的变量
例如：不换大模型、不换 benchmark、不加复杂模块、不调大规模超参。

## 本阶段 artifact
本阶段结束时交付什么：结果表、切片图、error analysis、reproduction report 等。
```

这张卡片是给你自己的。它能防止你同时打开太多不确定性。

### 3.2 第一阶段常见切口

拿到一个方向后，先从可验证、可复盘的切口进入。常见切口包括：

- 复现一个关键 baseline；
- 验证论文中的一个核心 claim；
- 检查一个已知方法的 failure mode；
- 对总体分数做 slice analysis；
- 分析 metric 是否误判真实能力；
- 比较简单 baseline 是否已经足够强；
- 检查某个方法是否依赖 hidden assumption；
- 测试一个方法在 corner case 或 OOD slice 上是否稳定。

不要一开始就把目标设成“提出一个新模型”。先建立对问题、数据和 baseline 的控制感。

### 3.3 动工前的战略过滤器

Boundary Card 回答的是“当前固定什么”。还需要回答另一个问题：这件事值不值得投入。

当一个想法需要连续投入一周以上、需要明显算力、或会影响项目主线时，先写一页 **Problem Value Filter**。它不是 proposal，也不是论文构思，而是开工前的自检。

```md
# Problem Value Filter

## What
我到底想解决什么问题？用一句不含术语堆叠的话说明。

## Current Baseline
当前最强或最可信的 baseline 怎么做？它的硬伤在哪里？

## Bottleneck
现有系统的瓶颈是信息缺失、优化困难、错误归纳偏置、数据缺陷、评测 mismatch，还是系统成本？

## My Bet
我的核心判断是什么？为什么这个改动应该影响这个瓶颈？

## Evidence Before Building
在写复杂代码之前，我已有的证据是什么？数据样本、failure mode、toy test、oracle test、简单 baseline 是否支持这条线？

## Who Cares
如果完全成功，谁会在乎？它会改变机制理解、系统能力、评测结论、实际风险判断，还是只改变一个小分数？

## Cheapest De-risking Step
最便宜的下一步是什么？半天到两天内能拿到什么信号？
```

如果最后两项答不清，先不要进入大工程。一个问题可以很小，但不能没有信息量。

---

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

## 5. 先看数据，再跑模型

很多实验问题不是模型问题，而是数据问题。不要一下载代码就跑 `python main.py`。正式训练或评测前，先直接看数据。

### 5.1 数据初检

至少做以下检查：

- 随机抽样 100 到 500 条样本，逐条看输入和标签；
- 检查 train / dev / test 的样本数是否符合论文或说明；
- 检查 label 分布、类别不平衡、空输入、异常长输入；
- 检查重复样本、近重复样本、train-test leakage；
- 检查文本是否被截断、乱码、转义错误、字段错位；
- 检查预处理后样本是否仍然可读；
- 把 tokenizer 后的 token decode 回文本，看关键信息是否丢失；
- 检查 PAD、MASK、special tokens、label alignment 是否正确；
- 检查 metric 是否真的对应任务目标；
- 建立一个粗略 human baseline：如果你自己看样本，能否判断正确？你依赖什么信息？

对于 LLM 项目，额外检查：

- prompt 是否包含完整任务说明；
- few-shot 示例是否和 test 样本泄漏或过度相似；
- 检索内容是否真的相关；
- 输出格式是否容易被 parser 错判；
- refusal、empty answer、format error 是否被单独统计；
- judge model 是否对答案顺序、语言、长度、表达风格敏感；
- 是否存在 false premise、过时知识、未定义实体等特殊输入。

### 5.2 Data Card

```md
# Data Card

## Dataset / Benchmark
名称、版本、下载位置、处理脚本。

## Task Definition
输入是什么，输出是什么，metric 是什么。

## Split
train / dev / test 样本数，split 方式。

## Label / Target Distribution
类别、答案类型或目标分布。

## Input Length Distribution
长度分布、截断比例、异常长样本。

## Obvious Noise
乱码、空字段、重复、错标、格式错误。

## Ambiguous Cases
人类也难以判断的样本。

## Possible Leakage
train-test 重复、近重复、prompt 泄漏、metadata 泄漏。

## Human Baseline Notes
我自己判断样本时依赖了什么信息？模型是否拿得到这些信息？

## Important Slices
按长度、实体频率、用户活跃度、item popularity、语言、时间、新旧知识等切片。

## Questions Before Modeling
在跑模型前需要确认的问题。
```

### 5.3 样本直觉

至少完成以下动作：

1. 随机看 100 条正常样本；
2. 随机看 50 条边界样本；
3. 看 30 条最长输入；
4. 看 30 条最短输入；
5. 看每个 label 或 answer type 的代表样本；
6. 看模型第一轮预测错误的 50 条样本；
7. 看高置信度错误样本；
8. 看 metric 判错但语义可能正确的样本。

看样本时不要只判断“对/错”。要写下：为什么错，错得是否系统，是否能形成切片。

---

## 6. 复现：建立可信参照系

复现不是“把别人代码跑起来”。复现的目的是建立一个可信的解剖台：后续所有改动、分析和结论都依赖它。

### 6.1 复现的五个层级

| 层级 | 状态 | 是否足够 |
|---|---|---|
| L0 | 环境装好，代码能跑 | 不够 |
| L1 | 跑出一个总体分数 | 不够 |
| L2 | 对齐论文主要结果或解释差异来源 | 勉强可用 |
| L3 | 理解数据流、loss、metric、关键超参和中间输出 | 可作为可信 baseline |
| L4 | 能指出方法在哪些 slice 上失败，并提出可检验假设 | 真正完成复现 |

大部分研究机会出现在 L3 到 L4 之间。

### 6.2 复现第一周 SOP

1. 记录环境：Python、CUDA、PyTorch、Transformers、GPU、OS；
2. 固定数据版本和下载地址；
3. 跑通最小 toy example；
4. 跑小数据子集；
5. 跑原始配置；
6. 对齐 metric 脚本；
7. 保存完整 config；
8. 保存 stdout / stderr / log；
9. 保存 prediction file；
10. 对比论文主结果；
11. 写出差异可能来源；
12. 做 sanity checks；
13. 读关键代码路径；
14. 输出 reproduction report。

### 6.3 保存样本级输出

每次 evaluation 都应保存 prediction file。至少包含：

```text
sample_id
raw_input
gold_label_or_answer
model_prediction
confidence_or_score
loss_per_sample
metadata / slice tags
retrieved_context_if_any
prompt_if_any
parsed_output
evaluation_result
```

没有样本级输出，就无法做 error analysis。只看总体分数，无法发现问题。

### 6.4 复现常见差异来源

结果对不上论文，不要直接说“复现失败”。先排查：

- 数据版本不同；
- train/dev/test split 不同；
- metric 实现不同；
- preprocessing 不同；
- tokenizer 或 special tokens 不同；
- max length / truncation 不同；
- batch size、learning rate、scheduler、warmup 不同；
- early stopping 规则不同；
- random seed 或多次运行方差；
- checkpoint 版本不同；
- hidden tricks：filtering、reranking、post-processing、prompt template、label verbalizer；
- 论文表格是否报告 best seed、average seed、dev 还是 test。

### 6.5 Reproduction Report

```md
# Reproduction Report

## Target Paper / Method
复现对象。

## Code and Data
repo、commit、dataset version。

## Environment
硬件、系统、核心依赖版本。

## Command
实际运行命令。

## Config
完整配置文件或关键参数。

## Result Table
论文结果、我的结果、差异。

## Difference Analysis
差异可能来自哪里？已经排除了哪些原因？

## Sanity Checks
完成了哪些 sanity checks？

## Sample Outputs
贴 10 到 20 个代表样本输出。

## Current Reproduction Level
L0 / L1 / L2 / L3 / L4。

## Next Step
下一步要做什么？
```

---

## 7. 读代码：沿着数据流，不沿着文件名

不要从 `utils.py` 开始逐文件阅读。正确方式是：拿一条真实样本，从 raw input 一路追到 final metric。

### 7.1 先找到五个入口

读一个 repo，先定位五个入口：

1. 数据在哪里读入：`Dataset` / `DataLoader` / preprocessing；
2. 输入如何变成 tensor：tokenizer、padding、mask、feature construction；
3. 模型 forward 在哪里；
4. loss 在哪里计算；
5. evaluation metric 在哪里计算。

### 7.2 Trace One Example

拿一条样本，打断点或加 print，记录以下内容：

- raw input 是什么；
- label 是什么；
- tokenizer 后的 token IDs 和 attention mask；
- decode 回文本后是否仍然正确；
- tensor shape：`[B, L]`、`[B, L, H]`、`[B, H]` 等；
- 每个关键模块输入输出 shape；
- 论文公式对应代码哪一行；
- `view`、`reshape`、`permute`、`transpose` 是否改变了语义；
- attention mask、loss mask、padding mask 是否一致；
- loss 的分母是什么；
- label 是否和 prediction 对齐；
- metric 是否用的是同一个字段；
- post-processing 是否改变输出；
- 是否有未在论文中说明的 filtering、truncation、reranking、prompt template 或 threshold。

输出一张简洁的数据流图：

```text
raw sample
→ preprocessing
→ tokenization / feature construction
→ dataloader batch
→ model.forward
→ logits / scores
→ loss
→ decoding / post-processing
→ metric
```

### 7.3 公式到代码映射

关键论文不要停留在“公式看懂”。要写出：

```md
# Equation-to-Code Map

- Paper Eq. (1): 对应 `model.py:L120-L136`
- Paper Eq. (2): 对应 `loss.py:L45-L62`
- Paper Section 3.2 的 retrieval step：对应 `retriever.py:L88-L140`
- 论文未说明但代码存在：top-k filtering、length normalization、特殊 prompt 模板
```

如果找不到公式对应的代码，要标注为风险点。

### 7.4 用 assert 让错误尽早暴露

深度学习代码经常静默失败：shape 广播错了、mask 反了、label 错位了，代码仍然能跑，loss 也会下降。关键路径上要加 assert。

常见 assert：

```python
assert input_ids.shape == attention_mask.shape
assert logits.shape[:2] == labels.shape[:2]
assert labels.min() >= -100
assert not torch.isnan(loss)
assert batch_size == input_ids.size(0)
assert decoded_text.strip() != ""
```

assert 不是为了让代码好看，而是为了让错误尽早出现。

### 7.5 AI 生成代码的使用边界

AI 编程助手可以提升写样板代码、数据转换脚本和可视化脚本的速度，但核心研究逻辑仍要你自己掌控。

以下部分可以借助 AI 起草，但要人工检查：

- 数据格式转换；
- 画图脚本；
- 日志解析；
- 表格整理；
- 单元测试模板；
- 非核心工具函数。

以下部分要先自己手推，再让 AI 帮忙实现或检查：

- 自定义 forward path；
- loss function；
- attention / padding / causal mask；
- label shift；
- negative sampling；
- retrieval reranking；
- evaluation metric；
- 与论文公式对应的特殊机制。

一个简单标准：

```text
如果你不能在纸上画出输入输出 shape、mask 含义和 loss 计算，先不要让 AI 替你写这段代码。
```

遇到复杂报错时，不要只让 AI 给“可运行补丁”。更好的问法是：

```text
请列出这个错误可能对应的 3 到 5 个底层原因；
每个原因应该用什么最小检查排除；
哪些检查能区分原因 A 和原因 B。
```

你的目标不是尽快让代码不报错，而是定位系统的真实状态。

### 7.6 不要过早重构

复现阶段不要急着把别人的 repo 改成自己喜欢的风格。先确认：

- 原始代码能跑；
- 结果能对齐；
- 关键路径看懂；
- prediction file 能导出；
- sanity checks 能通过。

在没有建立可信 baseline 前，大规模重构只会制造新的不确定性。

---

## 8. Sanity Checks：全量实验前的体检

全量实验很贵。正式跑大实验前，先用便宜检查确认 pipeline 基本可信。

### 8.1 基础检查

| 检查 | 目的 |
|---|---|
| 固定随机种子 | 确保同一配置可复现 |
| 小数据子集跑通 | 确认 pipeline 端到端可运行 |
| 查看 batch 内容 | 确认输入、label、mask、metadata 对齐 |
| decode tokenizer 输出 | 确认关键信息没有被截断或破坏 |
| verify loss at init | 检查 loss、label、softmax、初始化是否合理 |
| overfit one batch | 检查前向传播、loss 和梯度是否能工作 |
| input-independent baseline | 检查模型是否真的利用输入信息 |
| random-label test | 检查是否存在泄漏或评测脚本异常 |
| simple baseline | 确认复杂模型至少超过简单规则或多数类 baseline |
| metric hand-check | 手算几个样本，确认 metric 实现正确 |

### 8.2 Overfit One Batch

抽几条样本，关闭 dropout 和复杂 regularization，让模型在一个小 batch 上过拟合。

如果模型连一个小 batch 都无法拟合，先不要跑全量。常见原因：

- label 对齐错误；
- loss mask 写错；
- padding 参与了 loss；
- 学习率不合理；
- 梯度被截断或没有传回关键参数；
- 输入被截断到丢失关键信息；
- data augmentation 或 preprocessing 破坏了样本；
- metric 或 post-processing 使用了错误字段。

### 8.3 Verify Loss at Init

分类任务中，初始 loss 应该大致符合随机预测预期。例如 10 类均衡分类的 cross entropy 初始值应接近 `-log(0.1)`。如果偏差很大，优先检查：

- label index 是否从 0 开始；
- ignore index 是否正确；
- logits 是否进入了 softmax 两次；
- class imbalance 是否需要调整初始 bias；
- loss reduction 是否符合预期；
- batch 中是否混入空 label。

### 8.4 Visualize / Inspect Before the Model

在进入 `model(x)` 之前检查输入。这是模型真正看到的内容。

对于文本任务，至少打印：

- raw text；
- token IDs；
- decoded text；
- attention mask；
- label；
- prompt；
- truncation 后的最后 50 个 token。

对于推荐系统，至少打印：

- user id；
- item id；
- history sequence；
- negative samples；
- timestamp；
- train/test split；
- popularity bucket。

对于检索或 RAG，至少打印：

- query；
- retrieved documents；
- rank；
- score；
- 是否包含答案；
- prompt 中实际放入了哪些 context。

### 8.5 警惕异常高分

低分会促使你排查问题，异常高分反而更危险。一个简单改动如果突然大幅超过强 baseline，先不要把它当作发现，先把它当作需要解释的异常。

优先检查：

- **数据泄漏**：train / dev / test 是否重复或近重复；prompt、retrieval context、metadata 是否带入了答案；
- **特征穿越**：训练或推理时是否使用了真实部署中不可获得的信息，例如未来行为、gold label、人工标注字段；
- **评测脚本错误**：parser 是否把空答案、格式错误或多数类预测错误地判成正确；
- **多数类或模板塌陷**：模型是否只学会输出最常见答案，而总体指标恰好掩盖了 slice 退化；
- **test set 污染**：是否在观察测试样本后调整了 prompt、后处理或阈值；
- **预训练污染或 benchmark 记忆**：公开 benchmark 可能已经出现在基础模型训练数据中。对于 LLM zero-shot / few-shot 的异常高分，自己手写 20 到 50 条结构相同但内容全新的变体题，检查能力是否能迁移到新样本。

正结果也需要被“攻击”。在排除这些更简单的解释前，不要用异常高分支撑结论。

### 8.6 指标不是能力本身

指标是能力的代理，不是能力本身。一个模型在 benchmark 上涨分，可能是真的学会了目标能力，也可能只是学会了数据集、格式、judge 或评测脚本的偏好。

常见捷径包括：

- LLM judge 偏好更长、更有条理、带有固定格式的答案；
- 分类模型利用了模板、长度、标点、来源字段等伪特征；
- 推荐模型提升来自更强的 popularity bias；
- 安全模型降低 attack success 的同时大幅增加 over-refusal；
- RAG 系统通过 retrieval context 泄漏了答案；
- 后处理规则把错误格式伪装成正确答案。

所以，涨分后要抽查“做对的样本”。看模型是否真的用了你希望它使用的证据，还是走了更便宜的捷径。

---

## 9. Error Analysis：从分数进入问题本身

总体分数是高度压缩的信息。只看 `Accuracy = 85%` 或 `F1 = 72`，你不知道模型为什么错，也不知道下一步该做什么。

Error analysis 的目标是把“分数不够好”变成具体、可验证的 failure mode。

### 9.0 测试集隔离

Error analysis 原则上只在 train、dev 或 validation set 上做。Test set 的作用是最终盲测，而不是给你反复观察、改 prompt、调阈值、选 checkpoint。

一旦你看了 test set 的错误样本，并据此修改了代码、prompt、数据处理、超参或后处理规则，这个 test set 就不再是干净的最终评估。它已经变成了开发集的一部分。

更稳的做法：

- 用 dev set 做 error analysis、slice analysis 和模型选择；
- 如果公开 benchmark 的 test 标签已经可见，把它当作 dev-like 诊断集，同时留出新的 held-out set 或使用官方不可见 test；
- 所有根据 test observation 做出的改动，都要在实验记录里明确标注；
- 最终报告结果前，确认模型、prompt、超参、后处理和评测脚本已经固定。

把“脑子里的过拟合”当成代码里的过拟合一样防范。看过的样本会影响你的设计判断，这种影响很难事后清除。

### 9.1 标准流程

1. 导出样本级预测；
2. 先看总体结果；
3. 按 slice 看结果；
4. 抽 50 到 100 个错误样本人工阅读；
5. 特别看高置信度错误样本；
6. 标注错误类型；
7. 把错误类型整理成 taxonomy；
8. 写脚本在全量数据上统计这些类型；
9. 找最大、最稳定、最有解释力的 failure mode；
10. 为该 failure mode 提出 candidate cause；
11. 设计最小实验验证 candidate cause。

### 9.2 保存 prediction file

每次 evaluation 后保存类似文件：

```json
{
  "id": "sample_001",
  "input": "...",
  "gold": "...",
  "prediction": "...",
  "confidence": 0.91,
  "loss": 2.37,
  "is_correct": false,
  "slice": {
    "length_bucket": ">2k",
    "entity_frequency": "low",
    "language": "en",
    "answer_type": "date"
  },
  "retrieved_context": [...],
  "prompt": "...",
  "parsed_output": "..."
}
```

没有 prediction file，就没有可靠的 error analysis。

### 9.3 Failure Taxonomy

错误分类要具体，能指导下一步实验。不要只写“模型能力不够”。

示例：LLM QA

| 错误类型 | 现象 | 可能原因 | 下一步 |
|---|---|---|---|
| 过时知识 | 回答旧事实 | 训练知识陈旧、未使用检索 | 加 freshness slice，比较 RAG |
| false premise | 接受错误前提 | 缺少前提检测 | 加前提识别 prompt 或 classifier |
| 格式错误 | 语义正确但 parser 判错 | 输出约束不稳定 | 改 parser，单独报告 format error |
| 长文本定位失败 | 线索在中间时答错 | lost-in-the-middle | 按证据位置切片 |
| 低频实体混淆 | 混淆相似实体 | 知识稀缺或检索失败 | 按实体频率切片 |

示例：推荐系统

| 错误类型 | 现象 | 可能原因 | 下一步 |
|---|---|---|---|
| head item 偏置 | 热门 item 推荐过多 | popularity shortcut | head / tail slice |
| cold-start 失败 | 新用户表现差 | history 太短 | 按用户交互数切片 |
| sequence drift | 长序列后半段错 | 兴趣变化未建模 | 按时间跨度切片 |
| negative sampling 偏差 | 离线指标高，实际差 | negative 太容易 | hard negative 对照 |

### 9.4 Slice Analysis

人工看到的错误只是线索。下一步要写脚本验证它是否是系统性现象。

常见 slice：

- 输入长度；
- label / answer type；
- entity frequency；
- item popularity；
- user activity；
- language；
- time / freshness；
- source domain；
- prompt type；
- retrieval hit / miss；
- confidence bucket；
- evidence position；
- train-test similarity；
- data quality bucket。

好的 slice analysis 至少回答：

```text
模型整体表现如何？
在哪些 slice 上明显退化？
该退化是否稳定？
这个退化能否解释总体瓶颈？
这个退化是否指向一个可验证假设？
```

### 9.5 Corner Cases 和压力测试

不要只在舒适分布上评估 baseline。把它放到边界条件下看。

常见压力测试：

- 同义改写；
- 改变语序；
- 替换实体；
- 增加无关上下文；
- 把关键信息放到开头、中间、结尾；
- 增加输入长度；
- 跨语言或混合语言；
- 改变 prompt 模板；
- 改变 retrieval 文档顺序；
- 测试 OOD 数据；
- 对推荐系统改变用户历史长度、item popularity、时间跨度。

压力测试的目标不是“把模型打挂”，而是定位它的失效边界。

### 9.6 高分样本审查：不要只看错题

Error analysis 不只看错题。异常高分、某个 slice 突然提升、某个方法“全都答对”，同样需要抽样审查。

至少抽查三类样本：

- **模型高置信度做对的样本**：确认它是否真的用了正确证据；
- **新方法做对、baseline 做错的样本**：确认提升是否来自目标机制，而不是格式、长度或后处理；
- **整体涨分最大的 slice**：确认是否存在数据泄漏、捷径学习或 metric 偏置。

审查时问四个问题：

```text
模型为什么会答对？
它依赖的证据是否在人类看来合理？
如果去掉这个证据或加入干扰项，答案是否会变？
它是否只是迎合了评测脚本或 judge 偏好？
```

一个可信的正结果，应当能经得起这些问题。

### 9.7 建立私有 Probing Set

标准 benchmark 给你大盘表现，但很难说明机制。每个项目都应维护一个小而精的私有 probing set，用来持续检查你真正关心的能力。

建议规模：20 到 50 条样本。

样本来源：

- error analysis 中最能代表主要 failure mode 的样本；
- 手写的 corner cases；
- 反事实扰动样本；
- 加入干扰证据、误导实体或格式变化的样本；
- 你认为 SOTA 应该失败、但新机制应该成功的样本。

每条样本至少记录：

```md
- Input:
- Expected behavior:
- Target capability:
- Distractor / challenge:
- Why baseline fails:
- What would count as success:
```

Probing set 不能替代正式 evaluation，也不能反复用来调到“刚好过关”。它的作用是帮助你快速观察机制、暴露捷径、解释为什么总体分数变化值得相信。

### 9.8 Error Analysis Report

```md
# Error Analysis Report

## Experiment Background
baseline、dataset、metric、checkpoint、run id。

## Overall Result
总体指标。

## Sampling Strategy
看了哪些样本：随机错误、高置信错误、特定 slice、边界样本。

## Failure Mode Taxonomy
错误类型、比例、代表样本。

## Slice Statistics
每个 slice 的指标、样本数、置信区间或多 seed 结果。

## Key Examples
贴 10 到 20 个代表样本。

## Candidate Cause
最可能的原因是什么？还有哪些替代解释？

## Hypothesis
如果这个原因成立，我应该观察到什么？

## Minimal Experiment
下一步最便宜的验证是什么？
```

---

## 10. 从 failure mode 生成研究问题

新手常问：“怎么想 idea？”更稳的路径是：先确定系统性 failure mode，再问它为什么发生，最后设计最小修复或验证。

### 10.0 研究品味需要刻意训练

执行纪律能防止你把事情做烂，但不能自动告诉你什么事情值得做。研究品味需要单独训练。

每周留出一小段时间，不跑实验，只回答三个问题：

```text
1. 我所在小方向里，真正重要但还没解决的问题是什么？
2. 这些问题为什么现在还没解决？缺数据、缺评测、缺机制理解、缺算力、缺理论，还是社区没有问对问题？
3. 我当前项目与其中哪个重要问题有连接？连接很强、很弱，还是只是表面相关？
```

把答案写进 `docs/notes/research_taste_log.md`。不要求马上变成项目，但要持续训练“什么值得注意”的嗅觉。

培养研究品味的几个实际方法：

- 读强论文时，不只问“它怎么做”，还问“为什么这个问题值得做”；
- 读弱论文时，不只问“哪里不严谨”，还问“如果重做，哪个问题会更有价值”；
- 组会听别人项目时，练习判断：这个项目最强的 claim 是什么，最薄弱的证据是什么；
- 每个月回看自己的 `parking_lot.md`，删掉低价值想法，保留能连接重要问题的想法。

研究品味不是灵感，而是长期比较问题、证据和影响后形成的判断能力。

### 10.1 先判断：这个问题是否值得继续

不是每个 failure mode 都值得投入。一个值得继续追的 failure mode，通常满足以下条件：

- 影响重要能力、重要用户、重要数据切片或主结论，而不是只影响边角样本；
- 现象稳定，可复现，不是单个 seed、单个 prompt 或单次运行的偶然波动；
- 有可解释的候选原因，而不是只能笼统归结为“模型不够强”；
- 能设计便宜的最小验证，而不是一上来就要求完整系统和大量算力；
- 解决它会改变下一步决策，而不只是让总体分数小幅变化。

投入一周以上之前，先问：

```text
如果预期结果完全成立，谁会在乎？为什么在乎？
它揭示了机制、边界或隐含假设，还是只是在某个表格上多了一个小数点？
有没有更便宜的 proxy 能先判断方向是否值得继续？
```

如果这些问题答不清，先不要写复杂代码。把想法放进 `parking_lot.md`，回到当前最重要的 failure mode。

### 10.2 动工前的 Heilmeier-style 四问

当你准备把一个想法升级成项目主线时，用更尖锐的四问再过滤一次。尽量用普通语言回答，不用术语堆叠。

```text
1. 你到底要做什么？
2. 目前最强 baseline 怎么做？它的根本限制在哪里？
3. 你的核心直觉是什么？为什么它现在可能成立？
4. 如果成功，谁会在乎？会改变什么判断或能力？
```

第四问最容易暴露问题。如果答案只是“某个 benchmark 上可能涨 0.5”，而无法说明能力、机制、风险或实际系统判断的变化，这条线通常还没有足够研究价值。

### 10.3 不要把“没人做过”当成价值

新手常见的错误句式是：

```text
方法 A 在领域 X 用过，但领域 Y 还没人用过，所以我把 A 用到 Y。
```

“没人做过”只说明组合在文献里少见，不说明它有价值。很多组合没人做，是因为问题不重要、机制不匹配、成本过高，或者简单 baseline 已经足够强。

写新代码前，先回答四个问题：

```text
1. 我到底要解决哪个具体 failure mode？
2. 当前强 baseline 为什么解决不了？瓶颈是信息丢失、优化困难、归纳偏置不合适、评测 mismatch，还是数据缺陷？
3. 我的新机制为什么应该有效？背后的直觉是数学、统计、系统结构，还是任务先验？
4. 如果它成功，谁会在乎？它改变的是机制理解、系统能力、评测结论，还是只改变了一个小分数？
```

如果只能说“可以试试”，先不要上复杂实现。把它改写成一个可验证 hypothesis，或者放回 `parking_lot.md`。

### 10.4 观察到问题不等于形成问题

| 阶段 | 示例 |
|---|---|
| Observation | 模型在长输入上表现差 |
| Failure mode | 当证据位于输入中部时，准确率下降最明显 |
| Candidate cause | 位置偏置导致模型更依赖开头和结尾信息 |
| Hypothesis | 如果重新排序证据位置，性能会随证据位置系统变化 |
| Minimal test | 固定样本内容，只改变证据位置，画 accuracy-position 曲线 |
| Research question | 如何降低长上下文模型对证据位置的敏感性？ |

只有到了最后两行，才开始接近研究问题。

### 10.5 常见问题来源

**从论文 claim 找问题**

论文声称方法提升了 reasoning。你要问：

- 提升是否只来自更长输出？
- 是否只在某类问题上提升？
- 是否依赖 prompt 格式？
- 是否在更强 baseline 下仍成立？
- ablation 是否排除了简单替代解释？

**从 SOTA failure mode 找问题**

如果 SOTA 总体强，但在某个重要 slice 上弱，这通常比“总体涨一点”更有研究价值。

例子：

- knowledge editing 后直接问题答对，但相关问题不一致；
- unlearning 后模型拒答，但换 probe 仍暴露知识；
- 推荐模型整体 NDCG 提升，但 tail item 明显下降；
- jailbreak defense 降低 attack success，但 over-refusal 大幅上升。

**从评测 mismatch 找问题**

如果 metric 不能反映真实能力，就先研究评测。

例子：

- 模型语义正确但格式被判错；
- LLM judge 偏好长答案；
- 推荐离线 metric 被 popularity shortcut 放大；
- 安全评测只看拒答，不看任务可用性。

**从 hidden assumption 找问题**

代码和实验常有论文没有明说的假设。

例子：

- 特定 prompt template；
- 特定数据过滤；
- 特定 answer normalization；
- 只报告 best seed；
- 只在短输入上评估；
- 使用了测试集可见的 metadata。

**从简单 baseline 太强找问题**

如果简单 baseline 已经很强，要问复杂模型到底学到了什么。很多研究问题来自“为什么简单方法这么强”。

### 10.6 目标驱动，而不是机制驱动

不要因为看到一个新机制很漂亮，就马上把它加到自己的模型里。这样容易变成“拿着锤子找钉子”：先有模块，再硬找任务。更稳的做法是目标驱动：先明确你想让系统具备什么能力，再定位当前系统在哪个 failure mode 上失败，最后寻找合适的机制。

一个更好的路径：

```text
目标能力 → 当前失败场景 → 可能原因 → 候选机制 → 最小验证
```

示例：

```text
目标能力：模型能处理长上下文中的中部证据。
失败场景：证据在中部时准确率下降。
可能原因：位置偏置、attention 稀释、检索排序不稳。
候选机制：重排 evidence、分块聚合、位置扰动训练。
最小验证：固定内容，只改变证据位置，比较不同机制的 accuracy-position 曲线。
```

机制只是工具。真正要解释的是：它解决了哪个失败场景，为什么应该有效，怎样用最小实验验证。

### 10.7 抽象失败，再找解法

当你确认一个 failure mode 后，不要只在同一个小圈子里找方法。先抽象问题：

- 这是记忆问题、检索问题、校准问题、组合泛化问题、分布偏移问题，还是优化问题？
- 其他领域是否处理过类似结构？
- 有没有更简单的规则、数据处理、评测设计或 pipeline 改动可以先验证？

跨领域借鉴不是“把别人的模块硬塞进来”。它是把你遇到的失败抽象成更一般的问题，再寻找成熟机制。

### 10.8 投入大实验前，先画预期结果表

这不是论文写作训练，而是实验设计检查。

在你准备投入一周时间或大量算力前，先画一张空表或空图，写出你预期看到的趋势。可以只填方向，不一定填精确数字。这样做的价值是：提前暴露缺失的 baseline、缺失的 ablation、无意义的指标列和无法解释的结果。

```md
| Setting | Baseline | My Change | Expected Signal | Decision if Observed |
|---|---:|---:|---|---|
| Overall |  |  |  |  |
| Long input |  |  |  |  |
| Short input |  |  |  |  |
| Low-frequency entity |  |  |  |  |
| High-frequency entity |  |  |  |  |
```

然后问：

```text
如果这张表完全按预期出现，它会改变什么判断？
它能排除哪个替代解释？
它能说明机制，还是只说明分数变化？
```

如果表格填完后你发现“即使成功也没有明显信息量”，不要跑。先重新定义问题或改成更便宜的 proxy。

---

## 11. 假设与实验设计：不要随机试错

每次运行实验前，先写一张 Experiment Card。没有 hypothesis、control、metric、success signal、failure signal 和 stop condition 的实验，通常只会增加混乱。

### 11.1 Experiment Card

```md
# Experiment Card

## Core Question
这次实验在回答什么问题？

## Hypothesis
如果我的理解是对的，应该观察到什么现象？

## Minimal Test
最便宜、最快的验证方式是什么？小数据、小模型、短训练、规则、人工检查、toy / synthetic setup、proxy 是否可用？

## Control
我要和谁比？是否只改变了一个变量？

## Metric
看哪些指标？总体指标、slice 指标、成本、稳定性、over-refusal、utility 等。

## Data Slice
本实验最关心哪个 slice？为什么？

## Success Signal
什么结果支持假设？

## Failure Signal
什么结果反驳假设？

## Confounders
可能有哪些混杂因素？随机种子、数据泄漏、prompt 长度、参数量、训练预算、评测脚本。

## Time Budget
最多投入多少时间？

## Stop Condition
看到什么结果就停止这条线？
```

### 11.2 实验设计规则

1. 一次只改一个核心变量；
2. 先小规模，再全量；
3. 先验证方向性，再追求最好分数；
4. 先做 sanity check，再跑长实验；
5. 先比较简单 baseline，再比较复杂方法；
6. 每个结论至少有一个对照；
7. 每个正结果都要做至少一个替代解释排查，尤其是异常高分；
8. 每个负结果都要回答它排除了什么；
9. 不用单个 seed 支撑结论；
10. 不用总体分数掩盖关键 slice 的退化。

### 11.3 小幅提升先当噪声处理

在 ML 实验中，单个 benchmark 上的小幅提升经常只是 seed、数据切分、超参、评测脚本或随机波动。看到 `+0.3`、`+0.5` 这类改进时，先不要急着解释成机制有效。

更稳的做法：

- 报告 effect size，而不是只报告是否涨分；
- 对关键结论跑多个 seed，至少估计均值和方差；
- 看 slice，而不是只看 overall；
- 比较多个合理 baseline，而不是只挑最弱的一个；
- 对多次尝试后的最好结果保持警惕，记录你试过多少配置；
- 如果你做了大量超参搜索，结果解释时要承认这个搜索过程。

一个结果可信，通常不是因为它在某个表格上高一点，而是因为总体指标、关键 slice、样本级行为、消融、负对照和重复运行相互支持。

### 11.4 最小验证优先

不要一开始就做完整系统。先问：能不能用最便宜方式判断方向是否值得继续？

一个特别有用的最小验证是 **toy setup**：构造一个小到你完全看得懂的环境，只保留主问题的核心结构。

例子：

- 研究长上下文中部证据遗忘：先手写 100 条短序列，把证据位置作为唯一变量；
- 研究 retrieval fusion：先构造包含一个正确文档和多个干扰文档的小集合；
- 研究推荐长尾问题：先把 item popularity 分成几个清楚桶，检查简单 reweighting 是否有趋势；
- 研究 unlearning：先用少量可控事实和多个 probe 模板检查“忘记”是否只是拒答。

如果一个机制在 toy setup 中都没有方向性信号，不要直接期待它在真实复杂系统中突然有效。toy setup 不能证明最终结论，但能快速暴露机制是否可能成立。

常见 minimal test：

- 用 5% 数据跑一个趋势；
- 用小模型替代大模型；
- 只跑一个关键 slice；
- 用规则或 oracle 上限估计空间；
- 人工标注 50 个样本验证错误类型；
- 只改 prompt，不改模型；
- 只替换 retrieval 排序，不重新训练；
- 只在 frozen embedding 上做 linear probe；
- 只做 inference-time intervention。

最小验证不是为了证明最终结论，而是为了决定下一步是否值得投入。

### 11.5 寻找上限：Oracle / Ceiling Test

不要只问“能不能比 baseline 好”。还要问“这个模块最多值得优化多少”。

当你怀疑系统瓶颈在某个局部模块时，先做一次 oracle test：用人工标注、gold evidence、gold retrieval、gold rationales、perfect labels 或手工规则，模拟这个模块达到理想状态时，端到端指标最多能提升多少。

例子：

- RAG：如果把 gold document 强行放在 top-1，总体 QA 准确率只涨 1 到 2 点，说明主要瓶颈可能不在 retrieval；
- 多步推理：如果给模型 gold intermediate steps 仍然答错，瓶颈可能在 final inference 或 answer formatting；
- 推荐：如果用真实未来点击作为 reranker 上限仍然不能改善目标 metric，可能是 metric 或 candidate generation 有问题；
- unlearning：如果 oracle refusal policy 仍然严重损伤 retain set，说明评测或目标函数存在结构性冲突。

Oracle test 不能作为真实方法，但它能告诉你天花板在哪里。没有上限判断的局部优化，容易把几周时间花在非瓶颈模块上。

### 11.6 结果解释三句话

出结果后，不要只说“涨了”或“没涨”。写三句话：

```text
事实：我观察到了什么？总体指标、slice 指标、方差、代表样本是什么？
解释：这个结果支持、削弱还是反驳了哪个假设？替代解释是什么？
行动：下一步 Go / Pivot / Stop？具体做什么？
```

示例：

```text
事实：总体 EM 没变，但证据位于中部的 slice 提升 4.2，开头和结尾 slice 基本不变。
解释：这支持位置敏感性是主要瓶颈之一，但不能排除 prompt 长度变化带来的影响。
行动：下一步固定 prompt 长度，做 evidence-position controlled test；如果趋势保留，再扩展到全量。
```

---

## 12. 项目管理与信息外化：建立 Project OS

很多项目在中后期变得难以推进，不是因为缺少想法，而是因为信息开始熵增：想法停在脑子里，决定散落在聊天记录里，数据版本互相覆盖，实验结果找不到对应代码，几周后已经说不清某个数字从哪里来。

Project OS 的目标不是增加流程负担，而是让研究过程保持可追溯、可恢复、可协作。大脑适合做推演，不适合当硬盘。关键上下文一旦没有外化，项目就会逐渐变成信息考古。

基本原则：

- **单一事实源**：问题边界、关键决定、数据版本、主结果和待办，都有固定位置；
- **轻量纯文本**：优先使用 Markdown、YAML、JSONL、CSV 等可 diff、可搜索、可复制的格式；
- **靠近代码仓库**：项目文档放在 repo 里，和代码一起 version control；
- **低维护成本**：文件少而稳定，宁可简陋，也不要变成需要额外维护的大系统；
- **证据可回放**：任何关键结果都能追溯到 code commit、data version、config、command、seed、prediction file 和 evaluation script；
- **定期清理**：每周固定做一次项目垃圾回收，防止临时文件、临时决定和临时想法淹没主线。

### 12.1 推荐 repo 结构

结构不需要复杂，但要让三个月后的自己和合作者都能快速恢复上下文。

```text
project/
  README.md
  configs/
  data/
    raw/                  # 原始数据，只读；通常不进 Git，只保留 README / manifest
    processed/            # 由脚本生成的中间数据
    manifests/            # checksum、schema、split 信息
  docs/
    01_project_overview.md
    02_decision_log.md
    03_meeting_notes.md
    04_data_model_provenance_log.md
    05_task_list.md
    06_result_log.md
    weekly/
    notes/
  scripts/
    01_preprocess_data.py
    train.sh
    predict.sh
    evaluate.sh
  src/
  runs/
    20260419_exp001/
      config.yaml
      command.txt
      stdout.log
      stderr.log
      metrics.json
      predictions.jsonl
      notes.md
  analysis/
    data_check.md
    error_analysis.md
    slice_analysis.ipynb
  archive/
  parking_lot.md
```

`docs/` 是项目控制面板，`runs/` 是实验账本，`analysis/` 是分析现场，`archive/` 存放不再使用但暂时不想删除的旧脚本和旧结果。

### 12.2 六个核心文档

建议在每个项目的 `docs/` 下保留以下六个 Markdown 文件。它们不需要写得长，但要持续更新。

#### 1. `01_project_overview.md`：项目控制台

离开项目几天后再回来，先读这个文件恢复上下文。只保留当前最重要的信息。

```md
# Project Overview

## One-Sentence Research Question
本项目到底在回答什么问题？

## Fixed Setting
Dataset / baseline / metric / model / codebase 中哪些已经固定？

## Current Phase
数据检查 / baseline 复现 / error analysis / 最小验证 / ablation / 阶段复盘。

## This Week's Only Priority
本周唯一优先回答的问题是什么？

## Current Artifact
本周要交付什么可复盘产物？

## Key Links and Paths
- Code repo:
- Data path:
- Run directory:
- Main result log:
- Draft / shared doc if any:

## Last Updated
YYYY-MM-DD
```

这个文件的价值在于限制发散。每次想开新支线前，先看它是否服务于本周唯一优先问题。

#### 2. `02_decision_log.md`：关键决定日志

研究中最容易丢失的是“当初为什么这样做”。每个关键分岔都应留下一条记录。

```md
# Decision Log

## [YYYY-MM-DD] Decision: 简短标题

### Context
当时遇到了什么问题？

### Options Considered
A / B / C 分别是什么？

### Decision
最终选择了什么？

### Reason
为什么这样选？依据是什么？

### Scope
影响了哪些代码、数据、config、metric 或实验？

### Follow-up
后续需要补什么验证？
```

示例：

```md
## [2026-04-16] Decision: 将 max_length 从 4096 暂时固定为 1024

### Context
4096 设置在当前 A100 上频繁 OOM，导致 baseline 复现周期过长。

### Options Considered
1. 继续优化 4096 训练；
2. 暂时切到 1024，先完成 baseline 和 error analysis；
3. 换更小模型。

### Decision
当前阶段固定 max_length=1024。

### Reason
数据初检显示 95% 的关键证据出现在前 1000 tokens 内；长文本位置效应单独作为 robustness slice 处理。

### Scope
更新 preprocessing config、training config 和 result log。

### Follow-up
第 9 周补 evidence-position / long-context slice。
```

半年后有人问“为什么当时不测 4096”，你不需要回忆聊天记录，直接回到这条决定。

#### 3. `03_meeting_notes.md`：结构化会议纪要

会议结束时如果没有记录，很多“达成一致”其实只是暂时的错觉。每次 1-on-1、组会或合作者讨论后，用同一格式补上纪要。

```md
# Meeting Notes

## YYYY-MM-DD

### Participants

### Context
这次会议前的状态是什么？

### Decided
已经确定的事情：固定了什么，否定了什么，下一步优先什么。

### Open Questions
仍未解决的问题和争议点。

### Action Items
| Owner | Action | Deliverable | Due Date |
|---|---|---|---|
| | | | |
```

Action item 应写成可执行动作，而不是模糊状态。例如“周三前交出 50 个 bad cases 的人工归类表”，比“继续做 error analysis”更有用。

#### 4. `04_data_model_provenance_log.md`：数据与模型来源记录

AI 项目里，数据 split、预处理和 checkpoint 的混乱会直接破坏可复现性。这个文件记录所有会影响结果的外部来源和版本。

```md
# Data and Model Provenance Log

## Raw Dataset
- Name:
- Source URL / storage path:
- Download date:
- Version / commit / release tag:
- License / access restriction:
- Checksum if available:

## Split
- Split script:
- Split seed:
- Train / dev / test size:
- Any filtering rules:

## Preprocessing
- Script:
- Config:
- Output path:
- Output checksum / manifest:
- Known caveats:

## External Annotation or LLM-Generated Data
- Model / API version:
- Prompt version:
- Date:
- Sampling parameters:
- Post-processing:

## Base Model / Checkpoint
- Model name:
- Source:
- Revision / commit:
- Checkpoint path:
- License / access restriction:
- Fine-tuning provenance if any:
```

如果原始链接失效、数据集作者更新了版本、API 模型悄悄变化，provenance log 是你恢复事实的依据。

#### 5. `05_task_list.md`：动词驱动的待办清单

研究拖延经常不是工作量太大，而是下一步动作不明确。任务要写成能立刻执行的动作。

```md
# Task List

## Next
- [ ] 

## Waiting
- [ ] 

## Done
- [x] 
```

不好的写法：

```text
推进数据处理
优化代码
研究 baseline
```

更好的写法：

```text
写脚本统计 train/dev/test 的 label distribution
排查 dataloader.py 第 45 行 attention_mask shape 是否和 input_ids 对齐
用 20 条样本跑通 predict.py 并导出 predictions.jsonl
```

一个好的 task 应该包含动词、对象、交付物，必要时加上文件名或路径。

#### 6. `06_result_log.md`：主线结果账本

不是所有 run 都值得放进主表。`06_result_log.md` 只记录会影响项目判断的关键结果。

```md
# Result Log

| Date | Run ID | Question | Method / Setting | Dataset / Slice | Metric | Seed | Commit | Config | Prediction File | Takeaway |
|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |
```

每一行都要能回答：这个结果改变了什么判断？如果只是一次无信息的重复或明显失败的坏 run，放在 `runs/` 目录里即可，不需要进入主线结果账本。

### 12.3 Raw data 只读，所有中间数据由脚本生成

后期大量不可复现问题，往往不是模型阶段产生的，而是早期数据准备阶段留下的。

数据工程遵守三条规则：

1. **Raw data 只读**：下载得到的原始数据放在 `data/raw/`，不手动改、不覆盖、不用 Excel 或文本编辑器“顺手修一下”。发现错别字、空行、乱码或坏样本，也通过脚本处理，并在 provenance log 中记录。
2. **中间数据由代码生成**：清洗、去重、格式转换、过滤、采样、切分，都写成可复用脚本，脚本和 config 进入 Git。
3. **训练数据可重建**：只要保留 raw data、预处理脚本、config 和 split seed，就应能重新生成同一份 processed data。

如果数据太大无法进 Git，至少保留 manifest、schema、checksum、生成命令、存储路径和访问权限说明。

### 12.4 实验命名规范

不要用 `test1`、`final`、`final2`、`new_new`。使用可追溯命名：

```text
YYYYMMDD_task_method_dataset_seed
20260419_ke_memit_zsre_seed42
```

每个 run directory 至少保存：

- config；
- command；
- stdout / stderr；
- metrics；
- predictions；
- notes；
- code commit 或 uncommitted diff；
- data / model provenance 指向。

### 12.5 Experiment Log：run-level 证据

每个关键 run 至少记录以下内容。短实验可以简化，但长实验、主结果和任何可能进入结果表的实验都应完整记录。

```md
# Experiment Log

## Run ID
YYYYMMDD_project_exp001

## Date

## Core Question
这次 run 回答什么问题？

## Code Commit
Git commit hash；是否有 uncommitted changes；如有，diff 保存在哪里。

## Data Version
数据集版本、split、预处理脚本 hash、data manifest 路径。

## Model Version
base checkpoint、fine-tuned checkpoint、adapter、tokenizer revision。

## Config
完整 config 路径或关键参数。

## Command
实际命令。

## Seed

## Hardware
GPU、CPU、memory。

## Runtime
训练与评测时间。

## Metrics
总体指标与关键 slice 指标。

## Prediction File
保存路径。

## Observations
loss 曲线、异常、代表样本。

## Interpretation
结果说明什么？排除了什么？

## Next Step
下一步动作。
```

实验记录不是事后补材料，而是证据链的一部分。没有 provenance 的结果，即使数字很好，也很难支撑判断。

### 12.6 算力直觉与集群礼仪

算力是公共资产，也会塑造实验节奏。更大的 GPU 不能替代更清楚的问题定义。

基本习惯：

- **开跑前做费米估算**：提交大任务前，用 3 分钟估算训练时间、显存和 IO。样本数 × 单步耗时 × epoch 大约需要多久？参数、梯度、优化器状态、activation、KV cache 是否塞得进当前 GPU？CPU 预处理和磁盘读取能否喂饱 GPU？如果第一个反馈信号要 10 天后才出现，先缩小问题。
- **先极小环境，再上集群**：用 10 条样本在本地或小 GPU 上跑通 dataloader、forward、loss、predict 和 metric。不要用昂贵 GPU 排查 `KeyError`、路径错误或 parser bug。
- **大实验前 5 分钟留在屏幕前**：观察 log、显存、GPU utilization、CPU utilization 和数据读取速度。如果显存占满但 GPU utilization 很低，通常是数据读取、tokenization、IO 或 batch construction 在拖慢。
- **OOM 不只靠减 batch size**：先检查 eval 是否用了 `torch.no_grad()`，loss 或 logits 是否被完整保存导致计算图无法释放，gradient accumulation 是否正确，sequence length 是否异常，cache 是否被无意保留。
- **避免低信息长跑**：如果一个实验需要跑三天，它应回答一个清楚的高价值问题，并且已经通过小规模 sanity check。
- **先找趋势，再堆算力**：在无卡、单卡、小模型或 5% 数据上至少看到方向性信号，再考虑大规模训练。大算力适合验证清楚假设，不适合替代思考。
- **提交任务前写清问题**：每次排队前写一句话：这个 run 在验证哪个假设？如果结果为正或为负，我会怎么决策？

好的算力使用不是“能跑多大”，而是用尽可能低的成本获得能改变判断的信息。

### 12.7 拒绝幽灵实验

幽灵实验指跑完后无法复现、无法解释、无法定位版本的实验。它们占用了算力，却不能成为证据。

跑长实验前，先确认：

- 当前代码已经 commit，或明确记录了 uncommitted diff；
- config、command、seed、data version、model version、checkpoint、环境版本已经保存；
- stdout / stderr 被写入日志；
- prediction file 和 metrics 有固定输出路径；
- run id 能对应到代码、数据、配置和结果。

如果一个实验跑出好结果，但找不到当时改了什么、用了哪份数据、跑的哪条命令，这个结果就不能支撑任何判断。

### 12.8 拆分训练、预测和评测

不要把训练、推理和算分全部塞进一个巨大的 `main.py`。建议至少拆成三类入口：

```text
train.py       # 训练并保存 checkpoint
predict.py     # 读取 checkpoint，导出 prediction file
evaluate.py    # 读取 prediction file，计算 metrics 和 slice metrics
```

这种拆分有两个好处：

1. 评测脚本或 slice 定义出错时，可以快速重算，不需要重新训练或推理；
2. prediction file 成为稳定中间产物，便于 error analysis、人工检查和复现实验。

早期验证可以写短脚本，但关键流程要有清楚边界。代码结构服务于可追溯性，不服务于形式美观。

### 12.9 合作者接入时的基本契约

项目一旦有合作者接入，隐性假设会迅速变多。合作低效往往不是能力问题，而是大家以为自己在用同一套代码、同一份数据、同一个 baseline，但实际上并不是。

合作开始时先写清楚：

- **Single Source of Truth**：主代码仓库在哪里，主分支是什么，标准工作路径在哪里；
- **数据与模型版本**：标准 split、preprocessing config、baseline checkpoint 和 tokenizer revision 是什么；
- **结果账本**：哪些结果进入 `06_result_log.md`，谁负责更新；
- **主线与探索**：哪些实验是会影响主结论的主线实验，哪些只是局部探索；
- **合并方式**：代码通过 pull request、patch、还是共享分支合入；不要通过聊天软件互传 zip 合并代码；
- **会议纪要**：每次关键讨论后，谁负责把 decided / open questions / action items 写入 `03_meeting_notes.md`。

这不是形式主义，而是让每个人减少猜测。

### 12.10 Git 分支、环境和大文件

Project OS 只记录信息还不够，代码和环境也要能恢复。

**分支策略**：

- `main` 或 `master` 保持可运行；
- 每个较大实验变体使用单独分支，例如 `exp/long-context-rerank`；
- 小修小改可以直接 commit，但不要让一个分支同时混入多个研究假设；
- 合并前写清楚：这个分支改变了什么变量，是否改变数据处理或评测。

**环境可复现**：

至少保存一种环境描述：

```text
environment.yml / requirements.txt / pyproject.toml / uv.lock / conda-lock.yml / Dockerfile
```

长项目中，CUDA、PyTorch、Transformers、tokenizers、vLLM、flash-attn、评测库的小版本变化都可能影响结果。关键 run 的 experiment log 里要记录环境版本；如果多人协作或依赖复杂，优先使用 lock file 或 Docker。

**大文件管理**：

数据、checkpoint、embedding、prediction dump 通常不直接进 Git。它们至少要有：

- 稳定存储路径；
- checksum 或 manifest；
- 生成脚本和命令；
- 访问权限说明；
- 是否可公开或需要保密的标记。

如果组内已有 DVC、Git LFS、对象存储或模型仓库规范，遵守统一规范。没有统一规范时，也不要把 `final_model.pt` 这种大文件散落在个人目录里。

### 12.11 每周 30 分钟 Project GC

Project OS 不是开题时建好文件夹就结束了。和代码需要 garbage collection 一样，项目也需要固定清理节奏。

每周结束前花 30 分钟做一次 Project GC：

1. **更新 overview**：把当前问题、阶段、本周产物和下周唯一优先任务写入 `01_project_overview.md`；
2. **同步决定**：把本周关键选择补进 `02_decision_log.md`；
3. **整理会议**：把会议纪要中的 action items 转成 `05_task_list.md` 里的具体任务；
4. **结算结果**：把会影响判断的主结果写入 `06_result_log.md`；
5. **清理临时文件**：把 `test_tmp.py`、`result_new_v2.csv`、`debug_final.ipynb` 这类临时文件归档到 `archive/`，或确认无用后删除；
6. **提交代码**：确认关键代码、配置、文档已经 commit；需要共享时 push 到远端。

高效的研究者通常不是记忆力更好，而是更早把整理变成固定动作。

## 13. 沟通与协作：你是项目 owner

科研不是单人闭门完成的。及时沟通不是能力弱，而是缩短反馈回路。

你需要逐渐从“等别人给下一步”转成“主动管理项目上下文”。导师和 mentor 是高级顾问，不是项目经理。你离数据、代码和日志最近，所以你最清楚当前证据是什么、风险在哪里、下一步有哪些选项。

### 13.1 48 小时规则

同一个 bug、实验异常、理论推理或决策问题，如果连续卡住超过 48 小时，应主动求助。

求助前先整理证据，不要只发一句“跑不通”。48 小时规则不是鼓励浅尝辄止，而是防止你在错误方向上安静地消耗一周。

### 13.2 主导 1-on-1 会议

不要把 1-on-1 当成被动答辩。开会前 2 小时，先更新 `01_project_overview.md`、`05_task_list.md` 和必要的 `02_decision_log.md`。这三个文件能让导师或 mentor 快速恢复上下文：你当前在回答什么问题，已经做了什么决定，下一步准备做什么。

然后发一份很短的 agenda。建议结构如下：

```md
# 1-on-1 Agenda

## Context
上次我们决定验证什么？预期看到什么？

## Facts
这次实际观察到了什么？附结果表、slice 图、bad cases 或日志。

## Interpretation
我认为这些结果说明了什么？支持或反驳了哪个假设？

## Options
接下来有哪两个或三个选项？各自成本和风险是什么？

## My Proposal
我倾向于先做什么？想请您重点判断哪里可能有盲区。
```

好的会议不是把所有细节讲完，而是让关键决策更快发生。会议结束后，把 decided / open questions / action items 补回 `03_meeting_notes.md`，再把 action items 转成 `05_task_list.md` 里的具体任务。

### 13.3 标准求助格式

```md
# Help Request

## 我在做什么
我正在验证：____。

## 当前异常
我观察到：____。

## 证据
- log：
- result table：
- prediction examples：
- tensor shape / screenshot：

## 我已经排查了什么
1. ____，结果是 ____。
2. ____，结果是 ____。
3. ____，结果是 ____。

## 我目前的假设
我怀疑原因是：____。

## 我准备下一步做什么
我倾向于先做：____。

## 我希望得到的反馈
我想确认排查方向是否合理，或者是否应优先检查其他原因。
```

好的求助方式是带着证据和选项来讨论，而不是把问题直接转交给别人。

### 13.4 面对一个初步无效的 idea

某个 idea 初步无效 时，不要只汇报“无效”。你需要回答：

- 它在哪个 setting 下无效？
- 是总体无效，还是只在某些 slice 上无效？
- 是否通过了 sanity checks？
- 有没有一个接近的变体显示弱信号？
- 负结果排除了哪个假设？
- 下一步是 Go、Pivot 还是 Stop？

更好的做法是，在原始 idea 附近做一个很小的探索：微调一个条件、换一个切片、加一个简单对照、构造一个更容易成功或更容易失败的 toy case。目标不是证明原始 idea 对，而是判断它为什么不对，以及附近是否存在更清楚的问题。

一个更好的汇报：

```text
原始设定没有提升，总体 F1 下降 0.8。
但在长输入 slice 上提升 2.1，短输入 slice 下降 1.5。
我排查了 seed 和 metric，结果稳定。
这说明机制可能只对长输入有效，但引入了短输入噪声。
我建议下一步只在长输入上做 controlled test，并加入一个 gating baseline。
```

### 13.5 当你和导师判断不一致

你离数据和代码最近，导师通常离全局问题和学术判断更近。分歧不一定说明谁错了，通常说明双方掌握的信息不同。

不要用情绪或直觉反驳，也不要沉默执行一个你已经高度怀疑的方向。更好的做法是把分歧转成可验证问题：

```text
我们分歧的具体判断是什么？
我看到的证据是什么？
导师担心的风险是什么？
能否设计一个 1 到 2 天的 proxy test 来区分这两种判断？
```

汇报时避免说“我觉得这个不行”。改成：

```text
我担心这个方向的主要风险是 B。
我用 30 条样本做了一个小测试，结果显示 B 在 18 条里出现。
我建议先做一个更便宜的变体 C，确认是否能绕开 B，再决定是否进入完整实现。
```

有证据的不同意见是项目贡献；没有证据的抵触只会增加沟通成本。

### 13.6 防御性最小验证

导师或合作者提出一个很大的 idea 时，不要直接投入完整工程。先用最小验证把它压成一个可检验信号。

做法：

1. 抽 20 到 50 条典型样本；
2. 用最简单的规则、硬编码、人工 oracle、手写 prompt 或小脚本模拟核心机制；
3. 记录它解决了哪些 case，又引入了哪些新错误；
4. 在下一次讨论中带着样本和结果汇报。

标准汇报：

```text
我用一个小切片快速模拟了这个思路。
它确实解决了 A 类错误，但系统性引入了 B 类错误。
如果全面实现，风险是 ____。
我建议先 Pivot 到较小版本 C，或者先做 oracle test 看上限是否值得。
```

这不是“拒绝导师 idea”，而是用高密度证据帮助团队更快看清真实约束。

### 13.7 读懂支持信号，主动提出转向

导师是否仍然支持一个项目，通常不只体现在一句“继续做”。你要观察讨论中反复出现的信号：

- 讨论是否仍围绕主问题，而不是只剩工程细节；
- 导师是否愿意继续帮你排序下一步；
- 新结果是否还在改变判断，还是每周都只是重复“再试试”；
- 你的 result log 是否连续几周没有信息增量；
- 是否已经出现明确的天花板、强 baseline 反证或成本失衡。

如果连续两到三周没有信息增量，不要等别人宣布停止。主动准备 Go / Pivot / Stop note，带着证据讨论。成熟的研究者不是一直坚持，而是知道坚持什么、放弃什么，以及为什么。

### 13.8 不懂就立即澄清

讨论公式、算法、代码或文献时，如果你没有听懂，不要点头跳过。直接说：

```text
这里我没有完全理解。您能不能再解释一下这个变量 / 假设 / 结论的含义？
```

这不是打断讨论，而是在保护项目。一个没有澄清的误解，可能让你沿着错误理解做两周。承认“不知道”通常比假装听懂成本低得多。

---

## 14. 常见陷阱与处理方式

### 14.1 读了很多论文，但没有想法

原因通常不是读得不够，而是没有带问题读。

处理方式：

- 每篇论文写 paper card；
- 每 10 篇论文整理 literature map；
- 每篇论文读完后写一个 action item；
- 只深读与你当前核心问题直接相关的论文；
- 读论文时标注 claim、assumption、evidence、boundary。

### 14.2 复现了，但不知道下一步

说明复现停在 L1 或 L2。继续做：

- 导出样本级预测；
- 读 50 到 100 个错误样本；
- 做 slice analysis；
- 检查 hidden assumptions；
- 找一个最大 failure mode；
- 为它设计 minimal test。

### 14.3 一直调参，没有进展

处理方式：

- 停止大规模调参；
- 写清当前 hypothesis；
- 检查 sanity checks；
- 固定其他变量，只改一个参数；
- 记录每次调参改变了什么判断；
- 如果只是试运气，暂停。

### 14.4 看不懂代码

处理方式：

- 不要从文件名读；
- 从一条样本开始 trace；
- 找 data、forward、loss、metric 五个入口；
- 每走一步记录 shape；
- 把论文公式映射到代码行；
- 找不到映射的地方标为风险点；
- 不要急着重构。

### 14.5 结果和论文不一样

处理方式：

1. 确认数据版本和 split；
2. 确认 metric；
3. 确认 preprocessing；
4. 确认 checkpoint；
5. 确认超参；
6. 跑多个 seed；
7. 检查是否报告 best seed；
8. 保存差异表；
9. 如果仍不同，写 reproduction gap report。

结果不一致本身也可能是有价值的发现，但前提是你能清楚说明差异来自哪里，或已经排除了哪些来源。

### 14.6 不知道什么时候该停

停下来的条件包括：

- 关键假设被干净地反驳；
- 改进只来自不重要 slice；
- 结果对 seed 或 prompt 极不稳定；
- 相比简单 baseline 没有信息增量；
- 成本太高，反馈周期太长；
- 继续做不能改变主判断；
- 需要资源明显超出当前阶段。

停止时写 Go / Pivot / Stop note，留下可复用经验。

### 14.7 总想把系统做完整

新手常把“系统完整”当成进展。研究初期更重要的是最小验证。

处理方式：

- 先用最简单 pipeline 验证关键假设；
- 不要在信号出现前做复杂工程；
- 不要为了未来可能用到的功能重构；
- 能用 20 行脚本验证的，不要先写框架。

### 14.8 AI 编程助手的静默错误

AI 工具可以帮助解释报错、生成样板代码、整理表格和写临时脚本，但它不能替你承担研究判断，也不能替你保证深度学习代码的语义正确。

最危险的部分是张量操作和 mask 逻辑。AI 很容易写出语法正确、shape 也对得上、但语义完全错位的代码，例如：

- `.view()`、`.reshape()`、`.permute()`、`.transpose()` 用错维度；
- attention mask、loss mask、padding mask 方向相反；
- batch 维和 sequence 维被混合；
- label shift 错一位；
- logits 和 labels 在 post-processing 后不再对齐；
- eval loop 忘记关闭梯度，导致显存持续增长。

对于核心 forward 路径、loss 计算、mask、label alignment、metric 和数据切片，AI 生成的代码要逐行人工审查，并配合 assert、toy example 和手算样本验证。

使用边界：

- 可以让 AI 解释报错，但你要用最小例子验证；
- 可以让 AI 生成分析脚本，但你要检查统计口径和样本；
- 可以让 AI 总结非核心论文，但不能替代核心 baseline 的原文阅读和 paper card；
- 可以让 AI 提供候选思路，但你要转成 hypothesis 和 minimal test；
- 可以让 AI 写代码补丁，但更应该让它先给诊断清单：可能原因、最小检查、如何区分不同原因。

任何进入实验 pipeline 的代码，你都要能解释。AI 是协助你加快外部化和排查的工具，不是替你建立系统直觉的替代品。

### 14.9 只报好结果，隐藏干净的负结果

研究记录要包含负结果。只报好结果会让整个项目判断失真，也会让合作者重复踩同一个坑。

一个负结果有价值的前提是闭环清楚：

- 具体设置；
- 结果；
- sanity checks 是否通过；
- control 是否公平；
- 它反驳了什么；
- 它排除了哪条看似合理的路线；
- 下一步是否 Go / Pivot / Stop。

如果代码可信、对照干净、变量受控，发现“某个热门机制在当前场景下无效或有害”，这不是丢人的结果，而是项目资产。把它写入 result log 或 internal note，避免全组以后继续为同一个错误选项付费。

### 14.10 把“忙”当成“推进”

常见伪工作：

- 无目的刷 arXiv；
- 长时间盯训练进度条；
- 反复重构不影响结论的代码；
- 没有假设地调参；
- 收集很多结果但不解释；
- 写很长过程汇报但没有结论；
- 遇到异常不记录、不定位，只重跑。

有效推进通常长这样：

- 看真实样本；
- 手算几个 metric；
- trace 一条数据流；
- 写出 failure taxonomy；
- 把观察变成 slice 统计；
- 设计最小实验；
- 用结果更新 Go / Pivot / Stop。

### 14.11 ArXiv 焦虑与文献逃避

新手常见两种相反问题。

第一种是过度刷文献。每天看到新论文、新榜单、新模型，就觉得自己的方向失去价值。实际上，大多数新结果只和你当前核心问题有间接关系。不要让信息流持续打断本周主问题。

第二种是用阅读逃避行动。读论文比面对报错、坏结果和混乱数据更舒服，所以很容易连续读两周，却没有任何实验、样本分析或判断更新。

停止阅读的条件很简单：如果一篇论文读完后，你写不出与当前项目相关的 action item，就先关掉 PDF。回到数据、代码或实验。等遇到具体 gap，再回来查文献。

### 14.12 沉迷工具与过度工程

现象：baseline 没跑通，prediction file 还不能导出，却花很多时间配置复杂环境、重构 repo、封装框架、搭建看板。

处理方式：

- 验证核心假设时，允许代码简短、直接、不优雅；
- 信号明确前，不做大规模重构；
- 同一段代码被反复使用、需要多人协作或已经进入稳定实验后，再清理结构；
- 能用 20 行脚本回答的问题，不要先写一个框架。

先获得可信信号，再做工程化。顺序反了，项目会被工具本身拖住。

### 14.13 把自我评价和实验结果分开

实验失败、分数下降、环境配不好，只说明当前假设、代码或系统还有问题，不说明你这个人不适合研究。

需要训练的是把坏结果转化成信息：

```text
这个负结果排除了什么？
它暴露了哪个环节最可疑？
我能否构造一个更小的例子验证？
下一步是 Go、Pivot 还是 Stop？
```

一个干净的负结果，比靠随机种子碰到的正结果更有价值。不要隐藏坏消息。越早暴露问题，越容易修正方向。

### 14.14 精力卫生

研究需要长时间和不确定性相处。连续熬夜、长时间盯训练进度、一天切换十个方向，通常会降低判断质量。

更可持续的节奏是：每天保留一段不被打断的深度工作时间，用来做最关键的 action，例如看样本、trace 代码、写 experiment card、分析错误。其余时间处理脚本、环境、邮件和零碎沟通。

不要用工作时长替代工作质量。一天真正推进一个判断，通常比十二小时无目的忙碌更有价值。

### 14.15 ArXiv 撞车焦虑

现象：你刚开始做一个想法，发现 arXiv 上出现了非常相近的新论文，于是立刻觉得方向失去价值。

处理方式：先读清楚，再决定。相似 idea 不等于相同问题、相同证据或相同边界。重点检查：

- 它解决的是不是同一个 failure mode；
- 它是否只在某个 narrow setting 有效；
- 它的 limitation、ablation 和 bad cases 里留下了什么问题；
- 它是否能成为你当前工作的更强 baseline；
- 你的角度是否可以转向更深入的机制解释、更严格的评测或更重要的 slice。

撞车不一定意味着停止。有时它说明这个问题确实重要，也说明你需要更快转向“比它多知道什么”。

### 14.16 和稻草人 baseline 打架

现象：你设计了复杂模型，终于超过了一个弱 baseline，但没有比较最简单、最便宜、最强的替代方法。最后发现复杂系统打不过多数类、BM25、热门推荐、长度规则、简单 reranking 或一个更合理的 prompt。

处理方式：在引入复杂机制前，先建立低成本下限和简单强 baseline。

例子：

- 分类任务：多数类、关键词规则、linear probe；
- 检索任务：BM25、dense retriever 默认配置、长度或 overlap heuristic；
- 推荐任务：全局热门、用户最近交互、item popularity bucket；
- LLM 任务：zero-shot、few-shot、简单 instruction、RAG/no-RAG 对照；
- 安全任务：无防御、简单拒答模板、输入过滤、utility-preserving baseline。

复杂方法的价值不在于打败一个故意很弱的对手，而在于解释它相对简单强 baseline 多解决了什么问题。

### 14.17 分数迷信和捷径学习

现象：模型在榜单或总体指标上提升，但人工查看样本后发现，它并没有学会目标能力，只是学会了迎合数据集、prompt、judge 或 metric 的偏好。

处理方式：

- 除了错题，也抽查做对的样本；
- 对提升最大的 slice 做人工检查；
- 加入反事实扰动，看答案是否随关键证据变化；
- 比较简单强 baseline，确认复杂机制不是在利用同一个捷径；
- 建立 20 到 50 条私有 probing set，持续检查你最关心的能力。

分数是入口，不是结论。结论来自分数、样本、切片、对照、消融和机制解释的一致性。

### 14.18 用模板替代思考

现象：卡片、日志、周报都填了，但项目没有推进。所有文字都像流水账，没有一个地方说明“这个结果改变了什么判断”。

处理方式：

每个模板最后都加一句：

```text
So what? 这条记录改变了什么下一步？
```

如果答不上来，说明你只是完成了流程动作，而不是完成了研究动作。模板的价值是暴露判断，不是制造整齐感。

### 14.19 长期负结果期

现象：连续几周没有正结果，开始怀疑自己或项目，逐渐减少沟通，只靠更多实验维持“还在推进”的感觉。

处理方式：

把负结果分成三类：

- **脏负结果**：sanity check 没过、代码不可信、控制变量混乱。这类不能解释，先修系统；
- **窄负结果**：只说明某个实现、某个超参、某个 slice 不行。这类需要继续定位；
- **干净负结果**：代码可信、对照清楚、替代解释基本排除，说明一个假设不成立。这类是正式进展，应写进 result log 或 internal note。

长期负结果期最重要的是保持判断力。每周至少回答一次：我是在排除有价值的不确定性，还是在重复低信息实验？如果是后者，准备 Pivot 或 Stop。

---

## 15. 向外看：在社区中定位你的发现

研究不是只在本地 repo 里推进。几个月后，你需要知道自己的发现和社区的关系：别人是否已经做过，最近进展在哪里，你的结果是否只是重复，还是找到了更清楚的切片、机制或反例。

### 15.1 维护一个轻量 community watch

不要每天无目的刷 arXiv，也不要完全不看外部进展。建议每周固定 30 到 60 分钟，更新一个轻量笔记：

```md
# Community Watch

## This Week
- Paper / repo / benchmark:
- Relation to my project:
- Does it change my baseline, metric, failure mode, or hypothesis?
- Action item, if any:

## Strong Baselines to Track

## Competing Claims

## Open Gaps
```

判断一篇新论文是否需要立刻处理，只问一件事：它是否改变你的当前决策。如果没有，就记录后回到主线。

### 15.2 被抢发时怎么处理

看到相似工作时，不要马上换题。先做三件事：

1. 比较 setting：数据、模型、metric、假设、成本、约束是否相同；
2. 找 limitation：它在哪些 slice、case、成本或安全约束下没有解决；
3. 升级 baseline：如果它确实强，把它纳入你的 baseline，而不是假装不存在。

“别人先发了”可能说明这条线没有空间，也可能说明问题重要。真正要判断的是：你是否还能提出更清楚的 failure mode、更强的 evidence、更严谨的 evaluation，或更一般的机制解释。

### 15.3 Workshop、poster 和外部讨论的作用

Workshop、poster、reading group 和 seminar 不只是展示成果，也可以用来校准问题价值。早期项目可以带着很小的 artifact 出去讨论：一张 failure taxonomy、一组反例、一个 oracle test、一个 probing set。你要收集的不是掌声，而是三类反馈：

- 这个问题是否被认为重要；
- 哪个 baseline 或相关工作你漏了；
- 哪个替代解释别人最担心。

外部反馈要回写到 decision log 或 community watch。否则它只是聊天，不会改变项目。

---

## 16. AI Safety 方向的特殊注意

AI Safety 不是把普通 ML 流程换一组 benchmark。很多安全问题的难点在于：评测对象本身有争议，ground truth 可能不存在，行为指标容易被模型或 prompt 策略利用，解释性实验还需要因果证据。

### 16.1 Behavioral evaluation：同时看风险与可用性

安全评测不要只报 attack success 或 refusal rate。至少同时看：

- risk reduction：风险行为是否下降；
- utility：正常任务能力是否受损；
- over-refusal：是否把安全性伪装成全面拒答；
- robustness：改写、翻译、模板变化、上下文变化后是否稳定；
- judge reliability：人工标注、规则、LLM judge 是否一致。

如果一个 defense 只是在所有请求上更保守，它未必更安全，可能只是更不可用。

### 16.2 没有 ground truth 时，先定义证据层级

有些安全问题很难有单一正确答案。此时不要假装 metric 等于真理。先写清证据层级：

```text
Level 1: 个案观察
Level 2: 人工标注一致性
Level 3: 可复现行为切片
Level 4: 对抗扰动后仍稳定
Level 5: 有因果干预或机制解释支持
```

你的结论只能声称到证据能支持的层级。不要用 Level 1 的样本截图支持 Level 5 的机制结论。

### 16.3 Mechanistic interpretability：可视化不等于解释

如果做 mechanistic interpretability，仅展示 attention map、activation pattern 或某个 neuron 的相关性通常不够。更可信的路径是：

- 明确要解释的行为；
- 提出机制假设；
- 设计干预，例如 activation patching、ablation、feature steering、counterfactual input；
- 检查干预是否按预期改变行为；
- 排除更简单的统计相关解释。

解释性研究的目标不是画出漂亮图，而是给出能经受干预检验的机制证据。

### 16.4 安全研究的额外记录

安全项目还应在 Project OS 中记录：

- 是否涉及 dual-use 风险；
- 攻击样本、jailbreak prompt 或 exploit 是否需要限制传播；
- 模型访问、API 日志和人工标注是否包含敏感信息；
- 对外分享结果前是否需要和导师确认风险边界。

这不是形式主义。安全研究的对象有时本身就是风险源，研究记录也要有边界意识。

---

## 17. 十二周上手路线

这不是固定进度表，也不是承诺。真实项目经常回跳：第 8 周发现 metric 有问题，可能要回到第 5 周重新做 error analysis；第 10 周发现 baseline 不可信，可能要回到第 3 周重建参照系。

这条路线只提供节奏参考。你的目标是在 8 到 12 周内完成一次可复盘的最小研究闭环，而不是机械满足每一周的清单。

### 第 0 周：准备工作

目标：能进入项目环境，知道方向背景。

产出：

- repo 跑通最小命令；
- 数据下载和路径确认；
- 3 篇 anchor papers 列表；
- `boundary_card.md` 草稿。

### 第 1 周：明确当前问题

目标：把方向压成一个当前阶段能回答的问题。

产出：

- `problem_brief.md`；
- `literature_map.md` 初版；
- 当前 baseline、dataset、metric 列表；
- 本周唯一核心问题。

### 第 2 周：小数据跑通 baseline

目标：端到端跑通，先确认 pipeline。

产出：

- 小数据运行结果；
- environment log；
- command 和 config；
- 第一版 prediction file；
- 初步 sanity checks。

### 第 3 周：可信复现 baseline

目标：对齐论文主要结果，或解释差异来源。

产出：

- `reproduction_report.md`；
- 结果对比表；
- 差异来源分析；
- 多 seed 或方差估计。

### 第 4 周：读代码和追踪数据流

目标：从黑盒调用变成白盒理解。

产出：

- `code_trace_note.md`；
- 数据流图；
- equation-to-code map；
- 关键 shape、mask、loss、metric 说明；
- hidden assumptions 列表。

### 第 5 周：系统 error analysis

目标：从总体分数进入样本和错误类型。

产出：

- 50 到 100 个错误样本标注；
- failure taxonomy；
- 高置信度错误样本分析；
- 高置信度做对样本抽查；
- 初步 slice 结果。

### 第 6 周：补文献，定位 failure mode

目标：围绕最大 failure mode 补关键文献，而不是泛读。

产出：

- 更新后的 literature map；
- 2 到 3 个 candidate causes；
- 一个主 hypothesis；
- 一个 minimal test 方案；
- 一个 toy setup 或 proxy setup 方案。

### 第 7 周：最小实验

目标：用最便宜的实验验证主 hypothesis。

产出：

- `experiment_card.md`；
- 小规模实验结果；
- toy / proxy 实验结果；
- 三句话结果解释；
- Go / Pivot / Stop 初判。

### 第 8 周：对照和 ablation

目标：排除简单替代解释。

产出：

- 对照实验表；
- ablation；
- seed 方差；
- 关键 slice 变化。

### 第 9 周：压力测试和切片扩展

目标：确认方法或现象的边界。

产出：

- corner case 测试；
- 私有 probing set 初版；
- OOD 或扰动测试；
- 失败边界说明；
- 代表样本。

### 第 10 周：整理技术小结

目标：把问题、证据、假设、实验和判断整理清楚。

产出：

- `internal_technical_note.md`；
- 结果表和图；
- 关键样本；
- 已排除方向；
- 当前最大风险。

### 第 11 周：内部短报告

目标：用 10 分钟讲清楚研究闭环，不讲流水账。

产出：

- 5 到 8 页内部 slides；
- 只回答：问题是什么，证据是什么，判断是什么，下一步是什么。

### 第 12 周：阶段决策

目标：明确下一阶段。

产出：

- Go / Pivot / Stop note；
- 下一阶段唯一核心问题；
- 需要补的实验或资源；
- 当前方向的复盘。

---

## 18. 模板库

### 18.1 Problem Brief

```md
# Problem Brief

## Problem
我现在要回答的问题是：

## Why It Matters
这个问题为什么重要：

## Closest Work
- Paper A：
- Paper B：
- Paper C：

## Current Gap
现有工作没有解决什么，或哪里仍不清楚：

## Fixed Conditions
当前固定的数据、baseline、metric、模型、代码：

## Open Variable
当前只观察或改变什么变量：

## Current Hypothesis
我当前最想验证的假设：

## First Artifact
本周交付什么：
```

### 18.2 Code Trace Note

```md
# Code Trace Note

## Sample ID

## Raw Input / Label

## Preprocessing
输入如何被清洗、截断、拼接？

## Tokenization / Feature Construction
token ids、mask、decode 后文本、feature shape。

## Model Forward
关键模块输入输出 shape。

## Equation-to-Code Map
论文公式对应代码行。

## Loss
loss 输入、mask、reduction、分母。

## Metric
metric 输入字段、post-processing、normalization。

## Hidden Assumptions
论文未说明但代码存在的处理。

## Suspicious Points
需要进一步检查的风险点。
```

### 18.3 Error Analysis Report

见第 9.6 节，可直接复制使用。

### 18.4 Experiment Log

见第 12.5 节，可直接复制使用。

### 18.5 Weekly Report

```md
# Weekly Report

## 1. 本周唯一核心问题

## 2. 为什么它是当前最重要的问题

## 3. 本周完成的动作

## 4. 关键结果
总体结果、slice 结果、代表样本。

## 5. 结果意味着什么
它支持、削弱或反驳了什么假设？

## 6. 哪个结果改变了我的判断

## 7. 最大阻塞或风险

## 8. 下周唯一优先任务

## 9. 当前明确不做什么
```

### 18.6 Go / Pivot / Stop Note

```md
# Go / Pivot / Stop Note

## Current Question

## Evidence Collected

## Strongest Positive Evidence

## Strongest Negative Evidence

## Main Risk

## Decision
Go / Pivot / Stop

## Reason
为什么做这个判断？

## Next Four-Week Plan
如果 Go 或 Pivot，接下来四周回答什么问题？
如果 Stop，留下哪些可复用记录？
```

### 18.7 Parking Lot

```md
# Parking Lot

| Date | Idea | Why It Came Up | Related Evidence | When to Revisit |
|---|---|---|---|---|
| | | | | |
```

把新想法写下来，不要让它打断本周主问题。

### 18.8 Toy / Proxy Test Card

```md
# Toy / Proxy Test Card

## Target Failure Mode
这个 toy setup 对应哪个真实 failure mode？

## Simplified World
我保留了哪些核心结构？删掉了哪些复杂因素？

## Synthetic / Small Data
数据如何构造？样本数量、变量、标签规则是什么？

## Expected Behavior
如果机制有效，应该看到什么趋势？

## Failure Interpretation
如果 toy setup 中无效，说明什么？是否需要修改机制或停止？

## Link to Real Setting
toy 结果如何影响真实实验设计？
```

### 18.9 Probing Set Card

```md
# Probing Set Card

## Purpose
这个 probing set 检查哪种能力或 failure mode？

## Construction Rule
样本如何构造？哪些变量被控制？

## Size
当前样本数：

## Categories
- Category A:
- Category B:
- Category C:

## Expected Behavior
模型在每类样本上应该如何表现？

## Baseline Behavior
当前 baseline 在这些样本上如何失败？

## Usage Rule
它用于机制检查、debug 和解释；不用于反复调参到过拟合。
```

### 18.10 Project OS 文件模板

建议每个项目在 `docs/` 下初始化以下文件：

```text
docs/
  01_project_overview.md
  02_decision_log.md
  03_meeting_notes.md
  04_data_model_provenance_log.md
  05_task_list.md
  06_result_log.md
```

最小模板如下。

```md
# 01_project_overview.md

## One-Sentence Research Question

## Fixed Setting

## Current Phase

## This Week's Only Priority

## Current Artifact

## Key Links and Paths

## Last Updated
```

```md
# 02_decision_log.md

## [YYYY-MM-DD] Decision: 

### Context

### Options Considered

### Decision

### Reason

### Scope

### Follow-up
```

```md
# 03_meeting_notes.md

## YYYY-MM-DD

### Participants

### Context

### Decided

### Open Questions

### Action Items
| Owner | Action | Deliverable | Due Date |
|---|---|---|---|
```

```md
# 04_data_model_provenance_log.md

## Raw Dataset

## Split

## Preprocessing

## External Annotation or LLM-Generated Data

## Base Model / Checkpoint
```

```md
# 05_task_list.md

## Next
- [ ] 

## Waiting
- [ ] 

## Done
- [x] 
```

```md
# 06_result_log.md

| Date | Run ID | Question | Method / Setting | Dataset / Slice | Metric | Seed | Commit | Config | Prediction File | Takeaway |
|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |
```


---

## 19. 一页版 SOP

可以直接贴在项目 README 顶部。

```text
1. 每周只回答一个核心问题。
2. 每周交一个可复盘 artifact。
3. 项目文档放在 docs/，决定、数据、会议、任务和主结果都有固定位置。
4. 模板只用于外化判断，不能替代思考；每个记录都要回答 So what。
5. 跑模型前先看 raw data；raw data 只读，所有中间数据由脚本生成。
6. 进入新方向时先补教材 / lecture notes / survey，再读最新论文。
7. Error analysis 只在 train/dev/validation 上做，test set 留作最终盲测。
8. 全量实验前先完成 sanity checks。
9. 异常高分先按 bug、泄漏、预训练污染和捷径学习排查。
10. 做对样本也要抽查。
11. 复现至少做到 L3：看懂数据流、loss、metric 和样本输出。
12. 每次 evaluation 保存 prediction file。
13. 每个实验先写 hypothesis、control、metric、success signal、failure signal 和 stop condition。
14. 一周以上投入前先做 Problem Value Filter：Who cares，为什么现在值得做。
15. 大实验前先画预期结果表，确认它能改变判断。
16. 大工程前先做 toy / proxy / oracle test，确认趋势和上限。
17. 一次只改一个核心变量。
18. 核心 forward、loss、mask、metric 需要手推、assert 和 toy example；AI 不能替代这一步。
19. 小幅提升先按噪声处理，报告 effect size、方差和关键 slice。
20. 结果用“事实、解释、行动”三句话汇报。
21. 卡住 48 小时，带着证据求助。
22. 与导师判断不一致时，用防御性最小验证制造证据，不要沉默执行或空泛反驳。
23. 长实验前记录 code commit、data/model version、config、seed、command、metrics 和 prediction path。
24. 大实验先做费米估算：时间、显存、IO 是否合理。
25. 简单强 baseline 是必要对照，不要和稻草人打架。
26. 建立 20 到 50 条私有 probing set，检查真正关心的能力。
27. 负结果也要记录，只要它干净并改变了判断。
28. side ideas 进入 parking lot，不打断主线。
29. 每周做一次 Project GC：更新 overview、decision log、task list、result log，清理临时文件并 commit。
30. 每周维护轻量 community watch，确认外部进展是否改变当前决策。
31. 四周一次 Go / Pivot / Stop。
```

---

## 20. 推荐继续阅读

这些材料不是入门前置条件。遇到具体问题时再读对应部分。

### 读论文与建立判断

- S. Keshav, **How to Read a Paper**：三遍读论文法，适合建立高效阅读流程。  
  https://ccr.sigcomm.org/online/files/p83-keshavA.pdf

- John Schulman, **An Opinionated Guide to ML Research**：关于 goal-driven research、研究品味和时间组织的建议。  
  https://joschu.net/blog/opinionated-guide-ml-research.html

### 实验与工程习惯

- Andrej Karpathy, **A Recipe for Training Neural Networks**：关于先看数据、sanity checks、overfit one batch、debug neural nets 的实用流程。  
  https://karpathy.github.io/2019/04/25/recipe/

- Bill Freeman, **How to do research**：关于从课程问题进入研究问题、slow down to speed up、一次只改一个变量、不要只汇报“it doesn't work”的建议。  
  https://people.csail.mit.edu/billf/publications/How_To_Do_Research.pdf

- MIT VisionBook, **How to Do Research**：Freeman 研究建议的网页版本，包含 toy model、epsilon-neighborhood exploration 和 progress 的定义。  
  https://visionbook.mit.edu/how_to_do_research.html

### 研究节奏与长期判断

- Jason Eisner, **How to Find Research Problems**：关于从复现、controlled comparison、系统缺陷和数据现象中寻找研究问题。  
  https://www.cs.jhu.edu/~jason/advice/how-to-find-research-problems.html

- Richard Hamming, **You and Your Research**：关于重要问题、长期投入和研究品味的经典演讲。  
  https://www.cs.virginia.edu/~robins/YouAndYourResearch.html

- Jacob Steinhardt, **Research as a Stochastic Decision Process**：把研究看作在不确定性下持续选择高信息率行动，适合理解 minimal test、de-risking 和 Go / Pivot / Stop。  
  https://cs.stanford.edu/~jsteinhardt/ResearchasaStochasticDecisionProcess.html

- Colin Raffel, **How to be an academic machine learning researcher**：关于 ML 研究实践、研究目标和避免盲目追逐榜单的建议。  
  https://colinraffel.com/talks/dlrl2022how.pdf

### 问题价值与项目过滤

- DARPA, **The Heilmeier Catechism**：用一组问题检查研究目标、现有方法、创新点、风险、成本和成功标准。  
  https://www.darpa.mil/about/heilmeier-catechism

### 统计严格性与可复现性

- Henderson et al., **Deep Reinforcement Learning that Matters**：关于随机性、seed、统计显著性和实验报告标准的经典提醒。  
  https://arxiv.org/abs/1709.06560

- Bouthillier et al., **Accounting for Variance in Machine Learning Benchmarks**：关于 benchmark 方差、trial variance 和算法比较的统计问题。  
  https://proceedings.mlsys.org/paper_files/paper/2021/file/0184b0cd3cfb185989f858a1d9f5c1eb-Paper.pdf

### AI Safety 与解释性研究

- Bereska and Gavves, **Mechanistic Interpretability for AI Safety: A Review**：关于机制解释、因果干预、可扩展性和双重用途风险的综述。  
  https://arxiv.org/abs/2404.14082

- **Tips for Empirical Alignment Research**：关于 empirical alignment research 中快速反馈、迭代和评测的实用建议。  
  https://www.alignmentforum.org/posts/dZFpEdKyb9Bf4xYn7/tips-for-empirical-alignment-research

---

## 21. 最核心的原则

研究新手最重要的训练，不是更快地跑更多实验，而是更稳定地把事情往前推进。

你应该反复练习这条链：

```text
看数据 → 看代码 → 看样本级输出 → 找 failure mode → 提假设
→ 做 toy / proxy 最小验证 → 审查正负样本 → 更新判断 → 留下记录
```

当你能稳定完成这条链，就已经从“完成任务”进入了“推进研究”。
