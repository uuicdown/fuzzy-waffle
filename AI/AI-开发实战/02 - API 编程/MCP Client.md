# MCP Client：连接并调用工具

## 代码

```python
import asyncio
from pathlib import Path
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def main():
    # 指定要连接的 MCP Server
    server_path = Path(__file__).parent / "mcp_server.py"
    server_params = StdioServerParameters(command="python", args=[str(server_path)])

    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            # ① 自动发现工具有什么（不用手写）
            tools = await session.list_tools()
            print("可用工具：")
            for tool in tools.tools:
                print(f"  - {tool.name}: {tool.description}")

            # ② 调用工具
            result = await session.call_tool("get_weather", arguments={"city": "北京"})
            print(result.content[0].text)

asyncio.run(main())
```

## 和之前的对比

```python
# 以前（手写 tools JSON）
tools = [{...}]  # 要自己写 JSON schema
data["tools"] = tools
# 调用时自己判断 finish_reason，自己解析参数

# 现在（MCP Client）
session.list_tools()     # 自动发现
session.call_tool(...)   # 自动调用，自动管理参数
```

## 三种输出

```
mcp_client.py 跑完输出：
  可用工具：
    - get_weather: 查询指定城市的天气
    - calculate: 做数学计算，支持加减乘除
  天气查询：晴天，22°C
  计算：计算结果：66
```

## 关联阅读
- [[MCP 概念]] — MCP 的基本概念
- [[MCP Server]] — 写一个 MCP Server 暴露工具
- [[工具注册]] — 对比 MCP 自动注册和手写 JSON 的区别
- [[finish_reason判断]] — MCP Client 帮你封装了 finish_reason 的判断
- [[Agent 循环]]
