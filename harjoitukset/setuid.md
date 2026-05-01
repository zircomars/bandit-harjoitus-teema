# setuid

Setuid (lyhenne sanoista _"set user ID"_) on Unix/Linux-järjestelmissä tiedosto-oikeuksiin liittyvä ominaisuus. Se tarkoittaa, että kun tietty ohjelma suoritetaan, se ei toimi suorittavan käyttäjän oikeuksilla, vaan tiedoston omistajan oikeuksilla.

Miten se toimii käytännössä?

Normaalisti:
- > ohjelma → toimii sen käyttäjän oikeuksilla, joka sen käynnistää

Setuid päällä:
- > ohjelma → toimii tiedoston omistajan oikeuksilla

Miksi sitä käytetään?
- mahdollistaa tavallisille käyttäjille rajatun pääsyn root-tason toimintoihin
- ilman että annetaan täydet sudo-oikeudet

Setuid voi olla vaarallinen:
- jos ohjelmassa on bugi → hyökkääjä voi saada root-oikeudet
- siksi setuid-ohjelmia pitää olla mahdollisimman vähän

Esim 1. Miltä se näyttää?

- Kun listaat tiedostoja: `$ls -l`
- Saatat nähdä jotain tällaista: `-rwsr-xr-x 1 root root 54256 passwd`

Huomaa:
- s käyttäjän execute-kohdassa (rws)
- → tämä tarkoittaa setuid

Teknisesti tämä tarkoittaa sitä, että ohjelman prosessille asetetaan niin sanottu effective user ID (eUID) ohjelman omistajan mukaan. Vaikka käyttäjä pysyy samana, järjestelmä tarkistaa oikeudet tämän eUID:n perusteella. Näin ohjelma voi tehdä asioita, joihin tavallisella käyttäjällä ei muuten olisi lupaa.

Esimerkki tästä on Linuxin `passwd`-komento. Tavallinen käyttäjä voi vaihtaa oman salasanansa tällä komennolla, vaikka salasanat tallennetaan tiedostoon `/etc/shadow`, johon on normaalisti pääsy vain root-käyttäjällä. Tämä toimii siksi, että `passwd`-ohjelma on asetettu setuid-tilaan ja sen omistaja on root. Kun ohjelma suoritetaan, se saa tarvittavat oikeudet muokata kyseistä tiedostoa, mutta käyttäjä ei saa näitä oikeuksia suoraan itselleen.

Setuidin tarkoitus ei ole estää käyttäjiä käyttämästä komentoja, vaan tarjota turvallinen tapa suorittaa rajattuja, korkeampia oikeuksia vaativia toimintoja ilman, että käyttäjälle annetaan laajoja pääkäyttäjäoikeuksia. Tämä eroaa esimerkiksi sudo-komennosta, jonka avulla käyttäjälle voidaan antaa lupa suorittaa lähes mitä tahansa komentoja toisena käyttäjänä, usein rootina.

Tietoturvan näkökulmasta setuid on hyödyllinen mutta riskialtis mekanismi. Jos setuid-oikeuksilla varustetussa ohjelmassa on ohjelmointivirhe tai haavoittuvuus, hyökkääjä voi mahdollisesti hyödyntää sitä saadakseen korkeammat oikeudet järjestelmässä. Tällaista tilannetta kutsutaan nimellä Privilege Escalation. Tämän vuoksi kyberturvallisuudessa noudatetaan periaatetta Principle of Least Privilege, jonka mukaan ohjelmille ja käyttäjille annetaan vain ne oikeudet, jotka ovat välttämättömiä toiminnan kannalta.

Eettisessä hakkeroinnissa, eli Ethical Hacking-toiminnassa, setuid-ohjelmat ovat usein tarkastelun kohteena. Turvallisuustestaajat analysoivat, onko tällaisissa ohjelmissa heikkouksia, joiden avulla oikeuksia voisi nostaa luvattomasti. Tällainen testaaminen tehdään kuitenkin aina luvan kanssa ja tarkoituksena on parantaa järjestelmien turvallisuutta, ei murtaa niitä.

Setuid on tiedoston oikeusbitin asetus, ei suojausmenetelmä eikä salaus.
Se ei “lukitse” tiedostoa tietylle käyttäjälle, vaan määrittää mitä oikeuksia ohjelma käyttää suoritettaessa.

Kun setuid on asetettu:
- kuka tahansa, jolla on oikeus ajaa tiedosto, voi käynnistää sen
- mutta ohjelma toimii tiedoston omistajan oikeuksilla (usein root)

> Jotenkin parempi esim ja kirjoitettu, että tajutaan.
> Normaalisti Linuxissa ohjelma toimii sen käyttäjän oikeuksilla, joka sen suorittaa. Tämä tarkoittaa, että käyttäjä voi tehdä vain niitä asioita, joihin hänellä itsellään on oikeus.
>
> Kun tarkastellaan tiedoston oikeuksia komennolla `ls -l`, voidaan nähdä esimerkiksi muoto `-rwsr-xr-x`. Tässä rwx tarkoittaa omistajan oikeuksia ja kirjain `s` tarkoittaa, että setuid on asetettu.
>
> Setuid tarkoittaa sitä, että kun ohjelma suoritetaan, se toimii tiedoston omistajan oikeuksilla, eikä suorittavan käyttäjän oikeuksilla. Usein tiedoston omistaja on root, jolloin ohjelma saa ajon ajaksi root-oikeudet.
>
> Tämä ei kuitenkaan tarkoita, että käyttäjä itse saisi root-oikeudet, vaan ainoastaan ohjelma käyttää niitä rajatusti. Root-käyttäjä voi silti ajaa kyseisen ohjelman normaalisti ilman rajoituksia.


lukemista: 
- https://www.linux.fi/wiki/Setuid
- https://www.lenovo.com/fi/fi/glossary/setuid/?srsltid=AfmBOopEUxNb9hE4Y-q9goHurqoVxpH5BsakwX0H_EyLIfMyt7CXwDgo

