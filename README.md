# bandit-harjoitus-teema
pieni harjoittelu, perus linux komentoja ja järjestelmän perusominaisuutta. Tämä "overthewire" sivuston alta löytyy muita harjoituspelejä, että taso alkaa 0 (nollasta) liikeelle ja siitä menee vaikeammaksi. 

- **Tarkoitus:** Bandit on tarkoitettu aloittelijoille ja se keskittyy komentorivikäytön, tiedostojärjestelmän ja perus-hakkeroinnin opetteluun.
- **Teema:** Bandit on parhaiten tunnettu sen simppelistä ja suoraviivaisesta lähestymistavastaan. Haasteet käsittelevät pääasiassa oikeuksia, tiedostoja, hakemistoja ja salasanan murtamista.

![alt text](images/Bandit-page.png)

## harjoitus linkki:
https://overthewire.org/wargames/bandit/ <br>
https://overthewire.org/wargames/ (sama linkki, mutta etusivu)

## Miten se toimii ja mitä tästä oppii?

- Kirjaudut etäpalvelimelle (SSH-yhteydellä)
- Saat tehtävän jokaisella tasolla
- Ratkaiset tehtävän Linux-komennoilla
- Saat salasanan seuraavalle tasolle

Esimerkki yhteydestä: `$ssh bandit0@bandit.labs.overthewire.org -p 2220`

- Peruskomennot (ls, cd, cat)
- Tiedostojen käsittely
- Piilotiedostot
- Käyttöoikeudet (permissions)
- Prosessit
- Verkkoperusteet
- Salaus ja dekoodaus


## SSH
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



