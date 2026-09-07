# 自回归解码（Autoregressive Decoding）

#核心机制 #decoding

## 一句话
LLM 生成文本的方式：一次只预测下一个 token，把新生成的 token 拼到已有序列末尾，再预测下下个 token，依次类推。

## 关键点
- 每一步只看已生成的内容+输入 → 猜下一个
- 不存在"全局规划"——写到第 100 个 token 时不知道第 200 个会是什么
- 所有"看起来像在思考"的效果，都是注意力机制在前几步和后几步之间建立的关联

## 降维映射
一个人蒙眼走迷宫——每走一步只能看到脚下的路，走完这一步才能看到下一步。

---

## 🧪 摸底自测

#### Q1（入门）
> 为什么 LLM 生成时必须一个 token 一个 token 地来，而不能一次生成完整段落？

<details>
<summary>点击查看解答思路</summary>

因为模型是自回归的：每一步的输入依赖于上一步的输出。在生成第 10 个 token 之前，模型不知道自己会生成第 1-9 个 token 是什么——它必须"先走完才能看到下一步"。
</details>

#### Q2（进阶）
> Greedy Decoding（贪心解码）和 Beam Search（束搜索）的核心区别是什么？

<details>
<summary>点击查看解答思路</summary>

Greedy Decoding：每一步只选当前概率最高的 token，不考虑后续影响。Beam Search：每一步保留 top-k 条候选路径，探索更多的可能性再选最优的。Greedy 快但可能错过更好的路径，Beam Search 质量更高但更慢。
</details>

## 参见
- [[Temperature & Sampling]] — 决定每一步"怎么猜"
- [[Attention]] — 每一步猜的时候看之前哪一步的信息

## 参考文献
- Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS.
- Brown, T. et al. (2020). *Language Models are Few-Shot Learners* (GPT-3). NeurIPS.
- Sutskever, I. et al. (2014). *Sequence to Sequence Learning with Neural Networks*. NeurIPS.
