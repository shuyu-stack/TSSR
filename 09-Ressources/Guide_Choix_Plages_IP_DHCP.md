# Guide complet : Choisir les plages d'adresses IP pour DHCP

> **Pour Rimk** - Guide pratique pour comprendre comment et pourquoi on choisit les plages IP  
> **Date** : 13 janvier 2026  
> **Formation** : TSSR - Technicien Supérieur Systèmes et Réseaux

---

## 📚 Table des matières

1. [Comprendre les bases](#comprendre-les-bases)
2. [Anatomie d'une adresse IP](#anatomie-dune-adresse-ip)
3. [Comment choisir sa plage DHCP](#comment-choisir-sa-plage-dhcp)
4. [Stratégie de découpage réseau](#stratégie-de-découpage-réseau)
5. [Cas pratiques avec exemples](#cas-pratiques-avec-exemples)
6. [Tests à effectuer](#tests-à-effectuer)
7. [Erreurs courantes à éviter](#erreurs-courantes-à-éviter)

---

## 🎯 Comprendre les bases

### Qu'est-ce qu'une plage d'adresses IP ?

Une plage d'adresses IP, c'est comme **un parking avec des places numérotées**.

```
Analogie du parking :

┌────────────────────────────────────────────┐
│         Parking (Réseau)                   │
│         192.168.230.0/24                   │
├────────────────────────────────────────────┤
│                                            │
│  Places 1-50      │  Places 51-200         │
│  (réservées)      │  (disponibles)         │
│  ────────────     │  ──────────────        │
│  • Entrée         │  • Visiteur 1          │
│  • Gardien        │  • Visiteur 2          │
│  • Direction      │  • Visiteur 3          │
│  • ...            │  • ...                 │
│                                            │
└────────────────────────────────────────────┘

Réseau = Parking complet
Plage DHCP = Places pour visiteurs (attribution automatique)
IPs fixes = Places réservées nominativement
```

**En développement, c'est comme :**

```javascript
// Un pool de connexions à une base de données

const connectionPool = {
  total: 254,           // Nombre total de connexions possibles
  reserved: 50,         // Connexions réservées pour services critiques
  dynamic: 150,         // Connexions pour utilisateurs temporaires
  available: function() {
    return this.dynamic - this.inUse;
  }
};
```

---

## 🔍 Anatomie d'une adresse IP

### Structure d'une adresse IPv4

Une adresse IP, c'est **4 nombres séparés par des points**.

```
192.168.230.134
 │   │   │   │
 │   │   │   └─► Hôte (identifiant de la machine)
 │   │   └─────► Sous-réseau
 │   └─────────► Réseau privé
 └─────────────► Classe d'adresse
```

### Le masque de sous-réseau

Le masque définit **combien de machines peuvent être dans ton réseau**.

```
255.255.255.0  (aussi écrit /24)
 │   │   │  │
 └───┴───┴──┴─► Les 255 = partie réseau (fixe)
            └─► Le 0 = partie hôte (variable)

Exemple :
Réseau : 192.168.230.0
Masque : 255.255.255.0

Les 3 premiers nombres sont fixes : 192.168.230
Le dernier nombre varie : 0 à 255

Nombre de machines possibles : 254
(on enlève .0 = adresse réseau et .255 = broadcast)
```

**Analogie développeur :**

```javascript
// Le masque, c'est comme un pattern de validation

const ipPattern = {
  network: "192.168.230",  // Partie fixe (comme un namespace)
  host: "[0-255]"          // Partie variable (comme un ID)
};

// Adresses valides dans ce réseau :
// 192.168.230.1 ✓
// 192.168.230.50 ✓
// 192.168.230.134 ✓
// 192.168.1.50 ✗ (mauvais réseau)
```

### Calcul du nombre d'hôtes

```
Formule : 2^n - 2

n = nombre de bits pour les hôtes
-2 car on retire l'adresse réseau et broadcast

Masque /24 (255.255.255.0) :
└─► 8 bits pour les hôtes
└─► 2^8 - 2 = 256 - 2 = 254 machines possibles

Masque /16 (255.255.0.0) :
└─► 16 bits pour les hôtes
└─► 2^16 - 2 = 65536 - 2 = 65 534 machines possibles

Masque /25 (255.255.255.128) :
└─► 7 bits pour les hôtes
└─► 2^7 - 2 = 128 - 2 = 126 machines possibles
```

### Adresses spéciales à connaître

```
Dans le réseau 192.168.230.0/24 :

192.168.230.0    → Adresse RÉSEAU
                    (Ne JAMAIS l'attribuer à une machine)
                    C'est l'identifiant du réseau lui-même

192.168.230.1    → Généralement la PASSERELLE
                    (Routeur, point de sortie vers Internet)

192.168.230.255  → Adresse BROADCAST
                    (Pour envoyer à toutes les machines du réseau)
                    (Ne JAMAIS l'attribuer à une machine)

192.168.230.1    → Plage UTILISABLE
   à                pour les machines
192.168.230.254
```

**Analogie :**

```
Immeuble (Réseau) : 192.168.230.0
├─ Rez-de-chaussée (.0) : Hall d'entrée (réseau)
├─ 1er étage (.1) : Gardien/Concierge (passerelle)
├─ 2e au 254e (.2 à .254) : Appartements (machines)
└─ Toit (.255) : Antenne commune (broadcast)
```

---

## 🎯 Comment choisir sa plage DHCP

### Étape 1 : Identifier ton réseau existant

**Sur Windows :**

```powershell
ipconfig

# Tu verras quelque chose comme :
# Adresse IPv4 : 192.168.230.10
# Masque : 255.255.255.0
```

**Ce que ça te dit :**

```
Réseau : 192.168.230.0/24

Plage totale utilisable : 192.168.230.1 à 192.168.230.254
Nombre de machines possibles : 254
```

### Étape 2 : Recenser les IPs déjà utilisées

**Liste les équipements qui ont des IPs fixes :**

```
192.168.230.1    → Passerelle (routeur virtuel VirtualBox/VMware)
192.168.230.2    → Parfois aussi la passerelle
192.168.230.10   → Ton serveur Windows (contrôleur de domaine)
192.168.230.11   → Éventuel autre serveur
192.168.230.12   → ...
```

**Règle d'or :** Les serveurs et équipements réseau doivent TOUJOURS avoir des IPs **FIXES** (statiques), jamais en DHCP.

### Étape 3 : Découper en zones

Imagine ton réseau comme des **quartiers d'une ville** :

```
┌──────────────────────────────────────────────────────┐
│         Réseau 192.168.230.0/24 (254 IPs)            │
├──────────────────────────────────────────────────────┤
│                                                      │
│  Zone 1 : Infrastructure (IPs fixes)                 │
│  192.168.230.1 → 192.168.230.50                     │
│  ├─ .1 ou .2 : Passerelle                          │
│  ├─ .10 : Serveur AD/DNS/DHCP                      │
│  ├─ .11 : Serveur de fichiers                      │
│  ├─ .12 : Serveur d'impression                     │
│  ├─ .20-30 : Switches, points d'accès WiFi         │
│  └─ .31-50 : Réserve pour futurs serveurs          │
│                                                      │
│  Zone 2 : Postes clients (DHCP)                     │
│  192.168.230.51 → 192.168.230.200                   │
│  ├─ 150 adresses disponibles                        │
│  ├─ Attribution automatique                         │
│  └─ Pour : PC, portables, tablettes, smartphones   │
│                                                      │
│  Zone 3 : Réserve future                            │
│  192.168.230.201 → 192.168.230.254                  │
│  └─ 54 adresses disponibles pour extension         │
│                                                      │
└──────────────────────────────────────────────────────┘
```

### Étape 4 : Définir la plage DHCP

**Configuration recommandée pour un lab :**

```
Plage DHCP :
├─ Début : 192.168.230.51
└─ Fin   : 192.168.230.200

Nombre d'IPs : 200 - 51 + 1 = 150

Exclusions DHCP :
└─ 192.168.230.1 à 192.168.230.50
   (Pour empêcher le DHCP de distribuer ces IPs)
```

**En PowerShell sur le serveur :**

```powershell
# Créer l'étendue
Add-DhcpServerv4Scope `
    -Name "Etendue Lab" `
    -StartRange 192.168.230.51 `
    -EndRange 192.168.230.200 `
    -SubnetMask 255.255.255.0 `
    -State Active

# Ajouter les exclusions
Add-DhcpServerv4ExclusionRange `
    -ScopeId 192.168.230.0 `
    -StartRange 192.168.230.1 `
    -EndRange 192.168.230.50
```

---

## 🏗️ Stratégie de découpage réseau

### Petit réseau (Lab / PME)

**Réseau : 192.168.1.0/24 (254 hôtes)**

```
Zone Infrastructure : .1 à .20 (20 IPs)
├─ .1 : Routeur
├─ .10 : Serveur principal
└─ .11-20 : Autres serveurs/équipements

Zone DHCP : .21 à .200 (180 IPs)
└─ Pour ordinateurs et appareils

Zone Réserve : .201 à .254 (54 IPs)
└─ Extension future
```

### Réseau moyen (Entreprise)

**Réseau : 192.168.0.0/22 (1022 hôtes)**

Attention, là c'est un masque /22, donc 4 fois plus grand qu'un /24 !

```
192.168.0.0 à 192.168.3.254 (1022 IPs)

Zone Management : 192.168.0.1 → .50
├─ Serveurs critiques
└─ Équipements réseau

Zone Postes fixes : 192.168.0.51 → 192.168.1.254 (DHCP)
└─ 460 IPs pour PC de bureau

Zone WiFi : 192.168.2.1 → 192.168.2.254 (DHCP)
└─ 254 IPs pour appareils mobiles

Zone VoIP : 192.168.3.1 → 192.168.3.100 (DHCP)
└─ 100 IPs pour téléphones IP

Réserve : 192.168.3.101 → .254
```

### Bonnes pratiques de découpage

```
1. Infrastructure (5-10% du réseau)
   └─ IPs FIXES pour serveurs et équipements critiques

2. DHCP (60-80% du réseau)
   └─ IPs DYNAMIQUES pour postes clients

3. Réserve (10-30% du réseau)
   └─ Pour croissance future
```

**Analogie développeur :**

```javascript
// Comme l'allocation de mémoire

const networkAllocation = {
  total: 254,
  infrastructure: Math.floor(254 * 0.10),  // 10% = 25 IPs
  dhcp: Math.floor(254 * 0.70),            // 70% = 177 IPs
  reserve: Math.floor(254 * 0.20)          // 20% = 50 IPs
};

// On laisse toujours une marge pour la croissance
// Exactement comme un buffer ou une pool de threads
```

---

## 💼 Cas pratiques avec exemples

### Cas 1 : Ton lab TSSR (configuration actuelle)

**Contexte :**
- 1 serveur Windows 2025 (AD/DNS/DHCP)
- 2-3 VMs clientes pour tests
- Réseau NAT VMware : 192.168.230.0/24

**Choix de la plage :**

```
Réseau total : 192.168.230.0/24 (254 IPs)

Zone fixe : .1 à .50 (50 IPs)
├─ .2 : Passerelle VMware (imposée)
├─ .10 : Serveur WS2025
└─ .11-50 : Réserve pour futurs serveurs

Zone DHCP : .51 à .200 (150 IPs)
└─ Largement suffisant pour 10-20 VMs de test

Zone libre : .201 à .254 (54 IPs)
└─ Pour extension ou tests spéciaux
```

**Pourquoi ce choix ?**

✅ 150 IPs pour DHCP = 10-15x plus que nécessaire (marge confortable)  
✅ 50 IPs réservées = assez pour 5-10 serveurs de test  
✅ 54 IPs de réserve = flexibilité pour expérimentations  

### Cas 2 : PME avec 30 employés

**Contexte :**
- 30 PC de bureau
- 30 smartphones
- 5 serveurs
- 3 imprimantes réseau
- 2 switches
- 1 routeur

**Total :** ~70 appareils

**Choix de la plage :**

```
Réseau : 192.168.10.0/24 (254 IPs)

Zone Infrastructure : .1 à .20 (20 IPs)
├─ .1 : Routeur
├─ .10 : Serveur AD/DNS
├─ .11 : Serveur fichiers
├─ .12 : Serveur mail
├─ .13 : Serveur web
├─ .14 : Serveur backup
├─ .15-17 : Imprimantes
└─ .18-20 : Switches

Zone DHCP PC : .21 à .120 (100 IPs)
└─ Pour les 30 PC (marge x3)

Zone DHCP Mobile : .121 à .200 (80 IPs)
└─ Pour smartphones/tablettes (marge x2.5)

Réserve : .201 à .254 (54 IPs)
```

**Pourquoi ces marges ?**

```
Marge x2 à x3 = Bonne pratique

Raisons :
1. Croissance de l'entreprise
2. Invités/visiteurs avec leurs appareils
3. Appareils temporaires (techniciens, formations)
4. Plusieurs appareils par personne
5. Baux DHCP qui se chevauchent

Exemple :
30 employés × 2 appareils/personne = 60 appareils
+ 10 invités = 70 appareils
+ 20% de marge = 84 appareils
→ Prévoir 100 IPs minimum
```

### Cas 3 : Café avec WiFi public

**Contexte :**
- 1 routeur
- 1 borne WiFi
- 1 caisse enregistreuse
- Max 50 clients simultanés

**Choix de la plage :**

```
Réseau : 192.168.100.0/24 (254 IPs)

Zone Infrastructure : .1 à .10 (10 IPs)
├─ .1 : Routeur
├─ .2 : Borne WiFi
└─ .10 : Caisse

Zone DHCP : .11 à .150 (140 IPs)
└─ Pour clients WiFi (marge x2.8)

Bail DHCP court : 2 heures
└─ Rotation rapide des clients
```

**Spécificité :** Bail court (2h) au lieu de 8 jours

```
Pourquoi un bail court ?

WiFi public = rotation rapide de clients
- Client arrive : reçoit une IP
- Client part : IP libérée après 2h
- Nouveaux clients peuvent réutiliser les IPs

Calcul :
50 clients max × 2 heures = 100 IPs max
+ marge 40% = 140 IPs

Analogie :
C'est comme des tables de restaurant :
- Bail long (8j) = Restaurant avec réservations
- Bail court (2h) = Fast-food sans réservation
```

### Cas 4 : Réseau segmenté par VLAN

**Contexte :** Entreprise avec départements isolés

```
VLAN 10 - Direction : 192.168.10.0/24
├─ .1-20 : Infrastructure
└─ .21-100 : DHCP (10 postes)

VLAN 20 - Comptabilité : 192.168.20.0/24
├─ .1-20 : Infrastructure
└─ .21-100 : DHCP (15 postes)

VLAN 30 - Production : 192.168.30.0/24
├─ .1-20 : Infrastructure
└─ .21-200 : DHCP (50 postes)

VLAN 40 - Invités : 192.168.40.0/24
├─ .1-10 : Infrastructure
└─ .11-254 : DHCP (jusqu'à 244 invités)
```

**Avantages de la segmentation :**

```
Sécurité :
- Direction isolée de la production
- Invités isolés du réseau interne

Performance :
- Broadcast limité à chaque VLAN
- Moins de traffic sur chaque segment

Gestion :
- Politiques DHCP différentes par VLAN
- Durées de bail adaptées (8j pour staff, 4h pour invités)
```

---

## 🧪 Tests à effectuer

### Test 1 : Vérifier la configuration réseau

**Sur le serveur DHCP :**

```powershell
# Vérifier le service DHCP
Get-Service DHCPServer
# Résultat attendu : Running

# Voir l'étendue configurée
Get-DhcpServerv4Scope
# Vérifier : StartRange, EndRange, State = Active

# Voir les options DHCP
Get-DhcpServerv4OptionValue -ScopeId 192.168.230.0
# Vérifier : Routeur (passerelle), DNS
```

**Résultat attendu :**

```
Status  Name         DisplayName
------  ----         -----------
Running DHCPServer   Serveur DHCP

ScopeId       StartRange      EndRange        State
-------       ----------      --------        -----
192.168.230.0 192.168.230.51  192.168.230.200 Active

OptionId Name          Value
-------- ----          -----
3        Routeur       {192.168.230.2}
6        Serveurs DNS  {192.168.230.10}
```

### Test 2 : Attribution d'IP à un client

**Sur une VM cliente :**

```powershell
# Libérer l'IP actuelle
ipconfig /release

# Vérifier qu'on n'a plus d'IP
ipconfig
# Tu devrais voir une IP APIPA (169.254.x.x) temporairement

# Demander une nouvelle IP
ipconfig /renew

# Vérifier la nouvelle IP
ipconfig /all
```

**Résultat attendu :**

```
Carte Ethernet Ethernet0 :

   DHCP activé : Oui
   Adresse IPv4 : 192.168.230.XXX
                  (XXX entre 51 et 200)
   Masque : 255.255.255.0
   Passerelle : 192.168.230.2
   Serveur DHCP : 192.168.230.10
   Serveurs DNS : 192.168.230.10
```

**✅ Vérifications :**
- [ ] L'IP est bien dans la plage (.51 à .200)
- [ ] La passerelle est correcte (.2)
- [ ] Le serveur DHCP est identifié (.10)
- [ ] Le DNS est configuré (.10)

### Test 3 : Vérifier le bail côté serveur

**Sur le serveur :**

```powershell
# Lister tous les baux actifs
Get-DhcpServerv4Lease -ScopeId 192.168.230.0

# Voir les statistiques
Get-DhcpServerv4ScopeStatistics -ScopeId 192.168.230.0
```

**Résultat attendu :**

```
IPAddress     ClientId          HostName           LeaseExpiryTime
---------     --------          --------           ---------------
192.168.230.51 00-0c-29-xx-xx-xx Client1           21/01/2026 12:00
192.168.230.52 00-0c-29-yy-yy-yy Client2           21/01/2026 13:30

Free  InUse  PercentageInUse
----  -----  ---------------
148   2      1.33
```

**✅ Vérifications :**
- [ ] Les clients apparaissent avec leur IP
- [ ] L'adresse MAC (ClientId) est enregistrée
- [ ] La date d'expiration est dans le futur
- [ ] Le nombre d'IPs libres est correct (150 - nombre de clients)

### Test 4 : Test de connectivité réseau

**Depuis le client :**

```powershell
# Ping vers la passerelle
ping 192.168.230.2
# Résultat attendu : Réponses reçues

# Ping vers le serveur
ping 192.168.230.10
# Résultat attendu : Réponses reçues

# Ping vers un nom de domaine (test DNS)
ping WIN2025TP
# ou
ping WIN2025TP.solaris.local
# Résultat attendu : Résolution DNS + Réponses

# Test de résolution DNS
nslookup WIN2025TP.solaris.local
# Résultat attendu : Adresse IP du serveur
```

**Résultat attendu :**

```
C:\> ping 192.168.230.2
Réponse de 192.168.230.2 : octets=32 temps<1ms TTL=64
✓ La passerelle est accessible

C:\> ping 192.168.230.10
Réponse de 192.168.230.10 : octets=32 temps<1ms TTL=128
✓ Le serveur est accessible

C:\> ping WIN2025TP
Réponse de 192.168.230.10 : octets=32 temps<1ms TTL=128
✓ Le DNS fonctionne
```

### Test 5 : Renouvellement du bail

**Objectif :** Vérifier que le client peut prolonger son bail

**Sur le client :**

```powershell
# Noter l'heure d'expiration actuelle
ipconfig /all | Select-String "Bail"

# Forcer le renouvellement
ipconfig /renew

# Vérifier la nouvelle heure d'expiration
ipconfig /all | Select-String "Bail"
```

**Résultat attendu :**

```
AVANT :
Bail obtenu : 13/01/2026 10:00:00
Bail expirant : 21/01/2026 10:00:00

APRÈS renouvellement :
Bail obtenu : 13/01/2026 14:30:00  ← Heure actuelle
Bail expirant : 21/01/2026 14:30:00  ← +8 jours
```

**✅ Vérification :**
- [ ] L'IP reste la même (généralement)
- [ ] Le bail obtenu = heure du renouvellement
- [ ] Le bail expirant = obtenu + durée configurée

### Test 6 : Saturation de la plage DHCP (avancé)

**Objectif :** Voir ce qui se passe quand toutes les IPs sont utilisées

**Configuration pour le test :**

```powershell
# Sur le serveur, créer une petite étendue de test
Add-DhcpServerv4Scope `
    -Name "Test Saturation" `
    -StartRange 192.168.230.250 `
    -EndRange 192.168.230.252 `
    -SubnetMask 255.255.255.0 `
    -State Active
# Cette étendue n'a que 3 IPs disponibles
```

**Procédure :**

1. Connecter 3 clients → Ils obtiennent .250, .251, .252
2. Connecter un 4ème client → Il ne peut pas obtenir d'IP
3. Observer le comportement

**Résultat du 4ème client :**

```
Adresse d'autoconfiguration IPv4 : 169.254.x.x
(Adresse APIPA = échec DHCP)
```

**✅ Conclusion :**
- Quand le pool est épuisé, les clients reçoivent une IP APIPA
- Ils peuvent communiquer entre eux localement
- Mais pas d'accès Internet (pas de passerelle)

### Test 7 : Réservation DHCP

**Objectif :** Garantir qu'une machine reçoit toujours la même IP

**Sur le serveur :**

```powershell
# Créer une réservation pour un client spécifique
Add-DhcpServerv4Reservation `
    -ScopeId 192.168.230.0 `
    -IPAddress 192.168.230.100 `
    -ClientId "00-0c-29-xx-xx-xx" `
    -Name "Client-Important" `
    -Description "Poste comptabilité"

# Vérifier la réservation
Get-DhcpServerv4Reservation -ScopeId 192.168.230.0
```

**Sur le client réservé :**

```powershell
# Renouveler l'IP
ipconfig /release
ipconfig /renew

# Vérifier l'IP reçue
ipconfig
```

**Résultat attendu :**

```
Le client reçoit TOUJOURS : 192.168.230.100
Même après :
- Redémarrage
- Release/Renew
- Expiration du bail
```

**✅ Cas d'usage des réservations :**
- Imprimantes réseau
- Serveurs légers (ex: NAS)
- Postes critiques (direction, comptabilité)
- Équipements qui doivent être joignables à une IP fixe

### Test 8 : Options DHCP personnalisées

**Objectif :** Distribuer des configurations avancées

**Sur le serveur :**

```powershell
# Option 42 : Serveur NTP (horloge réseau)
Set-DhcpServerv4OptionValue `
    -ScopeId 192.168.230.0 `
    -OptionId 42 `
    -Value "192.168.230.10"

# Option 15 : Nom de domaine DNS
Set-DhcpServerv4OptionValue `
    -ScopeId 192.168.230.0 `
    -OptionId 15 `
    -Value "solaris.local"

# Vérifier toutes les options
Get-DhcpServerv4OptionValue -ScopeId 192.168.230.0
```

**Sur le client après renouvellement :**

```powershell
ipconfig /all
```

**Résultat attendu :**

```
Serveurs DNS : 192.168.230.10
Suffixe DNS : solaris.local
Liste de recherche DNS : solaris.local
```

### Récapitulatif des tests

```
┌─────────────────────────────────────────────────────┐
│ Tests essentiels (à faire systématiquement)         │
├─────────────────────────────────────────────────────┤
│ ✓ Service DHCP en cours d'exécution                 │
│ ✓ Étendue active et correctement configurée         │
│ ✓ Client obtient une IP dans la bonne plage         │
│ ✓ Options DHCP distribuées (passerelle, DNS)        │
│ ✓ Connectivité réseau (ping serveur, passerelle)    │
│ ✓ Résolution DNS fonctionnelle                      │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│ Tests avancés (pour aller plus loin)                │
├─────────────────────────────────────────────────────┤
│ ✓ Renouvellement du bail                            │
│ ✓ Réservations DHCP                                 │
│ ✓ Comportement en cas de saturation                 │
│ ✓ Options DHCP personnalisées                       │
│ ✓ Statistiques d'utilisation                        │
└─────────────────────────────────────────────────────┘
```

---

## ⚠️ Erreurs courantes à éviter

### Erreur 1 : Plage DHCP qui chevauche des IPs fixes

**❌ Mauvaise configuration :**

```
Serveur fixe : 192.168.1.10
Plage DHCP : 192.168.1.1 → 192.168.1.100

Problème :
Le DHCP pourrait distribuer .10 à un client
→ Conflit d'IP !
→ Le serveur et le client ne fonctionneront plus correctement
```

**✅ Bonne configuration :**

```
Serveur fixe : 192.168.1.10
Plage DHCP : 192.168.1.51 → 192.168.1.200
Exclusion : 192.168.1.1 → 192.168.1.50

Résultat :
Le DHCP ne distribuera JAMAIS d'IP entre .1 et .50
Ton serveur en .10 est protégé
```

### Erreur 2 : Plage trop petite

**❌ Mauvaise configuration :**

```
Entreprise avec 50 employés
Plage DHCP : 192.168.1.100 → 192.168.1.130 (31 IPs)

Problème :
50 employés × 2 appareils = 100 appareils
31 IPs disponibles = saturation garantie !
```

**✅ Bonne configuration :**

```
50 employés × 2 appareils = 100 appareils
+ 50% de marge = 150 IPs minimum

Plage DHCP : 192.168.1.51 → 192.168.1.200 (150 IPs)
```

**Règle :** Toujours prévoir 50-100% de marge

### Erreur 3 : Oublier d'exclure la passerelle

**❌ Mauvaise configuration :**

```
Passerelle : 192.168.1.1
Plage DHCP : 192.168.1.1 → 192.168.1.254

Problème :
Le DHCP pourrait distribuer .1 à un client
→ Plus d'accès Internet pour ce client
→ Conflit avec le routeur
```

**✅ Bonne configuration :**

```
Passerelle : 192.168.1.1
Plage DHCP : 192.168.1.50 → 192.168.1.200
Exclusion : 192.168.1.1 → 192.168.1.49
```

### Erreur 4 : Inclure l'adresse réseau ou broadcast

**❌ Mauvaise configuration :**

```
Réseau : 192.168.1.0/24
Plage DHCP : 192.168.1.0 → 192.168.1.255

Problème :
.0 = adresse réseau (interdite)
.255 = broadcast (interdite)
```

**✅ Bonne configuration :**

```
Plage DHCP : 192.168.1.1 → 192.168.1.254
(Ou mieux : .51 → .200)
```

### Erreur 5 : Durée de bail inadaptée

**❌ Trop longue pour un réseau dynamique :**

```
Café WiFi public
Durée du bail : 30 jours

Problème :
50 clients par jour × 30 jours = 1500 baux actifs
Mais seulement 200 IPs disponibles !
→ Saturation en 4 jours
```

**✅ Bail court :**

```
Café WiFi public
Durée du bail : 2 heures

Résultat :
Client part → IP libérée après 2h
→ Rotation efficace des IPs
```

**❌ Trop courte pour un réseau stable :**

```
Bureau d'entreprise
Durée du bail : 1 heure

Problème :
Renouvellement toutes les 30 min (T1 = 50%)
→ Charge inutile sur le serveur DHCP
→ Logs saturés
```

**✅ Bail long :**

```
Bureau d'entreprise
Durée du bail : 8 jours

Résultat :
Postes fixes gardent leur IP
→ Moins de trafic DHCP
→ Configuration stable
```

### Erreur 6 : Mauvaise configuration DNS

**❌ Mauvaise configuration :**

```
Options DHCP :
├─ Passerelle : 192.168.1.1  ✓
└─ DNS : (vide)              ✗

Résultat :
Les clients peuvent ping des IPs
Mais ne peuvent pas résoudre les noms
(ping google.com échoue)
```

**✅ Bonne configuration :**

```
Options DHCP :
├─ Passerelle : 192.168.1.1
├─ DNS primaire : 192.168.1.10 (ton serveur AD)
└─ DNS secondaire : 8.8.8.8 (Google, en backup)
```

### Erreur 7 : Ne pas documenter les plages

**❌ Pas de documentation :**

```
6 mois plus tard :
"Pourquoi l'IP .100 ne répond plus ?"
"Quelle est notre plage DHCP déjà ?"
"Est-ce que .75 est libre ?"
→ Confusion, perte de temps
```

**✅ Documentation claire :**

```markdown
# Plan d'adressage réseau 192.168.1.0/24

## Infrastructure (IPs fixes)
- .1-20 : Équipements réseau
  - .1 : Routeur
  - .10 : Serveur AD/DNS/DHCP
  - .11 : Serveur fichiers

## DHCP (IPs dynamiques)
- .51-200 : Postes clients (150 IPs)

## Réservations DHCP
- .100 : Imprimante RH (MAC: xx:xx:xx:xx:xx:01)
- .101 : Imprimante Compta (MAC: xx:xx:xx:xx:xx:02)

## Réserve
- .201-254 : Extension future (54 IPs)

Dernière mise à jour : 13/01/2026
```

---

## 📚 Mémo rapide

### Pour choisir une plage DHCP :

```
1. Identifier le réseau
   └─► ipconfig → noter l'IP et le masque

2. Calculer la plage utilisable
   └─► Réseau /24 = .1 à .254 (254 IPs)

3. Réserver les IPs fixes
   └─► .1 à .50 pour infrastructure

4. Définir la plage DHCP
   └─► .51 à .200 (avec marge 2-3x)

5. Garder une réserve
   └─► .201 à .254 pour extension

6. Configurer les exclusions
   └─► Exclure .1 à .50 dans DHCP

7. Tester !
   └─► ipconfig /renew sur un client
```

### Commandes PowerShell essentielles :

```powershell
# Créer une étendue
Add-DhcpServerv4Scope -Name "Lab" -StartRange 192.168.230.51 -EndRange 192.168.230.200 -SubnetMask 255.255.255.0

# Ajouter des exclusions
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.230.0 -StartRange 192.168.230.1 -EndRange 192.168.230.50

# Configurer les options
Set-DhcpServerv4OptionValue -ScopeId 192.168.230.0 -Router 192.168.230.2 -DnsServer 192.168.230.10

# Voir les baux actifs
Get-DhcpServerv4Lease -ScopeId 192.168.230.0

# Statistiques
Get-DhcpServerv4ScopeStatistics -ScopeId 192.168.230.0
```

---

## 🎯 Conclusion

Ne t'inquiète pas pour l'âge, Rimk ! À 46 ans, tu as l'avantage de l'expérience et de la méthodologie. Le réseau, c'est comme le code : une fois qu'on comprend la logique, le reste suit naturellement.

**Points clés à retenir :**

1. **Pense en zones** : Infrastructure / DHCP / Réserve
2. **Garde des marges** : 2-3x le besoin actuel
3. **Documente tout** : Ton futur toi te remerciera
4. **Teste systématiquement** : Release/Renew est ton ami
5. **Exclure les IPs fixes** : Évite les conflits

**Le secret** : Commence par un découpage simple (comme ton lab), teste, comprends, puis complexifie progressivement.

Tu as déjà réussi ton TP DHCP, preuve que tu maîtrises les concepts ! 🎉

---

**Document créé par Claude pour Rimk**  
**Formation TSSR - Nextformation**  
**Janvier 2026**
