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
[```sys.login.cancel```](#syslogincancel) | 取消某个客户端发送的登录请求，即登录失败；目前登录失败唯一的可能是有人使用了已在线账户的昵称尝试登录

## 2. Content 规范

### ```sys.login```

```json5
{
    "action": "sys.login",
    "content": {
        "username": "female_robot" // 用户名称
    }
}
```

### ```sys.logout```

```json5
{
    "action": "sys.logout",
    "content": {}
}
```

### ```sys.disconnect```

```json5
{
    "action": "sys.disconnect",
    "content": {}
}
```

### ```sys.login.cancel```

```json5
{
    "action": "sys.login.cancel",
    "content": {
        "reason": "Username occupied." // 拒绝登陆的理由
    }
}
```
