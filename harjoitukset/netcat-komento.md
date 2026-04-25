# netcat 

`nc` - netcat komento on työkalu, joka: 

- avaa yhteyden (TCP tai UDP) - kertauksena TCP/UDP portteja on 0-65 535
  - kaikki portit ei ole auki, mutta jettu kolmeen ryhmään
  - 0–1023 (System/Well-known ports): Varattu tutuille peruspalveluille (esim. HTTP on 80, HTTPS on 443 ja SSH on 22).
  - 1024–49151 (Registered ports): Yritysten ja sovellusten rekisteröimiä portteja (esim. monet pelit tai tietokannat).
  - 49152–65535 (Dynamic/Private ports): Käytetään yleensä tilapäisinä portteina, kun tietokoneesi ottaa yhteyden palvelimeen.
  - Tietoturvan vuoksi on tapana pitää mahdollisimman monet portit suljettuina palomuurilla, jotta kutsumattomat vieraat eivät pääse koneelle.
- lukee stdinistä
- lähettää datan
- tulostaa vastauksen

Se ei “rajoita” portteja tai yhteyksiä — rajat tulevat:
- käyttöjärjestelmästä
- verkosta (firewall)
- palvelusta johon yhdistät
- toimii suoraan verkon kanssa
- yhdistyy mihin tahansa TCP/UDP-palveluun

Milloin kannattaa käyttää?

Käytä nc:tä kun:
- haluat testata nopeasti porttia
- debuggaat verkko-ongelmaa
- haluat nähdä “raakadatan”
- et halua raskaita työkaluja

perusmuoto
```
$nc [optioita] <host> <portti>
```

pipe-mallit:
```
echo "hello" | nc localhost 30000

cat tiedosto.txt | nc localhost 30000
```

TCP vs UPD tarkistus
TCP oletus:
```
nc localhost 30000
```

UDP:
```
nc -u localhost 30000
```

Tiedoston siirto (klassinen esimerkki)

vastaanottaja:
```
nc -l 1234 > vastaanotettu.txt
```

lähettäjä:
```
cat tiedosto.txt | nc <ip> 1234
```

Timeout (hyödyllinen usein)
esim. odottaa 3 sekunttia ja sulkee yhteyden
```
nc -w 3 localhost 30000
```

Portien tarkistus (esim. onko pystyssä ja portti auki ja vastaako joku siellä)
```
nc -zv localhost 30000
```

Yhteyksien debuggaus (erittäin käytännöllinen), esim. tarkista palveluiden "raaka", mikäli vastaa se saattaa antaa
 - GET / HTTP/1.1
 - Host: example.com
```
nc example.com 80
```

Port forwarding (yksinkertainen viritys) - toimii kuin tason "välittäjänä"
```
nc -l 1234 | nc target.com 80
```

## komennon tietoturva, kyber ja käyttötarkoitus

Netcat (`nc`) on monikäyttöinen työkalu, jota voidaan käyttää täysin normaalisti esimerkiksi omien tai luvallisten järjestelmien porttien ja palveluiden testaamiseen. Sillä voidaan tarkistaa, onko esimerkiksi portit 80 (HTTP) tai 443 (HTTPS) auki ja mitä palveluita niiden takana toimii. Vastaava toiminnallisuus löytyy myös Windows PowerShell-ympäristössä komennolla `Test-NetConnection`.

Tietoturvan ja eettisen hakkeroinnin näkökulmasta ratkaisevaa on aina lupa ja tarkoitus. Sallittua on testata omia järjestelmiä tai kohteita, joihin on annettu lupa, kuten bug bounty -ohjelmissa. Sen sijaan satunnainen porttien skannaus tai palveluiden testaaminen ilman lupaa on kiellettyä ja voi olla laitonta, vaikka kyse olisi pelkästä uteliaisuudesta.

nc toimii ns. low-level työkaluna:
-  ei piilota mitään
- ei automatisoi liikaa
- näyttää miten verkko oikeasti toimii

Siksi sitä käytetään:
- auditoinneissa
- penetraatiotestauksessa
- debuggaamisessa


Eettisessä hakkeroinnissa:

✔ sallittua:
- testata omia järjestelmiä
- testata järjestelmiä joihin on lupa
- bug bounty -ohjelmat

❌ ei sallittua:
- satunnainen porttiskannaus netissä
- palveluihin yhdistely ilman lupaa
- “kokeilu uteliaisuudesta” toisten järjestelmiin


nc on täysin normaali työkalu
- käytetään sekä ylläpidossa että hakkeroinnissa
- porttien tarkistus (80, 443 jne.) on perusjuttu
- ratkaisevaa on lupa ja tarkoitus
