---
tags:
  - 开发
  - agent
---
# Agent 循环

## 一句话

**Agent 循环 = 聊天循环 + 工具调用分支。**

```
聊天循环：
  while True:
      收到用户输入
      调 API
      显示 AI 回复
      回到开头

Agent 循环：
  for _ in range(5):
      调 API
      if finish_reason == "stop":
          显示 AI 回复 → 结束
      elif finish_reason == "tool_calls":
          执行工具 → 把结果发回去 → 继续
```

## 代码结构

```python
def run_agent(user_input: str) -> str:
    messages = [system_prompt, user_input]

    for _ in range(5):                    # 防死循环
        result = call_llm(messages)        # 调 API
        msg = result["message"]
        reason = result["finish_reason"]

        if reason == "stop":
            return msg["content"]          # 结束

        elif reason == "tool_calls":
            tool_result = execute_tool(...)
            messages.append(msg)           # 工具调用信息
            messages.append(tool_result)   # 工具返回结果
            # → 下一轮循环，LLM 拿到工具结果继续推理
```

## 核心理解

- Agent 循环就是对 `finish_reason` 的 **二路分支**
- 调一次 API 可能不够——Agent 循环就是**反复调直到 LLM 觉得够了**
- 工具结果放回 messages 里，LLM 就能"看到"自己调了什么工具、返回了什么
