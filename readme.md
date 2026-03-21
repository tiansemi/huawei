# 🧠 HUAWEI ICT DATACOM – TEMPLATE ULTRA OPTIMISÉ & EXHAUSTIF

---

# 🔰 0. INITIALISATION

```
undo terminal monitor
system-view
sysname <DEVICE>
```

---

# 🧱 1. VLAN & SWITCHING

## Création VLAN

```
vlan batch 2 to 4094
```

## Port ACCESS

```
interface GigabitEthernet0/0/X
port link-type access
port default vlan <ID>
```

## Port TRUNK

```
interface GigabitEthernet0/0/X
port link-type trunk
port trunk pvid vlan <ID>
port trunk allow-pass vlan all
```

## Hybrid (rare mais examen)

```
port link-type hybrid
port hybrid tagged vlan 10 20
port hybrid untagged vlan 1
```

---

# 🌐 2. INTER-VLAN ROUTING

```
interface Vlanif10
ip address 192.168.10.1 24
```

---

# 🔀 3. SOUS-INTERFACE

```
interface GigabitEthernet0/0/1.10
dot1q termination vid 10
ip address 192.168.10.1 24
```

---

# 🌳 4. STP / MSTP

```
stp mode mstp
stp region-configuration
region-name RG1
instance 1 vlan 1 to 100
active region-configuration
quit
stp enable
```

## Root

```
stp instance 1 root primary
stp instance 1 root secondary
```

## Sécurité

```
stp edged-port enable
stp bpdu-protection
stp root-protection
```

---

# ⚖️ 5. VRRP

```
interface Vlanif10
vrrp vrid 1 virtual-ip 192.168.10.254
vrrp vrid 1 priority 120 // Si master
vrrp vrid 1 preempt-mode timer delay 20 //optional
vrrp vrid 1 track interface GigabitEthernet1/0/1 reduced 30 //optional
quit

```

---

# 🔁 6. ETH-TRUNK (LACP)

```
interface Eth-Trunk1
mode lacp

interface GigabitEthernet0/0/1
eth-trunk 1
```

---

# 🔥 7. FIREWALL BASICS

```
firewall zone trust
add interface GigabitEthernet1/0/1
```

---

# 🔐 8. ACL

```
acl number 2000
rule permit source 192.168.1.0 0.0.0.255
```

---

# 🌍 9. NAT

```
nat-policy
rule name NAT
 source-zone trust
 destination-zone untrust
 action source-nat easy-ip
```

---

# 🧭 10. STATIC ROUTING

```
ip route-static 0.0.0.0 0 X.X.X.X
```

---

# 🛰️ 11. OSPF

```
router id 10.10.10.2
ospf 1
ospf 1 vpn-instance Finance&OA // Si routeur est PE
area 0
network 10.0.12.2 0.0.0.3
quit

```

---

# 🧠 12. IS-IS

```
isis 1
is-level level-2
cost-style wide
network-entity 49.0001.0100.1001.0002.00
is-name R2
quit
interface LoopBack0
isis enable 1
quit
interface GigabitEthernet0/0/1
isis enable 1
quit
```

---

# 🌐 13. BGP

```
bgp 65100
 undo default ipv4-unicast // optional
 router-id 1.1.1.1
 peer 10.10.10.3 as-number 65100
 peer 10.10.10.3 connect-interface LoopBack0
 peer 10.10.10.4 as-number 65100
 peer 10.10.10.4 connect-interface LoopBack0
 ipv4-family vpnv4
  undo policy vpn-target // Si routeur RR | P (celui entre les PE)
  peer 10.10.10.3 enable
  peer 10.10.10.3 reflect-client // Si routeur RR | P (celui entre les PE)
  peer 10.10.10.4 enable
  peer 10.10.10.4 reflect-client // Si routeur RR | P (celui entre les PE)
  quit
 quit
```

---

# 🏷️ 14. MPLS

```
mpls lsr-id 10.10.10.2
mpls
quit
mpls ldp
quit
interface GigabitEthernet0/0/1
mpls
mpls ldp
quit
interface GigabitEthernet0/0/2
mpls
mpls ldp
quit
```

---

# 🧳 15. VPN MPLS

```
ip vpn-instance Finance&OA
 route-distinguisher 65100:12
 vpn-target 65100:12 65001:65002
 quit
interface GigabitEthernet0/0/3
 display this
 ip binding vpn-instance Finance&OA
 ip address 10.0.12.2 255.255.255.0
 quit
```

---

# 🌉 16. GRE

```
interface Tunnel0/0/0
tunnel-protocol gre
```

---

# 📡 17. WLAN

```
wlan
ssid-profile name WIFI
```

---

# ⚙️ 18. DHCP

```
dhcp enable
ip pool LAN
network 192.168.1.0 mask 255.255.255.0
```

---

# 🔍 19. COMMANDES DE DEBUG

```
display ip interface brief
display routing-table
display arp
display mac-address
```

---

# 🧪 20. TEST

```
ping X.X.X.X
tracert X.X.X.X
```

---

# 🎯 ASTUCES EXAM

* Toujours configurer Loopback
* Toujours vérifier VLAN
* Toujours activer routing
* Vérifier ACL/NAT ordre
* Vérifier VRRP priorité
