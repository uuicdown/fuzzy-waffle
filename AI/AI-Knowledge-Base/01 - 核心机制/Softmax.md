# Softmax（Softmax 函数）

#核心机制 #softmax

## 一句话
把一组任意大小的数字（logits）转换成一堆加起来等于 1 的概率——位于神经网络输出层的最后一关。

## 数学公式

```
Softmax(xᵢ) = eˣⁱ / Σⱼ eˣʲ
```

每个值取指数（e 的 x 次方）→ 除以总和 → 变成 0~1 之间的概率。

## 举个例子

假设输出层给出三个分数：`[2.0, 1.0, 0.1]`

1. 取指数：`[e², e¹, e⁰·¹]` = `[7.39, 2.72, 1.11]`
2. 除以总和（7.39+2.72+1.11=11.22）：
   - `[7.39/11.22, 2.72/11.22, 1.11/11.22]` = `[0.66, 0.24, 0.10]`
3. 结果是 **66% 概率选 A，24% 选 B，10% 选 C**

## 为什么取指数？

**放大了"大"和"小"的差距**。2.0 和 1.0 的原始差是 1 倍，取指数后差变成 7.39/2.72 ≈ 2.7 倍。让模型更"自信"地选择高分的选项。

## 与 Sigmoid 的区别

| 函数 | 输出范围 | 用途 |
|---|---|---|
| **Softmax** | 多个值，总和=1 | 多分类（选一个类别） |
| **Sigmoid** | 单个值在 0~1 之间 | 二分类（是/否） |

## 在 Transformer 中的位置

Transformer 的最后一步：最后一层的输出 → 线性变换（LM Head）→ **Softmax** → 下一个 token 的概率分布 → 按概率采样输出。

---

## 🧪 摸底自测

#### Q1（入门）
> Softmax 把 logits 变成概率分布的核心公式是什么？它保证输出的两个关键性质是什么？

<details>
<summary>点击查看解答思路</summary>

公式：P(i) = e^z_i / Σ e^z_j。两个性质：① 所有输出的和为 1（概率分布的基本要求）；② 每个输出在 0 到 1 之间。
</details>

#### Q2（进阶）
> Temperature 对 Softmax 的影响是什么？Temperature → 0 和 Temperature → ∞ 会怎样？

<details>
<summary>点击查看解答思路</summary>

Softmax 变成：P(i) = e^(z_i/T) / Σ e^(z_j/T)。T→0：概率分布极端化（最高概率的 token 趋近 1，其他趋近 0）→ 确定性输出。T→∞：概率分布平坦化（所有 token 概率趋近相等）→ 随机输出。T=1：保持原始分布。
</details>

## 参见
- [[Transformer]] — Softmax 在架构中的最终位置
- [[Temperature & Sampling]] — 在 Softmax 之前调节概率分布的"陡峭度"

## 参考文献
- Bridle, J. S. (1990). *Probabilistic Interpretation of Feedforward Classification Network Outputs, with Relationships to Statistical Pattern Recognition*. NATO ASI Series.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press.
