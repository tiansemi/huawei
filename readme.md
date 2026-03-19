Très bonne base 👌 — ton deuxième fichier est déjà structuré, mais il mélange encore :

* **template générique**
* **config spécifique topo**
* **et quelques erreurs / incohérences Huawei**

Je vais faire 3 choses utiles pour toi :

1. **Réorganiser en vrai template pro (réutilisable examen/projet)**
2. **Corriger les commandes Huawei**
3. **Compléter avec les manques importants**

---

# 🔧 VERSION ORGANISÉE & CORRIGÉE (TEMPLATE FINAL)

Je te donne une version **claire, modulaire et propre**, inspirée des deux fichiers  

---

## 🔰 0. INITIALISATION (COMMUN À TOUS)

```bash
undo terminal monitor
system-view
sysname <HOSTNAME>

# Sauvegarde
quit
quit
save
yes
```

---

## 🧱 1. VLAN & PORTS (SWITCH)

```bash
# Création VLAN
vlan batch 2 to 20

# PORT ACCESS
interface GigabitEthernet0/0/X
port link-type access
port default vlan <VLAN_ID>

# PORT TRUNK
interface GigabitEthernet0/0/X
port link-type trunk
port trunk pvid vlan <NATIVE_VLAN>
port trunk allow-pass vlan <LIST>

# Vérification
display vlan
display port vlan
```

✅ Correction :

* `allow-pass vlan 10 20 30` OK
* éviter mélange `to` et liste sans logique

---

## 🌐 2. ADRESSAGE IP

```bash
interface GigabitEthernet0/0/X
ip address X.X.X.X 24

# Loopback
interface LoopBack0
ip address X.X.X.X 32

# VLANIF (L3 Switch)
interface Vlanif10
ip address 192.168.10.1 24
```

---

## 🔀 3. SOUS-INTERFACES

### Routeur

```bash
interface GigabitEthernet0/0/1.10
dot1q termination vid 10
ip address X.X.X.X 24
```

### Firewall Huawei

```bash
interface GigabitEthernet1/0/5.1
vlan-type dot1q 10
ip address X.X.X.X 24
```

---

## 🌳 4. MSTP

```bash
stp mode mstp

stp region-configuration
region-name RG1
instance 1 vlan 2 to 10
instance 2 vlan 11 to 20
active region-configuration
quit

stp enable

# Root
stp instance 1 root primary
stp instance 2 root secondary

# Sécurité
interface GigabitEthernet0/0/X
stp edged-port enable
stp root-protection
stp disable
```

---

## 🔥 5. FIREWALL (ZONES)

```bash
firewall zone trust
add interface GigabitEthernet1/0/X
quit

firewall zone untrust
add interface GigabitEthernet1/0/X
quit

firewall zone dmz
add interface GigabitEthernet1/0/X
quit

display firewall zone
```

---

## ⚖️ 6. VRRP (CORRIGÉ)

⚠️ ERREUR dans ton fichier : `active / standby` ≠ syntaxe Huawei standard

### MASTER

```bash
interface GigabitEthernet1/0/5.1
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode timer delay 20
```

### BACKUP

```bash
interface GigabitEthernet1/0/5.1
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 1 priority 100
```

### Tracking (important en exam)

```bash
vrrp vrid 1 track interface GigabitEthernet1/0/1 reduced 30
```

---

## 🔁 7. HRP (FIREWALL HA)

⚠️ Correction majeure (ta config incorrecte)

```bash
hrp enable

hrp interface GigabitEthernet1/0/1
hrp peer 152.1.1.2

hrp auto-sync enable
hrp mirror session enable

# Optionnel
hrp track interface GigabitEthernet1/0/1

display hrp state
```

---

## 🔐 8. SECURITY POLICY

```bash
security-policy
rule name INTERNET
 source-zone trust
 destination-zone untrust
 source-address 192.168.1.0 24
 action permit
quit
```

---

## 🌍 9. NAT

```bash
nat-policy
rule name NAT-OUT
 source-zone trust
 destination-zone untrust
 action source-nat easy-ip
quit
```

### NAT SERVER

```bash
nat server protocol tcp global 100.1.1.1 8080 inside 172.16.1.1 80
```

---

## 🧭 10. ROUTES STATIQUES

```bash
ip route-static 0.0.0.0 0 X.X.X.X

# Route spécifique
ip route-static 100.1.1.1 32 X.X.X.X
```

---

## 🛰️ 11. OSPF

```bash
ospf 1
area 0
network X.X.X.X 0.0.0.255
```

---

## 🧠 12. IS-IS

```bash
isis 1
is-level level-2
network-entity 49.0001.0000.0000.0001.00

interface GigabitEthernet0/0/X
isis enable 1
isis authentication-mode md5 Huawei@123
```

---

## 🌐 13. BGP

```bash
bgp 100
router-id 1.1.1.1

peer 2.2.2.2 as-number 100
peer 2.2.2.2 connect-interface LoopBack0

ipv4-family unicast
undo synchronization
peer 2.2.2.2 enable
```

---

## 🏷️ 14. MPLS + LDP

```bash
mpls lsr-id 1.1.1.1
mpls
mpls ldp

interface GigabitEthernet0/0/X
mpls
mpls ldp
```

---

## 🧳 15. VPN MPLS

```bash
ip vpn-instance CLIENT1
ipv4-family
route-distinguisher 100:1
vpn-target 100:1 both
```

---

## 🌉 16. GRE

```bash
interface Tunnel0/0/0
ip address 10.10.10.1 24
tunnel-protocol gre
source X.X.X.X
destination X.X.X.X
```

---

## 📡 17. WLAN (AC)

```bash
wlan
ap-group name default

ssid-profile name WIFI
ssid ICT

security-profile name SEC
security wpa-wpa2 psk pass-phrase Huawei@123 aes

vap-profile name VAP1
service-vlan vlan-id 20
ssid-profile WIFI
security-profile SEC
```


