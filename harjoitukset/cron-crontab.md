# Linux cron – perusteet, käyttö ja tietoturva

## Mikä on cron?

cron on Linuxin taustalla pyörivä palvelu (daemon), joka suorittaa komentoja automaattisesti tiettyinä ajankohtina.

Yksinkertaisesti:
cron = ajastettu tehtävien suorittaja


## Miksi cron on olemassa?

Monia asioita pitää tehdä automaattisesti:

- varmuuskopiot
- lokien siivous
- skriptien ajaminen
- järjestelmän ylläpito

Ilman cronia nämä pitäisi tehdä käsin.


## Mikä on crontab?

crontab on työkalu, jolla hallitaan cron-ajastuksia.

Tärkeimmät komennot:

crontab -e    = muokkaa ajastuksia
crontab -l    = näyttää ajastukset
crontab -r    = poistaa ajastukset


## Mikä on crontab(5)?

Komento:
man 5 crontab

Selitys:
- numero 5 tarkoittaa manuaalin osiota "file formats"
- tämä sivu kertoo, miten crontab-tiedosto kirjoitetaan

## Crontab syntaksi

Ajastusrivi näyttää tältä:

* * * * * komento

Jokainen tähti (*) tarkoittaa "mikä tahansa arvo" kyseisessä kentässä.

| Sijainti | Kenttä        | Mitä se tarkoittaa                  |
|----------|--------------|------------------------------------|
| 1        | Minuutti      | Millä minuutilla (0–59)            |
| 2        | Tunti         | Millä tunnilla (0–23)              |
| 3        | Päivä         | Kuukauden päivä (1–31)             |
| 4        | Kuukausi      | Kuukausi (1–12)                    |
| 5        | Viikonpäivä   | Päivä viikossa (0–7, su = 0 tai 7) |

### Mitä * tarkoittaa?

Tähti (*) = "joka"

Esimerkki:
* * * * * echo "hei"

Tämä tarkoittaa:
- joka minuutti
- joka tunti
- joka päivä
- joka kuukausi
- joka viikonpäivä

→ eli komento suoritetaan joka minuutti

---

### Esimerkkejä

| Ajastus        | Selitys                     |
|----------------|-----------------------------|
| * * * * *      | joka minuutti               |
| 0 * * * *      | joka tasatunti             |
| 0 0 * * *      | joka päivä klo 00:00       |
| 0 0 * * 0      | joka sunnuntai             |


## Missä cron-jobeja on?

- /etc/crontab          = koko järjestelmän ajastukset
- /etc/cron.d/          = erilliset cron-job tiedostot
- /var/spool/cron/      = käyttäjäkohtaiset ajastukset


## Esimerkki cron-jobista

* * * * * bandit22 /usr/bin/script.sh

Tämä tarkoittaa:
- ajetaan joka minuutti
- käyttäjänä bandit22
- suoritetaan skripti /usr/bin/script.sh


## Mitä cron tekee käytännössä?

1. cron käynnissä taustalla
2. lukee ajastukset
3. suorittaa komennot ajallaan
4. käyttää määriteltyä käyttäjää (esim. root tai bandit22)


## Tietoturva (erittäin tärkeä)

cron voi olla vaarallinen, jos sitä käytetään väärin.


### Yleisiä virheitä:

1. Salasanan vuotaminen

cat /salainen/tiedosto > /tmp/file

- /tmp on usein kaikkien luettavissa
- kuka tahansa voi lukea tiedoston


2. Liian löysät oikeudet

chmod 777 tiedosto

- kaikki voivat lukea ja muokata
- erittäin vaarallista


3. Skriptit ajetaan rootina

- jos skriptiä voi muokata
- hyökkääjä voi lisätä sinne omia komentoja
- cron ajaa ne root-oikeuksilla


## Hyvät käytännöt

- älä kirjoita salasanoja /tmp kansioon
- käytä turvallisia oikeuksia (esim. chmod 600)
- rajoita kuka voi muokata skriptejä
- tarkista cron-jobit säännöllisesti


## Kyberturvallisuus näkökulma

Hyökkääjä etsii:

- cron-jobeja jotka ajavat rootina
- tiedostoja joihin kirjoitetaan dataa
- skriptejä joita voi muokata


### Esimerkki hyökkäyksestä:

1. cron ajaa skriptiä korkeammilla oikeuksilla
2. skripti kirjoittaa tiedoston /tmp
3. tiedosto on kaikkien luettavissa
4. hyökkääjä lukee tiedon

= ei murtoa, vaan huonon suunnittelun hyödyntäminen


## Yhteenveto

- cron = ajastaa tehtäviä automaattisesti
- crontab = hallitsee ajastuksia
- crontab(5) = kertoo syntaksin
- tärkeä työkalu, mutta myös tietoturvariski


## Muistisääntö

"Jos cron tekee jotain puolestasi, varmista ettei se tee sitä myös hyökkääjän puolesta."

---

## lukemista ja ohjeita:

- https://www.linux.fi/wiki/Komentojen_ajastaminen
- https://tietokettu.net/knowledgebase/242/Cronin-asennus-ja-kayttotarkoitus..html
- https://fi.eitca.org/tietoverkkojen/eitc-on-lsa-linux--j%C3%A4rjestelm%C3%A4n-hallinta/Linuxin-sysadmin-teht%C3%A4viss%C3%A4-edistyminen/teht%C3%A4vien-ajoittaminen-cronilla/tentti-tarkistaa-aikatauluteht%C3%A4v%C3%A4t-cronilla/mik%C3%A4-on-crontab-ja-kuinka-voit-tarkastella-ja-luoda-sellaisen/
