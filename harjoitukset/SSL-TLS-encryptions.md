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

## SSL/TLS sertifikaatin luonti (Linux vs PowerShell)

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

---

## SSL/TLS sertifikaatin luonti (Linux vs PowerShell)

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

