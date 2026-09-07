# 位置编码（Positional Encoding / RoPE）

> ✅ 已学习 — 2026-05-18
>
> Transformer 架构的最后一块缺的拼图。

---

## 一句话本质

**给每个 token 加上"我在第几位"的信号，让 Self-Attention 知道 token 的顺序。**

没有位置编码时，"I love you" 和 "you love I" 在 Attention 看来完全一样——因为它对"集合"做运算，不天然区分顺序。

---

## 为什么简单的整数不行

```
token 1 → 位置值 1
token 2 → 位置值 2
...
token 1000 → 位置值 1000

两个致命问题：
  ① 数值太大，盖过 Embedding 本身
     位置 1000 的数值是 Embedding 平均值的几十倍 → 语义信号被淹没
     
  ② 无法泛化到没见过长度
     训练时最长 512 token → 模型没见过位置 513
     给 1000 token → 位置 513-1000 是全新数字 → 乱掉
```

---

## 方案一：正弦波编码（Sinusoidal，Transformer 原作）

```
不直接写整数，用 sin/cos 在不同频率上生成位置向量：

位置 1： [sin(1/1), cos(1/1), sin(1/2), cos(1/2), ...]
位置 2： [sin(2/1), cos(2/1), sin(2/2), cos(2/2), ...]

值在 [-1, 1] 之间 → 不爆炸
任意长度都能算 → 函数可以生成任何位置的值
```

**局限：位置信号只在输入层加一次，经过几十层 Transformer 后逐渐被稀释。**

---

## 方案二：RoPE（旋转位置编码）—— 当前主流

**不是在 Embedding 上加位置信号，而是在每层 Attention 计算 Q·K^T 时对 Q 和 K 做旋转。**

```
Attention(Q_i, K_j) 在算 i 和 j 的相似度时，
自动把 (i-j) 的位置差编码进分数里。

模型天然关注"两个 token 相距多远"而非"各自在第几位"。
```

| 特性 | 正弦波编码 | RoPE |
|---|---|---|
| 编码方式 | 固定函数值 | 旋转操作 |
| 位置类型 | 绝对位置 | 相对位置 |
| 施加位置 | 输入层一次 | 每层 Attention 都施加 |
| 长文本泛化 | 差（训练 4K，推 4K） | 强（训练 4K，推 128K） |
| 代表模型 | Transformer 原作 | Llama / DeepSeek / Qwen |

---

## 降维映射

```
正弦波编码 = 进公司时在工牌上写"我是第 3 号员工"
  10 年后，这张工牌早就看不清了（位置信息被层层稀释）

RoPE = 每次开会介绍时都说"我比张三晚来两年"
  不是靠一次性的标签，而是靠持续的关系比较
  天然支持"第 100 个和第 1000 个人的关系"
```

---

## 🧪 摸底自测

#### Q1（入门）
> 为什么 Transformer 需要位置编码？整数编码为什么不行？

<details>
<summary>解答</summary>

Self-Attention 对集合做运算，不天然区分顺序。整数编码的问题：大数值盖过语义信号，且无法泛化到训练时没有见过的长度。
</details>

#### Q2（进阶）
> RoPE 和正弦波编码最大的区别是什么？

<details>
<summary>解答</summary>

正弦波编码用固定函数算位置值（绝对位置），只在输入层加一次。RoPE 在每层 Attention 计算时对 Q 和 K 做旋转（相对位置），位置信息不会随层数衰减，支持更好的长文本泛化。
</details>

---

## 参考文献
- Vaswani, A. et al. (2017). *Attention Is All You Need* (Section 3.5: Positional Encoding). NeurIPS.
- Su, J. et al. (2021). *RoFormer: Enhanced Transformer with Rotary Position Embedding*. arXiv:2104.09864.

*关联阅读：[[Transformer]] · [[Attention]]*
