# level 20

0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

OHJE JA VIHJE: There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21). Tarvittavia komentoja: ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)


```
PS C:\> ssh bandit20@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit20@bandit.labs.overthewire.org's password:

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

bandit20@bandit:~$ whoami
bandit20
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15612 Apr  3 15:17 suconnect
```

## Toiminta ja tarkistus 

```
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15612 Apr  3 15:17 suconnect
bandit20@bandit:~$ ./suconnect cat /etc/bandit_pass/bandit21
getaddrinfo: Servname not supported for ai_socktype

bandit20@bandit:~$ ./suconnect | ssh bandit21@localhost port 2220
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit20/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit20/.ssh/known_hosts).

                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server on port 22, which is not intended.
!!! If you are trying to log in to an OverTheWire game, use the port mentioned in
!!! the "SSH Information" on that game's webpage (in the top left corner).

bandit21@localhost: Permission denied (publickey).

bandit20@bandit:~$ ./suconnect | ssh bandit21@bandit.labs.overthewire.org -p 2220
Pseudo-terminal will not be allocated because stdin is not a terminal.
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit20/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit20/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server with a password on port 2220 from localhost.
!!! Connecting from localhost is blocked to conserve resources.
!!! Please log out and log in again.

backend: gibson-1
Received disconnect from 127.0.0.1 port 2220:2: no authentication methods enabled
Disconnected from 127.0.0.1 port 2220

bandit20@bandit:~$ ./suconnect | nc localhost 2220
SSH-2.0-OpenSSH_9.6p1
Invalid SSH identification string.


bandit20@bandit:~$ jobs
bandit20@bandit:~$ id
uid=11020(bandit20) gid=11020(bandit20) groups=11020(bandit20)
bandit20@bandit:~$ fg
-bash: fg: current: no such job


bandit20@bandit:~$ echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 1234 &
[1] 18
bandit20@bandit:~$ ./suconnect 18
Could not connect
bandit20@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15612 Apr  3 15:17 suconnect
bandit20@bandit:~$ ./suconnect 1234
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
[1]+  Done                    echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 1234


```
## ratkaisu ja vastaus

kokeilin satunnaisen portin (numeronsa) - ja toistin sen perässä ja toimi
```
bandit20@bandit:~$ echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 3241 &
[1] 24
bandit20@bandit:~$ ./suconnect 3241
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
bandit20@bandit:~$ echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 6661 &
[1] 29
bandit20@bandit:~$ ./suconnect 6661
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
[1]+  Done                    echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 6661

```

## pieni pohdnta

Testasin useita portteja ja huomasin, että joissakin tapauksissa tuli virhe “Permission denied” (esim. portit 666 ja 99).

Tämä johtuu siitä, että Linux-järjestelmässä portit 0–1023 ovat niin sanottuja privileged portteja (etuoikeutettuja portteja). Näitä portteja voivat käyttää vain:

root-käyttäjä
tai ohjelmat, joilla on erityiset oikeudet

Koska käyttäjäni on: `uid=11020(bandit20)`

en ole root, joten en voi avata (bindata) näitä portteja → tästä syntyy virhe “Permission denied”.

Sen sijaan portit kuten 1234, 3241 ja 6661 toimivat, koska ne ovat yli 1023 ja siten tavallisen käyttäjän käytettävissä.

Yhteenveto:
- Portit < 1024 → vaativat root-oikeudet → eivät toimi tässä tapauksessa
- Portit ≥ 1024 → toimivat normaalilla käyttäjällä

## linkkejä:

- https://david-varghese.medium.com/overthewire-bandit-level-20-level-21-cc769247866a
- https://mayadevbe.me/posts/overthewire/bandit/level21/
- https://medium.com/secttp/overthewire-bandit-level-20-a1af9a042c56
