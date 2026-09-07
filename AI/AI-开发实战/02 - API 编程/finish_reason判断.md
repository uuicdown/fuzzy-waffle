# finish_reason 判断

## 是什么

模型每次返回时，`finish_reason` 告诉你它下一步想干什么。

## 三种情况

```python
result = response.json()
reason = result["choices"][0]["finish_reason"]
msg = result["choices"][0]["message"]

if reason == "stop":
    # 模型说完了，直接取 content
    print(msg["content"])

elif reason == "tool_calls":
    # 模型要调工具，取 tool_calls
    tool_call = msg["tool_calls"][0]
    name = tool_call["function"]["name"]
    args = json.loads(tool_call["function"]["arguments"])
    # → 执行工具 → 结果喂回 → 再调一次 API

elif reason == "length":
    # 回答被 max_tokens 截断了
    print("回答不完整，需要继续生成")
```

## 为什么需要循环

```
第 1 轮：finish_reason = "tool_calls"  → 调工具
第 2 轮：finish_reason = "stop"        → 出结果
```

一次 API 调用不够用。**Agent 循环就是靠反复检查 finish_reason 来决定继续还是停止。**

## 关联阅读
- [[工具注册]] — 注册完工具后，模型返回 tool_calls
- [[工具执行]] — 拿到 tool_calls 后执行代码
- [[流式输出]] — 流式输出时 finish_reason 在最后一行传过来
