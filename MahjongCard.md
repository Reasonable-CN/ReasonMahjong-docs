# MahjongCard

This protocol described how does an integer represent a card and its role in the game. 

In this version, a card is represented in an integer in range \[0x1, 0xfff\], which is \[1, 4095\]. 

We consider the integer is in BIN.

## Basic

[-8:] records the content of the card.

0x01 ~ 0x09 : 1m ~ 9m  
0x11 ~ 0x19 : 1p ~ 9p  
0x21 ~ 0x29 : 1s ~ 9s  
0x31 ~ 0x37 : 1z ~ 7z  
0x40 ~ 0xff : KEEP

[-9] records whether the card is an Akadora. When [-9:] = 0x134, this represents Pei is hanutta.

## River

When card is in river:

[-10] records whether the card is used to announce Riichi. 

[-11] records whether the card is Tsumo-kiru.

## Calculate

When card is used to calculate:

[-12] records whether the card is by Tsumo. Only available when used as parameter of done-card.

[-13] records whether the card is from Wan mountain.

[-14] records whether the card is used to Pukan.



