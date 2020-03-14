# CENTER 类别数据包

## 1. 类型总览

### 客户端

动作名 | 用途
:- | :-
[```center.list.games```](#centerlisttypes) | 获取游戏中心当前提供的游戏 ID 列表
[```center.list.rooms```](#centerlistrooms) | 获取某个游戏种类当前的房间列表
[```center.create```](#centercreate) | 创建某个游戏的新房间
[```center.join```](#centerjoin) | 加入某个游戏的某个房间
[```center.leave```](#centerleave) | 退出当前房间

### 服务端

动作名 | 用途
:- | :-
[```center.list.games.response```](#centerlisttypesresponse) | 对游戏类型请求的响应
[```center.list.rooms.response```](#centerlistroomsresponse) | 对游戏房间请求的回应
[```center.room.change```](#centerroomchange) | 用于告知客户端当前房间的变化，或者没有变化。当接收到客户端发送的```center.create```，```center.join```或```center.leave```时，服务器应当发回此包

## 2. Content 规范

### ```center.list.games```

```json5
{
    "action": "center.list.games",
    "content": {}
}
```

### ```center.list.rooms```

```json5
{
    "action": "center.list.rooms",
    "content": {
        "game_type": "" // 请求的游戏种类（不一定是游戏 ID），空字符串以获得全部房间
    }
}
```

### ```center.create```

```json5
{
    "action": "center.create",
    "content": {
        "room_name": "university", // 要创建的房间的名称
        "game": "nswc" // 要创建的游戏 ID
    }
}
```

### ```center.join```

```json5
{
    "action": "center.join",
    "content": {
        "room_name": "university" // 要加入的游戏房间名
    }
}
```

### ```center.leave```

```json5
{
    "action": "center.leave",
    "content": {}
}
```

### ```center.list.games.response```

```json5
{
    "action": "center.list.games.response",
    "content": {
        "games": [
            "nswc",
            "ngword",
            "sftql"
        ]
    }
}
```

### ```center.list.rooms.response```

```json5
{
    "action": "center.list.rooms.response",
    "content": {
        "rooms": [
            {
                "name": "university", // 房间名称
                "game": "nswc", // 游戏 ID
				"extra": {
					// 房间逻辑实现的扩展数据，可为空
				}
            }
        ]
    }
}
```

### ```center.room.change```

为以下两种格式之一

```json5
{
    "action": "center.room.change",
    "content": {
        "room": {
            "name": "university", // 房间名称
            "game": "nswc", // 游戏 ID
			"extra": {
				// 房间逻辑实现的扩展数据，可为空
			}
        }
    }
}
```

```json5
{
    "action": "center.room.change",
    "content": {
        "room": null // 通知客户端此时已不在房间内
    }
}
```