

*Mikrotik 
RB4011iGS+5HacQ2HnD-IN 
Gigabit Ethernet 
2,4 GHz / 5 GHz
Must*

---
#### [Базовая настройка маршрутизатора MikroTik для начинающих.](https://youtu.be/w0T9sxwSJ2Y?si=ulZNo9XXdEzEW505)


1) [Как изменит mac адрес на роутере MikroTik](https://youtu.be/Va80KAHxfL0?si=v1Z_PgaFXgGncwOq)
   eth1 MAC Addtess: 
   Original: F4:1E:57:43:FE:B8,
   TP-Link: CC:32:E5:17:A9:2D 
   Teminal:`interface ethernet set mac-address=CC:32:E5:17:A9:2D ether1`

2) *Bridge:*
	1) **Add** new -> bridge1
	2) **Ports** -> Add eth ports (2...5)

3) *Set IP bridge1:*
	**IP** -> Addresses -> Add:
		1) Address: 192.168.0.1/24
		2) Interface: bridge1

4) **ISP IP**:
	**IP** -> DHCP Cl. -> Add -> DHCP:
		Interface: ether1

5) Cl. Setup:
	1) DHCP server:
		1) IP->DHCP server
		2) DHCP -> DHCP Setup
		3) DHCP Interface: bridge1
		4) DHCP Address Space: 192.168.0.0/24
		5) GW for DHCP Net: 192.168.0.1
		6) Addresses to Give: 192.168.0.2-192.168.0.254
		7) DNS: 212.7.0.33, 212.7.9.34
		8) Lease Time: 00:30:00
		
	2) Internet for Cl. (NAT):
		1) IP -> Firewall -> NAT -> Add
		2) Chain: srcnat
		3) Out Intercafe: ether1
		4) Action -> Action: masquerade

6) Cl. WiFi:
	1) Interfaces -> Interface -> wlan2 -> Enable
	2) WirelessTables -> Sec.Profiles ->default
	3) Mode: dynamic keys
	4) WPA2PSK; KeySet `****`
	5) WiFi interface -> wlan2 up -> Wireless
	6) Mode: ap bridge
	7) Band: B/G/N
	8) Chanel Width: 20/40Mhz eC
	9) freq: auto (2447)
	10) SSID: ´_´
	11) Hide SSID

7) WireWall:
	1) IP->Firewall->Filter Rules -> Add
	2) General: 
		1) RULE 1 allow:
			1) Chain: Input
			2) In Interface: ether1
			3) Con-on State: established, related
			4) Action: Action:accept
		2) RULE 2 block:
			1) Chain: Input
			2) In Interface: ether1
			3) Con-on State: invalid
			4) Action: Action:drop
		3) RULE 3 forward:
			1) Chain: Forward
			2) Con-on State: established, related
			3) Action: Action:accept
		4) RULE 4 forward:
			1) Chain: Forward
			2) Con-on State: invalid
			3) Action: Action:drop
		5) RULE 2 block:
			1) Chain: Input
			2) In Interface: ether1
			3) Con-on State: invalid
			4) Action: Action:drop

8) System 
	1) Password
	2) Packages: 
		1) check, downloadand install
	3) RouterBOARD
		1) upgrade





























