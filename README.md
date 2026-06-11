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

## 如何阅读

- 想看完整原文：读 [`handbook.md`](./handbook.md)。
- 想按主题查看：改 [`modules/`](./modules/) 下对应模块。
- 想要一个帮助你科研的skill：看看[research-assistant-skill](https://github.com/RickT34/research-assistant-skill)

## 模块目录

| 模块 | 一句话 |
|---|---|
| [`00-introduction.md`](./modules/00-introduction.md) | 说明手册目标，并建立“推进问题”而不是“完成任务”的基本视角。 |
| [`01-weekly-workflow-and-boundaries.md`](./modules/01-weekly-workflow-and-boundaries.md) | 用每周唯一问题、可复盘产物和边界卡片限制发散。 |
| [`02-reading-literature.md`](./modules/02-reading-literature.md) | 把读论文变成定位 baseline、claim、evidence、boundary 和 action item 的过程。 |
| [`03-data-reproduction-and-code.md`](./modules/03-data-reproduction-and-code.md) | 先看数据，再复现 baseline，最后沿真实样本追踪代码数据流。 |
| [`04-sanity-checks-and-error-analysis.md`](./modules/04-sanity-checks-and-error-analysis.md) | 在全量实验前做便宜体检，并从总体分数进入样本级错误分析。 |
| [`05-research-questions-and-experiments.md`](./modules/05-research-questions-and-experiments.md) | 从系统性 failure mode 形成研究问题，并用最小实验验证假设。 |
| [`06-project-os.md`](./modules/06-project-os.md) | 建立项目事实源，让问题、决定、数据、结果和任务可追溯。 |
| [`07-communication-and-collaboration.md`](./modules/07-communication-and-collaboration.md) | 用证据、选项和明确请求缩短导师、合作者与自己的反馈回路。 |
| [`08-common-pitfalls.md`](./modules/08-common-pitfalls.md) | 汇总新手常见卡点，并给出对应处理动作。 |
| [`09-community-and-ai-safety.md`](./modules/09-community-and-ai-safety.md) | 把本地发现放回社区坐标，并补充 AI Safety 评测与记录边界。 |
| [`10-twelve-week-route.md`](./modules/10-twelve-week-route.md) | 给出 8 到 12 周完成最小研究闭环的节奏参考。 |
| [`11-templates-and-sop.md`](./modules/11-templates-and-sop.md) | 提供可复制的卡片、日志、报告模板和一页版 SOP。 |
| [`12-further-reading-and-principles.md`](./modules/12-further-reading-and-principles.md) | 收集继续阅读材料，并收束到最核心的研究推进链条。 |

## 模块导读

### [`01-weekly-workflow-and-boundaries.md`](./modules/01-weekly-workflow-and-boundaries.md)

新手最容易同时做太多事：读很多论文、跑很多脚本、改很多模块、记很多想法，但最后没有一个清楚结论。本模块要求每周只抓一个核心问题，并在拿到方向后先写清固定项、变量和暂不处理的边界。

### [`02-reading-literature.md`](./modules/02-reading-literature.md)

读论文的目的不是“看完”，而是回答这篇论文在项目里的用途：benchmark、baseline、方法来源、诊断视角，还是一个需要被检验的 claim。本模块提供读前定位、三遍读法、Paper Card 和 Literature Map。

### [`03-data-reproduction-and-code.md`](./modules/03-data-reproduction-and-code.md)

很多实验问题不是模型问题，而是数据问题；不要一下载代码就直接跑 `python main.py`。本模块把数据初检、可信复现和沿样本读代码放在同一条证据链上。

### [`04-sanity-checks-and-error-analysis.md`](./modules/04-sanity-checks-and-error-analysis.md)

全量实验很贵，正式跑大实验前要先用便宜检查确认 pipeline 基本可信。总体分数只是入口，本模块要求保存样本级输出，并把错误变成可验证的 failure mode。

### [`05-research-questions-and-experiments.md`](./modules/05-research-questions-and-experiments.md)

更稳的 idea 生成路径是：先确定系统性 failure mode，再问它为什么发生，最后设计最小修复或验证。本模块强调问题价值过滤、toy/proxy/oracle test、对照、方差和结果解释。

### [`06-project-os.md`](./modules/06-project-os.md)

很多项目中后期难推进，不是因为缺想法，而是因为信息开始熵增：决定散落、版本混乱、结果无法追溯。本模块给出轻量 Project OS，让研究过程可恢复、可协作、可回放。

### [`07-communication-and-collaboration.md`](./modules/07-communication-and-collaboration.md)

科研不是单人闭门完成的；及时沟通不是能力弱，而是缩短反馈回路。本模块说明如何主导 1-on-1、结构化求助、处理初步无效 idea，以及在和导师判断不一致时制造可验证证据。

### [`08-common-pitfalls.md`](./modules/08-common-pitfalls.md)

本模块把“读了很多论文但没有想法”“复现后不知道下一步”“一直调参”“看不懂代码”等常见问题拆成可执行处理方式，适合卡住时反查。

### [`09-community-and-ai-safety.md`](./modules/09-community-and-ai-safety.md)

研究不能只在本地 repo 里推进；几个月后需要知道自己的发现和社区的关系。本模块同时提醒 AI Safety 方向不要只报单一风险指标，要记录证据层级、可用性、鲁棒性和潜在 dual-use 边界。

### [`10-twelve-week-route.md`](./modules/10-twelve-week-route.md)

这不是固定进度表，也不是承诺。它提供从准备、明确问题、小数据跑通、可信复现、读代码、error analysis 到阶段决策的 12 周节奏参考。

### [`11-templates-and-sop.md`](./modules/11-templates-and-sop.md)

本模块集中放置 Problem Brief、Code Trace Note、Weekly Report、Go/Pivot/Stop Note、Parking Lot、Toy/Proxy Test Card、Probing Set Card 和 Project OS 文件模板。

### [`12-further-reading-and-principles.md`](./modules/12-further-reading-and-principles.md)

这些材料不是入门前置条件，遇到具体问题时再读对应部分。最后的核心原则是持续练习“看数据 -> 看代码 -> 看样本级输出 -> 找 failure mode -> 提假设 -> 做最小验证 -> 更新判断 -> 留下记录”。

## 贡献方式

建议每次贡献只改一个主题模块，避免一次 PR 同时修改多个不相关部分。详细约定见 [`CONTRIBUTING.md`](./CONTRIBUTING.md)。
