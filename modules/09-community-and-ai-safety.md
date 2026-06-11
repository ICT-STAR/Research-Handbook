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

