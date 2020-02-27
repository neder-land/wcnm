# SYS 类别数据包

## 1. 类型总览

### 客户端

动作名 | 用途
:- | :-
[```sys.login```](#syslogin) | 控制用户登录；目前版本的游戏中心只需要昵称，不需要密码即可进入
[```sys.logout```](#syslogout) | 控制用户登出
[```sys.disconnect```](#sysdisconnect) | 用于主动断开与服务器的连接

### 服务端

动作名 | 用途
:- | :-
[```sys.login.accept```](#sysloginaccept) | 批准某个客户端发送的登录请求
[```sys.login.cancel```](#syslogincancel) | 取消某个客户端发送的登录请求，即登录失败；目前登录失败唯一的可能是有人使用了已在线账户的昵称尝试登录
[```sys.logout.accept```](#syslogoutaccept) | 批准某个客户端的登出请求
[```sys.disconnect```](#sysdisconnect) | 服务端主动发起断开连接请求；目前仅会在重启服务器时这么做

## 2. Content 规范

### ```sys.login```

```json
{
    "action": "sys.login",
    "data": {
        "username": "female_robot" // 用户名称
    },
    "time": 0
}
```

### ```sys.logout```

```json
{
    "action": "sys.logout",
    "data": {},
    "time": 0
}
```

### ```sys.disconnect```

```json
{
    "action": "sys.disconnect",
    "data": {},
    "time": 0
}
```

### ```sys.login.accept```

```json
{
    "action": "sys.login.accept",
    "data": {},
    "time": 0
}
```

### ```sys.login.cancel```

```json
{
    "action": "sys.login.cancel",
    "data": {
        "reason": "Username occupied." // 拒绝登陆的理由
    },
    "time": 0
}
```

### ```sys.logout.accept```

```json
{
    "action": "sys.logout.accept",
    "data": {},
    "time": 0
}
```
