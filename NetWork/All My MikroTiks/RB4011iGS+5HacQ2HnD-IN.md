

*Mikrotik 
RB4011iGS+5HacQ2HnD-IN 
Gigabit Ethernet 
2,4 GHz / 5 GHz
Must*

---
1) eth1 MAC Addtess: 
   Original: F4:1E:57:43:FE:B8,
   TP-Link: CC:32:E5:17:A9:2D 
   Teminal:`interface ethernet set mac-address=CC:32:E5:17:A9:2D ether1`

2) *Bridge:*
	1) **Add** new -> bridge1
	2) **Ports** -> Add eth ports (1...5)

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
	1) Interfaces -> Interface -> wlan -> Enable
	2) Wireless -> WiFi interface


