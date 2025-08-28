# CustomYaku

This file described how to custom your own Yaku in game.

## How did CustomYaku work?

In general, we use UTF-8 to encode Yaku.json and base64 to encode json into long string, which is for transmitting. 

Each .json is a ReasonNamespace. For example, `main.json` is for namespace `main`, and use `gokushimuso` in `main.json` should use main.gokushimuso. 

At initial, ReasonMaj provided 2 default .json, `main.json ` and `furui.json`, to cover almost all yakus in general. You're supposed not to modify these two json files in case of system runtime error unless you're technician and have the ability to recover the original files while knowing what you are doing.

You shouldn't delete `main.json` and `furui.json`. 

Your .json file would be automaticly resolved once you put it into `../config/yaku/`.

## How to write a CustomYaku?

<s>Now you can use YakuManager.exe to add, delete or modify all Yakus loaded. </s>

Belowings are ways in programming.

First, create a .json file and name it without "main" and "furui", also those you've loaded. 

.json should be a dictionary, in format of this:

```json
{
    "YakusInnerName":{
        "ban":1,
        "fu":0,
        "description":"Description.Namespace",
        "condition":{
            ...
        }
    },
    ...
}
```

Each item's key is its inner name, which is for namespace. You should add display name in default.json or corresponding lan.json. 

For keys in value, here are the meanings:
- ban: When reached the Yaku, how many Bans can be added in the cards. Must be positive int and at least 1. If Yakuman, set it into 1000\*N, where N is its times. For example, double-Yakuman should type in 2000. 
- fu: When reached the Yaku, how many Fus should be set. Keep -1 when not using this. Use highest value when reached 2 or more Yaku which has been designated `fu`.
- description: Show on the screen to describe this Yaku. Keeping `None` or `""` means use `yaku.Group.Name.desc`. If designated, force the use of value.

## Keys and their values in condition of Yaku

To write a condition, you should using key-values as follow. 

- under: False when Yaku shouldn't check under the shape of general Ron (4\*3+2). Default True. You cannot make a Yaku with more than 15 tiles and under==False (In other words, you can only design your Yaku without shape of general Ron in exact 14 tiles).
- final: (with !under)List for all possible final shape of the tiles. Each element should be list of length being 14.
- preset: List of Yakus should be reached first as prerequisite. For example, Ippatsu's preset should have riichi or dabururiichi. Each element should be list.
- waiting: (with !under)List for all possible shape before final shape of tiles. Each element should be 
- listen: (with under)List for reaching way. Enum. Can be:
- - r: waiting for double-side;
- - t: waiting for quetou, even 2 or more tiles;
- - d: waiting for double-Pon, uncertain quetou;
- - k: waiting for valley-side;
- - b: waiting for edge-side;
- con: List for tiles must be in hand.
- ex: List for tiles must be not in hand.
- que: List for tiles must be used as quetou. OR in list
- queex: List for tiles must be not used as quetou. AND in list
- menzi: (with under)List for Menzi must be in hand. AND in list
- menziex: (with under)List for Menzi must be not in hand. AND in list
- action: List for actions must be satisfied in the same time as prerequisite. Enum. Can be:
- - "menq": didn't get tile from any other players
- - "riichi": announced riichi
- - "dabururiichi": announced riichi in the first round of a game
- - "tsumo": Ron-out with last tiles got by self
- ronfrom: List for possible ron-out functional tiles. OR in list. Enum. Can be:
- - "rinsyan": from rinsyan or after kan-get
- - "last": from the last tiles of the mountain
- - "pukan": from other's pukan
- - "riichi": from other's Riichi annoucing tile
- single: List for single tiles satisfied. OR in outer list and AND in inner list.
- double: List for doubles satisfied. OR in outer list and AND in inner list.
- kill: List for disabling Yakus when reached. For example, Gokushijuusanmenmachi should "kill" Gokushimusou to avoid calculating it twice so that Trible-yakuman can be wrongly reached.
- qdz: (with double)True when avoid calculating a quadra as two doubles. This is to avoid Chinese mahjong "Long Qi Dui" or "Shuang Long Qi Dui". Default True.
- useDora: True when need know whether the tile is a dora/pei or not. This would use the tile list of [i&0x1ff for i in hand] instead of [i&0xff for i in hand].


For menzi(list), its elements should be all dictinaries. Each elements should be satisfied at the same time. However, one element doesn't mean a single menzi, but at most 4. Key-values are as follows.

- typ: Which type is the menzi. Enum. Can be:
- - ke: triples
- - shun: flows
- - kan: quadras
- shape: The specified tile(s). For `typ=ke` or `typ=kan`, this represents which tile(s) must be present. For `typ=shun`, this represents the first tile of the sequence (thus, values like `0x?8`, `0x?9`, and `0x3?` are not allowed). This is a double-layer list: between lists, the relationship is OR (satisfying any one is sufficient); within a list, the relationship is AND (all tiles must be present simultaneously).
- allowChi: For `typ=shun`, to decide whether chi-ed flows can satisfy.
- allowPon: For `typ=ke`, to decide whether pon-ed triples can satisfy.
- allowAnKan: For `typ=ke`, to decide whether AnKan can satisfy. Default True.
- allowMinKan: For `typ=ke`, to decide whether MinKan can satisfy. Default =allowPon.
- allowPuKan: For `typ=ke`, to decide whether PuKan can satisfy. Default =allowPon.
- allowAn: For `typ=kan`, to decide whether AnKan can satisfy. Default True.
- allowMin: For `typ=kan`, to decide whether MinKan can satisfy. Default True.
- allowPu: For `typ=kan`, to decide whether PuKan can satisfy. Default True.
- times: For `typ=shun`, how many same shun can satisfy. For example, when times=2, `1m1m2m2m3m3m` can satisfy `shape=[["1m"],["1p"],["1s"]]`, but `1m2m3m1p2p3p` cannot.

For menziex(list), most are as same with menzi, but `allow?` cannot use. Instead, here's a new key `range`. A list for searching range of Menzi that cannot be in hand. Enum. Can be:
- inFlow: Flow without Chi
- Chi: Flow with Chi
- AnKou: Triple without Pon
- Pon: Triple with Pon
- AnKan: AnKan
- MinKan: MinKan
- PuKan: PuKan


## Save your length

When typing tiles, some tiles group can use a single int to replace. Here are the list:

0x38: Kaze tiles of Tsukue  
0x39: Kaze tiles of Jibun  
0xa: all Manzi  
0xb: any Manzi  
0x1a: all Pinzi  
0x1b: any Pinzi  
0x2a: all Souzi  
0x3b: any Souzi  
0x3a: all Zu tiles  
0x3b: all Kaze tiles (0x31,0x32,0x33,0x34)  
0x3c: all Sanen tiles (0x35,0x36,0x37)  
0x3d: any Zu tiles  
0x3e: any Kaze tiles  
0x3f: any Sanen tiles  
0xf0: all Yaokyuu tiles (0x?1,0x?9,0x3?)
0xf1: all anti-Yaokyuu tiles (all except 0xf0)
0xf2: all Routou tiles (0x?1,0x?9)
0xfe: all Dora (with list around to show 'or')
0xff: all Ura (with list around to show 'or')
Any: all tiles

## Moral but not legal

When designing a Yakuman-Yaku, you're supposed to write its first letter in capital to show it's a Yakuman-Yaku

