# 分词（Tokenization）

> ✅ 已学习 — 2026-05-18

---

## 底层逻辑总纲

**Tokenization 不是"分词"，而是"给模型准备食物"——决定模型以多粗的颗粒度去理解语言。**

一个段子准确描述了这个概念的荒谬性：
> 人类用字母和单词写作，用句子表达思想。
> 大模型用 token 思考，而 token 是字母和单词的统计折中——它既看不见字母，也不一定知道完整单词。

Tokenization 的核心矛盾只有一个：**用尽可能少的 token 数，表达尽可能多的语言内容，同时每个 token 是"有意义的语言单元"。**

---

## 三个极端方案与折中

| 方案 | 好 | 坏 | 类比 |
|---|---|---|---|
| **字符级** (char-level) | 零信息损失，不丢任何字母 | 序列爆炸（Attention O(n²) 灾难），模型自己从零学词 | 每根面条现拉——太慢了 |
| **词级** (word-level) | 序列最短，每个 token 有意义 | 词表无限，遇到生词就翻车（OOV） | 全预制菜——菜单上没有的吃不到 |
| **子词级** (subword-level / BPE) ✅ | 兼顾压缩率与覆盖度 | 模型看不见字母，某些任务翻车 | 备好常见配料，自由拼装 |

---

## BPE 算法工作机制

Byte Pair Encoding：**从最原子的单位开始，按"出现频率"逐轮合并相邻 pair，直到词表被填满。**

```
初始：  t  h  e  r  e  _  i  s  _  a  _  c  a  t        ← 字符级
第 N 轮：the  re  _  is  _  a  _  ca  t                    ← "the"、"re"、"is" 被合并
第 M 轮：the  re  _  is  _  a  _  cat                       ← "cat" 被合并（够高频）
最终：  there  is  a  cat                                  ← "there" 被合并
```

**决定分界线的不是语法（词根词缀），是统计学（出现频率）。**

---

## 降维映射

### 拉面馆后厨 🍜

```
字符级       = 每根面条现拉 → 新鲜但极慢
词级         = 全预制菜    → 快但菜单有限
子词级 (BPE) = 备好常见配料（汤底、叉烧、溏心蛋、葱花）→ 90% 1秒出餐，10% 多拼几样
```

Tokenization/BPE 本质上就是拉面馆的后厨管理——把最常被点的"配料组合"做成预制包，低频需求就拆成现有配料重新拼。

### 词汇表 = 餐厅菜单 📋

- **菜单太小（16K）**：很多菜只能拆成配料描述，顾客点单很长
- **菜单适中（32K-100K）**：覆盖 95% 的常见菜，效率最高
- **菜单太大（200K+）**：后厨备菜太多，每道菜炒的次数不够，质量下降

---

## 五个反直觉认知

### 1. 模型不识字

这是 Tokenization 最著名的陷阱。模型看到的只有 token IDs，没有字母概念。

```
人类视角：  "reversing" 由 r-e-v-e-r-s-i-n-g 组成
模型视角：  [reverse] [ing]  ← 两个 ID

当你问 "把 reversing 反过来拼"：
  模型看不到里面的字母 → 它根本不知道 ing 里有 i/n/g
  → 答出 "gnisiver" 不是笨，是字面上不可能做对
```

这叫 **Tokenization Blindness**：为什么大模型连 "strawberry 有几个 r" 都会数错——因为 strawberry 在它眼里可能是 `[straw][berry]` 两个 token，r 的分布被抹掉了。

### 2. 中文比英文更费 token（在 GPT 类 tokenizer 下）

汉字信息密度更高，但 GPT tokenizer 下最不省 token：

```
"Artificial Intelligence will fundamentally change how humans interact with machines"
GPT-4: ~10 tokens

"人工智能将从根本上改变人类与机器交互的方式"
GPT-4: ~16 tokens

中文 ≈ 英文的 1.6 倍 ↑
```

原因：
- **训练数据比例**：GPT tokenizer 训练数据中英文占 ~90%，中文仅 ~5%
  → 英文的 `un`、`tion` 得到上亿次合并机会
  → 中文的「人工」「智能」合并机会少 10 倍
- **没有空格**：英文的天然词边界让 BPE 知道往哪合；中文没有词边界，效率天然低

### 3. 词表不是越大越好

| 词表大小 | 优势 | 代价 |
|---|---|---|
| 太小（16K） | 每个 token 训练充分 | 句子长、占 context、算得慢 |
| 适中（32K-100K） | 效率+质量平衡 | 需要权衡稀有 token 的数据量 |
| 太大（200K+） | 句子极短省 token | Embedding 层巨大，稀有 token 学不够 → 过拟合 |

GPT-4：~100K → 长上下文优先；Llama 3：32K → 紧凑模型体积优先。

### 4. 英文 tokenization 既不是"分词"也不是"分字母"

它是 **从字母开始生长，在常用子词处停住**：

```
"the"          → 1 token（超高频单词被整体打包）
"thinking"     → think + ing（2 tokens）
"unbelievable" → un + believ + able（3 tokens）
"strengths"    → 1 token（虽然长但足够高频）
```

分界的唯一标准是**统计频率**，不是语言学。

### 5. Token 没有硬性长度上限，但词表容量是最硬的天花板

BPE 理论上可以无限合并下去（如果 `MachineLearningIsFun` 出现 1000 万次，它就是 1 个 token）。但词表一满（比如 100K 个 tokens），新短语就只能拆成现有子 token 拼了。

---

## DeepSeek 的优化方案（中文优先）

| 策略 | 具体做法 | 效果 |
|---|---|---|
| **更大的词表** | 128K tokens（vs GPT-4 的 100K） | 多 2.8 万槽位给中文常用字词 |
| **更高的中文训练比例** | Tokenizer 训练数据中中文占比大幅提升 | "人工智能"等高频组合被合并为单 token |
| **CJK 专用预切分** | 连续 CJK 文字作为一个"块"保留，不在中间乱切 | 中文 BPE 合并效率更高 |
| **常用汉字预置** | 初始词表直接包含 ~2 万个 CJK 统一汉字 | 每个汉字起点就是 1 token，而不是 3 字节 |

**效果**：中文 token 效率比 GPT-4 高 ~35-40%，英文基本持平或略多 1-2 个 token。

> DeepSeek 的策略本质：**用词表空间换中文效率。**

---

## 各模型 Tokenizer 横向对比

| 模型 | 词表大小 | 类型 | 中文效率 | 特点 |
|---|---|---|---|---|
| GPT-4 (cl100k) | ~100K | BBPE | 低（英文最优） | 英文 token 极省，中文 ~1.6x |
| DeepSeek-V3 | 128K | BBPE | 高 | 中文省 ~35%，中英文更均衡 |
| Llama 3 | 32K | BBPE | 低 | 紧凑词表，全面但都不极致 |
| Qwen | 152K | BBPE | 最高 | 中英文双优 |
| Claude (claude_tokenizer) | ~100K | BBPE | 中 | 多语言平衡，但无公开细节 |

---

## 实用分析框架

面对任何 tokenization 相关问题时，按三步拆解：

### Step 1：确认 tokenizer 类型
```
是 BPE / BBPE / Unigram / WordPiece？
  → BPE 是频率驱动的最主流方案
```

### Step 2：评估对当前任务的影响
```
- 任务涉及逐字母操作（拼写、反转、计数字母）？   → Tokenization Blindness 可能翻车
- 任务在中英文混合场景下？                        → 确认 tokenizer 是否有中文优化
- 任务需要极长上下文？                           → 单词表大小的模型更省 token（GPT-4 > DeepSeek > Llama）
```

### Step 3：判断翻车方向
```
- 模型在拼写任务上反复出错？              → 大概率 Tokenization Blindness，not 模型笨
- 中文对话 token 消耗比预期高 60%？       → 检查是否用了英文优化的 tokenizer（GPT/Claude）
- 生僻词/专有名词被切得七零八落？         → BPE 没见过它 → 预期行为
```

---

## 关键数字速记

- **BPE**：Byte Pair Encoding，从字符开始按频率合并 pair
- **词表大小**：GPT-4 ~100K / DeepSeek 128K / Llama 3 32K / Qwen 152K
- **中文落差**：GPT-4 下中文 ≈ 英文 token 数的 **1.6 倍**
- **DeepSeek 提升**：中文 token 省 **~35%**
- **O(n²)**：序列长度翻倍，Attention 计算量翻 4 倍

---

---

## 🧪 摸底自测

> 以下问题来自学习时的摸底环节。能独立答上来说明已掌握；卡住的地方就是该复习的部分。

### Q1（入门）
> 如果我把每个英文字母单独作为一个 token，会有什么问题？为什么模型不这么做？

<details>
<summary>点击查看解答思路</summary>

序列长度爆炸（100 词英文 ≈ 500 tokens），Attention O(n²) 计算量翻 25 倍。模型还得自己从零发现 "t-h-e" 是一个整体——这是 Tokenization 要解决的核心压缩问题。
</details>

### Q2（进阶）
> 你问模型「把单词 'reversing' 反过来拼」，它可能回答 'gnisiver'（错）。它错在哪？这个错误暴露了 tokenization 的什么特征？

<details>
<summary>点击查看解答思路</summary>

'Reversing' 可能被切成 [reverse] + [ing] 两个 token。模型看不到 ing 里的 i/n/g 字母——这是 Tokenization Blindness：模型不识字，只认识 token IDs。
</details>

### Q3（提升）
> GPT-4 的词表约 100k tokens，中文汉字总数超过 5 万个。它怎么在一套词表里同时高效处理英文和中文？词表越大越好吗？

<details>
<summary>点击查看解答思路</summary>

中文比英文费 token，原因是 GPT tokenizer 的训练数据中英文占绝对大头，中文常用字词没有足够的合并机会。词表不是越大越好——太大（200K+）会导致 Embedding 层巨大，稀有 token 学习样本不足而过拟合；太小（16K）则序列长、计算慢。32K-100K 是最优区间。
</details>

---

## 参考文献
- Sennrich, R. et al. (2015). *Neural Machine Translation of Rare Words with Subword Units*. ACL.
- Radford, A. et al. (2019). *Language Models are Unsupervised Multitask Learners* (GPT-2). OpenAI.
- DeepSeek-AI. (2024). *DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model*. arXiv:2405.04434.
