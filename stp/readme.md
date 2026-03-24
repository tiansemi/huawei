# CONFIGURATION STP MSTP RSTP

```bash
system-view

stp mode rstp
stp enable
(optional) stp priority 4096
(optional) stp pathcost-standard dot1t

(optional) stp root primary
(optional) stp root secondary

(optional) stp bpdu-protection
(optional) stp tc-protection interval 10
(optional) stp tc-protection threshold 5

interface GigabitEthernet0/0/1
 (optional) stp cost 20000
 (optional) stp priority 16
 (optional) stp edged-port enable
 (optional) stp root-protection
 (optional) stp loop-protection

quit

display stp
display stp brief
display stp interface GigabitEthernet0/0/1
```

---

```bash
[Huawei] stp mode { stp | rstp | mstp }
```

* **Configurer le mode de fonctionnement**
   Le switch supporte trois modes : **STP, RSTP et MSTP**.
   Par défaut, le switch fonctionne en **mode MSTP**.

```bash
[Huawei] stp root primary
```

* *(Optionnel)* **Définir le switch comme root bridge**
   Par défaut, un switch n’est root d’aucun arbre STP.
   Après cette commande, sa **priorité est fixée à 0 (non modifiable)**.

```bash
[Huawei] stp root secondary
```

* *(Optionnel)* **Définir le switch comme root secondaire**
   Par défaut, aucun switch n’est root secondaire.
   Après cette commande, sa **priorité est fixée à 4096 (non modifiable)**.

```bash
[Huawei] stp priority priority
```

* *(Optionnel)* **Configurer la priorité STP du switch**
   Valeur : **0 à 61440**, pas de **4096**
   Par défaut : **32768**

```bash
[Huawei] stp pathcost-standard { dot1d-1998 | dot1t | legacy }
```

* *(Optionnel)* **Configurer la méthode de calcul du coût de chemin**
   Par défaut : **IEEE 802.1t (dot1t)**
   👉 Tous les switches doivent utiliser **la même méthode**

```bash
[Huawei-GigabitEthernet0/0/1] stp cost cost
```

→ **Configurer le coût STP du port**

```bash
[Huawei-GigabitEthernet0/0/1] stp priority priority
```

* *(Optionnel)* **Configurer la priorité de l’interface**
   Valeur : **0 à 240**, pas de **16**
   Par défaut : **128**

```bash
[Huawei] stp enable
```

* **Activer STP/RSTP**
   Par défaut : **activé**

```bash
[Huawei-GigabitEthernet0/0/1] stp edged-port enable
```

* **Configurer le port comme edge port**
   Par défaut : tous les ports sont **non-edge**

```bash
[Huawei] stp bpdu-protection
```

* **Activer la protection BPDU (sur ports edge)**
   Par défaut : **désactivée**

```bash
[Huawei-GigabitEthernet0/0/1] stp root-protection
```

* **Configurer la protection du root**
   Par défaut : **désactivée**
   👉 Fonctionne uniquement sur les **ports désignés**
   👉 Incompatible avec **loop protection**

```bash
[Huawei-GigabitEthernet0/0/1] stp loop-protection
```

* **Configurer la protection contre les boucles**
   Pour **root port** ou **alternate port**
   Par défaut : **désactivée**

```bash
[Huawei] stp tc-protection interval interval-value
```

* **Configurer la protection contre les attaques TC BPDU**
   Définit l’intervalle de traitement des TC BPDU
   Par défaut : basé sur le **Hello timer**

```bash
[Huawei] stp tc-protection threshold threshold
```

* **Limiter le nombre de TC BPDU traités**
   Définit combien de TC BPDU sont traités dans un intervalle
   Par défaut : **1 seul TC BPDU**
