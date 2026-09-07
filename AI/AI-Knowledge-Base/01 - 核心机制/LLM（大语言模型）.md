# 大语言模型（LLM，Large Language Model）

#核心机制 #llm

## 一句话
基于海量文本训练、用自回归方式预测下一个 token 的神经网络模型。这是目前所有现代 AI 对话系统的顶层形态。

## 关键点
### LLM 的本质
- **不是计算器，不是知识库，不是思考者**
- 底层机制只有一个：给定前面的 token，预测下一个最可能的 token
- "看起来在推理/思考" = Attention 在潜空间中建立的长程关联产生的涌现效果

### 规模划分
- **小模型**（< 10B）：可在消费级 GPU 上运行，适合简单任务、本地部署
- **中模型**（10B ~ 100B）：需要多卡部署，适合通用任务
- **大模型**（100B ~ 1T+）：需要大规模集群，涌现复杂推理能力

### LLM 的典型能力从哪里来
| 能力 | 来源 |
|---|---|
| 理解和生成语言 | Pre-training |
| 像人一样对话 | SFT（监督微调） |
| 安全礼貌 | RLHF |
| 事实准确 | RAG |
| 特定领域精通 | Fine-tuning |
| 自主执行任务 | Agent + Function Calling |

## 降维映射
一个背下了整个互联网的超级模仿者——它没有"自己的想法"，但能根据你给的上下文，极其精准地猜出你最想听到的下一句话。

## 认知盲区
❌ LLM 在"思考" → ✅ LLM 在"匹配模式"。你看到的是思考的表象，底层只有概率分布。

---

## 🧪 摸底自测

#### Q1（入门）
> LLM 的全称是什么？它的"大"具体体现在哪几个方面？

<details>
<summary>点击查看解答思路</summary>

Large Language Model。"大"体现在三方面：参数量大（数十亿到数千亿）、训练数据量大（万亿级 token）、计算量大（数万 GPU·月）。这三个"大"共同导致能力的涌现。
</details>

#### Q2（进阶）
> 大模型（70B+）和小模型（1B-8B）的本质区别是什么？不仅是参数数量。

<details>
<summary>点击查看解答思路</summary>

大模型在某些能力上存在"涌现阈值"——参数不够多时某些能力完全不会出现（比如多步推理、In-Context Learning），而不是"变弱了而已"。小模型永远达不到这些能力，这不是量变是质变。
</details>

## 参见
- [[Embedding]] — 进入 LLM 的第一道门
- [[Attention]] — LLM 的核心运算
- [[Autoregressive Decoding]] — LLM 生成方式的本质
- [[Transformer]] — LLM 的架构载体
- [[模型规模分类（大模型 vs 小模型）]] — 规模划分的详细边界

## 参考文献
- Brown, T. et al. (2020). *Language Models are Few-Shot Learners* (GPT-3). NeurIPS.
- OpenAI. (2023). *GPT-4 Technical Report*. arXiv:2303.08774.
- Kaplan, J. et al. (2020). *Scaling Laws for Neural Language Models*. arXiv:2001.08361.
