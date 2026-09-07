# Agent 开发规范

> Agent 开发和普通 API 调用的规范完全不同。Agent 要管理工具、维护对话历史、控制循环、处理错误。

---

## 当前主流语言

| 语言             | 生态     | 适合场景                 | 代表框架                                        |
| -------------- | ------ | -------------------- | ------------------------------------------- |
| **Python**     | 🔥 最强  | 入门开发、快速原型、中小规模       | LangChain / LangGraph / CrewAI / PydanticAI |
| **Rust**       | 🌱 增长中 | 大规模生产、高并发、Serverless | AutoAgents / Rig / OpenFANG                 |
| **TypeScript** | 🔶 中等  | Node 技术栈团队           | GraphBit                                    |

**建议入门走 Python**，生态最全、资料最多。Rust 是生产环境大规模部署时的优化方向。

---

## 一、System Prompt 管理

### 1.1 版本化

```
❌ 每次改 prompt 直接在代码里改，旧版本丢失
✅ 用单独的文件或变量管理版本

SYSTEM_PROMPT_V1 = "你是一个友好的助手。"
SYSTEM_PROMPT_V2 = "你是一个专业的客服，语气正式，不超过200字。"
```

### 1.2 放在 messages 最前面

```python
messages = [
    {"role": "system", "content": SYSTEM_PROMPT},  # 放最前面 → 缓存命中
    {"role": "user", "content": user_input}
]
```

---

## 二、工具注册规范

### 2.1 工具描述必须清晰

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "查询指定城市指定日期的天气",
            # ↑ description 越清晰，模型选错工具的概率越低
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "城市名，如北京、上海"
                    }
                },
                "required": ["city"]
            }
        }
    }
]
```

### 2.2 工具数量控制

```
1-3 个工具 → 模型几乎不会选错
4-6 个工具 → 偶尔选错，需要重试机制
7+ 个工具 → 频繁选错，建议拆成多个子 Agent
```

---

## 三、多轮对话历史维护

### 3.1 历史管理策略

```python
MAX_HISTORY_TOKENS = 4000  # 历史上限

def trim_messages(messages):
    """超过上限时，丢弃最旧的对话"""
    total = count_tokens(messages)
    while total > MAX_HISTORY_TOKENS and len(messages) > 2:
        messages.pop(1)  # 丢弃最早的对话（保留 system prompt）
        total = count_tokens(messages)
    return messages
```

### 3.2 三种裁减策略

| 策略       | 做法              | 适合场景             |
| -------- | --------------- | ---------------- |
| **丢弃最早** | 删最早的对话，保留最近的    | 大多数场景            |
| **摘要压缩** | 把旧对话用 LLM 压缩成摘要 | 长对话需要保留关键信息      |
| **滑动窗口** | 每次只保留最近 N 轮     | Mistral 等模型的默认方案 |

---

## 四、Function Calling 错误处理

### 4.1 工具调用失败的基本流程

```python
MAX_RETRIES = 3

for attempt in range(MAX_RETRIES):
    response = call_llm(messages, tools)
    
    if response.has_tool_call():
        tool_name = response.tool_call.name
        try:
            result = execute_tool(tool_name, response.tool_call.args)
            messages.append({"role": "tool", "content": result})
            break  # 成功了，退出重试
        except Exception as e:
            error_msg = f"工具 {tool_name} 执行失败：{str(e)}"
            messages.append({"role": "tool", "content": error_msg})
            # 继续循环，让模型重新选择工具
```

### 4.2 错误信息要结构化

```
❌ "出错了"
✅ "查询天气工具调用失败：城市参数 'Beijng' 拼写错误，请重新输入"
   # 模型可以根据错误信息自己修正
```

---

## 五、Agent 循环控制

### 5.1 必须设最大迭代次数

```python
MAX_ITERATIONS = 10  # 防无限循环

for i in range(MAX_ITERATIONS):
    # Agent 逻辑
    if should_stop(response):
        break
```

### 5.2 终止条件

```
模型决定不再调用工具 → 正常结束
达到最大迭代次数     → 强制结束，返回当前结果
工具连续报错 N 次    → 结束循环，返回"无法完成"
```

---

## 六、Session 复用

```python
# 多轮对话/Agent 循环用 Session 复用连接
session = requests.Session()
session.headers.update(headers)

for i in range(MAX_ITERATIONS):
    response = session.post(url, json=data)  # 复用连接
```

Session 解决的问题不是流式，是**复用 TCP 连接**，多轮调用时节省重复建连的开销。

---

## 七、安全规范

```
① API Key 永不在代码里硬编码 → .env 文件
② 工具执行结果要校验 → 防止模型返回恶意参数
③ 设置 max_tokens 上限 → 防无限生成
④ 设置迭代次数上限 → 防无限循环
⑤ 日志不要打印 API Key → 防泄露
```
