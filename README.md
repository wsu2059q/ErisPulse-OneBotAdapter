# 模块介绍

## OneBotAdapter
OneBot V11协议适配模块，提供异步的OneBot触发器功能。支持两种运行模式：
- Server模式：作为WebSocket服务器接收OneBot客户端连接
- Connect模式：作为WebSocket客户端连接OneBot服务器

主要功能：
- 支持消息、通知、请求、元事件等事件类型的处理
- 提供触发器注册机制，可灵活扩展事件处理逻辑
- 支持Token验证，确保连接安全性
- 提供消息发送和Action调用接口

## OneBotMessageHandler
OneBot协议消息处理模块，用于处理OneBot的消息事件。

主要功能：
- 提供消息处理器注册机制
- 支持异步消息处理
- 可与其他模块协同工作

## OneBotNoticeHandler  
OneBot协议通知处理模块，用于处理OneBot的通知事件。

主要功能：
- 提供通知处理器注册机制
- 支持异步通知处理
- 完善的错误处理机制

## OneBotRequestHandler
OneBot协议请求处理模块，用于处理OneBot的请求事件。

主要功能：
- 提供请求处理器注册机制
- 支持异步请求处理
- 详细的日志记录

# 使用示例

以下是一个完整的使用示例，展示了如何初始化SDK并添加触发器与处理器：

```python
import asyncio
from ErisPulse import sdk

# 初始化SDK
sdk.init()

# 定义一个简单的通知处理器
async def simple_notice_handler(data):
    print(f"收到通知: {data}")

# 定义一个简单的消息处理器
async def simple_message_handler(data):
    print(f"收到消息: {data}")

# 定义一个简单的请求处理器
async def simple_request_handler(data):
    print(f"收到请求: {data}")

async def main():
    # 添加触发器
    sdk.OneBotAdapter.AddTrigger(sdk.OneBotMessageHandler)  # 注册消息触发器
    sdk.OneBotAdapter.AddTrigger(sdk.OneBotNoticeHandler)   # 注册通知触发器
    sdk.OneBotAdapter.AddTrigger(sdk.OneBotRequestHandler)  # 注册请求触发器

    # 添加具体的处理器
    sdk.OneBotNoticeHandler.AddHandle(simple_notice_handler)   # 注册通知处理函数
    sdk.OneBotMessageHandler.AddHandle(simple_message_handler) # 注册消息处理函数
    sdk.OneBotRequestHandler.AddHandle(simple_request_handler) # 注册请求处理函数

    # 启动OneBot适配器
    await sdk.OneBotAdapter.Run()

# 运行主程序
asyncio.run(main())
```

# 配置说明

在 `env.py` 中进行以下配置：

```python
sdk.env.set("OneBotAdapter", {
    "mode": "connect",  # 或 "server"
    "WSServer": {
        "host": "127.0.0.1",
        "port": 8080,
        "path": "/",
        "token": "your_token"
    },
    "WSConnect": {
        "url": "ws://127.0.0.1:3001/",
        "token": "your_token"
    }
})
```

# AddHandle 方法使用说明

`AddHandle` 方法用于向各个模块中添加具体的事件处理函数。以下是各模块中使用 `AddHandle` 的方式：

- **消息处理模块 (OneBotMessageHandler)**:
  ```python
  sdk.OneBotMessageHandler.AddHandle(your_message_handler_function)
  ```

- **通知处理模块 (OneBotNoticeHandler)**:
  ```python
  sdk.OneBotNoticeHandler.AddHandle(your_notice_handler_function)
  ```

- **请求处理模块 (OneBotRequestHandler)**:
  ```python
  sdk.OneBotRequestHandler.AddHandle(your_request_handler_function)
  ```

请确保在调用 `Run()` 前完成所有处理器的注册工作。

# 参考链接

- [ErisPulse 主库](https://github.com/ErisPulse/ErisPulse/)
- [ErisPulse 模块开发指南](https://github.com/ErisPulse/ErisPulse/tree/main/docs/DEVELOPMENT.md)
