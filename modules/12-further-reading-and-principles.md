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
