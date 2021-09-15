# Sonicgraphy Fallout

A hacker named Blue_Blur was recently arrested and is accused of hiding some evidence. The evidence was reported to have been hidden in the hacker's OC Sonic comic. See if you can find any hidden files. Reportedly, it was a 'video' of some sort. On the other hand, the comics are pretty good. Enjoy! :)

Credit goes to Deebs/Fini-mun for the artwork of the comics.

Author: Andrew Prajogi

[Fallout_-_New_Mobius.zip](https://drive.google.com/file/d/18mw0sgQNNSCgWHhrk9iI3Zfq-k9jBwEp/view?usp=sharing)

## Initial Research

We are given a zip file with 40 files, all pictures. all .jpg or .png. one of them has a hidden video.

![image](https://user-images.githubusercontent.com/62060867/133386590-b8ff3ffb-b7cc-42dc-8e33-83b3b78a9281.png)

I use ```pngcheck``` all of .png to see if there is anything suspicious from the file and almost everything is fine but except:

```
Page 7.png  additional data after IEND chunk
ERROR: Page 7.png
page7.txt  this is neither a PNG or JNG image nor a MNG stream
ERROR: page7.txt
```

and 
```
Buzz Bomber Fight 2.png  this is neither a PNG or JNG image nor a MNG stream
ERROR: Buzz Bomber Fight 2.png
```

## Check file Buzz Bomber Fight 2.png
I use `xxd` to see hex dump of file 

```
┌──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius]
└─$ xxd Buzz\ Bomber\ Fight\ 2.png | less

0000000: 5249 4646 8a4d 0200 5745 4250 5650 3858  RIFF.M..WEBPVP8X
00000010: 0a00 0000 1600 0000 f301 00e5 0100 414e  ..............AN
00000020: 494d 0600 0000 0000 0000 0000 414e 4d46  IM..........ANMF
00000030: 18ed 0100 0000 0000 0000 f301 00e5 0100  ................
```


It's just a .RIFF file (RIFF is Raster Image Files - Painter Raster Image, in Binary format developed by Corel. Image saved by Corel Painter, a digital art studio used to create digital paintings; contains raster graphics divided on one or more layers; allows painted graphics to be separate from the canvas so that artists can customize the canvas later.
)

![Buzz Bomber Fight 2](https://user-images.githubusercontent.com/62060867/133388358-301d3b8c-ef7d-4d21-bd50-fdf7cd0b0179.png)

## Check page7.png

I use `zsteg` to look data after IEND chunk

```┌──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius]
└─$ zsteg Page\ 7.png                                                                                           1 ⚙
[?] 4320060 bytes of extra data after image end (IEND), offset = 0x158d89
extradata:0         .. file: ISO Media, MP4 v2 [ISO 14496-14]
    00000000: 00 00 00 1c 66 74 79 70  6d 70 34 32 00 00 00 01  |....ftypmp42....|
    00000010: 69 73 6f 6d 6d 70 34 31  6d 70 34 32 00 00 0a 8a  |isommp41mp42....|
    00000020: 6d 6f 6f 76 00 00 00 6c  6d 76 68 64 00 00 00 00  |moov...lmvhd....|
    00000030: dd 44 da c9 dd 44 da ce  00 00 02 58 00 00 0b b8  |.D...D.....X....|
    00000040: 00 01 00 00 01 00 00 00  00 00 00 00 00 00 00 00  |................|
    00000050: 00 01 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
    *
    00000070: 40 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |@...............|
    00000080: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 02  |................|
    00000090: 00 00 0a 16 74 72 61 6b  00 00 00 5c 74 6b 68 64  |....trak...\tkhd|
    000000a0: 00 00 00 01 dd 44 da c9  dd 44 da ce 00 00 00 01  |.....D...D......|
    000000b0: 00 00 00 00 00 00 0b b8  00 00 00 00 00 00 00 00  |................|
    000000c0: 00 00 00 00 00 00 00 00  00 01 00 00 00 00 00 00  |................|
    *
    000000e0: 00 00 00 00 00 00 00 00  40 00 00 00 02 d0 00 00  |........@.......|
    000000f0: 05 00 00 00 00 00 00 24  65 64 74 73 00 00 00 1c  |.......$edts....|
b2,r,lsb,xy         .. text: "mA0J]nQs"
b2,g,msb,xy         .. text: "QqlMg0$P#"
b2,rgba,lsb,xy      .. text: "{;;;;KoC3"
b4,r,lsb,xy         .. text: "\"333\"DC!"
b4,rgb,lsb,xy       .. text: "1##4cF4cE$REFdgEc4\"!"
b4,bgr,lsb,xy       .. text: "!3#d6Cd5CT%BfGdeD3\"!"
b4,rgba,lsb,xy      .. text: "1/2?4o4o4o4_$_$_FoF"
                                                  
```

We see an `.mp4` file in Page 7 at `offset 0x158d89` and `size 4320060`

So We can cut it with `Hxd` or use `foremost` to extract file `.mp4`
```
┌──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius]
└─$ foremost Page\ 7.png                                                                                        1 ⚙
Processing: Page 7.png
|*|
```
```
──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius]
└─$ cd output                                                                                                   1 ⚙
                                                                                                                    
┌──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius/output]
└─$ ls                                                                                                          1 ⚙
audit.txt  mov  mp4  png
                                                                                                                    
┌──(kali㉿kali)-[~/Desktop/Fallout_-_New_Mobius/Fallout - New Mobius/output]
└─$ cd mp4                                                                                                      1 ⚙
                                                                                                                    
┌──(kali㉿kali)-[~/…/Fallout_-_New_Mobius/Fallout - New Mobius/output/mp4]
└─$ ls                                                                                                          1 ⚙
00002758.mp4


```

https://user-images.githubusercontent.com/62060867/133390125-f1d90466-593b-4a0c-99a5-97436241dfe5.mp4

SO we got the flag







