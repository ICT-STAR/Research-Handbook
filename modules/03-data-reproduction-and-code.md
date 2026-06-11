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

