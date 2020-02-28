# MSG 结构

## 1. 类型总览

### 客户端

前缀 | 含义
:- | :-
[```.msg.send```](#msgsend) | 发送一条消息

### 服务端

前缀 | 含义
:- | :-
[```.msg.receive```](#msgreceive) | 服务端收到消息后转发

## 2. Content 规范

### ```.msg.send```

```json5
{
    "action": "xxx.msg.send",
    "content": {
        "scope": "<string>", // 消息的作用域，由不同的实例自行规定,
        "sender": "switefaster", // 消息的发出者
        "message": {} // 消息的正文，我们对此类型不做任何规定，可以是字符串，亦可以是游戏需要的特殊对象
    }
}
```

### ```.msg.receive```

```json5
{
    "action": "xxx.msg.receive",
    "content": {
        "sender": "switefaster", // 消息的发出者
        "message": {} // 消息的正文，我们对此类型不做任何规定，可以是字符串，亦可以是游戏需要的特殊对象
    }
}
```
