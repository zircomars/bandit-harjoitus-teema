# level 11

OHJE JA VIHJE: The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions & komennot: grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd



```
PS C:\> ssh bandit11@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit11@bandit.labs.overthewire.org's password:

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit11@bandit:~$
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ whoami
bandit11

```

Tästä pitäisi saada jotenkin seuraavasti pienkirjaimet (a-z) ja isot kirjaimet (A-Z) kirjaimia on kierretty 13 pykälällä

Ajattelin pientä testiä, mutta se on varmasti liittyen `tr` komennon kanssa tekemisessä
```
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
bandit11@bandit:~$ cat data.txt | tr [a-z] [AZ]
G]] ]]]]]Z]] ]] 7]16JA]UV]5L]V]J]]SV]ZZ]AHG]]9D4
bandit11@bandit:~$ cat data.txt | tr [a-z] [AZ] [13]
tr: extra operand ‘[13]’
Try 'tr --help' for more information.
bandit11@bandit:~$ cat data.txt | tr [a-z] [AZ] [:13:]
tr: extra operand ‘[:13:]’
Try 'tr --help' for more information.
bandit11@bandit:~$ cat data.txt | tr -c [a-z] [AZ]
]ur]cnffjbeq]vf]]k]]]]r]]v]]x]u]fs]]dbbta]]lw]]]]bandit11@bandit:~$ cat data.txt | tr -t [a-z] [AZ]
Gur ]nffjZeq vf 7k16JArUVv5LxVuJfsSVdZZtAHGlw9D4

```

virallinen vastaus, miksi näin ja huomoina näissä on eroa:
- toinen on väärin/epäkelpo, toinen oikea ROT13-dekoodaus.
```
bandit11@bandit:~$ cat data.txt | tr -t [a-z] [AZ]
Gur ]nffjZeq vf 7k16JArUVv5LxVuJfsSVdZZtAHGlw9D4

bandit11@bandit:~$ cat data.txt | tr -t 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```


## tr komento

- `tr`-komennosta ja Caesar/ROT13-tyylisestä muunnoksesta

```
bandit11@bandit:~$ tr --help
Usage: tr [OPTION]... STRING1 [STRING2]
Translate, squeeze, and/or delete characters from standard input,
writing to standard output.  STRING1 and STRING2 specify arrays of
characters ARRAY1 and ARRAY2 that control the action.

  -c, -C, --complement    use the complement of ARRAY1
  -d, --delete            delete characters in ARRAY1, do not translate
  -s, --squeeze-repeats   replace each sequence of a repeated character
                            that is listed in the last specified ARRAY,
                            with a single occurrence of that character
  -t, --truncate-set1     first truncate ARRAY1 to length of ARRAY2
      --help        display this help and exit
      --version     output version information and exit

ARRAYs are specified as strings of characters.  Most represent themselves.
Interpreted sequences are:

  \NNN            character with octal value NNN (1 to 3 octal digits)
  \\              backslash
  \a              audible BEL
  \b              backspace
  \f              form feed
  \n              new line
  \r              return
  \t              horizontal tab
  \v              vertical tab
  CHAR1-CHAR2     all characters from CHAR1 to CHAR2 in ascending order
  [CHAR*]         in ARRAY2, copies of CHAR until length of ARRAY1
  [CHAR*REPEAT]   REPEAT copies of CHAR, REPEAT octal if starting with 0
  [:alnum:]       all letters and digits
  [:alpha:]       all letters
  [:blank:]       all horizontal whitespace
  [:cntrl:]       all control characters
  [:digit:]       all digits
  [:graph:]       all printable characters, not including space
  [:lower:]       all lower case letters
  [:print:]       all printable characters, including space
  [:punct:]       all punctuation characters
  [:space:]       all horizontal or vertical whitespace
  [:upper:]       all upper case letters
  [:xdigit:]      all hexadecimal digits
  [=CHAR=]        all characters which are equivalent to CHAR

Translation occurs if -d is not given and both STRING1 and STRING2 appear.
-t is only significant when translating.  ARRAY2 is extended to length of
ARRAY1 by repeating its last character as necessary.  Excess characters
of ARRAY2 are ignored.  Character classes expand in unspecified order;
while translating, [:lower:] and [:upper:] may be used in pairs to
specify case conversion.  Squeezing occurs after translation or deletion.

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Full documentation <https://www.gnu.org/software/coreutils/tr>
or available locally via: info '(coreutils) tr invocation'
```


# level 12

VIHJE JA OHJE: The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!) & KOMENNOT: grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

```
PS C:\> ssh bandit12@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit12@bandit.labs.overthewire.org's password:

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ whoami
bandit12
```

Salasana on "data.txt" alla, että pitäisi käyttää _"hexdump"_ koska tiedosto on pakattu. Pitäisi hyödyntää tiedoston polkua `_/tmp_` ja luoda tiedosto komennolla `mkdir` - vaikeasti arvattakseen hakemiston nimikettä. 



```
bandit12@bandit:~$ cat data.txt
00000000: 1f8b 0808 00da cf69 0203 6461 7461 322e  .......i..data2.
00000010: 6269 6e00 0136 02c9 fd42 5a68 3931 4159  bin..6...BZh91AY
00000020: 2653 5978 ae89 f600 001c 7fff db7d bfef  &SYx.........}..
00000030: 8ff7 f7ff ffc2 ffcd 7cbd 2ee4 dff9 aff3  ........|.......
00000040: ef7b d577 e9f7 adbd dfbb fbff b001 3b30  .{.w..........;0
00000050: 63a0 6806 8326 400d 0031 0000 00d1 a313  c.h..&@..1......
00000060: 2000 001a 034d 1a19 0000 0032 0681 89a6   ....M.....2....
00000070: 9a32 0da9 934c 6847 9220 0034 0346 9a00  .2...LhG. .4.F..
00000080: d340 3462 0006 d4d3 d41a 064d 0308 6868  .@4b.......M..hh
....
00000230: 5274 43e0 0054 86d7 cce2 5933 7dd3 1e95  RtC..T....Y3}...
00000240: 4fb8 4103 fe2e e48a 70a1 20f1 5d13 ecd3  O.A.....p. .]...
00000250: 2afd bb36 0200 00                        *..6...
bandit12@bandit:~$ hd -b data.txt
0000000 060 060 060 060 060 060 060 060 072 040 061 146 070 142 040 060
0000010 070 060 070 040 060 060 144 141 040 143 146 066 071 040 060 062
0000020 060 063 040 066 064 066 061 040 067 064 066 061 040 063 062 062
....
00009f0 040 040 040 040 040 040 040 040 040 040 040 040 040 040 040 040
0000a00 040 040 040 040 040 040 040 052 056 056 066 056 056 056 012
0000a0f
bandit12@bandit:~$ hd -c data.txt
0000000   0   0   0   0   0   0   0   0   :       1   f   8   b       0
0000010   8   0   8       0   0   d   a       c   f   6   9       0   2
0000020   0   3       6   4   6   1       7   4   6   1       3   2   2
0000030   e           .   .   .   .   .   .   .   i   .   .   d   a   t
00009d0   .   .   .  \n   0   0   0   0   0   2   5   0   :       2   a
00009e0   f   d       b   b   3   6       0   2   0   0       0   0
00009f0
0000a00                               *   .   .   6   .   .   .  \n
0000a0f
```

aikalailla linux näköinen komento tämä *hexdump* komento.


## hexdump

`hexdump` on Linux/Unix-komento, jolla voit katsoa tiedoston sisältöä raakana binäärimuodossa. Se näyttää datan yleensä:

- heksadesimaalisina arvoina (0–9, a–f)
- ja usein rinnalla ASCII-merkkeinä

Koska suurin osa tiedostoista ei ole pelkkää tekstiä → lopputulos näyttää helposti “koodisateelta” = Matrix-fiilis.

Miksi se näyttää “Matrixilta”:
- näet raakadataa ilman tulkintaa
- mukana on myös ei-tulostettavia merkkejä
- heksaluvut + satunnaiset symbolit → visuaalisesti kaaos

Mihin tätä käytetään oikeasti?
- debuggaus (miksi tiedosto ei toimi)
- tietoturva (analysoidaan binäärejä / payloadia)
- verkko- ja protokolladata
- tiedostojen rakenteen tutkiminen


```
bandit12@bandit:~$ hd --help

Usage:
 hd [options] <file>...

Display file contents in hexadecimal, decimal, octal, or ascii.

Options:
 -b, --one-byte-octal      one-byte octal display
 -c, --one-byte-char       one-byte character display
 -C, --canonical           canonical hex+ASCII display
 -d, --two-bytes-decimal   two-byte decimal display
 -o, --two-bytes-octal     two-byte octal display
 -x, --two-bytes-hex       two-byte hexadecimal display
 -L, --color[=<mode>]      interpret color formatting specifiers
                             colors are enabled by default
 -e, --format <format>     format string to be used for displaying data
 -f, --format-file <file>  file that contains format strings
 -n, --length <length>     interpret only length bytes of input
 -s, --skip <offset>       skip offset bytes from the beginning
 -v, --no-squeezing        output identical lines

 -h, --help                display this help
 -V, --version             display version

Arguments:
 <length> and <offset> arguments may be followed by the suffixes for
   GiB, TiB, PiB, EiB, ZiB, and YiB (the "iB" is optional)

For more details see hexdump(1).
```

## mitä tässä tapahtuu & ohje

Tämä taso on _näyttää kiertelyltä_ mutta kyseessä on systemaattisesta purkamisesta kerros ja ideana on **tunnistaa tiedostomuoto -> nimeä oikein -> purkaa -> toista**.

Annettii ensin tiedosto (data.txt), joka ei ole suoraan luettava data vaan ja kuin on monta kerrosta: 
  - **heksadumppi → pakattu → pakattu → arkisto → pakattu → … → lopullinen teksti**

## Tarkennus 

Tässä kokonaisuus tuntuu kuin kierettiin jännästi, sekä vähä kuin **hexump > xxd -r >> gxip >> gz >> bzip2 >> bz2 >> gzip >> tar >> bzip2 >> tar >> gzip ** 

- Tässä alkaa iso välivaihe - START HERE; 
- Ohjeiden mukaanluodaan tiedosto `/tmp` - polkuun ja tarvittaessa luoda kansionsa kommennolla `$mktemp -d`. Kopsaa **data.txt** tiedosto ja muutta se nimike.


1. Luodaan ja siirretään väliaikainen hakemisto & kopioidaan tiedosto siihen kansion alle
- eli tästä kopioita toi `data.txt`ja siitä muutettu nimi (`mv`) siksi lukee **hexdump_data** - väliaikainen hakemisto
- Tästä tiedosto näkee on heksamuotoinen

```
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ cd /tmp
bandit12@bandit:/tmp$ ls
ls: cannot open directory '.': Permission denied
bandit12@bandit:/tmp$ mktemp -d
/tmp/tmp.HW0UKoiThV
bandit12@bandit:/tmp$ cd /tmp/tmp.HW0UKoiThV
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cp ~/data.txt .
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
data.txt
```

2. tiedosto hakemisto on heksamuotoinen
  - jälkimmäisessä `cat`komenosta tuo sen virallisen datan, mutta sitä dataa ei voi kuitenkaan lukea
```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv data.txt hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cat hexdump_data | head
00000000: 1f8b 0808 00da cf69 0203 6461 7461 322e  .......i..data2.
00000010: 6269 6e00 0136 02c9 fd42 5a68 3931 4159  bin..6...BZh91AY
00000020: 2653 5978 ae89 f600 001c 7fff db7d bfef  &SYx.........}..
00000030: 8ff7 f7ff ffc2 ffcd 7cbd 2ee4 dff9 aff3  ........|.......
00000040: ef7b d577 e9f7 adbd dfbb fbff b001 3b30  .{.w..........;0
00000050: 63a0 6806 8326 400d 0031 0000 00d1 a313  c.h..&@..1......
00000060: 2000 001a 034d 1a19 0000 0032 0681 89a6   ....M.....2....
00000070: 9a32 0da9 934c 6847 9220 0034 0346 9a00  .2...LhG. .4.F..
00000080: d340 3462 0006 d4d3 d41a 064d 0308 6868  .@4b.......M..hh
00000090: 0340 1a19 3264 321b 50f5 00c8 01ea 0d00  .@..2d2.P.......
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
```

3. heksadumppi takaisin binääriksi
  - Nyt tässä alettaan pientä operaatiota, muutettaan **heksadumppi takaisin binääriksi**.
  - Nyt tästä alkaa toistuva purkaminen 3. eteenpäin. Nyt pitää selvitää aikaisemman **heksadumppi-tiedoston** kautta ensimmäisenä tavuista (byte) löydetääkseen tiedoston allekirjoitusta. 
  - Etsitään ensimmäisen tavusta tiedoston allekirjoitusta luettelosta.
  - GZIP pakettujen tiedosto otsikko on toi ensimäinen rivi eli `00000000: 1f8b 0808 00da cf69 0203 6461 7461 322e  .......i..data2.` - joten ensimmäinen tavu (byte) tiedostosta.
```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ xxd -r hexdump_data compressed_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cat compressed_data | head
��LhG� 4F��@4b���␦hh@␦2d2B�.#1ŀp��������V1V_/�٩��|LK'ZC��>�����sW}Z0T$�V/␦���Zx�\;m�����-~8��=�G�P#?:����};�5�p.��
                                                                                                                  8�<���9ڷ�D�;ܣ�fP *?;�U-Bo�"z�,��b��ŭ�D�����o�+��<��@�8{C      ɠ�:��
                             �␦>����Z4��x_ҩ:W�T5�v���"!���V�h�h%9&��`��yb�0������h�c�� ��x�Cf�%IҸh_+x��(;8��n,a���Q�t����8�2e�(��/*�c)
              ��RtC�T����Y3}��O�A�.�p� �]��*��6bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cat hexdump_data
00000000: 1f8b 0808 00da cf69 0203 6461 7461 322e  .......i..data2.
00000010: 6269 6e00 0136 02c9 fd42 5a68 3931 4159  bin..6...BZh91AY
00000020: 2653 5978 ae89 f600 001c 7fff db7d bfef  &SYx.........}..
00000030: 8ff7 f7ff ffc2 ffcd 7cbd 2ee4 dff9 aff3  ........|.......
00000040: ef7b d577 e9f7 adbd dfbb fbff b001 3b30  .{.w..........;0
00000050: 63a0 6806 8326 400d 0031 0000 00d1 a313  c.h..&@..1......
00000060: 2000 001a 034d 1a19 0000 0032 0681 89a6   ....M.....2....
00000070: 9a32 0da9 934c 6847 9220 0034 0346 9a00  .2...LhG. .4.F..
00000080: d340 3462 0006 d4d3 d41a 064d 0308 6868  .@4b.......M..hh
00000090: 0340 1a19 3264 321b 50f5 00c8 01ea 0d00  .@..2d2.P.......
000000a0: 7a80 d00d 030e 9ea1 a3d2 6231 0346 20c4  z.........b1.F .
000000b0: c801 a068 c200 d188 687a 9a00 0340 0000  ...h....hz...@..
000000c0: 0034 0d34 d034 c8c2 34c8 0c9a 3400 0013  .4.4.4..4...4...
000000d0: 6006 9600 30e0 6902 600b 990b ffd0 6e9e  `...0.i.`.....n.
000000e0: a8fc 1813 42c5 2e19 2331 c580 7009 1956  ....B...#1..p..V
000000f0: 3156 0e5f 2ff4 d9a9 12f3 f97c 4c4b 0127  1V._/......|LK.'
00000100: 5a43 cdd9 3ec3 c4e5 eedb 7357 7d5a 3054  ZC..>.....sW}Z0T
00000110: 1d24 7fc5 562f 1a86 8ac8 5a19 7890 125c  .$..V/....Z.x..\
00000120: 103b 6d80 fbf1 fdec 2d7e 1838 e1ed 3dae  .;m.....-~.8..=.
00000130: 47d8 5023 073f 3a1b 2c10 6784 9c90 b67d  G.P#.?:.,.g....}
00000140: 3ba5 3515 9a10 7002 2eca c60c 38c7 3ccd  ;.5...p.....8.<.
00000150: e3fd 7af3 a647 086c 5e7f 5bd0 d526 128f  ..z..G.l^.[..&..
00000160: ace4 2bd0 d744 e9cf 5182 8886 4a26 a938  ..+..D..Q...J&.8
00000170: f860 c668 c5ad 8444 98e4 cbd8 ca6f c22b  .`.h...D.....o.+
00000180: afa2 3cdc c540 03b2 387b 4309 c9a0 913a  ..<..@..8{C....:
00000190: a9c5 0d39 dab7 9544 b83b dca3 9466 5020  ...9...D.;...fP
000001a0: 2a3f 3bc8 552d 426f e422 7a99 2cd0 ed62  *?;.U-Bo."z.,..b
000001b0: ac90 0bac 1a3e fbb2 a3e3 5a34 a0c8 785f  .....>....Z4..x_
000001c0: d2a9 3a57 a554 35a8 76df 18da f622 21f6  ..:W.T5.v...."!.
000001d0: b3f0 56ab 68ad 6825 3926 9491 60a1 f979  ..V.h.h%9&..`..y
000001e0: 6203 ad30 0482 1684 d9f3 c0cf 68ac 63b9  b..0........h.c.
000001f0: ed20 ea11 7fb6 78ad 4366 f218 2549 d2b8  . ....x.Cf..%I..
00000200: 0268 5f2b 0678 85de 283b 3807 9aba 6e2c  .h_+.x..(;8...n,
00000210: 1061 04c0 0007 85f4 511e e974 bcc6 cfc8  .a......Q..t....
00000220: 3889 3265 ff28 c2f9 2f2a d163 290b bb90  8.2e.(../*.c)...
00000230: 5274 43e0 0054 86d7 cce2 5933 7dd3 1e95  RtC..T....Y3}...
00000240: 4fb8 4103 fe2e e48a 70a1 20f1 5d13 ecd3  O.A.....p. .]...
00000250: 2afd bb36 0200 00                        *..6...
```

5. Puretaan GZIP
- **GZIP osuus**
   - tässä muutettaan `compressed_data` tiedosto uudelle nimikeellä ja sen purkamaminen komennolla: `gzip -d`
   - ohjeessa mukaan `gzip`-komento tapaus tiedostoista uudelleenimeäminen oikealla päättellä on välttämätöntä muiden komentojen osalta, mutta sitä ei tarvitse tehdä.

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv compressed_data compressed_data.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.gz  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ gzip -d compressed_data.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data  hexdump_data
```

6. Puretaan BZIP2
- **BZIP osuus**
  - Data on vielä kesken, eli ei ole purettu, joten tarkistamiseen ensimmäisen rivin tavua uudelleen.
  - Nyt tässä on erilainen luku (ylempi ensimmäisestä rivin tavusta)
  - luettu vähä vastauksien ohjetta, että mukaan ensimmäisen rivin `425a` heksadesimaali käännettynä binääriksi on `BZ` ja tarkistettu googlen mukaankin se pitää paikkaansa. Se kertoo on bzip-tiedoston allekirjoitus ja seuraava kaksi tavua on **h** on `68` mikä osoittaa versionsa, että tässä tapauksesa se on versio 2. Jotekin nimeämisessä tiedostoon pitä muuttaa (.bz2) ja purkkaa sen komennolla `bzip2 -d`

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ xxd compressed_data
00000000: 425a 6839 3141 5926 5359 78ae 89f6 0000  BZh91AY&SYx.....
00000010: 1c7f ffdb 7dbf ef8f f7f7 ffff c2ff cd7c  ....}..........|
00000020: bd2e e4df f9af f3ef 7bd5 77e9 f7ad bddf  ........{.w.....
00000030: bbfb ffb0 013b 3063 a068 0683 2640 0d00  .....;0c.h..&@..
00000040: 3100 0000 d1a3 1320 0000 1a03 4d1a 1900  1...... ....M...
00000050: 0000 3206 8189 a69a 320d a993 4c68 4792  ..2.....2...LhG.
00000060: 2000 3403 469a 00d3 4034 6200 06d4 d3d4   .4.F...@4b.....
00000070: 1a06 4d03 0868 6803 401a 1932 6432 1b50  ..M..hh.@..2d2.P
00000080: f500 c801 ea0d 007a 80d0 0d03 0e9e a1a3  .......z........
00000090: d262 3103 4620 c4c8 01a0 68c2 00d1 8868  .b1.F ....h....h
000000a0: 7a9a 0003 4000 0000 340d 34d0 34c8 c234  z...@...4.4.4..4
000000b0: c80c 9a34 0000 1360 0696 0030 e069 0260  ...4...`...0.i.`
000000c0: 0b99 0bff d06e 9ea8 fc18 1342 c52e 1923  .....n.....B...#
000000d0: 31c5 8070 0919 5631 560e 5f2f f4d9 a912  1..p..V1V._/....
000000e0: f3f9 7c4c 4b01 275a 43cd d93e c3c4 e5ee  ..|LK.'ZC..>....
000000f0: db73 577d 5a30 541d 247f c556 2f1a 868a  .sW}Z0T.$..V/...
00000100: c85a 1978 9012 5c10 3b6d 80fb f1fd ec2d  .Z.x..\.;m.....-
00000110: 7e18 38e1 ed3d ae47 d850 2307 3f3a 1b2c  ~.8..=.G.P#.?:.,
00000120: 1067 849c 90b6 7d3b a535 159a 1070 022e  .g....};.5...p..
00000130: cac6 0c38 c73c cde3 fd7a f3a6 4708 6c5e  ...8.<...z..G.l^
00000140: 7f5b d0d5 2612 8fac e42b d0d7 44e9 cf51  .[..&....+..D..Q
00000150: 8288 864a 26a9 38f8 60c6 68c5 ad84 4498  ...J&.8.`.h...D.
00000160: e4cb d8ca 6fc2 2baf a23c dcc5 4003 b238  ....o.+..<..@..8
00000170: 7b43 09c9 a091 3aa9 c50d 39da b795 44b8  {C....:...9...D.
00000180: 3bdc a394 6650 202a 3f3b c855 2d42 6fe4  ;...fP *?;.U-Bo.
00000190: 227a 992c d0ed 62ac 900b ac1a 3efb b2a3  "z.,..b.....>...
000001a0: e35a 34a0 c878 5fd2 a93a 57a5 5435 a876  .Z4..x_..:W.T5.v
000001b0: df18 daf6 2221 f6b3 f056 ab68 ad68 2539  ...."!...V.h.h%9
000001c0: 2694 9160 a1f9 7962 03ad 3004 8216 84d9  &..`..yb..0.....
000001d0: f3c0 cf68 ac63 b9ed 20ea 117f b678 ad43  ...h.c.. ....x.C
000001e0: 66f2 1825 49d2 b802 685f 2b06 7885 de28  f..%I...h_+.x..(
000001f0: 3b38 079a ba6e 2c10 6104 c000 0785 f451  ;8...n,.a......Q
00000200: 1ee9 74bc c6cf c838 8932 65ff 28c2 f92f  ..t....8.2e.(../
00000210: 2ad1 6329 0bbb 9052 7443 e000 5486 d7cc  *.c)...RtC..T...
00000220: e259 337d d31e 954f b841 03fe 2ee4 8a70  .Y3}...O.A.....p
00000230: a120 f15d 13ec                           . .]..
```

7. Puretaan GZIP2 uudelleen
  - tässä tapahtui siirto siis muutettu tiedoston pääte
  - KOSKIEN 6. vaihdetta - nimeämisessä tiedostoon pitää muuttaa (.bz2) ja purkkaa sen komennolla `bzip2 -d`

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv compressed_data compressed_data.bz2
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.bz2  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ bzip2 -d compressed_data.bz2
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
```

8. Puretaan GZIP uudelleen 
  - tästä tiedoston on edelleen pakattu. Komennolla `xxd` - näyttää, että se on pakattu uudelleen `gzip`-muodossa. Nyt toistettaan edellisensa vaiheet nimeämmisen tiedoston kautta ja purettaan se
  - **tar archieve** - `cat compressed_data` tai `xxd compressed_data | head` komennosta (`head` tarkoittaa tulostaakseen päällimäiset/ensimmäiset 10 riviä). Tätä voi käyttää ja nähdä merkkijononsa `data5.bin`, joka on tiedoston nimi.

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv compressed_data compressed_data.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ gzip -d compressed_data.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cat compressed_data
data5.bin0000644000000000000000000002400015163755000011242 0ustar  rootrootdata6.bin000064400000000000000000000003311516���Ah�@���P��M5�*��x(���E']� )@�� AAY&SY�VI]����j@�}�� [#�t!�@f �
�}�X�a��/�VY~障�*��R�J�o��{�^�$'��W&�D� �I�9BY
                                              �Ԁ�j����U%JQ�`��rE8P��VI]bandit12@bandit:/tmp/tmp.HW0UKoiThV$

bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ xxd compressed_data | head
00000000: 6461 7461 352e 6269 6e00 0000 0000 0000  data5.bin.......
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000020: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000030: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000040: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000050: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000060: 0000 0000 3030 3030 3634 3400 3030 3030  ....0000644.0000
00000070: 3030 3000 3030 3030 3030 3000 3030 3030  000.0000000.0000
00000080: 3030 3234 3030 3000 3135 3136 3337 3535  0024000.15163755
00000090: 3030 3000 3031 3132 3432 0020 3000 0000  000.011242. 0...
```

9. Puretaan tar -> data5.bin
  - data5.bin on näyttävän tarkisto (tar archieve) - niin sitä pitää purkkaa **tar-ohjelmiston kautta**.

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv compressed_data compressed_data.tar
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ tar -xf compressed_data.tar
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  hexdump_data
```

10. Puretaan seuraava tar -> data6.bin
  - kun purettaan `data5.bin` arkistonsa - niin avautuu uusi tiedosto nimeltään `data6.bin`, joten purettaan tiedosto uudestaan

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ tar -xf data5.bin
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ xxd data6.bin
00000000: 425a 6839 3141 5926 5359 da56 495d 0000  BZh91AY&SY.VI]..
00000010: 8cff cfdc 6a00 40c0 7dff e120 5b23 8074  ....j.@.}.. [#.t
00000020: 21fe 8000 0840 0000 6682 1108 004c 0820  !....@..f....L.
00000030: 0094 0d42 08c8 d340 0034 6868 6232 64f5  ...B...@.4hhb2d.
00000040: 1a1b 536a 6d41 a291 fa91 a608 3432 6200  ..SjmA......42b.
00000050: 1a68 1a62 0068 372d ce29 fc81 0179 0020  .h.b.h7-.)...y.
00000060: 410d 1eae 1480 d641 68c3 0640 80f9 c150  A......Ah..@...P
00000070: a9a7 4d35 a02a 1b3a d1c2 7828 8fde f445  ..M5.*.:..x(...E
00000080: 1f27 5d8f 1020 2940 9af0 0a95 7ddb 58b0  .'].. )@....}.X.
00000090: 61fa bc2f 9656 597e e99a 9cd3 2a80 c352  a../.VY~....*..R
000000a0: da4a c76f bb87 7b81 5eb6 2427 c180 5726  .J.o..{.^.$'..W&
000000b0: ca44 8220 8149 f339 4259 0b84 d480 896a  .D. .I.9BY.....j
000000c0: 8513 f5fa ee55 1825 4a51 9f60 8080 7f17  .....U.%JQ.`....
000000d0: 7245 3850 90da 5649 5d                   rE8P..VI]
```

11. Puretaan BZIP2 -> data6.bin.out 
  - tässä alussa syötettiin väärä komento, mutta vasta jälkeen syötettiin oikein HUOM. `bzip`ja `bzip2`
  - nyt `data6.bin` on pakattu uudelleen `bzip2` - tiedostojärjestelmällä, koska toistettu `$ bzip2 -d data6.bin`

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ bzip -d data6.bin
Command 'bzip' not found, but there are 21 similar ones.
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin.out  hexdump_data
```

12. Puretaan tar -> data8.bin
  - **tar archieve** - toisen kerran, että tästä `data6.bin.out` - näyttää taas toisen tiedostonnimellä `data8.bin` - joten purettaan se tiedosto

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ tar -xf data6.bin.out
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin.out  data8.bin  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ xxd data8.bin
00000000: 1f8b 0808 00da cf69 0203 6461 7461 392e  .......i..data9.
00000010: 6269 6e00 0bc9 4855 2848 2c2e 2ecf 2f4a  bin...HU(H,.../J
00000020: 51c8 2c56 70f3 374d 2977 2b4e 3648 4e4a  Q.,Vp.7M)w+N6HNJ
00000030: f4cc f430 c8b0 f032 4a0d cd2e 362a 4b09  ...0...2J...6*K.
00000040: 7129 77cc e302 003e de32 4131 0000 00    q)w....>.2A1...
```

13. Puretaan viimeinen GZIP
  - Viimeisenä vielä yksi purku `gzip`-llä ja saadaan luettava muoto oleva tiedoston salasana

```
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ mv data8.bin  data8.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ gzip -d data8.gz
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ ls
compressed_data.tar  data5.bin  data6.bin.out  data8  hexdump_data
bandit12@bandit:/tmp/tmp.HW0UKoiThV$ cat data8
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```


## ohje lunttilappu:

Vastaukset ja tarkennettu linux toiminnasta ja siitä komennosta:

- https://axcheron.github.io/writeups/otw/bandit/#bandit-12-solution
- https://mayadevbe.me/posts/overthewire/bandit/level13/
- https://david-varghese.medium.com/overthewire-bandit-level-12-level-13-2ec761a88907
