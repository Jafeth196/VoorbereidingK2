# B1-K2-W1 – Installeert en configureert netwerk- en infrastructuuronderdelen

> Cheatsheet voor het examen. Gebaseerd op de proeftoets **Solar Plex Arnhem** (Packet Tracer).
> Op het examen krijg je waarschijnlijk **andere namen, VLAN's en IP's**. Leer dus de **werkwijze**, niet de getallen.
> Alle getallen hieronder zijn **voorbeelden uit de proeftoets**. Neem op het examen altijd de waarden uit **jouw** technisch ontwerp.
>
> Volledige commandolijst: zie de [CCNA-commandolijst](CCNA-commandos.md). Hier staat alleen wat je voor W1 nodig hebt, in de volgorde waarin je het doet.

---

## Inhoud

1. [Hoe je beoordeeld wordt](#1-hoe-je-beoordeeld-wordt)
2. [Voor je begint (5 minuten)](#2-voor-je-begint-5-minuten)
3. [Stap A – Bouwen en kabels](#3-stap-a--bouwen-en-kabels)
4. [Stap B – NIC's vervangen (WMP300N)](#4-stap-b--nics-vervangen-wmp300n)
5. [Stap C – VLAN's op de core](#5-stap-c--vlans-op-de-core)
6. [Stap D – VTP (server en clients)](#6-stap-d--vtp-server-en-clients)
7. [Stap E – Root bridge (spanning tree)](#7-stap-e--root-bridge-spanning-tree)
8. [Stap F – SVI's (IP per VLAN) + ip routing](#8-stap-f--svis-ip-per-vlan--ip-routing)
9. [Stap G – DHCP-pools](#9-stap-g--dhcp-pools)
10. [Stap H – Trunks](#10-stap-h--trunks)
11. [Stap I – OSPF](#11-stap-i--ospf)
12. [Stap J – Access-switches (hostname + access-poorten)](#12-stap-j--access-switches-hostname--access-poorten)
13. [Stap K – Wifi (AP, laptop, printer)](#13-stap-k--wifi-ap-laptop-printer)
14. [Stap L – ACL voor gastwifi](#14-stap-l--acl-voor-gastwifi)
15. [Stap M – Opslaan en Check Results](#15-stap-m--opslaan-en-check-results)
16. [Testrapport schrijven](#16-testrapport-schrijven)
17. [Troubleshooting-checklist (deeltesten)](#17-troubleshooting-checklist-deeltesten)
18. [Veelgemaakte fouten](#18-veelgemaakte-fouten)
19. [Snelle naslag (tabellen)](#19-snelle-naslag-tabellen)

---

## 1. Hoe je beoordeeld wordt

Per criterium 0–3 punten (0 = niet of nauwelijks, 1 = enigszins, 2 = grotendeels, 3 = volledig). **Max 9 punten.**

| Criterium | Wat de beoordelaar wil zien | Hoe je punten haalt |
|---|---|---|
| **T1 Installeren** | Apparaten zijn conform opdracht geïnstalleerd | Alle apparaten uit de "voorraadkast" geplaatst, **juiste kabel op de juiste poort** volgens de aansluitingentabel, juiste NIC's ingebouwd |
| **T1 Configureren** | Apparaten zijn conform opdracht geconfigureerd | VLAN's, VTP, root, SVI's, DHCP, trunks, OSPF, wifi, ACL **precies** zoals in het ontwerp (namen, hoofdletters, wachtwoorden) |
| **T2 Testen** | Werking getest, fouten **gelokaliseerd** en **passend afgehandeld** | Elke test: uitvoering, verwacht, werkelijk, screenshots. Werkt iets niet? Laat met **deeltesten** zien tot waar het wél werkt |

**Cijfer:** 9 = 10 · 8 = 9 · 7 = 8 · 6 = 7 · 5 = 6 · 4 = 5 · 3 = 4.

> **Belangrijk:** bij T2 kun je ook punten halen als iets **niet** werkt. "Ik denk dat het DHCP is" levert niks op. "PC4 heeft 169.254.x.x, de trunk Fa0/1 staat niet in `show interfaces trunk`, dus VLAN 30 komt niet bij SW01" levert wél punten op.

---

## 2. Voor je begint (5 minuten)

1. **Lees het hele technisch ontwerp één keer helemaal door** voordat je iets doet.
2. Markeer per apparaat wat er moet gebeuren. Maak voor jezelf een lijstje:

| Apparaat | Wat moet erop |
|---|---|
| Core (multilayer, bijv. 3560) | VLAN's, VTP **server**, root, SVI's, `ip routing`, DHCP-pools, trunks, OSPF, ACL |
| Access-switches (2960) | hostname, VTP **client**, trunks naar core/andere switch, access-poorten |
| AP's | SSID, WPA2-PSK, wachtwoord |
| Laptops/printer | NIC vervangen, wifi, DHCP of vast IP |
| PC's | DHCP |

3. Zet in Packet Tracer aan: **Options > Preferences > "Always Show Port Labels in Logical Workspace"**. Dan zie je bij elke kabel de poortnaam.
4. Let op het verschil tussen locaties. In de proeftoets stond er een wifi-tabel van **Doetinchem** in het ontwerp, maar jij bouwde **Arnhem**. Doe alleen wat bij jouw opdracht hoort.

---

## 3. Stap A – Bouwen en kabels

**Wat:** apparaten uit de voorraadkast halen en aansluiten volgens de **aansluitingentabel**.

### Welke kabel?

| Van ↔ Naar | Kabel in Packet Tracer |
|---|---|
| PC/laptop ↔ switch | Copper **Straight-Through** |
| AP ↔ switch | Copper **Straight-Through** |
| Router ↔ switch | Copper **Straight-Through** |
| Switch ↔ switch | Copper **Cross-Over** |
| PC ↔ router | Copper **Cross-Over** |
| PC ↔ console-poort | **Console** (blauw) |

### Werkwijze
- Kies de kabel **handmatig** en klik zelf de juiste poort aan. Gebruik **niet** de bliksem ("automatically choose connection type"): die kiest zelf een poort en dat is vaak de verkeerde.
- Lees de tabel als zin: *"Van SW01-ARN poort Fa0/24 naar SW02-ARN poort Fa0/23"*. Let goed op dat de poortnummers aan beide kanten **verschillend** kunnen zijn.

### Controleren
- Muis stil op een kabel houden: geel kadertje met de poorten aan beide kanten.
- In de CLI: `show cdp neighbors` (Cisco-buren, met Local Intrfce en Port ID).

```
SW02-ARN#show cdp neighbors
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
Core01_ARN   Fas 0/3          166          S         3560        Fas 0/3
SW01-ARN     Fas 0/23         166          S         2960        Fas 0/24
```
Lezen: *"Mijn poort Fa0/23 zit aan SW01-ARN op zijn poort Fa0/24."* Klopt dat met de tabel? Dan zit de kabel goed.

### Lampjes
| Kleur | Betekenis |
|---|---|
| Groen | Link up |
| Oranje | Poort wordt opgestart, of **STP blokkeert hem** (bij een lus tussen switches is dat normaal) |
| Rood | Geen link: verkeerde kabel, poort `shutdown`, of apparaat uit |

---

## 4. Stap B – NIC's vervangen (WMP300N)

Voorbeeld proeftoets: Laptop_gast, Laptop 2 en Printer0 krijgen een **WMP300N** (wireless 2.4 GHz).

1. Klik op het apparaat > tab **Physical**.
2. Zet het apparaat **uit** (aan/uit-knopje op de afbeelding). Zonder uitzetten kun je geen module wisselen.
3. Sleep de oude kaart (bijv. PT-LAPTOP-NM-1CFE) **uit** het apparaat naar de lijst links.
4. Sleep de **WMP300N** in de lege sleuf.
5. Zet het apparaat weer **aan**.
6. Check: tab **Config** > er staat nu een interface **Wireless0**.

> Een kaart van 2.4 GHz kan **niet** verbinden met een 5 GHz-netwerk. Moet iets op 5 GHz, kies dan een kaart die dat ondersteunt.

---

## 5. Stap C – VLAN's op de core

**Alleen op de VTP-server (de core).** De clients krijgen de VLAN's automatisch via VTP.

```
Core01_ARN> enable
Core01_ARN# configure terminal
Core01_ARN(config)# vlan 30
Core01_ARN(config-vlan)# name Productie
Core01_ARN(config-vlan)# vlan 40
Core01_ARN(config-vlan)# name Marketing
Core01_ARN(config-vlan)# vlan 50
Core01_ARN(config-vlan)# name Gasten_Wifi
Core01_ARN(config-vlan)# exit
```
Doe dit voor **alle** VLAN's uit de tabel (in de proeftoets ook 5 IT, 10 Management, 20 HR & Finance).

### Controleren
```
Core01_ARN# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/4, ...
5    IT                               active
30   Productie                        active
40   Marketing                        active
50   Gasten_Wifi                      active    Fa0/24
```
- Staan alle VLAN-nummers erin met de **juiste naam** (hoofdlettergevoelig)?
- Trunkpoorten staan **niet** in deze lijst. Dat is normaal.

---

## 6. Stap D – VTP (server en clients)

**VTP** = de server deelt zijn VLAN-lijst via trunks uit aan de clients. Je maakt VLAN's dus maar op één plek.

### Core (server)
```
Core01_ARN(config)# vtp domain SOLAR
Core01_ARN(config)# vtp mode server
```

### Access-switches (client)
```
SW01-ARN(config)# vtp domain SOLAR
SW01-ARN(config)# vtp mode client
```
(Staat er een VTP-wachtwoord in het ontwerp: `vtp password <wachtwoord>` op **alle** switches, exact gelijk.)

### Controleren
```
SW01-ARN# show vtp status
VTP Version                     : 2
Configuration Revision          : 6
Number of existing VLANs        : 11
VTP Operating Mode              : Client
VTP Domain Name                 : SOLAR
```
| Regel | Wat moet je zien |
|---|---|
| VTP Domain Name | **Exact** hetzelfde als op de core (hoofdlettergevoelig: `SOLAR` ≠ `Solar`) |
| VTP Operating Mode | Core = Server, access-switches = Client |
| Configuration Revision | Op de client **gelijk** aan de server. Lager? Dan komt de info niet aan |
| Number of existing VLANs | Gelijk aan de server |

Daarna op de client: `show vlan brief`. Staan VLAN 30/40/50 erin? Dan werkt VTP.

> **Melding op een client:** `VTP VLAN configuration not allowed when device is in CLIENT mode.` Dat is **goed**: een client mag geen VLAN's maken. Maak ze op de core.
>
> **VTP werkt alleen over een trunk.** Zie je de VLAN's niet op de client: eerst de trunk checken (stap H).

---

## 7. Stap E – Root bridge (spanning tree)

### Onderzoeken wie de root is (opdracht "onderzoek welke switch de root is")
```
Core01_ARN# show spanning-tree vlan 30
VLAN0030
  Spanning tree enabled protocol ieee
  Root ID    Priority    32798
             Address     0001.C7A2.1B03
             Cost        19
             Port        1(FastEthernet0/1)
  Bridge ID  Priority    32798  (priority 32768 sys-id-ext 30)
             Address     0060.5C3E.88D1
```
| Wat je ziet | Betekenis |
|---|---|
| `This bridge is the root` | Deze switch **is** de root |
| Root ID Address ≠ Bridge ID Address | Deze switch is **niet** de root. Het Root ID-adres hoort bij de root. Zoek via `show spanning-tree` op de andere switches welke dat MAC-adres heeft |
| Allemaal priority 32768 | Dan wint het **laagste MAC-adres** |

Maak een screenshot van deze output **vóór** je het aanpast (bewijs voor je onderzoek).

### Core root maken voor alle VLAN's
```
Core01_ARN(config)# spanning-tree vlan 1,5,10,20,30,40,50 root primary
```
(Of met een vaste prioriteit: `spanning-tree vlan 1,5,10,20,30,40,50 priority 4096`.)

### Controleren
```
Core01_ARN# show spanning-tree vlan 30
  Root ID    Priority    24606
             Address     0060.5C3E.88D1
             This bridge is the root
```
Check dit voor elk VLAN (of `show spanning-tree` en scroll).

---

## 8. Stap F – SVI's (IP per VLAN) + ip routing

**SVI** = een virtuele interface per VLAN op de multilayer switch. Dit wordt de **default gateway** van dat VLAN.

```
Core01_ARN(config)# ip routing

Core01_ARN(config)# interface vlan 1
Core01_ARN(config-if)# ip address 10.10.0.2 255.255.255.0
Core01_ARN(config-if)# no shutdown

Core01_ARN(config-if)# interface vlan 30
Core01_ARN(config-if)# ip address 10.10.30.1 255.255.255.0
Core01_ARN(config-if)# no shutdown

Core01_ARN(config-if)# interface vlan 40
Core01_ARN(config-if)# ip address 10.10.40.1 255.255.255.0
Core01_ARN(config-if)# no shutdown

Core01_ARN(config-if)# interface vlan 50
Core01_ARN(config-if)# ip address 10.10.50.1 255.255.255.0
Core01_ARN(config-if)# no shutdown
```

> **`ip routing` niet vergeten.** Zonder dit routeert de multilayer switch niet tussen VLAN's en werkt OSPF niet.

### Controleren
```
Core01_ARN# show ip interface brief | include Vlan
Vlan1        10.10.0.2     YES manual up     up
Vlan30       10.10.30.1    YES manual up     up
Vlan40       10.10.40.1    YES manual up     up
Vlan50       10.10.50.1    YES manual down   down
```
| Status | Betekenis |
|---|---|
| `up / up` | Goed |
| `administratively down` | `no shutdown` vergeten |
| `down / down` | VLAN bestaat niet, of er is **geen actieve poort** in dat VLAN en geen trunk die het doorlaat. Lost zich vaak op zodra de kabels/trunks goed zijn |

---

## 9. Stap G – DHCP-pools

**Volgorde:** eerst `excluded-address`, dan de pool. Zo deelt de pool nooit een adres uit dat al vast in gebruik is (gateway, printer, servers).

Tip uit de proeftoets: "Kijk op de Core01_HK hoe de pools zijn aangemaakt". Doe dat echt: `show running-config | section dhcp` op die switch laat precies de schrijfwijze zien die verwacht wordt (bijv. de poolnaam).

```
Core01_ARN(config)# ip dhcp excluded-address 10.10.30.1 10.10.30.50
Core01_ARN(config)# ip dhcp excluded-address 10.10.40.1 10.10.40.50
Core01_ARN(config)# ip dhcp excluded-address 10.10.50.1 10.10.50.50

Core01_ARN(config)# ip dhcp pool Vlan30
Core01_ARN(dhcp-config)# network 10.10.30.0 255.255.255.0
Core01_ARN(dhcp-config)# default-router 10.10.30.1
Core01_ARN(dhcp-config)# dns-server 62.36.55.85
Core01_ARN(dhcp-config)# exit
```
Herhaal voor elke pool. **Per pool hetzelfde derde octet** in network, default-router en exclusions:

| Pool | network | default-router | excluded | dns-server |
|---|---|---|---|---|
| Vlan30 | 10.10.**30**.0 | 10.10.**30**.1 | 10.10.**30**.1 – 10.10.**30**.50 | 62.36.55.85 |
| Vlan40 | 10.10.**40**.0 | 10.10.**40**.1 | 10.10.**40**.1 – 10.10.**40**.50 | 62.36.55.85 |
| Vlan50 | 10.10.**50**.0 | 10.10.**50**.1 | 10.10.**50**.1 – 10.10.**50**.50 | 62.36.55.85 |

### Een pool verbeteren
Typ gewoon opnieuw `ip dhcp pool Vlan40` en zet de foute regel opnieuw met de goede waarde. De nieuwe waarde overschrijft de oude.

### Controleren
```
Core01_ARN# show running-config | section dhcp
Core01_ARN# show ip dhcp binding
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
10.10.30.51      0001.6421.A1B2           --                     Automatic
10.10.50.51      00D0.BA44.1C09           --                     Automatic
```
- Een client heeft een adres vanaf **.51** (want .1–.50 is uitgesloten). Goed.
- Lege binding-lijst? De DHCP-verzoeken komen niet aan bij de core. Kijk naar trunks/VTP/access-poorten.

> **Check Results zegt dat alleen DNS en subnetmask goed zijn?** Dan klopt network/default-router niet. Dat was in de oefening een **kopieerfout**: pool Vlan40 met de adressen van VLAN 30.

---

## 10. Stap H – Trunks

**Trunk** = kabel tussen switches die **meerdere VLAN's** draagt (getagd met 802.1Q).
**Access** = poort naar één eindapparaat (pc, AP, printer), in **één** VLAN.

**Regel:** staat er aan de andere kant een **switch** → trunk. Staat er een **pc/AP/printer/laptop** → access.

### Op de core (3560 multilayer): éérst encapsulation
```
Core01_ARN(config)# interface range fa0/1 , fa0/3
Core01_ARN(config-if-range)# switchport trunk encapsulation dot1q
Core01_ARN(config-if-range)# switchport mode trunk
```
> Op een 3560 weigert `switchport mode trunk` als je eerst geen encapsulation kiest:
> `Command rejected: An interface whose trunk encapsulation is "Auto" can not be configured to "trunk" mode.`

"Laat alle VLAN's door" = **geen** `allowed vlan`-lijst instellen. Standaard mag alles.

### Op de access-switches (2960): alleen mode trunk
Een 2960 kent alleen dot1q, dus geen encapsulation-commando.
```
SW01-ARN(config)# interface fa0/1
SW01-ARN(config-if)# switchport mode trunk
SW01-ARN(config-if)# interface fa0/24
SW01-ARN(config-if)# switchport mode trunk

SW02-ARN(config)# interface fa0/3
SW02-ARN(config-if)# switchport mode trunk
SW02-ARN(config-if)# interface fa0/23
SW02-ARN(config-if)# switchport mode trunk
```
**Een trunk moet aan beide kanten van de kabel trunk zijn.**

### Per ongeluk access gemaakt? Terugzetten:
```
Core01_ARN(config)# interface range fa0/1 , fa0/3
Core01_ARN(config-if-range)# no switchport access vlan
Core01_ARN(config-if-range)# switchport trunk encapsulation dot1q
Core01_ARN(config-if-range)# switchport mode trunk
```

### Controleren
```
Core01_ARN# show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1
Fa0/3       on           802.1q         trunking      1

Port        Vlans allowed on trunk
Fa0/1       1-1005
Fa0/3       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,5,10,20,30,40,50
Fa0/3       1,5,10,20,30,40,50
```
| Wat je ziet | Betekenis |
|---|---|
| Poort staat in de lijst, Status `trunking` | Trunk werkt |
| Poort staat er **niet** in | Geen trunk (nog access, of andere kant niet goed) |
| "allowed and active" mist een VLAN | VLAN bestaat niet op deze switch (VTP-probleem) of is uit de allowed-lijst gehaald |

Eén poort zeker weten: `show interfaces fa0/1 switchport` → regel **Operational Mode**: `trunk` of `static access`.

---

## 11. Stap I – OSPF

Het commando staat letterlijk in het ontwerp. Typ het precies over:
```
Core01_ARN(config)# router ospf 1
Core01_ARN(config-router)# network 10.0.0.0 0.255.255.255 area 0
```
Wildcard `0.255.255.255` = alles wat met 10. begint doet mee.

### Controleren
```
Core01_ARN# show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
10.10.0.1         1   FULL/DR         00:00:36    10.10.0.1       Vlan1

Core01_ARN# show ip route
O    172.16.5.0/24 [110/2] via 10.10.0.1, 00:01:10, Vlan1
O*E2 0.0.0.0/0 [110/1] via 10.10.0.1, ...
C    10.10.30.0/24 is directly connected, Vlan30
```
- State **FULL** = buren werken.
- `O` in de routetabel = geleerd via OSPF. Zie je routes naar 172.16.x.x (hoofdkantoor)? Dan kun je straks naar het intranet.
- Geen buren? Check `ip routing`, of `interface vlan 1` up is, en of de kabel naar de router goed zit.

---

## 12. Stap J – Access-switches (hostname + access-poorten)

### Hostname
```
Switch(config)# hostname SW01-ARN
```
Precies zoals in het ontwerp (streepje vs. underscore: `SW01-ARN` is niet `SW01_ARN`).

### Access-poorten
Je typt dit **op de switch**, op de poort **waar het eindapparaat aan zit**. Niet op de pc.
```
SW01-ARN(config)# interface range fa0/4 - 5
SW01-ARN(config-if-range)# switchport mode access
SW01-ARN(config-if-range)# switchport access vlan 30

SW02-ARN(config)# interface fa0/4
SW02-ARN(config-if)# switchport mode access
SW02-ARN(config-if)# switchport access vlan 40
SW02-ARN(config-if)# interface g0/1
SW02-ARN(config-if)# switchport mode access
SW02-ARN(config-if)# switchport access vlan 40
```
De **AP** krijgt zijn VLAN via de switchpoort. Op de AP zelf stel je geen VLAN in.

Gastwifi: de poort op de core naar AP_Gast (Fa0/24) wordt access in VLAN 50:
```
Core01_ARN(config)# interface fa0/24
Core01_ARN(config-if)# switchport mode access
Core01_ARN(config-if)# switchport access vlan 50
```

### Controleren
```
SW01-ARN# show vlan brief
VLAN Name          Status    Ports
1    default       active    Fa0/2, Fa0/3, Fa0/6, ...
30   Productie     active    Fa0/4, Fa0/5
40   Marketing     active
```
Staat Fa0/4 achter het goede VLAN? Goed.

---

## 13. Stap K – Wifi (AP, laptop, printer)

In Packet Tracer heet "WPA2-personal" **WPA2-PSK**. Let op: **niet** WPA-PSK kiezen (dat is een andere optie en kost punten).

### Access Point
AP > tab **Config** > links **Port 1**:

| Veld | Waarde (voorbeeld AP02) | Waarde (voorbeeld AP_Gast) |
|---|---|---|
| SSID | `Solar_Arnhem` | `Solar_gast` |
| Authentication | **WPA2-PSK** | **WPA2-PSK** |
| PSK Pass Phrase | `solar12345` | `welkom123` |
| Encryption Type | AES | AES |

### Laptop (DHCP)
Laptop > **Config** > **Wireless0**:
- SSID: exact hetzelfde als de AP (hoofdlettergevoelig)
- Authentication: WPA2-PSK, zelfde wachtwoord
- IP Configuration: **DHCP**

Of via **Desktop > PC Wireless** > Connect.

### Printer (vast IP)
Printer > **Config** > **Wireless0**: SSID + WPA2-PSK + wachtwoord, IP Configuration **Static**:
```
IP Address:      10.10.40.60
Subnet Mask:     255.255.255.0
Default Gateway: 10.10.40.1
```
(Config > Settings: ook de Gateway daar invullen als het veld er staat.)

### PC's (DHCP)
PC > **Desktop > IP Configuration** > **DHCP**. Na even wachten: "DHCP request successful".

### Controleren
- Laptop: in de topologie verschijnt een stippellijn/golfjes naar de AP.
- `Desktop > Command Prompt > ipconfig`:
```
C:\>ipconfig
Wireless0 Connection:(default port)
   IPv4 Address....................: 10.10.50.51
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: 10.10.50.1
```
| Wat je ziet | Betekenis |
|---|---|
| Adres in het juiste subnet, gateway klopt | Goed |
| **169.254.x.x** (APIPA) | **Geen** DHCP-antwoord gekregen. Zie troubleshooting |
| 0.0.0.0 of geen verbinding | Wifi-instellingen kloppen niet (SSID/wachtwoord/WPA2) |

---

## 14. Stap L – ACL voor gastwifi

Doel: gasten (10.10.50.0/24) mogen **niet** naar de interne netwerken, wel naar internet (www.solar.com).

```
Core01_ARN(config)# access-list 150 deny ip 10.10.50.0 0.0.0.255 10.10.30.0 0.0.0.255
Core01_ARN(config)# access-list 150 deny ip 10.10.50.0 0.0.0.255 10.10.40.0 0.0.0.255
Core01_ARN(config)# access-list 150 deny ip 10.10.50.0 0.0.0.255 172.16.10.0 0.0.0.255
Core01_ARN(config)# access-list 150 deny ip 10.10.50.0 0.0.0.255 172.16.5.0 0.0.0.255
Core01_ARN(config)# access-list 150 deny ip 10.10.50.0 0.0.0.255 172.16.20.0 0.0.0.255
Core01_ARN(config)# access-list 150 permit ip any any

Core01_ARN(config)# interface vlan 50
Core01_ARN(config-if)# ip access-group 150 in
```

### Waarom zo?
| Onderdeel | Uitleg |
|---|---|
| `150` | Nummer 100–199 = **extended** ACL (kijkt naar bron **én** bestemming) |
| `deny ip <bron> <wildcard> <bestemming> <wildcard>` | Blokkeer al het IP-verkeer van gasten naar dat netwerk |
| `0.0.0.255` | Wildcard voor een /24 (omgekeerd subnetmasker) |
| `permit ip any any` | **Verplicht aan het eind**, anders blokkeert de onzichtbare `deny any` alles (ook internet) |
| `interface vlan 50` + `in` | Verkeer van gasten komt de core **binnen** via de SVI van VLAN 50 |

### Controleren
```
Core01_ARN# show access-lists
Extended IP access list 150
    10 deny ip 10.10.50.0 0.0.0.255 10.10.30.0 0.0.0.255 (4 match(es))
    ...
    60 permit ip any any (12 match(es))

Core01_ARN# show ip interface vlan 50 | include access list
  Inbound  access list is 150
```
`match(es)` gaat omhoog als er verkeer op die regel valt. Handig als bewijs in je testrapport.

---

## 15. Stap M – Opslaan en Check Results

Op **elk** apparaat:
```
copy running-config startup-config
```
(of kort: `wr`)

Packet Tracer-activiteit: knop **Check Results** onderaan. Tab **Assessment Items** laat per onderdeel zien wat goed/fout is. Gebruik dit als controle, en zoek bij elk rood kruisje uit **waarom**.

> **Je percentage gaat omlaag na een wijziging?** Draai de wijziging terug en lees nog eens in de aansluitingentabel wat er aan de andere kant van die poort zit. (Oefenvoorbeeld: trunks Fa0/1 en Fa0/3 op de core per ongeluk access gemaakt → −10%.)

---

## 16. Testrapport schrijven

Per testactiviteit vul je dit blok in. Hier zit **een heel criterium (3 punten)** in.

| Onderdeel | Wat schrijf je | Fout voorbeeld | Goed voorbeeld |
|---|---|---|---|
| **Toelichting** | De opdracht | – | "Ga via PC3 naar www.solar.com" |
| **Uitvoering** | Stap voor stap wat je doet + **bron en bestemming** | "Ik open de browser" | "Bron: PC3 (VLAN 40). Bestemming: www.solar.com. PC3 > Desktop > Web Browser > URL www.solar.com > Go" |
| **Verwacht resultaat** | Concreet wat er moet gebeuren volgens het ontwerp | "Ik verwacht dat er iets laadt" | "De webpagina van Solar Plex wordt geladen" |
| **Werkelijk resultaat** | Wat er gebeurde + **screenshot** | "Werkt niet" | "Request Timeout, zie screenshot" |
| **Afwijkingen** | Leeg als het werkt. Anders: **deeltesten** tot waar het wél werkt + oorzaak + wat je eraan gedaan hebt | "Ik denk dat het DHCP is" | "ipconfig PC4: 169.254.12.4 → geen DHCP. `show interfaces trunk` op Core: Fa0/1 ontbreekt. Oorzaak: trunk naar SW01 niet geconfigureerd. Opgelost met ... Hertest: PC4 krijgt 10.10.30.51, ping werkt" |

### Tips voor elk type test

| Type test | Uitvoering | Extra bewijs |
|---|---|---|
| Webpagina openen (naam) | Desktop > Web Browser > URL | Werkt het niet: probeer IP in plaats van naam (scheidt DNS van routering) + `nslookup www.solar.com` |
| Webpagina op IP (bijv. intranet 172.16.5.30) | Browser > `http://172.16.5.30` | `ping 172.16.5.30`, `tracert 172.16.5.30` |
| Ping tussen pc's | Eerst `ipconfig` op **beide** pc's, dan `ping <ip>` | Screenshot van beide ipconfigs |
| "Dit zou **niet** mogen" (ACL) | Test **beide** kanten: wat wel mag (solar.com **lukt**) én wat niet mag (intranet/10.10.30.x **faalt**) | `show access-lists` met matches |

> **Ping nooit naar een 169.254.x.x-adres** als test. Een APIPA-adres is vanaf een ander netwerk nooit bereikbaar, dus die ping bewijst niets. Laat in plaats daarvan zien **dat** het een APIPA-adres is en zoek uit waarom.

---

## 17. Troubleshooting-checklist (deeltesten)

Werk van **onder naar boven** (client → switch → core → router). Waar de keten breekt, zit je fout. Dat heet **lokaliseren**.

### Op de client (Command Prompt)
| # | Commando | Werkt? Dan... | Werkt niet? Dan... |
|---|---|---|---|
| 1 | `ipconfig` | Goed IP/gateway/DNS → ga naar 2 | 169.254.x.x → DHCP komt niet aan → **switch-kant checken** |
| 2 | `ipconfig /renew` | Krijgt nu een adres → was timing | Nog steeds APIPA → switch-kant |
| 3 | `ping <eigen gateway>` (bijv. 10.10.30.1) | → 4 | SVI down of VLAN/access-poort fout |
| 4 | `ping <andere VLAN-gateway>` | → 5 | `ip routing` mist |
| 5 | `ping 10.10.0.1` (router) | → 6 | VLAN 1 / kabel naar router / OSPF |
| 6 | `ping 172.16.5.30` of DNS 62.36.55.85 | → 7 | OSPF-routes ontbreken (`show ip route`) of ACL blokkeert |
| 7 | Browser met **IP** | → 8 | Server/dienst of ACL |
| 8 | Browser met **naam** / `nslookup` | Klaar | DNS-server in de pool fout |

### Op de switches
| Probleem | Commando | Waar kijk je naar |
|---|---|---|
| VLAN bestaat niet op client | `show vlan brief`, `show vtp status` | Domeinnaam exact gelijk? Mode client? Revision gelijk? |
| Trunk werkt niet | `show interfaces trunk` | Staat de poort erin? Beide kanten trunk? Op 3560 encapsulation dot1q? |
| Poort in verkeerd VLAN | `show vlan brief`, `show interfaces fa0/4 switchport` | Poort achter juiste VLAN? Operational Mode `static access`? |
| SVI down | `show ip interface brief` | VLAN bestaat + actieve poort/trunk in dat VLAN + `no shutdown` |
| DHCP | `show run \| section dhcp`, `show ip dhcp binding` | Zelfde octet in network/router/exclusions? Bindings aanwezig? |
| Routering | `show ip route`, `show ip ospf neighbor` | `ip routing`? OSPF-buur FULL? O-routes aanwezig? |
| ACL | `show access-lists` | Juiste regels, `permit ip any any` aan het eind, toegepast op juiste interface + richting |
| Root | `show spanning-tree` | "This bridge is the root" op de core |

### Snelle logica
- **Gast-VLAN (50) krijgt wél DHCP, VLAN 30/40 niet** → de DHCP-server werkt. Het probleem zit tussen core en access-switches: **trunk, VTP of access-poorten**.
- **Alles in één VLAN werkt, tussen VLAN's niet** → `ip routing` of SVI.
- **Intern werkt alles, naar 172.16.x.x niet** → OSPF / verbinding naar router.

---

## 18. Veelgemaakte fouten

| Fout | Gevolg | Hoe voorkom je het |
|---|---|---|
| **Kopieerfout tussen DHCP-pools** (pool Vlan40 met 10.10.30.0 en 10.10.30.1) | Check Results: alleen DNS en subnetmask goed; clients krijgen adres uit verkeerd netwerk | Per pool checken: network, default-router en exclusions **zelfde derde octet** |
| **Trunks op access gezet** (Fa0/1 en Fa0/3 op de core) | VTP-info komt niet meer aan, SVI's gaan down, −10% | Poort naar een **switch** = trunk. Alleen poorten naar pc/AP/printer zijn access |
| `switchport mode trunk` op 3560 zonder encapsulation | "Command rejected" | Eerst `switchport trunk encapsulation dot1q` |
| Trunk maar aan één kant | Geen trunk | Beide kanten van de kabel instellen |
| VLAN's maken op een VTP-client | Foutmelding | Alleen op de server maken |
| VTP-domeinnaam met andere hoofdletters | Clients krijgen geen VLAN's | Exact overnemen (`SOLAR`) |
| `ip routing` vergeten | Geen verkeer tussen VLAN's, geen OSPF | Direct als eerste op de core typen |
| `no shutdown` vergeten op SVI | administratively down | Bij elke `interface vlan X` meteen `no shutdown` |
| **WPA-PSK** gekozen i.p.v. **WPA2-PSK** | Config fout (T1 Configureren) | Altijd dubbel checken op AP én client |
| Kabels verwisseld (PC3 op G0/1, AP02 op Fa0/4) | Check Results: "Connects to Port ..." fout | Aansluitingentabel regel voor regel aflopen, poortlabels aanzetten |
| Kabel in verkeerde poort van de AP | Fout bij "Connects to Port 0" | AP heeft "Port 0" voor de kabel |
| Wifi-tabel van een andere locatie configureren | Tijdverlies | Lees welke locatie jij bouwt |
| `permit ip any any` vergeten in de ACL | Gasten kunnen helemaal niks, ook geen internet | Altijd als laatste regel |
| ACL op verkeerde interface/richting | ACL doet niets | `interface vlan 50` + `ip access-group 150 in` |
| Testrapport: "denk ik" | Weinig punten T2 | Deeltesten met screenshots |
| Ping naar APIPA-adres als test | Test bewijst niets | Laat het APIPA-adres zien en zoek de oorzaak |
| Test "mag niet" alleen positief getest | Half getest | Test wat wel mag **en** wat niet mag |
| NIC wisselen met apparaat aan | Lukt niet | Eerst uitzetten |
| Niet opgeslagen | Config weg na reload | `copy running-config startup-config` op elk apparaat |

---

## 19. Snelle naslag (tabellen)

### CLI-modi
| Prompt | Modus | Hoe kom je erin |
|---|---|---|
| `Switch>` | User EXEC | Na inloggen |
| `Switch#` | Privileged EXEC | `enable` |
| `Switch(config)#` | Global config | `configure terminal` (`conf t`) |
| `Switch(config-if)#` | Interface | `interface fa0/1` |
| `Switch(config-vlan)#` | VLAN | `vlan 30` |
| `Switch(dhcp-config)#` | DHCP-pool | `ip dhcp pool X` |
| `Switch(config-router)#` | Routing | `router ospf 1` |

Show-commando vanuit config-modus: zet er `do` voor, bijv. `do show vlan brief`.

### Filters (Ctrl+F in de CLI)
| Filter | Voorbeeld |
|---|---|
| `\| include` | `show run \| include hostname` |
| `\| section` | `show run \| section dhcp` / `section interface Vlan` |
| `\| begin` | `show run \| begin line vty` |

### Subnetmaskers
| CIDR | Masker | Wildcard |
|---|---|---|
| /8 | 255.0.0.0 | 0.255.255.255 |
| /16 | 255.255.0.0 | 0.0.255.255 |
| /24 | 255.255.255.0 | 0.0.0.255 |
| /30 | 255.255.255.252 | 0.0.0.3 |

### Volgorde op de core (spiekbriefje)
```
conf t
 ip routing
 vtp domain <DOMEIN>
 vtp mode server
 vlan <id> / name <naam>                (alle VLAN's)
 spanning-tree vlan <lijst> root primary
 interface vlan <id> / ip address ... / no shutdown   (alle SVI's)
 ip dhcp excluded-address <van> <tot>   (per VLAN)
 ip dhcp pool <naam> / network / default-router / dns-server
 interface range <trunkpoorten>
  switchport trunk encapsulation dot1q
  switchport mode trunk
 interface <poort naar AP/pc> / switchport mode access / switchport access vlan <id>
 router ospf 1 / network ... area 0
 access-list ... / interface vlan <gast> / ip access-group <nr> in
end
copy running-config startup-config
```

### Volgorde op een access-switch (spiekbriefje)
```
conf t
 hostname <NAAM>
 vtp domain <DOMEIN>
 vtp mode client
 interface <poort naar core/andere switch> / switchport mode trunk
 interface <poort naar pc/AP> / switchport mode access / switchport access vlan <id>
end
copy running-config startup-config
show vtp status
show vlan brief
show interfaces trunk
```
