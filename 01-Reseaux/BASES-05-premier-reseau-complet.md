# Ton premier réseau d'entreprise complet

## Message du formateur

**BRAVO !** Tu as appris les IPs, le subnetting, les VLANs, le DHCP. Maintenant on va **TOUT assembler** pour créer un **VRAI réseau d'entreprise**.

C'est le moment de briller ! Tu vas créer un réseau professionnel du début à la fin, comme dans une vraie entreprise.

**Ne stresse pas :** On va y aller **pas à pas**, très lentement. Tu as toutes les connaissances nécessaires. Il suffit juste de les mettre ensemble.

---

## Le projet final débutant

### Contexte

Une petite entreprise de **15 personnes** te demande de créer son réseau informatique.

**L'entreprise s'appelle :** TechCorp Solutions

**Leurs besoins :**
- Séparer les services par VLAN (sécurité)
- Distribution automatique des IPs (DHCP)
- Accès Internet pour tous
- Chaque service isolé des autres

---

### Les 3 départements

```
┌────────────────────────────────────┐
│  DIRECTION                         │
│  ────────────────                  │
│  3 personnes                       │
│  Besoin : Confidentialité max      │
│  Équipements : 3 PCs               │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│  COMPTABILITÉ                      │
│  ────────────────────              │
│  5 personnes                       │
│  Besoin : Sécurité (données paie)  │
│  Équipements : 5 PCs, 1 serveur    │
└────────────────────────────────────┘

┌────────────────────────────────────┐
│  COMMERCIAL                        │
│  ────────────────                  │
│  7 personnes                       │
│  Besoin : Accès Internet, mobilité │
│  Équipements : 7 PCs               │
└────────────────────────────────────┘

TOTAL : 15 PCs + 1 serveur
```

---

### Les exigences techniques

```
✅ 3 VLANs séparés (1 par service)
✅ IPs automatiques (DHCP)
✅ Accès Internet
✅ Chaque service isolé des autres
✅ Communication inter-VLANs possible (via routeur)
✅ Plan d'adressage clair
```

---

## Schéma de la solution

```
                      INTERNET
                         │
                         │
                 ┌───────▼────────┐
                 │    ROUTEUR     │
                 │                │
                 │  DHCP activé   │
                 │  3 sous-ifs    │
                 │  .10, .20, .30 │
                 └───────┬────────┘
                         │ (TRUNK)
                 ┌───────▼────────┐
                 │     SWITCH     │
                 │                │
                 │  VLANs:        │
                 │  10, 20, 30    │
                 └─┬──────┬───────┬┘
                   │      │       │
          ┌────────┘      │       └────────┐
          │               │                │
    ┌─────▼─────┐   ┌─────▼─────┐   ┌─────▼─────┐
    │ VLAN 30   │   │ VLAN 10   │   │ VLAN 20   │
    │ Direction │   │  Compta   │   │ Commerce  │
    │           │   │           │   │           │
    │ 3 PCs     │   │ 5 PCs     │   │ 7 PCs     │
    │           │   │ 1 Serveur │   │           │
    └───────────┘   └───────────┘   └───────────┘
   192.168.30.x   192.168.10.x    192.168.20.x
```

---

## Plan d'adressage IP

### Vue d'ensemble

```
┌──────────────────────────────────────────────────────┐
│  VLAN 10 : Comptabilité                              │
│  ─────────────────────────                           │
│  Réseau : 192.168.10.0/24                            │
│  Passerelle : 192.168.10.1 (routeur)                 │
│  DHCP range : 192.168.10.20 à 192.168.10.100         │
│  IPs fixes :                                         │
│    - Serveur Compta : 192.168.10.10                  │
│  Nombre de PCs : 5 + 1 serveur                       │
├──────────────────────────────────────────────────────┤
│  VLAN 20 : Commercial                                │
│  ─────────────────                                   │
│  Réseau : 192.168.20.0/24                            │
│  Passerelle : 192.168.20.1 (routeur)                 │
│  DHCP range : 192.168.20.20 à 192.168.20.100         │
│  Nombre de PCs : 7                                   │
├──────────────────────────────────────────────────────┤
│  VLAN 30 : Direction                                 │
│  ─────────────────                                   │
│  Réseau : 192.168.30.0/24                            │
│  Passerelle : 192.168.30.1 (routeur)                 │
│  DHCP range : 192.168.30.20 à 192.168.30.100         │
│  Nombre de PCs : 3                                   │
└──────────────────────────────────────────────────────┘
```

### Détail par VLAN

```
VLAN 10 (Comptabilité) - 192.168.10.0/24
────────────────────────────────────────
192.168.10.1       → Passerelle (routeur)
192.168.10.10      → Serveur Compta (IP fixe)
192.168.10.20-100  → PCs Compta (DHCP)

VLAN 20 (Commercial) - 192.168.20.0/24
───────────────────────────────────────
192.168.20.1       → Passerelle (routeur)
192.168.20.20-100  → PCs Commercial (DHCP)

VLAN 30 (Direction) - 192.168.30.0/24
──────────────────────────────────────
192.168.30.1       → Passerelle (routeur)
192.168.30.20-100  → PCs Direction (DHCP)
```

---

## Configuration complète - Étape par étape

### PARTIE 1 : Créer la topologie dans Packet Tracer

**Étape 1.1 : Ajouter les équipements**

1. Ouvre **Packet Tracer**

2. Ajoute **1 routeur 1841** :
   - Clique sur "Routers"
   - Choisis "1841"
   - Place-le en haut de l'écran

3. Ajoute **1 switch 2960** :
   - Clique sur "Switches"
   - Choisis "2960"
   - Place-le au centre de l'écran

4. Ajoute **15 PCs** :
   - Clique sur "End Devices"
   - Choisis "PC"
   - Ajoute 15 PCs en bas de l'écran
   - Renomme-les (clique sur le nom) :
     ```
     PC-Dir1, PC-Dir2, PC-Dir3
     PC-Compta1, PC-Compta2, PC-Compta3, PC-Compta4, PC-Compta5
     PC-Comm1, PC-Comm2, PC-Comm3, PC-Comm4, PC-Comm5, PC-Comm6, PC-Comm7
     ```

5. Ajoute **1 serveur** :
   - Clique sur "End Devices"
   - Choisis "Server"
   - Renomme-le en "Serveur-Compta"

---

**Étape 1.2 : Câbler les PCs au switch**

Utilise des câbles **Copper Straight-Through** (câbles droits).

```
Direction (VLAN 30) :
PC-Dir1 → Switch Fa0/1
PC-Dir2 → Switch Fa0/2
PC-Dir3 → Switch Fa0/3

Comptabilité (VLAN 10) :
PC-Compta1 → Switch Fa0/4
PC-Compta2 → Switch Fa0/5
PC-Compta3 → Switch Fa0/6
PC-Compta4 → Switch Fa0/7
PC-Compta5 → Switch Fa0/8
Serveur-Compta → Switch Fa0/9

Commercial (VLAN 20) :
PC-Comm1 → Switch Fa0/10
PC-Comm2 → Switch Fa0/11
PC-Comm3 → Switch Fa0/12
PC-Comm4 → Switch Fa0/13
PC-Comm5 → Switch Fa0/14
PC-Comm6 → Switch Fa0/15
PC-Comm7 → Switch Fa0/16
```

---

**Étape 1.3 : Câbler le switch au routeur (TRUNK)**

```
Switch Fa0/24 → Router Fa0/0
```

Attends que tous les câbles deviennent **VERTS**.

---

### PARTIE 2 : Configurer le switch (VLANs)

Clique sur le **Switch** → onglet **CLI** → Appuie sur Entrée

```cisco
! ═══════════════════════════════════════════════════════
! CONFIGURATION DU SWITCH - VLANs
! ═══════════════════════════════════════════════════════

Switch> enable
Switch# configure terminal

! ─────────────────────────────────────
! ÉTAPE 1 : Créer les 3 VLANs
! ─────────────────────────────────────

Switch(config)# vlan 10
Switch(config-vlan)# name COMPTABILITE
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name COMMERCIAL
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name DIRECTION
Switch(config-vlan)# exit

! ─────────────────────────────────────
! ÉTAPE 2 : Assigner les ports aux VLANs
! ─────────────────────────────────────

! Ports 1-3 : VLAN 30 (Direction)
Switch(config)# interface range fastEthernet 0/1-3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit

! Ports 4-9 : VLAN 10 (Comptabilité)
Switch(config)# interface range fastEthernet 0/4-9
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

! Ports 10-16 : VLAN 20 (Commercial)
Switch(config)# interface range fastEthernet 0/10-16
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

! ─────────────────────────────────────
! ÉTAPE 3 : Configurer le port TRUNK
! ─────────────────────────────────────

! Port 24 : TRUNK vers le routeur
Switch(config)# interface fastEthernet 0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit

! ─────────────────────────────────────
! ÉTAPE 4 : Sauvegarder
! ─────────────────────────────────────

Switch(config)# exit
Switch# write memory
```

---

**Vérification :**

```cisco
Switch# show vlan brief
```

**Résultat attendu :**

```
VLAN Name                             Status    Ports
---- -------------------------------- --------- ------------------------
1    default                          active    Fa0/17-23
10   COMPTABILITE                     active    Fa0/4-9
20   COMMERCIAL                       active    Fa0/10-16
30   DIRECTION                        active    Fa0/1-3
```

**Si tu vois ça, PARFAIT ! Les VLANs sont configurés.**

---

### PARTIE 3 : Configurer le routeur (DHCP + Routing)

Clique sur le **Router** → onglet **CLI** → Appuie sur Entrée

```cisco
! ═══════════════════════════════════════════════════════
! CONFIGURATION DU ROUTEUR - DHCP + ROUTING INTER-VLAN
! ═══════════════════════════════════════════════════════

Router> enable
Router# configure terminal

! ─────────────────────────────────────────────────────
! ÉTAPE 1 : Pools DHCP pour les 3 VLANs
! ─────────────────────────────────────────────────────

! Pool DHCP VLAN 10 (Comptabilité)
Router(config)# ip dhcp pool VLAN10_COMPTA
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.19

! Pool DHCP VLAN 20 (Commercial)
Router(config)# ip dhcp pool VLAN20_COMMERCIAL
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.19

! Pool DHCP VLAN 30 (Direction)
Router(config)# ip dhcp pool VLAN30_DIRECTION
Router(dhcp-config)# network 192.168.30.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.30.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.30.1 192.168.30.19

! ─────────────────────────────────────────────────────
! ÉTAPE 2 : Sous-interfaces pour routing inter-VLAN
! ─────────────────────────────────────────────────────

! Sous-interface pour VLAN 10 (Comptabilité)
Router(config)# interface fastEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

! Sous-interface pour VLAN 20 (Commercial)
Router(config)# interface fastEthernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! Sous-interface pour VLAN 30 (Direction)
Router(config)# interface fastEthernet 0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit

! Activer l'interface physique
Router(config)# interface fastEthernet 0/0
Router(config-if)# no shutdown
Router(config-if)# exit

! ─────────────────────────────────────────────────────
! ÉTAPE 3 : Sauvegarder
! ─────────────────────────────────────────────────────

Router(config)# exit
Router# write memory
```

---

**Vérification :**

```cisco
Router# show ip interface brief
```

**Résultat attendu :**

```
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        unassigned      YES unset  up                    up
FastEthernet0/0.10     192.168.10.1    YES manual up                    up
FastEthernet0/0.20     192.168.20.1    YES manual up                    up
FastEthernet0/0.30     192.168.30.1    YES manual up                    up
```

**Si tu vois ça, PARFAIT ! Le routeur est configuré.**

---

### PARTIE 4 : Configurer les PCs (DHCP automatique)

**Pour TOUS les PCs (sauf le serveur) :**

1. Clique sur le PC
2. Va dans **Desktop**
3. Clique sur **IP Configuration**
4. Coche **DHCP**
5. Attends quelques secondes...

**Résultats attendus :**

```
PCs Direction (VLAN 30) :
PC-Dir1 → 192.168.30.20 (ou autre IP en .30.x)
PC-Dir2 → 192.168.30.21
PC-Dir3 → 192.168.30.22

PCs Comptabilité (VLAN 10) :
PC-Compta1 → 192.168.10.20
PC-Compta2 → 192.168.10.21
PC-Compta3 → 192.168.10.22
PC-Compta4 → 192.168.10.23
PC-Compta5 → 192.168.10.24

PCs Commercial (VLAN 20) :
PC-Comm1 → 192.168.20.20
PC-Comm2 → 192.168.20.21
...
PC-Comm7 → 192.168.20.26
```

---

**Pour le serveur (IP fixe) :**

1. Clique sur **Serveur-Compta**
2. Desktop → IP Configuration
3. Coche **Static**
4. Entre :
   ```
   IP Address: 192.168.10.10
   Subnet Mask: 255.255.255.0
   Default Gateway: 192.168.10.1
   DNS Server: 8.8.8.8
   ```

---

### PARTIE 5 : Tests de validation

**Test 1 : PC du même VLAN**

```
PC-Compta1 ping PC-Compta2
→ ✅ Doit marcher (même VLAN 10)
```

1. Clique sur **PC-Compta1**
2. Desktop → Command Prompt
3. Tape : `ipconfig` (note l'IP de PC-Compta2)
4. Tape : `ping 192.168.10.21` (exemple)
5. **Résultat attendu : Reply from... ✅**

---

**Test 2 : PC vers serveur (même VLAN)**

```
PC-Compta1 ping Serveur-Compta
→ ✅ Doit marcher
```

1. Depuis **PC-Compta1**
2. Tape : `ping 192.168.10.10`
3. **Résultat attendu : Reply from... ✅**

---

**Test 3 : PC de VLANs différents (via routeur)**

```
PC-Compta1 ping PC-Comm1
→ ✅ Doit marcher (grâce au routeur)
```

1. Depuis **PC-Compta1**
2. Tape : `ipconfig` sur PC-Comm1 pour connaître son IP
3. Tape : `ping 192.168.20.20` (exemple)
4. **Résultat attendu : Reply from... ✅**

---

**Test 4 : Traceroute (voir le chemin)**

```
PC-Compta1 tracert PC-Comm1
→ Montre le passage par le routeur
```

1. Depuis **PC-Compta1**
2. Tape : `tracert 192.168.20.20`
3. **Résultat attendu :**
   ```
   Tracing route to 192.168.20.20 over a maximum of 30 hops:

   1   <1 ms   <1 ms   <1 ms   192.168.10.1 (routeur)
   2   <1 ms   <1 ms   <1 ms   192.168.20.20 (PC-Comm1)

   Trace complete.
   ```

**Analyse :** Le paquet passe d'abord par le routeur (192.168.10.1) avant d'arriver au PC-Comm1. C'est normal, c'est le routing inter-VLAN !

---

**Test 5 : Vérifier les IPs distribuées par DHCP**

```
Sur le routeur :
Router# show ip dhcp binding
```

**Résultat attendu :**

```
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.10.20    0001.9652.5D42          Mar 02 2024 12:00 PM    Automatic
192.168.10.21    0001.4325.AB12          Mar 02 2024 12:01 PM    Automatic
...
192.168.20.20    0001.8765.CD34          Mar 02 2024 12:02 PM    Automatic
...
192.168.30.20    0001.1234.EF56          Mar 02 2024 12:03 PM    Automatic
```

**Si tu vois 15 IPs distribuées (15 PCs), BRAVO !**

---

## Schéma récapitulatif de ton réseau

```
┌───────────────────────────────────────────────────────────┐
│                   RÉSEAU TECHCORP                         │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  VLAN 10 : COMPTABILITE (192.168.10.0/24)                 │
│  ──────────────────────────────────────                   │
│  - 5 PCs : 192.168.10.20-24 (DHCP)                        │
│  - Serveur : 192.168.10.10 (IP fixe)                      │
│  - Passerelle : 192.168.10.1                              │
│                                                           │
│  VLAN 20 : COMMERCIAL (192.168.20.0/24)                   │
│  ────────────────────────────────────                     │
│  - 7 PCs : 192.168.20.20-26 (DHCP)                        │
│  - Passerelle : 192.168.20.1                              │
│                                                           │
│  VLAN 30 : DIRECTION (192.168.30.0/24)                    │
│  ───────────────────────────────────                      │
│  - 3 PCs : 192.168.30.20-22 (DHCP)                        │
│  - Passerelle : 192.168.30.1                              │
│                                                           │
│  ÉQUIPEMENTS :                                            │
│  ────────────                                             │
│  - 1 routeur 1841 (DHCP + routing inter-VLAN)             │
│  - 1 switch 2960 (3 VLANs)                                │
│  - 15 PCs                                                 │
│  - 1 serveur                                              │
│                                                           │
│  FONCTIONNALITÉS :                                        │
│  ────────────────                                         │
│  ✅ Séparation par VLANs (sécurité)                       │
│  ✅ DHCP automatique (simplicité)                         │
│  ✅ Routing inter-VLAN (communication)                    │
│  ✅ Plan d'adressage clair (maintenabilité)               │
└───────────────────────────────────────────────────────────┘
```

---

## Exercice bonus : Ajouter un nouveau service

**Contexte :**

TechCorp embauche **4 personnes** pour un nouveau service : **Support Technique**.

**Tâches :**

1. Créer un **VLAN 40** (Support)
2. Réseau : **192.168.40.0/24**
3. Configurer **DHCP** pour ce VLAN
4. Ajouter **4 PCs** dans ce VLAN
5. Tester la communication

<details>
<summary>📖 Voir la solution</summary>

**Sur le switch :**

```cisco
Switch> enable
Switch# configure terminal

! Créer VLAN 40
Switch(config)# vlan 40
Switch(config-vlan)# name SUPPORT
Switch(config-vlan)# exit

! Assigner ports 17-20 au VLAN 40
Switch(config)# interface range fastEthernet 0/17-20
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 40
Switch(config-if-range)# exit

Switch(config)# exit
Switch# write memory
```

**Sur le routeur :**

```cisco
Router> enable
Router# configure terminal

! Pool DHCP VLAN 40
Router(config)# ip dhcp pool VLAN40_SUPPORT
Router(dhcp-config)# network 192.168.40.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.40.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.40.1 192.168.40.19

! Sous-interface VLAN 40
Router(config)# interface fastEthernet 0/0.40
Router(config-subif)# encapsulation dot1Q 40
Router(config-subif)# ip address 192.168.40.1 255.255.255.0
Router(config-subif)# exit

Router(config)# exit
Router# write memory
```

**Ajouter 4 PCs :**
- PC-Support1 → Switch Fa0/17
- PC-Support2 → Switch Fa0/18
- PC-Support3 → Switch Fa0/19
- PC-Support4 → Switch Fa0/20

**Configurer en DHCP :**
- Chaque PC : Desktop → IP Configuration → DHCP

**Tests :**
```
PC-Support1 ping PC-Support2 → ✅ OK
PC-Support1 ping PC-Compta1 → ✅ OK (via routeur)
```

**BRAVO ! Tu as ajouté un nouveau service au réseau !**

</details>

---

## Documentation de ton réseau (bonnes pratiques)

En entreprise, tu dois **documenter** ton réseau pour que d'autres techniciens puissent le comprendre.

### Tableau récapitulatif

```
┌──────────────┬─────────┬────────────────┬────────────┬──────────────┐
│ Service      │ VLAN ID │ Réseau         │ Passerelle │ DHCP Range   │
├──────────────┼─────────┼────────────────┼────────────┼──────────────┤
│ Comptabilité │ 10      │ 192.168.10.0/24│ .1         │ .20-.100     │
│ Commercial   │ 20      │ 192.168.20.0/24│ .1         │ .20-.100     │
│ Direction    │ 30      │ 192.168.30.0/24│ .1         │ .20-.100     │
└──────────────┴─────────┴────────────────┴────────────┴──────────────┘

IPs fixes :
- Routeur Fa0/0.10 : 192.168.10.1
- Routeur Fa0/0.20 : 192.168.20.1
- Routeur Fa0/0.30 : 192.168.30.1
- Serveur Compta : 192.168.10.10

Ports switch :
- Fa0/1-3 : VLAN 30 (Direction)
- Fa0/4-9 : VLAN 10 (Comptabilité)
- Fa0/10-16 : VLAN 20 (Commercial)
- Fa0/24 : TRUNK vers routeur

DNS : 8.8.8.8 (Google)
Durée bail DHCP : 24 heures
```

---

## Dépannage - Si ça ne marche pas

### Problème 1 : Un PC n'a pas d'IP (DHCP failed)

**Vérifications :**

1. **Le câble est-il vert ?**
   - Non → Vérifie les connexions

2. **Le PC est-il dans le bon VLAN ?**
   - Sur le switch : `show vlan brief`
   - Vérifie que le port est dans le bon VLAN

3. **Le pool DHCP est-il configuré ?**
   - Sur le routeur : `show ip dhcp pool`

4. **Le routeur a-t-il l'IP correcte ?**
   - Sur le routeur : `show ip interface brief`

5. **Refais une demande DHCP :**
   - Sur le PC : `ipconfig /release`
   - Puis : `ipconfig /renew`

---

### Problème 2 : Les PCs du même VLAN ne communiquent pas

**Vérifications :**

1. **Les PCs sont-ils dans le même réseau ?**
   - PC1 : 192.168.10.20
   - PC2 : 192.168.10.21
   - → OK, même réseau .10

2. **Le masque est-il correct ?**
   - Doit être 255.255.255.0

3. **Essaie de ping l'IP de ton propre PC :**
   - `ping 192.168.10.20` (ta propre IP)
   - Si ça ne marche pas, problème de config IP

---

### Problème 3 : Les VLANs ne communiquent pas entre eux

**Vérifications :**

1. **Le trunk est-il configuré ?**
   - Sur le switch : `show interfaces trunk`
   - Le port Fa0/24 doit être en mode trunk

2. **Les sous-interfaces sont-elles up ?**
   - Sur le routeur : `show ip interface brief`
   - Toutes les sous-interfaces doivent être "up"

3. **Les PCs ont-ils la bonne passerelle ?**
   - PC VLAN 10 → passerelle 192.168.10.1
   - PC VLAN 20 → passerelle 192.168.20.1

4. **Teste un ping vers la passerelle :**
   - Depuis PC-Compta1 : `ping 192.168.10.1`
   - Si ça marche, le routeur est joignable

---

## Félicitations !

```
🎉 🎉 🎉  BRAVO ! 🎉 🎉 🎉

Tu as créé ton PREMIER réseau d'entreprise COMPLET !

Ce que tu sais faire maintenant :
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Concevoir un plan d'adressage IP
✅ Créer des VLANs pour séparer les services
✅ Configurer DHCP pour automatiser la distribution d'IPs
✅ Configurer le routing inter-VLAN
✅ Câbler une topologie réseau
✅ Tester et dépanner un réseau
✅ Documenter un réseau

Tu peux être FIER de toi ! 💪

Compétences acquises :
━━━━━━━━━━━━━━━━━━━
- Adressage IP (IPv4)
- Subnetting (découpage de réseaux)
- VLANs (isolation par service)
- DHCP (distribution automatique)
- Routing inter-VLAN (router-on-a-stick)
- CLI Cisco (IOS)
- Packet Tracer (simulation réseau)

Niveau atteint : TECHNICIEN RÉSEAU JUNIOR
Tu es capable de créer et gérer un petit réseau d'entreprise !
```

---

## Prochaines étapes (pour aller plus loin)

**Tu veux progresser encore ? Voici les sujets à explorer :**

```
Niveau Intermédiaire :
──────────────────────
✓ Routage dynamique (OSPF, EIGRP)
✓ ACLs (listes de contrôle d'accès pour la sécurité)
✓ NAT/PAT (pour l'accès Internet)
✓ VPN (réseaux privés virtuels)
✓ QoS (qualité de service)
✓ STP (Spanning Tree Protocol)

Niveau Avancé :
───────────────
✓ BGP (routage Internet)
✓ MPLS (Multi-Protocol Label Switching)
✓ IPv6 (nouvelle version IP)
✓ SD-WAN (réseaux WAN définis par logiciel)
✓ Sécurité réseau avancée (pare-feu, IPS/IDS)
```

**Mais pour l'instant, PROFITE de ta réussite ! Tu as fait un ÉNORME travail ! 🏆**

---

## Challenges supplémentaires (si tu veux t'amuser)

### Challenge 1 : Grande entreprise

Crée un réseau pour une entreprise de **100 personnes** avec **5 services**.

### Challenge 2 : Multi-sites

Crée **2 sites distants** reliés par un routeur WAN.

### Challenge 3 : Redondance

Ajoute un **2e routeur** pour avoir de la redondance (si un tombe, l'autre prend le relais).

### Challenge 4 : WiFi

Ajoute un **point d'accès WiFi** pour le VLAN Commercial.

### Challenge 5 : Serveur DHCP dédié

Au lieu d'utiliser le routeur comme serveur DHCP, utilise un **serveur Windows/Linux** comme serveur DHCP.

---

**Tu as maintenant toutes les bases pour devenir un excellent technicien réseau. Bonne continuation ! 🚀**
