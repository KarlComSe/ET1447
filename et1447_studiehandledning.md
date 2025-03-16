# Omfattande Studiehandledning för Nätverksexamen

## Innehållsförteckning
1. [Nätverkslager och Protokollstack](#nätverkslager-och-protokollstack)
2. [DNS (Domain Name System)](#dns-domain-name-system)
3. [ARP (Address Resolution Protocol)](#arp-address-resolution-protocol)
4. [DHCP (Dynamic Host Configuration Protocol)](#dhcp-dynamic-host-configuration-protocol)
5. [IPv4 och IPv6](#ipv4-och-ipv6)
6. [MAC-adresser](#mac-adresser)
7. [NAT (Network Address Translation)](#nat-network-address-translation)
8. [Subnät](#subnät)
9. [TCP och UDP](#tcp-och-udp)
10. [Nätverksfördröjningar och förluster](#nätverksfördröjningar-och-förluster)
11. [Routrar och Switchar](#routrar-och-switchar)
12. [Autonoma System (AS) och Routingprotokoll](#autonoma-system-as-och-routingprotokoll)
13. [Nätverksåtkomstteknologier](#nätverksåtkomstteknologier)
14. [Traceroute, Ping och Nätverksverktyg](#traceroute-ping-och-nätverksverktyg)
15. [Nätverkssäkerhetskoncept](#nätverkssäkerhetskoncept)

**Kompletterande material** 
1. [HTTP och Webbteknologier](#http-och-webbteknologier)
2. [Beräkningar av Nätverksprestanda](#beräkningar-av-nätverksprestanda)
3. [DHCP i Praktiken](#dhcp-i-praktiken)
4. [E-postprotokoll](#e-postprotokoll)
5. [WLAN och Trådlösa Teknologier](#wlan-och-trådlösa-teknologier)
6. [Mobila Nätverk](#mobila-nätverk)
7. [TCP i Praktiken](#tcp-i-praktiken)



## Nätverkslager och Protokollstack

### Internets Protokollstack (5 Lager)

| Lager | Namn | Paketnamn | Viktiga Protokoll | Primära Funktioner |
|-------|------|-------------|--------------|-------------------|
| 5 | Applikationslager | N/A | HTTP, FTP, SMTP, DNS, SNMP, POP3, IMAP, SSH, DASH | • Applikationsspecifika funktioner<br>• Användargränssnitt<br>• Dataformatering och kryptering |
| 4 | Transportlager | Segment | TCP, UDP, SCTP, DCCP | • Ändpunkt-till-ändpunkt anslutningar<br>• Flödeskontroll<br>• Felåterställning<br>• Multiplexing av applikationer |
| 3 | Nätverkslager | Datagram | IP (v4/v6), ICMP, IGMP, IPSec, RIP, OSPF, BGP | • Logisk adressering (IP)<br>• Routing mellan nätverk<br>• Paketvidarebefordran<br>• Fragmentering/återmontering |
| 2 | Länklager (Datalänk) | Ramar (Frames) | Ethernet, Wi-Fi (802.11), PPP, MPLS, ARP, VLAN | • Fysisk adressering (MAC)<br>• Medieåtkomstkontroll<br>• Felupptäckt<br>• Ramsynkronisering |
| 1 | Fysiska lagret | Bitar | IEEE 802.3, Bluetooth, DSL, ISDN, Fiber Channel | • Bitöverföring<br>• Specifikationer för fysiskt medium<br>• Signalkodning<br>• Hårdvarustandarder |

### Viktiga Portnummer och Adresser
- **Portnummer**: Används på Transportlagret (Lager 4)
  - Syfte: Koppla paket till specifika applikationer/processer
  - Välkända portar: 0-1023 (HTTP: 80, HTTPS: 443, DNS: 53, etc.)
  - Registrerade portar: 1024-49151
  - Dynamiska/privata portar: 49152-65535

- **MAC-adresser**: Används på Datalänklagret (Lager 2)
  - Syfte: Identifiera specifika nätverksgränssnitt på ett lokalt nätverk
  - Format: 48 bitar (6 byte), vanligtvis skrivet i hexadecimal (t.ex. 00:1A:2B:3C:4D:5E)
  - Används av switchar för att vidarebefordra ramar på ett lokalt nätverk
  - Används i ARP-processen för att mappa IP-adresser till fysiska adresser

### Sockets
- Anslutningspunkt mellan applikations- och transportlagren
- Definieras av kombinationen av IP-adress och portnummer
- Möjliggör för processer att kommunicera över ett nätverk
- Abstraktion som döljer nätverkskomplexitet för applikationer
- Socket-API tillhandahålls av operativsystemet

## DNS (Domain Name System)

### DNS-hierarkin (Tre Nivåer)

1. **Root DNS-servrar**
   - Toppen av DNS-hierarkin
   - 13 logiska rotservrarkluster (namngivna A till M)
   - Hanteras av 12 olika organisationer
   - Innehåller information om TLD-servrar
   - Lagrar inte faktiska domänposter

2. **Toppdomänservrar (TLD-servrar)**
   - Hanterar domäner för specifika TLDs (.com, .org, .net, landkoder som .se)
   - Pekar till auktoritativa namnservrar för specifika domäner
   - Drivs av olika organisationer (Verisign hanterar .com och .net)

3. **Auktoritativa Namnservrar**
   - Lagrar faktiska DNS-poster för specifika domäner
   - Ger definitiva svar på frågor om sina domäner
   - Drivs av organisationer eller deras tjänsteleverantörer
   - Innehåller resursposter som A, AAAA, MX, CNAME, TXT

### Varför DNS Använder en Hierarkisk Struktur
- **Skalbarhet**: Fördelar belastningen över miljontals servrar världen över
- **Administrativ autonomi**: Varje domän kan hantera sina egna poster
- **Feltolerans**: Ingen enskild felkälla
- **Effektivitet**: Lokaliserar frågor till lämpliga servrar
- **Logisk organisation**: Matchar domännamnets struktur

### Lokala DNS-servrars Roll (Rekursiva Resolvrar)
- Första kontaktpunkt för klientens DNS-frågor
- Inte officiellt en del av DNS-hierarkin men kritisk för driften
- Utför rekursiv upplösning genom att fråga rot-, TLD- och auktoritativa servrar
- Cachelagrar resultat för att snabba upp framtida frågor
- Tillhandahålls vanligtvis av internetleverantörer eller organisationer som Google (8.8.8.8)
- Kan implementera lokala policyer (filtrering, säkerhet)

### DNS-upplösningsprocess
1. Klienten frågar lokal DNS-server
2. Om inte cachad, frågar den lokala servern rotservern
3. Rotservern hänvisar till lämplig TLD-server
4. TLD-servern hänvisar till auktoritativ server
5. Auktoritativ server ger svaret
6. Lokal server cachelagrar och returnerar resultatet till klienten

## ARP (Address Resolution Protocol)

### Syfte och Funktion
- Mappar IP-adresser (Lager 3) till MAC-adresser (Lager 2)
- Nödvändig för kommunikation på lokala nätverk
- Fungerar endast inom en enskild broadcast-domän

### Hur ARP Fungerar
1. Enhet behöver skicka data till en IP-adress på lokalt nätverk
2. Kontrollerar ARP-cachen för befintlig mappning
3. Om inte hittad, sänder ut ARP-förfrågan: "Vem har IP x.x.x.x?"
4. Enhet med matchande IP skickar unicast ARP-svar med sin MAC-adress
5. Avsändaren uppdaterar ARP-cache och skickar ramen

### ARP-paketfält
- **Hårdvarutyp**: Specificerar länkprotokoll (vanligtvis Ethernet)
- **Protokolltyp**: Specificerar nätverksprotokoll (vanligtvis IPv4)
- **Hårdvaruadresslängd**: Storlek på MAC-adress (6 byte)
- **Protokolladresslängd**: Storlek på IP-adress (4 byte för IPv4)
- **Operation**: Förfrågan (1) eller Svar (2)
- **Avsändarens hårdvaru-/protokolladresser**: MAC och IP för avsändaren
- **Målets hårdvaru-/protokolladresser**: MAC (nollor i förfrågan) och IP för målet

## DHCP (Dynamic Host Configuration Protocol)

### DHCP-process (4 Steg)
1. **DHCP Discovery**: Klienten sänder ut för att hitta tillgängliga servrar (destination: 255.255.255.255)
2. **DHCP Offer**: Servern erbjuder IP-adress och konfigurationsparametrar
3. **DHCP Request**: Klienten begär erbjuden IP-adress (broadcast för att informera alla servrar)
4. **DHCP Acknowledgment**: Servern bekräftar tilldelningen, klienten konfigurerar gränssnittet

### Information som Tillhandahålls av DHCP
- IP-adress och subnätmask
- Standard-gateway (router)
- DNS-serveradresser
- Lånetid (hur länge IP-adressen kan användas)
- Domännamn
- NTP-servrar
- MTU-storlek

### Fördelar med DHCP
- Förhindrar IP-adresskonflikter genom centraliserad hantering
- Effektivt utnyttjande av IP-adresser genom utlåning
- Minskar administrativ overhead
- Förenklar klientkonfiguration
- Stödjer mobila användare som ansluter till olika nätverk
- Möjliggör hantering av IP-adresspooler

## IPv4 och IPv6

### Viktiga Skillnader

| Funktion | IPv4 | IPv6 |
|---------|------|------|
| Adresstorlek | 32 bitar (4 byte) | 128 bitar (16 byte) |
| Adressformat | Punktad decimal (192.168.1.1) | Hexadecimal med kolon (2001:0db8:85a3:0000:0000:8a2e:0370:7334) |
| Adressutrymme | ~4,3 miljarder adresser | 340 undecillion adresser (3,4×10^38) |
| Header-storlek | Variabel (20-60 byte) | Fast (40 byte) |
| Header-fält | 12 basfält + alternativ | 8 fält (förenklad) |
| Checksumma | Inkluderad i header | Borttagen (hanteras av högre lager) |
| Fragmentering | Av routrar och källa | Endast av källan |
| Konfiguration | Manuell eller DHCP | Statslös autokonfiguration eller DHCPv6 |
| NAT-krav | Vanligt på grund av adressbrist | Generellt onödigt |
| Säkerhet | IPsec valfritt | IPsec inbyggt (men fortfarande valfritt i praktiken) |


### Privata IPv4-adressintervall
- 10.0.0.0 till 10.255.255.255 (10.0.0.0/8)
- 172.16.0.0 till 172.31.255.255 (172.16.0.0/12)
- 192.168.0.0 till 192.168.255.255 (192.168.0.0/16)

## MAC-adresser

### Struktur och Format
- 48 bitar (6 byte) i längd
- Skrivs som sex par hexadecimala siffror (t.ex. 00:1A:2B:3C:4D:5E)
- Första 24 bitar (3 byte): Organisatoriskt Unik Identifierare (OUI)
- Sista 24 bitar (3 byte): Enhetsidentifierare tilldelad av tillverkaren

### Funktioner
- Identifierar unikt nätverksgränssnitt på Lager 2
- Används för ramleverans inom ett lokalt nätverk
- Används av switchar för att bygga och underhålla vidarebefordringstabeller
- Används i ARP-processen för IP-till-MAC-adressupplösning
- Inbrännd i hårdvaran men kan ofta ändras via programvara

## NAT (Network Address Translation)

### Typer av NAT
- **Statisk NAT**: En-till-en-mappning mellan privata och publika IP-adresser
- **Dynamisk NAT**: Mappar privata IP-adresser till en pool av publika IP-adresser
- **PAT/NAPT** (Port Address Translation): Mappar flera privata IP-adresser till en enda publik IP-adress med olika portar

### NAT-process
1. Enhet på privat nätverk skickar paket till extern destination
2. NAT-enhet (router) tar emot paketet
3. Routern ändrar käll-IP (och ibland port) till sin publika adress
4. Routern registrerar översättningen i sin NAT-tabell
5. När svar kommer tillbaka använder routern NAT-tabellen för att vidarebefordra till rätt intern enhet

### Fördelar med NAT
- Sparar publika IP-adresser
- Ger säkerhet genom att dölja intern nätverksstruktur
- Kan förenkla nätverksdesign och routing
- Möjliggör internetåtkomst för stora interna nätverk

### Nackdelar med NAT
- Bryter ändpunkt-till-ändpunkt-anslutningsprincipen
- Komplicerar peer-to-peer-applikationer
- Lägger till bearbetningsoverhead och latens
- Stör vissa protokoll och applikationer
- Gör spårbarhet och felsökning svårare

## Subnät

### Grundläggande om Subnät
Subnät delar upp ett större nätverk i mindre logiska nätverk för att:
- Förbättra nätverksprestanda genom att minska broadcast-domäner
- Förbättra säkerhet genom segmentering
- Optimera adresstilldelning
- Förenkla hantering

### Viktiga Koncept
- **Nätverksadress**: Första adressen i ett subnät (alla värd-bitar är 0)
- **Broadcast-adress**: Sista adressen i ett subnät (alla värd-bitar är 1)
- **Användbara Adresser**: Alla adresser mellan nätverks- och broadcast-adress
- **Subnätmask**: Identifierar vilken del av IP-adressen som är nätverk vs. värd
  - Skriven i punktad decimal (255.255.255.0) eller CIDR-notation (/24)
  - Måste ha sammanhängande 1:or följt av sammanhängande 0:or

### Steg för Subnätsberäkning
1. **Fastställ hur många subnät du behöver**
   - Beräkna bitar som behövs: 2^n ≥ antal subnät
   - Ta n bitar från värddelen av adressen

2. **Fastställ subnätmasken**
   - Lägg till subnätbitar till ursprungliga nätverksbitar
   - Exempel: 192.168.1.0/24 delat i 4 subnät
     - Behöver 2 bitar för 4 subnät (2^2 = 4)
     - Ny mask är /26 (24 + 2)
     - I decimal: 255.255.255.192

3. **Beräkna subnätintervaller**
   - Öka med "subnätstorlek" (2^återstående_värdbitar)
   - För /26: öka med 64 (2^6 = 64)
   - Subnät: 192.168.1.0/26, 192.168.1.64/26, 192.168.1.128/26, 192.168.1.192/26

4. **Beräkna användbara adresser**
   - Första adress: Nätverksadress (kan ej användas)
   - Sista adress: Broadcast-adress (kan ej användas)
   - Användbara adresser: Nätverk+1 till Broadcast-1
   - Ett /26-subnät har 62 användbara adresser (2^6 - 2)

### Subnätmaskens Giltighet
Giltiga subnätmasker har sammanhängande 1:or följt av sammanhängande 0:or. Exempel:
- **Giltig**: 255.255.255.0 (/24), 255.255.255.240 (/28), 255.255.248.0 (/21)
- **Ogiltig**: 255.255.255.64 (icke-sammanhängande mönster)

### VLSM (Variable Length Subnet Masking)
- Tillåter subnät av olika storlekar inom samma nätverk
- Maximerar adressutrymmesutnyttjande
- Kräver noggrann planering för att undvika överlappning

### Praktiskt Subnätexempel
För att dela upp 192.168.24.0/24 i 8 lika stora subnät:

1. Vi behöver 3 bitar för att skapa 8 subnät (2^3 = 8)
2. Ny mask är /27 (24 + 3)
3. Varje subnät har 32 IP-adresser (2^5 = 32)
4. Subnäten är:
   - Nät A: 192.168.24.0/27 (användbara: 192.168.24.1-30, broadcast: 192.168.24.31)
   - Nät B: 192.168.24.32/27 (användbara: 192.168.24.33-62, broadcast: 192.168.24.63)
   - Nät C: 192.168.24.64/27 (användbara: 192.168.24.65-94, broadcast: 192.168.24.95)
   - Nät D: 192.168.24.96/27 (användbara: 192.168.24.97-126, broadcast: 192.168.24.127)
   - Nät E: 192.168.24.128/27 (användbara: 192.168.24.129-158, broadcast: 192.168.24.159)
   - Nät F: 192.168.24.160/27 (användbara: 192.168.24.161-190, broadcast: 192.168.24.191)
   - Nät G: 192.168.24.192/27 (användbara: 192.168.24.193-222, broadcast: 192.168.24.223)
   - Nät H: 192.168.24.224/27 (användbara: 192.168.24.225-254, broadcast: 192.168.24.255)

### Hitta Nätverksadress från IP och Mask
Utför en bitvis AND-operation mellan IP-adressen och subnätmasken:
```
IP:     192.168.30.5  11000000.10101000.00011110.00000101
Mask:   255.255.255.0 11111111.11111111.11111111.00000000
Result: 192.168.30.0  11000000.10101000.00011110.00000000 (Nätverksadress)
```

### Beräkning av Användbara Adresser
Formel: 2^(32-prefix_längd) - 2
Exempel:
- /24 subnät: 2^(32-24) - 2 = 2^8 - 2 = 254 användbara adresser
- /27 subnät: 2^(32-27) - 2 = 2^5 - 2 = 30 användbara adresser
- /29 subnät: 2^(32-29) - 2 = 2^3 - 2 = 6 användbara adresser

## TCP och UDP

### TCP (Transmission Control Protocol)
Ett förbindelseorienterat protokoll som säkerställer tillförlitlig, ordnad leverans av data.

#### TCP Header-fält
- **Käll- och Destinationsportar** (16 bitar vardera)
- **Sekvensnummer** (32 bitar): Position av data i strömmen
- **Bekräftelsenummer** (32 bitar): Nästa förväntade byte
- **Header-längd** (4 bitar): Storlek på TCP-header i 32-bitars ord
- **Flaggor** (9 bitar): Kontrollflaggor som SYN, ACK, FIN, RST
- **Fönsterstorlek** (16 bitar): Flödeskontrollmekanism
- **Checksumma** (16 bitar): Felupptäckt
- **Urgent Pointer** (16 bitar): Pekar på brådskande data
- **Alternativ** (variabel): Ytterligare funktioner

#### TCP Anslutningsetablering (Three-Way Handshake)
1. **Klient → Server**: SYN-paket med initialt sekvensnummer
2. **Server → Klient**: SYN-ACK-paket med serverns sekvensnummer
3. **Klient → Server**: ACK-paket som bekräftar serverns sekvensnummer

#### TCP Anslutningsavslutning (Four-Way Handshake)
1. **Initiator → Mottagare**: FIN-paket (tillstånd: FIN-WAIT-1)
2. **Mottagare → Initiator**: ACK för FIN (tillstånd: CLOSE-WAIT och FIN-WAIT-2)
3. **Mottagare → Initiator**: FIN-paket när redo (tillstånd: LAST-ACK)
4. **Initiator → Mottagare**: ACK för FIN (tillstånd: TIME-WAIT och CLOSED)

#### TCP Tillförlitlighetsmekanismer
- **Bekräftelser**: Bekräftar mottagande av data
- **Återöverföring**: Skickar om paket som inte bekräftas
- **Sekvensnummer**: Håller ordning på datasegment
- **Checksummor**: Upptäcker korruption

#### TCP Flödeskontroll
- **Sliding Window**: Mottagaren annonserar fönsterstorlek (rwnd) för att kontrollera flödet
- **Nollfönster**: Mottagaren kan stoppa överföring genom att annonsera fönster på 0
- **Window Scale Option**: Tillåter fönster större än 65 535 byte

#### TCP Överbelastningskontroll
- **Slow Start**: Börjar med litet överbelastningsfönster (cwnd), fördubblar det varje RTT
- **Överbelastningsundvikande**: Linjär ökning efter tröskelvärde
- **Fast Retransmit**: Återöverför efter 3 duplicerade ACKs
- **Fast Recovery**: Undviker att gå tillbaka till slow start

### UDP (User Datagram Protocol)
Ett förbindelselöst protokoll som tillhandahåller minimal transportlagerfunktionalitet.

#### UDP Header-fält (8 byte totalt)
- **Källport** (16 bitar)
- **Destinationsport** (16 bitar)
- **Längd** (16 bitar): Längd på UDP-header och data
- **Checksumma** (16 bitar): Valfri felupptäckt

#### UDP-egenskaper
- **Förbindelselöst**: Ingen anslutningsetablering
- **Otillförlitligt**: Ingen garanti för leverans, ordning eller dupliceringsskydd
- **Låg Overhead**: Minimal header-storlek
- **Ingen Flödeskontroll**: Skickar data i valfri takt
- **Ingen Överbelastningskontroll**: Anpassar sig inte till nätverksförhållanden

### Viktiga Skillnader Mellan TCP och UDP

| Funktion | TCP | UDP |
|---------|-----|-----|
| Anslutning | Förbindelseorienterad | Förbindelselös |
| Tillförlitlighet | Garanterad leverans | Best-effort leverans |
| Ordning | Upprätthåller ordning | Ingen ordningsgaranti |
| Hastighet | Högre overhead, långsammare | Lägre overhead, snabbare |
| Header-storlek | 20-60 byte | 8 byte |
| Flödeskontroll | Ja (fönsterbaserad) | Nej |
| Överbelastningskontroll | Ja | Nej |
| Användningsfall | Webbsurfning, e-post, filöverföring | Streaming, DNS, VoIP |

## Nätverksfördröjningar och förluster

### Typer av Nätverksfördröjning
1. **Bearbetningsfördröjning**: Tid för att undersöka paketheader och fatta vidarebefordringsbeslut
2. **Köfördröjning**: Tid i routerbuffertar i väntan på att överföras
3. **Överföringsfördröjning** (tranismissions-losses): Tid för att skicka ut alla paketbitar på länken
   - Formel: Överföringsfördröjning = Paketstorlek (bitar) / Länkbandbredd (bps)
4. **Utbredningsfördröjning** (propagation-losses): Tid för signal att färdas genom mediet
   - Formel: Utbredningsfördröjning = Avstånd / Signalhastighet

### Faktorer som Påverkar Fördröjning
- **Nätverksbelastning**: Högre trafik ökar köfördröjningar
- **Paketstorlek**: Större paket ökar överföringsfördröjningar
- **Länkkapacitet**: Lägre bandbredd ökar överföringsfördröjningar
- **Fysiskt avstånd**: Större avstånd ökar utbredningsfördröjningar
- **Routerprestanda**: Långsammare processorer ökar bearbetningsfördröjningar

### Minska Överföringsfördröjning
- Öka länkbandbredd/hastighet
- Minska paketstorlek
- Implementera komprimering
- Använd protokoll med mindre overhead

### Orsaker till Paketförlust
- **Buffertöverfyllnad**: Vanligaste orsaken, inträffar vid överbelastning
- **Överföringsfel**: Korruption på grund av brus eller störningar
- **Nätverksfel**: Utrustnings- eller länkfel
- **Överbelastningskontroll**: Avsiktligt bortkastande av routrar
- **Timeout-baserat bortkastande**: Paket som tar för lång tid kasseras

### Round-Trip Time (RTT)
- Tid för ett paket att färdas från avsändare till mottagare och tillbaka
- Kritiskt för TCP timeout-beräkningar
- Mäts genom ping eller TCP-tidsstämplar
- Påverkar överföringsprestanda och överbelastningskontroll

### Throughput kontra Bandbredd
- **Bandbredd**: Maximal teoretisk dataöverföringshastighet
- **Throughput**: Faktisk mängd data som framgångsrikt överförs
- Throughput är alltid mindre än eller lika med bandbredd på grund av overhead, fördröjningar och förluster

## Routrar och Switchar

### Switch-operation
- Arbetar på Datalänklagret (Lager 2)
- Använder MAC-adresser för att vidarebefordra ramar
- Upprätthåller en MAC-adresstabell som mappar portar till adresser
- Skapar separata kollisionsdomäner för varje port
- Opererar inom en enskild broadcast-domän

### Router-operation
- Arbetar på Nätverkslagret (Lager 3)
- Använder IP-adresser för att vidarebefordra paket mellan nätverk
- Upprätthåller routingtabeller för att bestämma bästa väg
- Skapar separata broadcast-domäner
- Kan utföra NAT, brandväggsfunktioner och QoS

### Viktiga Skillnader Mellan Switchar och Routrar

| Funktion | Switch | Router |
|---------|--------|--------|
| Lager | Datalänk (Lager 2) | Nätverk (Lager 3) |
| Adressering | MAC-adresser | IP-adresser |
| Funktion | Ansluter enheter i LAN | Ansluter olika nätverk |
| Broadcast | Vidarebefordrar broadcasts | Blockerar broadcasts |
| Latens | Lägre (snabbare) | Högre (långsammare) |
| Intelligens | Mindre komplexa beslut | Mer komplexa routingbeslut |
| Säkerhet | Begränsad | Fler säkerhetsfunktioner |

### Longest Prefix Matching
- Används av routrar för att välja den mest specifika rutten
- När flera rutter matchar en destination, vinner den med längst subnätmask (prefix)
- Exempel: 
  - Rutter: 192.168.0.0/16, 192.168.1.0/24, 192.168.1.128/25
  - Paket till 192.168.1.130 matchar alla tre rutter
  - Routern väljer 192.168.1.128/25 (mest specifik)

## Autonoma System (AS) och Routingprotokoll

### Autonoma System
- Samling av nätverk under enskild administrativ kontroll
- Identifieras av ett unikt AS-nummer (ASN)
- Utgör grunden för internets routinghierarki

### Inter-AS kontra Intra-AS Routing

| Funktion | Inter-AS (Exterior Gateway) | Intra-AS (Interior Gateway) |
|---------|------------------------------|------------------------------|
| Omfattning | Mellan autonoma system | Inom ett autonomt system |
| Primärt protokoll | BGP | OSPF, RIP, EIGRP |
| Primärt fokus | Policy, kontrakt, affärsrelationer | Teknisk effektivitet |
| Komplexitet | Mer komplex, politiskt driven | Mer tekniskt driven |
| Kontroll | Kontrolleras av avtal mellan organisationer | Kontrolleras av enskild organisation |

### Intra-AS Routingprotokoll

#### OSPF (Open Shortest Path First)
- **Typ**: Link-state routingprotokoll
- **Algoritm**: Dijkstras kortaste väg-algoritm
- **Funktioner**:
  - Skapar komplett karta över nätverkstopologin
  - Snabb konvergens
  - Stöder VLSM och CIDR
  - Hierarkisk design med områden
  - Lastbalansering över likvärdiga vägar
  - Autentiseringsstöd

#### RIP (Routing Information Protocol)
- **Typ**: Distance-vector routingprotokoll
- **Algoritm**: Bellman-Ford-algoritmen
- **Funktioner**:
  - Enkel att konfigurera
  - Maximal hopgräns på 15
  - Använder periodiska broadcast
  - Långsam konvergens
  - Begränsad skalbarhet

### Inter-AS Routingprotokoll: BGP (Border Gateway Protocol)
- **Typ**: Path-vector protokoll
- **Funktioner**:
  - Utbyter nätverkstillgänglighetsinformation
  - Policybaserad routing
  - Attribut påverkar vägval
  - Förhindrar routingloopar
  - Skalar till internets storlek
- **Vägvalsfaktorer**:
  - AS-väglängd
  - Lokal preferens
  - Multi-exit diskriminator (MED)
  - Ursprungstyp
  - Vägtyp (intern vs. extern)

## Nätverksåtkomstteknologier

### Trådburna Teknologier

#### Fiberoptik
- **Hur det fungerar**: Överför data som ljuspulser genom glas- eller plastfibrer
- **Fördelar**:
  - Extremt hög bandbredd (Gbps till Tbps)
  - Låg latens och signalförsämring
  - Immun mot elektromagnetiska störningar
  - Långdistansöverföring
- **Nackdelar**:
  - Dyr installation, särskilt på landsbygden
  - Specialiserad utrustning behövs för reparationer
  - Fysiska skador kan vara svåra att åtgärda

#### DSL (Digital Subscriber Line)
- **Hur det fungerar**: Använder befintliga telefonlinjer för dataöverföring
- **Fördelar**:
  - Använder befintlig infrastruktur
  - Dedikerad anslutning (delas inte med grannar)
  - Rimliga hastigheter för de flesta applikationer
- **Nackdelar**:
  - Hastigheten minskar med avståndet från telestation
  - Begränsad maximal bandbredd
  - Kvaliteten beror på telefonlinjens skick

#### Kabel
- **Hur det fungerar**: Använder koaxial-kabel-TV-infrastruktur
- **Fördelar**:
  - Bred tillgänglighet i stads- och förortsområden
  - Högre hastigheter än traditionell DSL
  - Relativt billig
- **Nackdelar**:
  - Delat medium (långsammare under högtrafik)
  - Asymmetriska hastigheter (nedladdning > uppladdning)
  - Kan ha datatak

### Trådlösa Teknologier

#### Mobilnät (4G/5G)
- **Hur det fungerar**: Använder radiovågor och mobilmaster
- **4G vs. 5G**:
  - 4G: Bättre räckvidd, mer lämplig för landsbygden
  - 5G: Högre hastigheter, lägre latens, kortare räckvidd (bättre för städer)
- **Fördelar**:
  - Mobilitet
  - Allt mer konkurrenskraftiga hastigheter
  - Ingen fysisk infrastruktur till varje byggnad
- **Nackdelar**:
  - Varierande tillförlitlighet baserat på plats
  - Påverkas av väder och hinder
  - Har ofta datatak
  - Delat medium (överbelastning under högtrafik)

#### Satellit
- **Hur det fungerar**: Kommunikationssatelliter vidarebefordrar data mellan markstationer
- **Typer**:
  - GEO (Geostationär): Hög omloppsbana, högre latens
  - LEO (Låg jordomloppsbana): Lägre omloppsbana, minskad latens (t.ex. Starlink)
- **Fördelar**:
  - Tillgänglig nästan överallt med fri sikt mot himlen
  - Oberoende av markbunden infrastruktur
- **Nackdelar**:
  - Högre latens (särskilt med GEO-satelliter)
  - Väderkänslig
  - Ofta dyr med datatak
  - Lägre hastigheter jämfört med fiber eller kabel

#### Wi-Fi
- **Hur det fungerar**: Trådlöst lokalt nätverk med radiovågor
- **Standarder**:
  - 802.11n (Wi-Fi 4): Upp till 600 Mbps
  - 802.11ac (Wi-Fi 5): Upp till 3,5 Gbps
  - 802.11ax (Wi-Fi 6): Upp till 9,6 Gbps
- **Fördelar**:
  - Inga kablar inom det lokala området
  - Lätt att lägga till enheter
  - Moderna standarder erbjuder bra hastigheter
- **Nackdelar**:
  - Begränsad räckvidd
  - Störningar från väggar och andra enheter
  - Säkerhetsproblem
  - Delat medium (långsammare med fler användare)

### Varför Trådbundna är Mer Tillförlitliga än Trådlösa
- **Fysisk isolering**: Skyddad från externa störningar
- **Dedikerat medium**: Delas inte med andra användare
- **Väderimmunitet**: Påverkas inte av miljöförhållanden
- **Konsekvent signal**: Förutsägbar dämpning över avstånd
- **Ingen frekvenskonkurrens**: Ingen konkurrens om spektrum
- **Lägre latens**: Färre variabler som påverkar överföringen
- **Fysisk säkerhet**: Svårare att avlyssna utan fysisk åtkomst

## Traceroute, Ping och Nätverksverktyg

### Traceroute
- **Syfte**: Fastställer vägen paket tar till en destination
- **Information som tillhandahålls**:
  - IP-adresser för varje router på vägen
  - Svarstid för varje hopp
  - Paketförlust vid varje hopp
  - Värdnamn (om reverse DNS är aktiverat)
- **Hur det fungerar**:
  - Skickar paket med ökande TTL-värden
  - När varje paket löper ut, skickar routrar ICMP Time Exceeded-meddelanden
  - Kartlägger rutten genom att analysera källan till dessa meddelanden
  - Windows-version (tracert) använder ICMP, Unix/Linux-version kan använda UDP eller TCP

### Ping
- **Syfte**: Testar grundläggande anslutning och mäter round-trip-tid
- **Information som tillhandahålls**:
  - Round-trip-tid (minimum, genomsnitt, maximum)
  - Paketförlustprocent
  - Mottagna TTL-värden
- **Hur det fungerar**:
  - Skickar ICMP Echo Request-paket
  - Destinationen svarar med ICMP Echo Reply-paket
  - Mäter tiden mellan att skicka och ta emot

### Andra Användbara Nätverksverktyg

#### nslookup / dig
- **Syfte**: Frågar DNS-servrar efter resursposter
- **Information som tillhandahålls**:
  - IP-adresser för domännamn
  - Mailservrar, namnservrar och andra DNS-poster
  - DNS-serversvarstid

#### netstat
- **Syfte**: Visar nätverksanslutningar, routingtabeller och gränssnittsstatistik
- **Information som tillhandahålls**:
  - Aktiva TCP-anslutningar
  - Lyssnande portar
  - Nätverksgränssnittsstatistik
  - Routingtabellinformation

#### Wireshark / tcpdump
- **Syfte**: Paketfångst och analys
- **Information som tillhandahålls**:
  - Detaljerat paketinnehåll
  - Protokollanalys
  - Nätverkstrafikmönster
  - Felsökningsinformation

## Nätverkssäkerhetskoncept

### Kryptering och Säkerhetsprotokoll

#### TLS/SSL
- **Syfte**: Säker kommunikation över datornätverk
- **Funktioner**:
  - Kryptering för integritet
  - Autentisering för att verifiera identiteter
  - Meddelandeintegritetskontroller
- **Hur det fungerar**:
  - Handskakningsprotokoll etablerar säker anslutning
  - Använder asymmetrisk kryptering för nyckelutbyte
  - Använder symmetrisk kryptering för dataöverföring
  - Används av HTTPS, FTPS, SMTPS, etc.

#### IPsec
- **Syfte**: Säkerhet på IP-lager
- **Funktioner**:
  - Autentisering
  - Kryptering
  - Dataintegritet
- **Lägen**:
  - Transportläge: Skyddar endast payload
  - Tunnelläge: Kapslar in hela IP-paketet (används i VPN)

### Brandväggstyper
- **Paketfiltrering**: Undersöker paketheaders mot regler
- **Tillståndsinspektion**: Spårar aktiva anslutningar
- **Applikationslager**: Analyserar paketinnehåll och applikationsspecifika beteenden
- **Nästa generation**: Kombinerar traditionella med nyare funktioner som intrångsförebyggande

### VPN (Virtuellt Privat Nätverk)
- **Syfte**: Utvidgar privat nätverk över offentliga nätverk
- **Typer**:
  - Site-to-site: Ansluter hela nätverk
  - Fjärråtkomst: Ansluter enskilda användare till nätverk
- **Teknologier**:
  - IPsec
  - SSL/TLS
  - WireGuard
  - OpenVPN

### Vanliga Nätverksattacker
- **DDoS (Distributed Denial of Service)**: Överbelastar mål med trafik
- **Man-in-the-Middle**: Avlyssnar kommunikation mellan två parter
- **ARP Poisoning**: Manipulerar ARP-tabeller för att omdirigera trafik
- **DNS Spoofing**: Korrumperar DNS-cache för att omdirigera trafik
- **Portskanning**: Söker efter öppna portar och sårbarheter

# HTTP och Webbteknologier

### HTTP Cookies
- **Definition**: En HTTP cookie är en liten datafil som webbservern sparar på besökarens dator
- **Syfte**:
  - Lagra användaridentitet och autentiseringsinformation
  - Spara användarpreferenser
  - Spåra användarbeteende över flera besök
  - Hantera sessioner
- **Livslängd**:
  - Sessionscookies: Försvinner när webbläsaren stängs
  - Beständiga cookies: Har ett specificerat utgångsdatum
- **Säkerhetsaspekter**:
  - HttpOnly: Förhindrar JavaScript från att komma åt cookien
  - Secure: Skickas endast över HTTPS-anslutningar
  - SameSite: Kontrollerar när cookies skickas med förfrågningar från andra webbplatser

### HTTP-metoder: GET vs POST

| Egenskap | GET | POST |
|---------|-----|------|
| Synlighet | Parametrar syns i URL | Data skickas i meddelandekroppen |
| Cachning | Kan cachas | Cachas normalt inte |
| Idempotens | Idempotent (upprepning ger samma resultat) | Inte idempotent |
| Datalängd | Begränsad (ca 2048 tecken i URL) | I princip obegränsad |
| Användningsområden | - Hämta data<br>- Sökfunktioner<br>- Navigationslänkar | - Formulärinlämningar<br>- Filuppladdningar<br>- Uppdatera/skapa data |
| Säkerhet | Mindre säker för känslig data | Säkrare för känslig data |

### Web-caching
- **Definition**: Temporär lagring av webbsidor/resurser för snabbare åtkomst
- **Typer**:
  - Webbläsarcaching: Lokalt på användarens dator
  - Proxy-caching: På en proxy-server mellan användare och webbserver
  - CDN (Content Delivery Network): Distribuerat nätverk av servrar
- **Fördelar**:
  - Minskar svarstid för användare
  - Reducerar bandbreddsanvändning
  - Minskar belastning på ursprungsservern
- **Utmaningar**:
  - Säkerställa att användare får uppdaterat innehåll
  - Hantera personligt innehåll som inte bör cachas
  - Konfiguration av cachningspolicy

## Beräkningar av Nätverksprestanda

### Beräkning av Datagramantal
För att beräkna antalet datagram som behövs för att överföra en fil:

1. **Formel**: Antal datagram = ⌈Filstorlek / Maximal datagramstorlek för data⌉
   
2. **Obs**: Vi behöver ta hänsyn till header-storleken
   - IPv4-header: 20-60 byte
   - IPv6-header: 40 byte
   - TCP-header: 20-60 byte
   - UDP-header: 8 byte

3. **Exempel**:
   - Fil: 1456 kB (1456000 byte)
   - Maximal datagramstorlek: 1500 byte
   - IPv6-header: 40 byte
   - Tillgänglig plats för data: 1500 - 40 = 1460 byte
   - Antal datagram: ⌈1456000 / 1460⌉ = ⌈997,26⌉ = 998 datagram
   - **OBS** EXEMPLET ÄR FELAKTIGT. TCP HEADER behöver också inkluderas.

### Beräkning av Överföringstider

#### Överföringsfördröjning (Transmission Delay)
- **Formel**: d_trans = Datastorlek (bitar) / Bandbredd (bps)
- **Exempel**:
  - Fil: 2 MB (16 Mbits)
  - Länkhastighet: 10 Mbps
  - Överföringsfördröjning = 16 Mbit / 10 Mbps = 1,6 sekunder

#### Utbredningsfördröjning (Propagation Delay)
- **Formel**: d_prop = Avstånd / Signalhastighet
- **Exempel**:
  - Avstånd: 5000 km
  - Signalhastighet: 2 × 10^8 m/s (ungefär 2/3 av ljusets hastighet i vakuum)
  - Utbredningsfördröjning = 5000000 / (2 × 10^8) = 0,025 sekunder = 25 ms

#### Total Fördröjning (End-to-End Delay)
- **Formel**: d_total = d_proc + d_queue + d_trans + d_prop
  - d_proc: Bearbetningsfördröjning
  - d_queue: Köfördröjning
  - d_trans: Överföringsfördröjning
  - d_prop: Utbredningsfördröjning

## DHCP i Praktiken

### DHCP Process i Detalj

1. **DHCP Discovery** (Klient → Broadcast)
   - Klienten skapar en DHCPDISCOVER-paket
   - Skickas till broadcast-adressen 255.255.255.255 (destination) och 0.0.0.0 (källa)
   - Innehåller klientens MAC-adress och ev. begäran om specifik IP

2. **DHCP Offer** (Server → Klient)
   - DHCP-servern svarar med DHCPOFFER
   - Innehåller: 
     - Föreslagen IP-adress
     - Subnätmask
     - Lånetid
     - Server-ID (serveradress)

3. **DHCP Request** (Klient → Broadcast)
   - Klienten väljer ett erbjudande (om det finns flera)
   - Sänder DHCPREQUEST till broadcast-adressen
   - Innehåller vald server-ID för att identifiera vilken server som valdes
   - Broadcast används för att informera andra DHCP-servrar att deras erbjudanden avvisats

4. **DHCP Acknowledgment** (Server → Klient)
   - Servern bekräftar tilldelningen med DHCPACK
   - Innehåller:
     - Bekräftad IP-adress
     - Alla konfigurationsparametrar
     - Lånetid

### DHCP Förnyelse och Frisläppning

- **Förnyelse (Renewal)**:
  - Klienten försöker förnya sin lease när 50% av lånetiden har passerat
  - Skickar DHCPREQUEST direkt till servern som gav IP-adressen
  - Om ingen bekräftelse erhålls när 87.5% av lånetiden gått, försöker klienten få en ny IP-adress

- **Frisläppning (Release)**:
  - När en klient lämnar nätverket kan den skicka DHCPRELEASE
  - Informerar servern att IP-adressen inte längre används
  - Servern kan återanvända adressen för nya förfrågningar

### Fördelar med Automatisk IP-adresstilldelning

1. **Centraliserad hantering**:
   - Undviker IP-adresskonflikter genom automatisk allokering
   - Förenklar administrering av stora nätverk

2. **Effektiv adressanvändning**:
   - Återanvänder adresser när enheter lämnar nätverket
   - Minskar slöseri med IP-adresser

3. **Mobilitetshantering**:
   - Underlättar för användare som flyttar mellan nätverk
   - Automatisk konfiguration utan manuell inställning

4. **Konsekvent konfiguration**:
   - Säkerställer att alla enheter får korrekta nätverksinställningar
   - Minskar konfigurationsfel

### DHCP-tillhandahållen Information

- **Obligatorisk information**:
  - IP-adress
  - Subnätmask
  - Lånetid
  - Server-ID

- **Vanlig tilläggsinformation**:
  - Standard-gateway (router)
  - DNS-serveradresser
  - Domännamn
  - NTP-servrar (tidservrar)
  - WINS-servrar (Windows)
  - Proxyinställningar

## E-postprotokoll

- **SMTP (Simple Mail Transfer Protocol)**
  - Syfte: Överför e-post mellan servrar och från klient till server
  - Port: 25 (standard), 587 (krypterad)
  - Grundfunktion: Etablerar anslutning och överför meddelanden

- **Nerladdningsprotokoll**
  - **POP3**: Hämtar e-post från server till klient, raderar vanligtvis från server
  - **IMAP**: Hanterar e-post direkt på servern, möjliggör synkronisering mellan flera enheter


## WLAN och Trådlösa Teknologier

### Beacon Frames i WiFi-nätverk

- **Definition**: Paket som regelbundet sänds ut av accesspunkter för att annonsera sina nätverk
- **Innehåll**:
  - SSID (Service Set Identifier): Nätverkets namn
  - MAC-adress till accesspunkten (BSSID)
  - Tidssynkroniseringsinformation
  - Stödda hastigheter
  - Säkerhetsfunktioner
  - Kanalinformation
- **Funktioner**:
  - Annonserar nätverkets existens
  - Möjliggör för klienter att hitta och ansluta till nätverk
  - Upprätthåller synkronisering mellan anslutna enheter
- **Vad innehåller INTE beacon frames**:
  - IP-adresser (IP-adresstilldelning sker via DHCP efter anslutning)
  - Användarautentiseringsuppgifter

### Skillnader mellan Trådbundna och Trådlösa Nätverk

| Egenskap | Trådbundna Nätverk | Trådlösa Nätverk |
|----------|-------------------|-----------------|
| **Fördelar** | • Högre hastighet<br>• Mer tillförlitligt<br>• Säkrare<br>• Mindre interferens | • Mobilitet<br>• Flexibel installation<br>• Enklare utbyggnad<br>• Inga kablar |
| **Nackdelar** | • Begränsad mobilitet<br>• Kräver kabeldragning<br>• Svårare att installera i vissa miljöer | • Lägre hastighet<br>• Säkerhetsrisker<br>• Mer interferens<br>• Täckningsproblem |
| **Fysiskt medium** | Koppar- eller fiberoptiska kablar | Radiovågor (elektromagnetisk strålning) |
| **Säkerhet** | Naturligt isolerad från extern åtkomst | Kräver kryptering (WPA2/WPA3) |
| **Störningskänslighet** | Låg (särskilt fiber) | Hög (påverkas av väggar, elektronik, etc.) |

### Personal Area Networks (PAN)

- **Definition**: Personliga nätverk med kort räckvidd för anslutning av enheter runt en person
- **Exempel**:
  - Bluetooth: För anslutning av telefoner, hörlurar, högtalare, etc.
  - Zigbee: För smart hem-enheter, sensorer, etc.
  - Near Field Communication (NFC): För betalning, ID, etc.
- **Egenskaper**:
  - Kort räckvidd (vanligtvis <10 meter)
  - Låg strömförbrukning
  - Generellt lägre bandbredd än WLAN
  - Ofta mesh-nätverk (Zigbee) eller punkt-till-punkt (Bluetooth)
- **Användningsområden**:
  - Smarta hem-enheter
  - Wearables (smartwatches, träningsarmband)
  - Kortdistansöverföring mellan mobila enheter
  - Medicinsk övervakning

### Ad-hoc-nätverk

- **Definition**: Tillfälliga nätverk som bildas direkt mellan enheter utan att använda en centraliserad accesspunkt
- **Egenskaper**:
  - Ingen central styrpunkt
  - Dynamisk topologi
  - Själv-kongurerande
  - Ingen fördefinierad infrastruktur
- **Exempel**:
  - Bluetooth Direct Connect
  - WiFi Direct
  - MANET (Mobile Ad-hoc Network)
- **Användningsområden**:
  - Fildelning mellan enheter
  - Temporära nätverk vid katastrofer
  - Militära tillämpningar
  - Sensornätverk

## Mobila Nätverk

### Utveckling av Mobilstandarder

| Generation | Viktiga Funktioner | Datateknik | Talhantering |
|------------|-------------------|------------|--------------|
| 2G (GSM) | • Digitalt tal<br>• SMS<br>• Data upp till 384 kbps | Kretsförmedlad | Kretsförmedlad |
| 3G (UMTS) | • Förbättrad datahastighet<br>• Videosamtal<br>• Data upp till 42 Mbps | Paketförmedlad (primärt) | Kretsförmedlad (primärt) |
| 4G (LTE) | • Höghastighetsdata<br>• IP-baserad<br>• Data upp till 1 Gbps | Paketförmedlad | Paketförmedlad (VoLTE) |
| 5G | • Ultra-låg latens<br>• Massiv IoT-anslutning<br>• Data upp till 10 Gbps | Paketförmedlad | Paketförmedlad |

### 4G/LTE Specifika Egenskaper

- **All-IP Nätverk**:
  - Alla tjänster, inklusive tal, använder IP-protokollet
  - Paketförmedlad teknik för all kommunikation
  - Eliminerar behov av kretsförmedlad infrastruktur

- **Voice over LTE (VoLTE)**:
  - Röstsamtal som IP-datapaket
  - Fördelar:
    - Högre röstljudkvalitet (HD Voice)
    - Snabbare samtalsuppkoppling
    - Parallell dataanvändning utan hastighetsminskning
    - Effektivare spektrumanvändning


## TCP i Praktiken

### Beräkning av TCP Sekvensnummer och ACK-nummer

- **Sekvensnummer**:
  - Initialt slumpvärde vid anslutningsetablering (SYN)
  - Räknar byte-position i dataströmmen (inte paketantal)

- **ACK-nummer**:
  - Representerar nästa förväntade byte
  - ACK N innebär att alla byte fram till (men exklusive) byte N har mottagits

- **Beräkningsexempel**:
  - Initial sekvensnummer: 650
  - 8 paket skickas med 900 byte i varje
  - Total mängd överförda data: 8 × 900 = 7200 byte
  - Förväntat ACK-nummer: 650 + 7200 = 7850

### TCP Flödeskontroll i Praktiken

- **Mottagarfönster (rwnd)**:
  - Flödeskontrollmekanism baserad på mottagarens buffertkapacitet
  - Annonseras i varje ACK-paket
  - Mäts i byte
  - Dynamiskt justeras baserat på mottagarens buffertillgänglighet

- **Exempel**:
  - Mottagare har 64 KB buffert
  - 32 KB redan använt i bufferten
  - Advertised window (rwnd) = 32 KB
  - Sändaren får inte ha mer än 32 KB obekräftade data

### TCP Överbelastningskontroll i Praktiken

- **Överbelastningsfönster (cwnd)**:
  - Begränsar mängden obekräftade data baserat på uppfattad nätverkskapacitet
  - Hanteras av sändaren (inte annonserat i protokollet)
  - Dynamiskt justeras baserat på nätverksförhållanden

- **Överbelastningshanteringsalgoritmer**:
  1. **Slow Start**:
     - Börjar med litet cwnd (vanligtvis 1-10 MSS)
     - Fördubblar cwnd för varje RTT
     - Fortsätter tills ssthresh nås eller paketförlust detekteras

  2. **Överbelastningsundvikande**:
     - Linjär ökning: cwnd += 1/cwnd för varje ACK
     - Ungefär en ökning av cwnd med 1 MSS per RTT
     - Mer försiktig tillväxt än Slow Start

  3. **Fast Recovery**:
     - Efter Fast Retransmit (3 duplicerade ACKs)
     - Reducerar cwnd till hälften, sedan linjär ökning
     - Undviker att gå tillbaka till Slow Start

- **Exempel**:
  - Initialt cwnd = 4 MSS
  - Efter 1 RTT utan förlust: cwnd = 8 MSS
  - Efter 2 RTT utan förlust: cwnd = 16 MSS
  - Paketförlust detekteras: cwnd minskas till 8 MSS
  - Inträder överbelastningsundvikande: cwnd ökar med ~1 MSS per RTT

---
