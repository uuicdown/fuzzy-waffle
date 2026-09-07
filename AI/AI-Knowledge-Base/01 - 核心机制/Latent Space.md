# 潜空间（Latent Space）

#核心机制 #latentspace

## 一句话
AI 进行所有内部推理的高维数学空间——你看到的是文字，AI 看到的是这个空间里的坐标运动。

## 关键点
- 所有输入被投影到这个空间，所有推理在这里完成，所有输出从这里反推回去
- 人脑无法直接理解这个空间——它摊平了一个概念的所有特征维度
- 参数越多 = Latent Space 越大（10m² 毛坯房 vs 三层别墅）

## 降维映射
柏拉图的洞穴寓言——你看到墙上的影子（输入文字），AI 的工作发生在墙后的真实世界（Latent Space）。

---

## 🧪 摸底自测

#### Q1（入门）
> Latent Space 的"空间"到底指什么？不是物理空间，那是什么？

<details>
<summary>点击查看解答思路</summary>

Latent Space 是一个高维向量空间（比如 4096 维），每个 token 在进入模型后被投影到这个空间中的一个点（向量）。"空间"是指数学意义上的向量空间——维度之间相互独立，可以在里面做加法减法、测量距离。
</details>

#### Q2（进阶）
> 模型在 Latent Space 里做的"推理"到底是在做什么操作？

<details>
<summary>点击查看解答思路</summary>

推理 = 在 Latent Space 中沿着特定方向移动向量。Attention 把其他位置的向量拉向当前位置（信息聚合），FFN 对向量做非线性变换（计算加工）。模型的"思考过程"就是向量在 Latent Space 中不断移动和变换的过程。
</details>

## 参见
- [[Embedding]] — 进入 Latent Space 的入口编码器
- [[Attention]] — 在 Latent Space 里的主要操作
- [[Transformer]] — 在 Latent Space 里堆叠的多层变换

## 参考文献
- Bengio, Y. et al. (2013). *Representation Learning: A Review and New Perspectives*. TPAMI.
- Tenenbaum, J. et al. (2000). *A Global Geometric Framework for Nonlinear Dimensionality Reduction*. Science.
