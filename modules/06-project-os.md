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

