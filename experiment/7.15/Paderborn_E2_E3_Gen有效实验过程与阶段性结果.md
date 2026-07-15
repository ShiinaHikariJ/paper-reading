# Paderborn E2-Gen 与 E3-Gen 有效实验过程及阶段性结果

## 1. 实验目的

本实验围绕以下核心问题展开：

> 在 1500 rpm 目标工况仅有少量真实样本时，使用 900 rpm 的大量源域数据进行预训练，能否改善提示词条件生成模型对 1500 rpm 振动信号的生成效果，并提升下游故障诊断性能？

因此设置两组核心实验：

- **E2-Gen：目标域直接生成**
- **E3-Gen：源域预训练后适配到目标域**

两组实验只在“是否使用 900 rpm 源域数据预训练”这一点上不同，其余数据划分、目标 few-shot、文本条件、生成数量、分类器和测试集均保持一致。

---

# 2. 数据集与任务设置

## 2.1 数据集

使用 Paderborn 轴承故障数据集。

处理后数据目录：

```text
/root/autodl-tmp/test1/paderborn/processed_strict_v3
```

信号设置：

```text
采样率：16000 Hz
窗口长度：4096
单窗口时长：4096 / 16000 ≈ 0.256 s
输入通道：vibration_1
```

故障类别：

| 标签 | 类别 |
|---:|---|
| 0 | Normal |
| 1 | Outer race fault |
| 2 | Inner race fault |

---

## 2.2 strict_v3 数据划分

### 900 rpm 源域训练集

```text
source_train
shape = (4500, 4096)
```

每类数量：

```text
Normal：1500
Outer：1500
Inner：1500
```

### 1500 rpm 目标域训练池

```text
target_train_pool
shape = (2700, 4096)
```

每类数量：

```text
Normal：900
Outer：900
Inner：900
```

### 1500 rpm 验证集

```text
target_val
shape = (900, 4096)
```

每类 300 个。

### 1500 rpm 测试集

```text
target_test
shape = (900, 4096)
```

每类 300 个。

### 当前主实验的目标域 few-shot

```text
shot = 20个/类
seed = 42
总真实目标训练样本 = 60
```

对应文件：

```text
processed_strict_v3/fewshot/shot_20/seed_42.npz
```

---

# 3. 已有下游基线

## 3.1 E0：仅使用真实目标域 few-shot

20-shot/类，5 个种子结果：

```text
Accuracy = 0.933333 ± 0.021830
Macro-F1 = 0.932582 ± 0.023257
```

## 3.2 E1：传统数据增强

传统增强包括：

```text
加噪
幅值缩放
循环平移
时间遮挡
组合增强
```

20-shot/类，5 个种子结果：

```text
Accuracy = 0.944667 ± 0.014216
Macro-F1 = 0.944420 ± 0.014574
```

E0 和 E1 用于判断生成增强是否真正优于不增强和传统增强。

---

# 4. 文本条件构建

## 4.1 使用的模型服务

使用阿里云百炼：

```text
文本生成模型：qwen-plus
文本向量模型：text-embedding-v4
Embedding维度：256
```

通过百炼 OpenAI 兼容 REST API 调用，不依赖额外 SDK。

---

## 4.2 每条振动信号提取的统计特征

对每条信号提取：

```text
RMS
Kurtosis
Crest factor
High-frequency energy ratio
Spectral centroid
Envelope kurtosis
```

这些特征用于构造样本级技术描述。

---

## 4.3 特征等级划分

使用当前目标域 few-shot 的 60 个真实信号统一计算：

```text
low
medium
high
```

阈值。

900 rpm 源域信号和 1500 rpm 目标域信号使用同一套目标域语义阈值。

未使用：

```text
target_train_pool全量数据
target_val
target_test
```

因此没有引入验证集或测试集信息泄露。

---

## 4.4 Qwen 描述示例

结构化属性输入类似：

```text
Fault class: outer race fault.
Speed: 1500 rpm.
Vibration amplitude: high.
Impulsiveness: medium.
Crest factor: low.
High-frequency energy: low.
Spectral centroid level: low.
Envelope impact intensity: medium.
```

Qwen 将其改写为简洁技术描述。

随后追加样本自身的数值特征，例如：

```text
Measured signal features:
RMS=0.30896;
kurtosis=3.6966;
crest factor=4.4375;
high-frequency energy ratio=0.71999;
spectral centroid=3551.66 Hz;
envelope kurtosis=4.3260.
```

最终描述通过 `text-embedding-v4` 转为 256 维向量。

---

## 4.5 条件缓存结果

当前 20-shot、seed 42：

```text
Qwen离散描述组合：155
唯一Embedding文本：4545
```

最终缓存目录：

```text
/root/autodl-tmp/test1/paderborn/
condition_cache_qwen/shot_20/seed_42/
```

主要文件：

```text
source_conditions.npz
target_conditions.npz
source_descriptions.jsonl
target_descriptions.jsonl
qwen_description_cache.json
qwen_embedding_cache.json
manifest.json
```

缓存对应：

```text
source embedding shape = (4500, 256)
target embedding shape = (60, 256)
```

---

# 5. 条件扩散模型

扩散模型的条件由以下部分组成：

```text
类别Embedding
+
转速Embedding
+
Qwen文本Embedding
+
扩散时间步Embedding
```

加入显式类别和转速条件，是为了确保最基本的类别与工况信息不会仅依赖文本编码器。

E2 和 E3 使用完全相同的条件结构。

---

# 6. E2-Gen：目标域直接生成

## 6.1 实验含义

E2-Gen 仅使用：

```text
1500 rpm目标域
20个真实样本/类
总计60个
```

训练目标域提示词条件生成器。

不使用 900 rpm 源域信号训练生成模型。

---

## 6.2 Autoencoder训练

设置：

```text
训练数据：1500 rpm目标域60个few-shot
epochs：250
batch size：32
```

训练过程：

```text
epoch 1   loss = 2.659268
epoch 25  loss = 0.098863
epoch 50  loss = 0.026602
epoch 100 loss = 0.014856
epoch 150 loss = 0.011769
epoch 200 loss = 0.010968
epoch 250 loss = 0.010951
```

Autoencoder 正常收敛。

---

## 6.3 目标域提示词条件扩散训练

设置：

```text
训练数据：1500 rpm目标域60个few-shot
epochs：1200
learning rate：2e-4
```

训练过程节选：

```text
epoch 1    loss = 1.113952
epoch 100  loss = 0.509505
epoch 200  loss = 0.396893
epoch 400  loss = 0.353517
epoch 600  loss = 0.295186
epoch 800  loss = 0.287983
epoch 1000 loss = 0.301512
epoch 1200 loss = 0.295591
```

扩散训练正常完成，无 NaN 或程序错误。

---

## 6.4 生成与筛选

每类先生成约 300 个候选信号：

```text
Normal候选：300
Outer候选：300
Inner候选：300
```

然后根据对应类别的目标域真实 few-shot 进行质量筛选，最终保留：

```text
100个生成信号/类
总生成信号：300
```

筛选仅使用目标域真实 few-shot，不使用验证集和测试集。

---

## 6.5 下游评估

分类训练采用：

```text
真实目标few-shot
+
E2生成信号
```

并通过加权采样，使每个训练 epoch 中：

```text
真实数据约50%
生成数据约50%
```

测试集固定为 1500 rpm 的 900 个真实测试窗口。

结果：

```text
E2-Gen Accuracy = 0.883333
E2-Gen Macro-F1 = 0.882375
```

---

# 7. E3-Gen：900 rpm源域辅助生成

## 7.1 实验含义

E3-Gen 首先使用：

```text
900 rpm源域4500个真实信号
```

进行提示词条件生成器预训练。

然后使用与 E2 完全相同的：

```text
1500 rpm
20个真实样本/类
seed 42
```

进行目标域适配。

---

## 7.2 源域Autoencoder训练

设置：

```text
训练数据：900 rpm源域4500个信号
epochs：20
batch size：64
```

训练过程：

```text
epoch 1  loss ≈ 0.347550
epoch 20 loss ≈ 0.005626
```

源域 Autoencoder 正常收敛。

---

## 7.3 源域提示词条件扩散预训练

设置：

```text
训练数据：900 rpm源域4500个信号
epochs：20
learning rate：2e-4
```

源域条件包括：

```text
类别
900 rpm转速
Qwen描述Embedding
```

源域扩散预训练完成后保存预训练权重。

---

## 7.4 目标域适配

使用：

```text
1500 rpm目标域20个/类
总计60个
```

对源域扩散模型进行低学习率短微调。

设置：

```text
epochs：300
learning rate：2e-5
```

相较早期高学习率、1200轮微调，该设置用于减少源域知识被少量目标域样本覆盖的问题。

---

## 7.5 生成与下游评估

与 E2 完全相同：

```text
每类生成约300个候选
每类筛选保留100个
真实/生成约1:1采样
相同分类器
相同验证集
相同测试集
```

结果：

```text
E3-Gen Accuracy = 0.891111
E3-Gen Macro-F1 = 0.888326
```

---

# 8. 当前核心对比结果

20-shot/类，seed 42：

| 实验 | Accuracy | Macro-F1 |
|---|---:|---:|
| E2-Gen | 0.883333 | 0.882375 |
| E3-Gen | 0.891111 | 0.888326 |
| E3 − E2 | **+0.007778** | **+0.005951** |

即：

```text
Accuracy提升约0.78个百分点
Macro-F1提升约0.60个百分点
```

---

# 9. 当前阶段可以得出的结论

在相同的：

```text
目标域20-shot
seed 42
Qwen描述生成方法
文本Embedding模型
扩散模型结构
生成数量
筛选方式
下游分类器
验证集和测试集
```

条件下：

> 使用 900 rpm 大量源域数据预训练后再适配到 1500 rpm 的 E3-Gen，性能高于只使用 1500 rpm 少样本直接训练的 E2-Gen。

该结果初步说明：

> 900 rpm 源域预训练对 1500 rpm 少样本提示词条件生成具有一定帮助。

---

# 10. 当前结果的限制

## 10.1 目前只有一个种子

当前：

```text
seed = 42
完整种子数 = 1
```

因此汇总中的标准差为 0，仅仅是因为只有一个结果，不代表模型没有波动。

当前 `+0.78%` 的提升可能来自：

```text
真实的源域辅助增益
或
训练随机性
```

必须补充 5 个种子后才能判断稳定性。

---

## 10.2 E3仍低于E0和E1

当前结果关系：

```text
E3-Gen = 0.8911
E2-Gen = 0.8833
E0     ≈ 0.9333
E1     ≈ 0.9447
```

因此当前只能说明：

```text
E3-Gen优于E2-Gen
```

还不能说明：

```text
生成增强优于不增强
生成增强优于传统增强
```

当前更准确的结论是：

> 源域预训练改善了直接 Gen，但当前生成信号质量仍不足以超过真实 few-shot 基线和传统增强基线。

---

## 10.3 当前实验不是完整的统计结论

在完成 5 个种子之前，不能使用：

```text
显著提升
稳定优于
证明有效
```

等强结论。

应使用：

```text
初步提升
出现正向趋势
阶段性结果支持该思路
```

---

# 11. 下一步正式实验

## 11.1 固定20-shot主设置

继续运行：

```text
seed 42
seed 43
seed 44
seed 45
seed 46
```

分别完成：

```text
E2-Gen
E3-Gen
```

最终报告：

```text
Accuracy mean ± std
Macro-F1 mean ± std
每类Recall mean ± std
E3-E2平均差值
5个种子中E3优于E2的次数
```

---

## 11.2 20-shot稳定后再更换样本量

推荐补充：

```text
10-shot/类
30-shot/类
```

原因：

- 10-shot：验证极少样本条件下源域辅助是否更明显；
- 20-shot：当前主设置；
- 30-shot：验证目标数据增多后源域增益是否减弱。

可选饱和点：

```text
50-shot/类
```

不建议将 100-shot 作为主实验，因为 E0 已接近满分，生成增强几乎没有提升空间。

---

# 12. 结果文件位置

## Qwen条件缓存

```text
/root/autodl-tmp/test1/paderborn/
condition_cache_qwen/shot_20/seed_42/
```

## E2-Gen结果

```text
/root/autodl-tmp/test1/paderborn/
results_strict_v3/gen_compare/
e2-gen/shot_20/seed_42/
```

关键文件：

```text
generation_quality.json
generation_plots/
classifier/metrics.json
classifier/confusion_matrix.png
classifier/classification_report.txt
```

## E3-Gen结果

```text
/root/autodl-tmp/test1/paderborn/
results_strict_v3/gen_compare/
e3-gen/shot_20/seed_42/
```

## E2/E3对比汇总

```text
/root/autodl-tmp/test1/paderborn/
results_strict_v3/gen_compare/
e2_e3_comparison.csv
```

---

# 13. 阶段性总结

本轮有效实验完成了以下闭环：

```text
Paderborn strict_v3数据
→ 目标域20-shot
→ Qwen生成样本级技术描述
→ text-embedding-v4生成256维文本向量
→ E2目标域直接提示词条件生成
→ E3源域预训练后目标域适配
→ 每类生成并筛选100个信号
→ 统一下游分类评估
→ E3与E2直接比较
```

当前阶段性结果：

```text
E2-Gen Accuracy = 0.8833
E3-Gen Accuracy = 0.8911
E3相对E2提升 = +0.0078
```

一句话总结：

> 在 20-shot/类、seed 42 下，900 rpm 源域预训练后的 E3-Gen 相比目标域直接训练的 E2-Gen 出现了约 0.78 个百分点的准确率提升，初步支持源域辅助少样本提示词条件生成的研究思路，但仍需通过 5 个种子验证其稳定性，且当前性能尚未超过真实 few-shot 和传统增强基线。
