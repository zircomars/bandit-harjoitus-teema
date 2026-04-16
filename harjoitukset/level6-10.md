# level 6

VIHJE JA OHJE: The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size


```
PS C:\> ssh bandit6@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit6@bandit.labs.overthewire.org's password:

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

bandit6@bandit:~$ whoami
bandit6
```

eli pitää saada komentona selivtettyä, omistaja on user bandit7, ja ryhmässä bandit6, että oisko tiedosto/kansio tai jossakin serverin alla käytettynä 33 tavua (bytes)

ainakin kokeilin, mutta omalla testauksella ja muutamia denied tai kyseisen tiedosto / kansio ei ole olemassa
```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c
find: ‘/tmp’: Permission denied
find: ‘/etc/credstore.encrypted’: Permission denied
find: ‘/etc/sudoers.d’: Permission denied
find: ‘/etc/stunnel’: Permission denied
find: ‘/etc/multipath’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
find: ‘/etc/polkit-1/rules.d’: Permission denied
find: ‘/etc/credstore’: Permission denied
...
find: ‘/proc/2/fdinfo’: Permission denied
find: ‘/proc/2/ns’: Permission denied
find: ‘/proc/22/task/22/fd/6’: No such file or directory
find: ‘/proc/22/task/22/fdinfo/6’: No such file or directory
find: ‘/proc/22/fd/5’: No such file or directory
find: ‘/proc/22/fdinfo/5’: No such file or directory
find: ‘/manpage/manpage3-pw’: Permission denied
find: ‘/var/crash’: Permission denied
find: ‘/var/tmp’: Permission denied
find: ‘/var/log’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/ubuntu-advantage/apt-esm/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/amazon’: Permission denied
/var/lib/dpkg/info/bandit7.password
find: ‘/var/lib/udisks2’: Permission denied
find: ‘/var/lib/snapd/void’: Permission denied
find: ‘/var/lib/snapd/cookie’: Permission denied
find: ‘/var/lib/polkit-1’: Permission denied
....
find: ‘/var/lib/snapd/void’: Permission denied
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

```

Ylemmästä ainain yksi tulosti infonsa hmm.. mielenkiintoista

Tästä komennosta siis hyvinkin toimi , mutta sekin yksi vaihtoehto ja yleinen tapa:
```
$ find / -user bandit7 -group bandit6 -size 33c
```
Mitä se tekee ja täsmähaku todella tarkasti määritellylle tiedostolle
- `find /` → etsii koko järjestelmästä
`-user bandit7` → tiedoston omistaja on bandit7
`-group bandit6` → ryhmä on bandit6
`-size 33c` → koko on tasan 33 tavua

Toinen vaihtoehto pieni parannus (käytännössä lähes aina) ja mahdolista saada paljon "permission denied" viestejä:
```
$find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Kolmas vaihtoehto, mutta tämä on toisten bloggi ja antama vastaus, että testattu toimii, mutta se suodataa pois "permission denied" ja jää vaan muut:

```
bandit6@bandit:~$ find / -type f -size 33c -group bandit6 -user bandit7 2>&1 | grep -v "Permission denied"
find: ‘/proc/17/task/17/fdinfo/6’: No such file or directory
find: ‘/proc/17/fdinfo/5’: No such file or directory
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

- `find /` = etsi koko järjestelmästä
- `-type f` = vain tiedostot (ei kansiot)
- `-size 33c` = koko tasan 33 tavua
- `-group bandit6` = ryhmä bandit6
- `-user bandit7` = omistaja bandit7
- `2>&1` = ohjaa virheilmoitukset (stderr) samaan kuin normaali tuloste (stdout)
- `| grep -v "Permission denied"` - pipe + grep (suodattaa tekstiä) ja `-v` = kääteinen (poistaa rivit)


```
2>/dev/null = heittää virheet kokonaan pois

2>&1 | grep = näyttää kaiken, mutta suodattaa itse
```




# level 7

OHJE JA VIHJE: The password for the next level is stored in the file data.txt next to the word millionth & komentto vinkkeinä: man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

meidän pitää nyt tietää se salasana, joka just tähän kohdistuva "millionth"


```
PS C:\Users\zhao-> ssh bandit7@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit7@bandit.labs.overthewire.org's password:

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
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ pwd
/home/bandit7
bandit7@bandit:~$ whoami
bandit7
```

pitkä liirum laarum tekstiä.. mutta supistin osan ja ainakin miljoonia tekstiä

```
bandit7@bandit:~$ ls
data.txt
circumstantially        RnwoVfYuXOiwuqZ5oacoiiQ4m5nG6DIm
overkilled      DrsnAXpvYvr7VRrOTrhkLePjn2hLGuEm
polyphonic      eiOokq35YVPJuWTWvRITVy28SBhfWPBr
hustings's      QGpLHzymJu3JG53Ug0o1ciLaB3Lj1xET
...
Castro  eveLea8yGeo0RDmUdsgv3b4FCqCMmIh7
Castro's        iUBJcXtkgYrGP2AUgxkQiSweV7nkkXeL
casts   ioE9GbSwKhC394TZ4r9NHYo6yie35lIg


```

ainakin greppataan - vähä kuin halutaan hakea tämän nimen perusteella ja nämä kaikki voi olla hyvinkin salasana, että viee aika paljon aikaa ja testiä

```
bandit7@bandit:~$ grep bandit data.txt
banditry        gbGPx9kqz2IEp0MJiNr2dhH5aVDZEeVa
bandit's        rIffaST9g92X7abhj5Nr6CO1oxvWuhMR
banditry's      DI6anvPkLa22XKhpH7n0YVLR8vMMjBCo
banditti        RN2XlIbinmWbsfux3VByA9zSY3sfwBl8
bandits ewE3FYJGpzS7hXLDCoCJEKvE0jqUUtuh
bandit  rZRmiJtsrxuGS7RK2sKse4AffmUt5v1n
bandit7@bandit:~$ grep million data.txt
millionaire's   rZvZrqto36EG7ckYnv3shyqqM5gTWzm3
multimillionaires       Hm6LkKeZm6NF8HJTCe7QsMSFZ6h3fdyy
million HEMJEJL8JN4568b92e5WmjE4TVTfAXi4
multimillionaire        mauh8oucyoOQQh0joSaRIjwIQHauWMaK
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
millionaire     xaZGD7owrr71viTX7e7cxmtiYA9Twnkk
multimillionaire's      jbrOyH3BLvdCIbHTAxeDpY94MV7BJlcL
millionaires    8RAU9XbYOpb8ViBNOGuDSzaBB12zUHij
millions        LjP2QiuoxYAznkJHQVKidbAmM13Gp3p7
million's       JNJLKl0LlicSC07th2WXWlrL4i6EmMPd
```

nyt tehty ja tämä on varmaan se salasana ja testaamalla sitten tiedetään, koska vihjeenä ja ohjeena on _"millionth"_
```
bandit7@bandit:~$ grep millionth data.txt
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

tämä on toisten vastaus, mutta aika hyvä pohdinta osuus kuitenkin ja miksi näin pitkä, suodattu ja tarkennettu haettaan just tämä _"millionth"_
```
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ find / -name "data.txt" -exec grep -H 'millionth' {} \; 2>&1 | grep -v "Permission denied"
/home/bandit7/data.txt:millionth        dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```


# level 8


```
PS C: ssh bandit8@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit8@bandit.labs.overthewire.org's password:

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

bandit8@bandit:~$ whoami
bandit8
bandit8@bandit:~$ ls
data.txt


bandit8@bandit:~$ ll
total 56
drwxr-xr-x   2 root    root     4096 Apr  3 15:17 ./
drwxr-xr-x 150 root    root     4096 Apr  3 15:20 ../
-rw-r--r--   1 root    root      220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root    root     3851 Apr  3 15:10 .bashrc
-rw-r-----   1 bandit9 bandit8 33033 Apr  3 15:17 data.txt
-rw-r--r--   1 root    root      807 Mar 31  2024 .profile
bandit8@bandit:~$ ls > data.txt
-bash: data.txt: Operation not permitted
bandit8@bandit:~$ cat data.txt
McfCopsVMkSjH0RczhUpMNz3wj8lByZU
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
uoe0s8oZ9OAwOGB3AnlQHuTSUUueK5XZ
wpslsPAsYxj6MzzHL3EsTAdzhudfmvpE
...
.....
.......
WKQo7hbxjpBBijzifM5Z3FpZIWzU9SFP
rNOYB72WEMqUoieQw6sUbTWXdOFrlep8
DACc77sZL41E2siRkLU5R1zWBBxk3xo4
Wp09U3tVWYwMcRhXzKhL9UY1SuPFRt9i
bandit8@bandit:~$
bandit8@bandit:~$ wc -l data.txt
1001 data.txt
```

mysteerisiä salasanoja joten ei tiedetä mikä noista onkaan ja vie paljon aikaa ja testaamiseen.. ja n. 1001 riviä koodia/salasanaa. Vihje ja ohjeena sanottu se **tapahtuu vain kerran.**


Tämä on pieni yhdistelmä kolme työkalua siis komentoja. 
- `sort` - lajitellaan tiedoston rivit aakkosjärjestyksessä 
- `uniq -c` - tarkoittaa poistaa peräkkäisiä duplikaattirivejä ja c tarkoittaa laskee montako kertaa kukin rivi esintyy esim. `omena omena banaani` niin tulisi 2 omenaa ja 1 banaani
- `grep "1"` - tarkoittaa etsii rivejä, jotka sisältävät tekstin "1" (ykkösen + välilyönti). - eli valitaan vain rivit, jotka esiintyivät tasan kerran.

- Toi `grep "1"` - tarkoittaa missä tahansa kohtaa mm. 1 , 11, 159, 1dasf, 91v - eli etsii oletuksena osumia mistä tahansa riviltä sisältää, ei pelkästään kokonaisia numeroita, ja pätee välilyöntiä ja muita merkkejä mm. 11 omena ja 159 omena.

```
bandit8@bandit:~$ sort data.txt | uniq -c | grep "1 "
      1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

pientä omia testejä että tehdään pieni pohdinta liittyen aikaisempaan komentoonsa , että tulostettuna miksi näin:
eka testi, että ainakin huomataan toi numero 1 toistuu 
```
bandit8@bandit:~$ sort data.txt | grep "1"
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
2e82EBdaXT27y7li2PoUKkD8IqsECsL1
2e82EBdaXT27y7li2PoUKkD8IqsECsL1
2e82EBdaXT27y7li2PoUKkD8IqsECsL1
yGsI74pIPDWbp76NVY1l5JTswPPoELzc
yGsI74pIPDWbp76NVY1l5JTswPPoELzc
YvYRZxaDl1MJlR9IgY8UWO5fjxwF1wIB
YvYRZxaDl1MJlR9IgY8UWO5fjxwF1wIB
YvYRZxaDl1MJlR9IgY8UWO5fjxwF1wIB
```

toinen testi, että ainoastaan tämä toistui kerran eli missä on yksi & näyttää rivit, jotka esiintyvät tasan kerran (count = 1)
```
bandit8@bandit:~$ sort data.txt | uniq -c
bandit8@bandit:~$ sort data.txt | uniq -c
     10 0gGmyPWCYc25YtbwSP3kcNZdu9TFIB6s
     10 0GWwWUcG1DRo7zNiaZXfsSEGaHie3ij0
     10 0o1vydddtEpnKIsb6kFSbYcq0HPh8Mig
     10 0YJPG1dC42w6W80WLWO5FuRPKIlHuKQ3
     10 2e82EBdaXT27y7li2PoUKkD8IqsECsL1
     10 2voSd7kyMS0vZJ4vuQms4WhOKcM2sJUV
     10 3AMfEqrsPrROH3Evcy5s5DCtB72Ahj69
     10 3ifGXW6S8fVx0mfimxwjhnCoB2ddfAbU
     10 3VFt0jFn4RflJS1uEjLtgwR5ZZMiWGHa
      1 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
     10 4mGM8Yu0JIA0MXzlGWu3WFPoMwUvSBp4
     10 4N82O3SBYWXlQmnsFA7sFoj0BQqxQfpF
     10 4O3i4AyA1iBWR8hrL62yPNd09fr4dLYD
     10 4oXVNcmujmCdVGyqMWJ6vDPQ2D8FXVJu
```
# level 9



```
PS C:\> ssh bandit9@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit9@bandit.labs.overthewire.org's password:

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

bandit9@bandit:~$ whoami
bandit9
bandit9@bandit:~$ ls
data.txt


bandit9@bandit:~$ cat data.txt
��g�BՈJ��~�{��.�W�u�[�3����E"H� ��r�B��ۿ�տ �lH����%�=�be��(f��b� ���Y��%�u̡�[�j9جY��֣����נ�5."Y[��/(��4�<쓙�6�;�cQ̵x|�������:,�UdL�;^Ğ�l��,�rAV����yOe�Ǽ\�^0O���׳P�|3�CvB�␦c)/��/���Z�v�V$
4v�;e␦����K�4�|��:�d
c�U(��?�|���[�Mdt�����{jr��ң�`��~��
                       s�ύ�-b��>����LLڼ���Pp��[[�H���GO��Ҝ�`K��~�9�Эc.��雷[��v�fP6���KCb���gA䪝�U)v�]�~�
                                                                                                        j~��Y�okV3�}3���YF
  ��Ty�M�}��2��?c�:~p�ҋ������ErQ��� ��X���f��kZ�ɺ��z4�&L5�[Z�� ^G9ZpY�pN���N���w���ޤ�,��7Lp�B`W>3��o�(�D�$2K�!��P?�~�d��q��1�ĺsIf3y�vY��Sm�^Ԋ�~h
                        ð�����B�6�]RDdK��&^�p��煍��!\xc2�Hpi���i���j
\�5R����u��OЯE~w�I���������`�iqm�'�S�j����*�:и���LϜ��B�Y<�g�=H�"(n� ��2���!��8Ɇ�˱[$�q?:�_�`0����ʆ�;�2�Մ�݇ì��6␦}��D7
�����m�T��������vc�9|�
5���l��9�Q[��T�m<"'7ӭ ��%� [6��\�yp���l�O�о���P+�ŴU�
 wl�z�X9�����Qib~�*k2�����LR��g�Ŵ�ɋ�W����␦���7&�>�'�1�Sc�E��Ϳ�R��P}��F��n�/v-mԀ]�mu�>G�(==��r��]b�������g35{��p"zQ�ˠ�v7�éȪd_�_�dJ�      E�j�7+�:ܾ5��QiR.9+��qr�'qv4
                                          ��Q����C`�&o�h.�S     ,��t�s�?�%���9ƴq,�h(E9cp�Wcnkt��ܯ�,d��PTK�e�h'���L-P�����m��t,�O���w9�$�D�^&a/����wҢ3h��:��ȡHT5�␦��A��*?ޟ�Z6{K'1�fC�JЫ�Ysb��ۀ��TI�6�4�m�K�J'�҄Ùi=
�[��jA�<�8����6����&6g�G$���EP}�S}U�'+��V4N]j.�?�J�x��?���n����m�4!�ڹ�!/��l�]{S��I      �=�R����2P�D�N__��p6�uD|7�L�%ʂ��
bandit9@bandit:~$
```


salaista koodin tekstiä näyttävä ja salasana on data.txt sisällä kuitenkin, vihjeenä sanotaan pitää käytää ja luettavina stringi jossa edeltää useita '='-merkkejä

muita testiä:
- `sort` ja `uniq` - komennosta ainakin toistui data.txt sisällönsä

```
bandit9@bandit:~$ uniq data.txt | grep "="
grep: (standard input): binary file matches
bandit9@bandit:~$ sort data.txt |grep "="
grep: (standard input): binary file matches
bandit9@bandit:~$ grep "=" data.txt
grep: data.txt: binary file matches
bandit9@bandit:~$ strings data.txt | grep "="
,U=\[
========== the
=>XO
Qe=B
2========== password
=5J"
========== is
=oFt2
9=Dc
Yh6=o
=d\!#d=
H/=Q
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
bWG=cE>
s=>5j
[xxhr=*
```

## strings

strings on Linux-komento, joka etsii tiedostosta tulostettavia (luettavia) merkkijonoja.

Se on erityisen hyödyllinen:
- binääritiedostoissa (.bin, .exe, .dat) & rikkinäistä merkistöä & sekavia merkintöjä/sekava/dataa tiedostojen alla , mutta se sisältää todennäköisesti piilotettua tekstiä seassa
- tuntemattomissa tiedostoissa
- reverse engineering -tilanteissa
- kun epäilet tiedoston sisältävän piilotettua tekstiä

strings suodattaa pois ei-luettavan datan ja näyttää vain tekstin.


Tulostaa kaikki luettavat merkkijonot tiedostosta.

Tämä auttaa löytämään:
- käyttäjänimiä (user=)
- salasanoja (password=)
- API-avaimia
- URL-osoitteita
- konfiguraatioita

---

# level 10

OHJE JA VINKKIT: The password for the next level is stored in the file data.txt, which contains base64 encoded data & komennot: grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd


```
PS C:\> ssh bandit10@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit10@bandit.labs.overthewire.org's password:

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

bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ whoami
bandit10
```

Salasana on data.txt alla, että jouduttaan enkoodata ja muuttaa merkintä koska sisältyy just base64

```
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==

bandit10@bandit:~$ base
base32    base64    basename  basenc
bandit10@bandit:~$ base64 data.txt
VkdobElIQmhjM04zYjNKa0lHbHpJR1IwVWpFM00yWmFTMkl3VWxKelJFWlRSM05uTWxKWGJuQk9W
bW96Y1ZKeUNnPT0K


bandit10@bandit:~$ base64 --help
Usage: base64 [OPTION]... [FILE]
Base64 encode or decode FILE, or standard input, to standard output.

With no FILE, or when FILE is -, read standard input.

Mandatory arguments to long options are mandatory for short options too.
  -d, --decode          decode data
  -i, --ignore-garbage  when decoding, ignore non-alphabet characters
  -w, --wrap=COLS       wrap encoded lines after COLS character (default 76).
                          Use 0 to disable line wrapping
      --help        display this help and exit
      --version     output version information and exit

The data are encoded as described for the base64 alphabet in RFC 4648.
When decoding, the input may contain newlines in addition to the bytes of
the formal base64 alphabet.  Use --ignore-garbage to attempt to recover
from any other non-alphabet bytes in the encoded stream.

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Full documentation <https://www.gnu.org/software/coreutils/base64>
or available locally via: info '(coreutils) base64 invocation'

bandit10@bandit:~$ base64 -d data.txt
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

```

Toinen vaihtoehtoinen vastaus, mutta toimii:
```
bandit10@bandit:~$ cat data.txt  | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```










