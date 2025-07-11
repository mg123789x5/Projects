

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





