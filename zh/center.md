# CENTER 类别数据包

> TODO: 完成包 Content 的规范。似乎有一些必须的包缺失，例如对玩家/房间状态变化的推送

## 1. 类型总览

### 客户端

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

### 服务端

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

## 2. Content 规范

### ```center.list.types```

```json
{
    "action": "center.list.types",
    "data": {
        "_comment:" "TO BE DONE"
    },
    "time": 0
}
```

### ```center.list.rooms```

```json
{
    "action": "center.list.rooms",
    "data": {
        "rooms": [
            {
                "name": "TrashCan", // 房间名称
                "owner": "switefaster", // 房主 id
                "people": 3, // 房间人数
                "in_game": true // 房间是否在游戏中
            }
        ]
    },
    "time": 0
}
```