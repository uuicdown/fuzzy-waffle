# 环境变量与 API Key 管理

## 是什么

API Key 相当于你的身份密码，每次 API 调用都要带上。

## 规范做法

项目目录下创建 `.env` 文件：

```
DEEPSEEK_API_KEY=sk-你的密钥
```

代码里读取：

```python
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv("DEEPSEEK_API_KEY")
```

## 踩坑记录

```
❌ DEEPSEEK_API_KEY = "sk-xxx"  等号两边有空格，值带引号
✅ DEEPSEEK_API_KEY=sk-xxx      无空格，无引号

❌ print(api_key)  // 调试完记得删，防泄露
❌ 提交到 Git     // 把 .env 写进 .gitignore
```

## 为什么不能直接写代码里

```
api_key = "sk-xxx"  ← 多人协作时 key 会泄露
                    ← 不小心提交到 Git 就公开了
```
