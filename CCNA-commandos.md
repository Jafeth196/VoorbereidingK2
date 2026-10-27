# CCNA 1, 2 en 3 – Cisco IOS Commands

> Overzicht voor Cisco Networking Academy (CCNAv7 / Packet Tracer). De exacte ondersteuning verschilt per IOS-platform en cursusversie. Voorbeelden gebruiken demonstratieadressen en wachtwoorden; gebruik in het echt sterke, unieke wachtwoorden. `<...>` duidt een zelf in te vullen waarde aan.


---

## CCNA 1 – Introduction to Networks


### 1. IOS-navigatie

| Command | Betekenis |
|---|---|
| `enable` | Privileged EXEC |
| `disable` | Terug naar user EXEC |
| `configure terminal` | Global configuration |
| `exit` | Niveau terug |
| `end` | Terug naar privileged EXEC |
| `logout` | Uitloggen |
| `?` | Contextgevoelige hulp |
| `show history` | Commandogeschiedenis |
| `terminal history size 50` | Geschiedenis instellen |
| `no <command>` | Configuratie terugdraaien |
| `do <exec-command>` | EXEC-commando vanuit configuratiemodus |

### 2. Basisconfiguratie

| Command | Betekenis |
|---|---|
| `hostname R1` | Hostnaam |
| `no ip domain-lookup` | DNS-lookup bij typefouten uit |
| `enable secret Cisco123` | Enable-secret (voorbeeld) |
| `enable password Cisco123` | Verouderd enable-password |
| `service password-encryption` | Zwakke type-7-obfuscatie van ondersteunde plaintext wachtwoorden |
| `banner motd #Verboden toegang#` | Waarschuwingsbanner |
| `security passwords min-length 8` | Minimale lengte |
| `username admin secret Cisco123` | Lokale gebruiker |
| `ip domain-name school.local` | Domein instellen |

### 3. Console en VTY

| Command | Betekenis |
|---|---|
| `line console 0` | Consolelijn |
| `password Cisco123` | Lijnwachtwoord |
| `login` | Lijnwachtwoord vragen |
| `logging synchronous` | Logs niet door CLI-invoer heen |
| `exec-timeout 5 0` | 5 minuten timeout |
| `line vty 0 4` | VTY-lijnen 0–4 |
| `line vty 0 15` | VTY-lijnen 0–15 (indien beschikbaar) |
| `login local` | Lokale accounts gebruiken |
| `transport input ssh` | Alleen SSH |
| `transport input telnet` | Alleen Telnet (onveilig) |
| `transport input ssh telnet` | SSH en Telnet |

### 4. IPv4 en interfaces

| Command | Betekenis |
|---|---|
| `interface gigabitEthernet 0/0` | Interface selecteren |
| `interface g0/0` | Verkorte interfacekeuze |
| `interface range f0/1-12` | Poortbereik selecteren |
| `description Verbinding naar LAN` | Interfacebeschrijving |
| `ip address 192.168.1.1 255.255.255.0` | Statisch IPv4 |
| `ip address dhcp` | DHCP-client |
| `no shutdown` | Interface activeren |
| `shutdown` | Interface uitschakelen |
| `interface loopback 0` | Loopback aanmaken |

### 5. IPv6

| Command | Betekenis |
|---|---|
| `ipv6 unicast-routing` | IPv6 routeren |
| `ipv6 address 2001:DB8:ACAD:1::1/64` | Statisch IPv6 |
| `ipv6 address FE80::1 link-local` | Link-local instellen |
| `ipv6 enable` | IPv6 inschakelen op interface |
| `ipv6 address autoconfig` | SLAAC |
| `ipv6 address dhcp` | Stateful DHCPv6-client |

### 6. Switchmanagement

| Command | Betekenis |
|---|---|
| `interface vlan 1` | Management-SVI |
| `ip address 192.168.1.2 255.255.255.0` | IP op SVI |
| `ip default-gateway 192.168.1.1` | Gateway voor L2-switch |
| `show interfaces vlan 1` | SVI-status |

### 7. Configuratie, back-up en inspectie

| Command | Betekenis |
|---|---|
| `show running-config` | Actieve configuratie |
| `show startup-config` | Opgeslagen configuratie |
| `copy running-config startup-config` | Opslaan |
| `copy startup-config running-config` | Opgeslagen config samenvoegen |
| `erase startup-config` | Startup-config wissen |
| `reload` | Herstarten |
| `show version` | IOS/hardware |
| `show flash:` | Flash-inhoud (platformafhankelijk) |
| `dir flash:` | Flash-inhoud |
| `show boot` | Bootgegevens |

### 8. Show- en testcommando's

| Command | Betekenis |
|---|---|
| `show ip interface brief` | IPv4-interfaceoverzicht |
| `show ipv6 interface brief` | IPv6-interfaceoverzicht |
| `show interfaces` | Interfacecounters en details |
| `show interfaces g0/0` | Één interface |
| `show ip interface g0/0` | IPv4-instellingen |
| `show ipv6 interface g0/0` | IPv6-instellingen |
| `show ip route` | IPv4-routes |
| `show ipv6 route` | IPv6-routes |
| `show arp` | ARP-cache |
| `show mac address-table` | MAC-tabel |
| `show cdp neighbors` | Cisco-buren |
| `show cdp neighbors detail` | Gedetailleerde Cisco-buren |
| `show clock` | Tijd |
| `show users` | Sessies |
| `show protocols` | Protocolstatus |
| `ping 192.168.1.1` | IPv4-test |
| `ping 2001:DB8::1` | IPv6-test |
| `traceroute 8.8.8.8` | Pad controleren |
| `show running-config \| section interface` | Interfaceblokken filteren |
| `show running-config \| include hostname` | Hostnameregel filteren |
| `show running-config \| begin line` | Uitvoer vanaf match |

### 9. Packet Tracer PC Command Prompt (GEEN Cisco IOS)

| Command | Betekenis |
|---|---|
| `ipconfig` | IP-configuratie |
| `ipconfig /all` | Uitgebreide instellingen |
| `ipconfig /release` | DHCP-lease vrijgeven |
| `ipconfig /renew` | DHCP vernieuwen |
| `ping 192.168.1.1` | Test |
| `tracert 8.8.8.8` | Route volgen |
| `arp -a` | ARP tonen |
| `nslookup example.com` | DNS opvragen |
| `netstat` | Verbindingen |
| `ssh -l admin 192.168.1.1` | SSH naar apparaat |
| `telnet 192.168.1.1` | Telnet naar apparaat |

---

## CCNA 2 – Switching, Routing and Wireless Essentials


### 10. Switchpoorten

| Command | Betekenis |
|---|---|
| `interface f0/1` | Poort selecteren |
| `interface range f0/1-10` | Poortbereik |
| `switchport mode access` | Accesspoort |
| `switchport mode trunk` | Trunkpoort |
| `switchport access vlan 10` | VLAN-toewijzing |
| `speed 100` | Snelheid |
| `speed auto` | Auto-negotiation |
| `duplex full` | Full duplex |
| `duplex half` | Half duplex |
| `duplex auto` | Duplex auto |
| `mdix auto` | Auto-MDIX |
| `shutdown` | Uitschakelen |
| `no shutdown` | Inschakelen |

### 11. VLANs

| Command | Betekenis |
|---|---|
| `vlan 10` | VLAN maken |
| `name STUDENTEN` | VLAN-naam |
| `no vlan 10` | VLAN verwijderen |
| `show vlan brief` | VLAN-overzicht |
| `show vlan id 10` | Specifiek VLAN |
| `show interfaces f0/1 switchport` | Switchportstatus |
| `switchport voice vlan 150` | Voice VLAN |

### 12. Trunk/DTP

| Command | Betekenis |
|---|---|
| `switchport mode trunk` | Trunk |
| `switchport trunk native vlan 99` | Native VLAN |
| `switchport trunk allowed vlan 10,20,99` | Allowed VLANs |
| `switchport trunk allowed vlan add 30` | VLAN toevoegen |
| `switchport trunk allowed vlan remove 20` | VLAN verwijderen uit lijst |
| `switchport nonegotiate` | DTP uit |
| `switchport mode dynamic desirable` | DTP actief |
| `switchport mode dynamic auto` | DTP passief |
| `switchport trunk encapsulation dot1q` | 802.1Q kiezen (alleen ondersteunde platformen) |
| `show interfaces trunk` | Trunks controleren |
| `show interfaces switchport` | Switchportdetails |

### 13. Router-on-a-Stick

| Command | Betekenis |
|---|---|
| `interface g0/0.10` | Subinterface |
| `encapsulation dot1Q 10` | VLAN-tag 10 |
| `encapsulation dot1Q 99 native` | Native VLAN |
| `ip address 192.168.10.1 255.255.255.0` | VLAN-gateway |
| `show ip interface brief` | Overzicht |

### 14. Layer 3-switch

| Command | Betekenis |
|---|---|
| `ip routing` | IPv4-routing aan |
| `no switchport` | Routed port |
| `interface vlan 10` | SVI |
| `show ip route` | Routes bekijken |

### 15. STP

| Command | Betekenis |
|---|---|
| `spanning-tree mode pvst` | PVST+ |
| `spanning-tree mode rapid-pvst` | Rapid PVST+ |
| `spanning-tree vlan 10 root primary` | Primary root |
| `spanning-tree vlan 10 root secondary` | Secondary root |
| `spanning-tree vlan 10 priority 4096` | Bridge-prioriteit |
| `spanning-tree portfast` | PortFast (edge) |
| `spanning-tree bpduguard enable` | BPDU Guard |
| `spanning-tree guard root` | Root Guard |
| `show spanning-tree` | STP-overzicht |
| `show spanning-tree vlan 10` | STP per VLAN |
| `show spanning-tree summary` | STP-samenvatting |
| `show spanning-tree interface f0/1 detail` | Poortdetails |

### 16. EtherChannel

| Command | Betekenis |
|---|---|
| `channel-group 1 mode active` | LACP actief |
| `channel-group 1 mode passive` | LACP passief |
| `channel-group 1 mode desirable` | PAgP actief |
| `channel-group 1 mode auto` | PAgP passief |
| `channel-group 1 mode on` | EtherChannel zonder onderhandeling |
| `interface port-channel 1` | Logische interface |
| `show etherchannel summary` | Kort overzicht |
| `show etherchannel detail` | Details |
| `show etherchannel port-channel` | Port-channel informatie |
| `show interfaces port-channel 1` | Logische interface |
| `show lacp neighbor` | LACP-buren |
| `show pagp neighbor` | PAgP-buren |

### 17. DHCPv4

| Command | Betekenis |
|---|---|
| `ip dhcp excluded-address 192.168.10.1 192.168.10.20` | Reservering via uitsluiting |
| `ip dhcp pool VLAN10` | Pool |
| `network 192.168.10.0 255.255.255.0` | Uit te delen subnet |
| `default-router 192.168.10.1` | Gateway |
| `dns-server 8.8.8.8` | DNS |
| `domain-name school.local` | Domein |
| `lease 7` | 7 dagen lease |
| `ip helper-address 192.168.1.10` | DHCP-relay op interface |
| `ip address dhcp` | DHCP-client op interface |
| `show ip dhcp binding` | Leases |
| `show ip dhcp pool` | Poolstatistieken |
| `show ip dhcp server statistics` | DHCP-statistieken |
| `clear ip dhcp binding *` | Leases wissen |

### 18. DHCPv6/SLAAC

| Command | Betekenis |
|---|---|
| `ipv6 unicast-routing` | IPv6-routering |
| `ipv6 dhcp pool LAN` | DHCPv6-pool |
| `address prefix 2001:DB8:1::/64` | Stateful-adresprefix |
| `dns-server 2001:DB8::53` | IPv6 DNS |
| `domain-name school.local` | Domein |
| `ipv6 dhcp server LAN` | Pool op interface |
| `ipv6 nd managed-config-flag` | M-flag |
| `ipv6 nd other-config-flag` | O-flag |
| `ipv6 dhcp client pd PREFIX` | Prefix Delegation-client |
| `show ipv6 dhcp interface` | DHCPv6 interface |
| `show ipv6 dhcp pool` | Pools |
| `show ipv6 dhcp binding` | Bindings |
| `show ipv6 neighbors` | Neighbor cache |

### 19. Static/default routes

| Command | Betekenis |
|---|---|
| `ip route 192.168.20.0 255.255.255.0 10.0.0.2` | Next-hop static route |
| `ip route 0.0.0.0 0.0.0.0 10.0.0.2` | Default route |
| `ip route 192.168.20.0 255.255.255.0 g0/1` | Exit-interface route |
| `ip route 192.168.20.0 255.255.255.0 10.0.0.2 5` | Administrative distance 5 |
| `ip route 192.168.20.5 255.255.255.255 10.0.0.2` | Hostroute |
| `ipv6 route 2001:DB8:20::/64 2001:DB8:12::2` | IPv6-static route |
| `ipv6 route ::/0 2001:DB8:12::2` | IPv6-default route |
| `show ip route static` | IPv4-static routes |
| `show ipv6 route static` | IPv6-static routes |

### 20. Port Security

| Command | Betekenis |
|---|---|
| `switchport port-security` | Inschakelen |
| `switchport port-security maximum 2` | Maximaal 2 MACs |
| `switchport port-security mac-address sticky` | Sticky MAC-learning |
| `switchport port-security mac-address 0011.2233.4455` | Vast MAC-adres |
| `switchport port-security violation shutdown` | Err-disable bij overtreding |
| `switchport port-security violation restrict` | Blokkeren en loggen |
| `switchport port-security violation protect` | Blokkeren |
| `switchport port-security aging time 10` | Agingtijd |
| `show port-security` | Overzicht |
| `show port-security interface f0/1` | Interface-details |
| `show port-security address` | Beveiligde MACs |
| `shutdown / no shutdown` | Herstellen (na oorzaak oplossen) |

### 21. DHCP Snooping en DAI

| Command | Betekenis |
|---|---|
| `ip dhcp snooping` | Globaal aan |
| `ip dhcp snooping vlan 10` | Per VLAN aan |
| `ip dhcp snooping trust` | Interface vertrouwen |
| `ip dhcp snooping limit rate 10` | Rate-limit |
| `show ip dhcp snooping` | Status |
| `show ip dhcp snooping binding` | Bindings |
| `ip arp inspection vlan 10` | DAI per VLAN |
| `ip arp inspection trust` | ARP trusted interface |
| `show ip arp inspection` | DAI-status |
| `show ip arp inspection statistics` | DAI-statistieken |

### 22. Switchbeveiliging

| Command | Betekenis |
|---|---|
| `switchport mode access` | Statische accesspoort |
| `switchport nonegotiate` | DTP uit |
| `switchport access vlan 999` | Ongebruikt VLAN |
| `shutdown` | Ongebruikte poort uit |
| `spanning-tree portfast` | Edge-PortFast |
| `spanning-tree bpduguard enable` | Guard |

### 23. SSH

| Command | Betekenis |
|---|---|
| `crypto key generate rsa` | RSA-sleutel aanmaken |
| `crypto key generate rsa modulus 2048` | 2048-bit RSA (platformafhankelijk) |
| `ip ssh version 2` | SSHv2 |
| `ip ssh time-out 60` | Timeout |
| `ip ssh authentication-retries 2` | Maximaal pogingen |
| `show ip ssh` | SSH-configuratie |
| `show ssh` | Sessies |
| `ssh -l admin 192.168.1.1` | Verbinding starten |

### 24. HSRP (deels conceptueel)

| Command | Betekenis |
|---|---|
| `standby 1 ip 192.168.1.254` | Virtuele gateway |
| `standby 1 priority 110` | Prioriteit |
| `standby 1 preempt` | Preemption |
| `show standby` | HSRP-status |
| `show standby brief` | Samenvatting |

### 25. Wireless

Wireless LAN Controller-configuratie gebeurt in CCNA 2 vaak via een
GUI; daardoor geen universele IOS-commandolijst. Ken SSID,
beveiliging (WPA2/WPA3), RADIUS, VLAN en DHCP.

---

## CCNA 3 – Enterprise Networking, Security and Automation


### 26. OSPFv2

| Command | Betekenis |
|---|---|
| `router ospf 1` | OSPF-proces |
| `router-id 1.1.1.1` | Router-ID |
| `network 192.168.1.0 0.0.0.255 area 0` | OSPF-networkstatement |
| `network 10.0.0.0 0.0.0.3 area 0` | P2P-netwerk in area 0 |
| `passive-interface g0/0` | Geen Hello's via interface |
| `passive-interface default` | Standaard passief |
| `no passive-interface g0/1` | Buurvorming toestaan |
| `auto-cost reference-bandwidth 1000` | Reference bandwidth |
| `default-information originate` | Default route adverteren (als aanwezig) |
| `ip ospf 1 area 0` | OSPF op interface |
| `ip ospf cost 10` | OSPF-kost |
| `ip ospf priority 100` | DR/BDR-verkiezingsprioriteit |
| `ip ospf hello-interval 10` | Hellotimer |
| `ip ospf dead-interval 40` | Deadtimer |
| `bandwidth 100000` | Interfacebandbreedte voor o.a. metric |
| `clear ip ospf process` | OSPF herstarten |
| `no router ospf 1` | OSPF-proces verwijderen |

### 27. OSPF-inspectie

| Command | Betekenis |
|---|---|
| `show ip ospf` | OSPF-proces |
| `show ip ospf neighbor` | Buren |
| `show ip ospf neighbor detail` | Buurdetails |
| `show ip ospf interface` | OSPF-interfaces |
| `show ip ospf interface brief` | Kort overzicht |
| `show ip ospf database` | Link-state database |
| `show ip ospf database router` | Router-LSA's |
| `show ip protocols` | Routingprotocolstatus |
| `show ip route ospf` | OSPF-routes |
| `show ip route` | Routes |

### 28. Numbered standard ACLs

| Command | Betekenis |
|---|---|
| `access-list 10 permit 192.168.1.0 0.0.0.255` | Subnet toestaan |
| `access-list 10 deny 192.168.2.0 0.0.0.255` | Subnet weigeren |
| `access-list 10 permit any` | Alle bronnen toestaan |
| `access-list 10 permit host 192.168.1.10` | Eén host toestaan |
| `access-list 10 deny any` | Overige bronnen weigeren |
| `ip access-group 10 in` | ACL inkomend |
| `ip access-group 10 out` | ACL uitgaand |
| `no access-list 10` | ACL verwijderen |
| `show access-lists` | ACLs bekijken |
| `show ip interface g0/0` | ACL-koppeling controleren |

### 29. Named standard ACLs

| Command | Betekenis |
|---|---|
| `ip access-list standard MANAGEMENT` | Named standard ACL |
| `permit 192.168.1.0 0.0.0.255` | Subnet toestaan |
| `deny any` | Overig weigeren |
| `10 permit host 192.168.1.10` | Regel met volgnummer |
| `no 10` | Regel 10 verwijderen |
| `show ip access-lists` | IPv4 ACLs |

### 30. Numbered extended ACLs

| Command | Betekenis |
|---|---|
| `access-list 100 permit ip any any` | Alle IPv4 toestaan |
| `access-list 100 deny ip any any` | Alle IPv4 weigeren |
| `access-list 100 permit icmp any any` | ICMP toestaan |
| `access-list 100 permit tcp any any eq 80` | HTTP toestaan |
| `access-list 100 permit tcp any any eq 443` | HTTPS toestaan |
| `access-list 100 permit tcp any any eq 22` | SSH toestaan |
| `access-list 100 permit udp any any eq 53` | UDP DNS toestaan |
| `access-list 100 permit tcp any any established` | TCP met ACK/RST matchen |
| `access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80` | HTTP vanaf subnet |
| `access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443` | HTTPS vanaf subnet |
| `ip access-group 100 in` | ACL toepassen |

### 31. Named extended ACLs

| Command | Betekenis |
|---|---|
| `ip access-list extended WEB_ACCESS` | Named extended ACL |
| `remark Alleen webverkeer` | Opmerking |
| `permit tcp any any eq 443` | HTTPS |
| `permit tcp 192.168.10.0 0.0.0.255 any eq 80` | HTTP vanaf subnet |
| `permit tcp 192.168.10.0 0.0.0.255 any eq 443` | HTTPS vanaf subnet |
| `deny icmp any any` | ICMP weigeren |
| `deny ip any any` | Alle andere IPv4 weigeren |
| `no 20` | ACL-regel 20 verwijderen |
| `show access-lists WEB_ACCESS` | Eén ACL bekijken |
| `access-class 10 in` | ACL toepassen op VTY-inlogtoegang |

### 32. Static NAT

| Command | Betekenis |
|---|---|
| `ip nat inside source static 192.168.1.10 203.0.113.10` | Vast intern ↔ extern adres |
| `ip nat inside` | Inside-interface |
| `ip nat outside` | Outside-interface |
| `show ip nat translations` | Vertalingen |
| `show ip nat statistics` | NAT-statistieken |

### 33. Dynamic NAT

| Command | Betekenis |
|---|---|
| `access-list 1 permit 192.168.1.0 0.0.0.255` | Match intern netwerk |
| `ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0` | NAT-adrespool |
| `ip nat inside source list 1 pool PUBLIC` | ACL aan pool koppelen |
| `show ip nat translations` | Vertalingen |

### 34. PAT

| Command | Betekenis |
|---|---|
| `ip nat inside source list 1 interface g0/1 overload` | PAT via interface-IP |
| `ip nat inside source list 1 pool PUBLIC overload` | PAT via NAT-pool |
| `show ip nat translations` | Vertalingen |
| `show ip nat statistics` | Statistieken |
| `clear ip nat translation *` | Dynamische entries wissen |
| `debug ip nat` | NAT-debug |

### 35. NTP

| Command | Betekenis |
|---|---|
| `ntp server 192.168.1.100` | NTP-server |
| `ntp master 3` | Lokale NTP-master |
| `show ntp associations` | Peers |
| `show ntp status` | Synchronisatie |
| `clock set 14:30:00 27 Oct 2026` | Handmatige tijd |
| `show clock detail` | Tijd-details |

### 36. Syslog

| Command | Betekenis |
|---|---|
| `logging host 192.168.1.100` | Syslog-server |
| `logging trap informational` | Meldingsniveau |
| `logging buffered 16384` | Bufferomvang |
| `logging console warnings` | Consolelogniveau |
| `service timestamps log datetime msec` | Timestamps |
| `show logging` | Logs bekijken |

### 37. SNMP

| Command | Betekenis |
|---|---|
| `snmp-server community PUBLIC RO` | SNMPv1/v2c read-only (legacy) |
| `snmp-server community PRIVATE RW` | SNMPv1/v2c read-write (legacy en onveilig) |
| `snmp-server location Serverruimte` | Locatie |
| `snmp-server contact Beheerder` | Contact |
| `snmp-server host 192.168.1.100 version 2c PUBLIC` | Notificatiedoel |
| `show snmp` | SNMP-status |
| `show snmp community` | Communities |

### 38. CDP/LLDP

| Command | Betekenis |
|---|---|
| `cdp run` | CDP globaal aan |
| `no cdp run` | CDP globaal uit |
| `cdp enable` | CDP op interface aan |
| `no cdp enable` | CDP op interface uit |
| `show cdp neighbors` | Cisco-buren |
| `show cdp neighbors detail` | Details |
| `show cdp interface` | CDP-interfaces |
| `lldp run` | LLDP globaal aan |
| `no lldp run` | LLDP globaal uit |
| `lldp transmit` | LLDP zenden |
| `lldp receive` | LLDP ontvangen |
| `show lldp neighbors` | Buren |
| `show lldp neighbors detail` | Details |
| `show lldp interface` | Interfaces |

### 39. IOS/configuratiebestanden

| Command | Betekenis |
|---|---|
| `copy running-config startup-config` | Opslaan |
| `copy running-config tftp:` | Running config back-uppen |
| `copy startup-config tftp:` | Startup config back-uppen |
| `copy tftp: running-config` | Config samenvoegen |
| `copy tftp: startup-config` | Startup config herstellen |
| `copy flash: tftp:` | Bestand exporteren |
| `copy tftp: flash:` | Bestand importeren |
| `dir flash:` | Flash-inhoud |
| `show file systems` | Bestandssystemen |
| `verify /md5 flash:bestand.bin` | Hashcontrole (indien ondersteund) |

### 40. Troubleshooting/debug

| Command | Betekenis |
|---|---|
| `show ip interface brief` | IPv4-interfaces |
| `show ipv6 interface brief` | IPv6-interfaces |
| `show interfaces` | Errors/drops |
| `show ip route` | IPv4-routes |
| `show ipv6 route` | IPv6-routes |
| `show ip protocols` | Routingstatus |
| `show arp` | ARP |
| `show mac address-table` | MAC-learning |
| `show vlan brief` | VLANs |
| `show interfaces trunk` | Trunks |
| `show spanning-tree` | STP |
| `show etherchannel summary` | EtherChannel |
| `show ip ospf neighbor` | OSPF-buren |
| `show access-lists` | ACL-hits |
| `show ip nat translations` | NAT |
| `ping <IP>` | Connectiviteit |
| `traceroute <IP>` | Pad |
| `debug ip ospf adj` | OSPF-debug |
| `debug ip icmp` | ICMP-debug |
| `debug ip routing` | Routingdebug |
| `undebug all` | Alle debug uit |
| `terminal monitor` | Logs via VTY tonen |

### 41. QoS (aanvullend; platformafhankelijk)

| Command | Betekenis |
|---|---|
| `class-map match-any VOICE` | Class-map |
| `match dscp ef` | DSCP EF matchen |
| `policy-map QOS_POLICY` | Policy-map |
| `class VOICE` | Class in policy |
| `priority percent 20` | Prioriteitsbandbreedte |
| `bandwidth percent 30` | Bandbreedte reserveren |
| `service-policy output QOS_POLICY` | QoS-policy uitgaand |
| `show policy-map` | Policies |
| `show policy-map interface` | Policy-statistieken |

### 42. WAN/VPN (aanvullend; oudere platformen)

| Command | Betekenis |
|---|---|
| `encapsulation ppp` | PPP-encapsulation |
| `ppp authentication chap` | CHAP |
| `ppp authentication pap` | PAP (onveilig) |
| `show interfaces serial 0/0/0` | Serial-interface |
| `show controllers serial 0/0/0` | Serial-controller |
| `show crypto isakmp sa` | IKEv1 SA-status |
| `show crypto ipsec sa` | IPsec SA-status |

## Belangrijkste oefenblokken


- **CCNA 1**: enable, configure terminal, hostname, enable secret, interface, ip address, no shutdown, show ip interface brief, copy running-config startup-config, ipv6 unicast-routing.

- **CCNA 2**: vlan, switchport mode access, switchport access vlan, switchport mode trunk, encapsulation dot1Q, ip routing, ip dhcp pool, ip helper-address, spanning-tree, channel-group, switchport port-security, ip route.

- **CCNA 3**: router ospf, network ... area 0, show ip ospf neighbor, ip access-list, ip access-group, ip nat inside, ip nat outside, ip nat inside source ... overload, show ip nat translations, show access-lists.

> Let op: IOS-commando's zijn modusgevoelig. Sommige horen in global config, andere in interfaceconfig, routerconfig, ACL-config of EXEC. Deze lijst is een studieoverzicht en geen garantie dat ieder commando in jouw examen zit.
