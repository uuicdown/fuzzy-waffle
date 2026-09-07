# HTTP 请求结构

## 是什么

一次 API 调用 = 发送一个 HTTP POST 请求。

## 三个组成部分

```python
url = "https://api.deepseek.com/chat/completions"  # ① 地址

headers = {                                          # ② 请求头
    "Content-Type": "application/json",              # 告诉服务器发的是 JSON
    "Authorization": "Bearer sk-xxx"                 # 身份认证
}

data = {                                             # ③ 请求体（核心）
    "model": "deepseek-chat",
    "messages": [...],
    "stream": False,
    "temperature": 0.7
}
```

## 请求体（data）的必填字段

```python
{
    "model": "deepseek-chat",       # 必填：选哪个模型
    "messages": [                   # 必填：对话内容
        {"role": "system", "content": "..."},
        {"role": "user", "content": "..."}
    ]
}
```

其他都是可选参数（temperature / stream / max_tokens / response_format）。
