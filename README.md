# B1-K2 Beheert de infrastructuur – Examen cheatsheet

Cheatsheets voor het examen **B1-K2** (MBO 4 IT Systems & Devices), gemaakt op basis van de proeftoetsen in Packet Tracer.

| Bestand | Werkproces | Waar gaat het over |
|---|---|---|
| [W1-installeren-configureren.md](W1-installeren-configureren.md) | **B1-K2-W1** Installeert en configureert | Netwerk bouwen volgens technisch ontwerp: kabels, NIC's, VLAN's, VTP, root bridge, SVI's, DHCP, trunks, OSPF, wifi, ACL + testrapport |
| [W2-beheren-monitoren.md](W2-beheren-monitoren.md) | **B1-K2-W2** Beheert en monitort | Bestaand netwerk in kaart brengen: laptop op beheer-VLAN, SSH, LLDP-inventarisatie, trunk/access, Visio-tekening met snelheden, controletabel, afwijkingen oplossen, advies |
| [CCNA-commandos.md](CCNA-commandos.md) | Naslag | Alle Cisco IOS-commando's uit CCNA 1, 2 en 3 met betekenis |

Alle losse commando's staan in de [CCNA 1-2-3 commandolijst](CCNA-commandos.md). De cheatsheets hierboven vertellen **wanneer** en **waarom** je welk commando gebruikt, met voorbeeld-output en hoe je die leest.

## Beoordeling in één oogopslag

Per werkproces 3 criteria × 0–3 punten = **max 9 punten**.

| Punten | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|
| Cijfer | 4 | 5 | 6 | 7 | 8 | 9 | 10 |

| Werkproces | Criteria |
|---|---|
| W1 | T1 Installeren · T1 Configureren · T2 Testen (fouten lokaliseren met deeltesten) |
| W2 | T1 Gekozen tools · T2 Beheerwerkzaamheden · T3 Verbeteren of adviseren |

## De 10 dingen die je niet mag vergeten

1. **Lees het hele ontwerp/document eerst door** voordat je begint.
2. Poort naar een **switch** = **trunk**. Poort naar **pc/AP/printer** = **access**.
3. Op een **3560** eerst `switchport trunk encapsulation dot1q`, dan `switchport mode trunk`.
4. VLAN's alleen maken op de **VTP-server**. Domeinnaam exact gelijk.
5. **`ip routing`** op de multilayer switch.
6. DHCP-pools: **zelfde derde octet** in network, default-router en exclusions. Niet kopiëren-plakken zonder te checken.
7. Wifi: **WPA2-PSK**, niet WPA-PSK.
8. Testrapport: **verwacht** concreet, **deeltesten** bij elke fout, test ook wat **niet** mag.
9. W2: IOS-versie = **`show version`**. Elke afwijking: **herstelcommando + hertest**.
10. **`copy running-config startup-config`** op elk apparaat.

## Kan ook op het examen komen (niet in de proeftoetsen)

Volgens de examenomschrijving kan ook gevraagd worden:

- Windows Server gebruiksklaar maken: vast IP, hostname, rol **AD DS** installeren, promoveren tot domeincontroller
- Active Directory: OU's, gebruikers, groepen aanmaken, client lid maken van het domein
- Gebruikersprofielen controleren/aanpassen (wachtwoord resetten, account ontgrendelen, groepslidmaatschap)
- Een script aanpassen (PowerShell/batch), bijv. gebruikers aanmaken uit een CSV
- Thuis-/kleinbedrijfnetwerk: wifirouter met DHCP, SSID en WPA2
- Accesspoint vervangen met dezelfde instellingen, routers testen op defecten
- Software updaten op werkstations
- Logfiles lezen (Event Viewer, `show logging`) en een probleem herleiden
