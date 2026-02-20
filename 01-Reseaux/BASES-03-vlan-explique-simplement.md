# C'est quoi un VLAN ? - Expliqué à un enfant

## Message du formateur

Les VLANs, c'est juste une façon de **séparer les gens** sur le MÊME switch. Imagine des **cloisons invisibles**.

C'est un concept qui semble compliqué mais en fait c'est très simple. Je vais te montrer avec des analogies concrètes.

**Important :** Si tu n'as pas encore lu les cours sur les IPs et le subnetting, va les lire d'abord.

---

## L'analogie de l'open-space

Imagine un bureau en open-space (grande pièce avec plein de bureaux).

### SANS VLAN (Open-space classique)

```
┌────────────────────────────────────────────────────┐
│        OPEN-SPACE - TOUT LE MONDE ENSEMBLE         │
│                                                    │
│  [Compta] [RH] [Compta] [Direction] [RH] [Compta] │
│                                                    │
│  Tout le monde dans la MÊME PIÈCE                  │
└────────────────────────────────────────────────────┘

Problèmes :
❌ Tout le monde voit/entend tout le monde
❌ Documents confidentiels visibles par tous
❌ Bruit partout (conversations, téléphones...)
❌ Pas sécurisé
❌ Pas organisé
```

### AVEC VLAN (Open-space cloisonné)

```
┌──────────────┬──────────────┬──────────────┐
│   ZONE 1     │   ZONE 2     │   ZONE 3     │
│              │              │              │
│  [Compta]    │    [RH]      │ [Direction]  │
│  [Compta]    │    [RH]      │ [Direction]  │
│              │              │              │
│  VLAN 10     │  VLAN 20     │  VLAN 30     │
└──────────────┴──────────────┴──────────────┘

Avantages :
✅ Chaque service a SA ZONE
✅ Cloisons invisibles (virtuelles)
✅ Un service ne voit PAS les autres
✅ Documents confidentiels protégés
✅ Sécurisé
✅ Organisé
✅ Sur le MÊME switch !
```

**La magie des VLANs :** Tu crées des séparations **virtuelles** sur un **seul** switch. C'est comme si tu avais plusieurs switchs, mais en vrai c'est le même !

---

## Les VLANs en vrai

### Un seul switch, mais 3 réseaux séparés

```
        ┌─────────── SWITCH ───────────┐
        │                              │
        │  Ports 1-8    : VLAN 10      │ ← Compta
        │  Ports 9-16   : VLAN 20      │ ← RH
        │  Ports 17-24  : VLAN 30      │ ← Direction
        │                              │
        └──────────────────────────────┘
              ↓       ↓       ↓
           [PC1]   [PC9]   [PC17]
          Compta    RH    Direction

PC1 (VLAN 10) NE PEUT PAS communiquer avec PC9 (VLAN 20)
→ Comme s'ils étaient sur 2 switchs différents
→ Mais en vrai, c'est le MÊME switch !
```

### Comment ça marche concrètement ?

```
Exemple simple :

Switch avec 24 ports :

Ports 1-8 → VLAN 10 (Compta)
- PC1 branché sur port 1
- PC2 branché sur port 2
- PC3 branché sur port 3

Ports 9-16 → VLAN 20 (RH)
- PC4 branché sur port 9
- PC5 branché sur port 10

Ports 17-24 → VLAN 30 (Direction)
- PC6 branché sur port 17

Résultat :
PC1 peut parler avec PC2 et PC3 (même VLAN)
PC1 NE PEUT PAS parler avec PC4 ou PC5 (VLAN différent)

Pour faire communiquer les VLANs, il faut un ROUTEUR.
```

---

## Pourquoi utiliser des VLANs ?

### Les 3 raisons principales

```
1. SÉCURITÉ
───────────
La compta ne veut PAS que les commerciaux
voient leurs fichiers de paie ou leurs données sensibles.

Solution : VLAN séparé pour la compta
→ Isolation totale
→ Même si les câbles sont sur le même switch,
  les données ne passent PAS d'un VLAN à l'autre


2. ORGANISATION
───────────────
Chaque service a son réseau
→ Plus facile à gérer
→ Plus clair
→ Plus professionnel

Exemple d'organisation :
VLAN 10 = Compta (192.168.10.0/24)
VLAN 20 = RH (192.168.20.0/24)
VLAN 30 = Direction (192.168.30.0/24)
VLAN 40 = Commercial (192.168.40.0/24)


3. PERFORMANCE
──────────────
Moins de "bruit" réseau (broadcast)
→ Réseau plus rapide
→ Moins de problèmes

Sans VLAN : Quand un PC cherche une IP,
il crie sur TOUT le réseau (broadcast)
→ 200 PCs entendent le cri
→ Ralentissement

Avec VLAN : Il crie seulement dans son VLAN
→ 20 PCs entendent le cri
→ Plus rapide
```

---

## Les 2 types de ports VLAN

Sur un switch, il y a **2 types de ports** :

### 1. Port ACCESS (le plus courant)

```
Port ACCESS = Port pour brancher un PC

┌──────┐         ┌────────┐
│  PC  │────────│ Switch │
└──────┘         │        │
                 │ VLAN 10│
                 └────────┘

Caractéristiques :
- Branché à UN SEUL VLAN
- Pour connecter : PCs, imprimantes, téléphones IP, serveurs
- Le PC ne sait PAS qu'il est dans un VLAN (transparent)

Commande Cisco :
switchport mode access
switchport access vlan 10
```

### 2. Port TRUNK (pour relier des switchs)

```
Port TRUNK = Port pour relier 2 switchs (ou switch-routeur)

┌────────┐         ┌────────┐
│ Switch1│─────────│Switch2 │
│        │  TRUNK  │        │
│ VLAN   │ (tous   │ VLAN   │
│ 10,20  │  les    │ 10,20  │
│   30   │  VLANs) │   30   │
└────────┘         └────────┘

Caractéristiques :
- Transporte PLUSIEURS VLANs en même temps
- Pour connecter : switch à switch, switch à routeur
- Utilise le protocole 802.1Q (tagging)

Commande Cisco :
switchport mode trunk
```

**Analogie :**
- Port ACCESS = route à 1 voie (1 seul VLAN)
- Port TRUNK = autoroute à plusieurs voies (tous les VLANs)

---

## TP Packet Tracer - Créer 3 VLANs

### Objectif

Créer **3 VLANs** (Compta, RH, Direction) sur un même switch.

À la fin :
- Les PCs du même VLAN peuvent communiquer
- Les PCs de VLANs différents NE peuvent PAS communiquer (isolés)

---

### Topologie cible

```
     PC-Compta1        PC-RH1          PC-Dir1
         │               │                │
     PC-Compta2        PC-RH2          PC-Dir2
         │               │                │
     ┌───┴───────────────┴────────────────┴───┐
     │          SWITCH (2960)                  │
     │                                         │
     │  VLAN 10: Ports Fa0/1-2 (Compta)        │
     │  VLAN 20: Ports Fa0/3-4 (RH)            │
     │  VLAN 30: Ports Fa0/5-6 (Direction)     │
     └─────────────────────────────────────────┘
```

---

### Étape 1 : Créer la topologie

1. Ajoute **1 switch 2960**
2. Ajoute **6 PCs** :
   - PC0 et PC1 (Compta)
   - PC2 et PC3 (RH)
   - PC4 et PC5 (Direction)

3. Câble les PCs au switch :
   - PC0 (Compta1) → Switch FastEthernet0/1
   - PC1 (Compta2) → Switch FastEthernet0/2
   - PC2 (RH1) → Switch FastEthernet0/3
   - PC3 (RH2) → Switch FastEthernet0/4
   - PC4 (Dir1) → Switch FastEthernet0/5
   - PC5 (Dir2) → Switch FastEthernet0/6

Attends que tous les câbles soient **verts**.

---

### Étape 2 : Configurer les IPs des PCs

**PCs Compta (VLAN 10) :**
```
PC0 (Compta1) :
  IP : 192.168.10.10
  Masque : 255.255.255.0
  Passerelle : 192.168.10.1

PC1 (Compta2) :
  IP : 192.168.10.11
  Masque : 255.255.255.0
  Passerelle : 192.168.10.1
```

**PCs RH (VLAN 20) :**
```
PC2 (RH1) :
  IP : 192.168.20.10
  Masque : 255.255.255.0
  Passerelle : 192.168.20.1

PC3 (RH2) :
  IP : 192.168.20.11
  Masque : 255.255.255.0
  Passerelle : 192.168.20.1
```

**PCs Direction (VLAN 30) :**
```
PC4 (Dir1) :
  IP : 192.168.30.10
  Masque : 255.255.255.0
  Passerelle : 192.168.30.1

PC5 (Dir2) :
  IP : 192.168.30.11
  Masque : 255.255.255.0
  Passerelle : 192.168.30.1
```

**Note :** Pour l'instant, on n'a pas de routeur, donc les passerelles ne servent pas encore. Mais c'est une bonne pratique de les configurer.

---

### Étape 3 : Configurer le switch (créer les VLANs)

Maintenant, on va créer les VLANs sur le switch avec la **ligne de commande Cisco**.

**Ouvrir le CLI du switch :**

1. Clique sur le **Switch**
2. Va dans l'onglet **"CLI"**
3. Appuie sur **Entrée**

---

**Configuration complète (copie-colle ligne par ligne) :**

```cisco
! Passer en mode privilégié
Switch> enable
Switch#

! Passer en mode configuration
Switch# configure terminal
Switch(config)#

! ═══════════════════════════════════════
! ÉTAPE 1 : CRÉER LES 3 VLANs
! ═══════════════════════════════════════

Switch(config)# vlan 10
Switch(config-vlan)# name COMPTA
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name RH
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name DIRECTION
Switch(config-vlan)# exit

! ═══════════════════════════════════════
! ÉTAPE 2 : ASSIGNER LES PORTS AUX VLANs
! ═══════════════════════════════════════

! Ports 1-2 : VLAN 10 (Compta)
Switch(config)# interface range fastEthernet 0/1-2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

! Ports 3-4 : VLAN 20 (RH)
Switch(config)# interface range fastEthernet 0/3-4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

! Ports 5-6 : VLAN 30 (Direction)
Switch(config)# interface range fastEthernet 0/5-6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit

! ═══════════════════════════════════════
! ÉTAPE 3 : SAUVEGARDER
! ═══════════════════════════════════════

Switch(config)# exit
Switch# write memory
```

---

### EXPLICATIONS DÉTAILLÉES (chaque commande)

```cisco
vlan 10
name COMPTA
```
**Explication :** Je crée un VLAN avec l'ID 10 et je l'appelle "COMPTA".
Le nom est juste pour nous, humains. Le switch utilise l'ID (10).

---

```cisco
interface range fastEthernet 0/1-2
```
**Explication :** Je sélectionne PLUSIEURS ports en même temps (du port 1 au port 2).
C'est plus rapide que de les configurer un par un.

---

```cisco
switchport mode access
```
**Explication :** Je dis que ce port est un port ACCESS (pour brancher un PC).
Pas un port TRUNK (pour relier des switchs).

---

```cisco
switchport access vlan 10
```
**Explication :** Je mets ce port dans le VLAN 10.
Tous les PCs branchés ici seront dans le VLAN Compta.

---

```cisco
write memory
```
**Explication :** Je sauvegarde la configuration.
Sinon, si le switch redémarre, je perds tout.

---

### Étape 4 : Vérifier la configuration

**Commande 1 : Voir tous les VLANs**

```cisco
Switch# show vlan brief
```

**Résultat attendu :**

```
VLAN Name                             Status    Ports
---- -------------------------------- --------- ------------------------
1    default                          active    Fa0/7, Fa0/8, Fa0/9...
10   COMPTA                           active    Fa0/1, Fa0/2
20   RH                               active    Fa0/3, Fa0/4
30   DIRECTION                        active    Fa0/5, Fa0/6
```

**Analyse :**
- VLAN 10 a les ports Fa0/1 et Fa0/2 ✅
- VLAN 20 a les ports Fa0/3 et Fa0/4 ✅
- VLAN 30 a les ports Fa0/5 et Fa0/6 ✅

Si tu vois ça, **BRAVO !** Tes VLANs sont bien configurés.

---

### Étape 5 : Tester

**Test 1 : PC0 (Compta) ping PC1 (Compta) - Même VLAN**

1. Clique sur PC0
2. Desktop → Command Prompt
3. Tape : `ping 192.168.10.11`
4. **Résultat attendu : Reply from... ✅**

Les 2 PCs sont dans le VLAN 10, ils peuvent communiquer.

---

**Test 2 : PC0 (Compta) ping PC2 (RH) - VLAN différent**

1. Depuis PC0
2. Tape : `ping 192.168.20.10`
3. **Résultat attendu : Request timeout ❌**

**C'est NORMAL !** Les 2 PCs sont dans des VLANs différents (10 et 20).
Ils sont **isolés**, comme s'ils étaient sur 2 switchs différents.

---

**Test 3 : PC2 (RH) ping PC3 (RH) - Même VLAN**

1. Clique sur PC2
2. Desktop → Command Prompt
3. Tape : `ping 192.168.20.11`
4. **Résultat attendu : Reply from... ✅**

Les 2 PCs sont dans le VLAN 20, ils peuvent communiquer.

---

### Récap des tests

```
✅ PC du même VLAN → Peuvent communiquer
❌ PC de VLANs différents → NE peuvent PAS communiquer

C'est exactement le comportement voulu !
```

**Question :** Comment faire communiquer les VLANs entre eux ?

**Réponse :** Il faut un **ROUTEUR** ! On verra ça dans le cours suivant (routing inter-VLAN).

---

## Configuration avancée : Router-on-a-Stick (bonus)

Si tu veux faire communiquer les VLANs entre eux, tu dois ajouter un **routeur**.

### Topologie

```
     PC-Compta    PC-RH    PC-Dir
         │          │         │
     ┌───┴──────────┴─────────┴───┐
     │        SWITCH               │
     │                             │
     │  VLAN 10, 20, 30            │
     └──────────┬──────────────────┘
                │ (TRUNK)
         ┌──────▼──────┐
         │   ROUTEUR   │
         │             │
         │ Sous-ifs    │
         │ .10, .20, .30│
         └─────────────┘
```

### Configuration du trunk sur le switch

```cisco
! Port vers le routeur = TRUNK (pour passer tous les VLANs)
Switch(config)# interface fastEthernet 0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

### Configuration du routeur (sous-interfaces)

```cisco
Router> enable
Router# configure terminal

! Sous-interface pour VLAN 10
Router(config)# interface fastEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

! Sous-interface pour VLAN 20
Router(config)# interface fastEthernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! Sous-interface pour VLAN 30
Router(config)# interface fastEthernet 0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit

! Activer l'interface physique
Router(config)# interface fastEthernet 0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Explications :**
- `fastEthernet 0/0.10` → Sous-interface pour VLAN 10
- `encapsulation dot1Q 10` → Protocole 802.1Q avec tag VLAN 10
- `ip address 192.168.10.1 255.255.255.0` → IP de la passerelle

**Maintenant, les VLANs peuvent communiquer entre eux via le routeur !**

Test :
```
PC0 (VLAN 10) ping PC2 (VLAN 20)
→ ✅ Ça marche !
```

---

## Exercices progressifs

### Exercice 1 (FACILE) - Comprendre les VLANs

Tu as un switch avec 12 ports. Tu veux créer 3 VLANs :
- VLAN 5 : Production (4 PCs)
- VLAN 10 : Bureau (6 PCs)
- VLAN 15 : Serveurs (2 serveurs)

**Questions :**
1. Quels ports tu assignes à chaque VLAN ?
2. Écris les commandes pour créer le VLAN 5

<details>
<summary>📖 Voir la solution</summary>

**Réponse 1 : Répartition des ports**

```
VLAN 5 (Production) : Ports Fa0/1 à Fa0/4
VLAN 10 (Bureau) : Ports Fa0/5 à Fa0/10
VLAN 15 (Serveurs) : Ports Fa0/11 à Fa0/12
```

**Réponse 2 : Commandes pour VLAN 5**

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# vlan 5
Switch(config-vlan)# name PRODUCTION
Switch(config-vlan)# exit
Switch(config)# interface range fastEthernet 0/1-4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 5
Switch(config-if-range)# exit
Switch(config)# exit
Switch# write memory
```

</details>

---

### Exercice 2 (MOYEN) - TP complet

Crée ce réseau dans Packet Tracer :

```
Entreprise TechCorp
─────────────────────
3 départements :
- IT : 3 PCs (VLAN 100, réseau 10.0.100.0/24)
- Ventes : 4 PCs (VLAN 200, réseau 10.0.200.0/24)
- Admin : 2 PCs (VLAN 300, réseau 10.0.300.0/24)

Matériel :
- 1 switch 2960
- 9 PCs
```

**Tâches :**
1. Crée la topologie
2. Configure les VLANs sur le switch
3. Configure les IPs sur les PCs
4. Teste : PCs du même VLAN doivent communiquer
5. Teste : PCs de VLANs différents NE doivent PAS communiquer

<details>
<summary>📖 Voir la solution complète</summary>

**Configuration switch :**

```cisco
Switch> enable
Switch# configure terminal

! Créer les VLANs
Switch(config)# vlan 100
Switch(config-vlan)# name IT
Switch(config-vlan)# exit

Switch(config)# vlan 200
Switch(config-vlan)# name VENTES
Switch(config-vlan)# exit

Switch(config)# vlan 300
Switch(config-vlan)# name ADMIN
Switch(config-vlan)# exit

! Assigner les ports
Switch(config)# interface range fa0/1-3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 100
Switch(config-if-range)# exit

Switch(config)# interface range fa0/4-7
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 200
Switch(config-if-range)# exit

Switch(config)# interface range fa0/8-9
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 300
Switch(config-if-range)# exit

Switch(config)# exit
Switch# write memory
```

**Configuration PCs :**

```
IT (VLAN 100) :
PC0 : 10.0.100.10 / 255.255.255.0 / GW 10.0.100.1
PC1 : 10.0.100.11 / 255.255.255.0 / GW 10.0.100.1
PC2 : 10.0.100.12 / 255.255.255.0 / GW 10.0.100.1

Ventes (VLAN 200) :
PC3 : 10.0.200.10 / 255.255.255.0 / GW 10.0.200.1
PC4 : 10.0.200.11 / 255.255.255.0 / GW 10.0.200.1
PC5 : 10.0.200.12 / 255.255.255.0 / GW 10.0.200.1
PC6 : 10.0.200.13 / 255.255.255.0 / GW 10.0.200.1

Admin (VLAN 300) :
PC7 : 10.0.300.10 / 255.255.255.0 / GW 10.0.300.1
PC8 : 10.0.300.11 / 255.255.255.0 / GW 10.0.300.1
```

**Tests :**

```
PC0 ping PC1 → ✅ OK (même VLAN 100)
PC0 ping PC3 → ❌ Timeout (VLANs différents)
PC3 ping PC4 → ✅ OK (même VLAN 200)
PC7 ping PC8 → ✅ OK (même VLAN 300)
```

</details>

---

### Exercice 3 (AVANCÉ) - Routing inter-VLAN

Reprends l'exercice 2 et **ajoute un routeur** pour faire communiquer les VLANs entre eux.

**Tâches :**
1. Ajoute un routeur 1841
2. Configure le port trunk sur le switch
3. Configure les sous-interfaces sur le routeur
4. Teste : Tous les PCs doivent pouvoir communiquer

<details>
<summary>📖 Voir la solution</summary>

**Switch (port trunk) :**

```cisco
Switch(config)# interface fa0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

**Routeur (sous-interfaces) :**

```cisco
Router> enable
Router# configure terminal

Router(config)# interface fa0/0.100
Router(config-subif)# encapsulation dot1Q 100
Router(config-subif)# ip address 10.0.100.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0.200
Router(config-subif)# encapsulation dot1Q 200
Router(config-subif)# ip address 10.0.200.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0.300
Router(config-subif)# encapsulation dot1Q 300
Router(config-subif)# ip address 10.0.300.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Tests :**

```
PC0 (IT) ping PC3 (Ventes) → ✅ OK
PC3 (Ventes) ping PC7 (Admin) → ✅ OK
PC7 (Admin) ping PC0 (IT) → ✅ OK
```

**BRAVO ! Tu maîtrises les VLANs et le routing inter-VLAN !**

</details>

---

## Récapitulatif - Ce que tu as appris

✅ **VLAN** = réseau virtuel sur un switch (cloisons invisibles)

✅ **Pourquoi ?**
  - Sécurité (isolation)
  - Organisation (séparation par service)
  - Performance (moins de broadcast)

✅ **2 types de ports** :
  - ACCESS (pour PCs) → 1 seul VLAN
  - TRUNK (pour switchs/routeurs) → tous les VLANs

✅ **Commandes Cisco essentielles** :
  - `vlan X` → créer un VLAN
  - `switchport mode access` → port ACCESS
  - `switchport access vlan X` → assigner au VLAN X
  - `switchport mode trunk` → port TRUNK
  - `show vlan brief` → voir les VLANs

✅ **Routing inter-VLAN** :
  - Sous-interfaces (Fa0/0.10, Fa0/0.20...)
  - Encapsulation dot1Q
  - IP de la passerelle

---

## Prochaine étape

Maintenant que tu comprends les VLANs, on va voir le **DHCP** (distribution automatique d'IPs).

**Conseils avant de continuer :**
- Refais les exercices jusqu'à ce que ce soit fluide
- Entraîne-toi à créer des VLANs dans Packet Tracer
- Essaie de créer 5, 10 VLANs
- Configure le routing inter-VLAN

**Tu es maintenant capable de sécuriser et d'organiser un réseau ! 💪**
