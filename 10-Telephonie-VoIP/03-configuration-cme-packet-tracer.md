# Configuration Cisco CME - TP Packet Tracer

> 📚 **Module :** Téléphonie VoIP - Configuration CME
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 6 heures
> 🎯 **Niveau :** Intermédiaire
> 👨‍🏫 **Approche :** 70% pratique, 30% théorie

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Qu'est-ce que CME ?](#-quest-ce-que-cme-)
- [Architecture du lab](#-architecture-du-lab)
- [Configuration de base](#-configuration-de-base)
- [DHCP avec option 150](#-dhcp-avec-option-150)
- [Configuration des téléphones IP](#-configuration-des-téléphones-ip)
- [Plan de numérotation](#-plan-de-numérotation)
- [Fonctionnalités avancées](#-fonctionnalités-avancées)
- [TP complet guidé](#-tp-complet-guidé)
- [Dépannage courant](#-dépannage-courant)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

Ma **première config CME en 2007** m'a pris **8 heures**. Pourquoi ? Parce que je ne comprenais **rien** à la logique Cisco VoIP.

**Les pièges que j'ai rencontrés :**
- DHCP sans option 150 → téléphones bloqués sur "Configuring IP"
- TFTP pas activé → téléphones en erreur
- Mauvais fichier de config → téléphones redémarrent en boucle
- Dial-peer mal configuré → impossible d'appeler

**Aujourd'hui, je configure un CME complet en 20 minutes.**

**La différence ?** J'ai compris la **logique** :
1. Le téléphone se branche → DHCP
2. Il reçoit l'IP du serveur TFTP → Option 150
3. Il télécharge sa config → TFTP
4. Il s'enregistre → CME
5. Il peut appeler → Dial-peer

### 🎯 Ma promesse

À la fin de ces 6 heures, vous saurez :
- ✅ Configurer un CME complet de A à Z
- ✅ Créer et gérer des téléphones IP
- ✅ Mettre en place un plan de numérotation
- ✅ Ajouter messagerie vocale, transfert, conférence
- ✅ **Diagnostiquer** 90% des problèmes VoIP Cisco

**On va beaucoup pratiquer, préparez-vous !** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** le rôle de CME (Call Manager Express)
- ✅ **Configurer** un routeur CME complet
- ✅ **Créer** des téléphones IP (ephone, ephone-dn)
- ✅ **Mettre en place** le DHCP avec option 150
- ✅ **Définir** un plan de numérotation logique
- ✅ **Implémenter** messagerie vocale, transfert, conférence
- ✅ **Dépanner** les problèmes courants (registration, appels, qualité)

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir suivi **01-fondamentaux-voip.md** et **02-protocoles-voip.md**
- [ ] Maîtriser la **configuration de base Cisco** (IP, interface)
- [ ] Connaître le **DHCP** (serveur, pools, options)
- [ ] Avoir **Cisco Packet Tracer** installé (8.0+)

**Matériel nécessaire :**
- 💻 PC avec Packet Tracer
- 📝 De quoi prendre des notes
- ⏱️ 6 heures de concentration

---

## 📱 Qu'est-ce que CME ?

### Définition

**CME** = **Call Manager Express**

C'est un **IP-PBX intégré** dans un routeur Cisco (ISR 2800, 2900, 4000 series).

**Analogie :** CME, c'est le "cerveau" de votre téléphonie, logé directement dans votre routeur.

### CME vs CUCM

| Critère | CME | CUCM |
|---------|-----|------|
| **Déploiement** | Routeur ISR | Serveur dédié |
| **Capacité** | Jusqu'à 450 phones | Milliers de phones |
| **Coût** | $ (inclus dans IOS) | $$$ (licences) |
| **Complexité** | ⚠️ Moyenne | ❌ Élevée |
| **Usage** | PME, sites distants | Entreprises, groupes |
| **Interface** | CLI + web basique | GUI complète |

### Architecture CME

```
┌─────────────────────────────────────────────────────────────┐
│  ROUTEUR CISCO CME (ex: 2911)                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │   IP BASE   │  │   CME IOS   │  │    TFTP     │         │
│  │   (routing) │  │ (téléphonie)│  │  (configs)  │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐                          │
│  │    DHCP     │  │   SCCP/SIP  │                          │
│  │  (option    │  │ (signalisation)│                        │
│  │   150)      │  └─────────────┘                          │
│  └─────────────┘                                            │
└─────────────────────────────────────────────────────────────┘
         │                    │                    │
         │                    │                    │
    ┌────▼────┐          ┌────▼────┐          ┌────▼────┐
    │Téléphone│          │Téléphone│          │Téléphone│
    │IP 7841  │          │IP 8841  │          │IP 7961  │
    └─────────┘          └─────────┘          └─────────┘
```

### Les 3 concepts clés CME

```
┌─────────────────────────────────────────────────────────────┐
│  1. EPHONE-DN (Directory Number)                            │
├─────────────────────────────────────────────────────────────┤
│  = Le NUMÉRO de téléphone (ex: 2001)                        │
│  = C'est la ligne, pas le téléphone physique               │
│                                                             │
│  Exemple :                                                  │
│    ephone-dn 1                                              │
│      number 2001                                            │
│      name "Alice Dupont"                                    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  2. EPHONE (Ethernet Phone)                                 │
├─────────────────────────────────────────────────────────────┤
│  = Le TÉLÉPHONE physique (adresse MAC)                     │
│  = Le matériel                                              │
│                                                             │
│  Exemple :                                                  │
│    ephone 1                                                 │
│      mac-address 0011.2233.4455                             │
│      button 1:1    ← Bouton 1 = ephone-dn 1                │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  3. TELEPHONY-SERVICE                                       │
├─────────────────────────────────────────────────────────────┤
│  = Configuration GLOBALE du système CME                     │
│  = Paramètres généraux (max phones, TFTP, codecs, etc.)    │
│                                                             │
│  Exemple :                                                  │
│    telephony-service                                        │
│      max-ephones 20                                         │
│      max-dn 50                                              │
│      ip source-address 192.168.10.1                         │
└─────────────────────────────────────────────────────────────┘
```

**Point clé :** Un **ephone** (téléphone physique) peut avoir **plusieurs ephone-dn** (lignes). Exemple : secrétaire avec 3 lignes.

---

## 🏗️ Architecture du lab

### Topologie Packet Tracer

Voici le lab que nous allons construire :

```
                    INTERNET
                       │
                       │ (simulation)
                       │
                  ┌────▼────┐
                  │ Router  │
                  │  CME    │ 192.168.10.1
                  │ (2911)  │
                  └────┬────┘
                       │ Gi0/0
                       │
                  ┌────▼────┐
                  │ Switch  │
                  │ 2960    │ (PoE simulé)
                  │         │
                  └─┬─┬─┬─┬─┘
                    │ │ │ │
         ┌──────────┘ │ │ └──────────┐
         │            │ │            │
    ┌────▼────┐  ┌────▼────┐  ┌──────▼───┐
    │Téléphone│  │Téléphone│  │    PC    │
    │  2001   │  │  2002   │  │Softphone │
    │  Alice  │  │   Bob   │  │  2003    │
    └─────────┘  └─────────┘  └──────────┘
```

### Plan d'adressage

| Équipement | Interface | IP | Rôle |
|------------|-----------|-----|------|
| **Router CME** | Gi0/0 | 192.168.10.1/24 | Passerelle, DHCP, TFTP, CME |
| **Switch** | VLAN 1 | 192.168.10.2/24 | Management |
| **Téléphone Alice** | Auto (DHCP) | 192.168.10.101 | Poste 2001 |
| **Téléphone Bob** | Auto (DHCP) | 192.168.10.102 | Poste 2002 |
| **PC Softphone** | Auto (DHCP) | 192.168.10.103 | Poste 2003 |

### Fichiers requis dans Packet Tracer

```
Packet Tracer intègre nativement :
✅ IOS CME (dans les routeurs 2811, 2901, 2911)
✅ Téléphones IP Cisco (7960, 7961, 7841, 8841)
✅ Firmwares téléphones

Rien à télécharger ! Tout est prêt.
```

---

## 🔧 Configuration de base

### Étape 1 : Configuration réseau du routeur

```cisco
! Nom du routeur
Router> enable
Router# configure terminal
Router(config)# hostname CME-Router
CME-Router(config)#

! Interface LAN (vers le switch)
CME-Router(config)# interface GigabitEthernet 0/0
CME-Router(config-if)# ip address 192.168.10.1 255.255.255.0
CME-Router(config-if)# no shutdown
CME-Router(config-if)# exit

! Vérification
CME-Router# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.10.1    YES manual up                    up
```

### Étape 2 : Activation du service CME

```cisco
! Entrer en mode configuration CME
CME-Router(config)# telephony-service
CME-Router(config-telephony)#

! Nombre max de téléphones et lignes
CME-Router(config-telephony)# max-ephones 20
CME-Router(config-telephony)# max-dn 50

! Adresse IP source pour CME (IP du routeur)
CME-Router(config-telephony)# ip source-address 192.168.10.1 port 2000

! Création automatique des fichiers de config
CME-Router(config-telephony)# create cnf-files
Creating CNF files... OK
CME-Router(config-telephony)# exit
```

**Explication :**
- `max-ephones 20` : Maximum 20 téléphones physiques
- `max-dn 50` : Maximum 50 numéros/lignes
- `ip source-address` : IP sur laquelle CME écoute (SCCP port 2000)
- `create cnf-files` : Génère les fichiers de config pour les téléphones

---

## 🌐 DHCP avec option 150

### Pourquoi l'option 150 ?

**Sans option 150 :**
```
Téléphone branche → DHCP → Reçoit IP
Téléphone : "OK, j'ai une IP... mais où est le serveur TFTP ?"
Téléphone : Bloqué sur "Configuring IP"
```

**Avec option 150 :**
```
Téléphone branche → DHCP → Reçoit IP + IP du TFTP (option 150)
Téléphone : "Super ! Je contacte le TFTP pour ma config"
Téléphone : Télécharge sa config et démarre
```

### Configuration DHCP complet

```cisco
CME-Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
! Exclut les IPs .1 à .10 (réservées pour équipements réseau)

CME-Router(config)# ip dhcp pool VOICE
CME-Router(dhcp-config)# network 192.168.10.0 255.255.255.0
CME-Router(dhcp-config)# default-router 192.168.10.1
CME-Router(dhcp-config)# option 150 ip 192.168.10.1
! Option 150 = IP du serveur TFTP (notre routeur CME)
CME-Router(dhcp-config)# exit

! Vérification
CME-Router# show ip dhcp pool

Pool VOICE :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 254
 Leased addresses               : 0
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased/Excluded/Total
 192.168.10.1         192.168.10.1     - 192.168.10.254   0    / 10    / 254
```

### Mon anecdote option 150

**2009 - Le téléphone fantôme :**

```
Problème : Téléphone bloqué sur "Configuring IP"

Diagnostic (1h de galère) :
✅ DHCP fonctionne (le phone a une IP)
✅ Ping OK depuis le phone
❌ Mais le phone ne va pas plus loin

Solution :
J'avais oublié l'option 150 dans le DHCP !
→ Le téléphone ne savait pas où trouver le TFTP

Ajout de "option 150 ip 192.168.10.1"
→ Téléphone opérationnel en 30 secondes

Leçon : L'option 150 est CRITIQUE.
```

---

## 📞 Configuration des téléphones IP

### La logique ephone-dn + ephone

**Rappel :**
- **ephone-dn** = Le numéro (ligne)
- **ephone** = Le téléphone physique

**Analogie :**
- ephone-dn = Ligne téléphonique (numéro)
- ephone = Le combiné (matériel)

### Créer un ephone-dn (numéro)

```cisco
! Création du numéro 2001
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# number 2001
CME-Router(config-ephone-dn)# name Alice Dupont
CME-Router(config-ephone-dn)# description Poste Accueil
CME-Router(config-ephone-dn)# exit

! Création du numéro 2002
CME-Router(config)# ephone-dn 2
CME-Router(config-ephone-dn)# number 2002
CME-Router(config-ephone-dn)# name Bob Martin
CME-Router(config-ephone-dn)# exit

! Création du numéro 2003
CME-Router(config)# ephone-dn 3
CME-Router(config-ephone-dn)# number 2003
CME-Router(config-ephone-dn)# name Service Info
CME-Router(config-ephone-dn)# exit
```

### Créer un ephone (téléphone physique)

```cisco
! Téléphone d'Alice
CME-Router(config)# ephone 1
CME-Router(config-ephone)# mac-address 0001.9641.D4A1
! (Remplacer par la vraie MAC du téléphone dans Packet Tracer)
CME-Router(config-ephone)# type 7960
CME-Router(config-ephone)# button 1:1
! Bouton 1 du phone = ephone-dn 1 (numéro 2001)
CME-Router(config-ephone)# exit

! Téléphone de Bob
CME-Router(config)# ephone 2
CME-Router(config-ephone)# mac-address 0001.9641.D4A2
CME-Router(config-ephone)# type 7960
CME-Router(config-ephone)# button 1:2
! Bouton 1 = ephone-dn 2 (numéro 2002)
CME-Router(config-ephone)# exit

! Recréation des fichiers de config
CME-Router(config)# telephony-service
CME-Router(config-telephony)# create cnf-files
Creating CNF files... OK
CME-Router(config-telephony)# exit

! Reset des téléphones pour qu'ils se réenregistrent
CME-Router# telephony-service
CME-Router(config-telephony)# reset all
Resetting all phones...
CME-Router(config-telephony)# exit
```

### Vérification de l'enregistrement

```cisco
CME-Router# show ephone registered

ephone-1[0] Mac:0001.9641.D4A1 TCP socket:[1] activeLine:0 REGISTERED in SCCP ver 12/9
mediaActive:0 offhook:0 ringing:0 reset:0 reset_sent:0 paging 0 debug:0 caps:8
IP:192.168.10.101 52918 7960  keepalive 2399 max_line 6
button 1: dn 1  number 2001 CH1   IDLE

ephone-2[1] Mac:0001.9641.D4A2 TCP socket:[2] activeLine:0 REGISTERED in SCCP ver 12/9
mediaActive:0 offhook:0 ringing:0 reset:0 reset_sent:0 paging 0 debug:0 caps:8
IP:192.168.10.102 52919 7960  keepalive 2801 max_line 6
button 1: dn 2  number 2002 CH1   IDLE
```

**Statut REGISTERED = téléphones opérationnels ! 🎉**

### Mon retour d'expérience

**Piège fréquent : La MAC address**

```
Erreur classique débutant :
┌──────────────────────────────────────────────────┐
│ ephone 1                                         │
│   mac-address 0011.2233.4455  ← MAC inventée    │
│   button 1:1                                     │
└──────────────────────────────────────────────────┘

Résultat : Le téléphone ne s'enregistre JAMAIS

Pourquoi ?
La MAC doit être celle du VRAI téléphone physique.

Dans Packet Tracer :
1. Cliquer sur le téléphone
2. Onglet "Config"
3. Noter la MAC address
4. L'utiliser dans "ephone"

Ou méthode automatique :
CME-Router# show ephone unregistered
→ Voit tous les téléphones non configurés avec leur MAC
```

---

## 🔢 Plan de numérotation

### Principe du plan de numérotation

**Plan de numérotation** = Règles d'appel (qui peut appeler qui, comment)

**Exemple concret :**
```
┌──────────────────────────────────────────────────────────┐
│  PLAN DE NUMÉROTATION ENTREPRISE                         │
├──────────────────────────────────────────────────────────┤
│  2xxx  : Postes internes (2001-2999)                     │
│  3xxx  : Services (3000=Accueil, 3100=Compta, etc.)      │
│  9      : Préfixe pour appels externes                   │
│  0      : Standard (opérateur)                           │
│  8xxx  : Conférences                                     │
│  5xxx  : Messagerie vocale                               │
└──────────────────────────────────────────────────────────┘
```

### Plan simple pour notre lab

```
┌──────────────────────────────────────────────────────────┐
│  NOTRE PLAN                                              │
├──────────────────────────────────────────────────────────┤
│  2001  : Alice (Accueil)                                 │
│  2002  : Bob (IT)                                        │
│  2003  : Service Info                                    │
│  2999  : Messagerie vocale                               │
│  9XXX  : Appels externes (simulation)                    │
└──────────────────────────────────────────────────────────┘
```

### Déjà fait !

Notre plan est déjà configuré via les `ephone-dn` :
```cisco
ephone-dn 1
  number 2001   ← Alice peut être appelée au 2001
ephone-dn 2
  number 2002   ← Bob peut être appelé au 2002
```

### Appel externe (simulation)

Pour simuler des appels externes (vers l'extérieur) :

```cisco
! Numéro pour appels sortants
CME-Router(config)# ephone-dn 10
CME-Router(config-ephone-dn)# number 9....
! 9 suivi de n'importe quel chiffre
CME-Router(config-ephone-dn)# description Appels externes
CME-Router(config-ephone-dn)# exit

! Dial-peer pour route externe (simulation)
CME-Router(config)# dial-peer voice 9 voip
CME-Router(config-dial-peer)# destination-pattern 9..........
! 9 + 10 chiffres (numéro externe)
CME-Router(config-dial-peer)# session target ipv4:192.168.100.1
! (IP d'un autre CME ou gateway PSTN - en simulation)
CME-Router(config-dial-peer)# codec g711ulaw
CME-Router(config-dial-peer)# exit
```

**Note :** Dans Packet Tracer, on peut simuler un deuxième site avec un autre routeur CME pour tester les appels inter-sites.

---

## 🚀 Fonctionnalités avancées

### 1. Messagerie vocale

**Fonctionnalité :** Laisser un message vocal quand le correspondant ne répond pas.

#### Configuration messagerie vocale

```cisco
! Activer la messagerie vocale
CME-Router(config)# telephony-service
CME-Router(config-telephony)# voicemail 2999
! 2999 = numéro pour accéder à la messagerie
CME-Router(config-telephony)# exit

! Créer le ephone-dn pour messagerie
CME-Router(config)# ephone-dn 99
CME-Router(config-ephone-dn)# number 2999
CME-Router(config-ephone-dn)# name Messagerie Vocale
CME-Router(config-ephone-dn)# exit

! Associer messagerie aux ephone-dn
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# call-forward busy 2999
CME-Router(config-ephone-dn)# call-forward noan 2999 timeout 20
! noan = No Answer (pas de réponse après 20 secondes)
CME-Router(config-ephone-dn)# exit

CME-Router(config)# ephone-dn 2
CME-Router(config-ephone-dn)# call-forward busy 2999
CME-Router(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-Router(config-ephone-dn)# exit
```

**Utilisation :**
```
Utilisateur appelle Alice (2001)
→ Alice ne répond pas après 20 secondes
→ Renvoi automatique vers 2999 (messagerie)
→ L'appelant peut laisser un message
```

### 2. Transfert d'appel

**Fonctionnalité :** Transférer un appel en cours vers un autre poste.

#### Configuration transfert

```cisco
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# transfer-system full-consult
! full-consult = l'utilisateur peut parler au destinataire avant transfert
CME-Router(config-ephone-dn)# exit

CME-Router(config)# telephony-service
CME-Router(config-telephony)# transfer-system full-consult
CME-Router(config-telephony)# exit
```

**Utilisation sur le téléphone :**
```
1. Alice en communication avec un client
2. Alice appuie sur "Transfer" (bouton du phone)
3. Alice compose 2002 (Bob)
4. Alice parle à Bob : "C'est pour toi"
5. Alice appuie sur "Transfer" à nouveau
6. Le client est transféré à Bob
```

### 3. Conférence à 3

**Fonctionnalité :** Appel à 3 personnes simultanément.

#### Configuration conférence

```cisco
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# allow-connections all to all
! Autorise les connexions entre tous
CME-Router(config-ephone-dn)# exit

CME-Router(config)# telephony-service
CME-Router(config-telephony)# max-conferences 8
! Maximum 8 conférences simultanées
CME-Router(config-telephony)# exit

! Créer des ephone-dn dual-line (pour conférences)
CME-Router(config)# ephone-dn 1 dual-line
! dual-line = 2 appels simultanés sur cette ligne
CME-Router(config-ephone-dn)# number 2001
CME-Router(config-ephone-dn)# name Alice Dupont
CME-Router(config-ephone-dn)# exit
```

**Utilisation :**
```
1. Alice en communication avec Bob (2002)
2. Alice appuie sur "ConfRn" (Conference)
3. Alice compose 2003 (Service Info)
4. Service Info décroche
5. Alice appuie sur "ConfRn" à nouveau
6. Les 3 personnes sont en conférence
```

### 4. Musique d'attente

**Fonctionnalité :** Musique pendant qu'un appelant attend.

#### Configuration (basique dans Packet Tracer)

```cisco
CME-Router(config)# telephony-service
CME-Router(config-telephony)# moh music-on-hold.au
! Fichier audio (simulé dans Packet Tracer)
CME-Router(config-telephony)# exit
```

**Note :** Dans la vraie vie, vous uploadez un fichier .au ou .wav sur le flash du routeur.

### 5. Affichage du nom (Caller ID)

**Fonctionnalité :** Afficher le nom de l'appelant sur l'écran.

#### Configuration

```cisco
! Déjà activé par défaut avec "name" dans ephone-dn !
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# name Alice Dupont
! Ce nom s'affichera sur les autres phones quand Alice appelle
CME-Router(config-ephone-dn)# exit
```

**Résultat :** Quand Alice appelle Bob, l'écran de Bob affiche "Alice Dupont (2001)".

---

## 🎯 TP complet guidé

### Objectif du TP

Créer une infrastructure VoIP complète pour une petite entreprise de 5 personnes.

### Cahier des charges

```
┌──────────────────────────────────────────────────────────┐
│  ENTREPRISE : TechConsult SARL                           │
├──────────────────────────────────────────────────────────┤
│  Postes :                                                │
│    2001 - Accueil (Alice)                                │
│    2002 - Direction (Marc)                               │
│    2003 - IT (Bob)                                       │
│    2004 - Compta (Julie)                                 │
│    2005 - Commercial (Paul)                              │
│                                                          │
│  Fonctionnalités demandées :                             │
│    ✅ Appels internes                                    │
│    ✅ Messagerie vocale (2999)                           │
│    ✅ Transfert d'appel                                  │
│    ✅ Conférence à 3                                     │
│    ✅ Renvoi si occupé/pas de réponse                    │
└──────────────────────────────────────────────────────────┘
```

### Configuration complète pas à pas

#### Étape 1 : Topologie Packet Tracer

1. Ajouter les équipements :
   - 1× Routeur 2911
   - 1× Switch 2960
   - 5× Téléphones IP 7960 ou 7841
   - 1× PC (optionnel pour softphone)

2. Câbler :
   - Routeur Gi0/0 → Switch Gi0/1
   - Téléphones → Switch (ports Fa0/1 à Fa0/5)

#### Étape 2 : Configuration routeur (base)

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname CME-TechConsult
CME-TechConsult(config)#

! Interface LAN
CME-TechConsult(config)# interface GigabitEthernet 0/0
CME-TechConsult(config-if)# ip address 192.168.10.1 255.255.255.0
CME-TechConsult(config-if)# no shutdown
CME-TechConsult(config-if)# exit
CME-TechConsult(config)# exit
CME-TechConsult# write memory
```

#### Étape 3 : DHCP avec option 150

```cisco
CME-TechConsult(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

CME-TechConsult(config)# ip dhcp pool VOIP
CME-TechConsult(dhcp-config)# network 192.168.10.0 255.255.255.0
CME-TechConsult(dhcp-config)# default-router 192.168.10.1
CME-TechConsult(dhcp-config)# option 150 ip 192.168.10.1
CME-TechConsult(dhcp-config)# exit
```

#### Étape 4 : Activation CME

```cisco
CME-TechConsult(config)# telephony-service
CME-TechConsult(config-telephony)# max-ephones 10
CME-TechConsult(config-telephony)# max-dn 20
CME-TechConsult(config-telephony)# ip source-address 192.168.10.1 port 2000
CME-TechConsult(config-telephony)# create cnf-files
CME-TechConsult(config-telephony)# exit
```

#### Étape 5 : Création des numéros (ephone-dn)

```cisco
! 2001 - Accueil (Alice)
CME-TechConsult(config)# ephone-dn 1 dual-line
CME-TechConsult(config-ephone-dn)# number 2001
CME-TechConsult(config-ephone-dn)# name Alice Accueil
CME-TechConsult(config-ephone-dn)# exit

! 2002 - Direction (Marc)
CME-TechConsult(config)# ephone-dn 2 dual-line
CME-TechConsult(config-ephone-dn)# number 2002
CME-TechConsult(config-ephone-dn)# name Marc Direction
CME-TechConsult(config-ephone-dn)# exit

! 2003 - IT (Bob)
CME-TechConsult(config)# ephone-dn 3 dual-line
CME-TechConsult(config-ephone-dn)# number 2003
CME-TechConsult(config-ephone-dn)# name Bob IT
CME-TechConsult(config-ephone-dn)# exit

! 2004 - Compta (Julie)
CME-TechConsult(config)# ephone-dn 4 dual-line
CME-TechConsult(config-ephone-dn)# number 2004
CME-TechConsult(config-ephone-dn)# name Julie Compta
CME-TechConsult(config-ephone-dn)# exit

! 2005 - Commercial (Paul)
CME-TechConsult(config)# ephone-dn 5 dual-line
CME-TechConsult(config-ephone-dn)# number 2005
CME-TechConsult(config-ephone-dn)# name Paul Commercial
CME-TechConsult(config-ephone-dn)# exit

! 2999 - Messagerie vocale
CME-TechConsult(config)# ephone-dn 99
CME-TechConsult(config-ephone-dn)# number 2999
CME-TechConsult(config-ephone-dn)# name Messagerie
CME-TechConsult(config-ephone-dn)# exit
```

#### Étape 6 : Configuration des téléphones (ephone)

**Important :** Relever les MAC addresses des téléphones dans Packet Tracer.

```cisco
! Téléphone 1 (Alice)
CME-TechConsult(config)# ephone 1
CME-TechConsult(config-ephone)# mac-address 0001.9641.D4A1
! ⚠️ Remplacer par la vraie MAC
CME-TechConsult(config-ephone)# type 7960
CME-TechConsult(config-ephone)# button 1:1
CME-TechConsult(config-ephone)# exit

! Téléphone 2 (Marc)
CME-TechConsult(config)# ephone 2
CME-TechConsult(config-ephone)# mac-address 0001.9641.D4A2
CME-TechConsult(config-ephone)# type 7960
CME-TechConsult(config-ephone)# button 1:2
CME-TechConsult(config-ephone)# exit

! Téléphone 3 (Bob)
CME-TechConsult(config)# ephone 3
CME-TechConsult(config-ephone)# mac-address 0001.9641.D4A3
CME-TechConsult(config-ephone)# type 7960
CME-TechConsult(config-ephone)# button 1:3
CME-TechConsult(config-ephone)# exit

! Téléphone 4 (Julie)
CME-TechConsult(config)# ephone 4
CME-TechConsult(config-ephone)# mac-address 0001.9641.D4A4
CME-TechConsult(config-ephone)# type 7960
CME-TechConsult(config-ephone)# button 1:4
CME-TechConsult(config-ephone)# exit

! Téléphone 5 (Paul)
CME-TechConsult(config)# ephone 5
CME-TechConsult(config-ephone)# mac-address 0001.9641.D4A5
CME-TechConsult(config-ephone)# type 7960
CME-TechConsult(config-ephone)# button 1:5
CME-TechConsult(config-ephone)# exit

! Régénération des fichiers de config
CME-TechConsult(config)# telephony-service
CME-TechConsult(config-telephony)# create cnf-files
CME-TechConsult(config-telephony)# exit
```

#### Étape 7 : Fonctionnalités avancées

```cisco
! Messagerie vocale globale
CME-TechConsult(config)# telephony-service
CME-TechConsult(config-telephony)# voicemail 2999
CME-TechConsult(config-telephony)# max-conferences 4
CME-TechConsult(config-telephony)# transfer-system full-consult
CME-TechConsult(config-telephony)# exit

! Renvoi vers messagerie pour chaque poste
CME-TechConsult(config)# ephone-dn 1
CME-TechConsult(config-ephone-dn)# call-forward busy 2999
CME-TechConsult(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-TechConsult(config-ephone-dn)# exit

CME-TechConsult(config)# ephone-dn 2
CME-TechConsult(config-ephone-dn)# call-forward busy 2999
CME-TechConsult(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-TechConsult(config-ephone-dn)# exit

CME-TechConsult(config)# ephone-dn 3
CME-TechConsult(config-ephone-dn)# call-forward busy 2999
CME-TechConsult(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-TechConsult(config-ephone-dn)# exit

CME-TechConsult(config)# ephone-dn 4
CME-TechConsult(config-ephone-dn)# call-forward busy 2999
CME-TechConsult(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-TechConsult(config-ephone-dn)# exit

CME-TechConsult(config)# ephone-dn 5
CME-TechConsult(config-ephone-dn)# call-forward busy 2999
CME-TechConsult(config-ephone-dn)# call-forward noan 2999 timeout 20
CME-TechConsult(config-ephone-dn)# exit
```

#### Étape 8 : Tests

```
✅ Test 1 : Enregistrement des téléphones
   show ephone registered
   → Les 5 téléphones doivent être REGISTERED

✅ Test 2 : Appel interne
   Alice (2001) appelle Bob (2003)
   → Ça doit sonner et la communication passer

✅ Test 3 : Affichage du nom
   Vérifier que le nom s'affiche sur l'écran du destinataire

✅ Test 4 : Messagerie vocale
   Alice appelle Marc (2002)
   → Marc ne répond pas
   → Après 20s, renvoi vers 2999

✅ Test 5 : Transfert d'appel
   Alice en communication avec Bob
   → Alice transfère vers Marc
   → Bob et Marc en communication

✅ Test 6 : Conférence à 3
   Alice en communication avec Bob
   → Alice ajoute Marc en conférence
   → Les 3 en communication
```

### Configuration finale complète

<details>
<summary>Cliquez pour voir la config complète CME</summary>

```cisco
!
hostname CME-TechConsult
!
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
!
ip dhcp pool VOIP
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 option 150 ip 192.168.10.1
!
telephony-service
 max-ephones 10
 max-dn 20
 ip source-address 192.168.10.1 port 2000
 voicemail 2999
 max-conferences 4
 transfer-system full-consult
 create cnf-files
!
ephone-dn 1 dual-line
 number 2001
 name Alice Accueil
 call-forward busy 2999
 call-forward noan 2999 timeout 20
!
ephone-dn 2 dual-line
 number 2002
 name Marc Direction
 call-forward busy 2999
 call-forward noan 2999 timeout 20
!
ephone-dn 3 dual-line
 number 2003
 name Bob IT
 call-forward busy 2999
 call-forward noan 2999 timeout 20
!
ephone-dn 4 dual-line
 number 2004
 name Julie Compta
 call-forward busy 2999
 call-forward noan 2999 timeout 20
!
ephone-dn 5 dual-line
 number 2005
 name Paul Commercial
 call-forward busy 2999
 call-forward noan 2999 timeout 20
!
ephone-dn 99
 number 2999
 name Messagerie
!
ephone 1
 mac-address 0001.9641.D4A1
 type 7960
 button 1:1
!
ephone 2
 mac-address 0001.9641.D4A2
 type 7960
 button 1:2
!
ephone 3
 mac-address 0001.9641.D4A3
 type 7960
 button 1:3
!
ephone 4
 mac-address 0001.9641.D4A4
 type 7960
 button 1:4
!
ephone 5
 mac-address 0001.9641.D4A5
 type 7960
 button 1:5
!
end
```

</details>

---

## 🔍 Dépannage courant

### Problème 1 : Téléphone bloqué "Configuring IP"

**Symptômes :**
- Téléphone allumé
- Écran affiche "Configuring IP" ou "Obtaining IP Address"
- Ne va pas plus loin

**Diagnostic :**

```cisco
! Vérifier le DHCP
CME-Router# show ip dhcp pool

! Vérifier les baux DHCP
CME-Router# show ip dhcp binding
IP address       Hardware address        Lease expiration
192.168.10.101   0001.9641.D4A1          --

! Si aucun bail : problème réseau ou DHCP
```

**Solutions :**

```cisco
! 1. Vérifier que l'option 150 est présente
CME-Router# show run | include option 150
option 150 ip 192.168.10.1   ← Doit être là !

! 2. Si absente, l'ajouter
CME-Router(config)# ip dhcp pool VOIP
CME-Router(dhcp-config)# option 150 ip 192.168.10.1
CME-Router(dhcp-config)# exit

! 3. Relancer le téléphone (débrancher/rebrancher)
```

### Problème 2 : Téléphone non enregistré

**Symptômes :**
- Téléphone a une IP
- Écran affiche un numéro
- Mais pas de tonalité, impossible d'appeler

**Diagnostic :**

```cisco
CME-Router# show ephone registered
! Si le phone n'apparaît pas :

CME-Router# show ephone unregistered
ephone-1 Mac:0001.9641.D4A1 NOT REGISTERED   ← Problème !
```

**Solutions :**

```cisco
! 1. Vérifier la MAC address
CME-Router# show ephone
ephone-1 Mac:0001.9641.D4A1 TCP socket:[-1] activeLine:0 UNREGISTERED
  → Vérifier que la MAC correspond au vrai téléphone

! 2. Vérifier l'IP source
CME-Router# show telephony-service all
CONFIG (Version=12.0)
=====================
  Version 12.0
  Ip Address: 192.168.10.1 Port 2000   ← OK

! 3. Recréer les fichiers de config
CME-Router(config)# telephony-service
CME-Router(config-telephony)# create cnf-files
CME-Router(config-telephony)# exit

! 4. Reset le téléphone
CME-Router(config)# telephony-service
CME-Router(config-telephony)# reset all
```

### Problème 3 : Pas de tonalité

**Symptômes :**
- Téléphone enregistré
- Décroche le combiné
- Aucune tonalité

**Diagnostic :**

```cisco
CME-Router# show ephone 1
ephone-1 Mac:0001.9641.D4A1 TCP socket:[1] activeLine:0 REGISTERED
button 1: dn 1  number 2001 CH1   IDLE   ← Ligne assignée

! Vérifier le ephone-dn
CME-Router# show ephone-dn 1
ephone-dn 1 dual-line
 number 2001                              ← Numéro configuré
 name Alice Accueil
```

**Solutions :**

```cisco
! 1. Vérifier que le button est bien configuré
CME-Router(config)# ephone 1
CME-Router(config-ephone)# button 1:1
CME-Router(config-ephone)# exit

! 2. Recréer et reset
CME-Router(config)# telephony-service
CME-Router(config-telephony)# create cnf-files
CME-Router(config-telephony)# reset all
```

### Problème 4 : Appels ne passent pas

**Symptômes :**
- Téléphones enregistrés
- Tonalité OK
- Mais les appels ne passent pas (occupé, erreur, silence)

**Diagnostic :**

```cisco
! Activer le debug (ATTENTION : verbose !)
CME-Router# debug ephone detail
CME-Router# debug ephone-dn detail

! Faire un appel et observer les logs
! Désactiver le debug après
CME-Router# no debug all
```

**Causes fréquentes :**

```
❌ Numéro mal composé (ephone-dn number incorrect)
❌ Codec incompatible
❌ Pas de ephone-dn dual-line (ne permet qu'1 appel)
❌ Problème réseau (paquets perdus)
```

**Solutions :**

```cisco
! 1. Vérifier les numéros
CME-Router# show ephone-dn summary
DN  Tag  Num       State           CH  Port  Prefix  Name
==========================================================
1   1    2001      IDLE            0   -     -       Alice Accueil
2   2    2002      IDLE            0   -     -       Marc Direction

! 2. Tester un appel simple
! Depuis Alice (2001), composer 2002
! Observer avec "show ephone registered"

CME-Router# show ephone registered
! Vérifier que les 2 phones passent en état "CONNECTED"
```

### Commandes de diagnostic essentielles

```cisco
! === ÉTAT GÉNÉRAL ===
show telephony-service all     # Config globale CME
show ephone registered          # Téléphones enregistrés
show ephone summary             # Résumé téléphones
show ephone-dn summary          # Résumé numéros

! === DÉTAILS TÉLÉPHONE ===
show ephone 1                   # Détails ephone 1
show ephone phone-load          # Firmware téléphones
show ephone-dn 1                # Détails ephone-dn 1

! === APPELS EN COURS ===
show call active voice brief    # Appels actifs
show voice call summary         # Résumé appels

! === DHCP ===
show ip dhcp pool               # Pools DHCP
show ip dhcp binding            # Baux DHCP

! === DEBUG (avec précaution) ===
debug ephone detail             # Debug téléphones
debug ephone-dn detail          # Debug numéros
no debug all                    # STOP tous les debugs
```

---

## 📚 Ressources

### Documentation officielle

- [Cisco CME Configuration Guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucme/admin/configuration/manual/cmeadm/cmeprt1.html)
- [Cisco Packet Tracer VoIP Tutorial](https://www.netacad.com/courses/packet-tracer)

### Vidéos recommandées

- [CME Configuration Step-by-Step](https://www.youtube.com/results?search_query=cisco+cme+configuration)
- [Packet Tracer VoIP Lab](https://www.youtube.com/results?search_query=packet+tracer+voip+lab)

### Labs additionnels

- Créer un deuxième site CME et interconnecter les 2 sites (dial-peer VoIP)
- Ajouter une passerelle analogique (ATA) pour fax
- Configurer des groupes de sonnerie (hunt groups)
- Mettre en place un SVI VLAN voix séparé

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le TP)*

**Mes observations :**
-
-
-

**Problèmes rencontrés et solutions :**
-
-

**Améliorations à tester :**
-
-

---

## ✅ Checklist de révision

Avant de passer au cours suivant, assurez-vous de maîtriser :

- [ ] Je sais expliquer le rôle de CME
- [ ] Je comprends la différence entre ephone et ephone-dn
- [ ] Je sais configurer un serveur DHCP avec option 150
- [ ] Je peux créer un ephone-dn avec un numéro
- [ ] Je peux créer un ephone et l'associer à un ephone-dn
- [ ] Je sais vérifier l'enregistrement des téléphones
- [ ] Je peux configurer la messagerie vocale
- [ ] Je sais mettre en place le transfert d'appel
- [ ] Je peux configurer une conférence à 3
- [ ] Je maîtrise les commandes de diagnostic (show ephone, show ephone-dn)
- [ ] Je sais diagnostiquer les problèmes courants (pas de tonalité, non enregistré)

---

<div align="center">

**Cours précédent :** [02-protocoles-voip.md](02-protocoles-voip.md)

**Cours suivant :** [04-qos-vlans-voip.md](04-qos-vlans-voip.md)

[⬅️ Retour au sommaire](README.md)

</div>
