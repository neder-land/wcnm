# WCNM 协议标准(草稿)

> Version 0.1.0
> Created at 2.27.2020
> Author langyo(langyo.china@gmail.com)

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

### 2. 数据包类型命名

客户端所发送的数据包的类型分为以下几种：

- 系统消息，开头为```sys.```，规定了最底层的一些操作。

动作名 | 用途
:- | :-
```sys.login``` | 控制用户登录；目前版本的游戏中心只需要昵称，不需要密码即可进入
```sys.logout``` | 控制用户登出
```sys.disconnect``` | 用于主动断开与服务器的连接

- 游戏中心控制消息，开头为```center.```，规定了与外围游戏维护设施的一些操作。

动作名 | 用途
:- | :-
```center.list.types``` | 获取游戏中心当前提供的游戏列表
```center.list.rooms``` | 获取某个游戏当前的房间列表
```center.list.accounts``` | 获取当前服务器在线的账户列表；这仅需客户端执行一次，后续服务端会不断推送新上线或离线的账户动态
```center.create``` | 创建某个游戏的新房间；执行的账户不得在一个已有的房间内；创建后该账户即为房主
```center.join``` | 加入某个游戏的某个房间
```center.leave``` | 离开当前账户所在的房间；如果不在某个房间中，则该数据包无效；房主无法执行
```center.room.start``` | 启动当前房间的游戏；如果有其他玩家尚未准备，该数据包无效
```center.room.ready``` | 设置当前账户的状态为准备；房主无法使用
```center.room.unready``` | 设置当前账户的状态为未准备；房主无法使用
```center.room.break``` | 解散当前房间；只有房主才能使用
```center.room.set.master``` | 设置当前房间的房主；只有房主才能使用，否则数据包无效
```center.room.kick``` | 设置将某个账户踢出房间；只有房主才能使用，只能在准备阶段使用
```center.room.ban``` | 设置将某个在房间的账户设置为禁言；只有房主才能使用，只能在准备阶段使用

- 通讯消息，以```msg.```开头，支撑玩家间最基础的通讯：

动作名 | 用途
:- | :-
```msg.global``` | 用于向整个服务器发送消息；服务端可以限制此操作的频率，例如在若干秒之内不得第二次发送消息，且不得复读
```msg.room``` | 在房间内尚在准备阶段时发送消息；当游戏已经开始进行时，如果该游戏需要使用通讯的内容，则任何该类型的数据包均会被转发到游戏处理逻辑，而不是直接转发到其它在线的客户端
```msg.secret``` | 用于私聊某个账户；在游戏已经开始进行时，同在一个房间内的两个账户之间无法私聊

- 游戏处理逻辑，以```game.<游戏包名>.```开头，由具体游戏自行定制。

服务端所发送的数据包的类型分为以下几种：

- 系统消息，以```sys.```开头，规定了最底层的一些操作：

动作名 | 用途
:- | :-
```sys.login.accept``` | 批准某个客户端发送的登录请求
```sys.login.cancel``` | 取消某个客户端发送的登录请求，即登录失败；目前登录失败唯一的可能是有人使用了已在线账户的昵称尝试登录
```sys.logout.accept``` | 批准某个客户端的登出请求
```sys.disconnect``` | 服务端主动发起断开连接请求；目前仅会在重启服务器时这么做

- 游戏中心控制消息，以```center.```开头，用于提供基本的游戏与房间管制。

动作名 | 用途
:- | :-
```center.create.success``` | 用于告知客户端新建房间成功；客户端应当在收到此消息后自动设置自身状态为已经在某个房间内
```center.create.fail``` | 用于告知客户端新建房间失败
```center.join.success``` | 用于告知客户端加入房间成功；客户端应当在收到此消息后自动设置自身状态为已经在某个房间内；服务端反馈此信息时，同时还会告知客户端房间的具体信息，包括房间中的成员信息与房主信息
```center.join.fail``` | 用于告知客户端加入房间失败
```center.leave.success``` | 用于告知客户端离开房间成功
```center.leave.fail``` | 用于告知客户端离开房间失败；出现这种情况，既有可能是因为用户尚未在某个房间内，也有可能是因为用户本身为该房间的房主
```center.break.success``` | 用于告知客户端解散房间成功；只有房主才能收到这种反馈
```center.break.fail``` | 用于告知客户端解散房间失败
```center.room.start``` | 告知客户端游戏已开始
```center.room.ready``` | 告知客户端有某个在房间中的账户已准备；如果客户端主动发送了同名动作，服务端应当同样进行反馈，以下不再重复
```center.room.unready``` | 告知客户端有某个在房间中的账户已取消准备
```center.room.break``` | 告知客户端其所在房间已被解散
```center.room.set.master``` | 告知客户端房间更改了房主
```center.room.kick``` | 告知客户端某个用户被踢出房间
```center.room.ban``` | 告知客户端某个用户被禁言；这个消息也可能是由游戏逻辑发出的
```center.room.join``` | 告知客户端有新用户加入房间
```center.room.leave``` | 告知客户端有用户离开房间

- 通讯消息，以```msg.```开头，支撑玩家间最基础的通讯：

动作名 | 用途
:- | :-
```msg.global``` | 向客户端推送全服聊天信息；客户端自己发送的同名动作，服务端也应对此进行反馈，以下不再重复
```msg.room``` | 向客户端推送房间聊天信息；该动作也可能由游戏逻辑触发
```msg.secret``` | 向客户端推送某个账户对其的私聊信息

- 游戏处理逻辑，以```game.<游戏包名>.```开头，也由具体游戏自行定制。

## 3. 行为

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

### 4. 聊天

聊天分为全局频道与房间频道两种。全局频道可用于跨房间聊天，但仅此而已；房间频道不仅可用于房间内用户之间的交流，它同样是相当一部分文字游戏的基础。

房间频道在游戏未开始前与普通的聊天室没有区别，但在开始游戏后便交给游戏逻辑手动控制了－－游戏逻辑可以视情况选择不将某个客户端发来的消息转发给其它客户端，可以向其它客户端发送所谓的系统消息（本质上是昵称有些变化）。

聊天的内容目前只支持纯文字，但支持彩色字符与所有的 Unicode 字符－－这意味着游戏逻辑可以根据需要使用不同色彩的文字做一些特殊的效果，或是玩家可以发送 Unicode 范围内的 Emoji 字符。