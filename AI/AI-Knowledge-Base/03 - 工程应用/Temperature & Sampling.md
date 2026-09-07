# 温度与采样（Temperature & Sampling）

#工程应用 #temperature #sampling

## 一句话
控制模型输出"随机性"的旋钮。Temperature 越高，输出越不可预测、越"有创意"；越低，输出越稳定、越"一致"。

## 关键点
- **低 Temperature（0～0.3）**：每次都选概率最高的 token → 代码、算术、事实问答
- **中 Temperature（0.5～0.7）**：按概率随机选 → 日常对话、客服
- **高 Temperature（0.8～1.2）**：概率分布被抹平，小概率选项被激活 → 创作、头脑风暴
- 还有 Top-k 和 Top-p（Nucleus Sampling）等变体采样策略

## 降维映射
喝了几杯酒。清醒=低 Temperature，输出稳定；醉酒=高 Temperature，输出发散、不可预测。

## 认知盲区
❌ 高温=聪明有创意 → ✅ 高温=增加随机性。同一个模型高温时写出惊世之作的概率和写出垃圾的概率同时增加。

## 参见
- [[Autoregressive Decoding]] — 每一步都用 Temperature 来决定"怎么猜"
- [[Prompt Engineering]] — 低 Temperature 时 Prompt 更关键，因为模型不会自己发散补偿

## 参考文献
- Ackley, D. H. et al. (1985). *A learning algorithm for Boltzmann machines*. Cognitive Science.
- Hinton, G. E. (2002). *Training Products of Experts by Minimizing Contrastive Divergence*. Neural Computation.
- Holtzman, A. et al. (2019). *The Curious Case of Neural Text Degeneration* (Top-p sampling). ICLR.
