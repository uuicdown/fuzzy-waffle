# 基于人类反馈的强化学习（RLHF，Reinforcement Learning from Human Feedback）

#模型训练 #rlhf #alignment

## 一句话
用人类对模型输出的打分数据，训练一个评分模型，再用它来调整 LLM 的行为倾向——让模型学会"说人类喜欢听的话"。

## 关键点
- 没有改变模型"知道什么"，只改变了"选择说什么"的概率
- 模型依然"知道"怎么写敏感内容，但输出那条路被压到了极低概率
- 需要一个**Reward Model**（评分模型）做中介

## 降维映射
训狗。狗不知道"坐"是对的还是好的——它只知道"坐"了之后能拿到肉。

## 认知盲区
❌ RLHF 是在教 AI 对错 → ✅ 它是在教 AI "怎么让人类点赞"。AI的道德感是人类打分数据的统计学投影。

## 参见
- [[Alignment]] — 对齐的终极目标
- [[Fine-tuning]] — RLHF 通常跟在 SFT 之后
- [[AI Safety]] — 为什么要做对齐

## 参考文献
- Christiano, P. et al. (2017). *Deep Reinforcement Learning from Human Preferences*. NeurIPS.
- Ouyang, L. et al. (2022). *Training language models to follow instructions with human feedback* (InstructGPT). NeurIPS.
- Bai, Y. et al. (2022). *Constitutional AI: Harmlessness from AI Feedback*. arXiv:2212.08073.
