# CONFIGURATION FIREWALL (INTERFACE / ZONE / POLICY)

```bash
system-view

interface GigabitEthernet0/0/1
 (optional) service-manage ping permit
 (optional) service-manage ssh permit
 (optional) service-manage http permit
 (optional) service-manage https permit

quit

firewall zone name trust
 (optional) set priority 85
 add interface GigabitEthernet0/0/1

quit

firewall zone name untrust
 (optional) set priority 5

quit

security-policy

 rule name POLICY-1
  source-zone trust
  destination-zone untrust
  (optional) source-address 192.168.1.0 24
  (optional) destination-address any
  (optional) service any
  action permit

quit

display firewall zone
display security-policy
```

---

```bash
[Huawei] interface interface-type interface-number
```

* **Créer une interface ou entrer dans la vue interface**
  Identique aux routeurs/switches Huawei.

---

```bash
[Huawei-GigabitEthernet0/0/1] service-manage { http | https | ping | ssh | snmp | netconf | telnet | all } { permit | deny }
```

* *(Optionnel)* **Configurer les protocoles autorisés sur l’interface**
  Permet d’autoriser ou bloquer l’accès au firewall via :
  **HTTP, HTTPS, ping, SSH, SNMP, NETCONF, Telnet**

  👉 Par défaut :

  * Interface management : HTTP / HTTPS / ping autorisés
  * Interfaces non-management : tout désactivé

---

```bash
[Huawei] firewall zone name zone-name [ id id ]
```

* **Créer une zone de sécurité et entrer dans sa vue**
  ID : **4 à 99**

---

```bash
[Huawei-zone-name] set priority security-priority
```

* *(Optionnel)* **Configurer la priorité de la zone**
  Valeur : **1 à 100**
  👉 Plus la valeur est grande → plus la priorité est élevée

---

```bash
[Huawei-zone-name] add interface interface-type interface-number
```

* **Associer une interface à une zone**
  Interface physique ou logique

---

```bash
[Huawei] security-policy
```

* **Entrer dans la vue des politiques de sécurité**

---

```bash
[Huawei-policy-security] rule name rule-name
```

* **Créer une règle de sécurité**

---

```bash
[Huawei-policy-security-rule-name] source-zone { zone-name | any }
```

* **Configurer la zone source**

---

```bash
[Huawei-policy-security-rule-name] destination-zone { zone-name | any }
```

* **Configurer la zone destination**

---

```bash
[Huawei-policy-security-rule-name] source-address ipv4-address { mask-length | mask mask-address }
```

* *(Optionnel)* **Configurer l’adresse source**
  👉 support wildcard mask

---

```bash
[Huawei-policy-security-rule-name] destination-address ipv4-address { mask-length | mask mask-address }
```

* *(Optionnel)* **Configurer l’adresse destination**

---

```bash
[Huawei-policy-security-rule-name] service { service-name | any }
```

* *(Optionnel)* **Configurer le service**
  (ports TCP/UDP, protocoles IP…)

---

```bash
[Huawei-policy-security-rule-name] action { permit | deny }
```

* **Configurer l’action de la règle**

👉 Par défaut : **deny**

