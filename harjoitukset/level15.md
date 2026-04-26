# level 15

VIHJE JA OHJE: The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption. Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage. && KOMENNOT: ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss



```
PS C:\Users\zirco-> ssh bandit15@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit15@bandit.labs.overthewire.org's password:

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

bandit15@bandit:~$ whoami
bandit15
bandit15@bandit:~$ ls
bandit15@bandit:~$ ll
total 24
drwxr-xr-x   2 root     root     4096 Apr  3 15:17 ./
drwxr-xr-x 150 root     root     4096 Apr  3 15:20 ../
-rw-r-----   1 bandit15 bandit15   33 Apr  3 15:17 .bandit14.password
-rw-r--r--   1 root     root      220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root     root     3851 Apr  3 15:10 .bashrc
-rw-r--r--   1 root     root      807 Mar 31  2024 .profile

```

## testejä ja tarkistusta

nc (netcat)
→ yksinkertainen, ei salausta oletuksena
ncat (tulee usein Nmap mukana)
→ tukee suoraan SSL/TLS:ää
socat
→ “super-netcat”, paljon monipuolisempi
→ tukee SSL, tiedostoja, soketteja jne.

port 30 001 on localhost using SSL/TLS encryption.


telnet, perus nc → ❌ ei TLS:ää
ssh → ❌ eri protokolla
netstat, ss → ❌ vain paikalliseen tarkasteluun

```
bandit15@bandit:~$ cat /etc/bandit_pass/bandit16 | nc localhost 300001
cat: /etc/bandit_pass/bandit16: Permission denied
nc: port number too large: 300001
bandit15@bandit:~$ cat /etc/bandit_pass/bandit16 | ncat localhost 300001
cat: /etc/bandit_pass/bandit16: Permission denied
Ncat: Invalid port number "300001". QUITTING.

bandit15@bandit:~$ ncat --ssl -l 30001
Ncat: bind to 0.0.0.0:30001: Address already in use. QUITTING.
bandit15@bandit:~$ socat - OPENSSL:localhost:30001
2026/04/25 11:39:19 socat[31] W OpenSSL: Warning: this implementation does not check CRLs
2026/04/25 11:39:19 socat[31] E SSL_connect(): error:0A000086:SSL routines::certificate verify failed
bandit15@bandit:~$ socat OPENSSL-LISTEN:30001,cert=server.pem,key=server.key -
2026/04/25 11:39:31 socat[32] E SSL_CTX_use_certificate_file(): error:80000002:system library::No such file or director
```

Nyt alkoi tulla jotakin jännää
- alkoi muodostaa TLS-yhteyden
- näyttää sertifikaatin
- antoi kirojittaa syötettä

```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
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
MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
/AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
+Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
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
    Session-ID: 2B05213B97EE3629659433496380860329BBD0715A4ED66231C29B1632BEF1C4
    Session-ID-ctx:
    Resumption PSK: C2206F2EFC1E83261783BF334F9AF5EAC56AAFCC0A0BEFA37763B06CBC3FE35187EDEB09A5959C16854DB9E7922B85AB
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - f2 d4 a8 8b 8f dd 51 ce-29 24 65 50 30 4f 1f 82   ......Q.)$eP0O..
    0020 - 3b 1a e4 31 67 5e 40 b0-30 fa bb 28 ed c7 2d 5c   ;..1g^@.0..(..-\
    0030 - e5 34 bb 26 6b 98 c0 b2-68 08 ef 79 ed f6 31 59   .4.&k...h..y..1Y
    0040 - 7d 95 f6 70 61 d3 00 6d-94 7e 83 97 8e ab e1 c2   }..pa..m.~......
    0050 - 23 9d fa c3 7c b4 82 26-61 dd 12 bd 47 bd 9c 4a   #...|..&a...G..J
    0060 - a3 2d 50 9a d6 51 12 43-27 1b 90 03 67 13 da a2   .-P..Q.C'...g...
    0070 - 97 6e 81 a7 1b fd 9e 98-3e ea 2e 43 ca dd a5 fa   .n......>..C....
    0080 - 99 c6 f2 94 4c aa 4f 8f-d4 ed b7 ba 8b 61 2e 10   ....L.O......a..
    0090 - 73 fb 94 47 b4 15 57 b6-41 f2 82 76 d8 da ae d1   s..G..W.A..v....
    00a0 - 1e a4 d3 d4 ca ab cc 96-8c 32 2e 91 26 90 7a d9   .........2..&.z.
    00b0 - ff 67 4e 5f d7 4e 22 cd-83 78 8e a6 e7 a9 7b d3   .gN_.N"..x....{.
    00c0 - 6b a9 cb b8 38 68 6a 87-a6 73 57 3e b2 d9 e0 fe   k...8hj..sW>....
    00d0 - 43 8a 65 ff 4e 4e 2a 2e-f3 a8 2d 60 6b 17 31 f1   C.e.NN*...-`k.1.

    Start Time: 1777117246
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
    Session-ID: A8BC68C9B07B49D70FEF03CF1B685618DC3D8F9391FD63F1D413F2F8D40C2B9B
    Session-ID-ctx:
    Resumption PSK: 2D166E8F1133776907C9CA90B41E2343161F70A293FDD475565FEC18778F9F8E2BD33E667EB191D50CF9C632D6F2D303
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 3b ae fa 80 46 c1 3c 53-22 df 89 2c 1c 2b de 50   ;...F.<S"..,.+.P
    0020 - 66 54 47 17 73 65 b1 1e-21 8b 20 d7 42 32 84 bc   fTG.se..!. .B2..
    0030 - ec b1 5e 1d 71 eb d1 73-05 88 41 25 73 26 4a dc   ..^.q..s..A%s&J.
    0040 - 8c 70 66 ff e1 09 cf b3-8d 59 c3 5e 11 83 2f 2a   .pf......Y.^../*
    0050 - 29 df 5a 58 6e 19 b5 15-0a 1e fd 05 c8 d4 35 c6   ).ZXn.........5.
    0060 - f9 d4 d1 7f 58 9d 39 7b-d3 60 05 e2 92 85 f0 92   ....X.9{.`......
    0070 - 5e 4b d4 ba 87 02 f3 62-63 5b ad 60 6c 7f 51 4e   ^K.....bc[.`l.QN
    0080 - 79 29 bb de 99 6a a3 1d-87 d9 55 36 3e 85 b1 bb   y)...j....U6>...
    0090 - fd 32 27 6b fc 1c 3d a0-f2 99 4e a4 54 c7 1d 7e   .2'k..=...N.T..~
    00a0 - 2c f9 ca 95 4e f6 9a ef-46 4c 21 7f be f8 f3 c7   ,...N...FL!.....
    00b0 - 19 93 3a 75 3e d1 ea 69-6d 6c de 5d 5c 1f 96 f9   ..:u>..iml.]\...
    00c0 - 11 14 21 02 d1 16 7e 64-04 1e 6f 4d 6e e6 c7 c8   ..!...~d..oMn...
    00d0 - 7c ca ae f4 ba ea c9 f0-d3 c4 9a e8 62 12 47 e6   |...........b.G.

    Start Time: 1777117246
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
R
RENEGOTIATING
4067F0F7FF7F0000:error:0A00010A:SSL routines:can_renegotiate:wrong ssl version:../ssl/ssl_lib.c:2323:
```


Option2: 
`cat /etc/bandit_pass/bandit14 | openssl s_client -connect localhost:30001`

en tiedä onko tämä sama kuin aikaisempi mutta näyttä ainakin jotakin sertifikaatti dataa näyttävän

```
bandit15@bandit:~$ cat /etc/bandit_pass/bandit14 | openssl s_client -connect localhost:30001
cat: /etc/bandit_pass/bandit14: Permission denied
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
MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
/AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
+Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
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
DONE
```

```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
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
MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
/AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
+Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
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
    Session-ID: A63C9557153459AB24BEAEA1C07224EB0486FC136775F035892856C60DAE27CA
    Session-ID-ctx:
    Resumption PSK: F256E8CA3CA3AC541EB263373DE24DA04EC4E6AE3FF752B990B7DAD4D9277DB5A88C37B98EBDD296A8EA1C8CF1B8D09D
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 75 49 18 66 91 6c fb 0a-b2 37 bf fc ef e0 f5 09   uI.f.l...7......
    0020 - 5c 88 0d d0 20 b0 84 10-fe 0b 3a 6f 5f 72 f0 cb   \... .....:o_r..
    0030 - f4 bd 2c 22 71 14 53 b9-d6 58 50 cf 94 77 76 45   ..,"q.S..XP..wvE
    0040 - 19 fe 9c 89 fc fe c4 62-29 48 6d 51 8d 30 5e 31   .......b)HmQ.0^1
    0050 - 56 c8 a2 e6 d7 6a 42 5d-9a 9c 49 56 5e e5 83 bc   V....jB]..IV^...
    0060 - ad fc be ef 70 8a 9a 03-6b fb 5a 7d 52 14 53 08   ....p...k.Z}R.S.
    0070 - 40 b4 d2 ce ce 69 22 2f-d6 cb 14 b7 bb c0 e0 58   @....i"/.......X
    0080 - ef d2 06 ea 5a fc 1b 1f-e5 3a 3b e4 42 ce 5d 51   ....Z....:;.B.]Q
    0090 - 27 77 f8 25 d4 4a ab e5-a5 7a 5c 7d 6c 6f 43 38   'w.%.J...z\}loC8
    00a0 - 6e 23 f1 cf 8d 7f 09 82-b7 73 33 bd 60 51 c9 48   n#.......s3.`Q.H
    00b0 - 08 eb 92 ff 98 ab 4e 3a-dd ca 77 47 f8 8f 3e 31   ......N:..wG..>1
    00c0 - 24 dd 45 e8 29 dd ad 8c-7a fe e3 b2 67 d5 13 bf   $.E.)...z...g...
    00d0 - 08 1d d6 ea d0 ef 50 87-8b 42 bd ab d8 b8 77 e1   ......P..B....w.

    Start Time: 1777119735
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
    Session-ID: C8EE641A733EA9F89B2F459560E19197CB6A9B032BDFBF58F8C3ED229AA599BA
    Session-ID-ctx:
    Resumption PSK: D5C52990408B0C802FA43572D29427586C6A8118A7C16E20B7A086E3B080CB97E1FFF0168AA057E81C72ADB295298D66
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - ec b2 3c 6a df ed 2a c9-2a 1a d1 00 50 d7 40 32   ..<j..*.*...P.@2
    0020 - d1 de 9b 8c 89 d2 ce 4f-7d 9c 75 d1 f8 cc b6 46   .......O}.u....F
    0030 - 6b e7 96 63 c6 2e 6c e3-d9 5d 4a d8 d8 4a 55 e3   k..c..l..]J..JU.
    0040 - f6 64 b3 8c 5c 1e ac 1d-3b 36 ca db 7b 8e bd c4   .d..\...;6..{...
    0050 - 33 4b 79 0a ba e6 fc 3c-c2 2c f0 31 20 b6 00 31   3Ky....<.,.1 ..1
    0060 - 75 35 fe 01 f9 4c a0 6c-a3 dc e1 11 0c 32 4d d5   u5...L.l.....2M.
    0070 - be 38 27 22 94 da 22 b9-e6 11 dc a2 31 b3 e6 7a   .8'"..".....1..z
    0080 - ca da 2b fc 5a a5 f0 b2-3e 5c df a3 45 6d 8b 4c   ..+.Z...>\..Em.L
    0090 - cb f3 63 d0 0d 03 ce 47-09 eb a2 9b ed fa 62 fb   ..c....G......b.
    00a0 - bb 6f 3b c6 93 0d 37 04-14 90 bc 18 ed e0 96 4a   .o;...7........J
    00b0 - 27 51 d9 df e8 15 bd d5-59 95 48 91 56 f8 e9 78   'Q......Y.H.V..x
    00c0 - b2 fe 2b 1f 1d 26 51 06-17 30 3e 2d f7 24 01 5a   ..+..&Q..0>-.$.Z
    00d0 - 27 57 2c 8f 3f fb 4c 3e-f7 7d b3 e0 f7 85 90 e4   'W,.?.L>.}......

    Start Time: 1777119735
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
R
RENEGOTIATING
4067F0F7FF7F0000:error:0A00010A:SSL routines:can_renegotiate:wrong ssl version:../ssl/ssl_lib.c:2323:

```

välissä jotenkin keskeytytin jotakin, eiku hetkonen tämä tason 15 salasana jolla kirjauduttaan ja ei tiedetä vielä Bandit16 tason salasanaa
```
bandit15@bandit:~$ cat /etc/bandit_pass/bandit14 | openssl s_client -quiet -connect localhost:30001
cat: /etc/bandit_pass/bandit14: Permission denied
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1

^C
bandit15@bandit:~$ ls
bandit15@bandit:~$ whoami
bandit15
bandit15@bandit:~$ cat /etc/bandit_pass/bandit15
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
bandit15@bandit:~$ cat /etc/bandit_pass/bandit15
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

--- 


## ratkaisu 

Eli miten tätä ratkaisua ratkaistettaankaan ja koskien aikaisempien testien ja tarkistuksen perusteella


Aikaisemmassa tuosta komennosta mitä syötetty ja avattu localhost:30001 siinä loppussa lukee näin, niin syötetty "R" eli read, mutta sen jälkeen pitäisi avata vastauksensa, mutta ei. 

```
read R BLOCK
R
RENEGOTIATING
4067F0F7FF7F0000:error:0A00010A:SSL routines:can_renegotiate:wrong ssl version:../ssl/ssl_lib.c:2323:
```

Kuitenkin jouduttaan suorittaa jonkinlainen kikka kolmonen / kulkea mutkan kautta. Ohjeiden ja vinkin mukaan tässä **pitäisi käyttää nykyisen tason eli level 15 sitä salasanaa** porttiin 30 001 paikkallisen localhostia varten. Tästä tuli muutama vaihtoehtoista vastausta, mutta komennoista tuli tarkennus koskien **ncat**.

ESIM 1)
```
bandit15@bandit:~$ echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -ign_eof
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
MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
/AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
+Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
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
    Session-ID: 4760E0378182110559EF2798423BCBD0008D45433B1B134423E764B217D6BB51
    Session-ID-ctx:
    Resumption PSK: 9984753B8F6E391D050B4ED110B3BD9AF995FCA988EF49F4517A1012FDF094A886FDDE20FC617C065ACE15315A222267
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 18 a4 ad 93 28 80 be ac-f1 d0 49 4e 69 e6 fb a9   ....(.....INi...
    0020 - d0 42 ea 34 22 d3 45 a7-cf 28 77 9a de 73 1a 3b   .B.4".E..(w..s.;
    0030 - 45 52 b0 04 6f ef f5 84-b6 82 01 07 9a f9 2c d8   ER..o.........,.
    0040 - 15 93 8a 83 11 f6 70 6f-c6 9c fb 2e da 1e a2 a5   ......po........
    0050 - 07 fc 2f 1c 02 4d 9c a6-d8 ca d8 dd 8d 5a 1d e3   ../..M.......Z..
    0060 - 7d 63 34 7e 17 c5 a8 f5-58 17 f7 1f fe 49 df 83   }c4~....X....I..
    0070 - e0 be 3e 49 64 75 f9 83-02 c4 9d 68 52 d2 cc 9c   ..>Idu.....hR...
    0080 - 38 29 24 34 d2 42 c7 3e-51 aa 74 ba 73 e2 24 92   8)$4.B.>Q.t.s.$.
    0090 - c6 25 e5 79 ba 79 8a b7-32 f9 71 37 20 8f d8 7f   .%.y.y..2.q7 ...
    00a0 - 12 6d c5 2c 9d 75 42 ac-1d 74 e1 19 00 87 ef c8   .m.,.uB..t......
    00b0 - e0 e7 85 e5 ba 50 64 83-78 91 46 08 c2 86 ab 69   .....Pd.x.F....i
    00c0 - ec 1e ce d0 21 a3 a4 1f-c2 1e 45 6e ef ae cf e3   ....!.....En....
    00d0 - 55 a3 82 53 01 12 32 0b-e1 d9 31 79 44 ac a7 af   U..S..2...1yD...

    Start Time: 1777196287
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
    Session-ID: 791DA3FDAA723B27C61D481FEA87B458E984C9BA4EFC5AC0C82C68E7DE82B444
    Session-ID-ctx:
    Resumption PSK: 9299CBAD128573FD498933934B3112C862CF5C28B51858B2DC1BD12CCBB0988E1FCED1142918E207056AE749BF841C57
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 4d c5 5a 9a 82 bf f4 08-ca 51 90 8d f9 dd 5c a4   M.Z......Q....\.
    0020 - 4f e9 14 89 97 29 b1 6b-72 97 2c 6d 15 ca 0a 9b   O....).kr.,m....
    0030 - 46 bc ea b1 d6 06 12 92-78 da ee c3 fa 8d 3a ca   F.......x.....:.
    0040 - 91 3b eb 23 eb 54 b2 13-d4 f5 ec 23 8a c1 3a d3   .;.#.T.....#..:.
    0050 - 2c 52 97 64 41 4c 0e 00-23 f8 88 c9 ea 32 d9 00   ,R.dAL..#....2..
    0060 - 38 40 7c 9e ce c3 06 7c-47 93 13 14 1f 8f ed 14   8@|....|G.......
    0070 - 44 4c a5 65 23 4b 65 df-f6 cf 1d db fa 3c ec 42   DL.e#Ke......<.B
    0080 - fc 9b 17 44 fb 4d 2d 54-55 95 5a 8e 5a 28 b2 cd   ...D.M-TU.Z.Z(..
    0090 - ff d0 4b 27 94 05 fb c1-0d e4 7a f4 bb 81 d8 e9   ..K'......z.....
    00a0 - e2 24 56 ed d9 23 4a 7a-c6 a0 cc b1 1a 3d f5 c3   .$V..#Jz.....=..
    00b0 - 0e ab aa e3 58 a1 e1 be-f6 7c fa 76 3a df 94 60   ....X....|.v:..`
    00c0 - ff 18 b6 4b ef 64 49 45-b8 4c 2f b5 3e 3a 18 24   ...K.dIE.L/.>:.$
    00d0 - 81 9d c0 d2 aa 96 74 92-ca 30 c5 61 bf 4c cc 1f   ......t..0.a.L..

    Start Time: 1777196287
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
```

ESIM 2)
```
bandit15@bandit:~$ echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | ncat --ssl localhost 30001
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
HUOM 2.5) tätä testattu ja tämä ei toimi siksi ilman echo ei toimi:
```
bandit15@bandit:~$ ncat --ssl locahost 30001
Ncat: Could not resolve hostname "locahost": Temporary failure in name resolution. QUITTING.
```


ESIM 3)
```
bandit15@bandit:~$ echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -quiet -connect localhost:30001
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

ESIM 4)
```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
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
MIIFBzCCAu+gAwIBAgIUBLz7DBxA0IfojaL/WaJzE6Sbz7cwDQYJKoZIhvcNAQEL
BQAwEzERMA8GA1UEAwwIU25ha2VPaWwwHhcNMjQwNjEwMDM1OTUwWhcNMzQwNjA4
MDM1OTUwWjATMREwDwYDVQQDDAhTbmFrZU9pbDCCAiIwDQYJKoZIhvcNAQEBBQAD
ggIPADCCAgoCggIBANI+P5QXm9Bj21FIPsQqbqZRb5XmSZZJYaam7EIJ16Fxedf+
jXAv4d/FVqiEM4BuSNsNMeBMx2Gq0lAfN33h+RMTjRoMb8yBsZsC063MLfXCk4p+
09gtGP7BS6Iy5XdmfY/fPHvA3JDEScdlDDmd6Lsbdwhv93Q8M6POVO9sv4HuS4t/
jEjr+NhE+Bjr/wDbyg7GL71BP1WPZpQnRE4OzoSrt5+bZVLvODWUFwinB0fLaGRk
GmI0r5EUOUd7HpYyoIQbiNlePGfPpHRKnmdXTTEZEoxeWWAaM1VhPGqfrB/Pnca+
vAJX7iBOb3kHinmfVOScsG/YAUR94wSELeY+UlEWJaELVUntrJ5HeRDiTChiVQ++
wnnjNbepaW6shopybUF3XXfhIb4NvwLWpvoKFXVtcVjlOujF0snVvpE+MRT0wacy
tHtjZs7Ao7GYxDz6H8AdBLKJW67uQon37a4MI260ADFMS+2vEAbNSFP+f6ii5mrB
18cY64ZaF6oU8bjGK7BArDx56bRc3WFyuBIGWAFHEuB948BcshXY7baf5jjzPmgz
mq1zdRthQB31MOM2ii6vuTkheAvKfFf+llH4M9SnES4NSF2hj9NnHga9V08wfhYc
x0W6qu+S8HUdVF+V23yTvUNgz4Q+UoGs4sHSDEsIBFqNvInnpUmtNgcR2L5PAgMB
AAGjUzBRMB0GA1UdDgQWBBTPo8kfze4P9EgxNuyk7+xDGFtAYzAfBgNVHSMEGDAW
gBTPo8kfze4P9EgxNuyk7+xDGFtAYzAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
DQEBCwUAA4ICAQAKHomtmcGqyiLnhziLe97Mq2+Sul5QgYVwfx/KYOXxv2T8ZmcR
Ae9XFhZT4jsAOUDK1OXx9aZgDGJHJLNEVTe9zWv1ONFfNxEBxQgP7hhmDBWdtj6d
taqEW/Jp06X+08BtnYK9NZsvDg2YRcvOHConeMjwvEL7tQK0m+GVyQfLYg6jnrhx
egH+abucTKxabFcWSE+Vk0uJYMqcbXvB4WNKz9vj4V5Hn7/DN4xIjFko+nREw6Oa
/AUFjNnO/FPjap+d68H1LdzMH3PSs+yjGid+6Zx9FCnt9qZydW13Miqg3nDnODXw
+Z682mQFjVlGPCA5ZOQbyMKY4tNazG2n8qy2famQT3+jF8Lb6a4NGbnpeWnLMkIu
jWLWIkA9MlbdNXuajiPNVyYIK9gdoBzbfaKwoOfSsLxEqlf8rio1GGcEV5Hlz5S2
txwI0xdW9MWeGWoiLbZSbRJH4TIBFFtoBG0LoEJi0C+UPwS8CDngJB4TyrZqEld3
rH87W+Et1t/Nepoc/Eoaux9PFp5VPXP+qwQGmhir/hv7OsgBhrkYuhkjxZ8+1uk7
tUWC/XM0mpLoxsq6vVl3AJaJe1ivdA9xLytsuG4iv02Juc593HXYR8yOpow0Eq2T
U5EyeuFg5RXYwAPi7ykw1PW7zAPL4MlonEVz+QXOSx6eyhimp1VZC11SCg==
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
    Session-ID: C51481C8C3F6705773B32F0DD82B682A25115B56571E4070E57106A7E74940E7
    Session-ID-ctx:
    Resumption PSK: BC69AE901D7B6EBB2805A42BF82ED206B0DD9A55EC595F94E133DCFD568CAF9AF28D4A3DD0152CBA6CC8B269CF3FDDD8
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 51 b8 6f 8c 2d 90 40 76-6d 5c 3c 28 f7 ce 80 3f   Q.o.-.@vm\<(...?
    0020 - 78 92 1a 08 60 24 c3 2e-56 de f6 f3 f4 2b 34 cc   x...`$..V....+4.
    0030 - 8d 63 7e a6 61 4a d2 7c-4f cd 2a 6c 7f 5e 0f 72   .c~.aJ.|O.*l.^.r
    0040 - 42 e2 af 6c 77 66 5a b6-cd 31 4a ab 69 2d cb 35   B..lwfZ..1J.i-.5
    0050 - dc fe 89 a5 81 da 11 91-0d e0 32 c1 b6 3e 7b ea   ..........2..>{.
    0060 - ee a8 45 4c 34 5c 23 5f-bf 8a 5c 5b fd ae d5 1e   ..EL4\#_..\[....
    0070 - 40 77 e1 98 36 60 8a ee-bf 74 1d 26 c0 70 f0 c9   @w..6`...t.&.p..
    0080 - 8e a9 f3 d5 fb 68 65 0e-47 6c 94 63 e0 3c 49 0e   .....he.Gl.c.<I.
    0090 - 5e d9 2c 97 42 e0 d5 d6-38 f6 cd 89 2b 96 d3 be   ^.,.B...8...+...
    00a0 - 3c 4f 16 ad 4c b1 4b 07-02 c2 4f 92 2f af c6 22   <O..L.K...O./.."
    00b0 - 09 d8 93 74 58 25 20 0f-1e c2 00 e5 64 ab 9c 64   ...tX% .....d..d
    00c0 - 04 47 7f fc c4 38 9f e9-3a 28 5a cb 0e 41 7e 34   .G...8..:(Z..A~4
    00d0 - cc f8 47 04 5e b6 7e c2-b0 da 56 e8 62 0d 94 74   ..G.^.~...V.b..t

    Start Time: 1777198028
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
    Session-ID: 42632AFBB14305F65C67B3C04501EF0E6F9AEB485D0A9E623176DAC8566C2F10
    Session-ID-ctx:
    Resumption PSK: 0D161D88F401425A2B757C4A7C9D25D7E0D0C9B47FCA47B1760A8FD598D0894E3B5A3EF227C84B3666BA54410ECEB1F7
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - be 50 7c b5 aa 41 0f 14-53 95 2f 13 42 bd 46 cf   .P|..A..S./.B.F.
    0010 - 90 f4 aa 5b 82 8c 40 fa-0f b9 94 1e f7 9c d3 92   ...[..@.........
    0020 - 93 27 c0 2c b2 ca 7e 54-12 e7 c7 a7 f1 83 e8 52   .'.,..~T.......R
    0030 - da 72 92 18 1f b1 9c a3-e6 6c 9e f8 08 6d 10 d0   .r.......l...m..
    0040 - 7f 3e 76 57 69 c0 af 84-aa b4 e9 1c 8a d2 a9 3f   .>vWi..........?
    0050 - 93 34 22 9e c5 ff e9 a6-30 c1 2f 0d 83 fe 48 b9   .4".....0./...H.
    0060 - 02 01 4f 53 d3 67 d8 e9-fd 07 97 72 ec 93 31 73   ..OS.g.....r..1s
    0070 - 86 c3 a8 b9 92 0c cf c5-6e 29 a4 85 d9 a4 9f 42   ........n).....B
    0080 - 7f af 33 e3 de 31 cb e6-99 38 29 5b 39 93 48 0e   ..3..1...8)[9.H.
    0090 - d7 f3 67 b7 da b8 ca e1-a9 34 65 f3 3d 13 fd f1   ..g......4e.=...
    00a0 - 2e f3 53 27 61 13 d7 d2-e9 c7 7f ed 8b d3 71 7e   ..S'a.........q~
    00b0 - 27 87 4d 5f ab 54 df 57-88 28 c5 27 c8 72 2c ea   '.M_.T.W.(.'.r,.
    00c0 - 2a 9e df 8d b0 6f 2c 9a-cb f6 b0 23 fb 0a de 61   *....o,....#...a
    00d0 - f1 a4 9a 2c d0 d8 48 f9-1a 58 ad 60 c2 c3 e3 32   ...,..H..X.`...2

    Start Time: 1777198028
    Timeout   : 7200 (sec)
    Verify return code: 18 (self-signed certificate)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
```

---

## pieni yhteenveto koskien ratkaisuista

Tässä tasossa piti käyttää `ncat` komentoa, joka on monipuolinen ja nykyaikainen komentorivityökalu verkkodatansiirtoon, jota käytetään tiedon lukemiseen, kirjoittamiseen ja uudelleenohjaamiseen verkoissa (TCP/UDP/SCTP/SSL). Nmap-projektin kehittämä työkalu toimii sekä asiakkaana että palvelimena, ja se on tehokas apuväline verkon diagnosointiin, porttiskannaukseen, tiedostonsiirtoon ja etähallintaan (remote shell). Sillä onkin kätevä testata vaikkapa palomuurin tai pakettisuodattimen toimintaa.

Tässä olikin vaihtoehtoisia komentoja, että jokainen toimii hyvinkin 

```
echo "Password of Bandit15" | ncat --ssl <host> <port>
echo "Password of Bandit15" | openssl s_client -quiet -connect <host:port>
echo "Password of Bandith15" | ncat --ssl localhost 30001
```

Eli komennossa pitää syöttää mukanaan tämän tason bandit15 salasana niin se antaa sitten seuraavan tason Bandit16 salasana. Jos ilman Bandit15 salasanaa, niin sekin voi toimia, mutta joutuu liittää sen jälki käteen sitten.

Esim. tässä avataan openssl _client yhteys localhostiin portti numeroonsa ja vasta jälkikäteen syötettään Bandit15 tason salasana.
```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
...
....
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
```

## linkkejä ja ohjeita:

- https://mayadevbe.me/posts/overthewire/bandit/level16/
- https://medium.com/secttp/overthewire-bandit-level-15-223a2f5a940d
- https://medium.com/@pophacker996/overthewire-bandit-level-15-25-solutions-7e9b7121aa47
