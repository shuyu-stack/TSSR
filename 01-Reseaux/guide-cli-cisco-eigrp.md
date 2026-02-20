# Guide CLI Cisco - Commandes & Protocoles de Routage

> Guide pour Packet Tracer - Formation TSSR

---

## Sommaire

### Partie 1 : Commandes de base
1. [Navigation dans le CLI](#1-navigation-dans-le-cli)
2. [Raccourcis indispensables](#2-raccourcis-indispensables)
3. [Configuration initiale du routeur/switch](#3-configuration-initiale)
4. [Gestion des interfaces](#4-gestion-des-interfaces)
5. [Sauvegarde et effacement](#5-sauvegarde-et-effacement)
6. [Commandes de vérification](#6-commandes-de-vérification)

### Partie 2 : Les Protocoles
7. [VLANs - Réseaux virtuels](#7-vlans---réseaux-virtuels)
8. [Routage Inter-VLAN](#8-routage-inter-vlan)
9. [DHCP Multi-réseaux](#9-dhcp-multi-réseaux)
10. [RIP - Le protocole des ancêtres](#10-rip---le-protocole-des-ancêtres)
11. [EIGRP - Le rapide de Cisco](#11-eigrp---le-rapide-de-cisco)
12. [OSPF - Le standard universel](#12-ospf---le-standard-universel)
13. [HSRP - La haute disponibilité](#13-hsrp---la-haute-disponibilité)

### Annexes
- [Tableau des raccourcis](#tableau-des-raccourcis)
- [Wildcard Mask](#wildcard-mask)
- [Distance Administrative](#distance-administrative)
- [Dépannage](#dépannage)

---

# PARTIE 1 : COMMANDES DE BASE

---

## 1. Navigation dans le CLI

### Les 4 modes du CLI

Imagine le CLI comme un immeuble avec 4 étages. Plus tu montes, plus tu as de pouvoir.

```
┌─────────────────────────────────────────────────────────────┐
│  ÉTAGE 4 : Mode Interface                                   │
│  Router(config-if)#                                         │
│  → Tu configures UNE interface spécifique                   │
├─────────────────────────────────────────────────────────────┤
│  ÉTAGE 3 : Mode Configuration Globale                       │
│  Router(config)#                                            │
│  → Tu configures le routeur entier                          │
├─────────────────────────────────────────────────────────────┤
│  ÉTAGE 2 : Mode Privilégié (admin)                          │
│  Router#                                                    │
│  → Tu peux tout voir et exécuter des commandes              │
├─────────────────────────────────────────────────────────────┤
│  ÉTAGE 1 : Mode Utilisateur (invité)                        │
│  Router>                                                    │
│  → Tu peux juste regarder, pas toucher                      │
└─────────────────────────────────────────────────────────────┘
```

### Comment naviguer entre les modes

```cisco
Router>                           ! Tu arrives ici par défaut
Router> enable                    ! Monte à l'étage 2
Router#                           ! Mode privilégié
Router# configure terminal        ! Monte à l'étage 3
Router(config)#                   ! Mode config globale
Router(config)# interface g0/0    ! Monte à l'étage 4
Router(config-if)#                ! Mode interface
```

**Pour redescendre :**
```cisco
Router(config-if)# exit           ! Descend d'un étage
Router(config)# exit              ! Descend encore
Router# disable                   ! Retour mode utilisateur
Router>
```

**Raccourci pour redescendre direct au mode privilégié :**
```cisco
Router(config-if)# end            ! Direct à Router#
! ou
Router(config-if)# Ctrl+Z         ! Pareil
```

---

## 2. Raccourcis indispensables

### Raccourcis de commandes

| Commande complète | Raccourci | Description |
|-------------------|-----------|-------------|
| `enable` | `en` | Passer admin |
| `disable` | `dis` | Revenir utilisateur |
| `configure terminal` | `conf t` | Mode configuration |
| `interface GigabitEthernet0/0` | `int g0/0` | Aller sur une interface |
| `interface FastEthernet0/1` | `int f0/1` | Interface Fast Ethernet |
| `interface range` | `int range` | Plusieurs interfaces |
| `no shutdown` | `no shut` | Allumer l'interface |
| `shutdown` | `shut` | Éteindre l'interface |
| `show running-config` | `sh run` | Voir la config active |
| `show startup-config` | `sh start` | Voir la config sauvegardée |
| `show ip interface brief` | `sh ip int br` | Résumé des interfaces |
| `show ip route` | `sh ip ro` | Table de routage |
| `write memory` | `wr` | Sauvegarder |
| `copy running-config startup-config` | `copy run start` | Sauvegarder (autre méthode) |
| `hostname` | `host` | Nommer l'équipement |
| `no ip domain-lookup` | `no ip domain-lo` | Désactiver DNS lookup |

### Raccourcis clavier

| Touche | Action |
|--------|--------|
| `Tab` | Auto-complétion (tape `conf` puis Tab → `configure`) |
| `?` | Aide contextuelle (tape `show ?` pour voir les options) |
| `↑` / `↓` | Historique des commandes |
| `Ctrl+A` | Aller au début de la ligne |
| `Ctrl+E` | Aller à la fin de la ligne |
| `Ctrl+Z` | Retour direct mode privilégié |
| `Ctrl+C` | Annuler la commande en cours |
| `Ctrl+Shift+6` | Interrompre un ping/traceroute |

### Astuce : Désactiver les messages qui interrompent

```cisco
Router(config)# line console 0
Router(config-line)# logging synchronous    ! Les messages ne coupent plus ta frappe
Router(config-line)# exit
```

---

## 3. Configuration initiale

### Nommer un équipement (hostname)

> La première chose à faire sur un routeur/switch : lui donner un nom pour le reconnaître !

**Sur un Routeur :**
```cisco
Router> en
Router# conf t
Router(config)# hostname R1
R1(config)#
```

**Sur un Switch :**
```cisco
Switch> en
Switch# conf t
Switch(config)# hostname SW1
SW1(config)#
```

| Commande | Raccourci | Exemple |
|----------|-----------|---------|
| `hostname NOM` | `host NOM` | `host R1`, `host SW-PARIS` |

> **Astuce :** Utilise des noms explicites : `R1-SIEGE`, `SW-ETAGE2`, `R-WAN-PARIS`

### Désactiver la recherche DNS (évite les blocages)

Quand tu tapes une commande incorrecte, le routeur essaie de la résoudre en DNS et bloque pendant 30 secondes. Pour éviter ça :

```cisco
R1(config)# no ip domain-lookup
```
**Raccourci :** `no ip domain-lo`

### Mettre un mot de passe

**Mot de passe mode privilégié (chiffré) :**
```cisco
R1(config)# enable secret MonMotDePasse
```

**Mot de passe console :**
```cisco
R1(config)# line console 0
R1(config-line)# password console123
R1(config-line)# login
R1(config-line)# exit
```

**Mot de passe Telnet/SSH :**
```cisco
R1(config)# line vty 0 4
R1(config-line)# password telnet123
R1(config-line)# login
R1(config-line)# exit
```

### Bannière de connexion

```cisco
R1(config)# banner motd #Acces interdit aux personnes non autorisees#
```

### Configuration initiale complète (à copier-coller)

```cisco
en
conf t
hostname R1
no ip domain-lookup
enable secret admin123
line console 0
 logging synchronous
 password console123
 login
exit
line vty 0 4
 password telnet123
 login
exit
banner motd #Acces reserve - TSSR#
end
wr
```

---

## 4. Gestion des interfaces

### Types d'interfaces courantes

| Interface | Raccourci | Usage |
|-----------|-----------|-------|
| `GigabitEthernet0/0` | `g0/0` | Connexion rapide (1 Gbps) |
| `FastEthernet0/1` | `f0/1` | Connexion standard (100 Mbps) |
| `Serial0/0/0` | `s0/0/0` | Liaison WAN série |
| `Loopback0` | `lo0` | Interface virtuelle |
| `Vlan1` | `vlan1` | Interface de gestion switch |

### Configurer une interface

```cisco
R1(config)# int g0/0
R1(config-if)# description Lien vers Switch1
R1(config-if)# ip address 192.168.1.254 255.255.255.0
R1(config-if)# no shut
R1(config-if)# exit
```

**Version courte :**
```cisco
R1(config)# int g0/0
R1(config-if)# ip add 192.168.1.254 255.255.255.0
R1(config-if)# no shut
```

### Configurer plusieurs interfaces en même temps

```cisco
R1(config)# int range g0/0-2
R1(config-if-range)# no shut
R1(config-if-range)# exit
```

```cisco
! Interfaces non consécutives
R1(config)# int range f0/1, f0/5, f0/10
R1(config-if-range)# switchport mode access
R1(config-if-range)# exit
```

### Supprimer la configuration d'une interface

```cisco
R1(config)# int g0/0
R1(config-if)# no ip address
R1(config-if)# shut
R1(config-if)# exit
```

### Interface Loopback (interface virtuelle)

> Une loopback c'est une interface "fictive" qui est toujours UP. Très utile pour tester ou comme Router-ID.

```cisco
R1(config)# int loopback 0
R1(config-if)# ip add 1.1.1.1 255.255.255.255
R1(config-if)# exit
```

---

## 5. Sauvegarde et effacement

### Sauvegarder la configuration

```cisco
R1# write memory
! ou
R1# wr
! ou
R1# copy running-config startup-config
```

> **running-config** = configuration en mémoire (perdue si on éteint)
> **startup-config** = configuration sur le disque (chargée au démarrage)

### Effacer la configuration

**Effacement complet (reset usine) :**
```cisco
R1# write erase
R1# reload
```

**Effacer seulement la startup-config :**
```cisco
R1# erase startup-config
R1# reload
```

### Annuler une configuration spécifique

> Règle d'or : mets `no` devant n'importe quelle commande pour l'annuler

```cisco
! Supprimer un hostname
R1(config)# no hostname
Router(config)#

! Supprimer un protocole de routage
R1(config)# no router eigrp 1

! Supprimer une route statique
R1(config)# no ip route 10.0.0.0 255.0.0.0 192.168.1.1
```

---

## 6. Commandes de vérification

### Les "show" essentiels

```cisco
! Résumé des interfaces (LE PLUS UTILISÉ)
R1# sh ip int br

! Table de routage
R1# sh ip ro

! Configuration actuelle complète
R1# sh run

! Filtrer la config (section spécifique)
R1# sh run | section interface
R1# sh run | section eigrp
R1# sh run | include network

! Voir une interface en détail
R1# sh int g0/0

! Voir les protocoles de routage actifs
R1# sh ip protocols

! Voir les voisins CDP (équipements Cisco connectés)
R1# sh cdp neighbors
```

### Lecture du "show ip interface brief"

```
Interface           IP-Address      OK? Method Status    Protocol
GigabitEthernet0/0  192.168.1.254   YES manual up        up
GigabitEthernet0/1  unassigned      YES unset  administratively down down
GigabitEthernet0/2  10.0.0.1        YES manual up        down
```

| Status | Protocol | Signification | Solution |
|--------|----------|---------------|----------|
| up | up | Tout va bien | Rien |
| administratively down | down | Interface éteinte volontairement | `no shut` |
| up | down | Problème de câble/voisin | Vérifier le câble |
| down | down | Pas de signal | Vérifier connexion physique |

### Tests de connectivité

```cisco
! Ping basique
R1# ping 192.168.1.1

! Ping étendu (choisir source, nombre, taille)
R1# ping
Protocol [ip]:
Target IP address: 192.168.1.1
Repeat count [5]: 10
Datagram size [100]: 1500
...

! Traceroute
R1# traceroute 8.8.8.8
```

---

# PARTIE 2 : LES PROTOCOLES

---

## 7. VLANs - Réseaux virtuels

### C'est quoi un VLAN ?

> **Analogie :** Imagine un open space avec 50 personnes. Tout le monde entend tout le monde, c'est le bazar. Les VLANs, c'est comme mettre des cloisons pour créer des bureaux séparés. Chaque bureau (VLAN) a ses propres conversations, isolées des autres.

**Sans VLAN :** Tous les PC sont dans le même réseau, ils se voient tous.

**Avec VLAN :** Tu crées des groupes logiques. Le VLAN 10 (Comptabilité) ne voit pas le VLAN 20 (RH), même s'ils sont sur le même switch physique.

### Créer des VLANs (sur un switch)

```cisco
Switch> en
Switch# conf t
Switch(config)# vlan 10
Switch(config-vlan)# name COMPTABILITE
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name RH
Switch(config-vlan)# exit
Switch(config)# vlan 99
Switch(config-vlan)# name MANAGEMENT
Switch(config-vlan)# exit
```

### Affecter des ports aux VLANs (mode Access)

> Un port en mode **Access** = pour connecter un PC, une imprimante (1 seul VLAN)

```cisco
Switch(config)# int f0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Plusieurs ports d'un coup
Switch(config)# int range f0/1-10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
```

### Configurer un port Trunk

> Un port en mode **Trunk** = pour connecter un switch à un autre switch ou à un routeur. Il laisse passer TOUS les VLANs.

```cisco
Switch(config)# int g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan all    ! Optionnel, c'est par défaut
Switch(config-if)# exit
```

**Limiter les VLANs sur un trunk :**
```cisco
Switch(config-if)# switchport trunk allowed vlan 10,20,99
```

### Vérification VLANs

```cisco
Switch# sh vlan brief                    ! Liste des VLANs et ports
Switch# sh int trunk                     ! Ports en trunk
Switch# sh int f0/1 switchport           ! Détail d'un port
```

---

## 8. Routage Inter-VLAN

### Pourquoi ?

> Les VLANs sont isolés par défaut. Pour que VLAN 10 parle à VLAN 20, il faut un routeur qui fait le lien. C'est le **routage inter-VLAN**.

### Méthode "Router-on-a-Stick"

> On utilise UN seul câble entre le switch et le routeur, mais on crée des **sous-interfaces** (une par VLAN).

**Côté Switch :**
```cisco
Switch(config)# int g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

**Côté Routeur :**
```cisco
R1(config)# int g0/0
R1(config-if)# no shut
R1(config-if)# exit

! Sous-interface pour VLAN 10
R1(config)# int g0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip add 192.168.10.254 255.255.255.0
R1(config-subif)# exit

! Sous-interface pour VLAN 20
R1(config)# int g0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip add 192.168.20.254 255.255.255.0
R1(config-subif)# exit
```

> **encapsulation dot1Q 10** = cette sous-interface gère le trafic tagué VLAN 10

### Vérification

```cisco
R1# sh ip int br
! Tu dois voir g0/0.10 et g0/0.20 avec leurs IP

R1# sh vlans          ! Voir les VLANs sur les sous-interfaces
```

---

## 9. DHCP Multi-réseaux

### C'est quoi le problème ?

> Le DHCP fonctionne par **broadcast**. Or, les broadcasts ne passent pas les routeurs. Donc si ton serveur DHCP est dans le VLAN 10, les PC du VLAN 20 ne le trouvent pas.

**Solution : IP Helper** - Le routeur "relaie" les requêtes DHCP vers le serveur.

### Configurer un serveur DHCP sur le routeur

```cisco
R1(config)# ip dhcp pool VLAN10
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.254
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# exit

R1(config)# ip dhcp pool VLAN20
R1(dhcp-config)# network 192.168.20.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.20.254
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# exit

! Exclure les adresses réservées (gateway, serveurs)
R1(config)# ip dhcp excluded-address 192.168.10.250 192.168.10.254
R1(config)# ip dhcp excluded-address 192.168.20.250 192.168.20.254
```

### IP Helper (relais DHCP)

> Si le serveur DHCP est sur un autre réseau (ex: 192.168.100.10), configure le relais sur l'interface du routeur qui reçoit les requêtes des clients.

```cisco
R1(config)# int g0/0.10
R1(config-subif)# ip helper-address 192.168.100.10
R1(config-subif)# exit

R1(config)# int g0/0.20
R1(config-subif)# ip helper-address 192.168.100.10
R1(config-subif)# exit
```

### Vérification DHCP

```cisco
R1# sh ip dhcp binding              ! Voir les baux attribués
R1# sh ip dhcp pool                 ! Voir les pools
R1# sh ip dhcp conflict             ! Voir les conflits d'IP
```

---

## 10. RIP - Le protocole des ancêtres

### C'est quoi RIP ?

> **Analogie :** RIP c'est comme demander son chemin à chaque carrefour. "C'est encore loin ?" "Encore 3 rues." Il compte le nombre de sauts (routeurs traversés) sans se soucier de la vitesse de la route.

**RIP en bref :**
- **Type :** Distance Vector (vecteur de distance)
- **Métrique :** Nombre de sauts (max 15, 16 = infini = injoignable)
- **Mise à jour :** Toutes les 30 secondes (broadcast complet de la table)
- **Usage :** Obsolète, mais à connaître pour les vieux réseaux et les examens

### Pourquoi RIP est obsolète ?

1. Max 15 sauts = réseaux limités en taille
2. Convergence lente (peut prendre plusieurs minutes)
3. Utilise beaucoup de bande passante (broadcast toutes les 30s)
4. Ne prend pas en compte la vitesse des liens

### Configuration RIP v2

> Utilise **toujours RIPv2**. RIPv1 ne supporte pas les masques variables (VLSM).

```cisco
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# network 192.168.10.0
R1(config-router)# network 192.168.20.0
R1(config-router)# network 10.0.0.0
R1(config-router)# no auto-summary
R1(config-router)# passive-interface g0/2
R1(config-router)# exit
```

### Commandes expliquées

| Commande | Explication |
|----------|-------------|
| `router rip` | Active RIP |
| `version 2` | Utilise RIPv2 (supporte VLSM) |
| `network X.X.X.X` | Déclare un réseau à annoncer (réseau classful) |
| `no auto-summary` | Désactive le résumé automatique (TOUJOURS le faire) |
| `passive-interface g0/2` | N'envoie pas de mises à jour RIP sur cette interface |

### Vérification RIP

```cisco
R1# sh ip rip database
R1# sh ip protocols
R1# sh ip route rip
```

### Supprimer RIP

```cisco
R1(config)# no router rip
```

---

## 11. EIGRP - Le rapide de Cisco

### C'est quoi EIGRP ?

> **Analogie :** EIGRP c'est comme avoir un GPS intelligent. Il connaît tous les chemins possibles, calcule le meilleur selon plusieurs critères (distance, vitesse, embouteillages), et garde un chemin de secours au cas où.

**EIGRP en bref :**
- **Type :** Hybride (avancé) - le meilleur des deux mondes
- **Métrique :** Composite (bande passante + délai par défaut)
- **Convergence :** Ultra rapide (quelques secondes)
- **AS :** Tous les routeurs doivent avoir le même numéro d'AS
- **Propriétaire :** Cisco (mais maintenant ouvert)

### Pourquoi EIGRP est rapide ?

1. **DUAL Algorithm** : Calcule des routes de secours (Feasible Successor)
2. **Mises à jour partielles** : N'envoie que les changements, pas toute la table
3. **Triggered updates** : Mise à jour immédiate en cas de changement
4. **Hello packets** : Vérifie les voisins toutes les 5 secondes

### Configuration EIGRP

```cisco
R1(config)# router eigrp 1                          ! 1 = numéro AS
R1(config-router)# network 192.168.10.0 0.0.0.255   ! Avec wildcard mask
R1(config-router)# network 192.168.20.0 0.0.0.255
R1(config-router)# network 10.0.0.0 0.0.0.255
R1(config-router)# no auto-summary                   ! TOUJOURS
R1(config-router)# passive-interface g0/2            ! Vers les PC
R1(config-router)# exit
```

### C'est quoi le Wildcard Mask ?

> C'est l'inverse du masque de sous-réseau. Formule : **255 - valeur = wildcard**

| Masque | Calcul | Wildcard |
|--------|--------|----------|
| /24 = 255.255.255.0 | 255-255, 255-255, 255-255, 255-0 | 0.0.0.255 |
| /30 = 255.255.255.252 | 255-252 = 3 | 0.0.0.3 |
| /16 = 255.255.0.0 | | 0.0.255.255 |
| /25 = 255.255.255.128 | 255-128 = 127 | 0.0.0.127 |

### Commandes expliquées

| Commande | Explication |
|----------|-------------|
| `router eigrp 1` | Active EIGRP avec AS numéro 1 (doit être identique sur tous les routeurs) |
| `network X.X.X.X W.W.W.W` | Annonce ce réseau aux voisins (wildcard obligatoire) |
| `no auto-summary` | Empêche EIGRP de regrouper les réseaux (CRITIQUE) |
| `passive-interface` | L'interface reçoit mais n'envoie pas d'EIGRP |

### Vérification EIGRP

```cisco
R1# sh ip eigrp neighbors            ! Liste des voisins EIGRP
R1# sh ip eigrp topology             ! Routes + routes de secours
R1# sh ip eigrp interfaces           ! Interfaces participant à EIGRP
R1# sh ip route eigrp                ! Routes apprises par EIGRP
R1# sh ip protocols                  ! Résumé du protocole
```

### Lecture de la table de routage EIGRP

```
D    192.168.30.0/24 [90/2172416] via 10.0.0.2, GigabitEthernet0/1
```

| Élément | Signification |
|---------|---------------|
| `D` | Route EIGRP (D = DUAL) |
| `192.168.30.0/24` | Réseau de destination |
| `90` | Distance Administrative (confiance) |
| `2172416` | Métrique EIGRP |
| `via 10.0.0.2` | Prochain saut (next-hop) |
| `GigabitEthernet0/1` | Interface de sortie |

### Supprimer EIGRP

```cisco
R1(config)# no router eigrp 1
```

---

## 12. OSPF - Le standard universel

### C'est quoi OSPF ?

> **Analogie :** OSPF c'est comme avoir une carte complète de la ville. Chaque routeur construit sa propre carte en échangeant des infos avec ses voisins. Il calcule ensuite le meilleur chemin lui-même.

**OSPF en bref :**
- **Type :** Link-State (état de lien)
- **Métrique :** Coût (basé sur la bande passante)
- **Convergence :** Rapide
- **Standard :** Ouvert (RFC 2328) - fonctionne sur tout
- **Aires :** Organise le réseau en zones (Area 0 = backbone obligatoire)

### C'est quoi une Area ?

> Les **Aires** divisent un grand réseau en morceaux plus petits. L'**Area 0** (backbone) est obligatoire et toutes les autres aires doivent y être connectées.

```
        Area 1              Area 0              Area 2
      (Filiale)           (Backbone)          (Siège)
    ┌─────────┐         ┌─────────┐         ┌─────────┐
    │  R3  R4 │─────────│  R1  R2 │─────────│  R5  R6 │
    └─────────┘         └─────────┘         └─────────┘
```

### Configuration OSPF

```cisco
R1(config)# router ospf 1                           ! 1 = Process ID (local)
R1(config-router)# router-id 1.1.1.1                ! Identifiant unique
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# network 192.168.20.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.0.0.255 area 0
R1(config-router)# passive-interface g0/2
R1(config-router)# exit
```

### Différences avec EIGRP

| Aspect | EIGRP | OSPF |
|--------|-------|------|
| Numéro après la commande | AS (doit être identique partout) | Process ID (peut être différent) |
| Network | `network X.X.X.X wildcard` | `network X.X.X.X wildcard area X` |
| Aires | Non | Oui (Area 0 obligatoire) |
| Router-ID | Optionnel | Fortement recommandé |

### Router-ID : c'est quoi ?

> C'est l'identité du routeur dans OSPF. Si tu ne le configures pas, OSPF prend la plus haute IP de loopback ou la plus haute IP d'interface.

**Bonne pratique :** Configure-le explicitement avec une loopback.

```cisco
R1(config)# int loopback 0
R1(config-if)# ip add 1.1.1.1 255.255.255.255
R1(config-if)# exit
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
```

### Vérification OSPF

```cisco
R1# sh ip ospf neighbor              ! Liste des voisins
R1# sh ip ospf interface             ! État des interfaces
R1# sh ip ospf database              ! Base de données OSPF
R1# sh ip route ospf                 ! Routes OSPF
R1# sh ip protocols                  ! Résumé
```

### Lecture de la table de routage OSPF

```
O    192.168.30.0/24 [110/2] via 10.0.0.2, GigabitEthernet0/1
```

| Code | Signification |
|------|---------------|
| `O` | Route OSPF intra-area |
| `O IA` | Route OSPF inter-area |
| `O E1/E2` | Route OSPF externe |
| `110` | Distance Administrative |
| `2` | Coût OSPF |

### Supprimer OSPF

```cisco
R1(config)# no router ospf 1
```

---

## 13. HSRP - La haute disponibilité

### C'est quoi HSRP ?

> **Analogie :** Imagine que tu as un chauffeur principal et un chauffeur de secours. Si le principal est malade, le remplaçant prend le volant automatiquement. Les passagers (PC) ne voient aucune différence, ils montent toujours dans la même voiture (IP virtuelle).

**HSRP en bref :**
- **But :** Redondance de passerelle par défaut
- **Principe :** Plusieurs routeurs partagent une IP virtuelle
- **Cisco :** Propriétaire (VRRP est l'équivalent standard)
- **Failover :** Automatique et transparent pour les clients

### Comment ça marche ?

```
         PC (gateway: 192.168.1.254)
                    │
          ┌─────────┴─────────┐
          │                   │
    ┌─────┴─────┐       ┌─────┴─────┐
    │    R1     │       │    R2     │
    │  ACTIVE   │       │  STANDBY  │
    │ .252      │       │ .253      │
    └───────────┘       └───────────┘
          │                   │
          └────── .254 ───────┘
              (IP virtuelle)
```

- Les PC ont comme gateway **192.168.1.254** (IP virtuelle)
- R1 (Active) répond aux requêtes ARP pour .254
- Si R1 tombe, R2 (Standby) prend le relais instantanément
- Les PC ne se rendent compte de rien

### Configuration HSRP

**Sur R1 (sera Active - priorité la plus haute) :**
```cisco
R1(config)# int g0/0
R1(config-if)# ip add 192.168.1.252 255.255.255.0
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 110
R1(config-if)# standby 1 preempt
R1(config-if)# no shut
R1(config-if)# exit
```

**Sur R2 (sera Standby) :**
```cisco
R2(config)# int g0/0
R2(config-if)# ip add 192.168.1.253 255.255.255.0
R2(config-if)# standby 1 ip 192.168.1.254
R2(config-if)# standby 1 priority 100
R2(config-if)# standby 1 preempt
R2(config-if)# no shut
R2(config-if)# exit
```

### Commandes expliquées

| Commande | Explication |
|----------|-------------|
| `standby 1 ip X.X.X.X` | IP virtuelle partagée (groupe 1) |
| `standby 1 priority 110` | Priorité (plus haute = Active). Par défaut : 100 |
| `standby 1 preempt` | Reprend le rôle Active si priorité supérieure |

### Vérification HSRP

```cisco
R1# sh standby
R1# sh standby brief
```

**Sortie typique :**
```
Interface   Grp  Pri P State   Active          Standby         Virtual IP
Gi0/0       1    110 P Active  local           192.168.1.253   192.168.1.254
```

### Test de failover

1. Depuis un PC, fais un `ping -t 192.168.1.254`
2. Éteins l'interface de R1 : `R1(config-if)# shut`
3. Observe : quelques pings perdus puis ça reprend
4. Rallume R1 : grâce à `preempt`, il reprend le rôle Active

---

# ANNEXES

---

## Tableau des raccourcis

| Commande complète | Raccourci |
|-------------------|-----------|
| `enable` | `en` |
| `configure terminal` | `conf t` |
| `interface GigabitEthernet0/0` | `int g0/0` |
| `interface FastEthernet0/1` | `int f0/1` |
| `interface range` | `int range` |
| `no shutdown` | `no shut` |
| `shutdown` | `shut` |
| `show running-config` | `sh run` |
| `show startup-config` | `sh start` |
| `show ip interface brief` | `sh ip int br` |
| `show ip route` | `sh ip ro` |
| `show vlan brief` | `sh vlan br` |
| `write memory` | `wr` |
| `copy running-config startup-config` | `copy run start` |
| `switchport mode access` | `sw mo acc` |
| `switchport mode trunk` | `sw mo tr` |
| `switchport access vlan` | `sw acc vlan` |

---

## Wildcard Mask

| CIDR | Masque | Wildcard |
|------|--------|----------|
| /8 | 255.0.0.0 | 0.255.255.255 |
| /16 | 255.255.0.0 | 0.0.255.255 |
| /24 | 255.255.255.0 | 0.0.0.255 |
| /25 | 255.255.255.128 | 0.0.0.127 |
| /26 | 255.255.255.192 | 0.0.0.63 |
| /27 | 255.255.255.224 | 0.0.0.31 |
| /28 | 255.255.255.240 | 0.0.0.15 |
| /29 | 255.255.255.248 | 0.0.0.7 |
| /30 | 255.255.255.252 | 0.0.0.3 |
| /32 | 255.255.255.255 | 0.0.0.0 |

**Formule :** `255 - valeur_masque = wildcard`

---

## Distance Administrative

> La **Distance Administrative (AD)** indique la "confiance" qu'on accorde à une source de route. Plus c'est bas, plus c'est fiable.

| Source | AD |
|--------|-----|
| Connecté | 0 |
| Statique | 1 |
| EIGRP (résumé) | 5 |
| BGP externe | 20 |
| EIGRP interne | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP externe | 170 |
| BGP interne | 200 |

> Si un même réseau est appris par EIGRP (90) et OSPF (110), EIGRP gagne.

---

## Dépannage

### Checklist rapide

```
□ Interfaces UP/UP ?          → sh ip int br
□ IP correctes ?              → sh ip int br
□ Même AS/Area ?              → sh ip protocols
□ Réseaux déclarés ?          → sh run | section router
□ Passive-interface correct ? → sh ip protocols
□ Voisins présents ?          → sh ip eigrp neighbors / sh ip ospf neighbor
□ Routes apprises ?           → sh ip route
□ Ping fonctionne ?           → ping X.X.X.X
```

### Problème : Pas de voisins EIGRP/OSPF

**Causes possibles :**
1. Interface down → `no shut`
2. AS différent (EIGRP) → Vérifier le numéro
3. Area différente (OSPF) → Vérifier l'area
4. Réseau non déclaré → Ajouter `network`
5. Passive-interface sur la mauvaise interface → Retirer

### Problème : Route non présente

**Vérifier :**
```cisco
sh ip route                ! La route existe ?
sh ip protocols            ! Le protocole tourne ?
sh run | section router    ! La config est bonne ?
```

### Codes de la table de routage

| Code | Signification |
|------|---------------|
| C | Connected (directement connecté) |
| L | Local (IP de l'interface) |
| S | Static (route statique) |
| R | RIP |
| D | EIGRP |
| O | OSPF |
| B | BGP |
| * | Route par défaut candidate |

---

**Version 2.0 - Janvier 2025**
**Formation TSSR - Nextformation**
