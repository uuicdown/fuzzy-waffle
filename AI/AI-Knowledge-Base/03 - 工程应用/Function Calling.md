# 函数调用（Function Calling）

> ✅ 已学习 — 2026-05-18
>
> Function Calling 让模型从"只能输出文字"变成"可以调用工具"。
> 模型决定"该用什么工具、传什么参数"，系统执行工具调用，结果再喂回模型。

---

## 一句话本质

**模型不执行工具，模型只决定"该调用哪个工具"。**

```
用户：查一下北京明天的天气

模型内部推理（不展示）：
  "用户想知道天气 → 我有 get_weather 工具 → 参数 city=北京, date=明天 → 调用它"

模型返回给系统（你看不到这个）：
  { function: "get_weather", args: { city: "北京", date: "2026-05-19" } }
  ↑ 不是人话，是一个结构化指令

系统收到后执行：
  → 调天气 API → 拿到数据 → 拼回 messages
  → 再发给模型 → 模型生成最终回答："北京明天晴天，22°C"
```

---

## 工作流程

```
用户提问
  │
  ▼
┌─────────────────────┐
│ ① 模型决定           │  ← 模型看用户问题和可用的工具列表
│   "我需要调用工具 X"  │     决定要不要调用工具
│   返回函数名+参数     │
└─────────┬───────────┘
          │ { function: "get_weather", args: {...} }
          ▼
┌─────────────────────┐
│ ② 系统执行工具       │  ← API/Agent 平台执行实际调用
│   拿到结果           │     查询数据库、调外部 API、读文件……
└─────────┬───────────┘
          │ 结果: "北京明天晴天，22°C"
          ▼
┌─────────────────────┐
│ ③ 结果喂回模型       │  ← 把工具结果拼进 messages
│   模型生成最终回答    │     再次发给模型
└─────────┬───────────┘
          │ "北京明天晴天，22°C，建议带伞。"
          ▼
        用户看到回答
```

---

## 工具注册：模型怎么知道有哪些工具可用

不是模型"天生知道"可以调用什么——是你（或 Agent 配置）在每次请求时告诉它的：

```json
{
  "model": "deepseek-chat",
  "messages": [...],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "查询某城市某日天气",
        "parameters": {
          "type": "object",
          "properties": {
            "city": {"type": "string", "description": "城市名"},
            "date": {"type": "string", "description": "日期"}
          },
          "required": ["city", "date"]
        }
      }
    }
  ]
}
```

**关键：** 每次调用都要把工具列表传过去。模型看这个列表才知道"我今天能调什么工具"——所以你在 Agent 配置里"添加工具"时，背后就是生成了这个 JSON 结构。

---

## Function Calling vs MCP

| | Function Calling | MCP |
|---|---|---|
| **本质** | 一种 API 参数机制 | 一个标准化协议 |
| **做什么** | 让模型决定调哪个工具 | 统一工具的描述格式和通信方式 |
| **谁定义** | 各模型厂商各自实现 | Anthropic 发起，社区标准 |
| **关系** | MCP **基于** Function Calling 的思路 | Function Calling 的"标准化升级版" |

你的 Agent 配置里可能同时用到这两个：**Function Calling 是底层机制，MCP 是工具的描述协议。**

---

## 核心特性

| 特性 | 说明 |
|---|---|
| **模型不执行** | 模型只返回函数名和参数，执行由系统完成 |
| **工具列表需注册** | 每次调用都要传可用工具的描述 |
| **可并行调用** | 一次回复可以调多个工具（比如同时查天气+日程） |
| **循环调用** | 一次调用结果可以触发下一次工具调用（Agent 循环） |

---

## 🧪 摸底自测

#### Q1（入门）
> Function Calling 中，模型到底执行了工具调用吗？它返回的是什么？

<details>
<summary>解答</summary>

模型不执行工具。它只返回一个结构化指令（函数名 + 参数），由系统/API 平台实际执行。模型相当于"决策者"，系统是"执行者"。
</details>

#### Q2（进阶）
> 每个请求都要传工具列表过去才行？为什么？

<details>
<summary>解答</summary>

是的。模型不知道你有什么工具——它只看当前请求里的 tools 参数来决定"能调什么"。Agent 配置里添加工具，背后就是在往 tools 数组里加一条 JSON 描述。
</details>

#### Q3（思考）
> Function Calling 和 MCP 的关系是什么？

<details>
<summary>解答</summary>

Function Calling 是底层机制（模型决定调工具），MCP 是上层协议（让工具描述和通信标准化）。MCP 是 Function Calling 的标准化升级，不是替代。
</details>

---

## 参考文献
- OpenAI. (2023). *Function Calling Documentation*. OpenAI API Docs.
- Anthropic. (2024). *Model Context Protocol (MCP) Specification*. https://modelcontextprotocol.io

*关联阅读：[[MCP（模型上下文协议）]] · [[AI Agent]] · [[API 调用基础与流式输出]]*
