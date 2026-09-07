# 嵌入（Embedding）

#核心机制 #embedding

## 一句话
把单词/Token 转换成一个高维空间中的坐标点。语义相近的词，坐标距离近。

## 关键点
- 每个词对应一个由几百个数字组成的向量（如 768 维）
- 「国王」-「男人」+「女人」≈「女王」— 这是 Embedding 空间的**固有几何性质**
- Embedding ≠ Attention。Embedding 负责"投影"，Attention 负责"聚焦"

## 降维映射
RGB 颜色。 (255,0,0)=纯红，(255,50,50)=浅红——距离反映相似度。

## 认知盲区
❌ 以为"国王-男人+女人"是 Attention 做的 → ✅ 纯属 Embedding 空间的自带性质

---

## 🧪 摸底自测

#### Q1（入门）
> Embedding 的核心思想是什么？相比于 One-Hot 编码，它好在哪里？

<details>
<summary>点击查看解答思路</summary>

Embedding 把离散的 token 映射到连续的低维向量空间。One-Hot 的问题：维度太大（词表大小=100K 维）、向量之间没有语义关系（"猫"和"狗"的距离和"猫"和"椅子"一样远）。Embedding 让语义相近的词在向量空间中距离近。
</details>

#### Q2（进阶）
> "king - man + woman ≈ queen" 这个经典例子说明了 Embedding 的什么性质？

<details>
<summary>点击查看解答思路</summary>

说明 Embedding 空间具有语义方向和类比推理的能力。Embedding 不仅编码了"是什么"，还编码了"关系"——"男性"这个方向向量可以从 king 中减去，再加上"女性"方向得到 queen。
</details>

## 参见
- [[Tokenization]] — 先切碎，再投影
- [[Attention]] — 在 Embedding 空间里做动态聚焦
- [[Latent Space]] — Embedding 投影进去的那个"战场"

## 参考文献
- Mikolov, T. et al. (2013). *Efficient Estimation of Word Representations in Vector Space*. arXiv:1301.3781.
- Pennington, J. et al. (2014). *GloVe: Global Vectors for Word Representation*. EMNLP.
- Devlin, J. et al. (2018). *BERT: Pre-training of Deep Bidirectional Transformers*. NAACL.
