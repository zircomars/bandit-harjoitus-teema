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

ainakin greppataan - vähä kuin halutaan hakea tämän nimen perusteella ja nämä kaikki voi olla hyvinkin salasana.

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

nyt tehty ja tämä on varmaan se salasana ja testaamalla sitten tiedetään
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
```
