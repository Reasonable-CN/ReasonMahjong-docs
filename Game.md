# Game

该文件定义了对局对象, 由该对象控制对局流程

## 基类
```python
class Game():
    room: Room # 对局所属房间对象
    ju: int # 0, 1, 2, 3 分别代表该局游戏场风为 东, 南, 西, 北
    chang: int # 代表该局游戏为 m局 (如 东 m 局, 其中 m 为变量 chang 所存储的内容)
    riichi_n: int # 目前桌面上存在几根立直棒
    honba_n: int # 本场数, 同时也为场供数
    public: dict # 所有人均可看到的内容, 如 牌河, 副露, 立直状态, 含有 "0", "1", "2", "3" 四个 Key, 分别代表 0, 1, 2, 3 号位置上的玩家的状态 (位置由 Room 对象定义)
    tehai: dict # 手牌内容, 含有 "0", "1", "2", "3" 四个 Key, 分别代表 0, 1, 2, 3 号位置上的玩家的手牌 (位置由 Room 对象定义)
    temohai: dict # 摸牌内容, 含有 "0", "1", "2", "3" 四个 Key, 分别代表 0, 1, 2, 3 号位置上的玩家的摸牌 (位置由 Room 对象定义), 无摸牌时为 None
    yama: list # 牌山
    left: int # 剩余牌数, 为 0 时荒牌流局
    dora: list # 宝牌指示牌
    kanzu_num: list # 当前场上杠子数量
    rinshan_num: list # 当前场上剩余的岭上牌数量
    uradora: list # 里宝牌指示牌
    show_btns: dict # 客户端显示的按钮
    points: list # 各家点数, 含有 "0", "1", "2", "3" 四个 Key, 分别代表 0, 1, 2, 3 号位置上的玩家的点数 (位置由 Room 对象定义)
    settings: dict # 牌局设置, 由 Room 对象传入
    async def tick(self): # tick, while True循环
        pass
    async def start(self): # 游戏初始化, 启动 tick
        pass
```

## 基础日麻
基础的日麻对局对象
```python
class NormalGame(Game):
    pass
```

## 基础自由麻将
基础的自由麻将对象
```python
class ZiYouGame(Game):
    pass
```
