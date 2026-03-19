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
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode
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
ospf 1
area 0
network 10.0.0.0 0.0.0.255
```

---

# 🧠 12. IS-IS

```
isis 1
is-level level-2
network-entity 49.0001.0000.0000.0001.00
```

---

# 🌐 13. BGP

```
bgp 100
peer 1.1.1.1 as-number 100
```

---

# 🏷️ 14. MPLS

```
mpls
mpls ldp
```

---

# 🧳 15. VPN MPLS

```
ip vpn-instance VPN1
route-distinguisher 100:1
vpn-target 100:1 both
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

