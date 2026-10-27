# B1-K2-W2 – Beheert en monitort netwerk- en infrastructuuronderdelen

> Cheatsheet voor het examen. Gebaseerd op de proeftoets **Solarplex_Cloud** (Packet Tracer).
> In W2 **bouw je niets nieuws**. Je logt in op een bestaand netwerk, brengt het in kaart, controleert of alles volgens de documentatie is ingesteld, lost afwijkingen op en geeft advies.
> Op het examen kunnen namen, IP's en afwijkingen anders zijn. Leer de **werkwijze**: welk commando, waar kijk je naar, wat schrijf je op.
>
> Volledige commandolijst: zie de [CCNA-commandolijst](CCNA-commandos.md) (o.a. hoofdstuk 23 SSH, 35 NTP, 36 Syslog, 38 CDP/LLDP).

---

## Inhoud

1. [Hoe je beoordeeld wordt](#1-hoe-je-beoordeeld-wordt)
2. [Overzicht: de opdrachten in volgorde](#2-overzicht-de-opdrachten-in-volgorde)
3. [Stap 1 – Laptop aansluiten op VLAN 30](#3-stap-1--laptop-aansluiten-op-vlan-30)
4. [Stap 2 – Inloggen via SSH](#4-stap-2--inloggen-via-ssh)
5. [Stap 3 – Inventarisatie met LLDP](#5-stap-3--inventarisatie-met-lldp)
6. [Stap 4 – Trunk of access bepalen](#6-stap-4--trunk-of-access-bepalen)
7. [Stap 5 – Apparaten zonder LLDP vinden (servers, clusters)](#7-stap-5--apparaten-zonder-lldp-vinden-servers-clusters)
8. [Stap 6 – Netwerktekening (Visio) met snelheden](#8-stap-6--netwerktekening-visio-met-snelheden)
9. [Stap 7 – Controletabel: elke controle uitgelegd](#9-stap-7--controletabel-elke-controle-uitgelegd)
10. [Stap 8 – Afwijkingen oplossen](#10-stap-8--afwijkingen-oplossen)
11. [Stap 9 – Advies geven](#11-stap-9--advies-geven)
12. [Troubleshooting-checklist](#12-troubleshooting-checklist)
13. [Veelgemaakte fouten](#13-veelgemaakte-fouten)
14. [Snelle naslag: welke tool voor welke controle](#14-snelle-naslag-welke-tool-voor-welke-controle)

---

## 1. Hoe je beoordeeld wordt

Per criterium 0–3 punten. **Max 9 punten** (9 = 10, 6 = 7, 5 = 6).

| Criterium | Wat de beoordelaar wil zien | Hoe je punten haalt |
|---|---|---|
| **T1 Gekozen tools** | De juiste tools zijn correct gebruikt | Per controle het **beste** commando. Bijv. IOS-versie = `show version` (niet `show running-config`). NTP = `show ntp associations` naast de config |
| **T2 Beheerwerkzaamheden** | Conform opdracht uitgevoerd | Inventarisatietabel compleet en correct, **eigen** Visio-tekening met lagen en snelheden, controletabel helemaal ingevuld met bewijs (screenshots) |
| **T3 Verbeteren of adviseren** | Netwerk voldoet aan onderhouds- en veiligheidseisen, **of** er is advies gegeven | Elke afwijking: **wat** is fout, **herstelcommando**, **hertest**. Plus extra adviezen die je zelf vindt |

> Alleen "afwijking gevonden" opschrijven is maar half werk. Zet er altijd bij **hoe je het oplost** (het commando) en dat je het daarna opnieuw gecontroleerd hebt.

---

## 2. Overzicht: de opdrachten in volgorde

| Stap | Opdracht | Resultaat |
|---|---|---|
| 1 | Laptop aansluiten op beheer-VLAN | Laptop heeft IP in beheernetwerk, ping naar switch werkt |
| 2 | SSH naar elk apparaat | Ingelogd met account uit de documentatie |
| 3 | `show lldp neighbors` per apparaat | Inventarisatietabel (device, IP, interface, buur, buur-interface, trunk/access) |
| 4 | Trunk/access bepalen | Laatste kolom van de tabel |
| 5 | Servers/verborgen apparaten vinden | Compleet beeld voor de tekening |
| 6 | Visio-tekening | Lagen (Core / Distribution), hostnames, poorten, **snelheid per verbinding** |
| 7 | Controletabel | Per controle-item: apparaten, tool, uitvoering, resultaat, afwijking/advies |
| 8 | Afwijkingen oplossen | Herstelcommando + hertest |
| 9 | Advies | Extra verbeteringen voor veiligheid en onderhoud |

**Werkwijze:** start bij één switch en volg de buren. LLDP vertelt je steeds wie de volgende is. Elk nieuw apparaat dat je ontdekt: inloggen en weer `show lldp neighbors`.

---

## 3. Stap 1 – Laptop aansluiten op VLAN 30

**Opdracht:** "Sluit een laptop aan op de juiste poort (VLAN 30)".

### Welke poort?
Kijk in de netwerkdocumentatie bij **Verdeling VLAN's**. In de proeftoets:

> "Poort 24 is op alle access-switches vrij gelaten voor de netwerkbeheerder" → **Fa0/24 = VLAN 30 (IT_Beheer)** op SW1, SW2 en SW3.

### Aansluiten
1. Zet een **Laptop** neer (End Devices).
2. Kabel: **Copper Straight-Through** (laptop ↔ switch).
3. Laptop **FastEthernet0** → switch **Fa0/24**.
4. Wacht tot het lampje groen is.

### IP-adres
Laptop > **Desktop > IP Configuration**:
1. Probeer eerst **DHCP** (er staat een DHCP-server in het beheernetwerk).
2. Krijg je **169.254.x.x**? Geef dan zelf een **vrij** adres in het beheernetwerk:

| Veld | Voorbeeld proeftoets |
|---|---|
| IP Address | een vrij adres in 10.30.0.0/16, bijv. 10.30.0.20 (**niet** een adres uit de documentatie) |
| Subnet Mask | 255.255.0.0 (want **/16**) |
| Default Gateway | 10.30.0.1 (de L3-switch) |

### Controleren
```
C:\>ping 10.30.0.4

Pinging 10.30.0.4 with 32 bytes of data:
Reply from 10.30.0.4: bytes=32 time<1ms TTL=255
```
Eerste ping kan 1x "Request timed out" geven (ARP). Daarna moet het lukken.

---

## 4. Stap 2 – Inloggen via SSH

Laptop > **Desktop > Command Prompt** (dit is **geen** Cisco IOS, maar de pc-prompt).

```
C:\>ssh -l admin 10.30.0.4

Password:            ← wachtwoord uit de documentatie (je ziet niets tijdens typen)

SW1>enable
Password:            ← enable-wachtwoord uit de documentatie
SW1#
```

| Onderdeel | Betekenis |
|---|---|
| `ssh` | Beveiligde verbinding |
| `-l admin` | **l**ogin-naam = admin |
| `10.30.0.4` | IP van het apparaat (uit de IP-tabel van de documentatie) |
| `SW1>` → `SW1#` | Van user naar privileged mode |

Terug naar de laptop: `exit` (of `logout`). Daarna het volgende IP.

### Wat als het niet lukt?
| Melding | Betekenis | Wat doe je |
|---|---|---|
| `% Connection timed out; remote host not responding` | Apparaat niet bereikbaar of geen SSH | Eerst `ping`. Ping werkt maar SSH niet → SSH niet ingesteld = **bevinding** |
| `% Login invalid` | Fout wachtwoord/gebruiker | Hoofdletters checken (`WELkom@1`) |
| `% No password set` na `enable` | Er is **geen enable-wachtwoord** ingesteld | Dit is een **afwijking**: noteren + oplossen (zie stap 8) |

Noteer alles wat niet werkt. In W2 is het juist de bedoeling dat je afwijkingen vindt.

---

## 5. Stap 3 – Inventarisatie met LLDP

**LLDP** = protocol waarmee buurapparaten zich aan elkaar bekendmaken (zoals CDP, maar open standaard).

### Commando's per apparaat
```
SW1# show lldp neighbors
SW1# show lldp neighbors detail     ← meer info: IP-adres, IOS-versie van de buur
SW1# show interfaces trunk
SW1# show ip interface brief
```
Geen output van LLDP? Misschien staat LLDP uit: `conf t` → `lldp run`. Noteer dat als bevinding.

### Voorbeeld-output uitlezen
```
SW1#show lldp neighbors
Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
SW_Core             Gig0/1         120        B               Gig2/1

Total entries displayed: 1
```

Lees elke regel als een zin:
> *"Mijn poort **Gig0/1** zit vast aan **SW_Core**, op poort **Gig2/1** van SW_Core."*

| Kolom | Betekenis | Nodig voor tabel? |
|---|---|---|
| **Device ID** | Hostname van de buur | Ja → "Verbonden met: Device" |
| **Local Intf** | **Jouw** poort | Ja → "Interface" |
| Hold-time | Hoe lang de info geldig blijft | Nee |
| **Capability** | B = Bridge (switch), R = Router | Handig voor tekening (welk symbool) |
| **Port ID** | Poort **van de buur** | Ja → "Verbonden met: Interface" |

### Van output naar tabel
Elke LLDP-regel = **één rij**.

| Kolom in de tabel | Waar haal je het vandaan |
|---|---|
| Device | De prompt (`SW1#`) |
| IP-adres | Het IP waarmee je inlogde, of `show ip interface brief` (regel met IP, bijv. Vlan30) |
| Interface | LLDP **Local Intf** |
| Verbonden met – Device | LLDP **Device ID** |
| Verbonden met – Interface | LLDP **Port ID** |
| Trunk/Access | `show interfaces trunk` (zie stap 4) |

Voorbeeldrij (die je zelf in de oefening vond):

| Device | IP-adres | Interface | Verbonden met device | Interface | Trunk/Access |
|---|---|---|---|---|---|
| SW1 | 10.30.0.4 | Gig0/1 | SW_Core | Gig2/1 | Trunk |

### Zelfcontrole
Dezelfde kabel zie je **twee keer**, vanaf elke kant. Op SW_Core moet je dus een regel zien: `SW1   Gig2/1   ...   Gig0/1`. Klopt dat? Dan is je rij goed.

### Volgorde van de speurtocht
1. Access-switch (bijv. SW1) → ontdekt de core-switch
2. Core-switch → ontdekt alle access-switches + L3-switch
3. L3-switch → ontdekt de edge-router
4. Edge-router → ontdekt de volgende switch
5. Enz. tot een apparaat **geen nieuwe buren** meer heeft

Kun je een apparaat niet via SSH bereiken (ander subnet, bijv. 172.16.x.x)? Log dan in op het apparaat ervóór en gebruik vanaf daar `ssh -l admin <ip>`, of gebruik `show lldp neighbors detail` op de buur om de gegevens te zien.

---

## 6. Stap 4 – Trunk of access bepalen

### Methode 1: lijst van trunks
```
SW1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Gig0/1      20-23,30

Port        Vlans allowed and active in management domain
Gig0/1      20,21,22,23,30
```
**Staat de poort in deze lijst → trunk. Staat hij er niet in → access.**

### Methode 2: één poort
```
SW1#show interfaces fa0/4 switchport
Name: Fa0/4
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Access Mode VLAN: 20 (Productie)
```
Kijk naar **Operational Mode**: `trunk` of `static access`.

### Routerpoorten
Een router (of een routed poort op een L3-switch, `no switchport`) doet geen trunk/access zoals een switch. Komt hij niet in `show interfaces trunk` voor, dan noteer je hem als **access**. Heeft een routerpoort subinterfaces (`G0/0.10`, `encapsulation dot1Q`), dan is het verkeer getagd → trunk.

### Extra uit dezelfde output
`Vlans allowed on trunk` gebruik je meteen voor de controle "Trunking staat alleen aan voor VLAN ..." in de controletabel.

---

## 7. Stap 5 – Apparaten zonder LLDP vinden (servers, clusters)

Servers en pc's sturen **geen** LLDP. Je ziet ze dus niet in `show lldp neighbors`.

### Cluster (wolkje) openen in Packet Tracer
- **Dubbelklik** op het wolkje → je ziet de apparaten die erin zitten.
- Terug: knop **Back** (linksboven) of het pijltje naar het bovenliggende niveau.

### Poortnamen zien
1. **Muis stil op een kabel** → geel kadertje met poorten aan beide kanten.
2. **Options > Preferences > "Always Show Port Labels in Logical Workspace"** → alle poortnamen permanent zichtbaar. Handigst voor de tekening.

### Via de CLI: welke poorten zijn in gebruik?
```
Main_L3#show ip interface brief
Interface              IP-Address      OK? Method Status    Protocol
GigabitEthernet1/0/1   unassigned      YES unset  up        up
GigabitEthernet1/0/2   unassigned      YES unset  up        up
GigabitEthernet1/0/3   unassigned      YES unset  up        up
GigabitEthernet1/0/4   unassigned      YES unset  up        up
...
Vlan30                 10.30.0.1       YES manual up        up
```
Poorten die **up/up** zijn maar **niet** in LLDP staan = er hangt een eindapparaat aan (server, pc, printer).

Alternatief: `show interfaces status` (overzicht met VLAN en snelheid per poort), `show mac address-table` (welk MAC-adres zit op welke poort).

### Welke server zit op welke poort? (uitsluitingsmethode)
1. Ping vanaf je laptop alle server-IP's uit de documentatie. Noteer welke antwoorden.
2. Zet op het apparaat **één** verdachte poort uit:
   ```
   conf t
   interface g1/0/3
   shutdown
   ```
3. Ping de servers opnieuw. Welke ping valt weg? Die server zit op die poort.
4. **Zet de poort weer aan!**
   ```
   no shutdown
   end
   ```
5. Herhaal per poort.

Zo vind je ook apparaten achter een router: op de router `show ip interface brief` → zie welke interfaces up zijn en welk IP-netwerk erop zit, en vergelijk met de server-IP's.

### Pc's/eindapparaten aan een access-switch
Up/up-poorten zonder LLDP-buur op een access-switch (bijv. Fa0/4, Fa0/14) → muis op de kabel in PT om te zien wat eraan hangt. VLAN haal je uit de documentatie of `show vlan brief`.

---

## 8. Stap 6 – Netwerktekening (Visio) met snelheden

**Opdracht (proeftoets):** tekening van de distribution-infrastructuur. Benoem servers in de **Core-laag**, routers en switches in de **Distribution-laag**, en zet bij **elke** verbinding de **transmissiesnelheid**.

> Volg de lagen zoals de **opdracht** ze noemt, ook al zou je ze zelf anders indelen.

### Snelheid aflezen aan de poortnaam
| Poortnaam | Afkorting | Snelheid |
|---|---|---|
| Ethernet | Eth / E | 10 Mbit/s |
| FastEthernet | **Fa** / F | **100 Mbit/s** |
| GigabitEthernet | **Gi** / Gig / G | **1 Gbit/s** (1000 Mbit/s) |
| TenGigabitEthernet | Te | 10 Gbit/s |

**Verbinding Fa ↔ Gi?** Dan loopt de verbinding op de **laagste** snelheid: **100 Mbit/s**.

Zeker weten:
```
SW1#show interfaces gig0/1
GigabitEthernet0/1 is up, line protocol is up (connected)
  ...
  MTU 1500 bytes, BW 1000000 Kbit, DLY 10 usec,
  Full-duplex, 1000Mb/s
```
`BW 1000000 Kbit` = 1 Gbit/s. `BW 100000 Kbit` = 100 Mbit/s.

### Stappenplan Visio
1. Nieuw bestand: sjabloon **Network > Basic Network Diagram** (of Detailed Network Diagram).
2. Teken **horizontale lagen** met een rechthoek of stippellijn en zet er een label bij: **Core**, **Distribution** (en eventueel Access).
3. Sleep de juiste symbolen:

| Apparaat | Symbool |
|---|---|
| Router (Capability **R**) | Router |
| Switch (Capability **B**) | Switch |
| Multilayer/L3-switch | Layer 3 switch / Multilayer switch |
| Firewall | Firewall (of router met label FIREWALL) |
| Server | Server |

4. Bij **elk** apparaat: **hostname** (en IP).
5. Trek de verbindingen volgens je **inventarisatietabel** (+ de servers uit stap 5).
6. Bij **elk kabeluiteinde**: de **poortnaam** (Gig0/1 enz.).
7. In het **midden** van elke lijn: de **snelheid** (100 Mbit/s of 1 Gbit/s).
8. Optioneel: legenda, titel, datum, je naam.
9. Exporteer als **PNG** (Bestand > Exporteren) en plak het in je uitwerking.

### Checklist tekening
- [ ] Alle apparaten uit de inventarisatietabel staan erop
- [ ] Servers zichtbaar en in de juiste laag
- [ ] Lagen benoemd
- [ ] Hostname bij elk apparaat
- [ ] Poort bij elk kabeluiteinde
- [ ] **Snelheid bij elke verbinding**
- [ ] Leesbaar (niet te klein, niet te vol)

> Teken wat je **echt** ziet in Packet Tracer. Wijkt het af van het voorbeeld of de documentatie? Teken de werkelijkheid en noem het verschil als bevinding.

---

## 9. Stap 7 – Controletabel: elke controle uitgelegd

De tabel heeft deze kolommen. Vul **alle** kolommen in, voor **alle** genoemde apparaten.

| Controle item | Apparaten | Welke tools gebruikt? | Hoe heb je dit uitgevoerd? | Resultaat | Afwijking / advies |
|---|---|---|---|---|---|

- **Tools** = waarmee: "CLI via SSH vanaf beheerlaptop", "webbrowser op werkplek-pc", "Command Prompt".
- **Hoe uitgevoerd** = het exacte commando.
- **Resultaat** = de relevante regels uit de output + screenshot.
- **Afwijking/advies** = per apparaat wat afwijkt + herstelcommando. Niets gevonden? Schrijf "Geen afwijkingen".

Hieronder per controle: het beste commando, hoe "goed" eruitziet, en hoe "fout" eruitziet.

---

### 9.1 Trunking staat alleen aan voor bepaalde VLAN's

| | |
|---|---|
| **Commando** | `show interfaces trunk` |
| **Kijk naar** | Blok **Vlans allowed on trunk** |
| **Goed** | Precies de VLAN's uit de opdracht (bijv. `20-23,30`) |
| **Fout** | `1-1005` (alles toegestaan) of een VLAN te veel/te weinig |
| **Herstel** | `interface <poort>` → `switchport trunk allowed vlan 20-23,30` |

```
Port        Vlans allowed on trunk
Gig0/1      20-23,30
```

---

### 9.2 Telnet is uitgeschakeld

| | |
|---|---|
| **Commando** | `show running-config \| section line vty` |
| **Kijk naar** | `transport input` op **alle** vty-lijnen (0 4 **en** 5 15) |
| **Goed** | `transport input ssh` |
| **Fout** | `transport input telnet`, `transport input ssh telnet`, `transport input all`, of **geen** transport-regel (dan staat vaak alles open) |
| **Extra test** | Vanaf laptop `telnet <ip>` → moet **mislukken** (`% Connection refused by remote host`) |
| **Herstel** | `line vty 0 15` → `transport input ssh` |

```
SW1#show running-config | section line vty
line vty 0 4
 login local
 transport input ssh
line vty 5 15
 login local
 transport input ssh
```

---

### 9.3 Wachtwoorden zijn versleuteld opgeslagen

| | |
|---|---|
| **Commando** | `show running-config \| include service password` |
| **Goed** | `service password-encryption` staat er |
| **Fout** | `no service password-encryption` of de regel ontbreekt; wachtwoorden leesbaar in `show run` |
| **Extra check** | `show running-config \| include password\|secret` → zie je **leesbare** wachtwoorden? Dan fout. `secret 5 $1$...` en `password 7 0822...` zijn versleuteld |
| **Herstel** | `service password-encryption` |

---

### 9.4 Alleen via SSH remote inloggen

| | |
|---|---|
| **Commando's** | `show running-config \| section line vty` + `show ip ssh` |
| **Goed** | Op alle vty-lijnen `transport input ssh` **en** `login local`; `show ip ssh` → `SSH Enabled - version 2.0` |
| **Fout** | Telnet toegestaan, of `login` zonder `local` (dan wordt het lokale admin-account niet gebruikt), of SSH disabled |
| **Herstel** | Zie hieronder |

```
SW1#show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3
```

SSH opnieuw instellen (als het ontbreekt):
```
conf t
 hostname SW1
 ip domain-name solarplex.local
 crypto key generate rsa        ← 1024 of hoger
 ip ssh version 2
 username admin secret <wachtwoord>
 line vty 0 15
  login local
  transport input ssh
end
```

---

### 9.5 Werkplek-pc's kunnen op de intranetpagina

| | |
|---|---|
| **Tool** | Webbrowser op een werkplek-pc (niet de beheerlaptop) |
| **Uitvoering** | Werkplek-pc > Desktop > Web Browser > `http://intranet.local` |
| **Goed** | Intranetpagina laadt |
| **Fout? Deeltesten** | `ipconfig` (IP/DNS ok?) → `nslookup intranet.local` (DNS ok?) → `ping <ip webserver>` → browser met IP i.p.v. naam |
| **Bewijs** | Screenshot van de pagina, met bron-pc erbij vermeld |

---

### 9.6 Privileged EXEC (enable) is beveiligd

| | |
|---|---|
| **Commando's** | `enable` proberen via SSH + `show running-config \| include enable` |
| **Goed** | `enable` vraagt wachtwoord; config bevat `enable secret 5 $1$...` |
| **Fout** | Via SSH: `% No password set` → er is geen enable-wachtwoord. Of alleen `enable password` (zwak) i.p.v. `enable secret` |
| **Herstel** | `enable secret <wachtwoord uit documentatie>` (en evt. `no enable password`) |

> Lukt `enable` via SSH niet omdat er geen wachtwoord is, log dan in via de **console** (Packet Tracer: klik apparaat > CLI) om het te herstellen.

---

### 9.7 NTP-server is geconfigureerd

| | |
|---|---|
| **Commando's** | `show running-config \| include ntp` **én** `show ntp associations` / `show ntp status` |
| **Goed** | `ntp server <IP NTP-server uit documentatie>`; in associations een `*` voor dat adres (gesynchroniseerd) |
| **Fout** | Regel ontbreekt, of een **ander IP** dan in de documentatie |
| **Herstel** | `no ntp server <fout ip>` → `ntp server <juist ip>` |

```
SW_Core#show ntp associations
address         ref clock       st   when     poll    reach  delay   offset   disp
*~10.30.0.100   127.127.1.1     1    14       16      377    0.00    0.00     0.12
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
```
`*` = deze server wordt gebruikt. Alleen `~` zonder `*` = ingesteld maar (nog) niet gesynchroniseerd → server niet bereikbaar of fout IP.

`show clock` → staat de tijd op 1993? Dan is NTP niet gesynchroniseerd.

> `show ntp associations` is een **sterkere** controle dan alleen de config lezen: het bewijst dat NTP ook echt werkt. Dat telt bij T1 (tools).

---

### 9.8 Logging naar syslog-server is geconfigureerd

| | |
|---|---|
| **Commando's** | `show running-config \| include logging` + `show logging` |
| **Goed** | `logging <IP syslog-server uit documentatie>` (of `logging host <ip>`); in `show logging` staat bij **Trap logging** de host |
| **Fout** | Regel ontbreekt of verkeerd IP |
| **Herstel** | `no logging <fout ip>` → `logging host <juist ip>` |

```
Router#show logging
Syslog logging: enabled
    Trap logging: level informational, 21 message lines logged
        Logging to 10.30.0.100  (udp port 514, audit disabled, ...)
```
Extra bewijs: open de **syslog-server** in Packet Tracer (Services > SYSLOG) en kijk of er berichten van dat apparaat binnenkomen.

---

### 9.9 IOS-versie controleren

| | |
|---|---|
| **Commando** | **`show version`** (niet `show running-config`: daar staat de IOS-versie niet goed in) |
| **Kijk naar** | Eerste regels: `Version ...` |
| **Vergelijk met** | Tabel **IOS-versies** in de documentatie, per apparaat |
| **Afwijking** | Versie anders dan documentatie |
| **Herstel/advies** | IOS upgraden/downgraden naar de goedgekeurde versie (`copy tftp: flash:` + `boot system flash:<bestand>`), **of** documentatie bijwerken als de nieuwe versie bewust is gekozen |

```
SW1#show version
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(25)FX, RELEASE SOFTWARE (fc1)
...
ROM: Bootstrap program is C2960 boot loader
BOOTLDR: C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)
```
Let op: er kunnen **twee** versies in staan, de IOS-versie (bovenaan) en de bootloader (`BOOTLDR`/ROM). Schrijf op welke je vergelijkt en vergelijk exact (letters en haakjes tellen mee).

Tip: zet in je resultaat een klein tabelletje **Apparaat | Verwacht | Gevonden | Klopt?**.

---

## 10. Stap 8 – Afwijkingen oplossen

Werkwijze per afwijking:

1. **Log in** (SSH, of console als SSH/enable niet werkt).
2. **Los op** met het herstelcommando.
3. **Sla op**: `copy running-config startup-config`.
4. **Hertest** met hetzelfde show-commando.
5. **Noteer**: wat was fout, welk commando, resultaat hertest (+ screenshot).

### Herstelcommando's op een rij
| Afwijking | Herstel |
|---|---|
| Geen enable-wachtwoord | `enable secret <wachtwoord>` |
| Verkeerde NTP-server | `no ntp server <fout>` + `ntp server <goed>` |
| Geen/verkeerde syslog | `no logging <fout>` + `logging host <goed>` |
| Telnet aan | `line vty 0 15` + `transport input ssh` |
| `login` i.p.v. `login local` | `line vty 0 15` + `login local` |
| Wachtwoorden leesbaar | `service password-encryption` |
| Trunk laat te veel VLAN's door | `switchport trunk allowed vlan <lijst>` |
| LLDP uit | `lldp run` |
| Ongebruikte poorten actief | `interface range <poorten>` + `shutdown` |
| IOS-versie wijkt af | Advies: upgraden of documentatie bijwerken |

### Zo schrijf je het op (voorbeeldformaat)
> **Afwijking:** op <apparaat> staat `ntp server <fout IP>`, documentatie zegt <juist IP>.
> **Oplossing:** `no ntp server <fout IP>`, `ntp server <juist IP>`, `copy run start`.
> **Hertest:** `show ntp associations` toont nu `*~<juist IP>` (screenshot).

---

## 11. Stap 9 – Advies geven

Voor **T3 volledig** geef je ook advies dat verder gaat dan de controletabel. Kijk tijdens het controleren bewust naar dit soort dingen:

| Waar kijken | Mogelijke bevinding | Advies |
|---|---|---|
| `show run \| section line vty` | Sommige vty-lijnen hebben `login` i.p.v. `login local` | Overal `login local`, zodat het admin-account gebruikt wordt |
| `show ip interface brief` / `show vlan brief` | Ongebruikte poorten staan aan | `shutdown` op ongebruikte poorten, in ongebruikt VLAN zetten |
| Netwerkdocumentatie vs. werkelijkheid | VLAN-tabel klopt niet (bijv. ongebruikte poorten bij één switch anders benoemd), server in ander subnet dan verwacht | Documentatie bijwerken |
| `show version` | Verouderde of afwijkende IOS | Standaardiseren op één goedgekeurde versie |
| Config opgeslagen? | `show startup-config` ≠ running | Altijd `copy run start` na wijzigingen |
| Geen tekening aanwezig | Documentatie onvolledig | Netwerktekening (jouw Visio) toevoegen aan documentatie |
| `enable password` i.p.v. `secret` | Zwakke versleuteling | `enable secret` gebruiken |
| SSH versie | `version 1.99` of 1 | `ip ssh version 2` |
| Banner ontbreekt | Geen waarschuwing bij inloggen | `banner motd #Alleen geautoriseerde toegang#` |

Schrijf advies concreet: **wat**, **waarom** (veiligheid/beheer), **welk commando**.

---

## 12. Troubleshooting-checklist

| Probleem | Controleer | Commando |
|---|---|---|
| Laptop krijgt geen IP | Juiste poort (VLAN 30)? Kabel straight-through? | Laptop: `ipconfig`; switch: `show vlan brief`, `show interfaces fa0/24 switchport` |
| 169.254.x.x | DHCP niet bereikbaar | Zelf vast IP geven in beheersubnet, juiste masker (/16 = 255.255.0.0) |
| Ping naar switch faalt | IP/masker/gateway laptop, poort up? | `ipconfig`, poortlampje, `show ip interface brief` |
| SSH timed out | SSH ingesteld? vty transport? | `ping` eerst; via console `show ip ssh`, `show run \| section vty` |
| Login invalid | Gebruikersnaam/wachtwoord | Hoofdlettergevoelig, check documentatie |
| `% No password set` | Geen enable-wachtwoord | Afwijking noteren, via console oplossen |
| LLDP leeg | LLDP uit of geen LLDP-apparaat | `show lldp`; `lldp run` |
| Apparaat in ander subnet onbereikbaar | Routering | Vanaf tussenliggend apparaat `ssh -l admin <ip>` |
| Poort onbekend in cluster | Cluster niet geopend | Dubbelklik wolk, poortlabels aanzetten |

---

## 13. Veelgemaakte fouten

| Fout | Gevolg | Beter |
|---|---|---|
| IOS-versie gecontroleerd met `show running-config` | Punten kwijt op T1 (tools) | **`show version`** |
| NTP alleen in de config gelezen | Zwakkere controle | Ook `show ntp associations` |
| Afwijking genoemd zonder oplossing | Punten kwijt op T3 | Altijd herstelcommando + hertest |
| Geen **eigen** Visio-tekening, alleen het originele wolkschema | T2 niet volledig | Zelf tekenen met lagen en snelheden |
| Snelheden vergeten in de tekening | T2 niet volledig | Fa = 100 Mbit/s, Gi = 1 Gbit/s bij elke lijn |
| Servers vergeten (geen LLDP) | Tekening incompleet | Up-poorten zonder LLDP-buur onderzoeken |
| Poort uitgezet om servers te vinden en niet weer aangezet | Netwerk kapot | Altijd `no shutdown` erna |
| Eén apparaat gecontroleerd terwijl er acht gevraagd zijn | Onvolledig | Kolom "Apparaten" volledig aflopen, per apparaat resultaat |
| Laptop op een willekeurige poort | Geen toegang | Poort uit documentatie (beheer-VLAN) |
| Verkeerd subnetmasker (/24 i.p.v. /16) | Kan niet alles bereiken | /16 = 255.255.0.0 |
| Telnet-controle alleen op vty 0 4 | vty 5 15 gemist | `line vty` **alle** blokken bekijken |
| Geen screenshots | Geen bewijs | Bij elke rij relevante output |
| Wijziging niet opgeslagen | Fix weg na reload | `copy running-config startup-config` |

---

## 14. Snelle naslag: welke tool voor welke controle

| Doel | Beste commando | Aanvullend |
|---|---|---|
| Buren vinden | `show lldp neighbors` | `show lldp neighbors detail`, `show cdp neighbors` |
| Poorten en IP's | `show ip interface brief` | `show interfaces status` |
| Snelheid poort | `show interfaces <poort>` (BW-regel) | Poortnaam (Fa/Gi) |
| Trunk/access | `show interfaces trunk` | `show interfaces <poort> switchport` |
| Toegestane VLAN's op trunk | `show interfaces trunk` | – |
| VLAN per poort | `show vlan brief` | – |
| Telnet/SSH op vty | `show running-config \| section line vty` | `telnet <ip>` vanaf laptop moet falen |
| SSH aan | `show ip ssh` | – |
| Wachtwoordversleuteling | `show running-config \| include service password` | `show run \| include password\|secret` |
| Enable beveiligd | `enable` proberen | `show run \| include enable` |
| NTP | `show ntp associations` | `show run \| include ntp`, `show ntp status`, `show clock` |
| Syslog | `show logging` | `show run \| include logging`, syslog-server in PT |
| IOS-versie | **`show version`** | – |
| MAC op poort | `show mac address-table` | – |
| Intranet werkplek | Browser op werkplek-pc | `nslookup`, `ping` |
| SSH vanaf laptop | `ssh -l admin <ip>` | – |
| Opslaan | `copy running-config startup-config` | `show startup-config` |
