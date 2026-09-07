# 写一个 MCP Server

## 代码

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP(name="我的工具箱")

@mcp.tool()
def get_weather(city: str) -> str:
    """查询指定城市的天气"""
    weather_data = {"北京": "晴天，22°C", "上海": "多云，25°C"}
    return weather_data.get(city, f"暂无{city}的天气数据")

@mcp.tool()
def calculate(expression: str) -> str:
    """做数学计算，支持加减乘除"""
    allowed = "0123456789+-*/(). "
    if not all(c in allowed for c in expression):
        return "错误：表达式包含非法字符"
    try:
        result = eval(expression)
        return f"计算结果：{result}"
    except Exception as e:
        return f"计算错误：{str(e)}"

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

## 和之前手写 tools JSON 的对比

| | 手写 tools JSON | MCP Server |
|---|---|---|
| 注册方式 | 手写 JSON schema | `@mcp.tool()` 装饰器 |
| 参数描述 | 写 properties/required | 函数参数 + 类型注解 |
| 工具描述 | 写 description 字段 | 函数文档字符串 |
| 发现工具 | 代码里写死 | Client 自动查询 |

**MCP 不需要你手动写 JSON——从函数签名+文档字符串自动生成。**

## 关联阅读
- [[MCP 概念]] — MCP 的基本概念
- [[MCP Client]] — 怎么连接并调用 MCP Server
