# 混合专家模型（MoE, Mixture of Experts）

> ✅ 已学习 — 2026-05-18
>
> MoE = 把 Transformer 的 FFN 层拆成多个"专家"，每次只激活其中几个。
> **效果看总参数，成本看激活参数。**

---

## 一句话本质

**MoE 用更大的总参数量换效果，用稀疏激活换成本。**

```
Dense 模型（如 Llama 3 70B，你之前学的都是这种）：
  总参数 = 每次激活的参数 = 70B
  效果 70B 级 / 成本 70B 级

MoE 模型（如 DeepSeek-V3）：
  总参数 = 671B（100+ 专家）
  每次激活 = 37B（只激活 2 个专家）
  效果接近 671B 级 / 成本接近 37B 级
```

---

## MoE vs Dense

```
Dense 模型的一层：
  输入 → 一个大 FFN（全算）→ 输出

MoE 模型的一层：
  输入 → Router → 专家 A + 专家 B（只算这两个）→ 合并 → 输出
                  其他专家不动，不占计算量
```

| 对比 | Dense（Llama 3 70B） | MoE（DeepSeek-V3） |
|---|---|---|
| **总参数** | 70B | 671B |
| **每次激活** | 70B（全部） | 37B（top-2） |
| **效果基准** | 70B 级 | ~671B 级 |
| **推理成本** | 70B 级的 GPU | 37B 级的 GPU |
| **显存需求** | 70B 级 | 671B 级（全部专家要加载） |

---

## Router（路由网络）：谁来分配专家

Router 是一个极小的神经网络，和模型一起训练出来的。

```
输入 token → Router 线性层 → 每个专家的分数（256 维）
            → Softmax → 概率 → 选 top-2 专家
```

Router 不是人写规则——它自己在训练中学会：

```
token "猫" → Router 发现专家 37 在处理动物类 token 时表现更好
            → 自动给专家 37 高分 → 下次还给专家 37

token "print" → Router 发现专家 102 处理代码类 token 更好
              → 自动给专家 102 高分
```

---

## MoE 的核心挑战：专家失衡

```
训练到一半，Router 总是选同一个专家：
  专家 7 被调用了 5000 次（训练充分）
  专家 132 被调用了 3 次（浪费参数）

→ Router 陷入路径依赖
```

**解法：负载均衡 Loss**

```
总 Loss = 标准 Loss + λ × 负载均衡惩罚

λ 控制力度：
  太大 → 均匀分配但效果下降  
  太小 → 失衡依旧
```

---

## DeepSeek-V3 的 MoE 结构

```
16 层 MoE，每层 256 个专家
每个专家 ≈ 2.6B 参数

总参数：256 × 2.6B × 多层重叠 ≈ 671B
激活参数：2 × 2.6B + Router + 共享层 ≈ 37B

效果 ≈ 671B Dense 模型
成本 ≈ 37B Dense 模型
```

---

## MoE 的代价

| 代价 | 原因 |
|---|---|
| **显存大** | 671B 总参数都要加载进显存 |
| **通信开销** | 专家可能在不同 GPU 上 → 跨卡传输 |
| **Router 选错** | 影响输出质量 |
| **训练不稳定** | 专家失衡 + Router 震荡 |

---

## Scaling Law 和 MoE 的关系

```
Scaling Law 撞墙了（2024-2025）：
  堆参数 → 效果提升减少，成本翻倍

MoE 是解：
  堆参数 → 总参数暴涨，但激活参数没涨多少
  Scaling Law 还在起作用——只是从"堆全部参数"变成了"堆总参数"
```

---

## 降维映射

```
Dense 公司：70 人，干任何项目都是全员出动
  → 哪怕改一个标点符号，70 人全参与

MoE 公司：671 人，100 多个专业部门
  → 接"写封邮件"的任务 → 只派邮件部的 4 个人干
  → 接"修服务器"的任务 → 只派运维部的 3 个人干
  → 公司总人数 671（能力），每次干活只花几个人的钱（成本）

Router = 前台，会慢慢学会"技术问题找技术部"
  前台犯错 → 公司赔钱（Loss 升高）→ 前台学会不犯同样错
```

---

## 🧪 摸底自测

#### Q1（入门）
> MoE 和 Dense 模型的核心区别是什么？Dense 模型你学过的哪些是？

<details>
<summary>解答</summary>

Dense：所有参数在推理时全部激活（GPT、Llama、Claude）。MoE：总参数很大（如 671B），但每次只激活一部分（如 37B）。效果看总参数，成本看激活参数。
</details>

#### Q2（进阶）
> Router 是怎么决定 token 分配给哪个专家的？它是写死的还是学出来的？

<details>
<summary>解答</summary>

Router 是一个小神经网络，给每个专家打分，选 top-2。不是写死的——它和模型一起训练，自己学会哪种 token 适合哪个专家。
</details>

#### Q3（思考）
> 专家失衡是什么问题？怎么解决的？

<details>
<summary>解答</summary>

Router 总选某些专家，其他专家用不上。解法：在总 Loss 里加负载均衡惩罚，让 Router 更均匀地分配。
</details>

---

## 参考文献
- Shazeer, N. et al. (2017). *Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer*. ICLR.
- DeepSeek-AI. (2024). *DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model*. arXiv:2405.04434.
- Kaplan, J. et al. (2020). *Scaling Laws for Neural Language Models*. arXiv:2001.08361.

*关联阅读：[[Transformer]] · [[FFN]] · [[Scaling Law]] · [[Dense 模型]] · [[推理优化与部署]] · [[Pre-training]]*
