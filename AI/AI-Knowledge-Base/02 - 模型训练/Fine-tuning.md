# 微调（Fine-tuning）

#模型训练 #finetuning

## 一句话
用特定领域的数据，对预训练好的模型做"二次训练"，修改模型参数，让它更擅长某个任务。

## 关键点
- 闭卷考试——知识被写进模型参数里
- 成本高（需要算力）、改知识麻烦（需要重新训）
- 适用场景：行为风格迁移、特定能力固化
- 轻量级变体：LoRA（只改 1% 的参数，成本极低）

## 降维映射
封校考试前把《中国通史》背进脑子里——不翻书也能答，但背错了要重新背。

## 行业铁律
能 RAG 就别 Fine-tune。先用检索解决问题，再考虑微调。

## 参见
- [[RAG]] — 开卷考试方案
- [[LoRA & PEFT]] — 低成本微调
- [[SFT（监督微调）]] — Fine-tuning 的一种具体方式

## 参考文献
- Howard, J. & Ruder, S. (2018). *Universal Language Model Fine-tuning for Text Classification* (ULMFiT). ACL.
- Brown, T. et al. (2020). *Language Models are Few-Shot Learners* (GPT-3). NeurIPS.
- Hu, E. et al. (2021). *LoRA: Low-Rank Adaptation of Large Language Models*. ICLR.
