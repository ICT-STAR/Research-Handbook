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

