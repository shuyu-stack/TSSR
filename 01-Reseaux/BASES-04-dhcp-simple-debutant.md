# DHCP - Distribuer des IPs automatiquement

## Message du formateur

Le DHCP, c'est un **truc génial**. Au lieu de taper manuellement l'IP sur chaque PC (galère !), le serveur DHCP les distribue **automatiquement**.

Imagine : 200 PCs dans une entreprise. Sans DHCP, tu passes 1 mois à tout configurer. Avec DHCP, c'est **automatique** en 5 secondes par PC.

**Important :** Si tu n'as pas encore lu les cours sur les IPs et les VLANs, va les lire d'abord.

---

## C'est quoi le DHCP ? (Analogie distributeur)

### SANS DHCP (à la main)

```
Scène : Tu arrives au bureau avec ton nouveau PC

Toi : "Quelle IP je dois mettre ?"

Admin réseau : "Euh... attends je regarde mon fichier Excel...
                Voyons voir... 192.168.10.47 est libre.
                Prends celle-là."

Toi : "OK, je note. Et le masque ?"

Admin : "255.255.255.0"

Toi : "Et la passerelle ?"

Admin : "192.168.10.1"

Toi : "Et le DNS ?"

Admin : "8.8.8.8"

Toi : [tu tapes tout manuellement sur le PC]
      [ça prend 5 minutes]

Admin : [il note dans son Excel que .47 est pris]

─────────────────────────────────────────────────

Problèmes :
❌ Long (1 PC = 5 minutes)
❌ Risque d'erreur (faute de frappe)
❌ Risque de doublon (2 PCs avec la même IP = crash réseau)
❌ Fichier Excel à tenir à jour (cauchemar)
❌ Si tu changes de salle, il faut tout refaire
```

### AVEC DHCP (automatique)

```
Scène : Tu arrives au bureau avec ton nouveau PC

Toi : [tu branches le câble réseau]

PC : "Bonjour réseau, j'ai besoin d'une IP !"

Serveur DHCP : "OK, tiens : 192.168.10.47
                Masque : 255.255.255.0
                Passerelle : 192.168.10.1
                DNS : 8.8.8.8
                C'est valable pendant 24 heures."

PC : "Merci !"

Toi : [tu ne fais RIEN, c'est automatique]
      [ça prend 5 secondes]

─────────────────────────────────────────────────

Avantages :
✅ Rapide (automatique)
✅ Pas d'erreur (pas de faute de frappe)
✅ Pas de doublon (le serveur gère les IPs)
✅ Pas de fichier Excel à tenir
✅ Tu changes de salle ? Pas de problème, nouvelle IP auto
✅ Tu peux gérer 1000 PCs facilement

C'EST COMME UN DISTRIBUTEUR AUTOMATIQUE D'IPS !
```

---

## Comment ça marche ? (les 4 étapes DORA)

Le processus DHCP s'appelle **DORA** : Discover, Offer, Request, Acknowledgement.

### Les 4 étapes expliquées simplement

```
Étape 1 : DISCOVER (Découverte)
────────────────────────────────
PC : "Hé ! Y'a un serveur DHCP dans le coin ?"
     [broadcast = crie sur tout le réseau]


Étape 2 : OFFER (Offre)
───────────────────────
Serveur DHCP : "Ouais, je suis là !
                Je te propose l'IP 192.168.10.47"


Étape 3 : REQUEST (Demande)
───────────────────────────
PC : "OK, je prends cette IP !
      C'est bien pour moi ?"


Étape 4 : ACKNOWLEDGEMENT (Confirmation)
─────────────────────────────────────────
Serveur DHCP : "Validé !
                L'IP 192.168.10.47 est à toi
                pendant 24 heures (bail/lease)"
```

### Schéma visuel complet

```
    PC                    Serveur DHCP
     │                         │
     │  1. DISCOVER            │
     │  "Qui a une IP ?"       │
     │────────────────────────>│
     │         (broadcast)     │
     │                         │
     │  2. OFFER               │
     │  "Prends 192.168.10.47" │
     │<────────────────────────│
     │                         │
     │  3. REQUEST             │
     │  "OK je la prends"      │
     │────────────────────────>│
     │                         │
     │  4. ACK                 │
     │  "Validé pour 24h"      │
     │<────────────────────────│
     │                         │
     ✅ PC a son IP !
        192.168.10.47
        Masque : 255.255.255.0
        Passerelle : 192.168.10.1
        DNS : 8.8.8.8
```

**Note importante :** Si plusieurs serveurs DHCP répondent (OFFER), le PC prend généralement la **première offre** reçue.

---

## Le bail DHCP (lease)

### C'est quoi un bail ?

Le serveur DHCP **prête** une IP au PC pour une **durée limitée** (par défaut 24 heures).

```
Analogie : Location de voiture

Jour 1 : Tu loues une voiture pour 24h
         → Tu as la voiture

Jour 2 (après 12h) : Tu demandes à prolonger
         → Le loueur dit OK, encore 24h

Jour 3 : Tu rends la voiture
         → Le loueur peut la louer à quelqu'un d'autre

─────────────────────────────────────────────────

Avec DHCP :

Jour 1 : Tu reçois l'IP 192.168.10.47 pour 24h
         → Tu as l'IP

Jour 2 (après 12h) : Ton PC demande à renouveler
         → Le serveur dit OK, encore 24h

Jour 3 : Tu éteins ton PC et le ranges
         → L'IP retourne dans le pool (disponible)
```

### Durée du bail

```
Durée par défaut : 24 heures (86400 secondes)

Renouvellement :
- Après 50% du bail (12h) → Le PC essaie de renouveler
- Après 87,5% du bail (21h) → Le PC redemande avec insistance
- Après 100% (24h) → Si pas renouvelé, le PC perd l'IP

Bonne pratique :
- Réseau bureau : 8 jours (les gens viennent tous les jours)
- Réseau WiFi public : 1 heure (rotation rapide)
- Réseau serveurs : IP fixe (pas de DHCP)
```

---

## Les informations distribuées par DHCP

Le serveur DHCP ne donne pas QUE l'IP. Il donne **toutes les infos** dont le PC a besoin.

```
┌─────────────────────────────────────────────┐
│  INFORMATIONS DHCP                          │
├─────────────────────────────────────────────┤
│  1. IP ADDRESS (obligatoire)                │
│     L'adresse IP du PC                      │
│     Ex : 192.168.10.47                      │
├─────────────────────────────────────────────┤
│  2. SUBNET MASK (obligatoire)               │
│     Le masque de sous-réseau                │
│     Ex : 255.255.255.0                      │
├─────────────────────────────────────────────┤
│  3. DEFAULT GATEWAY (passerelle)            │
│     L'IP du routeur pour sortir du réseau   │
│     Ex : 192.168.10.1                       │
├─────────────────────────────────────────────┤
│  4. DNS SERVER (serveur de noms)            │
│     Pour convertir google.com → IP          │
│     Ex : 8.8.8.8 (Google DNS)               │
├─────────────────────────────────────────────┤
│  5. LEASE TIME (durée du bail)              │
│     Combien de temps tu gardes l'IP         │
│     Ex : 86400 secondes (24h)               │
├─────────────────────────────────────────────┤
│  Optionnel (mais utile) :                   │
│  - Serveur TFTP (pour les téléphones IP)    │
│  - NTP Server (serveur de temps)            │
│  - Domain Name (nom de domaine)             │
└─────────────────────────────────────────────┘
```

---

## Configurer DHCP sur un routeur Cisco

### Configuration de base (étape par étape)

Imagine que tu veux distribuer des IPs dans le réseau **192.168.10.0/24**.

**Ouvrir le CLI du routeur :**

```cisco
Router> enable
Router# configure terminal
Router(config)#
```

---

**Étape 1 : Créer un pool DHCP (= réserve d'IPs)**

```cisco
Router(config)# ip dhcp pool MON_RESEAU
Router(dhcp-config)#
```

**Explication :** Je crée une "réserve" d'IPs appelée `MON_RESEAU`.
Tu peux l'appeler comme tu veux : COMPTA, VLAN10, BUREAU, etc.

---

**Étape 2 : Définir le réseau à distribuer**

```cisco
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
```

**Explication :** Le serveur DHCP va distribuer des IPs dans le réseau 192.168.10.0/24 (de .1 à .254).

---

**Étape 3 : Définir la passerelle par défaut**

```cisco
Router(dhcp-config)# default-router 192.168.10.1
```

**Explication :** Les PCs auront comme passerelle l'IP 192.168.10.1 (souvent l'IP du routeur lui-même).

---

**Étape 4 : Définir le serveur DNS**

```cisco
Router(dhcp-config)# dns-server 8.8.8.8
```

**Explication :** Les PCs utiliseront le DNS de Google (8.8.8.8) pour résoudre les noms de domaine.

Alternatives :
- `8.8.8.8` → Google DNS (fiable)
- `1.1.1.1` → Cloudflare DNS (rapide)
- `192.168.10.10` → Ton propre serveur DNS interne

---

**Étape 5 : Définir la durée du bail (optionnel)**

```cisco
Router(dhcp-config)# lease 7
```

**Explication :** Le bail dure 7 jours (au lieu des 24h par défaut).

---

**Étape 6 : Sortir de la config du pool**

```cisco
Router(dhcp-config)# exit
Router(config)#
```

---

**Étape 7 : Exclure les IPs réservées**

```cisco
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
```

**Explication :** Le serveur DHCP NE DOIT PAS distribuer les IPs de .1 à .10.
Pourquoi ? Elles sont réservées pour :
- 192.168.10.1 → Routeur (passerelle)
- 192.168.10.2 → Serveur de fichiers
- 192.168.10.3 → Serveur mail
- 192.168.10.10 → Imprimante réseau
- etc.

---

**Étape 8 : Configurer l'interface du routeur**

```cisco
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Explication :** Le routeur doit avoir une IP dans le réseau pour pouvoir distribuer les IPs.

---

**Étape 9 : Sauvegarder**

```cisco
Router(config)# exit
Router# write memory
```

---

### Configuration complète (à copier-coller)

```cisco
! Entrer en mode configuration
Router> enable
Router# configure terminal

! Créer un pool DHCP
Router(config)# ip dhcp pool MON_RESEAU
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! Exclure les IPs réservées (routeur, serveurs)
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

! Configurer l'interface du routeur
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Sauvegarder
Router(config)# exit
Router# write memory
```

---

## TP Packet Tracer - DHCP pour 3 PCs

### Objectif

Configurer un serveur DHCP sur un routeur pour distribuer automatiquement des IPs à 3 PCs.

À la fin, les 3 PCs auront des IPs automatiques et pourront communiquer entre eux.

---

### Topologie cible

```
 PC1    PC2    PC3
  │      │      │
  └──────┴──────┘
         │
      Switch
         │
      Router
      (DHCP activé)
         │
   [Internet simulé]
```

---

### Étape 1 : Créer la topologie

1. Ajoute **3 PCs** (PC0, PC1, PC2)
2. Ajoute **1 switch 2960**
3. Ajoute **1 routeur 1841**

4. Câble :
   - PC0 → Switch Fa0/1
   - PC1 → Switch Fa0/2
   - PC2 → Switch Fa0/3
   - Switch Fa0/24 → Router Fa0/0

Attends que tous les câbles soient **verts**.

---

### Étape 2 : Configurer le routeur DHCP

Clique sur le routeur → CLI

```cisco
Router> enable
Router# configure terminal

! Créer le pool DHCP
Router(config)# ip dhcp pool RESEAU_BUREAU
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

! Exclure l'IP du routeur
Router(config)# ip dhcp excluded-address 192.168.10.1

! Configurer l'interface du routeur
Router(config)# interface fastEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

---

### Étape 3 : Configurer les PCs en DHCP

**Pour PC0 :**

1. Clique sur **PC0**
2. Va dans **Desktop**
3. Clique sur **IP Configuration**
4. Coche **DHCP** (au lieu de Static)
5. Attends quelques secondes...

**Résultat attendu :**

```
DHCP request successful

IP Address: 192.168.10.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
DNS Server: 8.8.8.8
```

**BRAVO ! Le PC a reçu son IP automatiquement !**

---

**Répète pour PC1 et PC2 :**

- PC1 : Coche DHCP
  - Résultat : 192.168.10.3 (ou une autre IP disponible)

- PC2 : Coche DHCP
  - Résultat : 192.168.10.4 (ou une autre IP disponible)

---

### Étape 4 : Vérifier les IPs distribuées

**Sur le routeur, tape cette commande :**

```cisco
Router# show ip dhcp binding
```

**Résultat attendu :**

```
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.10.2     0001.9652.5D42          Mar 02 2024 12:00 PM    Automatic
192.168.10.3     0001.4325.AB12          Mar 02 2024 12:01 PM    Automatic
192.168.10.4     0001.8765.CD34          Mar 02 2024 12:02 PM    Automatic
```

**Analyse :**
- 3 IPs ont été distribuées ✅
- Chaque IP a une adresse MAC associée
- Chaque IP a une date d'expiration (lease)

---

### Étape 5 : Tester la communication

**Test 1 : PC0 ping PC1**

1. Clique sur **PC0**
2. Desktop → Command Prompt
3. Tape : `ipconfig`
4. Note l'IP de PC0 (ex: 192.168.10.2)
5. Tape : `ping 192.168.10.3` (IP de PC1)
6. **Résultat attendu : Reply from... ✅**

**Test 2 : PC0 ping la passerelle (routeur)**

```
ping 192.168.10.1
```

**Résultat attendu : Reply from... ✅**

**Test 3 : PC0 ping DNS**

```
ping 8.8.8.8
```

**Résultat attendu : Reply from... ✅** (si Internet est configuré)

---

### Étape 6 : Renouveler le bail DHCP

Tu peux simuler le renouvellement du bail.

**Sur PC0 :**

1. Desktop → Command Prompt
2. Tape : `ipconfig /release` (libère l'IP)
3. Tape : `ipconfig` (tu n'as plus d'IP)
4. Tape : `ipconfig /renew` (redemande une IP)
5. **Le PC reçoit une nouvelle IP (ou la même)**

---

## Configuration DHCP avec VLANs (avancé)

Si tu as plusieurs VLANs, tu dois créer **un pool DHCP par VLAN**.

### Exemple : 3 VLANs

```
VLAN 10 : Compta (192.168.10.0/24)
VLAN 20 : RH (192.168.20.0/24)
VLAN 30 : Direction (192.168.30.0/24)
```

### Configuration routeur

```cisco
Router> enable
Router# configure terminal

! ═══════════════════════════════════
! Pool DHCP pour VLAN 10 (Compta)
! ═══════════════════════════════════
Router(config)# ip dhcp pool VLAN10_COMPTA
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

! ═══════════════════════════════════
! Pool DHCP pour VLAN 20 (RH)
! ═══════════════════════════════════
Router(config)# ip dhcp pool VLAN20_RH
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.10

! ═══════════════════════════════════
! Pool DHCP pour VLAN 30 (Direction)
! ═══════════════════════════════════
Router(config)# ip dhcp pool VLAN30_DIRECTION
Router(dhcp-config)# network 192.168.30.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.30.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.30.1 192.168.30.10

! ═══════════════════════════════════
! Sous-interfaces pour routing inter-VLAN
! ═══════════════════════════════════
Router(config)# interface fa0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fa0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Maintenant, chaque VLAN a son propre serveur DHCP !**

---

## Commandes de vérification DHCP

### Voir les IPs distribuées

```cisco
Router# show ip dhcp binding
```

**Affiche :**
- Les IPs attribuées
- Les adresses MAC des clients
- La date d'expiration du bail

---

### Voir les pools DHCP

```cisco
Router# show ip dhcp pool
```

**Affiche :**
- Les pools configurés
- Le réseau de chaque pool
- Le nombre d'IPs distribuées

---

### Voir les statistiques DHCP

```cisco
Router# show ip dhcp server statistics
```

**Affiche :**
- Nombre de DISCOVER reçus
- Nombre d'OFFER envoyés
- Nombre de REQUEST reçus
- Nombre d'ACK envoyés

---

### Effacer un bail DHCP (libérer une IP)

```cisco
Router# clear ip dhcp binding 192.168.10.2
```

**Effet :** L'IP 192.168.10.2 est libérée et retourne dans le pool.

---

## Exercices progressifs

### Exercice 1 (FACILE) - Configuration basique

Configure un serveur DHCP sur un routeur pour le réseau **192.168.50.0/24**.

**Paramètres :**
- Pool : RESEAU_TEST
- Passerelle : 192.168.50.1
- DNS : 1.1.1.1 (Cloudflare)
- Exclure : 192.168.50.1 à 192.168.50.20

<details>
<summary>📖 Voir la solution</summary>

```cisco
Router> enable
Router# configure terminal

Router(config)# ip dhcp pool RESEAU_TEST
Router(dhcp-config)# network 192.168.50.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.50.1
Router(dhcp-config)# dns-server 1.1.1.1
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 192.168.50.1 192.168.50.20

Router(config)# interface fa0/0
Router(config-if)# ip address 192.168.50.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

</details>

---

### Exercice 2 (MOYEN) - TP complet avec 2 réseaux

Crée cette topologie dans Packet Tracer :

```
Réseau 1 : 10.0.10.0/24 (3 PCs)
Réseau 2 : 10.0.20.0/24 (2 PCs)

Routeur avec DHCP sur les 2 réseaux
```

**Tâches :**
1. Crée la topologie
2. Configure DHCP pour les 2 réseaux
3. Configure les PCs en DHCP
4. Vérifie que tous les PCs ont des IPs
5. Teste les pings

<details>
<summary>📖 Voir la solution</summary>

**Configuration routeur :**

```cisco
Router> enable
Router# configure terminal

! Pool réseau 1
Router(config)# ip dhcp pool RESEAU1
Router(dhcp-config)# network 10.0.10.0 255.255.255.0
Router(dhcp-config)# default-router 10.0.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 10.0.10.1

! Pool réseau 2
Router(config)# ip dhcp pool RESEAU2
Router(dhcp-config)# network 10.0.20.0 255.255.255.0
Router(dhcp-config)# default-router 10.0.20.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp excluded-address 10.0.20.1

! Interfaces
Router(config)# interface fa0/0
Router(config-if)# ip address 10.0.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface fa0/1
Router(config-if)# ip address 10.0.20.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Test :**
```
PC réseau 1 ping PC réseau 2 → ✅ OK
```

</details>

---

### Exercice 3 (AVANCÉ) - DHCP avec VLANs

Crée un réseau complet avec :
- 1 switch configuré avec 3 VLANs (10, 20, 30)
- 1 routeur avec DHCP pour chaque VLAN
- 6 PCs (2 par VLAN)

**Tous les PCs doivent avoir des IPs automatiques.**

<details>
<summary>📖 Voir la solution</summary>

Voir la section "Configuration DHCP avec VLANs" plus haut pour la config complète.

**Résumé :**
1. Configure les VLANs sur le switch
2. Configure 3 pools DHCP sur le routeur (1 par VLAN)
3. Configure les sous-interfaces sur le routeur
4. Configure le trunk entre switch et routeur
5. Configure les PCs en DHCP

**Résultat :**
- PCs VLAN 10 → IPs en 192.168.10.x
- PCs VLAN 20 → IPs en 192.168.20.x
- PCs VLAN 30 → IPs en 192.168.30.x

</details>

---

## Récapitulatif - Ce que tu as appris

✅ **DHCP** = Distribution automatique d'IPs

✅ **DORA** (processus) :
  - Discover (le PC cherche un serveur)
  - Offer (le serveur propose une IP)
  - Request (le PC demande l'IP)
  - Acknowledgement (le serveur valide)

✅ **Bail DHCP** :
  - Durée : 24h par défaut
  - Renouvellement à 50% (12h)
  - Libération quand le PC s'éteint

✅ **Infos distribuées** :
  - IP, masque, passerelle, DNS, durée du bail

✅ **Configuration Cisco** :
  - `ip dhcp pool NOM`
  - `network X.X.X.X Y.Y.Y.Y`
  - `default-router X.X.X.X`
  - `dns-server X.X.X.X`
  - `ip dhcp excluded-address X.X.X.X Y.Y.Y.Y`

✅ **Vérification** :
  - `show ip dhcp binding`
  - `show ip dhcp pool`
  - `show ip dhcp server statistics`

---

## Prochaine étape

Maintenant que tu maîtrises les IPs, le subnetting, les VLANs et le DHCP, on va **assembler tout ça** pour créer un **réseau d'entreprise complet** !

**Conseils avant de continuer :**
- Refais les exercices jusqu'à ce que ce soit fluide
- Entraîne-toi à configurer DHCP avec plusieurs VLANs
- Teste les commandes de vérification

**Tu es prêt pour le projet final ! 💪**
