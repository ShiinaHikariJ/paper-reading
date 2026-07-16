# 2026-07-16 实验进度总结

## 一、今日实验目标

今天主要围绕以下问题推进：

> 在 Paderborn 数据集上，900 rpm 源域的大量数据是否能够帮助 1500 rpm 目标域少样本生成，并进一步提高故障诊断性能？

重点完成了三部分工作：

1. 汇总并分析原有 Teacher-BearGen 公平实验结果；
2. 完成 E3-VAE-only 消融实验；
3. 重新设计并开始验证物理轴承完全隔离的 bearing-disjoint 协议。

---

## 二、原有 strict_v3 公平实验结果

### 1. 实验任务

原 strict_v3 协议中：

- 源域：900 rpm；
- 目标域：1500 rpm；
- 任务类别：
  - Normal；
  - Outer race fault；
  - Inner race fault；
- 目标域训练：20-shot/类；
- 验证集和测试集来自与训练集相同的一组物理轴承，但测量文件不同。

该协议主要验证：

> 相同轴承在不同转速条件下，源域预训练是否能够帮助目标域生成。

### 2. 三个随机种子的 Teacher-BearGen 结果

| Seed | E2-Teacher Accuracy | E3-Full Accuracy | E3−E2 |
|---:|---:|---:|---:|
| 42 | 0.912222 | 0.905556 | -0.006667 |
| 43 | 0.908889 | 0.908889 | 0.000000 |
| 44 | 0.925556 | 0.935556 | +0.010000 |
| 均值 | 0.915556 | 0.916667 | +0.001111 |

Macro-F1：

```text
E2-Teacher = 0.914088
E3-Full    = 0.914618
平均提升    = +0.000530
```

### 3. 当前结论

三个种子中：

```text
E3胜出：1次
持平：1次
落后：1次
```

因此，当前 strict_v3 结果只能说明：

> E3-Full 与 E2-Teacher 基本持平，900 rpm 源域预训练尚未表现出稳定的正迁移作用。

虽然 E3 平均略高，但仅提升约 0.11 个百分点，远小于跨种子波动，不能作为稳定有效的证据。

---

## 三、E3-VAE-only 消融结果

### 1. 消融设计

为判断源域预训练的影响来自 VAE 还是扩散模型，新增：

```text
E3-VAE-only
```

其设置为：

```text
900 rpm：只预训练 VAE
1500 rpm：使用相同20-shot适配VAE
目标扩散模型：随机初始化
```

对比关系：

| 方法 | 900 rpm VAE预训练 | 900 rpm Diffusion预训练 |
|---|---:|---:|
| E2-Teacher | 否 | 否 |
| E3-VAE-only | 是 | 否 |
| E3-Full | 是 | 是 |

### 2. seed42 结果

| 方法 | Accuracy | Macro-F1 |
|---|---:|---:|
| E2-Teacher | 0.912222 | 0.909642 |
| E3-VAE-only | 0.890000 | 0.884247 |
| E3-Full | 0.905556 | 0.901847 |

差值：

```text
VAE-only − E2 = -0.022222
Full − E2     = -0.006667
```

### 3. 结果解释

seed42 中：

```text
E3-VAE-only < E3-Full < E2
```

说明：

1. 单独迁移 900 rpm VAE 会明显降低性能；
2. 加入 900 rpm 扩散预训练后，性能从 0.890000 恢复到 0.905556；
3. 但完整 E3 仍略低于目标域直接训练的 E2。

因此，目前更值得怀疑的是：

> 900 rpm 预训练 VAE 的潜空间可能带入了明显的跨转速偏差。

同时，源域扩散预训练并未进一步恶化结果，反而在 seed42 下补回了部分损失。

当前 VAE-only 只完成 seed42，尚不能作为最终结论。

---

## 四、对原 strict_v3 划分的重新认识

原 strict_v3 中：

```text
训练、验证和测试来自相同物理轴承
只是使用不同测量文件
```

20-shot E0 已达到：

```text
Accuracy ≈ 0.9333
```

E1 传统增强约为：

```text
Accuracy ≈ 0.9447
```

说明该任务基线较高，生成增强的剩余提升空间有限。

原协议仍然有价值，但更适合称为：

```text
same-bearing cross-speed adaptation
```

即：

> 同一批轴承在新转速条件下的少样本适配。

它不能直接代表对未知轴承的泛化能力。

---

## 五、建立 bearing-disjoint 新协议

### 1. 新协议目标

今天新增了更严格的：

```text
bearing-disjoint cross-speed generalization
```

该协议要求：

- 训练轴承、验证轴承、测试轴承完全隔离；
- 测试轴承在 900 rpm 和 1500 rpm 下都不参与训练；
- E3 只能使用训练轴承的 900 rpm 源域数据。

当前 Fold 1：

| 类别 | 训练轴承 | 验证轴承 | 测试轴承 |
|---|---|---|---|
| Normal | K001、K002、K003 | K004 | K005 |
| Outer | KA04、KA15、KA16 | KA22 | KA30 |
| Inner | KI04、KI14、KI16 | KI18 | KI21 |

### 2. 新协议识别任务

测试阶段识别的是：

```text
K005、KA30、KI21 的1500 rpm振动信号
```

输出三类：

```text
0 = Normal
1 = Outer race fault
2 = Inner race fault
```

这些测试轴承从未进入：

```text
900 rpm源域预训练
1500 rpm目标few-shot训练
```

因此该任务属于：

> 跨转速、跨物理轴承、跨故障实例的未知轴承故障分类。

---

## 六、bearing-disjoint Fold 1 的 E0 结果

### 1. E0 设置

```text
20-shot/类
seed = 42
只使用训练轴承的1500 rpm真实目标样本
```

### 2. 结果

```text
Test Accuracy = 0.265556
Test Macro-F1 = 0.194798
Best epoch = 29
```

三分类随机猜测的理论准确率约为：

```text
0.333333
```

当前 E0 低于随机准确率，且 Macro-F1 很低，说明模型可能严重偏向部分类别。

### 3. 对该结果的理解

这个结果不能简单理解为“实验错误”，更可能说明：

> 训练轴承和测试轴承之间存在非常强的分布差异。

同时，这个协议比老师最初提出的“源转速帮助目标转速少样本生成”更难，因为它还要求模型泛化到完全未知的物理轴承和故障实例。

不过 E0 不需要先达到较高水平才能继续 Gen。

E0 的作用是作为基线：

```text
E0：不用生成
E2：目标域直接生成
E3：源域辅助生成
```

如果 E3 能在该困难协议下明显超过 E0 和 E2，反而会成为更有说服力的结果。

---

## 七、今日新增代码与脚本

今天新增并整理了完整 bearing-disjoint 代码包：

```text
paderborn_bearing_disjoint_protocol.zip
```

主要文件：

```text
prepare_paderborn_bearing_disjoint.py
check_bearing_disjoint_split.py
train_paderborn_bearing_disjoint_e0.py
summarize_bearing_disjoint.py

01_prepare_and_run_e0_fold1.sh
02_run_teacher_gen_fold1.sh
03_optional_run_e0_all_5folds.sh
```

新目录：

```text
processed_bearing_disjoint/
condition_cache_teacher_beargen_bearing_disjoint/
results_bearing_disjoint/
logs_bearing_disjoint/
```

原 strict_v3 数据和结果不会被覆盖。

---

## 八、当前正在进行的实验

已启动或准备启动：

```text
Fold 1
20-shot/类
seed42
E2-Teacher
E3-Teacher
```

运行脚本：

```bash
bash 02_run_teacher_gen_fold1.sh
```

后台运行：

```bash
nohup env PYTHONUNBUFFERED=1 \
  bash 02_run_teacher_gen_fold1.sh \
  > bearing_disjoint_fold1_gen.log 2>&1 &
```

查看进度：

```bash
tail -f bearing_disjoint_fold1_gen.log
```

脚本流程：

```text
生成Qwen描述和Embedding
→ 训练E2-Teacher
→ E2生成与分类评估
→ 训练E3-Teacher
→ E3生成与分类评估
→ 公平协议检查
→ E0/E2/E3汇总
```

当前 Fold 1 Gen 结果尚未完成。

---

## 九、下一步结果判断标准

### 情况一：E2 > E0

说明：

> 目标域直接 Gen 对未知轴承泛化有帮助。

### 情况二：E3 > E2

说明：

> 900 rpm 源域数据对目标生成有额外帮助。

### 情况三：E3 > E2 且 E3 > E0

这是当前最理想的结果：

> 源域辅助生成不仅优于目标域直接生成，也优于只使用真实 few-shot 数据。

### 情况四：E3 > E2，但 E3 < E0

说明：

> 源域帮助了生成模型，但生成增强整体仍未超过不使用生成的方法。

### 情况五：E2、E3均不超过E0

说明：

> 当前生成方法没有改善未知轴承泛化，甚至可能强化训练轴承偏差。

---

## 十、下一步实验计划

当前优先顺序：

```text
1. 等待 Fold 1 的 E2/E3 结果
2. 比较 E0、E2、E3
3. 查看混淆矩阵与各类 Recall
4. 判断是否值得继续其他 Fold
5. 若 Fold 1 有明显正向趋势，再扩展 Fold 2～5
6. 若 E3 无提升，进一步考虑：
   - Diffusion-only 消融
   - 阶次域处理
   - 转速归一化
   - 潜空间域对齐
```

暂时不优先：

```text
strict_v3 的更多shot实验
strict_v3 的更多随机种子
bearing-disjoint的VAE-only多种子
```

先等待当前 Fold 1 的 E2/E3 结果。

---

## 十一、今日阶段性结论

今天的核心进展不是简单得到一个更高准确率，而是完成了实验问题的重新界定。

当前已经形成两套协议：

### Protocol A：strict_v3

```text
same-bearing cross-speed adaptation
```

特点：

```text
基线高
任务较容易
E2/E3基本持平
源域增益不稳定
```

### Protocol B：bearing-disjoint

```text
unseen-bearing cross-speed generalization
```

特点：

```text
训练、验证和测试物理轴承完全隔离
E0仅为26.56%
任务明显更难
更适合检验Gen是否具有跨轴承泛化能力
```

目前最客观的总结是：

> 在原 same-bearing 协议中，Teacher-BearGen 的 E3 与 E2 基本持平，源域预训练没有表现出稳定增益；VAE-only 消融提示跨转速 VAE 预训练可能引入负迁移。为增加任务难度并检验真实泛化能力，今天建立了物理轴承完全隔离的新协议。该协议下 Fold 1 的 E0 Accuracy 为 26.56%，说明训练轴承与测试轴承之间存在显著分布差异。目前正在继续运行 E2-Teacher 和 E3-Teacher，以判断生成增强和源域辅助能否改善未知轴承故障识别。
