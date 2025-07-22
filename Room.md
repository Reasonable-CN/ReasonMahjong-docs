# Room

该文件定义了对局时的房间

```python
class Room:
    room_id: str # 理论任意字符串, 目前实现为随机 5 为数字
    settings: dict # 房间设置, 可自定义内容
    players: list # 房间内玩家列表
    state: int # 房间内对局状态, 0 为正在等待, 1 为正在对局, 2 为正在结算
    num_index: dict # 包含 "0", "1", "2", "3" 四个 Key, 其 Value 为 0, 1, 2, 3 号位置上的玩家
    player_index: dict # num_index 的反字典
```
