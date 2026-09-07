# MCP（模型上下文协议）

#工程应用 #mcp #协议

## 一句话
AI 领域的 USB-C：一套标准化协议，让 LLM 与外部工具/数据源统一通信。一次开发，到处复用。

## 关键点
- **解决什么问题**：M 个 LLM × N 个工具 = M×N 种适配。MCP 把它变成 M + N。
- **底层协议**：基于 JSON-RPC 2.0
- **四大能力**：
  - **Resources**：只读数据（文件、数据库）
  - **Tools**：可执行动作（API 调用、脚本）
  - **Prompts**：预设指令模板
  - **Sampling**：Server 反向请求 LLM（进阶）
- **四层架构**：Host → Client（1:N）→ Server → Data Source
- **传输方式**：
  - stdio：本地进程间，轻量
  - Streamable HTTP（2025.3）：生产推荐，单端点独立鉴权

## MCP vs Function Calling vs Agent
三者是**层级关系**，不是替代关系：
- **Function Calling**：LLM 推理层——把意图映射成结构化调用（最低层）
- **MCP**：应用层通信协议——定义工具怎么接入（中间层）
- **Agent**：更高层系统概念——规划、记忆、工具调用全包（最高层）

## 降维映射
- MCP = USB-C 接口标准。不管你插的是鼠标还是硬盘，协议层统一，驱动层各管各的。
- Function Calling = 你的大脑决定「我要拿起杯子」。MCP = 神经系统把信号传到手臂的标准通路。Agent = 包含大脑+神经+肌肉的完整人体。

## 参见
- [[AI Agent]] — MCP 是 Agent 调用外部工具的标准化协议
- [[Function Calling]] — LLM 推理层的结构化调用能力
- [[RAG]] — MCP 的 Resources 能力可用于检索外部知识

## 参考文献
- Anthropic. (2024). *Model Context Protocol (MCP) Specification*. https://modelcontextprotocol.io
- OpenAI. (2023). *Function Calling Documentation*. OpenAI API Docs.
