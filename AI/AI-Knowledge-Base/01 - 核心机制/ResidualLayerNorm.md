# 残差连接与层归一化（Residual Connection & LayerNorm）

#核心机制 #残差连接 #归一化

## 一句话
Transformer 中每层 Attention 和 FFN 之后都跟着的两个"结构胶水"——一个让深层信号不消失（残差连接），一个让数值不失控（层归一化）。

---

## 残差连接（Add）

**问题**：信号经过 96 层 Transformer Block，每层都在被"改写"。信息会逐层稀释乃至消失（梯度消失问题）。

**做法**：
```
输出 = 这一层处理的结果 + 这一层的原始输入
```

每一层不是在改写信息，而是在**补充信息**。

**降维映射**：传话游戏中，第一个人说的话写在纸条上。每个人看完纸条、加上自己的话，再把纸条传下去。最后一个人手里有原始信息 + 所有人补充的内容。相比传统"一个接一个口述"的方式，信息完全不会丢失。

## 层归一化（LayerNorm）

**问题**：经过层层运算，数字可能变得极大（几万）或极小（0.0001），导致后续层的梯度爆炸或消失。

**做法**：把当前层所有神经元的输出值做一次规整：
1. 算出这层输出的均值和标准差
2. 减去均值，除以标准差
3. 输出稳定在均值 0、方差 1 的范围内

**降维映射**：班级考试成绩出来后，老师把所有分数做一次标准化——有人 98 分、有人 37 分、有人 -5 分，全部拉回 -1 到 1 之间。不是改了学生的水平，是把数据规整到同一尺度上。

## 在 Transformer 中的位置

```
Attention → Add（残差连接） → LayerNorm → FFN → Add（残差连接） → LayerNorm
  ↑        ↑会加上原始输入               ↑      ↑会加上 Attention 输出
```

每个 Transformer Block 里用两次，顺序固定。

---

## 🧪 摸底自测

#### Q1（入门）
> 残差连接（Residual Connection）解决的核心问题是什么？

<details>
<summary>点击查看解答思路</summary>

梯度消失/爆炸。深层网络中，梯度经过多层链式相乘后会指数级衰减（消失）或增长（爆炸）。残差连接让梯度可以直接"跳过"一些层往回传（旁路），保证靠近输入层的参数也能有效更新。
</details>

#### Q2（进阶）
> Layer Normalization 和 Batch Normalization 的关键区别是什么？为什么 Transformer 用 LayerNorm 而不是 BatchNorm？

<details>
<summary>点击查看解答思路</summary>

BatchNorm 对一个 batch 中同一位置的所有样本做归一化（依赖 batch 内统计量），LayerNorm 对一个样本的所有维度做归一化（不依赖 batch）。Transformer 用 LayerNorm 因为序列长度不固定、batch size 可能很小——LayerNorm 更适合 NLP 任务。
</details>

## 参见
- [[Transformer]] — 这两个组件在完整架构中的位置
- [[神经网络]] — 梯度消失/爆炸的根本原因

## 参考文献
- He, K. et al. (2015). *Deep Residual Learning for Image Recognition*. CVPR.
- Ba, J. L. et al. (2016). *Layer Normalization*. arXiv:1607.06450.
- Ioffe, S. & Szegedy, C. (2015). *Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift*. ICML.
