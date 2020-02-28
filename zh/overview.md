# WCNM 协议标准

> Version 0.1.0
> Created at 2.27.2020
> Author langyo(langyo.china@gmail.com)
> switefaster(switefaster@gmail.com)
>
> Credits to all nederland members

## 1. 绪论

WCNM 是专用于尼德兰游戏中心的通讯协议，它提供了必要的基本数据套接字定义。

WCNM 不限实现用的计算机语言，也不限具体的处理算法细节，但需要至少遵循以下规则：

- 使用 WebSocket 协议进行通讯
- 协议以文本流进行通讯，具体内容以 JSON 编码

## 2. 协议基本内容

### 1. 格式

WCNM 协议中，每一份收发的数据包均需要遵循以下的基本格式：

```json
{
    "action": String,      // 数据包类型
    "content": Object   ,     // 数据包具体内容
    "time": Number    // 数据包时间戳，遵循 Unix 扩展标准时（64 位有符号长整数）
}
```

### 2. 数据包命名规范

WCNM 中数据包类型依据数据包的名称区分，即数据包中的 ```action``` 字段。WCNM 规定 action 字段的名称中的字符应属于 ASCII 字符集并且由小写字符与下划线```_```与点```.```构成。其中点将名称中有意义的字段间隔离开，以方便数据包的分类。

WCNM 的将数据包分为以下 3 类以及一个**结构**，本文将稍后对两者的区别做出解释。值得注意的是，数据包分为服务器数据包(由服务器发出)与客户端数据包(由客户端发出)两种，但是 WCNM 协议中不会刻意在名称中表现这种分别。

- [系统消息](sys.md) - 由 ```sys.``` 为开头，主要负责一些系统底层的通讯
- [中心控制](center.md) - 由 ```center.``` 为开头，负责一些对游戏环境的基本控制
- [游戏逻辑](game.md) - 由 ```game.``` 为开头，此部分完全由不同实现 WCNM 的实例实现，我们只提供基础的框架
- [消息结构](msg.md) - 以 ```.msg.``` 为前缀，注意我们并不认为这是一个单独的类别

> ### 包类别与结构
>
> 事实上，在最初对 WCNM 协议的起草中，```msg``` 是作为一个单独的类别存在的。但是 @ZhengFourth 指出有一些游戏(e.g. 你说我猜)其游戏几乎完全依赖于消息的交换。于是 ```game``` 类别和 ```msg``` 类别产生了冲突。为了解决这种问题，我们决定将 ```msg``` 作为一种对包类型的规范，如果某些游戏逻辑中使用到了消息的交换，我们建议使用我们对消息包的规范。因此其并非最高级的 ```action``` 名称，而是作为一个中缀，例如 ```game.nswc.msg.send```

## 3. 基础行为

### 1. 握手阶段（Hand Shake）

客户端为了能和服务器建立有效连接，需要首先与服务器进行协议匹对。握手阶段所发送的数据包是一个固定不变的 JSON 数据包：

```json
{
    "version": String,      // 协议版本；本次协议的版本为 0.1.0
    "time": Number    // 握手时间戳
}
```

握手时间戳是用于抵御垃圾数据包、建立稳定连接的重要凭据－－服务端需要检查是否含有此时间戳键，以及检查此时间戳与当前时间是否相差过大。如果发现没有该键，或是该键所指时间与服务器当前时间相差过大（默认应当为 3 秒），服务器可以拒绝连接。

任何客户端都应当在双方的任意一方主动发起断开连接请求前，只发送一次握手包，第一次之后的一切握手包服务器均应当忽略，直到断开连接。

### 2. 登录

客户端应当第一时间告知服务器自己的使用者的昵称，否则无法加入任何一个房间。要进行登录，需要客户端发送```sys.login```消息。

### 3. 游戏类型与房间

客户端不必要支持服务端所能提供的所有游戏，而仅需实现其中的一两个即可。要创建某个游戏的房间，需要执行```center.room.create```动作，并附带上唯一的游戏 ID。

游戏 ID 本质上是一个字符串，同时具有标识游戏类型与对游戏进行编号的功能。游戏 ID 可以是中文。

每个游戏房间只能有一名房主，房主的权限也仅限于当前房间内，并且可以转让。在游戏过程中，房主没有权限禁言其它在房间内的普通用户，只有游戏实现逻辑可以禁言玩家。