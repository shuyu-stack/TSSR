# QoS et VLANs VoIP - La qualité avant tout

> 📚 **Module :** Téléphonie VoIP - QoS et VLANs
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 3 heures
> 🎯 **Niveau :** Intermédiaire
> 👨‍🏫 **Approche :** Terrain + Pratique

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Pourquoi séparer voix et données ?](#-pourquoi-séparer-voix-et-données-)
- [Les VLANs voix](#-les-vlans-voix)
- [QoS : Les fondamentaux](#-qos--les-fondamentaux)
- [Configuration QoS Cisco](#-configuration-qos-cisco)
- [Calculs de bande passante](#-calculs-de-bande-passante)
- [TP Pratique](#-tp-pratique)
- [Dépannage QoS](#-dépannage-qos)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**2006 - Mon premier échec VoIP.**

J'ai déployé 150 téléphones IP Cisco pour un client. Configuration parfaite, téléphones enregistrés, tout fonctionne... **pendant 2 heures**.

À 10h00 (ouverture des bureaux), **catastrophe** :
- Voix hachée, robotique
- Coupures aléatoires
- Latence de 2 secondes

**Cause ?** Pas de **QoS**. Les téléphones partageaient la bande passante avec :
- 200 PC qui téléchargeaient les emails du matin
- Les sauvegardes qui tournaient encore
- Les mises à jour Windows
- Les utilisateurs qui regardaient YouTube (oui, même en 2006)

**Solution ?** 3 jours de config QoS + VLANs voix sur **42 switchs**.

**La leçon :** **Sans QoS, votre VoIP est morte.**

### 🎯 Ma promesse

À la fin de ces 3 heures, vous saurez :
- ✅ Pourquoi la QoS est **critique** en VoIP
- ✅ Configurer un VLAN voix Cisco
- ✅ Mettre en place une QoS complète (switchs + routeurs)
- ✅ Calculer la bande passante nécessaire
- ✅ Diagnostiquer les problèmes de qualité vocale

**La QoS, c'est 50% de la réussite d'un projet VoIP !** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** pourquoi séparer voix et données (VLANs)
- ✅ **Configurer** un VLAN voix sur un switch Cisco
- ✅ **Comprendre** CoS, DSCP, et le marquage de paquets
- ✅ **Implémenter** une QoS complète (classification, marquage, queuing)
- ✅ **Calculer** la bande passante pour N appels simultanés
- ✅ **Diagnostiquer** les problèmes de qualité (jitter, perte, latence)

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir suivi **01-fondamentaux-voip.md** et **02-protocoles-voip.md**
- [ ] Connaître les **VLANs** (création, trunk)
- [ ] Comprendre le **modèle OSI** (L2/L3)
- [ ] Maîtriser les bases de **Cisco IOS**

**Matériel nécessaire :**
- 💻 PC avec Packet Tracer
- 📝 De quoi prendre des notes

---

## 🚦 Pourquoi séparer voix et données ?

### Le problème : Le trafic mélangé

Imaginez une route où circulent :
- 🚗 Des voitures (données)
- 🚑 Des ambulances (voix VoIP)

**Sans séparation :**
```
┌─────────────────────────────────────────────────────────────┐
│  RÉSEAU SANS VLAN VOIX                                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🚗💾📧🎵📂 🚑 📹🖼️📊 🚑 🎮📁💿 🚑 📝🔊               │
│                                                             │
│  Voix mélangée avec :                                       │
│    • Téléchargements (saturent la BP)                      │
│    • Sauvegardes (énormes fichiers)                        │
│    • Streaming vidéo (Netflix, YouTube)                    │
│    • Jeux en ligne (latence imprévisible)                  │
│                                                             │
│  Résultat : Voix DÉGRADÉE                                   │
└─────────────────────────────────────────────────────────────┘
```

**Avec séparation (VLAN voix) :**
```
┌─────────────────────────────────────────────────────────────┐
│  VLAN VOIX (Prioritaire)                                    │
│  🚑 🚑 🚑 🚑 🚑   ← Voie rapide, jamais encombrée           │
├─────────────────────────────────────────────────────────────┤
│  VLAN DATA (Normal)                                         │
│  🚗💾📧📹🖼️📊🎮📁💿📝🔊   ← Peut être encombré             │
└─────────────────────────────────────────────────────────────┘
```

### Les 3 avantages de la séparation

```
┌─────────────────────────────────────────────────────────────┐
│  1. SÉCURITÉ                                                │
├─────────────────────────────────────────────────────────────┤
│  • Isolation voix/données                                   │
│  • Prévention écoute clandestine depuis le VLAN data       │
│  • ACL spécifiques sur VLAN voix                           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  2. QUALITÉ (QoS)                                           │
├─────────────────────────────────────────────────────────────┤
│  • Priorisation du trafic voix                             │
│  • Bande passante garantie                                  │
│  • Réduction jitter et latence                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  3. ADMINISTRATION                                          │
├─────────────────────────────────────────────────────────────┤
│  • DHCP séparé (scope voix ≠ scope data)                   │
│  • Monitoring dédié                                         │
│  • Troubleshooting facilité                                │
└─────────────────────────────────────────────────────────────┘
```

### Mon retour d'expérience

**Projet 2013 - Banque 500 utilisateurs :**

```
AVANT (VLAN unique) :
┌──────────────────────────────────────────────────────────┐
│  Plaintes quotidiennes :                                 │
│    • "La voix est hachée le matin"                       │
│    • "J'ai des coupures lors des appels clients"        │
│    • "Impossible de comprendre mon interlocuteur"       │
│                                                          │
│  Cause : Sauvegardes nocturnes qui finissent à 9h30     │
│  → Saturation du réseau entre 9h et 10h                 │
└──────────────────────────────────────────────────────────┘

APRÈS (VLAN voix + QoS) :
┌──────────────────────────────────────────────────────────┐
│  0 plainte en 3 ans                                      │
│  Qualité vocale constante (MOS 4.2)                     │
│  Même pendant les sauvegardes                           │
└──────────────────────────────────────────────────────────┘

Temps de mise en place : 2 jours (42 switchs)
ROI : Immédiat (satisfaction utilisateurs)
```

---

## 📶 Les VLANs voix

### Principe du VLAN voix Cisco

**Architecture typique :**

```
┌───────────────────────────────────────────────────────────┐
│  PORT SWITCH (ex: Fa0/1)                                  │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  ┌─────────────────────────┐  ┌─────────────────────┐    │
│  │  VLAN 10 (Voix)         │  │  VLAN 1 (Data)      │    │
│  │  Tag 802.1Q : 10        │  │  Untagged           │    │
│  │  CoS : 5                │  │  CoS : 0            │    │
│  └──────────┬──────────────┘  └──────────┬──────────┘    │
│             │                            │               │
│             │                            │               │
│        ┌────▼────┐                  ┌────▼────┐          │
│        │Téléphone│                  │   PC    │          │
│        │   IP    │──────────────────│ (derrière│         │
│        │         │  Port PC phone   │  phone)  │         │
│        └─────────┘                  └──────────┘          │
└───────────────────────────────────────────────────────────┘
```

**Points clés :**
- Le téléphone **tag** ses trames en VLAN 10 (802.1Q)
- Le PC derrière le phone reste en VLAN 1 (untagged)
- Le switch **marque** automatiquement le trafic voix avec CoS 5

### Plan d'adressage typique

| VLAN | Nom | Réseau | Usage |
|------|-----|--------|-------|
| **1** | DATA | 192.168.1.0/24 | PCs, serveurs, imprimantes |
| **10** | VOICE | 192.168.10.0/24 | Téléphones IP |
| **99** | MGMT | 192.168.99.0/24 | Management switchs |

### Configuration VLAN voix sur switch

#### Étape 1 : Créer le VLAN voix

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-Access-01
SW-Access-01(config)#

! Créer le VLAN voix
SW-Access-01(config)# vlan 10
SW-Access-01(config-vlan)# name VOICE
SW-Access-01(config-vlan)# exit

! Créer le VLAN data (si pas déjà fait)
SW-Access-01(config)# vlan 1
SW-Access-01(config-vlan)# name DATA
SW-Access-01(config-vlan)# exit
```

#### Étape 2 : Configurer les ports d'accès (téléphones)

```cisco
! Configuration d'un port avec téléphone + PC
SW-Access-01(config)# interface FastEthernet 0/1
SW-Access-01(config-if)# description Poste Alice - Phone + PC
SW-Access-01(config-if)#
SW-Access-01(config-if)# switchport mode access
SW-Access-01(config-if)# switchport access vlan 1
! VLAN 1 = Data (pour le PC derrière le phone)
SW-Access-01(config-if)#
SW-Access-01(config-if)# switchport voice vlan 10
! VLAN 10 = Voice (pour le téléphone)
SW-Access-01(config-if)#
SW-Access-01(config-if)# spanning-tree portfast
! PortFast pour éviter les délais STP
SW-Access-01(config-if)# spanning-tree bpduguard enable
! Protection contre les boucles
SW-Access-01(config-if)# exit

! Appliquer à plusieurs ports en même temps
SW-Access-01(config)# interface range FastEthernet 0/1 - 24
SW-Access-01(config-if-range)# switchport mode access
SW-Access-01(config-if-range)# switchport access vlan 1
SW-Access-01(config-if-range)# switchport voice vlan 10
SW-Access-01(config-if-range)# spanning-tree portfast
SW-Access-01(config-if-range)# spanning-tree bpduguard enable
SW-Access-01(config-if-range)# exit
```

**Explication :**
- `switchport access vlan 1` : Le PC derrière le phone sera en VLAN 1
- `switchport voice vlan 10` : Le téléphone IP sera en VLAN 10
- `spanning-tree portfast` : Accélère la mise en ligne du port
- `bpduguard` : Protège contre les boucles si quelqu'un branche un switch

#### Étape 3 : Configurer le trunk (vers routeur/core)

```cisco
SW-Access-01(config)# interface GigabitEthernet 0/1
SW-Access-01(config-if)# description Trunk vers Core
SW-Access-01(config-if)# switchport trunk encapsulation dot1q
SW-Access-01(config-if)# switchport mode trunk
SW-Access-01(config-if)# switchport trunk allowed vlan 1,10
! Autorise seulement VLANs 1 et 10 (sécurité)
SW-Access-01(config-if)# exit
```

#### Étape 4 : Vérification

```cisco
! Vérifier les VLANs
SW-Access-01# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- ------------------------
1    DATA                             active    Fa0/1, Fa0/2, ...
10   VOICE                            active
99   MGMT                             active

! Vérifier un port d'accès
SW-Access-01# show interfaces FastEthernet 0/1 switchport

Name: Fa0/1
Switchport: Enabled
Administrative Mode: access
Operational Mode: access
Administrative Trunking Encapsulation: dot1q
Negotiation of Trunking: Off
Access Mode VLAN: 1 (DATA)
Voice VLAN: 10 (VOICE)   ← OK !
...

! Vérifier le trunk
SW-Access-01# show interfaces trunk

Port        Mode         Encapsulation  Status        Native vlan
Gi0/1       on           802.1q         trunking      1

Port        Vlans allowed on trunk
Gi0/1       1,10

Port        Vlans allowed and active in management domain
Gi0/1       1,10
```

### Mon piège classique

**Oublier le native VLAN sur le trunk :**

```cisco
! MAUVAIS (Native VLAN différent des 2 côtés)
Switch-A(config-if)# switchport trunk native vlan 1
Switch-B(config-if)# switchport trunk native vlan 99
→ CDP Native VLAN Mismatch errors !

! BON (Même native VLAN partout)
Switch-A(config-if)# switchport trunk native vlan 1
Switch-B(config-if)# switchport trunk native vlan 1
```

---

## 🎯 QoS : Les fondamentaux

### Qu'est-ce que la QoS ?

**QoS** = **Quality of Service** = Qualité de Service

**Définition simple :** Donner la **priorité** à certains flux (voix) sur d'autres (données).

### Les 3 mécanismes QoS

```
┌─────────────────────────────────────────────────────────────┐
│  1. CLASSIFICATION                                          │
├─────────────────────────────────────────────────────────────┤
│  Identifier le type de trafic                               │
│                                                             │
│  Méthodes :                                                 │
│    • Port source/dest (UDP 16384-32767 = RTP)              │
│    • Protocole (SIP, RTP)                                   │
│    • DSCP (valeur déjà marquée)                            │
│    • VLAN (VLAN 10 = voix)                                  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  2. MARQUAGE                                                │
├─────────────────────────────────────────────────────────────┤
│  Attribuer une priorité au trafic                           │
│                                                             │
│  Méthodes :                                                 │
│    • CoS (L2 - 0 à 7) : sur trame Ethernet 802.1Q           │
│    • DSCP (L3 - 0 à 63) : dans en-tête IP                   │
│                                                             │
│  Valeurs recommandées Cisco :                               │
│    • Voix (RTP)         : DSCP 46 (EF)                      │
│    • Signalisation (SIP): DSCP 26 (AF31)                   │
│    • Données            : DSCP 0 (Best Effort)              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  3. QUEUING / SCHEDULING                                    │
├─────────────────────────────────────────────────────────────┤
│  Gérer les files d'attente                                  │
│                                                             │
│  Méthodes :                                                 │
│    • Priority Queue : Voix en premier (toujours)            │
│    • CBWFQ : Bande passante garantie par classe             │
│    • LLQ : Low Latency Queue (voix)                         │
│    • WRED : Gestion de congestion                           │
└─────────────────────────────────────────────────────────────┘
```

### CoS vs DSCP

#### CoS (Class of Service) - Couche 2

```
Trame Ethernet 802.1Q :
┌────────────────────────────────────────────────────────────┐
│  Destination MAC | Source MAC | 802.1Q Tag | Type | Data   │
├────────────────────────────────────────────────────────────┤
│                            802.1Q Tag (4 octets)           │
│  ┌──────────────────────────────────────────────────┐      │
│  │ TPID (2) | PCP (3 bits) | DEI (1) | VID (12)    │      │
│  │          │    CoS       │         │    VLAN     │      │
│  └──────────────────────────────────────────────────┘      │
│                                                            │
│  CoS (PCP) : 3 bits = 0 à 7                                │
│    0 = Best Effort (données)                               │
│    5 = Voix (RTP)                                          │
│    6 = Contrôle réseau                                     │
│    7 = Réservé                                             │
└────────────────────────────────────────────────────────────┘
```

**Limite :** CoS est **perdu** dès qu'on route (passage L3). Valable uniquement en L2 (switch).

#### DSCP (Differentiated Services Code Point) - Couche 3

```
En-tête IP :
┌────────────────────────────────────────────────────────────┐
│  Version | IHL | DSCP (6 bits) | ECN (2) | Total Length   │
├────────────────────────────────────────────────────────────┤
│            DSCP : 6 bits = 0 à 63                          │
│              (anciennement ToS)                            │
│                                                            │
│  Valeurs standard :                                        │
│    DSCP 46 (EF) = Expedited Forwarding = Voix              │
│    DSCP 26 (AF31) = Assured Forwarding = Signalisation    │
│    DSCP 0 (BE) = Best Effort = Données normales           │
└────────────────────────────────────────────────────────────┘
```

**Avantage :** DSCP est **conservé** même en routant (L3). Valable de bout en bout.

### Tableau CoS / DSCP

| Trafic | CoS (L2) | DSCP (L3) | Nom DSCP | Usage |
|--------|----------|-----------|----------|-------|
| **Voix (RTP)** | 5 | 46 | EF (Expedited Forwarding) | Flux RTP |
| **Signalisation VoIP** | 3 | 26 | AF31 | SIP, SCCP |
| **Vidéo** | 4 | 34 | AF41 | Visioconférence |
| **Données critiques** | 2 | 18 | AF21 | ERP, CRM |
| **Données normales** | 0 | 0 | BE (Best Effort) | Web, email |
| **Contrôle réseau** | 6 | 48 | CS6 | OSPF, BGP |

### Mon analogie préférée

```
AÉROPORT = RÉSEAU
────────────────────────────────

CoS / DSCP = Classe de billet
┌──────────────────────────────────────────────────────┐
│  DSCP 46 (Voix)       = Première Classe              │
│    → Embarquement prioritaire, siège confortable     │
│                                                      │
│  DSCP 26 (Signal)     = Business                     │
│    → Embarquement priorité moyenne                  │
│                                                      │
│  DSCP 0 (Données)     = Économique                   │
│    → On attend son tour                             │
└──────────────────────────────────────────────────────┘

Queuing = Files d'attente
┌──────────────────────────────────────────────────────┐
│  File Première (DSCP 46) → Passe TOUJOURS en premier│
│  File Business (DSCP 26) → Passe après Première     │
│  File Éco (DSCP 0)       → Passe quand il reste de  │
│                            la place                  │
└──────────────────────────────────────────────────────┘
```

---

## ⚙️ Configuration QoS Cisco

### QoS sur Switch (AutoQoS)

**AutoQoS** = Configuration QoS automatique Cisco (recommandée pour débutants).

#### Configuration AutoQoS

```cisco
! Activer AutoQoS globalement
SW-Access-01(config)# mls qos

! Configurer AutoQoS sur les ports avec téléphones
SW-Access-01(config)# interface range FastEthernet 0/1 - 24
SW-Access-01(config-if-range)# auto qos voip cisco-phone
! Cisco-phone = Le switch fait confiance au téléphone pour le marquage
SW-Access-01(config-if-range)# exit

! Configurer AutoQoS sur les trunks
SW-Access-01(config)# interface GigabitEthernet 0/1
SW-Access-01(config-if)# auto qos voip trust
! Trust = Le switch fait confiance aux marquages déjà présents
SW-Access-01(config-if)# exit
```

**Ce que fait AutoQoS :**
- Active `mls qos` globalement
- Configure les queues (files d'attente)
- Applique le marquage CoS/DSCP automatiquement
- Prioritise la voix (CoS 5 / DSCP 46)

#### Configuration QoS manuelle (avancé)

```cisco
! Activer QoS
SW-Access-01(config)# mls qos

! Définir les mappages CoS → DSCP
SW-Access-01(config)# mls qos map cos-dscp 0 8 16 26 34 46 48 56
!                                CoS: 0 1 2  3  4  5  6  7
!                               DSCP: 0 8 16 26 34 46 48 56
! CoS 5 → DSCP 46 (voix)

! Configurer un port d'accès
SW-Access-01(config)# interface FastEthernet 0/1
SW-Access-01(config-if)# mls qos trust cos
! Trust CoS = Faire confiance au marquage CoS du téléphone
SW-Access-01(config-if)# mls qos cos 0
! CoS par défaut 0 si pas de tag (PC)
SW-Access-01(config-if)# exit

! Configurer un trunk
SW-Access-01(config)# interface GigabitEthernet 0/1
SW-Access-01(config-if)# mls qos trust dscp
! Trust DSCP = Faire confiance au DSCP déjà marqué
SW-Access-01(config-if)# exit
```

#### Vérification QoS switch

```cisco
! Vérifier que QoS est activé
SW-Access-01# show mls qos
QoS is enabled   ← OK !

! Vérifier la config d'un port
SW-Access-01# show mls qos interface FastEthernet 0/1
FastEthernet0/1
trust state: trust cos
trust mode: trust cos
COS override: dis
default COS: 0
...

! Vérifier les statistiques
SW-Access-01# show mls qos interface FastEthernet 0/1 statistics
  dscp: incoming
  -----------------------------------------------
  0 -  4 :      123456        0        0        0        0
  ...
  45 - 49:           0    98765        0        0        0
                           ↑ DSCP 46 (voix) avec beaucoup de paquets
```

### QoS sur Routeur (Policy-Map)

**Objectif :** Prioriser la voix sur les liens WAN (faible bande passante).

#### Configuration QoS complète (LLQ)

```cisco
! === ÉTAPE 1 : CLASS-MAPS (Classification) ===
Router(config)# class-map match-any VOICE-RTP
Router(config-cmap)# match ip dscp ef
! EF = DSCP 46 (voix RTP)
Router(config-cmap)# exit

Router(config)# class-map match-any VOICE-SIGNAL
Router(config-cmap)# match ip dscp af31
! AF31 = DSCP 26 (signalisation SIP/SCCP)
Router(config-cmap)# exit

! === ÉTAPE 2 : POLICY-MAP (Marquage + Queuing) ===
Router(config)# policy-map WAN-QOS
Router(config-pmap)# class VOICE-RTP
Router(config-pmap-c)# priority percent 33
! Priority = File prioritaire (Low Latency Queue)
! 33% = Maximum 33% de la BP pour la voix
Router(config-pmap-c)# exit

Router(config-pmap)# class VOICE-SIGNAL
Router(config-pmap-c)# bandwidth percent 5
! Bande passante garantie de 5% pour signalisation
Router(config-pmap-c)# exit

Router(config-pmap)# class class-default
Router(config-pmap-c)# fair-queue
! Reste du trafic (données) = Best Effort
Router(config-pmap-c)# exit
Router(config-pmap)# exit

! === ÉTAPE 3 : APPLICATION (Service Policy) ===
Router(config)# interface Serial 0/0/0
Router(config-if)# description Lien WAN 2 Mbps
Router(config-if)# bandwidth 2000
! Déclare la BP réelle (2 Mbps) pour calculs QoS
Router(config-if)# service-policy output WAN-QOS
! Applique la QoS en SORTIE (output)
Router(config-if)# exit
```

**Explications :**
- `priority percent 33` : Voix = 33% max du lien (660 Kbps sur 2 Mbps)
- `bandwidth percent 5` : Signalisation = 5% garanti (100 Kbps)
- `class-default` : Reste = Best Effort (1240 Kbps pour données)

#### Vérification QoS routeur

```cisco
! Vérifier les class-maps
Router# show class-map

Class Map match-any VOICE-RTP (id 1)
   Match ip dscp ef (46)

Class Map match-any VOICE-SIGNAL (id 2)
   Match ip dscp af31 (26)

! Vérifier les policy-maps
Router# show policy-map WAN-QOS

Policy Map WAN-QOS
  Class VOICE-RTP
    priority 33% (660 kbps)
  Class VOICE-SIGNAL
    bandwidth 5% (100 kbps)
  Class class-default
    fair-queue

! Vérifier l'application sur interface
Router# show policy-map interface Serial 0/0/0

Serial0/0/0
  Service-policy output: WAN-QOS

    Class-map: VOICE-RTP (match-any)
      Queueing
      (priority)
      Output Queue: Conversation 256
      Bandwidth 33% (660 kbps)   ← OK
      (pkts matched/bytes matched) 45678/6543210

    Class-map: VOICE-SIGNAL (match-any)
      Queueing
      Output Queue: Conversation 265
      Bandwidth 5% (100 kbps)
      (pkts matched/bytes matched) 1234/123456

    Class-map: class-default (match-any)
      Flow-based Fair Queueing
      (pkts matched/bytes matched) 987654/98765432
```

### Mon retour d'expérience QoS

**2015 - Site distant 50 users, lien 2 Mbps :**

```
SANS QOS :
┌────────────────────────────────────────────────┐
│  08h00 - 09h00 : Qualité OK (peu de trafic)   │
│  09h00 - 10h30 : Qualité MÉDIOCRE (emails)    │
│  10h30 - 12h00 : Qualité OK                   │
│  14h00 - 15h00 : Qualité CATASTROPHIQUE       │
│                  (sauvegardes + navigation)   │
└────────────────────────────────────────────────┘

AVEC QOS (LLQ + 33% voix) :
┌────────────────────────────────────────────────┐
│  Qualité CONSTANTE 24h/24                     │
│  MOS moyen : 4.1 (excellent)                  │
│  0 plainte en 18 mois                         │
└────────────────────────────────────────────────┘

Temps config QoS : 1 heure (1 routeur + 2 switchs)
```

---

## 📊 Calculs de bande passante

### Formule de calcul par appel

```
BANDE PASSANTE PAR APPEL = Codec + En-têtes

En-têtes (Ethernet + IP + UDP + RTP) :
  Ethernet : 18 octets
  IP       : 20 octets
  UDP      : 8 octets
  RTP      : 12 octets
  ─────────────────────
  Total    : 58 octets par paquet

Exemples :

G.711 (20 ms par paquet) :
  Payload  : 160 octets
  En-têtes : 58 octets
  Total    : 218 octets
  Débit    : 218 × 8 × 50 = 87 200 bps = 87.2 Kbps

G.729 (20 ms par paquet) :
  Payload  : 20 octets
  En-têtes : 58 octets
  Total    : 78 octets
  Débit    : 78 × 8 × 50 = 31 200 bps = 31.2 Kbps
```

### Calcul pour un site

**Méthode :**

```
1. Nombre d'utilisateurs
2. Taux d'occupation téléphonique (15-25% en moyenne)
3. Appels simultanés max = Users × Taux
4. Bande passante = Appels × Débit codec
5. Ajouter 20-30% de marge
```

**Exemple concret :**

```
Site distant : 60 utilisateurs
Taux d'occupation : 20%
Codec : G.729 (31.2 Kbps/appel)

Calcul :
  Appels simultanés max = 60 × 20% = 12 appels
  BP voix = 12 × 31.2 Kbps = 374.4 Kbps
  BP voix + marge 30% = 374.4 × 1.30 = 486.7 Kbps

  BP signalisation (SIP) ≈ 5% voix = 24 Kbps
  ─────────────────────────────────────────────
  BP totale VoIP = 511 Kbps

Lien WAN recommandé :
  VoIP : 511 Kbps (33% de priorité)
  Données : 1000 Kbps (reste)
  ─────────────────────────────────
  Total : 1511 Kbps → Lien 2 Mbps minimum
```

### Tableau récapitulatif

| Codec | Kbps/appel | 10 appels | 20 appels | 50 appels | 100 appels |
|-------|------------|-----------|-----------|-----------|------------|
| **G.711** | 87 Kbps | 870 Kbps | 1.7 Mbps | 4.3 Mbps | 8.7 Mbps |
| **G.729** | 31 Kbps | 310 Kbps | 620 Kbps | 1.5 Mbps | 3.1 Mbps |
| **G.722** | 87 Kbps | 870 Kbps | 1.7 Mbps | 4.3 Mbps | 8.7 Mbps |
| **Opus** | 24-90 Kbps | Variable | Variable | Variable | Variable |

### Mon conseil dimensionnement

```
┌──────────────────────────────────────────────────────────┐
│  RÈGLES D'OR POUR DIMENSIONNEMENT                        │
├──────────────────────────────────────────────────────────┤
│  1. LAN (réseau local)                                   │
│     → G.711 (qualité max)                                │
│     → Pas de contrainte BP (Gigabit)                     │
│                                                          │
│  2. WAN < 2 Mbps                                         │
│     → G.729 (économie BP)                                │
│     → QoS OBLIGATOIRE                                    │
│                                                          │
│  3. WAN > 5 Mbps                                         │
│     → G.711 (qualité max)                                │
│     → QoS recommandée                                    │
│                                                          │
│  4. Toujours ajouter 20-30% marge                        │
│     → Pour pics d'appels (Noël, Black Friday, etc.)     │
│                                                          │
│  5. Priorité voix : 33% max du lien                      │
│     → Évite que la voix affame les données              │
└──────────────────────────────────────────────────────────┘
```

---

## 🎯 TP Pratique

### Objectif

Configurer un réseau VoIP complet avec VLANs et QoS pour 2 sites interconnectés.

### Topologie

```
Site A (Siège)                          Site B (Agence)
───────────────                         ────────────────

┌─────────────┐                         ┌─────────────┐
│   Router    │                         │   Router    │
│   CME-A     │◄─────WAN 2 Mbps────────►│   CME-B     │
│192.168.1.1  │   (Serial 0/0/0)        │192.168.2.1  │
└──────┬──────┘                         └──────┬──────┘
       │ Gi0/0                                 │ Gi0/0
       │                                       │
┌──────┴──────┐                         ┌──────┴──────┐
│   Switch    │                         │   Switch    │
│   SW-A      │                         │   SW-B      │
└─┬─────────┬─┘                         └─┬─────────┬─┘
  │         │                             │         │
┌─▼──┐   ┌──▼─┐                        ┌─▼──┐   ┌──▼─┐
│Tel │   │Tel │                        │Tel │   │Tel │
│2001│   │2002│                        │2101│   │2102│
└────┘   └────┘                        └────┘   └────┘

VLANs :
• VLAN 1 (DATA) : 192.168.X.0/24
• VLAN 10 (VOICE) : 192.168.1X.0/24
```

### Configuration pas à pas

#### Site A - Routeur CME-A

```cisco
! === CONFIGURATION DE BASE ===
Router> enable
Router# configure terminal
Router(config)# hostname CME-A
CME-A(config)#

! Interface LAN
CME-A(config)# interface GigabitEthernet 0/0
CME-A(config-if)# no shutdown
CME-A(config-if)# exit

! Sous-interface VLAN DATA
CME-A(config)# interface GigabitEthernet 0/0.1
CME-A(config-subif)# encapsulation dot1Q 1 native
CME-A(config-subif)# ip address 192.168.1.1 255.255.255.0
CME-A(config-subif)# exit

! Sous-interface VLAN VOICE
CME-A(config)# interface GigabitEthernet 0/0.10
CME-A(config-subif)# encapsulation dot1Q 10
CME-A(config-subif)# ip address 192.168.10.1 255.255.255.0
CME-A(config-subif)# exit

! Interface WAN
CME-A(config)# interface Serial 0/0/0
CME-A(config-if)# description Lien WAN vers Site B
CME-A(config-if)# ip address 10.0.0.1 255.255.255.252
CME-A(config-if)# bandwidth 2000
CME-A(config-if)# clock rate 2000000
CME-A(config-if)# no shutdown
CME-A(config-if)# exit

! === DHCP ===
CME-A(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

CME-A(config)# ip dhcp pool VOICE-A
CME-A(dhcp-config)# network 192.168.10.0 255.255.255.0
CME-A(dhcp-config)# default-router 192.168.10.1
CME-A(dhcp-config)# option 150 ip 192.168.10.1
CME-A(dhcp-config)# exit

! === CME ===
CME-A(config)# telephony-service
CME-A(config-telephony)# max-ephones 10
CME-A(config-telephony)# max-dn 20
CME-A(config-telephony)# ip source-address 192.168.10.1 port 2000
CME-A(config-telephony)# create cnf-files
CME-A(config-telephony)# exit

! ephone-dn
CME-A(config)# ephone-dn 1 dual-line
CME-A(config-ephone-dn)# number 2001
CME-A(config-ephone-dn)# name Alice Site A
CME-A(config-ephone-dn)# exit

CME-A(config)# ephone-dn 2 dual-line
CME-A(config-ephone-dn)# number 2002
CME-A(config-ephone-dn)# name Bob Site A
CME-A(config-ephone-dn)# exit

! === QoS ===
! Class-maps
CME-A(config)# class-map match-any VOICE-RTP
CME-A(config-cmap)# match ip dscp ef
CME-A(config-cmap)# exit

CME-A(config)# class-map match-any VOICE-SIGNAL
CME-A(config-cmap)# match ip dscp af31
CME-A(config-cmap)# exit

! Policy-map
CME-A(config)# policy-map WAN-QOS
CME-A(config-pmap)# class VOICE-RTP
CME-A(config-pmap-c)# priority percent 33
CME-A(config-pmap-c)# exit
CME-A(config-pmap)# class VOICE-SIGNAL
CME-A(config-pmap-c)# bandwidth percent 5
CME-A(config-pmap-c)# exit
CME-A(config-pmap)# class class-default
CME-A(config-pmap-c)# fair-queue
CME-A(config-pmap-c)# exit
CME-A(config-pmap)# exit

! Application
CME-A(config)# interface Serial 0/0/0
CME-A(config-if)# service-policy output WAN-QOS
CME-A(config-if)# exit

CME-A(config)# exit
CME-A# write memory
```

#### Site A - Switch SW-A

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-A
SW-A(config)#

! VLANs
SW-A(config)# vlan 1
SW-A(config-vlan)# name DATA
SW-A(config-vlan)# exit

SW-A(config)# vlan 10
SW-A(config-vlan)# name VOICE
SW-A(config-vlan)# exit

! QoS
SW-A(config)# mls qos

! Ports d'accès (téléphones)
SW-A(config)# interface range FastEthernet 0/1 - 10
SW-A(config-if-range)# switchport mode access
SW-A(config-if-range)# switchport access vlan 1
SW-A(config-if-range)# switchport voice vlan 10
SW-A(config-if-range)# auto qos voip cisco-phone
SW-A(config-if-range)# spanning-tree portfast
SW-A(config-if-range)# exit

! Trunk vers routeur
SW-A(config)# interface GigabitEthernet 0/1
SW-A(config-if)# switchport trunk encapsulation dot1q
SW-A(config-if)# switchport mode trunk
SW-A(config-if)# switchport trunk allowed vlan 1,10
SW-A(config-if)# auto qos voip trust
SW-A(config-if)# exit

SW-A(config)# exit
SW-A# write memory
```

#### Site B - Configuration similaire

(Adapter avec 192.168.2.x / 192.168.20.x et numéros 2101, 2102)

### Tests à effectuer

```
✅ Test 1 : Vérifier les VLANs
   SW-A# show vlan brief
   → VLAN 1 et 10 doivent exister

✅ Test 2 : Vérifier le trunk
   SW-A# show interfaces trunk
   → VLANs 1,10 autorisés

✅ Test 3 : Vérifier la QoS switch
   SW-A# show mls qos
   → QoS is enabled

✅ Test 4 : Vérifier la QoS routeur
   CME-A# show policy-map interface Serial 0/0/0
   → Policy WAN-QOS appliquée

✅ Test 5 : Enregistrement téléphones
   CME-A# show ephone registered
   → Téléphones en REGISTERED

✅ Test 6 : Appel interne
   2001 appelle 2002
   → Communication OK

✅ Test 7 : Appel inter-sites
   2001 appelle 2101
   → Communication OK via WAN

✅ Test 8 : Qualité vocale
   Pendant l'appel, générer du trafic data
   → La voix ne doit PAS être affectée (grâce à QoS)
```

---

## 🔍 Dépannage QoS

### Problème 1 : Qualité vocale dégradée

**Symptômes :**
- Voix hachée, robotique
- Coupures
- Écho

**Diagnostic :**

```cisco
! 1. Vérifier que QoS est activée
Switch# show mls qos
QoS is enabled   ← Doit être "enabled"

Router# show policy-map interface Serial 0/0/0
Service-policy output: WAN-QOS   ← Doit être appliquée

! 2. Vérifier les statistiques de drops (pertes)
Router# show policy-map interface Serial 0/0/0

Class-map: VOICE-RTP
  (pkts matched/bytes matched) 123456/12345678
  (total drops/bytes drops) 0/0   ← Doit être 0 !

Si drops > 0 : BP insuffisante ou QoS mal configurée

! 3. Vérifier la latence/jitter avec debug
Router# debug voice rtp session named-event
! Pendant un appel, observer les stats RTP
! Désactiver après : no debug all
```

**Solutions :**

```
❌ Drops > 0 en classe VOICE-RTP
   → Augmenter "priority percent" ou BP du lien

❌ QoS désactivée sur switch
   → Activer "mls qos"

❌ Pas de trust sur ports téléphones
   → Ajouter "auto qos voip cisco-phone"

❌ Pas de service-policy sur WAN
   → Appliquer la policy-map en output
```

### Problème 2 : VLAN voix non fonctionnel

**Symptômes :**
- Téléphones obtiennent IP du VLAN data
- Pas de séparation voix/données

**Diagnostic :**

```cisco
Switch# show interfaces FastEthernet 0/1 switchport

Access Mode VLAN: 1 (DATA)
Voice VLAN: none   ← PROBLÈME ! Devrait être 10
```

**Solutions :**

```cisco
! Configurer le VLAN voix sur le port
Switch(config)# interface FastEthernet 0/1
Switch(config-if)# switchport voice vlan 10
Switch(config-if)# exit

! Recréer les fichiers CME
Router(config)# telephony-service
Router(config-telephony)# create cnf-files
Router(config-telephony)# exit

! Redémarrer le téléphone
```

### Commandes de diagnostic QoS

```cisco
! === SWITCH ===
show mls qos                          # QoS activée ?
show mls qos interface Fa0/1          # Config QoS du port
show mls qos interface Fa0/1 statistics # Stats QoS

! === ROUTEUR ===
show class-map                        # Class-maps configurées
show policy-map                       # Policy-maps configurées
show policy-map interface Se0/0/0     # QoS appliquée + stats

! === APPELS ===
show call active voice brief          # Appels en cours
show voice call summary               # Résumé appels

! === DEBUG (avec précaution) ===
debug voice rtp session named-event   # Stats RTP temps réel
no debug all                          # STOP debugs
```

---

## 📚 Ressources

### Documentation officielle

- [Cisco QoS Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/qos/configuration/15-mt/qos-15-mt-book.html)
- [AutoQoS VoIP](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst6500/ios/12-2SX/configuration/guide/book/autoqos.html)
- [Voice VLAN Configuration](https://www.cisco.com/c/en/us/support/docs/unified-communications/unified-communications-manager-callmanager/15383-voice-vlan.html)

### Outils

- **Wireshark** : Analyser DSCP/CoS dans les paquets
- **PRTG** : Monitoring bande passante
- **SolarWinds VoIP Monitor** : Monitoring qualité VoIP

### Calculateurs

- [VoIP Bandwidth Calculator](http://www.bandcalc.com/)
- [Erlang Calculator](http://www.erlang.com/calculator/)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Mes questions :**
-
-
-

**Points à approfondir :**
-
-

**Projets à tester :**
-
-

---

## ✅ Checklist de révision

Avant de passer au cours suivant, assurez-vous de maîtriser :

- [ ] Je comprends pourquoi séparer voix et données (VLANs)
- [ ] Je sais créer et configurer un VLAN voix
- [ ] Je connais la différence entre CoS (L2) et DSCP (L3)
- [ ] Je sais les valeurs CoS/DSCP pour la voix (5/46)
- [ ] Je peux configurer AutoQoS sur un switch
- [ ] Je sais créer des class-maps et policy-maps
- [ ] Je peux appliquer une QoS (LLQ) sur un routeur
- [ ] Je sais calculer la bande passante pour N appels
- [ ] Je peux diagnostiquer un problème de qualité vocale
- [ ] Je maîtrise les commandes de vérification QoS

---

<div align="center">

**Cours précédent :** [03-configuration-cme-packet-tracer.md](03-configuration-cme-packet-tracer.md)

**Cours suivant :** [05-securite-voip.md](05-securite-voip.md)

[⬅️ Retour au sommaire](README.md)

</div>
