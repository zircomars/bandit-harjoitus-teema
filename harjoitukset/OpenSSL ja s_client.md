# OpenSSL ja s_client - perusidea

1. Mikä on OpenSSL?

- OpenSSL on työkalu ja kirjasto SSL/TLS-toimintoihin
- käytetään:
  - sertifikaattien luontiin
  - avainten hallintaan
  - yhteyksien testaamiseen

- toimii Linuxissa ja Windowsissa


2. Mikä on s_client?

- s_client on OpenSSL:n alikomento (subcommand)
- sitä käytetään TLS-yhteyksien testaamiseen

- komento:
  openssl s_client -connect host:port

- esimerkki:
  openssl s_client -connect localhost:30001


3. Miten ne liittyvät toisiinsa?

- openssl = päätyökalu (työkalupakki)
- s_client = yksi sen ominaisuus (työkalun pakin sisällä)

- muita openssl-komentoja:
  - openssl genrsa (avaimen luonti)
  - openssl req (CSR)
  - openssl x509 (sertifikaatti)


4. Mitä s_client tekee?

- muodostaa TLS-yhteyden palvelimeen
- näyttää:
  - sertifikaatin
  - salausasetukset
  - handshake-tiedot

- mahdollistaa:
  - datan lähettämisen palvelimelle
  - debuggaamisen


5. Käyttö esimerkki

- yhdistä:
  openssl s_client -connect localhost:30001

- lähetä data:
  echo "testi" | openssl s_client -connect localhost:30001

- hiljaisempi tila:
  openssl s_client -quiet -connect localhost:30001


6. Milloin käytetään?

- TLS-yhteyden testaus
- sertifikaatin tarkistus
- tietoturvatestaus
- debuggaus (miksi yhteys ei toimi)


7. Yhteenveto

- OpenSSL = työkalu SSL/TLS:lle
- s_client = osa OpenSSL:ää
- käytetään yhteyksien testaamiseen


8. Miksi komentoja yhdistetään (pipe) ja mitä jos ei yhdistä?

- komentoja yhdistetään pipe-merkillä (|)
- pipe siirtää ensimmäisen komennon tulosteen toisen syötteeksi

- esimerkki:
  cat tiedosto.txt | openssl s_client -connect localhost:30001

- mitä tapahtuu:
  - cat lukee tiedoston
  - pipe siirtää sisällön
  - openssl lähettää sen TLS-yhteyden kautta


Miksi tämä on hyödyllistä?

- automaatio
  - ei tarvitse kirjoittaa käsin

- nopeus
  - toimii yhdellä komennolla

- skriptit
  - helppo käyttää bashissa


Jos EI käytä pipea:

- komento:
  openssl s_client -connect localhost:30001

- mitä tapahtuu:
  - yhteys avautuu
  - sinun pitää kirjoittaa syöte käsin

- esimerkki:
  kirjoitat salasanan itse ja painat Enter


Erot käytännössä:

Pipe:
- automaattinen
- nopea
- käytetään tehtävissä ja skripteissä

Ilman pipea:
- manuaalinen
- hyvä testaukseen
- näet mitä tapahtuu


Yhteenveto:

- pipe = yhdistää komennot
- ilman pipea = manuaalinen syöttö
- molemmat toimivat, mutta eri käyttötarkoituksiin

