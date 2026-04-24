# SSH/OpenSSH/Keys

SSH-avaimet (OpenSSH keys) ovat salattuun tietoliikenteeseen tarkoitettuja menetelmiä, joilla etäpalvelimelle voi kirjautua turvallisesti ilman salasanaa. Ne perustuvat avainpariin, joka koostuu julkisesta ja yksityisestä avaimesta


Mikä on SSH-avain?
- Julkinen avain (Public key): Sijoitetaan etäpalvelimelle (esim. `~/.ssh/authorized_keys` -tiedostoon). Se on turvallista jakaa, eikä sitä tarvitse salata.
- Yksityinen avain (Private key): Pidetään omalla koneella suojattuna. Tätä käytetään tunnistaumiseen kirjautuessa.
- Hyöty: Mahdollistaa salasanattoman kirjautumisen, mikä parantaa turvallisuutta ja helpottaa automatisointia (skriptit).

SSH-avaimen säilytyksessä tärkein sääntö on: yksityinen avain on nimensä mukaisesti yksityinen. Jos joku saa sen käsiinsä, hän on käytännössä sinä (digitaalisessa mielessä).

Paras menetelmä: Suojaus ja hallinta
1. Käytä salasanaa (Passphrase): Kun luot avaimen (ssh-keygen), aseta sille aina vahva salasana. Vaikka joku varastaisi avaintiedoston, hän ei voi käyttää sitä ilman salasanaa.
2. SSH Agent: Käytä ohjelmaa nimeltä ssh-agent. Se pitää avaimen muistissa avattuna, jotta sinun ei tarvitse kirjoittaa salasanaa joka kerta uudestaan, mutta avain pysyy silti suojattuna levyllä.

3. Laitteistoavaimet (Hardware Security Key): Kaikkein turvallisin tapa on käyttää esimerkiksi YubiKeytä. Tällöin yksityinen avain on fyysisellä tikulla, eikä sitä voi kopioida pois sieltä.

Missä ja miten säilyttää (Hyvät paikat)
- Oletuskansio: .ssh/-kansio omalla koneellasi (Linux/Mac) tai Windowsin käyttäjäkansiossa. Varmista, että kansion oikeudet ovat tiukat (vain sinulla on luku/kirjoitusoikeus).
- Salasanojen hallintaohjelma: Esimerkiksi Bitwarden tai 1Password. Voit tallentaa avaimen tekstinä turvalliseen muistiinpanoon. Tämä on hyvä tapa pitää varmuuskopiota.


<br> 

- 🔐 SSH-avaimen säilytys – Hyvät vs Huonot käytännöt

| Sijainti | Hyvä / Huono | Miksi | Riskit | Suositus / Ohje |
|----------|-------------|------|--------|-----------------|
| Oma kone (`~/.ssh` tai `C:\Users\<user>\.ssh`) | ✅ Hyvä | Oletuspaikka, toimii suoraan SSH:n kanssa | Koneen hajoaminen → avain katoaa | Käytä tätä pääpaikkana + tee varmuuskopio |
| Salattu varmuuskopio (USB / password manager / salattu tiedosto) | ✅ Hyvä | Mahdollistaa palautuksen turvallisesti | Heikko salasana → murto mahdollinen | Käytä vahvaa salasanaa + säilytä erillään koneesta |
| Pilvipalvelu (salattuna) | ⚠️ Kohtalainen | Helppo saatavuus ja synkronointi | Vuoto jos ei salattu | Salaa tiedosto ennen uploadia |
| SSH-agent | ✅ Hyvä | Vähentää avaimen suoraa käyttöä | Väärin konfiguroituna riskejä | Käytä yhdessä passphrasen kanssa |
| Pilvipalvelu (ilman salausta) | ❌ Huono | Helppo mutta turvaton | Avain voi vuotaa suoraan | Älä koskaan säilytä plaintext-muodossa |
| Git repository (esim. GitHub) | ❌ Erittäin huono | Yleinen virhe | Avain leviää sekunneissa julkiseksi | Älä koskaan lisää repoihin |
| Sähköposti / Slack | ❌ Huono | Helppo jakaa | Tallentuu palvelimille → vuotoriski | Älä lähetä yksityisiä avaimia |
| Julkinen kone | ❌ Erittäin huono | Ei kontrollia ympäristöstä | Avaimen kopiointi / keylogger | Älä koskaan käytä SSH-avainta julkisella koneella |

- 🧠 Tietoturvaperiaate
> SSH-yksityinen avain on kuin henkilöllisyys: jos joku saa sen haltuunsa, hän voi esiintyä sinuna.



Avainten hallinta Windowsissa (Putty)
- Jos käytät PuTTY-ohjelmaa, avaimet luodaan PuTTYgen-työkalulla, jolla julkisen avaimen voi muuntaa oikeaan muotoon palvelimelle tallentamista varten. 


Lisätietoa ja lukemista:
- https://sshkeygenerator.com/fi/what-is-an-ssh-key/
- https://help.ubuntu.com/community/SSH/OpenSSH/Keys
- https://www.kapsi.fi/ohjeet/ssh-avain.html
- https://www.cs.helsinki.fi/group/kuje/compfac/ssh_avain.html
---

## 🔐 SSH-avaimen varmuuskopiointi – Nopea ohje

- Tee varmuuskopio **heti kun avain luodaan**
- Säilytä vähintään **2 kopiota** (aktiivinen + varmuuskopio)
- Pidä varmuuskopio **salattuna**
- Älä säilytä avainta koskaan **selkokielisenä pilvessä**
- Jos avain katoaa ilman backupia → joudut luomaan uuden ja päivittämään kaikkiin palveluihin


📊 SSH-avaimen säilytys ja varmuuskopiointi

| Tilanne | Toimenpide | Miksi | Riski jos ei tehdä | Suositus |
|--------|-----------|------|-------------------|----------|
| Uusi SSH-avain luotu | Tee varmuuskopio heti | Estää avaimen katoamisen | Avain menetetään pysyvästi | Tee backup heti |
| Päivittäinen käyttö | Säilytä koneella (`.ssh`) | Helppo ja toimiva | Ei backupia → menetysriski | Käytä oletussijaintia |
| Varmuuskopiointi | Tallenna salattuna (USB / password manager / salattu tiedosto) | Suojaa avainta | Vuoto jos salaamaton | Käytä vahvaa salausta |
| Pilvitallennus | Vain salattuna | Saatavuus usealla laitteella | Murto → avain vuotaa | Salaa ennen uploadia |
| Avaimen katoaminen | Palauta varmuuskopiosta | Nopea palautus | Ilman backupia kaikki uusiksi | Testaa palautus etukäteen |
| Avaimen vuoto | Luo uusi avain ja poista vanha | Estää väärinkäytön | Hyökkääjä pääsee sisään | Reagoi heti |

## 🔐 SSH-avaimen oikeudet (OpenSSH)

- Yksityinen avain pitää olla **vain omistajan luettavissa**
- Jos muilla on pääsy → SSH hylkää avaimen

📊 Oikeudet eri ympäristöissä

| Ympäristö | Tiedosto | Komento | Mitä tekee | Miksi tärkeä |
|----------|---------|--------|------------|--------------|
| Linux / macOS | Yksityinen avain | `chmod 600 id_rsa` | Vain omistaja voi lukea ja kirjoittaa | Estää muita lukemasta avainta |
| Linux / macOS | `.ssh` kansio | `chmod 700 ~/.ssh` | Vain omistajalla pääsy kansioon | Suojaa avaimet hakemistotasolla |
| Windows (PowerShell) | Yksityinen avain | `icacls key /inheritance:r` | Poistaa perityt oikeudet | Estää muiden pääsyn |
| Windows (PowerShell) | Yksityinen avain | `icacls key /grant:r "<user>:(R)"` | Antaa vain käyttäjälle lukuoikeuden | Vastaa `chmod 600` |
| Windows (PowerShell) | `.ssh` kansio | `icacls .ssh /inheritance:r` + grant | Rajoittaa kansion pääsyn | Suojaa avaimet |

## ⚠️ Yleisimmät virheet

| Virhe | Mitä tapahtuu | Korjaus |
|------|--------------|--------|
| Liian avoimet oikeudet | SSH error: "Permissions are too open" | Rajoita oikeudet (`chmod 600` / `icacls`) |
| Tiedosto kaikkien luettavissa | Avain voi vuotaa | Poista muut käyttäjät oikeuksista |
| Väärä käyttäjä | SSH ei käytä avainta | Varmista omistajuus |
| `.ssh` kansio avoin | Epäsuora tietoturvariski | `chmod 700` |

- Linux/macOS:

```
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/id_rsa
```

- windows powershell:
- <username> tulee siitä kun aktoit omaa powershell terminaalinsa se tulee siitä `PS C:\Users\<USERNAME>`.

```
icacls sshkey.private /inheritance:r
icacls sshkey.private /grant:r "<USERNAME>:(R)"
```
