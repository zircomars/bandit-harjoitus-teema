# find-komento (Linux)

find-komennolla etsitään tiedostoja ja kansioita hakemistorakenteesta. Sekä find komento voi yhdistää ehtoja , mikä tarkentaa tarkkaa hakua ja toimii myös oikeuksien ja metadatan perusteella, ei vain nimellä

## Perusmuoto
$`find` [hakemisto] [ehdot]

Esim:
```
find /home -name tiedosto.txt
```

---

## 🔍 Nimen perusteella etsiminen

Etsi tarkka nimi:
```
find . -name "file.txt"
```

Ei välitä isoista/pienistä kirjaimista:
```
find . -iname "file.txt"
```

Etsi osittaisella nimellä:
```
find . -name "file*"
find . -name "*.txt"
```

---

## 📁 Tiedosto vai kansio

Vain tiedostot:
```
find . -type f
```

Vain kansiot:
```
find . -type d
```

---

## 🔐 Oikeuksien perusteella

Etsi luettavat tiedostot:
```
find . -readable
```

Etsi kirjoitettavat:
```
find . -writable
```

Etsi suoritettavat:
```
find . -executable
```

Etsi tietyillä oikeuksilla:
```
find . -perm 644
```
---

## 👤 Omistajan mukaan
```
find . -user kali
```

---

## 📏 Koon mukaan
Eli tiedoston kokon mukaisesti

Yli 1MB:
```
find . -size +1M
```

Alle 10KB:
```
find . -size -10k
```

---

## ⏱️ Ajan mukaan

Muokattu viimeisen 1 päivän aikana:
```
find . -mtime -1
```

---

## ⚡ Useita ehtoja
```
find . -name "*.txt" -type f
```
---

## ❗ Virheiden piilotus
```
find / -name "file.txt" 2>/dev/null
```
---

## 💡 Hyödyllisiä käytännössä

Etsi kaikki .txt tiedostot:
```
find . -name "*.txt"
```

Etsi iso tiedosto:
```
find / -size +100M 2>/dev/null
```

Etsi käyttäjän tiedostot:
```
find / -user kali 2>/dev/null
```

---

## 📁 Jos tiedät kansion
Etsi vain tietystä kansiosta:
```
find /polku/kansioon -name "tiedosto.txt"
```

👉 Esim:
```
find /home/kali/Documents -name "notes.txt"
```
→ hakee VAIN tuon kansion sisältä (ja sen alikansiot)

## 🚫 Estä alikansioiden läpikäynti
Etsi vain yhdestä kansiosta (ei alikansioita):
```
find . -maxdepth 1 -name "tiedosto.txt"
```
👉 tärkeä:

`-maxdepth 1` = ei mene syvemmälle
📂 Etsi vain tietyn kansion sisältä nimellä
```
find /home -type d -name "Documents"
```
→ löytää kansion nimeltä Documents

## ⚡ Rajaa pois tietty kansio
```
find / -path "/proc" -prune -o -name "file.txt" -print
```
👉 tarkoittaa:

älä mene /proc-kansioon
etsi muualla
🔍 Yhdistelmä (yleinen “pro” tapa)
```
find /home/kali -type f -name "*.txt"
```
→ vain:

tietyssä kansiossa
vain tiedostot
vain .txt


## 🧠 Tärkein oivallus
find alkaa AINA siitä polusta jonka annat.

```
find /      = koko järjestelmä (hidas)
find .      = nykyinen kansio
find /home  = vain home-kansio (nopeampi)
```
💡 Käytännön vinkki (tosi tärkeä)

👉 ÄLÄ tee tätä turhaan:
```
find /
```
👉 tee mieluummin:
```
find /home/kali
```
→ nopeampi
→ vähemmän virheitä (permission denied)

---

## find-komento – virheiden käsittely (Linux)

Kun käytät find-komentoa koko järjestelmässä `(find /)`, saat usein paljon "Permission denied" -virheitä.

Niitä voi käsitellä kolmella eri tavalla:

---

## 1. Yksinkertaisin tapa (suositeltu)

```
find / -type f -size 33c -group bandit6 -user bandit7 2>/dev/null
```

✔ Poistaa virheilmoitukset kokonaan
✔ Siisti tuloste
✔ Nopein ja yleisin tapa

---

## 2. Suodatus grepillä

```
find / -type f -size 33c -group bandit6 -user bandit7 2>&1 | grep -v "Permission denied"
```

✔ Yhdistää virheet ja tulosteen
✔ Poistaa vain tietyt virheet
✔ Joustavampi tapa

---

## 3. Ilman suodatusta (ei suositella)

```
find / -type f -size 33c -group bandit6 -user bandit7
```

✘ Näyttää paljon virheitä
✘ Sekava tuloste

---

## 🧠 Yhteenveto

- `2>/dev/null` = yksinkertaisin ja siistein
- `grep -v` = joustavampi suodatus
- ilman mitään = sekava ja vaikea lukea
