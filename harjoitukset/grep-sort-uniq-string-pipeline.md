# Linux: grep, sort, uniq ja pipeline

Mikä on pipeline?

Pipeline tarkoittaa sitä, että yhdistetään komentoja putkella `|`

```
komento1 | komento2 | komento3
```

Data kulkee vasemmalta oikealle: yhden komennon tulos → seuraavan syöte


Pärjääkö ilman pipelinea? Kyllä, mutta rajoitetusti ja ei saisi helposti yhdistetyä toimintoja, mitä tekee useita välivaiheita

muutamia esimerkkejä:

toimii yksin (suodattaa rivit)
```
grep "abc" data.txt
```

toimii yksin (järjestää)
```
sort data.txt
```

toimii oikein vain jos data on valmiiksi järjestetty
```
uniq data.txt
```

--------------------------------------------------
YKSI KOMENTO
--------------------------------------------------
```
grep "test" data.txt
→ hakee rivit joissa “test”

sort data.txt
→ lajittelee kaiken

uniq -c data.txt
→ laskee vain peräkkäiset rivit (voi olla väärin)
```
--------------------------------------------------
KAKSI KOMENTOA
--------------------------------------------------
```
sort data.txt | uniq -c
→ oikea laskenta (koska sort ensin)

grep "test" data.txt | sort
→ suodata ensin, sitten järjestä
```

--------------------------------------------------
KOLME KOMENTOA (YLEINEN)
--------------------------------------------------
```
sort data.txt | uniq -c | grep "1 "
```
→ näyttää rivit jotka esiintyvät vain kerran

Vaiheet:
1. sort → tuo samat rivit yhteen
2. uniq -c → laskee määrät
3. grep "1 " → valitsee rivit joissa count = 1

--------------------------------------------------
SUODATUS ESIMERKKEJÄ (.txt)
--------------------------------------------------
```
$grep "error" data.txt
→ rivit joissa "error"

$grep -i "warning" data.txt
→ ei väliä iso/pieni kirjain

$grep -v "debug" data.txt
→ poistaa rivit joissa "debug"

$grep "^A" data.txt
→ rivit jotka alkavat A:lla

$grep "123" data.txt
→ rivit joissa numero 123
```

--------------------------------------------------
SUURI DATA / PALJON TOISTOJA
--------------------------------------------------
Tärkeää: suodata ensin
```
Hyvä:
$grep "error" data.txt | sort | uniq -c

Huono:
$cat data.txt | sort | uniq -c | grep "error"

Parempi:
$grep "error" data.txt | sort | uniq -c
```

Miksi?
- vähemmän dataa käsitellään
- nopeampi ja kevyempi

--------------------------------------------------
YLEISET KÄYTÖT
--------------------------------------------------
```
Uniikit rivit:
$sort data.txt | uniq

Laskenta:
$sort data.txt | uniq -c

Vain kerran esiintyvät:
$sort data.txt | uniq -c | grep "1 "

Yleisimmät rivit:
$sort data.txt | uniq -c | sort -nr
```

# strings

strings on Linux-komento, joka etsii tiedostosta tulostettavia (luettavia) merkkijonoja.
Esim. malli:
```
strings tiedosto.bin | grep -E "user.*="
```

Se on erityisen hyödyllinen:
- binääritiedostoissa (.bin, .exe, .dat) & rikkinäistä merkistöä & sekavia merkintöjä/sekava/dataa tiedostojen alla , mutta se sisältää todennäköisesti piilotettua tekstiä seassa
- tuntemattomissa tiedostoissa
- reverse engineering -tilanteissa
- kun epäilet tiedoston sisältävän piilotettua tekstiä

strings suodattaa pois ei-luettavan datan ja näyttää vain tekstin.


Tulostaa kaikki luettavat merkkijonot tiedostosta.

Tämä auttaa löytämään:
- käyttäjänimiä (user=)
- salasanoja (password=)
- API-avaimia
- URL-osoitteita
- konfiguraatioita
