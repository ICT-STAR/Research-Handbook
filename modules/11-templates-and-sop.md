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

