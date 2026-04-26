# SSL/TLS, sertifikaatit ja tietoturva

<img width="720" height="537" alt="image" src="https://github.com/user-attachments/assets/d36f1b35-eb5e-4845-81a7-81b2de4a305e" />

1. Mikä on SSL/TLS encryption?

- SSL/TLS (Secure Sockets Layer / Transport Layer Security) on salausprotokolla
- sitä käytetään suojaamaan verkkoliikennettä
- salaa yhteyden kahden osapuolen välillä (esim. selain ja palvelin)
- estää ulkopuolisia lukemasta tai muuttamasta dataa

- HTTP (portti 80) = ei salausta
- HTTPS (portti 443) = TLS-suojattu

- TLS on uudempi ja turvallisempi versio (SSL on vanhentunut)

2. Mikä on X.509-sertifikaatti?

- digitaalinen “henkilöllisyystodistus” palvelimelle
- käytetään TLS-yhteyksissä

- sisältää:
  - palvelimen nimen (domain)
  - julkisen avaimen (public key)
  - myöntäjän (CA, Certificate Authority)
  - voimassaoloajan

- muoto:
  -----BEGIN CERTIFICATE-----
  ...
  -----END CERTIFICATE-----

3. Mitä hyötyä TLS:stä ja sertifikaateista on?

- luottamuksellisuus
  - data on salattua
- eheys
  - dataa ei voi muuttaa huomaamatta
- autentikointi
  - varmistaa että palvelin on oikea
- turvallinen verkkoliikenne
  - perusta lähes kaikelle internet-liikenteelle

4. Riskit ja heikkoudet

- väärin konfiguroitu TLS
  - heikot salausasetukset
- vanhentuneet sertifikaatit
  - yhteys ei ole luotettava
- itse allekirjoitetut sertifikaatit
  - ei automaattista luottamusta
- man-in-the-middle -hyökkäykset
  - jos sertifikaattia ei tarkisteta
- private key vuotaa
  - suojaus rikkoutuu

5. Käyttötarkoitukset

- HTTPS (verkkosivut)
- sähköposti (IMAPS, SMTPS)
- VPN-yhteydet
- API-liikenne
- palvelinten välinen kommunikointi
- tietoturvatestaus (openssl, ncat jne.)

6. Sertifikaattien säilyttäminen

- private key pitää suojata
- käyttöoikeudet rajattava
- ei versionhallintaan (Git jne.)
- säilytys:
  - palvelin
  - HSM (Hardware Security Module)
  - salattu key vault

7. Varmuuskopiointi (kvartaali)

- varmuuskopioi:
  - private key
  - sertifikaatti
  - CA-chain

- säilytä:
  - salattuna
  - eri sijainnissa

- testaa palautus (restore)

- aikataulu:
  - 3 kk välein
  - tai muutosten yhteydessä
 
Sertifikaattien ja erityisesti private keyn varmuuskopiointi tulisi tehdä huolellisesti ja säilyttää turvallisessa paikassa, joka on erillään alkuperäisestä järjestelmästä. Järkevin ratkaisu on käyttää useampaa sijaintia: esimerkiksi yksi salattu varmuuskopio paikallisesti (offline tai rajoitetussa palvelimessa) ja toinen pilvessä turvallisessa key vault- tai salatussa tallennuspalvelussa. Tärkeintä on, että private key ei ole julkisesti saatavilla, ja että pääsy siihen on rajattu vain tarvittaville käyttäjille. Pilvessä säilyttäessä tulee varmistaa vahva tunnistautuminen (esim. MFA) ja salaus, kun taas paikallisessa säilytyksessä tulee huolehtia fyysisestä turvallisuudesta ja käyttöoikeuksista. 

Lisäksi varmuuskopioiden toimivuus tulee testata säännöllisesti (restore-testi), jotta tiedetään niiden oikeasti toimivan. Paras käytäntö on yhdistää sekä paikallinen että pilvipohjainen säilytys, jolloin saadaan suojaa sekä teknisiä vikoja että tietoturvauhkiakin vastaan.

Varmuuskopiointi - säilytys vaihtoehdot

Sijainti              | Kuvaus                              | Hyödyt                         | Riskit
---------------------|-------------------------------------|--------------------------------|-----------------------------
Paikallinen (offline)| USB, ulkoinen levy, offline media   | Ei verkossa, vaikea hakata     | Fyysinen katoaminen, rikkoutuminen
Palvelin (local)     | Sisäinen varmuuskopio palvelimella  | Nopea palautus                 | Jos palvelin kaatuu, backup voi mennä
Pilvi (cloud)        | Salattu cloud storage / key vault   | Skaalautuva, varma            | Väärä konfiguraatio, pääsyn hallinta
HSM / Key Vault      | Erillinen turvalaitteisto/palvelu   | Erittäin turvallinen           | Monimutkaisempi, kustannukset


8. Tietoturva ja kyberturvallisuus

- TLS suojaa:
  - salasanat
  - maksutiedot
  - liikenteen

- estää:
  - salakuuntelun (sniffing)

- hyvät käytännöt:
  - käytä TLS 1.2 tai 1.3
  - poista vanhat (SSL, TLS 1.0, 1.1)
  - käytä luotettavia CA:ita
  - uusi sertifikaatit ajoissa
  - monitoroi yhteyksiä

9. Yhteenveto

- TLS = salaa verkkoliikenne
- X.509 = todistaa identiteetin
- sertifikaatti ei ole vastaus vaan suojausmekanismi
- tärkeintä:
  - oikea konfiguraatio
  - avainten suojaus
  - sertifikaattien hallinta
---

## SSL/TLS sertifikaatin luonti 

SSL/TLS-sertifikaatin luonti voi tapahtua sekä Linux- että PowerShell-ympäristössä, eli se toimii molemmissa järjestelmissä samalla perusperiaatteella. Prosessissa luodaan ensin yksityinen avain (private key), jonka jälkeen tehdään varmennepyyntö (CSR) ja lopuksi sertifikaatti joko itse allekirjoitettuna tai varmentajan (CA) kautta. Tämä muistuttaa osittain SSH-avaimen luontia, koska molemmissa käytetään avainpareja ja ne täytyy tallentaa turvallisesti, mutta käyttötarkoitus on eri: SSH-avaimia käytetään tunnistautumiseen, kun taas SSL/TLS-sertifikaatteja käytetään yhteyden salaamiseen ja palvelimen identiteetin varmistamiseen. Tärkeintä on, että yksityinen avain säilytetään turvallisesti, käyttöoikeudet rajataan oikein eikä sitä koskaan jaeta.

1. Yleinen periaate

- SSL/TLS-sertifikaatti perustuu avainpariin:
  - private key (yksityinen avain)
  - public key (julkinen avain)

- prosessi:
  - luodaan private key
  - luodaan CSR (Certificate Signing Request)
  - allekirjoitetaan (CA tai self-signed)

- toimii samalla logiikalla kaikissa käyttöjärjestelmissä

2. Linux (yleisin tapa)

- käytetään yleensä OpenSSL-työkalua

- private key:
  openssl genrsa -out private.key 2048

- CSR:
  openssl req -new -key private.key -out request.csr

- self-signed sertifikaatti:
  openssl req -x509 -key private.key -out cert.pem -days 365

- yleistä:
  - käytössä palvelimilla (web, nginx, apache)
  - automaatio (scriptit, DevOps)
  - standardi tapa internetissä

3. Windows / PowerShell

- käytetään Windowsin omia työkaluja

- self-signed sertifikaatti:
  New-SelfSignedCertificate -DnsName "example.com" -CertStoreLocation "Cert:\LocalMachine\My"

- sertifikaatit tallennetaan:
  - Windows Certificate Store

- yleistä:
  - käytössä yritysympäristöissä (Active Directory)
  - helppo integroida Windows-palveluihin (IIS, RDP)

4. Kumpi on yleisempi?

- Linux + OpenSSL:
  - yleisin internet-palvelimissa
  - standardi DevOps-ympäristöissä

- PowerShell:
  - yleinen Windows-yritysympäristöissä
  - helpompi GUI/AD-integraatio

5. Hyvät käytännöt

- private key:
  - ei koskaan jaeta
  - rajoitetut käyttöoikeudet

- käytä vahvaa avainta:
  - RSA 2048 tai enemmän
  - tai ECC

- sertifikaatin voimassaolo:
  - ei liian pitkä
  - uusitaan ajoissa

6. Riskit

- private key vuotaa
  - koko TLS-suojaus rikkoutuu

- väärä konfiguraatio
  - heikko salaus

- self-signed tuotannossa
  - ei luotettava

7. Yhteenveto

- SSL/TLS toimii samalla periaatteella kaikissa järjestelmissä
- Linux (OpenSSL) = yleisin tapa internetissä
- PowerShell = yleinen Windows-ympäristössä
- tärkeintä:
  - avainten suojaus
  - oikea konfiguraatio


## Elinkaari

SSL/TLS-sertifikaateilla on elinkaari, joka alkaa avainparin (private key ja public key) luonnista ja päättyy sertifikaatin vanhenemiseen tai mitätöintiin. Prosessiin kuuluu sertifikaatin luonti, allekirjoitus varmentajalta (CA), käyttöönotto palvelimella, aktiivinen käyttö sekä säännöllinen valvonta ja uusiminen ennen voimassaolon päättymistä. Sertifikaatit eivät ole ikuisia, vaan ne ovat voimassa rajatun ajan (yleensä noin 90 päivästä yhteen vuoteen), minkä jälkeen ne täytyy uusia. Tarvittaessa sertifikaatti voidaan myös peruuttaa (revocation), esimerkiksi jos private key vuotaa. Hyvässä tietoturvakäytännössä sertifikaattien elinkaarta hallitaan aktiivisesti, jotta vältetään käyttökatkot ja turvallisuusongelmat.

SSL/TLS sertifikaatin elinkaari

Vaihe                | Kuvaus
---------------------|-----------------------------------------
Luonti (Creation)    | Luodaan private key ja CSR
Allekirjoitus (CA)   | CA allekirjoittaa sertifikaatin
Asennus (Deployment) | Sertifikaatti asennetaan palvelimelle
Käyttö (Usage)       | TLS-yhteydet toimivat normaalisti
Valvonta (Monitoring)| Tarkistetaan voimassaolo ja toiminta
Uusiminen (Renewal)  | Sertifikaatti uusitaan ennen vanhenemista
Peruutus (Revocation)| Sertifikaatti mitätöidään tarvittaessa
Vanhentuminen        | Sertifikaatti ei ole enää voimassa

Tärkeät huomiot

- sertifikaatti ei ole ikuinen
- uusiminen pitää tehdä ajoissa
- private keyn vuoto = sertifikaatti pitää peruuttaa
- automatisointi (esim. cron, scripts) suositeltavaa

### SSL/TLS - elinkaari, kustannukset ja vanheneminen (FAQ)

1. Kannattaako käyttää samaa sertifikaattia pitkään?

- ei yleensä
- sertifikaatteja ei ole tarkoitettu pysyvään käyttöön
- suositus:
  - lyhyt voimassaoloaika
  - säännöllinen uusiminen

- hyödyt:
  - pienempi riski jos private key vuotaa
  - parempi tietoturvan ylläpito


2. Onko SSL/TLS maksullinen?

- on sekä ilmaisia että maksullisia vaihtoehtoja

Ilmainen:
- Let's Encrypt
- yleensä 90 päivän voimassaolo
- automaattinen uusiminen mahdollista

Maksullinen:
- DigiCert, GlobalSign jne.
- yrityskäyttöön
- lisäominaisuuksia (esim. laajempi validointi)

- käytännössä:
  - suurin osa sivuista käyttää ilmaista sertifikaattia


3. Mitä tapahtuu jos sertifikaatti vanhenee?

- selain näyttää varoituksen (ei suojattu yhteys)
- käyttäjät eivät voi luottaa sivuun
- osa palveluista voi lakata toimimasta
- API-yhteydet voivat rikkoutua

- lopputulos:
  - palvelu voi olla käytännössä käyttökelvoton


4. Tuleeko siitä kalliimpaa jos unohtaa uusia?

- ei suoraa rahallista sakkoa
- mutta epäsuorat kustannukset:

- palvelukatko (downtime)
- asiakkaiden menetys
- mainehaitta
- kiireellinen korjaustyö


5. Mitä pitäisi tehdä käytännössä?

- automatisoi uusiminen (esim. certbot + cron)
- käytä hälytyksiä ennen vanhenemista (30 päivää ennen)
- älä odota viimeiseen päivään
- testaa uusiminen ennen tuotantoa


6. FAQ

Q: Voiko samaa sertifikaattia käyttää uudelleen?
- ei suositella, aina uusitaan

Q: Vaihtuuko private key aina?
- suositus: kyllä, mutta ei pakollinen

Q: Mitä jos private key vuotaa?
- sertifikaatti pitää peruuttaa ja luoda uusi

Q: Voiko vanhentuneen sertifikaatin korjata?
- ei, pitää luoda uusi

Q: Onko ilmainen riittävä?
- kyllä suurimmassa osassa tapauksista


7. Yhteenveto

- SSL/TLS ei ole kertaluontoinen asia
- se on jatkuva ylläpito ja hallinta
- tärkeintä:
  - uusiminen ajoissa
  - automaatio
  - private keyn suojaus
 
## SSL/TLS - private key säilyttäminen ja vuotaminen

1. Private keyn säilyttäminen

- private key on tärkein osa koko TLS-suojausta
- jos se vuotaa, koko suojaus on käytännössä murrettu

- säilytys:
  - vain palvelimella jossa sitä tarvitaan
  - ei jaeta muille
  - ei versionhallintaan (Git jne.)

- käyttöoikeudet:
  - rajattu vain tarvittaville käyttäjille
  - esim. Linux:
    chmod 600 private.key

- turvallinen säilytys:
  - salattu levy tai tiedosto
  - key vault / secret manager
  - HSM (Hardware Security Module)

- varmuuskopio:
  - salattuna
  - eri sijainnissa
  - ei julkisesti saatavilla


2. Mitä tapahtuu jos private key vuotaa?

- hyökkääjä voi:
  - esiintyä palvelimena (impersonation)
  - purkaa liikennettä (jos muita heikkouksia)
  - tehdä man-in-the-middle -hyökkäyksiä

- seuraukset:
  - käyttäjien data voi vaarantua
  - luottamus menetetään
  - mahdollinen tietoturvaloukkaus


3. Mitä tehdä jos vuoto tapahtuu?

- toimi heti:

  1. peruuta (revoke) sertifikaatti
  2. luo uusi private key
  3. luo uusi sertifikaatti
  4. ota uusi käyttöön palvelimella

- tarkista:
  - lokit (onko väärinkäyttöä)
  - mistä vuoto tapahtui


4. Miten estää vuoto?

- älä lähetä private keyta sähköpostilla
- älä tallenna suojaamattomana
- käytä vahvoja käyttöoikeuksia
- käytä salattuja varmuuskopioita
- rajoita pääsy (least privilege)

- lisäsuoja:
  - HSM tai key vault
  - MFA pääsyyn

5. Yhteenveto

- private key = tärkein osa TLS:ää
- sen vuoto = vakava tietoturvaongelma
- suojaus:
  - rajoitettu pääsy
  - salaus
  - turvallinen säilytys
- vuototilanne:
  - revoke + uusi key + uusi sertifikaatti heti

---

## lisätietoa ja lukemista:

- https://www.domainkeskus.com/ssl-sertifikaatit-mita-ne-ovat/?gad_source=1&gad_campaignid=21258741552&gbraid=0AAAAACbHuHiKZlMgwUOs10cXXmO2ZyuWK&gclid=EAIaIQobChMI-NW318eJlAMVCyGiAx3-RTMvEAAYAiAAEgL37_D_BwE
- https://www.domainhotelli.fi/ohjeet/kotisivut/ssl-sertifikaatti/








