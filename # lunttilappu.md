# lunttilappu
- PIENI lunttilappu toimivana listana - mitä ollaan tähän menessä tehty
  - komentoja: whoami, ls ja jne & `$cat <file>`
  - jos on jotakin certificate avainta niin kokeilee ensin siinä Linux alla avautua seuraavaan tasoon ja jos ei tallentaa työasemaan-lokaaliselle ja siitä kirjautua sisään.
    - jos tallentaa lokaaliselle asemaan niin pitää purkkaa sitä ja määrittää oikeudet ensin ennen kuin ajaa seuraavan komento
    - malli komento:`$ssh -i .\private.key -p 2220 banditXX@bandit.labs.overthewire.org`

  - Bandith tallennettu tiedosto polku: `/etc/bandit_pass/banditXX`

  - etsiä tiedostoa jotakin `$find <SOMETHING>` & `$grep <something>`
    - luettava, tiedoston koon ja tiedosto on suoritettava: `$find -readable -size 1033c ! -executable`
    - etsiä tiedoston koon tavun väliltä: `$find -size +900c -size -1100c`
    - etsii tekstiä tiedostojen sisällöstä: `$grep <name> <file.txt>`

    - laskee .txt tiedoston tai tiedoston monta riviä sisällä on: `$wc -l <file.txt>`
    - lajitellaan tiedoston rivit aakkosjärjestyksessä `$sort data.txt | <grep/sort/uniq>`
    - enkoodata muutosta, on base32 ja -64: `$cat data.txt  | base64 -d`

  - lukee kahden tiedoston sisällön eroa: `$diff tiedosto1.txt tiedosto2.txt`
    - `$diff -r kansio1 kansio2` - vertaa kaikki tiedostot kansiossa 1 ja 2 ja ikään kuin läpikäynti koskien jos on alikansiot ja mitä tiedostoja alla onkaan.


  - `-rwsr-xr-x` tarkoittaa, että omistajalla on `rwx`-oikeudet ja kirjain `s` execute-kohdassa kertoo, että setuid on asetettu, jolloin ohjelma suoritetaan omistajan oikeuksilla.
    - rwx ei ole pelkästään “omistajan oikeudet”, vaan koko tuo kohta tarkoittaa omistajan oikeuksia
    - -rws = setuid päällä → ohjelma ajetaan omistajan (usein root) oikeuksilla.









    