# 幻觉（Hallucination）

#安全与对齐 #hallucination

## 一句话
LLM 编造出看似合理但实际错误的"事实"——不是"说谎"，而是因为模型没有"事实准确"这个机制，只有"下一个词可信"这个机制。

## 关键点
- 根本原因：模型是 probability predictor，不是 database querier
- 表现：小到算错 3+5，大到编造论文引用和法庭判例
- 缓解方法：
  - RAG（检索→给原文参考）
  - 低 Temperature（减少发散）
  - Prompt（要求「不确定就说不知道」）

## 降维映射
一个很会说话的脱口秀演员，即兴被问到"法国 1789 年的小麦价格"——他不想冷场，所以编了一个听起来合理的数据。他不是恶意的，他只是不喜欢说"我不知道"。

## 参见
- [[RAG]] — 缓解幻觉的最实用方法
- [[Autoregressive Decoding]] — 产生幻觉的底层原因
- [[Temperature & Sampling]] — 高 Temperature 会加重幻觉

## 参考文献
- Ji, Z. et al. (2023). *Survey of Hallucination in Natural Language Generation*. ACM Computing Surveys.
- Maynez, J. et al. (2020). *On Faithfulness and Factuality in Abstractive Summarization*. ACL.
- Shuster, K. et al. (2021). *Retrieval Augmentation Reduces Hallucination in Conversation*. arXiv:2104.07567.
