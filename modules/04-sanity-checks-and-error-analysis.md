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

