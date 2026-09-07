# 检索增强生成（RAG，Retrieval-Augmented Generation）

> ✅ 已学习 — 2026-05-18（补全完整流水线）
>
> RAG = 让 AI 在回答问题时，先从外部知识库中检索相关原文，然后"看着原文回答"。不改模型参数。
> **能 RAG 就别 Fine-tune。**

---

## 完整流水线

```
用户提问："什么是 RAG？"
   │
   ▼
┌──────────────────────────────────────────┐
│ ① Embedding：用户问题 → 向量              │
│   用专门的 Embedding 模型（BGE/Ada/GTE）    │
│   不是 LLM 内部的 Embedding，是专门训的     │
│   输出：768 维 / 1024 维 向量              │
└────────────────┬─────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────┐
│ ② 向量搜索：在知识库里找 top-20           │
│   向量数据库（FAISS / Pinecone / Milvus）  │
│   算法：HNSW（分层可导航小世界图）          │
│   → 从顶层地标下到底层精确搜索             │
│   → 只比较几十个向量，不是 100 万个        │
│   参数 ef_search 控制精度 vs 速度          │
└────────────────┬─────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────┐
│ ③ Reranker（重排序器）                    │
│   向量搜索是"粗筛"——语义接近但不一定最相关   │
│   Reranker 把问题 + 文档原文逐条算匹配分    │
│   → 精确重排 → 取 top-3/5                 │
│   降维：海选按身高筛（向量）→ 面试逐人聊    │
└────────────────┬─────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────┐
│ ④ 拼接：用户问题 + 检索文档 → prompt      │
│   prompt = System + 检索结果 + 用户问题    │
└────────────────┬─────────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────┐
│ ⑤ LLM 生成：基于上下文回答                 │
│   模型看到的不是它"记住"的知识              │
│   是你刚塞进上下文的检索文档                │
└────────────────┬─────────────────────────┘
                 │
                 ▼
              输出回答
```

---

## 核心组件详解

### ① Embedding 模型

```
LLM 的 Embedding（4096 维）
  → 用来做推理，不是为搜索优化

检索专用的 Embedding（768/1024 维）
  → 专门训练来做"语义匹配"
  → 代表：BGE / text-embedding-3-small / gte-Qwen2
  → 输入：文本 → 输出：向量
```

### ② 向量搜索与 ANN 索引

```
暴力搜索：100 万篇逐条算距离 → 100 万次 → 太慢

ANN（Approximate Nearest Neighbor）：近似最近邻搜索
  先建索引 → 搜索时只查索引指向的那一小批 → 快几千倍

最主流算法：HNSW（分层可导航小世界图）
  第 3 层（顶层）：几个"地标"向量
  第 2 层：几十个"区域中心"
  第 1 层（底层）：全部 100 万向量
  
  搜索：顶层找地标 → 下第 2 层找区域 → 下底层找精确点
  全程只比较了几十个向量

参数 ef_search：
  = 10  → 非常快，可能漏掉最相关的
  = 200 → 较慢，几乎不漏（生产常用）
```

### ③ Reranker（重排序器）

```
向量搜索的问题：语义接近 ≠ 实际相关
  用户问："如何用 Python 读取 CSV？"
  向量 top-1："Python pandas 读取 Excel" ← 语义接近但不是 CSV
  
Reranker 做的事：
  把原始问题 + 每条文档原文 → 小模型逐条算匹配分
  → 重新排序 → 取最相关的 top-3/5

和向量搜索的区别：
  向量搜索：问题+文档都转向量 → 算距离（快，粗）
  Reranker：问题+文档原文一起算 → 算匹配分（慢，精）
  两者是互补关系，不是替代关系
```

---

## 常见向量数据库

| 产品 | 特点 | 场景 |
|---|---|---|
| **FAISS** | Meta 开源库，纯向量检索 | 本地/小规模，Python 直接调 |
| **Chroma** | 轻量级，本地运行 | 个人项目、原型 |
| **Pinecone** | 云端 SaaS，全托管 | 生产环境，不需要自己运维 |
| **Milvus** | 分布式，云原生 | 千万级以上大规模 |
| **Qdrant** | Rust 实现，性能好 | 中等规模 |

---

## 降维映射

```
开卷考试 vs 闭卷考试（Fine-tuning）

RAG = 开卷：翻书找答案，换书就能换知识
Fine-tune = 闭卷：把书背进脑子里，换书要重新背

RAG 改知识库 = 换书（换文档就行，不用动模型）
Fine-tune 改知识 = 背书（重新训练，成本高）
```

---

## 五层进化路线

| 层级 | 目标 | 代表方案 |
|---|---|---|
| **L1：搜得到** | 基础检索召回 | Naive RAG / Multi-Query RAG / HyDE |
| **L2：搜得准** | 检索质量提升 | Semantic Chunking / Hybrid Search / **Reranking** |
| **L3：搜不歪** | 自我纠错 | CRAG / Self-RAG / FLARE |
| **L4：知识图谱** | 结构化增强 | GraphRAG / LightRAG |
| **L5：Agentic** | 自主编排 | Agent 自主任意组合检索策略 |

**生产级基线 = L2**：语义分块 + Hybrid Search + Reranking

---

## 🧪 摸底自测

#### Q1（入门）
> RAG 流水线有哪五个步骤？向量搜索和 Reranker 各负责什么？

<details>
<summary>解答</summary>

Embedding → 向量搜索 → Reranker → 拼接 → LLM 生成。向量搜索做"粗筛"（快，语义接近），Reranker 做"精排"（慢，精确匹配）。
</details>

#### Q2（进阶）
> 向量数据库搜索 100 万篇文档时，为什么不用逐条算距离？HNSW 怎么加速的？

<details>
<summary>解答</summary>

用 ANN（近似最近邻搜索），HNSW 建多层索引（顶层地标→中层区域→底层全部）。搜索从顶层开始逐层下钻，只比较几十个向量而非 100 万个。
</details>

#### Q3（思考）
> RAG 和 Fine-tuning 各自最适合什么场景？

<details>
<summary>解答</summary>

RAG 适合：知识经常更新、需要引用来源、长尾冷门知识。Fine-tune 适合：需要改变模型行为/风格、频繁使用的核心知识、推理时不能有额外检索延迟。行业铁律：能 RAG 就别 Fine-tune。
</details>

---

## 参考文献
- Lewis, P. et al. (2020). *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*. NeurIPS.
- Izacard, G. et al. (2022). *Atlas: Few-shot Learning with Retrieval Augmented Language Models*. arXiv.
- Shuster, K. et al. (2021). *Retrieval Augmentation Reduces Hallucination in Conversation*. arXiv.
- Malkov, Y. A. & Yashunin, D. A. (2016). *Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs*. TPAMI.

*关联阅读：[[Fine-tuning]] · [[Embedding]] · [[Hallucination]] · [[LLM评估体系]] · [[向量数据库]]*
