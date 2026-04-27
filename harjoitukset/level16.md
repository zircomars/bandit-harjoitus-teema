# level 16

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

VIHJE JA OHJE; The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it. & komennot: ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss


```
PS C:\> ssh bandit16@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit16@bandit.labs.overthewire.org's password:

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

bandit16@bandit:~$ whoami
bandit16
bandit16@bandit:~$ ls
bandit16@bandit:~
```

## testejä ja tarkistusta

```
bandit16@bandit:~$ nmap -p 31000-32000 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-26 17:05 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00014s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds

bandit16@bandit:~$ nmap -p 31000-32000 -T4 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-26 17:06 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00020s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds

bandit16@bandit:~$ nmap -p 22,80,31000-32000 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-26 17:08 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds


```

nämä portit ovat auki:
31046 , 31518 , 31691 , 31790, 31960

Tehtävä:
- mitkä puhuu SSL/TLS
- mitkä ei
- vain yksi antaa oikean vastauksen


seuraavaksi testaaan localhost:ia näiden porttien mukaan yksittelen

```
bandit16@bandit:~$ nmap -p 31000-32000 -sV --open localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-26 17:24 UTC

bandit16@bandit:~$ openssl s_client -connect localhost:31046
CONNECTED(00000003)
4067F0F7FF7F0000:error:0A0000F4:SSL routines:ossl_statem_client_read_transition:unexpected message:../ssl/statem/statem_clnt.c:398:
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 293 bytes and written 300 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
bandit16@bandit:~$ openssl s_client -connect localhost:31518
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
Certificate chain
 0 s:CN = SnakeOil
   i:CN = SnakeOil
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 10 03:59:50 2024 GMT; NotAfter: Jun  8 03:59:50 2034 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END CERTIFICATE-----
subject=CN = SnakeOil
issuer=CN = SnakeOil
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2103 bytes and written 373 bytes
Verification error: self-signed certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 4096 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 18 (self-signed certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: D545DBC2A76618BEFBB8AB23D05FD1648FA262D1119374ACDF740F7C37FF6947
    Session-ID-ctx:
    Resumption PSK: FC94B9CDFEFA4E454A2FC25FB86EFFBE54AB517EF8EEC8634845F75873BED77436AB79362B9A2F996AC7929B23980E4E
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - f7 05 63 0f 97 80 31 37-c2 63 65 9a bd e4 2a ce   ..c...17.ce...*.
    0010 - 14 27 eb c0 0a cf b6 87-68 e8 57 0a 25 29 d6 3f   .'......h.W.%).?
    0020 - ee 5c 3a 5c dd b4 42 cb-6a 22 06 c8 32 0f 9c 63   .\:\..B.j"..2..c
    0030 - 00 f3 28 85 ed 55 ba 80-10 61 b3 b0 0a 8b 33 c9   ..(..U...a....3.
    0040 - ea ef f3 e1 d2 23 72 af-30 20 77 d4 7e b1 98 f2   .....#r.0 w.~...
    0050 - 9f 1d 11 65 a7 60 08 fa-67 5b 29 83 94 62 5a 13   ...e.`..g[)..bZ.
    0060 - e2 6f 8b f3 17 c6 fa f9-86 e1 0b 73 8a a0 2a f0   .o.........s..*.
    0070 - 30 7c 88 ca 73 78 51 94-49 1d cd 1e 92 7e 26 f8   0|..sxQ.I....~&.
    0080 - 2f ee c8 b0 98 05 7b d5-65 ea 3c 66 dd 37 9b 4b   /.....{.e.<f.7.K
    0090 - 6e c8 48 4e 3e 1b 66 bd-6e 63 e7 c9 1d 3b 2f 99   n.HN>.f.nc...;/.
    00a0 - 60 80 bb 25 fb 51 bd b1-46 36 19 97 89 55 31 bb   `..%.Q..F6...U1.
    00b0 - d4 8a b7 4c fb 08 fd de-49 cb 69 3b d4 91 00 d6   ...L....I.i;....
    00c0 - 65 bf 84 4f 48 29 da d0-8c 92 85 01 77 4e 83 37   e..OH)......wN.7
    00d0 - 1b cf 7e f4 42 07 b5 b6-90 6d 46 8e 7b 37 e4 6b   ..~.B....mF.{7.k

    Start Time: 1777224375
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 3E55C734A32FB76AA53CED1692BD6744BC65B6B76288422CEFF142B33B292E41
    Session-ID-ctx:
    Resumption PSK: D2A75784B74F2D8157484543A102FD4EDF7678B0984F54D0E4C0EF62E757A32DF60D305E41FF32AFEA4C0D6CE8D72A93
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - f7 05 63 0f 97 80 31 37-c2 63 65 9a bd e4 2a ce   ..c...17.ce...*.
    0010 - 54 2e 49 2b 20 4a 84 62-a0 19 5c d0 2f a4 c2 9b   T.I+ J.b..\./...
    0020 - 77 f0 fd 6b 99 68 47 9d-36 52 67 b8 60 33 60 23   w..k.hG.6Rg.`3`#
    0030 - 43 c9 7a 12 6a 57 2c e7-a2 7f e4 fd 50 5e 4f 65   C.z.jW,.....P^Oe
    0040 - b9 bf c3 12 6e 16 c6 b9-0f 70 00 97 ac e8 bc c5   ....n....p......
    0050 - 89 35 36 a9 63 d8 00 45-88 b4 08 e4 bb b0 ae f7   .56.c..E........
    0060 - bb ee d7 73 c7 85 87 a3-1a 00 05 7f b9 21 f1 82   ...s.........!..
    0070 - 1d 69 f3 ff 4a 67 dd 7a-23 22 58 ff f5 70 fb 57   .i..Jg.z#"X..p.W
    0080 - 09 ba 93 a9 e5 2e 7e ec-9d da 18 c7 b8 2d 61 c4   ......~......-a.
    0090 - cd 9b 5f ee c8 83 ba 69-0f 83 79 f1 c6 c6 7f 3e   .._....i..y....>
    00a0 - 04 ea 2c a4 41 12 93 05-cb cd ad 0f 66 a7 78 35   ..,.A.......f.x5
    00b0 - ec 4b 28 57 72 a6 51 a6-b1 ff d2 86 cb f3 22 9a   .K(Wr.Q.......".
    00c0 - 21 8c 47 9f 97 fd 22 28-a0 88 b3 03 5c a7 dd 9d   !.G..."(....\...
    00d0 - 98 d7 1d bb 17 92 46 e9-37 ae d8 fa 00 bc 67 04   ......F.7.....g.

    Start Time: 1777224375
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
closed

bandit16@bandit:~$ openssl s_client -connect localhost:31518
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
Certificate chain
 0 s:CN = SnakeOil
   i:CN = SnakeOil
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 10 03:59:50 2024 GMT; NotAfter: Jun  8 03:59:50 2034 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END CERTIFICATE-----
subject=CN = SnakeOil
issuer=CN = SnakeOil
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2103 bytes and written 373 bytes
Verification error: self-signed certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 4096 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 18 (self-signed certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 5C35BE92673403B52EECE9C6442CFE38950CE3AB4D40D7E637D974481CCFA5D0
    Session-ID-ctx:
    Resumption PSK: 0FD675C877F92B0CB2FC771F3BA3F01EBC455DB0DF9AF882E670670EF86C02CD89C62C06A44D69B1FC8164645E87251D
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - f7 05 63 0f 97 80 31 37-c2 63 65 9a bd e4 2a ce   ..c...17.ce...*.
    0010 - cc 3f 87 64 3e c1 31 b8-99 56 d0 92 9b 55 2b 51   .?.d>.1..V...U+Q
    0020 - cf 41 0c 00 53 60 b8 03-f5 9e 5d 46 d1 9e 98 41   .A..S`....]F...A
    0030 - 0e 93 9f 03 dd 5a e9 34-df b0 84 bd fb 33 05 28   .....Z.4.....3.(
    0040 - 88 59 00 71 1a d3 09 b4-90 66 a6 c3 97 3f 52 4b   .Y.q.....f...?RK
    0050 - 08 04 47 00 99 f9 16 f0-a2 50 08 6a 74 fa dd 23   ..G......P.jt..#
    0060 - 5b fc 36 b3 8d 3c e2 45-ac 92 8a 07 70 45 36 c1   [.6..<.E....pE6.
    0070 - 36 73 9a 94 7d a4 bb ce-7d 2d 01 f3 b4 c3 05 99   6s..}...}-......
    0080 - cd 66 20 b1 96 9d 15 97-16 df ac 48 90 94 ae c3   .f ........H....
    0090 - b9 5f 14 46 76 ef 84 e8-8f 2c 0d b1 a2 0b 11 65   ._.Fv....,.....e
    00a0 - ee c6 14 34 37 b9 ea b2-cd b1 fd 4f 3a ed 6f b8   ...47......O:.o.
    00b0 - 76 b8 65 79 41 8c 00 23-43 a9 23 b4 1a 86 45 a2   v.eyA..#C.#...E.
    00c0 - ca 5c a3 90 9d 17 72 58-3a 88 47 63 60 f9 92 8e   .\....rX:.Gc`...
    00d0 - 9b 57 e1 80 c0 a0 e8 63-1a 90 bc 87 61 69 ab 78   .W.....c....ai.x

    Start Time: 1777224393
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: E9828257CC6954943EC26B03ADBCE023CDF9E620537A44994C9A01B980BB5404
    Session-ID-ctx:
    Resumption PSK: 86CB2523F6BAE7E6422926DA505D3B8219B7C3892E3F85E35FAE423DAC9626B0D967F73249F02AD6F49771CCDA649D21
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - f7 05 63 0f 97 80 31 37-c2 63 65 9a bd e4 2a ce   ..c...17.ce...*.
    0010 - 28 41 f9 fd cf 23 e2 ff-37 10 72 1b 5a 8e 04 fc   (A...#..7.r.Z...
    0020 - 3e c2 00 81 00 c1 cf 10-b2 17 75 0d 21 a7 66 93   >.........u.!.f.
    0030 - 65 49 55 fd c5 df e7 e2-02 77 e5 8e 3a d2 bd 05   eIU......w..:...
    0040 - 1d 57 bd fd e2 a7 0e 67-ea 5d 50 06 2d da 5d 1f   .W.....g.]P.-.].
    0050 - 4b c9 d6 e4 c1 a8 1a ff-e1 b7 31 80 8c 48 be f7   K.........1..H..
    0060 - 22 78 59 83 99 7f af c1-83 2d 27 c9 07 7a 7d 1d   "xY......-'..z}.
    0070 - 06 d2 fe 27 ac a8 2c 2d-4e 07 8b 27 db c9 17 c8   ...'..,-N..'....
    0080 - 17 44 53 9a 2c 6e 43 d2-6a 84 ee a3 93 8d 2a 44   .DS.,nC.j.....*D
    0090 - 8a 8e 3b e3 59 f9 f8 e8-57 e6 ab ef df e4 ac 7a   ..;.Y...W......z
    00a0 - 5e bf cf 9d 7b 9b a5 c0-e5 f9 62 c9 a9 17 3c 95   ^...{.....b...<.
    00b0 - e3 0f ed ee 83 fb 83 d4-84 ca 59 bf a6 34 4a a1   ..........Y..4J.
    00c0 - e9 62 46 e1 f4 7f fe ab-ac c5 8a 5c ee 9f d2 24   .bF........\...$
    00d0 - 27 8b cc 2a 25 f7 6d f6-a6 2a 80 b2 a8 cb be 1e   '..*%.m..*......

    Start Time: 1777224393
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
KEYUPDATE
closed

bandit16@bandit:~$ openssl s_client -connect localhost:31691
CONNECTED(00000003)
4067F0F7FF7F0000:error:0A0000F4:SSL routines:ossl_statem_client_read_transition:unexpected message:../ssl/statem/statem_clnt.c:398:
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 293 bytes and written 300 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---


bandit16@bandit:~$ openssl s_client -connect localhost:31790
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
Certificate chain
 0 s:CN = SnakeOil
   i:CN = SnakeOil
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 10 03:59:50 2024 GMT; NotAfter: Jun  8 03:59:50 2034 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END CERTIFICATE-----
subject=CN = SnakeOil
issuer=CN = SnakeOil
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2103 bytes and written 373 bytes
Verification error: self-signed certificate
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 4096 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 18 (self-signed certificate)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: FC2F92AA52BED567EE6C56D94238BA1B1E50BDB5E287763126DC6A97FF06B598
    Session-ID-ctx:
    Resumption PSK: BF0898A06FA7F3C4335C103395F98893110A84D84161D3F83ED8AD553320DAC0AE46C85F819132F3D474987E6E2C8719
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - 21 ce 74 33 d0 14 1d 7b-55 e5 12 bc 30 7b e9 c9   !.t3...{U...0{..
    0010 - 53 fa a9 bc 29 4a ad 87-0c bc 8f de 58 f8 e3 46   S...)J......X..F
    0020 - 9c 0c aa 31 b6 aa db a2-99 c1 63 d7 a8 7f 2f 02   ...1......c.../.
    0030 - 7f 1f 22 1c 5a 30 4b 99-95 4b 2b 95 ae 93 b0 ce   ..".Z0K..K+.....
    0040 - 79 69 01 b1 f3 76 a4 24-9c 46 e4 90 a1 5d 9b 65   yi...v.$.F...].e
    0050 - a6 bf 9a 89 14 74 1f 34-3b e2 5e a0 9c d6 54 f2   .....t.4;.^...T.
    0060 - 9c 2b 4d 25 63 ce 33 64-00 2a 90 a3 5e ff 92 c5   .+M%c.3d.*..^...
    0070 - c4 6b 7e 2e 66 73 4a 09-a8 86 94 67 56 7e db 9c   .k~.fsJ....gV~..
    0080 - 9a 94 b4 7e 6c 76 db 92-ba e7 14 cb db 14 ff df   ...~lv..........
    0090 - 4d cd ab bc 73 b8 c9 95-34 7a 1b 93 f5 cf 9f 69   M...s...4z.....i
    00a0 - cf b7 70 da f8 b2 dd fc-46 d3 22 f9 8a 53 d5 54   ..p.....F."..S.T
    00b0 - 79 d7 64 28 6e 17 dc 5a-67 a6 63 69 a2 91 a8 92   y.d(n..Zg.ci....
    00c0 - b5 a2 d9 d8 2a 05 08 5c-7e 63 20 2f 45 2d 08 44   ....*..\~c /E-.D
    00d0 - e0 34 2f 97 08 64 fc 96-a2 cc c5 1e be 27 5a 2e   .4/..d.......'Z.

    Start Time: 1777224569
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 1B68EDF11C748CADF2B46F0EA24242CA1A587DBA27289A019BE53E2CA699CEBC
    Session-ID-ctx:
    Resumption PSK: 6BBED8F1A4A516184A3F66ADBD52B8235507E604D779585DCC258B5F1DFB04B2FC97CEA26A962E31F3D4F1433422130E
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - 21 ce 74 33 d0 14 1d 7b-55 e5 12 bc 30 7b e9 c9   !.t3...{U...0{..
    0010 - ed 23 41 60 33 87 e1 ad-26 72 63 88 df a0 9e 23   .#A`3...&rc....#
    0020 - c9 01 47 e6 a0 82 d7 e7-1e 83 bd c3 6e 78 82 6d   ..G.........nx.m
    0030 - e7 b5 fb 28 58 aa cb 91-d5 ef 08 e7 8d 35 d9 26   ...(X........5.&
    0040 - 9a 3a b1 d3 1d ca 0c 35-0a 3d 79 1d 11 54 e9 5c   .:.....5.=y..T.\
    0050 - aa 44 eb c1 57 4a f7 16-6c 79 bc 14 6c 5c b1 8b   .D..WJ..ly..l\..
    0060 - 18 15 91 cb ce 96 df b9-de 00 5b e4 34 7b 40 0a   ..........[.4{@.
    0070 - c7 1b ad fd 40 64 83 3a-9e 8c 21 25 f3 a4 c9 45   ....@d.:..!%...E
    0080 - b8 a1 df b9 35 cf 61 04-38 3d 42 9c e9 1c 8e 08   ....5.a.8=B.....
    0090 - 4d 35 e7 8e a1 d8 4c fc-a4 62 d4 63 7f 21 23 46   M5....L..b.c.!#F
    00a0 - 97 c0 28 f7 16 88 90 82-0d 6c 3d cd 7b 24 16 6c   ..(......l=.{$.l
    00b0 - 98 a4 3b 62 30 1d 52 cd-57 78 04 80 d4 7c 1d ab   ..;b0.R.Wx...|..
    00c0 - b8 c9 67 3b 70 92 c1 cd-32 be 3a de 07 72 fa 09   ..g;p...2.:..r..
    00d0 - c6 3a 32 78 ce 4a 05 86-12 ca ef f7 62 ac 85 39   .:2x.J......b..9

    Start Time: 1777224569
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
KEYUPDATE
CONNECTED COMMANDS
Wrong! Please enter the correct current password.
closed

bandit16@bandit:~$ openssl s_client -connect localhost:31960
CONNECTED(00000003)
4067F0F7FF7F0000:error:0A0000F4:SSL routines:ossl_statem_client_read_transition:unexpected message:../ssl/statem/statem_clnt.c:398:
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 293 bytes and written 300 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
```

saattiin taas lisää suodatusta, että vain kaksi-kolme jotka toimii mutta nissä tapahtui pitää syötää komentoa 

- openssl s_client -connect localhost:31046
- openssl s_client -connect localhost:31518
- openssl s_client -connect localhost:31790

cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31790

tämä eka on jotakin ja se on **SSH privat key**
```
bandit16@bandit:~$ cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31790
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
Correct!
-----BEGIN RSA PRIVATE KEY-----
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END RSA PRIVATE KEY-----

bandit16@bandit:~$ cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31518
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx


bandit16@bandit:~$ cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31046
4067F0F7FF7F0000:error:0A0000F4:SSL routines:ossl_statem_client_read_transition:unexpected message:../ssl/statem/statem_clnt.c:398:
```

---

## ratkaisu

Sekä huomoina näiden kolmen lohkossa (RSA certificate) ovat sama X.509-varmenne. 

