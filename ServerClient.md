# Server-Client

This protocol described how do server and client communicate with each other in ReasonMahjong. Use websocket. Each time, client should send a json pack to push a request, and keep alive to receive pack from server to get info. 

Keys start with \* are not nessessary. Values with <> around mean they are description, not a exact value.

## Server -> Client:
---

General communication pack format:
{
act: (enum.int)0x0-0xff

    To show how to this pack.

Table:

| Number | Action |
| :--: | :--: |
| 0x0 | Ask client if is alive |
| 0x1 | Accept and success |
| 0x2 | Someone left |
| 0x3 | Someone Joined |
| 0x4 | Denied |
| 0x5 | Accept, but need waiting |
| 0x6 | Player MSG |
| 0x7 | Invitation of joining room |
| 0x8 | Game start |
| 0x9 | Game end |
| 0xa | Game pause |

}


In-game communication pack format:

{
times: (float)

    To show when was the pack packed. For the time between pack packed and pack sent is too short, this times sometimes can be seen as the time pack was sent.

timestamp: (int)

    To show the sequence number of this pack in the game.  
    This is used to avoid network issues, keep client in the latest. Otherwise, client can request history packs.
    

act: (enum.int)0x0 - 0x113

    To show who acted and what action can do.

    The last 4 bits represent who acted or who can act. Here, why use combined way to represent acting players? This is to handle the situation in special rule when 2 or 3 players can say Ron at the same time.

Table:

| Number | Rp |
| :--: | :--: |
| 0 | Keep for 0x0 |
| 1 | East |
| 2 | South |
| 3 | E & S |
| 4 | West |
| 5 | W & E |
| 6 | W & S |
| 7 | W & S & E |
| 8 | North |
| 9 | N & E |
| a | N & S |
| b | N & S & E |
| c | N & W |
| d | N & W & E |
| e | N & W & S |
| f | N & W & S & E |

Following 8 bits represent what act has been done in server.

For self:

[-5] Whether you can Kan.  
[-6] Whether you can Tsumo.  
[-7] Whether you can Riichi.  
[-8] Whether you can Ryukoku.  

For Others:

[-5] Play if 0 else other actions.  
[-6] Whether you can Chi the card.  
[-7] Whether you can Pon the card.  
[-8] Whether you can Kan the card.  
[-9] Whether you can Ron the card.  
[-12:-10] Actions:  
> 0x0: Tsumo  
> 0x1: AnKan  
> 0x2: PuKan  
> 0x3: Ron  
> 0x4: Chi  
> 0x5: Pon  
> 0x6: Kan  

Table:

| Number | Self | Others |
| :--: | :--: | :--: |
| 0x0 | Get-normal | Play-normal |
| 0x1 | Get-with-Kan | Tsumo |
| 0x2 | Get-with-Tsumo | Play-can-Chi |
| 0x3 | Get-with-Kan-Tsumo | Keep |
| 0x4 | Get-with-Riichi | Play-can-Pon |
| 0x5 | Get-with-Kan-Riichi | Keep |
| 0x6 | Get-with-Tsumo-Riichi | Play-can-Chi-Pon |
| 0x7 | Get-with-Kan-Tsumo-Riichi | Keep |
| 0x8 | Get-with-Ryukoku | Play-can-Kan |
| 0x9 | Get-with-Ryukoku-Kan | Keep |
| 0xa | Keep | Play-can-Chi-Kan |
| 0xc | Get-with-Ryukoku-Riichi | Play-can-Pon-Kan |
| 0xe | Get-with-Ryukoku-Riichi-Tsumo | Play-can-Chi-Pon-Kan |
| 0x10 | Keep | Play-can-Ron |
| 0x12 | Keep | Play-can-Chi-Ron | 
| 0x14 | Keep | Play-can-Pon-Ron | 
| 0x16 | Keep | Play-can-Chi-Pon-Ron | 
| 0x18 | Keep | Play-can-Kan-Ron | 
| 0x1a | Keep | Play-can-Chi-Kan-Ron | 
| 0x1c | Keep | Play-can-Pon-Kan-Ron | 
| 0x1e | Keep | Play-can-Chi-Pon-Kan-Ron |
| 0x21 | Keep | AnKan |
| 0x31 | Keep | AnKan-can-Ron |
| 0x41 | Keep | PuKan |
| 0x51 | Keep | PuKan-can-Ron |
| 0x61 | Keep | Ron |
| 0x81 | Keep | Chi |
| 0xa1 | Keep | Pon |
| 0xc1 | Keep | Kan |


content: (enum.int)0x0 - 0x1000
    To show which card is focused in this pack. Should follow [MahjongCard](./MahjongCard.md).

} 

---

## Clinet-Server
---
When first connected, client should positively send a message to server to show client's user. Pack defined as followed:

{
time: (float)

    To show when was the pack packed. For the time between pack packed and pack sent is too short, this times sometimes can be seen as the time pack was sent.

id: (str)

    The OAuth account ID in ReasonMaj.

password: (str)

    The OAuth password in ReasonMaj.
}

General communication pack format:

{
act: (enum.int)0x0-0xff

    To show what did the pack do or want to do.



}

