# Agent + MCP 集成

## 一句话

**Agent 通过 MCP 自动发现工具并执行，不再手写 tools JSON。**

## 对比

```
旧版 Agent（agent_loop.py）：
  tools = [...]            ← 手写 JSON schema
  execute_tool(name, args) ← 手写每个工具的执行逻辑

MCP 版 Agent（agent_mcp.py）：
  session.list_tools()     ← MCP 自动发现
  session.call_tool(...)   ← MCP 自动执行
```

## MCP 自动转 tools JSON

MCP Server 暴露的工具自带 `name`、`description`、`inputSchema`，直接映射：

```python
{
    "type": "function",
    "function": {
        "name": t.name,              # 来自 @mcp.tool() 的函数名
        "description": t.description, # 来自文档字符串
        "parameters": t.inputSchema   # 来自类型注解，自动生成 JSON Schema
    }
}
```

## 支持多工具并行调用

LLM 可能会一次性返回多个 `tool_calls`（比如"查天气再算数"），Agent 必须全部响应：

```python
for tc in msg["tool_calls"]:          # 遍历所有工具调用
    result = await session.call_tool(...)
    messages.append({"role": "tool", ...})  # 每个都加 tool 消息
```

**少一个 tool 响应，API 就报错。**
