# 结构化输出（强制 JSON）

## 是什么

用 `response_format` 参数让模型必须输出合法 JSON，程序直接解析。

## 代码

```python
data = {
    "model": "deepseek-chat",
    "messages": [
        {"role": "system", "content": "用JSON格式输出姓名和年龄"},
        {"role": "user", "content": "张三今年25岁"}
    ],
    "stream": False,
    "response_format": {"type": "json_object"}  # 关键参数
}

response = requests.post(url, headers=headers, json=data)
result = response.json()
content = result["choices"][0]["message"]["content"]

import json
parsed = json.loads(content)  # 转成 Python 字典
print(parsed["姓名"])          # 张三
```

## 规则

```
① stream 必须关（stream 和 response_format 互斥）
② system prompt 必须带 "json" 关键词，否则 400 报错
③ 字段名要写清楚，最多不超过 10 个
```
