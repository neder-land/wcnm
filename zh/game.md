# GAME 类别数据包

这类游戏包完全由不同的游戏实例自行规定，实现了 WCNM 及对 GAME 类别数据包的规定的协议我们称之为 ```WCNM 下级协议```。需要注意的是， WCNM 建议 GAME 类别数据包在满足 WCNM 数据包命名规范的同时，应满足以下结构：

```wcnm
game.<game_id>.xxx
```

其中 <game_id> 为游戏的字符串 ID，应与在 CENTER 类数据包中使用的保持一致。
