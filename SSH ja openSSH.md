# SSH
SSH (secure shell) on salattuun tietoliikenteeseen tarkoitettu protokolla.
- Yleisin SSH:n käyttötapa on ottaa etäyhteys SSH-asiakasohjelmalla SSH-palvelimeen, jotta voidaan käyttää toista tietokonetta komentorivin (terminaalin) kautta.
- SSH:lla voidaan myös suojata muuta verkkoliikennettä, kuten FTP- ja HTTP-yhteyksiä.
- Rlogin, telnet, rsh, rcp ja rdist suositellaan korvattavaksi SSH:lla, koska näiden yhteydenottotapojen suojaustaso on varsin heikko.
- SSH on turvallinen tapa kirjautua etäkoneeseen ja käyttää sitä komentoriviltä.
- SSH käyttää oletuksena porttia 22

SSH:sta on olemassa kaksi versiota: SSH1 ja SSH2.

SSH1 on vanhempi versio, jossa on tietoturvaongelmia ja heikompi salaus. 
Sitä ei enää suositella käytettäväksi.

SSH2 on uudempi ja turvallisempi versio. 
Se käyttää vahvempaa salausta ja suojaa yhteyden paremmin.

SSH2:ta käytetään, koska se on turvallinen ja SSH1 on vanhentunut eikä sitä käytetä enää.

```
┌──(kali㉿kali)-[~]
└─$ ssh  
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface] [-b bind_address]
           [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
           [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file]
           [-J destination] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-P tag] [-p port] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           destination [command [argument ...]]
       ssh [-Q query_option]
```

For example: `$ssh bandit0@bandit.labs.overthewire.org -p 2220`


SSH asetetaan yleensä palvelimille, reitittimiin ja muihin verkkolaitteisiin:
- palvelimet (Linux / Windows serverit)
- reitittimet
- kytkimet
- palomuurit

Palomuuri ei sisällä SSH:ta, mutta se voi:
- sallia SSH-liikenteen (portti 22)
- estää SSH-liikenteen

SSH on etähallintaprotokolla, jota käytetään palvelimissa ja verkkolaitteissa. Se ei ole osa palomuuria, mutta palomuuri voi sallia tai estää sen käytön. SSH on tärkeä työkalu laitteiden turvalliseen etähallintaan.

SSH-yhteydessä voi olla aikakatkaisu (timeout).

Jos käyttäjä ei tee mitään hetkeen:
- yhteys suljetaan automaattisesti
- käyttäjä kirjataan ulos

Tämä on tietoturvaominaisuus.

Aikakatkaisu voidaan määrittää:
- palvelimella (server)
- tai asiakasohjelmassa (client)

## openssh

OpenSSH on ohjelmistopaketti, joka toteuttaa SSH-yhteydet (Secure Shell).

OpenSSH sisältää:
- SSH-asiakasohjelman (client) → kirjaudut toisiin koneisiin
- SSH-palvelimen (server) → muut voivat kirjautua sinun koneeseen

Miksi se on Windows 10/11:ssä valmiina?
- Microsoft lisäsi OpenSSH:n Windowsiin koska:
    - 🔧 Linux ja palvelimet käyttävät SSH:ta vakiona
    - 🌐 IT ja kehitys tarvitsee etähallintaa
    - 💻 Windowsista tuli “serveri- ja dev-ystävällisempi”
    - 🔐 haluttiin turvallinen standardi korvaamaan vanhat etätyökalut (kuten telnet)


PowerShell toimii vain käyttöliittymänä (terminaalina) eli cmd, ja OpenSSH taas on se ohjelma, joka oikeasti tekee yhteyden.
- Esim. powershell kautta ottaa yhteyttä SSH:lla `$ssh username@server`


SSH = protokolla (tapa / sääntö miten yhteys toimii) 
OpenSSH = ohjelma (työkalu joka käyttää SSH:ta)

SSH kertoo, miten turvallinen yhteys muodostetaan. OpenSSH on käytännön ohjelmisto, jolla SSH-yhteys toteutetaan. SSH-ohjelmia on olemassa useita, kuten PuTTY ja OpenSSH. Windows PowerShell käyttää OpenSSH:ta SSH-yhteyksien muodostamiseen, minkä vuoksi SSH toimii suoraan PowerShellissä.

OpenSSH:ssa on aikakatkaisu (timeout).

Jos yhteys on pitkään käyttämättä:
- yhteys voidaan katkaista automaattisesti

Aikakatkaisu voidaan määrittää:
- palvelimella (server)
- tai asiakasohjelmassa (client)

Tämä parantaa tietoturvaa.
