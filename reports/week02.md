1. Johdanto
Tässä tehtävässä tutustuttiin SNMP-protokollaan. SNMP on verkonhallintaprotokolla, jonka avulla voidaan kerätä tietoa verkon laitteista ja valvoa niiden tilaa.

2. Asennus
apt update
apt install snmp snmpd -y

Konfigurointi tehtiin muokkaamalla /etc/snmp/snmpd.conf -tiedostoa, johon lisättiin yhteisön nimi (rocommunity public)

Tämän jälkeen palvelu käynnistettiin uudelleen: service snmpd restart ja sitten tilan tarkistus: service snmpd status. 

3. Kerätyt tiedot

Kommenot ansible-palvelimelle: snmpwalk- ja snmpget-

Web1-laitteen tiedot:

Järjestelmän nimi: web1
Käyttöjärjestelmä: Linux web1 6.18.40.1-microsoft-standard-WSL2
Uptime: ~7 minuuttia (testihetkellä)

db1-laitteen tiedot:

Järjestelmän nimi: db1
Käyttöjärjestelmä: Linux db1 6.18.40.1-microsoft-standard-WSL2
Uptime: ~1 minuutti (testihetkellä)

Branch-client-laitteen tiedot:

Järjestelmän nimi: branch-client
Käyttöjärjestelmä: Linux branch-client 6.18.40.1-microsoft-standard-WSL2
Uptime: ~1 minuutti (testihetkellä)

4. Verkkorajapinnat

Laitteelta web1 verkkorajapinnat komennolla:
snmpwalk -v2c -c public web1 ifDescr

Tulokset:

Rajapintoja löytyi 2 kpl: lo ja eth0.
Rajapinta eth0 yhdistää laitteen verkkoon.

5. OID-analyysi

sysName.0 - Laitteen nimi
sysDescr.0 - Järjestelmän versiotiedot
sysUpTime.0 - Aika laitteen käynnistymisestä
ifDescr	- Verkkorajapintojen nimet
ifOperStatus - Rajapinnan toiminnallinen tila (up/down)

6. Pohdinta

Netin toimivuus: Rajapintojen tilaa (ifOperStatus) seuraamalla näkee heti, onko yhteys ylipäätään päällä vai onko esimerkiksi johto irti. Jos tila on "Down", netti ei toimi.

Laitteen hidastuminen: Uptime on tärkeä tieto, koska jos laite on ollut päällä todella pitkään, se voi alkaa hidastua. SNMP:n avulla ylläpitäjä näkee, milloin olisi aika käynnistää laite uudelleen, jotta se pysyy nopeana.

Automaatio: SNMP on hyödyllinen, koska ylläpitäjän ei tarvitse kirjautua jokaiselle koneelle erikseen, vaan hän näkee kaikkien laitteiden tilan keskitetysti Ansible-palvelimelta.

Turvallisuus: Huomasimme, että SNMPv2 käyttää selväkielistä "public"-salasanaa, mikä on tietoturvariski. Siksi oikeissa verkoissa kannattaisi käyttää SNMPv3:a, joka on suojattu paremmin.
