# level 21

EeoULMCra2q0dSkYj561DX7s1CpBuOBt

OHJE JA VIHJE: A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed. Tarvittavat komennot: cron, crontab, crontab(5) (use “man 5 crontab” to access this)
S

```
ssh bandit21@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit21@bandit.labs.overthewire.org's password:

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

bandit21@bandit:~$ whoami
bandit21
bandit21@bandit:~$ ls
bandit21@bandit:~$ cat /etc/bandit_pass/bandit21
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

## cron, crontab ja crontab(5)

Mikä on cron?
`cron` on Linuxin taustalla pyörivä palvelu (daemon), joka suorittaa komentoja automaattisesti ennalta määriteltyinä ajankohtina.

Sitä käytetään esimerkiksi:
- skriptien ajamiseen automaattisesti
- järjestelmän ylläpitotehtäviin
- ajastettuihin tarkistuksiin

Mikä on crontab?
`crontab` on komento, jolla hallitaan käyttäjän ajastettuja tehtäviä.

Yleisiä komentoja:
```bash
crontab -e    # muokkaa ajastuksia
crontab -l    # listaa ajastukset
crontab -r    # poistaa ajastukset
```

Mikä on crontab(5)?
Komento:
```bash
man 5 crontab
```

- `5` viittaa manuaalin osioon (file formats)
- tämä sivu selittää crontab-tiedoston syntaksin


Crontab-syntaksi

Ajastusrivi näyttää tältä:

```bash
* * * * * komento
```

Kenttien merkitys:
```
minuutti tunti päivä kuukausi viikonpäivä komento
```

Esimerkki:
```bash
* * * * * echo "hei"
```

→ suorittaa komennon joka minuutti

Yhteenveto:

- `cron` = taustapalvelu, joka suorittaa ajastettuja tehtäviä  
- `crontab` = työkalu ajastusten hallintaan  
- `man 5 crontab` = ohje syntaksille  

lukemista ja ohjeita:
- https://man7.org/linux/man-pages/man5/crontab.5.html
- https://www.linux.fi/wiki/Komentojen_ajastaminen

---

## tarkistus ja testaukset


```
bandit21@bandit:/etc/cron.d$ ls
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat

bandit21@bandit:/etc/cron.d$ ls -all
total 60
drwxr-xr-x   2 root root  4096 Apr  3 15:20 .
drwxr-xr-x 128 root root 12288 Apr  3 15:20 ..
-r--r-----   1 root root    47 Apr  3 15:18 behemoth4_cleanup
-rw-r--r--   1 root root   123 Apr  3 15:10 clean_tmp
-rw-r--r--   1 root root   120 Apr  3 15:17 cronjob_bandit22
-rw-r--r--   1 root root   122 Apr  3 15:17 cronjob_bandit23
-rw-r--r--   1 root root   120 Apr  3 15:17 cronjob_bandit24
-rw-r--r--   1 root root   201 Apr  8  2024 e2scrub_all
-r--r-----   1 root root    48 Apr  3 15:19 leviathan5_cleanup
-rw-------   1 root root   138 Apr  3 15:19 manpage3_resetpw_job
-rwx------   1 root root    52 Apr  3 15:20 otw-tmp-dir
-rw-r--r--   1 root root   102 Mar 31  2024 .placeholder
-rw-r--r--   1 root root   396 Jan  9  2024 sysstat

bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null

bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

## pohdinta

1. Tehtävä ja vihjeen mukaan sanoo: katso `/etc/cron.d/` ja selvitä mitä ajetaan

2. tarkistellaan se polku ja toistettaan se tiedosto `cat`
```
ls /etc/cron.d
cat cronjob_bandit22
```

3. löytyi toinen polku tiedosto: `* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null`
- tämä tarkoittaa käyttäjänä eli tässä tasossa bandit22 ajamaan skriptinsä: `/usr/bin/cronjob_bandit22.sh`

4. mitä tämä skripti tekekään? - `cat /usr/bin/cronjob_bandit22.sh`
5. skriptistä löytyi sisältyöä:
```
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
tämä tiedosto on normaalisti vain bandit22 luettavissa, että bandit21 ei pääse siihen suoraan käsiksi.

6. taas luettaan tämä polku `$cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

Cron-job suorittaa skriptin korkeamman tason käyttäjän oikeuksilla (bandit22) ja kirjoittaa arkaluontoisen tiedon (salasanan) julkisesti luettavaan tiedostoon `/tmp`-hakemistoon.

Tämä mahdollistaa sen, että alemmalla tasolla oleva käyttäjä (bandit21) voi lukea salasanan ilman suoraa pääsyä alkuperäiseen tiedostoon.

## ohjeita:

- https://mayadevbe.me/posts/overthewire/bandit/level22/
- https://david-varghese.medium.com/overthewire-bandit-level-21-level-22-6cb782b4b17b
