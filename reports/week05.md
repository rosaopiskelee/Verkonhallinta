Verkonhallinta - Viikko 05 Raportti

Wireshark

1. Ylen verkkoliikenne

Kaappasin työaseman verkkoliikennettä Wiresharkilla ja avasin selaimella osoitteen https://www.yle.fi. 

Capture-suodatin: port 53 or port 443

Näyttösuodatin:
1. dns
2. tls.handshake.type == 2

Tarkistin IP-osoitteen GeoIP-sijainnin Wiresharkin Statistics → Endpoints näkymästä. Wireshark näytti osoitteelle 52.85.154.29 maaksi Yhdysvallat. Tästä emme kuitenkaan voi olla täysin varmoja sillä pilvipalvelussa tulos voi vaihdella.

2. Learn.hamk.fi

Kaappasin liikennettä kirjautumisen aikana. DNS-vastauksessa learn.hamk.fi-osoitteen A-tietueeksi näkyi 195.148.239.84. Kuvan valitussa vastauksessa tietueen TTL oli 533 sekuntia.

Nimipalvelimiksi listautui ns-secondary.funet.fi, ns1.hamk.fi, ns2.hamk.fi ja ns3.hamk.fi. Wiresharkissa näkyi lisäksi SOA-tietue, jossa ensisijaiseksi nimipalvelimeksi ilmoitettiin ns1.hamk.fi

TLS-kättelyssä yhteyden protokollaksi näkyi TLS 1.3. Client Hello -paketissa selain tarjosi 16 erilaista salausvaihtoehtoa, kuten TLS_AES_128_GCM_SHA256, TLS_AES_256_GCM_SHA384 ja TLS_CHACHA20_POLY1305_SHA256. Palvelimen Server Hello -vastauksessa valituksi salausalgoritmiksi näkyi TLS_AES_128_GCM_SHA256.

Käytin TLS-pakettien tarkasteluun näyttösuodattimia:

ip.addr == 195.148.239.84 && tls.handshake.type == 1
ip.addr == 195.148.239.84 && tls.handshake.type == 2

3. Docker-kontin HTTP-liikenne

Käynnistin Docker-kontin ja aloitin verkkoliikenteen kaappauksen WSL:n terminaalissa tcpdump-komennolla. Sen jälkeen avasin selaimessa osoitteen http://localhost:8081 ja kirjauduin konttiin harjoituksessa määrittelemilläni käyttäjätunnuksilla. Kaappaus tallentui tiedostoon dump.pcap, jonka avasin Wiresharkissa.

Rajasin liikenteen Wiresharkin näyttösuodattimella: http

Etsin tunnistetietoja Authorization: Basic osiosta ja Wiresharkin Basic Credentials -näkymästä. Paketissa 15 näkyivät kontille määrittelemäni harjoitustunnus ja salasana. Liikenteessä näkyi ensin palvelimen 401 Unauthorized -vastaus ja onnistuneen kirjautumisen jälkeen 200 OK.



