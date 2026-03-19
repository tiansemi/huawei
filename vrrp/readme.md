**vue complète, propre et exploitable en lab/examen** de la configuration **VRRP sur Huawei** (routeur, switch L3, firewall).

---

# 🔥 1. VRRP — Rappels rapides

* VRRP = **passerelle virtuelle**
* 1 **Master**, 1+ **Backup**
* même **VRID + Virtual IP**

---

# 🖥️ 2. VRRP sur ROUTEUR Huawei (AR)

## 🔹 Configuration de base

```bash
system-view

interface GigabitEthernet0/0/0
 ip address 192.168.1.1 255.255.255.0

 vrrp vrid 1 virtual-ip 192.168.1.254
```

---

## 🔹 Définir priorité (Master)

```bash
vrrp vrid 1 priority 120
```

---

## 🔹 Backup

```bash
vrrp vrid 1 priority 100
```

---

## 🔹 Activer preemption

```bash
vrrp vrid 1 preempt-mode enable
```

---

## 🔹 Timer (optionnel)

```bash
vrrp vrid 1 timer advertise 1
```

---

## 🔹 Tracking (très important)

```bash
vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
```

👉 si lien tombe → priorité baisse

---

## 🔹 Authentification

```bash
vrrp vrid 1 authentication-mode simple cipher Huawei@123
```

---

## 🔹 Vérification

```bash
display vrrp
display vrrp brief
```

## Résumé des commandes

```
system-view
interface GigabitEthernet0/0/0
 ip address 192.168.1.1 255.255.255.0
 vrrp vrid 1 virtual-ip 192.168.1.254
 vrrp vrid 1 priority 120 (Master)
 vrrp vrid 1 priority 100 (Backup)
 vrrp vrid 1 track interface GigabitEthernet0/0/1 reduced 30
 vrrp vrid 1 preempt-mode enable (optionnel)
 vrrp vrid 1 timer advertise 1 (optionnel)
 vrrp vrid 1 authentication-mode simple cipher Huawei@123
 quit
display vrrp
display vrrp brief
```

---

# 🔁 3. VRRP sur SWITCH L3 (Huawei)

👉 Presque identique au routeur, mais sur interface VLAN (SVI)

---

## 🔹 Configuration

```bash
system-view

interface Vlanif10
 ip address 192.168.10.1 255.255.255.0

 vrrp vrid 10 virtual-ip 192.168.10.254
 vrrp vrid 10 priority 120
 vrrp vrid 10 preempt-mode enable
```

---

## 🔹 Backup switch

```bash
interface Vlanif10
 ip address 192.168.10.2 255.255.255.0

 vrrp vrid 10 virtual-ip 192.168.10.254
 vrrp vrid 10 priority 100
```

---

## 🔹 Tracking uplink

```bash
vrrp vrid 10 track interface GigabitEthernet0/0/1 reduced 20
```

---

## 🔹 Load balancing (multi-VRRP)

```bash
interface Vlanif10
 vrrp vrid 10 priority 120

interface Vlanif20
 vrrp vrid 20 priority 100
```

👉 Switch1 master VLAN10, Switch2 master VLAN20

---

## 🔹 Vérification

```bash
display vrrp
display vrrp interface Vlanif10
```

# Résumé des commandes

```
system-view
interface Vlanif10
 ip address 192.168.10.1 255.255.255.0
 vrrp vrid 10 virtual-ip 192.168.10.254
 vrrp vrid 10 priority 120
 vrrp vrid 10 preempt-mode enable
interface Vlanif10
 ip address 192.168.10.2 255.255.255.0
 vrrp vrid 10 virtual-ip 192.168.10.254
 vrrp vrid 10 priority 100
 vrrp vrid 10 track interface GigabitEthernet0/0/1 reduced 20
display vrrp
display vrrp interface Vlanif10
```

---

# 🔥 4. VRRP sur FIREWALL Huawei (USG)

👉 Très similaire mais utilisé avec HRP

---

## 🔹 Configuration

```bash
system-view

interface GigabitEthernet1/0/5.1
 ip address 192.168.1.1 255.255.255.0

 vrrp vrid 1 virtual-ip 192.168.1.254
 vrrp vrid 1 priority 120
 vrrp vrid 1 preempt-mode enable
```

---

## 🔹 Backup firewall

```bash
interface GigabitEthernet1/0/5.1
 ip address 192.168.1.2 255.255.255.0

 vrrp vrid 1 virtual-ip 192.168.1.254
 vrrp vrid 1 priority 100
```

---

## 🔹 Tracking WAN (très critique)

```bash
vrrp vrid 1 track interface GigabitEthernet1/0/3 reduced 30
```

---

## 🔹 VRRP + HRP (best practice)

```bash
hrp enable
hrp interface GigabitEthernet1/0/1
hrp peer 10.0.0.2
hrp auto-sync enable
display vrrp
display hrp state
```

---

# ⚙️ 5. Fonctions avancées VRRP (à connaître)

## 🔹 VRRP rapide

```bash
vrrp vrid 1 timer advertise 500
```

---

## 🔹 Delay preemption

```bash
vrrp vrid 1 preempt-mode timer delay 20
```

---

## 🔹 VRRP avec BFD (ultra rapide)

```bash
vrrp vrid 1 track bfd-session 1
```

---

## 🔹 Multi-VRRP (load balancing)

```bash
vrrp vrid 1 virtual-ip 192.168.1.254
vrrp vrid 2 virtual-ip 192.168.1.253
```

---

# 🧠 7. Commandes de debug essentielles

```bash
display vrrp
display vrrp brief
display vrrp statistics
display interface brief
```

---

# 🎯 8. Résumé rapide

| Équipement | Interface          |
| ---------- | ------------------ |
| Routeur    | Interface physique |
| Switch L3  | Vlanif             |
| Firewall   | Sous-interface     |

---

# 🚀 9. Astuce pro (très importante)

👉 En production :

* toujours configurer **tracking**
* toujours utiliser **preempt**
* sur firewall → toujours ajouter **HRP**

