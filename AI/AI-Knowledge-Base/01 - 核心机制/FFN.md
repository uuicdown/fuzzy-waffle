# 前馈网络（FFN，Feed-Forward Network）

#核心机制 #ffn

## 一句话
Transformer Block 中 Attention 之后的那一步——一个简单的两层 MLP，先把输入放大 4 倍、经过激活函数、再缩小回原维度，对 Attention 找到的信息做进一步加工。

## 数学公式

```
FFN(x) = W₂ · ReLU(W₁ · x + b₁) + b₂
```

- 第一层：`W₁ · x + b₁` — 输入从 d 维放大到 4d 维
- 激活：`ReLU(...)` — 小于 0 的置 0，大于 0 的保留
- 第二层：`W₂ · ... + b₂` — 从 4d 维缩小回 d 维

## 为什么先放大再缩小？

**降维映射**：你在写一份报告，先把所有想到的观点全部写到白板上（放大），然后筛选整理出最终版本（缩小）。

放大到更高维度让模型有更多"空间"来变换和重组信息，再压缩回去形成精简的输出。

## Attention vs FFN 的分工

| 组件 | 职责 | 类比 |
|---|---|---|
| **Attention** | 决定"该看哪里" | 派人去不同部门收集信息 |
| **FFN** | 对收集到的信息做加工 | 把收集到的信息分析处理 |

**Attention 不改变信息的维度，FFN 改变维度。** Attention 是做"路由"的，FFN 是做"计算"的。

---

## 🧪 摸底自测

#### Q1（入门）
> FFN 在 Transformer 中的角色是什么？如果说 Attention 是"做路由/做聚焦"的，那 FFN 是做什么的？

<details>
<summary>点击查看解答思路</summary>

Attention 决定"哪些信息值得关注"（路由/聚焦），FFN 对关注到的信息做"计算加工/特征变换"。Attention 不改变数据的维度，FFN 先升维再降维（通常 4x），在这个高维空间中进行更复杂的非线性变换。
</details>

#### Q2（进阶）
> Transformer Block 中为什么要先 Attention 再 FFN？调换顺序会怎样？

<details>
<summary>点击查看解答思路</summary>

Attention 先做"信息路由"，把不同位置的信息聚合到每个 token 上。聚合之后的信息再由 FFN 独立地做逐位置的计算加工。如果先 FFN 再 Attention，计算加工是对"没有看过上下文的信息"做的，效果大打折扣。
</details>

## 参见
- [[Transformer]] — FFN 是 Transformer Block 的右半部分
- [[神经网络]] — FFN 本质就是一个 MLP

## 参考文献
- Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS.
- Hendrycks, D. & Gimpel, K. (2016). *Gaussian Error Linear Units (GELUs)*. arXiv:1606.08415.
