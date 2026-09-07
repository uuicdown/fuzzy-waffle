# Session 复用

## 是什么

使用 `requests.Session()` 复用 TCP 连接，多轮调用时减少重复建连的开销。

## 代码

```python
session = requests.Session()
session.headers.update(headers)  # 设置一次，后续请求自动带

# 第 1 次调用（建立连接）
response1 = session.post(url, json=data1)

# 第 2 次调用（复用连接，不用重新建）
response2 = session.post(url, json=data2)

# 第 3 次调用（继续复用）
response3 = session.post(url, json=data3)
```

## 什么时候用

```
单次调用 → requests.post() 就够了
多轮对话 → 可以用 Session
Agent 循环 → 推荐用 Session
```

## 和流式输出的关系

```
Session 不解决流式问题
流式输出用 requests.post(stream=True) 就行
Session 解决的是"多次调用时复用连接"，和流式无关
```
