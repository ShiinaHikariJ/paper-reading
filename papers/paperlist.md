# 论文阅读清单

## 论文总表

| 编号 | 论文 | 年份 | 方向 | 来源 | 笔记 | 小总结 |
| --- | --- | ---: | --- | --- | --- | --- |
| P001 | [Time Series Data Augmentation for Deep Learning: A Survey](https://www.ijcai.org/proceedings/2021/631) | 2021 | 时间序列数据增强、综述 | IJCAI 2021 | [笔记](../notes/P001-example.md) | 系统梳理时间序列数据增强方法，强调增强必须保留时序依赖、变量关系和任务语义。 |
| P002 | [Data Augmentation using LLMs: Data Perspectives, Learning Paradigms and Challenges](https://aclanthology.org/2024.findings-acl.97/) | 2024 | 大模型数据增强、LLM生成数据、综述 | Findings of ACL 2024 | [笔记](../notes/P002-data-augmentation-using-llms.md) | 综述LLM数据增强的数据视角、学习范式和挑战，为理解大模型生成或改写训练数据提供框架。 |
| P003 | [Time-series Generative Adversarial Networks](https://proceedings.neurips.cc/paper/2019/hash/c9efe5f26cd17ba6216bbe2a7d26d490-Abstract.html) | 2019 | 时间序列生成、GAN、生成式数据增强 | NeurIPS 2019 | [笔记](../notes/P003-TimesGAN.md) | 提出TimeGAN，在潜在空间结合GAN和监督式时间动态损失，提升时序生成的动态一致性。 |
| P004 | [Self-Supervised Contrastive Pre-Training for Time Series via Time-Frequency Consistency](https://proceedings.neurips.cc/paper_files/paper/2022/hash/194b8dac525581c346e30a2cebe9a369-Abstract-Conference.html) | 2022 | 自监督学习、时频对比、跨域迁移 | NeurIPS 2022 | [笔记](../notes/P004-TF-C.md) | 提出TF-C，通过时域、频域增强和跨域一致性对比学习，提高时间序列迁移表征能力。 |
| P005 | [GT-GAN: General Purpose Time Series Synthesis with Generative Adversarial Networks](https://proceedings.neurips.cc/paper_files/paper/2022/hash/f03ce573aa8bce26f77b76f1cb9ee979-Abstract-Conference.html) | 2022 | 规则与不规则时序生成、连续时间建模、GAN | NeurIPS 2022 | [笔记](../notes/P005-GTGAN.md) | 提出GT-GAN，用连续时间建模和可逆流统一规则与不规则时间序列生成。 |
| P006 | [Diffusion-TS: Interpretable Diffusion for General Time Series Generation](https://openreview.net/forum?id=4h1apFjO99) | 2024 | 扩散模型、通用时序生成、时频结构保持 | ICLR 2024 | [笔记](../notes/P006-Diffusion-ST.md) | 提出Diffusion-TS，将扩散模型、Transformer、趋势周期分解和频域损失结合用于通用时序生成。 |
| P007 | [Diff-MTS: Temporal-Augmented Conditional Diffusion-Based AIGC for Industrial Time Series Toward the Large Model Era](https://ieeexplore.ieee.org/document/10697287) | 2024 | 工业多变量时序生成、条件扩散、时序分解 | IEEE Transactions on Cybernetics | [笔记](../notes/P007-P009-reading-notes.md) | 提出Diff-MTS，用条件扩散、Ada-MMD和TDR-UNet生成工业多变量时间序列。 |
| P008 | [FIDE: Frequency-Inflated Conditional Diffusion Model for Extreme-Aware Time Series Generation](https://proceedings.neurips.cc/paper_files/paper/2024/hash/cfce727868dcaf5295c0125f9d6fbc0b-Abstract-Conference.html) | 2024 | 极值感知时序生成、频域增强、条件扩散 | NeurIPS 2024 | [笔记](../notes/P007-P009-reading-notes.md) | 提出FIDE，通过高频膨胀、极值条件和GEV损失改善扩散模型对极端值的生成能力。 |
| P009 | [Constrained Posterior Sampling: Time Series Generation with Hard Constraints](https://openreview.net/forum?id=pKMpmbuKnd) | 2025 | 约束时序生成、扩散后验采样、硬约束 | NeurIPS 2025 | [笔记](../notes/P007-P009-reading-notes.md) | 提出CPS，在扩散采样阶段投影到约束集合，实现无需重训的硬约束时间序列生成。 |
| P010 | [Large language model to assist data augmentation in soft contrastive learning for few-shot machinery fault diagnosis](https://doi.org/10.1016/j.aei.2025.104078) | 2026 | 大模型辅助数据增强、软对比学习、少样本故障诊断 | Advanced Engineering Informatics | [笔记](../notes/P010-LLM-CoTAugSCLFD.md) | 用LLM推荐机械信号增强策略，并结合软标签对比学习缓解少样本故障诊断问题。 |
| P011 | [Virtual sample diffusion generation method guided by large language model-generated knowledge for enhancing information completeness and zero-shot fault diagnosis in building thermal systems](https://doi.org/10.1631/jzus.A2400560) | 2025 | 大模型知识引导、虚拟故障样本生成、零样本故障诊断 | Journal of Zhejiang University-SCIENCE A | [笔记](../notes/P011-LLM-KG-MTD.md) | 用LLM生成故障知识引导KG-MTD虚拟样本生成，支持零真实故障样本训练。 |
| P012 | [One Fits All: Power General Time Series Analysis by Pretrained LM](https://arxiv.org/abs/2302.11939) | 2023 | 预训练语言模型、通用时间序列分析、跨模态迁移、少样本与零样本 | NeurIPS 2023 |  | 将预训练语言模型用于通用时间序列分析，验证跨模态迁移和少样本、零样本能力。 |
| P013 | [FaultDiffusion: Few-Shot Fault Time Series Generation with Diffusion Model](https://arxiv.org/abs/2511.15174) | 2026 | 少样本故障时间序列生成、正常数据预训练、扩散模型、故障域适配与多样性约束 | AAAI 2026 / arXiv | [笔记](../notes/P013-FaultDiffusion-reading-notes.md) | 用扩散模型生成少样本故障时间序列，强调正常数据预训练、故障域适配和多样性约束。 |
| P014 | [LLM-based Framework for Bearing Fault Diagnosis](https://arxiv.org/abs/2411.02718) | 2025 | 大模型轴承故障诊断、振动信号文本化、原始信号Patch、参数高效微调、跨工况与跨数据集泛化 | Mechanical Systems and Signal Processing | [笔记](../notes/P014-LLM-based-bearing-fault-diagnosis.md) | 将轴承振动特征或原始Patch映射到LLM空间，探索大模型故障诊断与跨工况泛化。 |
| P015 | [DiagLLM: Multimodal Reasoning with Large Language Model for Explainable Bearing Fault Diagnosis](https://scholar.google.com/scholar?q=DiagLLM%3A+Multimodal+Reasoning+with+Large+Language+Model+for+Explainable+Bearing+Fault+Diagnosis) | 2025 | 多模态大模型、包络谱图像、故障频率知识、少数据诊断、跨数据集泛化与可解释性 | Science China Information Sciences | [笔记](../notes/P015-DiagLLM-multimodal-reasoning-for-explainable-bearing-fault-diagnosis.md) | 构建多模态DiagLLM，融合包络谱图像和故障频率知识，实现可解释轴承故障诊断。 |
| P016 | [BearLLM: A Prior Knowledge-Enhanced Bearing Health Management Framework with Unified Vibration Signal Representation](https://arxiv.org/abs/2408.11281) | 2024 | 多模态大模型、统一振动信号表示、大小模型协同、多数据集泛化、多任务轴承健康管理 | arXiv | [笔记](<../notes/P016-BearLLM-prior-knowledge-enhanced-bearing-health-management (1).md>) | 提出BearLLM，用统一振动表示和先验知识增强实现大小模型协同的轴承健康管理。 |
| P017 | [BearGen: LLM-guided Signal Generation Framework for Bearing Fault Diagnosis](https://doi.org/10.1016/j.aei.2026.104400) | 2026 | 大模型引导信号生成、文本条件扩散、轴承故障数据增强、类别不平衡 | Advanced Engineering Informatics | [笔记](../notes/P017-BearGen-reading-notes.md) | 利用GPT生成故障标签和工况对应的信号描述，并将知识蒸馏至本地Qwen模型，再以描述为条件引导潜空间扩散模型生成轴承振动信号；在多个轴承数据集和类别不平衡场景中优于GAN及标签条件扩散方法，但仍存在描述依赖已知标签、物理约束不足和跨工况生成泛化尚未充分验证等问题。 |


# 论文阅读清单

## 分类导航

| 类别               | 论文编号           | 主要作用                                |
| ---------------- | -------------- | ----------------------------------- |
| 综述与通用基础          | P001、P002、P012 | 建立数据增强、LLM数据增强和预训练语言模型处理时间序列的整体认识   |
| 数据增强与时间序列生成方法    | P003—P009、P013 | 提供传统增强、GAN、扩散模型、频域增强、约束生成和少样本故障生成基线 |
| 大模型辅助数据增强与虚拟样本生成 | P010、P011、P017 | 分析大模型如何规划增强策略、提供故障知识、生成条件并辅助构造虚拟样本 |
| 大模型与机械故障诊断进展     | P014—P016      | 分析大模型直接诊断、多模态知识融合和大小模型协同的发展现状       |

---

## 一、综述与通用基础

这部分回答三个基础问题：

1. 时间序列数据增强有哪些方法；
2. 大模型如何参与数据增强；
3. 预训练语言模型为什么能够处理时间序列。

| 编号   | 论文                                                                                                                                   |   年份 | 方向                             | 来源                   | 笔记                                                  | 小总结 |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------ | ---: | ------------------------------ | -------------------- | --------------------------------------------------- | --- |
| P001 | [Time Series Data Augmentation for Deep Learning: A Survey](https://www.ijcai.org/proceedings/2021/631)                              | 2021 | 时间序列数据增强、综述                    | IJCAI 2021           | [笔记](../notes/P001-example.md)                      | 系统梳理时间序列数据增强方法，强调增强必须保留时序依赖、变量关系和任务语义。 |
| P002 | [Data Augmentation using LLMs: Data Perspectives, Learning Paradigms and Challenges](https://aclanthology.org/2024.findings-acl.97/) | 2024 | 大模型数据增强、LLM生成数据、综述             | Findings of ACL 2024 | [笔记](../notes/P002-data-augmentation-using-llms.md) | 综述LLM数据增强的数据视角、学习范式和挑战，为理解大模型生成或改写训练数据提供框架。 |
| P012 | [One Fits All: Power General Time Series Analysis by Pretrained LM](https://arxiv.org/abs/2302.11939)                                | 2023 | 预训练语言模型、通用时间序列分析、跨模态迁移、少样本与零样本 | NeurIPS 2023         |                                                     | 将预训练语言模型用于通用时间序列分析，验证跨模态迁移和少样本、零样本能力。 |

---

## 二、数据增强与时间序列生成方法

### 2.1 非生成式表征学习与下游诊断基线

这类方法不直接生成新信号，而是通过自监督学习、对比学习和时频一致性学习更好的时间序列表征，可以作为生成数据下游诊断效果的对比基线。

| 编号   | 论文                                                                                                                                                                                                              |   年份 | 方向              | 来源           | 笔记                          | 小总结 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | --------------- | ------------ | --------------------------- | --- |
| P004 | [Self-Supervised Contrastive Pre-Training for Time Series via Time-Frequency Consistency](https://proceedings.neurips.cc/paper_files/paper/2022/hash/194b8dac525581c346e30a2cebe9a369-Abstract-Conference.html) | 2022 | 自监督学习、时频对比、跨域迁移 | NeurIPS 2022 | [笔记](../notes/P004-TF-C.md) | 提出TF-C，通过时域、频域增强和跨域一致性对比学习，提高时间序列迁移表征能力。 |

### 2.2 GAN类时间序列生成

这类方法学习真实时间序列的数据分布，是早期生成式数据增强的主要路线，可作为扩散模型的对比基线。

| 编号   | 论文                                                                                                                                                                                                         |   年份 | 方向                    | 来源           | 笔记                              | 小总结 |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | --------------------- | ------------ | ------------------------------- | --- |
| P003 | [Time-series Generative Adversarial Networks](https://proceedings.neurips.cc/paper/2019/hash/c9efe5f26cd17ba6216bbe2a7d26d490-Abstract.html)                                                               | 2019 | 时间序列生成、GAN、生成式数据增强    | NeurIPS 2019 | [笔记](../notes/P003-TimesGAN.md) | 提出TimeGAN，在潜在空间结合GAN和监督式时间动态损失，提升时序生成的动态一致性。 |
| P005 | [GT-GAN: General Purpose Time Series Synthesis with Generative Adversarial Networks](https://proceedings.neurips.cc/paper_files/paper/2022/hash/f03ce573aa8bce26f77b76f1cb9ee979-Abstract-Conference.html) | 2022 | 规则与不规则时序生成、连续时间建模、GAN | NeurIPS 2022 | [笔记](../notes/P005-GTGAN.md)    | 提出GT-GAN，用连续时间建模和可逆流统一规则与不规则时间序列生成。 |

### 2.3 扩散模型与工业时间序列生成

这类方法利用扩散过程学习复杂时间序列分布，是当前故障数据生成的主要方法基础。

| 编号   | 论文                                                                                                                                                                   |   年份 | 方向                                   | 来源                               | 笔记                                        | 小总结 |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | ------------------------------------ | -------------------------------- | ----------------------------------------- | --- |
| P006 | [Diffusion-TS: Interpretable Diffusion for General Time Series Generation](https://openreview.net/forum?id=4h1apFjO99)                                               | 2024 | 扩散模型、通用时序生成、时频结构保持                   | ICLR 2024                        | [笔记](../notes/P006-Diffusion-ST.md)       | 提出Diffusion-TS，将扩散模型、Transformer、趋势周期分解和频域损失结合用于通用时序生成。 |
| P007 | [Diff-MTS: Temporal-Augmented Conditional Diffusion-Based AIGC for Industrial Time Series Toward the Large Model Era](https://ieeexplore.ieee.org/document/10697287) | 2024 | 工业多变量时序生成、条件扩散、时序分解                  | IEEE Transactions on Cybernetics | [笔记](../notes/P007-P009-reading-notes.md) | 提出Diff-MTS，用条件扩散、Ada-MMD和TDR-UNet生成工业多变量时间序列。 |
| P013 | [FaultDiffusion: Few-Shot Fault Time Series Generation with Diffusion Model](https://arxiv.org/abs/2511.15174)                                                       | 2026 | 少样本故障时间序列生成、正常数据预训练、扩散模型、故障域适配与多样性约束 | AAAI 2026 / arXiv                | [笔记](../notes/P013-FaultDiffusion-reading-notes.md) | 用扩散模型生成少样本故障时间序列，强调正常数据预训练、故障域适配和多样性约束。 |

### 2.4 频域增强、极值保持与约束生成

这类方法关注“生成结果是否保留关键特征和满足规则”，可以为机械故障信号中的特征频率、周期冲击和包络谱约束提供方法启发。

| 编号   | 论文                                                                                                                                                                                                                    |   年份 | 方向                 | 来源           | 笔记                                        | 小总结 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | ------------------ | ------------ | ----------------------------------------- | --- |
| P008 | [FIDE: Frequency-Inflated Conditional Diffusion Model for Extreme-Aware Time Series Generation](https://proceedings.neurips.cc/paper_files/paper/2024/hash/cfce727868dcaf5295c0125f9d6fbc0b-Abstract-Conference.html) | 2024 | 极值感知时序生成、频域增强、条件扩散 | NeurIPS 2024 | [笔记](../notes/P007-P009-reading-notes.md) | 提出FIDE，通过高频膨胀、极值条件和GEV损失改善扩散模型对极端值的生成能力。 |
| P009 | [Constrained Posterior Sampling: Time Series Generation with Hard Constraints](https://openreview.net/forum?id=pKMpmbuKnd)                                                                                            | 2025 | 约束时序生成、扩散后验采样、硬约束  | NeurIPS 2025 | [笔记](../notes/P007-P009-reading-notes.md) | 提出CPS，在扩散采样阶段投影到约束集合，实现无需重训的硬约束时间序列生成。 |

---

## 三、大模型辅助数据增强与虚拟样本生成

这部分关注的不是让大模型直接完成故障分类，而是让大模型提供增强策略、故障知识或生成条件，再由传统工具、统计模型或小模型执行。

| 编号   | 论文                                                                                                                                                                                                                                      |   年份 | 方向                               | 来源                                       | 笔记                                     | 小总结 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | -------------------------------- | ---------------------------------------- | -------------------------------------- | --- |
| P010 | [Large language model to assist data augmentation in soft contrastive learning for few-shot machinery fault diagnosis](https://doi.org/10.1016/j.aei.2025.104078)                                                                       | 2026 | 大模型辅助数据增强、增强策略规划、软对比学习、少样本机械故障诊断 | Advanced Engineering Informatics         | [笔记](../notes/P010-LLM-CoTAugSCLFD.md) | 用LLM推荐机械信号增强策略，并结合软标签对比学习缓解少样本故障诊断问题。 |
| P011 | [Virtual sample diffusion generation method guided by large language model-generated knowledge for enhancing information completeness and zero-shot fault diagnosis in building thermal systems](https://doi.org/10.1631/jzus.A2400560) | 2025 | 大模型知识引导、虚拟故障样本生成、零真实故障样本诊断       | Journal of Zhejiang University-SCIENCE A | [笔记](../notes/P011-LLM-KG-MTD.md)      | 用LLM生成故障知识引导KG-MTD虚拟样本生成，支持零真实故障样本训练。 |
| P017 | [BearGen: LLM-guided Signal Generation Framework for Bearing Fault Diagnosis](https://doi.org/10.1016/j.aei.2026.104400) | 2026 | 大模型引导信号生成、文本条件扩散、轴承故障数据增强、类别不平衡 | Advanced Engineering Informatics | [笔记](../notes/P017-BearGen-reading-notes.md) | 利用LLM生成故障与工况描述并引导潜空间扩散模型生成轴承振动信号，探索大模型知识条件下的故障数据增强。 |

---

## 四、大模型与机械故障诊断进展

### 4.1 大模型直接处理振动特征或原始信号

这类方法将时频统计特征文本化，或将原始振动信号切分成Patch后映射到语言模型空间，研究大模型在跨工况、小样本和跨数据集诊断中的泛化能力。

| 编号   | 论文                                                                                  |   年份 | 方向                                            | 来源                                       | 笔记 | 小总结 |
| ---- | ----------------------------------------------------------------------------------- | ---: | --------------------------------------------- | ---------------------------------------- | -- | --- |
| P014 | [LLM-based Framework for Bearing Fault Diagnosis](https://arxiv.org/abs/2411.02718) | 2025 | 大模型轴承故障诊断、振动信号文本化、原始信号Patch、参数高效微调、跨工况与跨数据集泛化 | Mechanical Systems and Signal Processing | [笔记](../notes/P014-LLM-based-bearing-fault-diagnosis.md) | 将轴承振动特征或原始Patch映射到LLM空间，探索大模型故障诊断与跨工况泛化。 |

### 4.2 多模态大模型与故障机理知识融合

这类方法将包络谱、时频图等机械信号表示与故障频率知识共同输入多模态大模型，重点解决诊断泛化和物理可解释性问题。

| 编号   | 论文                                                                                                                                                                                                                                        |   年份 | 方向                                             | 来源                                 | 笔记 | 小总结 |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | ---------------------------------------------- | ---------------------------------- | -- | --- |
| P015 | [DiagLLM: Multimodal Reasoning with Large Language Model for Explainable Bearing Fault Diagnosis](https://scholar.google.com/scholar?q=DiagLLM%3A+Multimodal+Reasoning+with+Large+Language+Model+for+Explainable+Bearing+Fault+Diagnosis) | 2025 | 多模态大模型、包络谱图像、BPFI/BPFO故障频率知识、少数据诊断、跨数据集泛化与可解释性 | Science China Information Sciences | [笔记](../notes/P015-DiagLLM-multimodal-reasoning-for-explainable-bearing-fault-diagnosis.md) | 构建多模态DiagLLM，融合包络谱图像和故障频率知识，实现可解释轴承故障诊断。 |

### 4.3 大小模型协同与多任务健康管理

这类方法由小模型或传统信号处理模块负责振动信号分析，大模型负责理解用户任务、组织诊断结果并生成维修建议，实现大小模型分工协作。

| 编号   | 论文                                                                                                                                                       |   年份 | 方向                                      | 来源    | 笔记 | 小总结 |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: | --------------------------------------- | ----- | -- | --- |
| P016 | [BearLLM: A Prior Knowledge-Enhanced Bearing Health Management Framework with Unified Vibration Signal Representation](https://arxiv.org/abs/2408.11281) | 2024 | 多模态大模型、统一振动信号表示、大小模型协同、多数据集泛化、多任务轴承健康管理 | arXiv | [笔记](<../notes/P016-BearLLM-prior-knowledge-enhanced-bearing-health-management (1).md>) | 提出BearLLM，用统一振动表示和先验知识增强实现大小模型协同的轴承健康管理。 |

---

## 五、当前研究主线

### 主线一：数据增强与故障数据生成

```text
P001 数据增强综述
  ↓
P003、P005 GAN时间序列生成
  ↓
P006、P007 扩散时间序列生成
  ↓
P008、P009 频域保持与约束生成
  ↓
P013 少样本故障时间序列生成
  ↓
P017 大模型引导轴承故障信号生成
```

这条主线回答：

> 在故障数据不足时，如何生成真实、多样、可控并符合机械特征的数据？

### 主线二：大模型与机械故障诊断

```text
P012 预训练语言模型处理时间序列
  ↓
P014 大模型直接处理振动特征或原始信号
  ↓
P015 多模态大模型融合包络谱与故障知识
  ↓
P016 小模型处理信号、大模型统一健康管理任务
```

这条主线回答：

> 大模型在机械故障诊断中能够承担哪些角色？

### 主线三：大模型辅助数据增强

```text
P002 LLM数据增强综述
  ↓
P010 LLM规划机械信号增强策略
  ↓
P011 LLM知识引导虚拟故障样本生成
  ↓
P017 LLM文本条件引导轴承故障信号生成
  ↓
结合P013研究大模型知识引导的少样本故障信号生成
```

这条主线是目前最接近候选研究方向的一条：

> 大模型负责故障知识和增强策略，扩散模型负责数据生成，传统信号处理工具和小模型负责物理验证与诊断评价。
