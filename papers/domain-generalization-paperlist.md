# 域泛化论文清单

这份清单服务于第二条研究线：

> 训练工况、设备或数据集与实际应用不一致时，如何让故障诊断模型在未见目标域上仍然可靠？

这里的“域泛化”默认指 **训练阶段不使用目标域数据**。如果方法需要无标签目标域数据，更准确地说应归入“无监督域适应”；如果需要少量目标域标签，则归入“小样本迁移/微调”。三类问题可以相互借鉴，但实验设置要分清。

## 已读


## 一、当前已纳入仓库的相关论文

| 编号 | 论文 | 年份 | 方向 | 泛化设置 | 笔记 | 一句话定位 |
| --- | --- | ---: | --- | --- | --- | --- |
| P004 | [Self-Supervised Contrastive Pre-Training for Time Series via Time-Frequency Consistency](https://proceedings.neurips.cc/paper_files/paper/2022/hash/194b8dac525581c346e30a2cebe9a369-Abstract-Conference.html) | 2022 | 时频一致性、自监督预训练、迁移表征 | 跨域迁移/表征预训练 | [笔记](../notes/P004-TF-C.md) | 可作为故障诊断域泛化的表征学习基础，重点看时域和频域增强是否能学到稳定特征。 |
| P012 | [One Fits All: Power General Time Series Analysis by Pretrained LM](https://arxiv.org/abs/2302.11939) | 2023 | 预训练语言模型、通用时间序列分析 | 跨任务、少样本、零样本 |  | 说明预训练模型可以迁移到多类时间序列任务，是“大模型用于时序泛化”的通用背景。 |
| P014 | [LLM-based Framework for Bearing Fault Diagnosis](https://arxiv.org/abs/2411.02718) | 2025 | LLM轴承故障诊断、特征文本化、原始信号Patch | 跨工况、跨数据集、少样本目标域微调 | [笔记](../notes/P014-LLM-based-bearing-fault-diagnosis.md) | 适合作为“大模型直接做故障诊断泛化”的代表，但要注意其跨数据集实验仍包含目标域微调。 |
| P015 | [DiagLLM: Multimodal Reasoning with Large Language Model for Explainable Bearing Fault Diagnosis](https://scholar.google.com/scholar?q=DiagLLM%3A+Multimodal+Reasoning+with+Large+Language+Model+for+Explainable+Bearing+Fault+Diagnosis) | 2025 | 多模态LLM、包络谱、故障频率知识 | 跨数据集泛化、少数据诊断 | [笔记](../notes/P015-DiagLLM-multimodal-reasoning-for-explainable-bearing-fault-diagnosis.md) | 重点价值在于把故障机理知识和可解释诊断结合，可作为知识增强泛化路线。 |
| P016 | [BearLLM: A Prior Knowledge-Enhanced Bearing Health Management Framework with Unified Vibration Signal Representation](https://arxiv.org/abs/2408.11281) | 2024 | 统一振动表示、先验知识增强、大小模型协同 | 多数据集泛化、零样本数据集泛化 | [笔记](<../notes/P016-BearLLM-prior-knowledge-enhanced-bearing-health-management (1).md>) | 重点看“健康参照 + 残差表示”如何削弱数据集差异，是当前域泛化线最值得深挖的已有论文。 |

## 二、建议新增阅读的域泛化基础论文

| 编号 | 论文 | 年份 | 方向 | 为什么要读 | 状态 |
| --- | --- | ---: | --- | --- | --- |
| DG001 | [Domain Generalization via Invariant Feature Representation](https://arxiv.org/abs/1301.2115) | 2013 | 域不变表示、DICA、核方法 | 早期明确提出域泛化问题，可用于建立“多源域训练、未见域测试”的基本定义。 | TODO |
| DG002 | [Invariant Risk Minimization](https://arxiv.org/abs/1907.02893) | 2019 | 不变风险最小化、因果特征、OOD泛化 | 域泛化里最经典的不变特征思想之一，适合对照机械故障中的“稳定故障机理特征”。 | TODO |
| DG003 | [In Search of Lost Domain Generalization](https://arxiv.org/abs/2007.01434) | 2020 | DomainBed、评测协议、模型选择 | 重要性在于提醒：很多DG方法在公平评测下未必胜过ERM，做实验时必须设计强基线和模型选择策略。 | TODO |
| DG004 | [Domain Generalization with MixStyle](https://arxiv.org/abs/2104.02008) | 2021 | 特征统计混合、隐式域增强 | 可借鉴到时频图、包络谱或中间特征层，用来模拟未见工况的风格变化。 | TODO |
| DG005 | [SWAD: Domain Generalization by Seeking Flat Minima](https://arxiv.org/abs/2102.08604) | 2021 | 平坦极小值、权重平均、鲁棒训练 | 提供“不改模型结构、改训练策略”的强基线，适合和机械故障诊断模型结合。 | TODO |
| DG006 | [Fishr: Invariant Gradient Variances for Out-of-Distribution Generalization](https://arxiv.org/abs/2109.02934) | 2021 | 梯度方差对齐、OOD泛化 | 从优化动态角度做不变性约束，可作为IRM之外的域不变学习补充。 | TODO |

## 三、建议新增阅读的故障诊断相关论文

| 编号 | 论文 | 年份 | 方向 | 泛化设置 | 为什么要读 | 状态 |
| --- | --- | ---: | --- | --- | --- | --- |
| DG-FD001 | [Bearing fault diagnosis under varying working condition based on domain adaptation](https://arxiv.org/abs/1707.09890) | 2017 | 轴承故障诊断、无监督域适应、子空间对齐 | 使用无标签目标域 | 不是严格DG，但适合作为机械故障跨工况迁移的早期基线，帮助区分DA和DG。 | TODO |
| DG-FD002 | [Bearing fault diagnosis based on domain adaptation using transferable features under different working conditions](https://arxiv.org/abs/1806.01512) | 2018 | 可迁移特征、MMD、跨工况诊断 | 使用无标签目标域/伪标签 | 可作为MMD类分布对齐方法的机械故障代表，用来比较“目标域可见”和“目标域不可见”的差异。 | TODO |
| DG-FD003 | [Dual adversarial and contrastive network for single-source domain generalization in fault diagnosis](https://arxiv.org/abs/2407.13978) | 2024 | 单源域泛化、对抗学习、对比学习 | 单源训练、未见模式测试 | 更接近严格域泛化，适合关注“只有一个工况/模式数据时如何泛化”。 | TODO |
| DG-FD004 | [YOTOnet: Zero-Shot Cross-Domain Fault Diagnosis via Domain-Conditioned Mixture of Experts](https://arxiv.org/abs/2605.04528) | 2026 | 零样本跨域故障诊断、MoE、物理感知不变特征 | 多源训练、跨数据集零样本测试 | 新近工作，强调“train once”跨数据集部署，可作为大模型/基础模型式故障诊断泛化的候选前沿。 | TODO |
| DG-FD005 | [Progressive Knowledge-Guided Large Language Model Framework for Bearing Fault Diagnosis](https://arxiv.org/abs/2606.16684) | 2026 | 物理知识引导LLM、多尺度振动表征 | 多数据集、多工况验证 | 可补充P014-P016之后的LLM诊断线，重点看物理可追溯特征是否提升跨域稳健性。 | TODO |

## 四、按研究路线组织

### 4.1 传统域泛化基础线

```text
DG001 DICA 域不变表示
  -> DG002 IRM 因果/不变风险
  -> DG003 DomainBed 公平评测和强ERM基线
  -> DG005 SWAD / DG006 Fishr 训练与优化层面的稳健泛化
```

这条线回答：

> 域泛化到底应该如何定义、如何评测，以及“不变特征”是否真的比普通ERM更有效？

### 4.2 机械故障跨工况与跨数据集线

```text
DG-FD001 / DG-FD002 早期跨工况域适应
  -> DG-FD003 单源域泛化故障诊断
  -> P014 / P015 / P016 大模型、知识和统一表示参与泛化
  -> DG-FD004 / DG-FD005 零样本跨域与物理知识引导前沿
```

这条线回答：

> 面对未见转速、负载、设备、传感器或数据集，模型到底依靠什么特征保持诊断能力？

### 4.3 与数据增强线的交叉

```text
数据增强线：生成更多、更真实、更符合故障机理的训练样本
域泛化线：学习跨工况、跨设备、跨数据集稳定存在的故障表征
交叉问题：生成数据是否只是提升源域准确率，还是能真正提升未见域诊断？
```

后续可重点比较三类方案：

| 方案 | 作用 | 风险 |
| --- | --- | --- |
| 只做数据增强 | 扩充源域样本、缓解小样本 | 可能生成“源域内更像”的样本，未必改善未见域 |
| 只做域泛化训练 | 强调不变特征和鲁棒优化 | 小样本故障类别下可能仍然数据不足 |
| 数据增强 + 域泛化 | 用生成/变换扩大域多样性，再学习稳定故障特征 | 需要证明增强没有破坏故障频率、冲击周期和物理语义 |

## 五、优先阅读顺序

1. **P016 BearLLM**：先从已有笔记出发，梳理“健康参照 + 残差表示”的泛化逻辑。
2. **DG003 DomainBed**：建立实验警惕性，明确为什么DG需要强ERM基线和合理模型选择。
3. **DG002 IRM**：理解不变特征和因果泛化的理论直觉。
4. **DG-FD003 DACN**：进入机械故障的严格域泛化场景。
5. **P014 / P015**：比较LLM诊断、多模态知识和跨数据集泛化的实际证据。
6. **DG-FD004 / DG-FD005**：作为前沿补充，判断“基础模型式故障诊断”是否真的比传统DG更进一步。

## 六、读每篇域泛化论文时要记录的问题

- 源域和目标域分别是什么？目标域训练阶段是否完全不可见？
- 是单源DG、多源DG、无监督域适应，还是少量目标域微调？
- 域差异来自转速、负载、设备、传感器、噪声，还是数据集采集协议？
- 方法学核心是不变特征、数据增强、元学习、因果学习、对抗训练、物理知识，还是大模型先验？
- 是否与ERM、CNN/WDCNN、ResNet、Transformer、MMD/DA方法做了公平对比？
- 是否只提升平均准确率，还是在最难目标域、低样本类别和跨数据集场景也稳定？
- 是否解释了模型学到的特征与故障频率、冲击周期、包络谱结构之间的关系？
