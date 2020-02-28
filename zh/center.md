# CENTER 类别数据包

## 1. 类型总览

### 客户端

动作名 | 用途
:- | :-
[```center.list.types```](#centerlisttypes) | 获取游戏中心当前提供的游戏列表
[```center.list.rooms```](#centerlistrooms) | 获取某个游戏当前的房间列表
[```center.create```](#centercreate) | 创建某个游戏的新房间；执行的账户不得在一个已有的房间内；创建后该账户即为房主
[```center.join```](#centerjoin) | 加入某个游戏的某个房间
[```center.user_action```](#centeruseraction) | 用户对离开房间/准备/取消准备的请求
[```center.room.operation```](#centerroomoperation) | 由房主或游戏逻辑发出，对房间进行管理
[```center.room.user_operation```](#centerroomuseroperation) | 由房主或游戏逻辑发出，对房间成员进行管理

### 服务端

动作名 | 用途
:- | :-
[```center.list.types.response```](#centerlisttypesresponse) | 对游戏类型请求的响应
[```center.list.rooms.response```](#centerlistroomsresponse) | 对游戏房间请求的回应
[```center.create.success```](#centercreatesuccess) | 用于告知客户端新建房间成功；客户端应当在收到此消息后自动设置自身状态为已经在某个房间内
[```center.create.fail```](#centercreatefail) | 用于告知客户端新建房间失败
[```center.join.success```](#centerjoinsuccess) | 用于告知客户端加入房间成功；客户端应当在收到此消息后自动设置自身状态为已经在某个房间内；服务端反馈此信息时，同时还会告知客户端房间的具体信息，包括房间中的成员信息与房主信息
[```center.join.fail```](#centerjoinfail) | 用于告知客户端加入房间失败
[```center.leave.fail```](#centerleavefail) | 用于告知客户端离开房间失败；出现这种情况，既有可能是因为用户尚未在某个房间内，也有可能是因为用户本身为该房间的房主
[```center.break.fail```](#centerbreakfail) | 用于告知客户端解散房间失败
[```center.room.status_change```](#centerroomstatuschange) | 用于告知客户端房间状态的变化
[```center.room.user_status_change```](#centerroomuserstatuschange) | 告知客户端房间成员信息的变化

## 2. Content 规范

### ```center.list.types```

```json5
{
    "action": "center.list.types",
    "content": {},
    "time": 0
}
```

### ```center.list.rooms```

```json5
{
    "action": "center.list.rooms",
    "content": {
        "game": "" // 请求的游戏类型，空字符串以获得全部房间
    },
    "time": 0
}
```

### ```center.create```

```json5
{
    "action": "center.create",
    "content": {
        "room_name": "hell_s_fire", // 要创建的房间的名称
        "game": "nswc" // 要创建的游戏类型
    },
    "time": 0
}
```

### ```center.join```

```json5
{
    "action": "center.join",
    "content": {
        "room_name": "TrashCan" // 要加入的游戏房间名
    },
    "time": 0
}
```

### ```center.user_action```

```json5
{
    "action": "center.user_action",
    "content": {
        "intend_to": "leave", // 用户希望执行的操作，为 [leave,ready,unready] 三者中任意一个，分别代表离开房间，准备与取消准备
    },
    "time": 0
}
```

### ```center.room.room_operation```

```json5
{
    "action": "center.room.room_operation",
    "content": {
        "intend_to": "start", // 要对房间进行的操作，为 [start, break] 二者其一，分别代表开始游戏与解散房间
    },
    "time": 0
}
```

### ```center.room.user_operation```

```json5
{
    "action": "center.room.user_operation",
    "content": {
        "intend_to": "kick", // 要对用户进行的操作，为 [kick,mute,set_master] 三者其一，分布表示踢出，禁言与转让房主
        "target": "switefaster"
    },
    "time": 0
}
```

### ```center.list.types.response```

```json5
{
    "action": "center.list.types.response",
    "content": {
        "games": [
            "nswc",
            "ngword",
            "zftql"
        ]
    },
    "time": 0
}
```

### ```center.list.rooms.response```

```json5
{
    "action": "center.list.rooms.response",
    "content": {
        "rooms": [
            {
                "name": "TrashCan", // 房间名称
                "owner": "switefaster", // 房主 id
                "people": 3, // 房间人数
                "in_game": true, // 房间是否在游戏中
                "game": "nswc"
            }
        ]
    },
    "time": 0
}
```

### ```center.create.success```

```json5
{
    "action": "center.create.success",
    "content": {
        "room": "hell_s_fire", // 创建成功的游戏房间名称，主要是为了方便客户端处理
        "game": "nswc" // 创建成功的游戏类型
    },
    "time": 0
}
```

### ```center.create.fail```

```json5
{
    "action": "center.create.fail",
    "content": {
        "reason": "Name occupied" // 创建失败的理由
    },
    "time": 0
}
```

### ```center.join.success```

```json5
{
    "action": "center.join.success",
    "content": {
        "room": {
            "name": "TrashCan", // 房间名称
            "owner": "switefaster", // 房主 id
            "people": [
                "switefaster",
                "ZhengFourth",
                "InitAuther97",
                "Yaossg",
                "Nikkeru"
            ], // 房间人员，顺序并无规定
            "in_game": false, // 房间是否在游戏中
            "game": "nswc"
        }
    },
    "time": 0
}
```

### ```center.join.fail```

```json5
{
    "action": "center.join.fail",
    "content": {
        "reason": "You are too strong to join this room." // 加入失败的原因
    },
    "time": 0
}
```

### ```center.leave.fail```

```json5
{
    "action": "center.leave.fail",
    "content": {
        "reason": "You are not in a room." // 离开失败的理由
    },
    "time": 0
}
```

### ```center.break.fail```

```json5
{
    "action": "center.break.fail",
    "content": {
        "reason": "Permission denied!" // 解散失败的理由
    },
    "time": 0
}
```

### ```center.room.status_change```

```json5
{
    "action": "center.room.status_change",
    "content": {
        "event": "start", // 状态变化的具体内容，为 [start,break] 二者其一，分别表示开始于解散
    },
    "time": 0
}
```

### ```center.room.user_status_change```

```json5
{
    "action": "center.room.user_status_change",
    "content": {
        "what": "ready", // 用户状态改变的具体内容，为 [ready,unready,kick,mute,set_master,join,leave] 七者其一，分布表示准备/取消准备/踢出/禁言/转让房主/加入/离开
        "who": "Yaossg" // 状态改变的用户
    },
    "time": 0
}
```
