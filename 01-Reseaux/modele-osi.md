# Modèle OSI et TCP/IP - Le guide du technicien terrain

> 📚 **Module :** Réseaux - Fondamentaux  
> 📅 **Date :** Janvier 2026  
> ⏱️ **Durée :** 8 heures (2 jours)  
> 🎯 **Niveau :** Débutant (formation pour reconversion)  
> 👨‍🏫 **Approche :** Architecte réseau → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Pourquoi ce cours est crucial](#-pourquoi-ce-cours-est-crucial)
- [Le modèle OSI - Les 7 couches](#-le-modèle-osi---les-7-couches)
- [Le modèle TCP/IP - La réalité terrain](#-le-modèle-tcpip---la-réalité-terrain)
- [Comparaison OSI vs TCP/IP](#-comparaison-osi-vs-tcpip)
- [Protocoles essentiels par couche](#-protocoles-essentiels-par-couche)
- [Dépannage par couche](#-dépannage-par-couche)
- [Scénarios réels](#-scénarios-réels)
- [Outils du technicien](#-outils-du-technicien)
- [Exercices pratiques](#-exercices-pratiques)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

Je suis architecte réseau depuis 20 ans. J'ai conçu des infrastructures pour des entreprises de 50 à 5000 employés. J'ai passé mes nuits à dépanner des pannes réseau urgentes. J'ai vu tous les problèmes possibles.

**Pourquoi je vous dis ça ?** Parce que je sais exactement ce dont vous avez besoin pour réussir votre reconversion en tant que TSSR.

### 🎯 La vérité sur le modèle OSI

**Ce qu'on vous dira ailleurs :**  
"Le modèle OSI est un standard de 7 couches définissant comment les systèmes communiquent..."

**Ce que je vais vous dire :**  
"Le modèle OSI est votre GPS pour dépanner un problème réseau. Quand un utilisateur vous appelle parce que 'Internet ne marche pas', le modèle OSI vous dit exactement par où commencer."

### 🔧 Mon approche

Dans ce cours, je ne vais pas vous faire mémoriser des définitions. Je vais vous montrer :

✅ **Comment les pros pensent** quand ils dépannent  
✅ **Les erreurs classiques** (que j'ai faites aussi)  
✅ **Les astuces terrain** qu'on n'apprend pas dans les livres  
✅ **Ce qui arrive vraiment** en entreprise  

### 💬 Pour les reconversions

Si vous venez d'un autre métier, **c'est une force**. Vous avez :
- Une méthodologie de travail
- Une expérience client/utilisateur
- Une capacité à apprendre

Le réseau, ça s'apprend. Je l'ai vu 100 fois. Faites-moi confiance et accrochez-vous aux **2 premiers jours**. Après, ça va devenir naturel.

Allez, on attaque ! 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** les 7 couches OSI à un collègue ou un recruteur
- ✅ **Diagnostiquer** un problème réseau en utilisant la méthode OSI
- ✅ **Identifier** à quelle couche se situe un problème
- ✅ **Utiliser** les bons outils pour chaque couche
- ✅ **Communiquer** efficacement avec les équipes réseau (ils parlent OSI)
- ✅ **Documenter** vos interventions de manière professionnelle

---

## 📋 Prérequis

- [ ] Aucun prérequis technique ! Ce cours part de zéro
- [ ] Avoir un ordinateur avec accès à un terminal
- [ ] Être motivé(e) et curieux(se)
- [ ] Accepter de se tromper (c'est en se trompant qu'on apprend)

**Matériel nécessaire :**
- 💻 PC Windows 10/11 ou Linux
- 🌐 Connexion Internet
- 📝 De quoi prendre des notes

---

## 🔥 Pourquoi ce cours est crucial

### En entretien d'embauche

**Question classique :**  
*"Expliquez-moi le modèle OSI."*

**Mauvaise réponse :**  
"Euh... c'est 7 couches... physique, liaison... je sais plus."

**Bonne réponse :**  
"Le modèle OSI est un référentiel de dépannage. Par exemple, si un utilisateur ne peut pas accéder à un site web, je commence par vérifier la couche 1 (câble branché ?), puis couche 2 (adresse MAC ?), couche 3 (IP et routage ?), etc. Ça me permet de diagnostiquer méthodiquement."

### Sur le terrain

**Scénario réel - 2h du matin, téléphone :**

```
Utilisateur : "JE NE PEUX PAS ENVOYER MES EMAILS ! C'EST URGENT !"
```

**Sans connaître OSI :**  
Vous paniquez. Vous ne savez pas par où commencer.

**Avec OSI :**  
Vous pensez : "Email = Couche 7 (Application). Mais attendez..."

1. **Couche 1** : Le câble réseau est branché ? ✅
2. **Couche 2** : La carte réseau a une adresse MAC ? ✅  
3. **Couche 3** : L'ordinateur a une IP ? ✅ Il ping Google ? ✅
4. **Couche 4** : Le port 25 (SMTP) est ouvert ? ❌ **PROBLÈME TROUVÉ !**

**Temps de résolution : 3 minutes.**

### Dans votre évolution de carrière

Quand vous passerez de TSSR Niveau 1 → Niveau 2 → Admin réseau, le modèle OSI sera votre langage commun avec :
- Les autres techniciens
- Les ingénieurs réseau  
- Les architectes
- Les équipes sécurité

**Maîtriser OSI = Crédibilité professionnelle**

---

## 🏗️ Le modèle OSI - Les 7 couches

### Vue d'ensemble - L'analogie du courrier

Imaginez que vous envoyez une lettre :

| Couche OSI | Analogie courrier | Rôle réel |
|------------|-------------------|-----------|
| **7 - Application** | Écrire la lettre | L'application (Chrome, Outlook) |
| **6 - Présentation** | Traduire en français/anglais | Chiffrement, compression |
| **5 - Session** | Conversation suivie entre 2 personnes | Maintien de la connexion |
| **4 - Transport** | Envelope recommandée ou normale | TCP (fiable) ou UDP (rapide) |
| **3 - Réseau** | Adresse postale sur l'envelope | Adresse IP, routage |
| **2 - Liaison de données** | Facteur qui livre dans votre rue | Adresse MAC, switch |
| **1 - Physique** | Camion, routes | Câbles, ondes WiFi |

### Mnémonique pour retenir l'ordre

**De haut en bas (7→1) :**  
**"A**h **P**etit **S**alaud, **T**u **R**egarderas **L**a **P**hoto !"

- **A**pplication
- **P**résentation  
- **S**ession
- **T**ransport
- **R**éseau
- **L**iaison
- **P**hysique

**De bas en haut (1→7) :**  
**"P**as **L**e **R**éseau **T**op **S**ans **P**rotocoles **A**ctuels"

---

### 🔌 Couche 1 : Physique

#### Rôle
Transporte les **bits** (0 et 1) sous forme de signaux électriques, lumineux ou radio.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Les câbles Ethernet (RJ45)
- Les ondes WiFi
- La fibre optique
- Les connecteurs, ports
- Les répéteurs, hubs

**Problèmes courants :**
- ❌ Câble débranché
- ❌ Câble abîmé (plié, coupé)
- ❌ Port réseau défectueux
- ❌ WiFi trop faible
- ❌ Mauvaise connectique (câble droit au lieu de croisé)

#### 🔧 Diagnostic Couche 1

```bash
# Windows - Vérifier l'état de la carte réseau
ipconfig /all
# Cherchez : "État du média : Média déconnecté" ❌

# Ou dans Panneau de configuration
ncpa.cpl
# La carte est-elle en croix rouge ? ❌
```

**Tests physiques :**
1. Le voyant de la carte réseau est allumé ? 🟢
2. Le câble est bien clipsé des 2 côtés ?
3. Essayer un autre port sur le switch
4. Essayer un autre câble
5. Tester avec un autre ordinateur

#### 💡 Astuce de pro

> "80% des pannes réseau que j'ai vues dans ma carrière étaient couche 1. Un câble débranché par le technicien de ménage, un câble écrasé par une chaise à roulettes, un port switch éteint..."

**Règle d'or :** Toujours commencer par la couche 1. Toujours.

#### 🎓 En entretien

**Question :** "Un utilisateur dit qu'il n'a plus Internet. Par où commencez-vous ?"

**Réponse pro :** "Je commence systématiquement par la couche 1. Je vérifie que le câble est branché, que les voyants de la carte réseau sont allumés, et que Windows détecte bien une connexion. C'est la cause la plus fréquente."

---

### 🔗 Couche 2 : Liaison de données

#### Rôle
Transporte les **trames** entre 2 équipements directement connectés. Utilise les **adresses MAC**.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Adresses MAC (48 bits, ex: `AA:BB:CC:DD:EE:FF`)
- Switches (commutateurs)
- VLAN (séparation logique)
- Protocole ARP (résolution IP → MAC)

**Éléments clés :**
- Chaque carte réseau a une **adresse MAC unique au monde**
- Le switch utilise les MAC pour savoir où envoyer les trames
- ARP fait le lien entre IP (couche 3) et MAC (couche 2)

#### 🔧 Diagnostic Couche 2

```bash
# Windows - Voir l'adresse MAC
ipconfig /all
# Cherchez : "Adresse physique : 00-1A-2B-3C-4D-5E"

# Voir la table ARP (IP vers MAC)
arp -a

# Exemple de sortie :
# 192.168.1.1     00-1a-2b-3c-4d-5e   dynamique
# 192.168.1.10    aa-bb-cc-dd-ee-ff   statique
```

**Problèmes courants :**
- ❌ Conflit d'adresse MAC (très rare, mais ça arrive avec des VM)
- ❌ VLAN mal configuré (l'utilisateur est dans le mauvais VLAN)
- ❌ Port switch désactivé
- ❌ Table ARP corrompue

#### 💡 Astuce de pro

> "Quand un PC a une IP en 169.254.x.x, c'est qu'il n'a pas reçu de réponse du serveur DHCP. Mais pourquoi ? Souvent, c'est un problème de VLAN : le PC est sur le VLAN 10, le DHCP est sur le VLAN 20. Couche 2 !"

#### Cas pratique

**Problème :** Un PC ne peut pas communiquer avec un autre PC sur le même réseau.

**Diagnostic couche 2 :**
```bash
# 1. Vérifier que la carte réseau fonctionne
ipconfig
# IP : 192.168.1.50 ✅

# 2. Vérifier la table ARP
arp -a
# 192.168.1.51 ne figure pas dans la liste ❌

# 3. Tenter de contacter l'autre PC
ping 192.168.1.51
# Requête expirée ❌

# 4. Effacer la table ARP et réessayer
arp -d *
ping 192.168.1.51
```

**Cause possible :** Les 2 PCs sont sur des VLANs différents (problème de configuration switch).

---

### 🌐 Couche 3 : Réseau

#### Rôle
Transporte les **paquets** entre réseaux différents. Utilise les **adresses IP**. Gère le **routage**.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Adresses IP (IPv4 : 192.168.1.10)
- Routeurs
- Protocole IP
- Subnetting
- Passerelle par défaut

**Concept clé : Le routage**

```
PC1 (192.168.1.10)  →  Routeur  →  PC2 (192.168.2.20)
     [Réseau A]                       [Réseau B]
```

Sans routeur, les 2 PCs ne peuvent **pas** communiquer (réseaux différents).

#### 🔧 Diagnostic Couche 3

```bash
# Vérifier la configuration IP
ipconfig
# Cherchez :
# - Adresse IPv4 : 192.168.1.10 ✅
# - Masque : 255.255.255.0 ✅
# - Passerelle : 192.168.1.1 ✅

# Tester la connectivité locale
ping 192.168.1.1
# Si ça fonctionne : Couche 3 OK vers la passerelle ✅

# Tester Internet
ping 8.8.8.8
# Si ça fonctionne : Routage OK ✅

# Si 8.8.8.8 fonctionne mais pas google.com :
# → Problème DNS (toujours couche 3, mais spécifique)
```

**Problèmes courants :**
- ❌ Pas d'adresse IP (DHCP HS)
- ❌ Mauvaise passerelle configurée
- ❌ IP en conflit (2 machines avec la même IP)
- ❌ Masque de sous-réseau incorrect
- ❌ Routage défaillant

#### 💡 Astuce de pro - La méthode du ping

```bash
# Test en 4 étapes (méthode universelle)

# 1. Ping localhost (couche logicielle)
ping 127.0.0.1
# Si ça marche : TCP/IP est bien installé ✅

# 2. Ping de votre propre IP
ping 192.168.1.10  # (votre IP)
# Si ça marche : Votre carte réseau fonctionne ✅

# 3. Ping de la passerelle
ping 192.168.1.1
# Si ça marche : Vous communiquez avec le routeur ✅

# 4. Ping d'une IP Internet
ping 8.8.8.8
# Si ça marche : Internet est accessible ✅

# 5. Ping d'un nom de domaine
ping google.com
# Si ça marche : DNS fonctionne ✅
```

**Si ça bloque à l'étape 3 :**  
→ Problème entre vous et le routeur (couche 2 ou 3)

**Si ça bloque à l'étape 4 :**  
→ Problème de routage ou de connexion Internet

**Si l'étape 4 marche mais pas la 5 :**  
→ Problème DNS uniquement

#### 🎓 En entretien

**Question :** "Un utilisateur peut pinger 8.8.8.8 mais pas google.com. Quel est le problème ?"

**Réponse pro :** "C'est un problème DNS. La couche 3 fonctionne (routage OK), mais la résolution de noms ne marche pas. Je vérifierais la configuration DNS sur le poste : `ipconfig /all`, puis je testerais avec `nslookup google.com` pour voir quel serveur DNS est interrogé."

---

### 📦 Couche 4 : Transport

#### Rôle
Transporte les **segments**. Assure la **fiabilité** (TCP) ou la **rapidité** (UDP). Utilise les **ports**.

#### Ce que vous devez retenir

**2 protocoles principaux :**

| Protocole | Caractéristiques | Utilisation |
|-----------|-----------------|-------------|
| **TCP** | Fiable, avec accusé de réception | Web (HTTP), Email (SMTP), Fichiers (FTP) |
| **UDP** | Rapide, sans garantie | Vidéo en direct, VoIP, DNS, DHCP |

**Concept de port :**

```
Votre PC : 192.168.1.10:54782  →  Google : 142.250.185.46:443 (HTTPS)
           [  IP  :  Port  ]              [   IP    :  Port  ]
```

Le **port** identifie le **service** sur la machine.

#### Ports essentiels à connaître

| Port | Service | Usage |
|------|---------|-------|
| 21 | FTP | Transfert de fichiers |
| 22 | SSH | Connexion sécurisée à distance |
| 23 | Telnet | Connexion non sécurisée (obsolète) |
| 25 | SMTP | Envoi d'emails |
| 53 | DNS | Résolution de noms |
| 80 | HTTP | Sites web (non sécurisé) |
| 110 | POP3 | Réception emails |
| 143 | IMAP | Réception emails (moderne) |
| 443 | HTTPS | Sites web (sécurisé) |
| 445 | SMB | Partage de fichiers Windows |
| 3389 | RDP | Bureau à distance Windows |

#### 🔧 Diagnostic Couche 4

```bash
# Windows - Voir les connexions actives
netstat -an

# Exemple de sortie :
# Proto  Adresse locale         Adresse distante       État
# TCP    192.168.1.10:54782     142.250.185.46:443     ESTABLISHED ✅
# TCP    192.168.1.10:3389      0.0.0.0:0              LISTENING ✅

# Tester si un port est ouvert (avec PowerShell)
Test-NetConnection -ComputerName google.com -Port 443
# TcpTestSucceeded : True ✅

# Tester un port sur votre machine
Test-NetConnection -ComputerName 192.168.1.10 -Port 3389
```

**Problèmes courants :**
- ❌ Pare-feu bloque le port
- ❌ Service non démarré (ex: serveur web éteint)
- ❌ Port utilisé par une autre application
- ❌ Timeout (serveur trop lent à répondre)

#### 💡 Astuce de pro

> "Quand un utilisateur dit 'je ne peux pas accéder au serveur', ne demandez pas juste l'IP. Demandez QUEL service : RDP ? Partage de fichiers ? Web ? Chaque service utilise un port différent. Le problème peut être à la couche 4."

**Exemple réel :**

```
Utilisateur : "Je ne peux pas me connecter au serveur !"
Vous : "Quel service utilisez-vous ?"
Utilisateur : "Bureau à distance."
Vous : "Le port 3389 est-il ouvert ?"
→ Test → Port bloqué par le pare-feu ❌
→ Ouverture du port → Problème résolu ✅
```

---

### 🔄 Couche 5 : Session

#### Rôle
Gère l'**ouverture, le maintien et la fermeture** des sessions de communication.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Maintient une "conversation" entre 2 applications
- Gère les tokens d'authentification
- Synchronise les échanges

**Protocoles utilisant la couche 5 :**
- **NetBIOS** (partages Windows)
- **RPC** (Remote Procedure Call)
- **PPTP** (VPN)

#### 💡 Point important pour les TSSR

> "Honnêtement, vous verrez rarement des problèmes isolés à la couche 5. Si une session ne s'établit pas, c'est souvent un problème couche 4 (port) ou couche 7 (application). Ne perdez pas trop de temps sur cette couche en dépannage."

#### Exemple pratique

**Scénario :** Connexion RDP qui se déconnecte toutes les 5 minutes.

- Ce n'est pas un problème réseau (couche 3) : le ping fonctionne
- Ce n'est pas un problème de port (couche 4) : la connexion s'établit
- C'est un problème de **session** (couche 5) : timeout configuré trop court

**Solution :** Modifier les paramètres de timeout RDP sur le serveur.

---

### 🎨 Couche 6 : Présentation

#### Rôle
**Traduction, chiffrement et compression** des données.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Conversion de formats (ASCII, Unicode)
- Chiffrement SSL/TLS (HTTPS)
- Compression des données

**Exemples :**
- Conversion JPEG → format binaire
- Chiffrement des mots de passe
- Compression d'un email avec pièce jointe

#### 🔧 Diagnostic Couche 6

**Problèmes typiques :**
- ❌ Certificat SSL expiré ou invalide
- ❌ Problème de chiffrement (navigateur ancien)
- ❌ Encodage de caractères incorrect (é → Ã©)

```bash
# Vérifier un certificat SSL
# Dans le navigateur : Clic sur le cadenas 🔒
# Ou avec PowerShell :
$url = "https://google.com"
$req = [Net.HttpWebRequest]::Create($url)
$req.GetResponse() | Out-Null
$req.ServicePoint.Certificate
```

#### 💡 Exemple réel

**Problème :** Un utilisateur voit "ERR_CERT_DATE_INVALID" sur Chrome.

**C'est quoi ?** Problème couche 6 (certificat SSL expiré).

**Solution :**
1. Vérifier la date système du PC (souvent la cause !)
2. Vérifier que le certificat du site est valide
3. Vider le cache SSL : `chrome://net-internals/#sockets`

---

### 🖥️ Couche 7 : Application

#### Rôle
Interface entre l'utilisateur et le réseau. C'est ce que vous **voyez et utilisez**.

#### Ce que vous devez retenir

**C'est quoi concrètement ?**
- Les applications : Chrome, Outlook, Teams, FileZilla
- Les protocoles : HTTP, FTP, SMTP, DNS, DHCP

**Protocoles couche 7 essentiels :**

| Protocole | Usage | Port(s) |
|-----------|-------|---------|
| **HTTP** | Web non sécurisé | 80 |
| **HTTPS** | Web sécurisé | 443 |
| **FTP** | Transfert fichiers | 21, 20 |
| **SMTP** | Envoi emails | 25 |
| **POP3** | Réception emails | 110 |
| **IMAP** | Réception emails (moderne) | 143 |
| **DNS** | Résolution noms | 53 |
| **DHCP** | Attribution IP automatique | 67, 68 |
| **SMB** | Partages Windows | 445 |
| **RDP** | Bureau à distance | 3389 |

#### 🔧 Diagnostic Couche 7

```bash
# Test HTTP
curl http://www.google.com

# Test HTTPS
curl https://www.google.com

# Test DNS
nslookup google.com
# Ou
ping google.com

# Test SMTP (envoi email)
telnet smtp.gmail.com 25

# Test connectivité RDP
mstsc /v:192.168.1.10
```

**Problèmes courants :**
- ❌ Application plantée ou mal configurée
- ❌ Serveur DNS incorrect
- ❌ Certificat HTTPS invalide
- ❌ Authentification refusée (mauvais login/password)

#### 💡 Astuce de pro

> "Si un utilisateur ne peut pas se connecter à un site web spécifique, mais que les autres sites marchent, c'est un problème couche 7 (l'application/le site), pas un problème réseau."

**Tests de différenciation :**

```bash
# Ce site marche ?
ping google.com ✅

# Ce site marche ?
ping site-problematique.com ❌

# Conclusion : Problème avec ce site précis (couche 7)
# ou ce site est bloqué (pare-feu)
```

---

## 🔄 Le modèle TCP/IP - La réalité terrain

### Pourquoi TCP/IP ?

OSI = **Modèle théorique** (années 1980)  
TCP/IP = **Modèle réellement utilisé** sur Internet

### Les 4 couches TCP/IP

| Couche TCP/IP | Équivalent OSI | Description |
|---------------|----------------|-------------|
| **4 - Application** | OSI 5, 6, 7 | HTTP, FTP, SMTP, DNS |
| **3 - Transport** | OSI 4 | TCP, UDP |
| **2 - Internet** | OSI 3 | IP, ICMP, ARP |
| **1 - Accès réseau** | OSI 1, 2 | Ethernet, WiFi |

### Schéma comparatif

```
┌─────────────────┐     ┌─────────────────┐
│   APPLICATION   │ ←→  │ 7 - Application │
│   (TCP/IP)      │     │ 6 - Présentation│
│                 │     │ 5 - Session     │
├─────────────────┤     ├─────────────────┤
│   TRANSPORT     │ ←→  │ 4 - Transport   │
├─────────────────┤     ├─────────────────┤
│   INTERNET      │ ←→  │ 3 - Réseau      │
├─────────────────┤     ├─────────────────┤
│ ACCÈS RÉSEAU    │ ←→  │ 2 - Liaison     │
│                 │     │ 1 - Physique    │
└─────────────────┘     └─────────────────┘
   TCP/IP (4)              OSI (7)
```

### 💡 En pratique

**Dans votre métier de TSSR :**
- Vous utiliserez **OSI pour diagnostiquer** (méthodologie)
- Vous utiliserez **TCP/IP pour comprendre** comment ça marche vraiment

**En entretien :**
- On vous demandera OSI (c'est le standard)
- Mais mentionner TCP/IP montre que vous savez faire le lien avec la réalité

---

## 🔍 Comparaison OSI vs TCP/IP

### Points clés à retenir

| Critère | OSI | TCP/IP |
|---------|-----|--------|
| **Nombre de couches** | 7 | 4 |
| **Approche** | Théorique, académique | Pratique, pragmatique |
| **Utilisation** | Référence pour dépanner | Internet, réseaux réels |
| **Créé par** | ISO | DoD (armée US) |
| **Année** | 1984 | 1970 |

### 💡 Conseil pro

> "Apprenez OSI par cœur. C'est ce qu'on vous demandera en formation et en entretien. Mais comprenez que dans la vraie vie, vous utiliserez TCP/IP. Les 2 sont complémentaires."

---

## 📚 Protocoles essentiels par couche

### Vue d'ensemble

```
┌─────────────────────────────────────────┐
│ COUCHE 7 : HTTP, HTTPS, FTP, SMTP,     │
│            DNS, DHCP, SSH, Telnet       │
├─────────────────────────────────────────┤
│ COUCHE 4 : TCP, UDP                     │
├─────────────────────────────────────────┤
│ COUCHE 3 : IP, ICMP, ARP                │
├─────────────────────────────────────────┤
│ COUCHE 2 : Ethernet, WiFi (802.11)      │
├─────────────────────────────────────────┤
│ COUCHE 1 : Câbles, ondes radio          │
└─────────────────────────────────────────┘
```

### Détails des protocoles clés

#### HTTP / HTTPS (Couche 7)
- **Rôle :** Navigation web
- **Port :** 80 (HTTP), 443 (HTTPS)
- **Commande test :** `curl http://site.com`

#### DNS (Couche 7)
- **Rôle :** Résolution nom → IP
- **Port :** 53 (UDP principalement)
- **Commande test :** `nslookup google.com`

#### DHCP (Couche 7)
- **Rôle :** Attribution automatique d'IP
- **Port :** 67 (serveur), 68 (client)
- **Commande test :** `ipconfig /release` puis `ipconfig /renew`

#### TCP (Couche 4)
- **Rôle :** Transport fiable avec accusé réception
- **Caractéristique :** 3-way handshake (SYN, SYN-ACK, ACK)
- **Usage :** Web, Email, Fichiers

#### UDP (Couche 4)
- **Rôle :** Transport rapide sans garantie
- **Caractéristique :** Pas de vérification
- **Usage :** Streaming, VoIP, DNS

#### IP (Couche 3)
- **Rôle :** Adressage et routage
- **Format :** 192.168.1.10
- **Commande test :** `ping`, `tracert`

#### ICMP (Couche 3)
- **Rôle :** Messages d'erreur et diagnostic
- **Usage :** Commande `ping`, `traceroute`

#### ARP (Couche 2)
- **Rôle :** Résolution IP → MAC
- **Commande :** `arp -a`

#### Ethernet (Couche 2)
- **Rôle :** Communication sur réseau local
- **Format adresse :** MAC (AA:BB:CC:DD:EE:FF)

---

## 🛠️ Dépannage par couche - Méthodologie pro

### La méthode "Bottom-Up" (de bas en haut)

**Règle d'or :** Toujours commencer par la couche 1, puis monter.

```
❓ Problème réseau signalé

↓

🔌 Couche 1 : Câble branché ? Voyants allumés ?
    ✅ OUI → Passer à couche 2
    ❌ NON → Réparer couche 1

↓

🔗 Couche 2 : Adresse MAC présente ? VLAN OK ?
    ✅ OUI → Passer à couche 3
    ❌ NON → Réparer couche 2

↓

🌐 Couche 3 : IP configurée ? Ping passerelle OK ?
    ✅ OUI → Passer à couche 4
    ❌ NON → Réparer couche 3

↓

📦 Couche 4 : Port ouvert ? TCP/UDP fonctionne ?
    ✅ OUI → Passer à couche 7
    ❌ NON → Réparer couche 4

↓

🖥️ Couche 7 : Application lancée ? Bon protocole ?
    ✅ OUI → Problème applicatif
    ❌ NON → Réparer couche 7

↓

✅ Problème résolu !
```

### Checklist de dépannage par couche

#### Couche 1 - Physique ✅

```bash
□ Câble réseau branché des 2 côtés ?
□ Voyant LED vert sur la carte réseau ?
□ Port switch actif (LED switch allumée) ?
□ Essayer un autre câble ?
□ Essayer un autre port switch ?
□ WiFi activé sur le PC ?
□ Signal WiFi suffisant (>50%) ?
```

**Commandes :**
```bash
# Windows
ncpa.cpl  # Voir état carte réseau
```

---

#### Couche 2 - Liaison ✅

```bash
□ Adresse MAC affichée dans ipconfig ?
□ Table ARP contient l'IP de destination ?
□ VLAN correctement configuré ?
□ Switch accessible ?
```

**Commandes :**
```bash
# Voir adresse MAC
ipconfig /all | findstr "Physique"

# Voir table ARP
arp -a

# Effacer table ARP (si corrompue)
arp -d *
```

---

#### Couche 3 - Réseau ✅

```bash
□ Adresse IP configurée (pas 169.254.x.x) ?
□ Masque de sous-réseau correct ?
□ Passerelle par défaut configurée ?
□ DNS configuré ?
□ Ping de la passerelle fonctionne ?
□ Ping de 8.8.8.8 fonctionne ?
□ Ping d'un nom de domaine fonctionne ?
```

**Commandes :**
```bash
# Configuration IP complète
ipconfig /all

# Test passerelle
ping 192.168.1.1

# Test Internet
ping 8.8.8.8

# Test DNS
nslookup google.com

# Renouveler IP (DHCP)
ipconfig /release
ipconfig /renew

# Vider cache DNS
ipconfig /flushdns

# Tracer le chemin réseau
tracert google.com
```

---

#### Couche 4 - Transport ✅

```bash
□ Port destination ouvert ?
□ Pare-feu local autorise le port ?
□ Pare-feu réseau autorise le port ?
□ Service écoutant sur le port ?
```

**Commandes :**
```bash
# Voir ports ouverts localement
netstat -an | findstr LISTENING

# Tester connexion vers un port
Test-NetConnection -ComputerName google.com -Port 443

# Tester port localement
telnet localhost 80
```

---

#### Couche 7 - Application ✅

```bash
□ Application lancée ?
□ Configuration application correcte ?
□ Identifiants corrects ?
□ Serveur distant accessible ?
□ Certificat SSL valide ?
```

**Commandes :**
```bash
# Test HTTP
curl http://site.com

# Test DNS
nslookup site.com

# Vérifier services locaux
services.msc
```

---

## 🔥 Scénarios réels - Cas pratiques

### Scénario 1 : "Internet ne marche pas"

**Contexte :**  
Un utilisateur vous appelle : "Je ne peux plus aller sur Internet !"

**Diagnostic méthodique :**

```bash
# COUCHE 1
1. "Le câble est branché ?"
   → Utilisateur vérifie → OUI ✅

# COUCHE 2
2. ipconfig /all
   → Adresse MAC affichée → OUI ✅

# COUCHE 3
3. ipconfig
   → IP : 169.254.x.x ❌ PROBLÈME !

# Diagnostic : DHCP ne répond pas
```

**Causes possibles :**
- Serveur DHCP éteint
- Câble vers le serveur DHCP débranché
- VLAN mal configuré

**Solution :**
```bash
# Essayer de renouveler l'IP
ipconfig /release
ipconfig /renew

# Si IP en 169.254 persiste :
# → Configurer IP statique temporaire
# → Contacter admin réseau
```

---

### Scénario 2 : "Je ne peux pas accéder au partage réseau"

**Contexte :**  
Un utilisateur ne peut pas accéder à `\\SERVEUR\Partage`

**Diagnostic :**

```bash
# COUCHE 3 - Tester connectivité IP
ping SERVEUR
# Réponse reçue ✅ → Couche 3 OK

# COUCHE 4 - Tester port SMB (445)
Test-NetConnection -ComputerName SERVEUR -Port 445
# TcpTestSucceeded : False ❌ → PROBLÈME COUCHE 4

# Diagnostic : Port 445 bloqué
```

**Causes possibles :**
- Pare-feu bloque le port 445
- Service "Serveur" arrêté sur le serveur

**Solution :**
```bash
# Sur le serveur, vérifier le service
Get-Service -Name LanmanServer
# Si arrêté :
Start-Service LanmanServer

# Vérifier pare-feu
netsh advfirewall firewall show rule name=all | findstr 445
```

---

### Scénario 3 : "Le site web affiche une erreur"

**Contexte :**  
Un site affiche "ERR_CONNECTION_TIMED_OUT"

**Diagnostic :**

```bash
# COUCHE 3 - IP accessible ?
ping www.site.com
# Réponse reçue ✅

# COUCHE 4 - Port 443 ouvert ?
Test-NetConnection -ComputerName www.site.com -Port 443
# TcpTestSucceeded : True ✅

# COUCHE 7 - Requête HTTP
curl https://www.site.com
# Erreur : "SSL certificate problem" ❌

# Diagnostic : Problème certificat (Couche 6/7)
```

**Causes possibles :**
- Certificat SSL expiré
- Horloge système incorrecte (!)

**Solution :**
```bash
# Vérifier date/heure système
date
time

# Si incorrecte, corriger :
# Windows : Paramètres → Heure et langue
```

---

### Scénario 4 : "Connexion RDP impossible"

**Contexte :**  
Impossible de se connecter en bureau à distance à un serveur

**Diagnostic :**

```bash
# COUCHE 3
ping 192.168.1.10
# OK ✅

# COUCHE 4
Test-NetConnection -ComputerName 192.168.1.10 -Port 3389
# ÉCHEC ❌

# Causes possibles :
# 1. RDP désactivé sur le serveur
# 2. Port 3389 bloqué par pare-feu
# 3. Service "Bureau à distance" arrêté
```

**Solution :**
```bash
# Sur le serveur :
# 1. Activer RDP
SystemPropertiesRemote
# Cocher "Autoriser les connexions..."

# 2. Vérifier le service
Get-Service -Name TermService
# Si arrêté :
Start-Service TermService

# 3. Pare-feu
netsh advfirewall firewall add rule name="RDP" dir=in action=allow protocol=TCP localport=3389
```

---

## 🧰 Outils du technicien TSSR

### Outils Windows essentiels

| Outil | Commande | Usage | Couche |
|-------|----------|-------|--------|
| **ping** | `ping google.com` | Test connectivité | 3 |
| **ipconfig** | `ipconfig /all` | Voir config réseau | 3 |
| **nslookup** | `nslookup google.com` | Test DNS | 7 |
| **tracert** | `tracert google.com` | Tracer chemin réseau | 3 |
| **netstat** | `netstat -an` | Voir connexions actives | 4 |
| **arp** | `arp -a` | Voir table ARP | 2 |
| **route** | `route print` | Voir table de routage | 3 |
| **Test-NetConnection** | `Test-NetConnection -Port 443` | Test port | 4 |

### Outils graphiques Windows

```bash
# Gestionnaire réseau
ncpa.cpl

# Moniteur de ressources
resmon
# Onglet Réseau → Voir toutes les connexions

# Gestionnaire des tâches
taskmgr
# Onglet Performances → Réseau

# Pare-feu
wf.msc
```

### Outils en ligne

- **[WhatIsMyIP](https://www.whatismyip.com/)** - Voir votre IP publique
- **[DNS Checker](https://dnschecker.org/)** - Vérifier propagation DNS
- **[Down For Everyone](https://downforeveryoneorjustme.com/)** - Site en panne ?
- **[Speedtest](https://www.speedtest.net/)** - Test débit

### Outils avancés (bonus)

- **Wireshark** - Capture et analyse de paquets
- **Nmap** - Scan de ports
- **PuTTY** - Client SSH/Telnet
- **FileZilla** - Client FTP

---

## 🎯 Exercices pratiques

### Exercice 1 : Identifier la couche du problème

**Pour chaque scénario, identifiez la couche OSI concernée :**

1. Un câble Ethernet est débranché → Couche ?
2. Un PC a l'IP 169.254.10.5 → Couche ?
3. Le port 80 est bloqué par le pare-feu → Couche ?
4. `ping google.com` fonctionne mais pas le navigateur → Couche ?
5. La table ARP est vide → Couche ?
6. Le certificat SSL est expiré → Couche ?

**Solution :**

<details>
<summary>Cliquez pour voir les réponses</summary>

1. **Couche 1** (Physique) - Problème matériel
2. **Couche 3** (Réseau) - DHCP n'a pas attribué d'IP
3. **Couche 4** (Transport) - Problème de port
4. **Couche 7** (Application) - DNS ou problème navigateur
5. **Couche 2** (Liaison) - Table MAC/IP vide
6. **Couche 6** (Présentation) - Problème chiffrement

</details>

---

### Exercice 2 : Diagnostic complet

**Scénario :**  
Un utilisateur ne peut pas accéder à `http://intranet.local`

**Effectuez un diagnostic complet en utilisant la méthode OSI :**

<details>
<summary>Cliquez pour voir la démarche</summary>

```bash
# COUCHE 1
1. Vérifier câble branché
   → cmd : ncpa.cpl
   → Voyant vert ? ✅

# COUCHE 2
2. Vérifier adresse MAC
   → cmd : ipconfig /all
   → Adresse physique présente ? ✅

# COUCHE 3
3. Vérifier IP
   → cmd : ipconfig
   → IP : 192.168.1.50 ✅
   → Ping passerelle : ping 192.168.1.1 ✅
   → Ping intranet : ping intranet.local ❌

# DIAGNOSTIC : DNS ne résout pas "intranet.local"

4. Vérifier DNS
   → cmd : nslookup intranet.local
   → Erreur : "Serveur inconnu" ❌

# CAUSE : Configuration DNS incorrecte

# SOLUTION
ipconfig /all
# Serveur DNS : 8.8.8.8 ❌ (DNS public, pas intranet)
# Il faut le DNS interne : 192.168.1.2

# Corriger configuration DNS :
# Paramètres → Réseau → Propriétés → IPv4
# DNS préféré : 192.168.1.2

# Vider cache DNS
ipconfig /flushdns

# Retester
nslookup intranet.local
# → IP retournée : 192.168.1.100 ✅

# Test final
ping intranet.local ✅
# Ouvrir navigateur : http://intranet.local ✅
```

</details>

---

### Exercice 3 : À vous de jouer !

**Pratiquez sur votre propre PC :**

```bash
# 1. Affichez votre configuration IP complète
ipconfig /all

# 2. Identifiez :
#    - Votre IP
#    - Votre masque
#    - Votre passerelle
#    - Votre DNS

# 3. Testez la connectivité
ping 8.8.8.8
ping google.com

# 4. Affichez vos connexions actives
netstat -an

# 5. Trouvez quelle application utilise le port 443
netstat -ano | findstr :443

# 6. Videz votre cache DNS
ipconfig /flushdns

# 7. Affichez votre table ARP
arp -a
```

---

## 📚 Ressources complémentaires

### Vidéos recommandées

- [Modèle OSI en 7 minutes](https://www.youtube.com/watch?v=example) (cherchez sur YouTube)
- [TCP/IP Explained - NetworkChuck](https://www.youtube.com/watch?v=example)
- [Wireshark Tutorial](https://www.youtube.com/watch?v=example)

### Documentation officielle

- [RFC 1122 - Internet Protocol](https://tools.ietf.org/html/rfc1122)
- [Cisco - OSI Model](https://www.cisco.com/c/en/us/support/docs/lanwan/ethernet/lan/osi-model.html)

### Livres

- "Réseaux" - Andrew S. Tanenbaum (⭐⭐⭐⭐⭐)
- "TCP/IP Illustrated" - W. Richard Stevens
- "Cisco CCNA - Routing et Switching" - Todd Lammle

### Sites web

- [NetworkLessons.com](https://networklessons.com) - Tutoriels gratuits
- [PacketLife.net](https://packetlife.net) - Cheat sheets réseau
- [SubReddit r/networking](https://reddit.com/r/networking)

---

## ✅ Checklist de révision

Avant de passer au cours suivant, vous devez être capable de :

### OSI - Connaissance théorique
- [ ] Réciter les 7 couches dans l'ordre
- [ ] Expliquer le rôle de chaque couche en 1 phrase
- [ ] Donner 2 protocoles par couche (3, 4, 7)
- [ ] Expliquer la différence TCP vs UDP

### TCP/IP - Connaissance pratique
- [ ] Expliquer les 4 couches TCP/IP
- [ ] Faire le lien entre OSI et TCP/IP
- [ ] Citer 5 protocoles couche application

### Diagnostic - Compétence terrain
- [ ] Appliquer la méthode "Bottom-Up"
- [ ] Utiliser ping, ipconfig, nslookup
- [ ] Identifier la couche d'un problème donné
- [ ] Documenter un diagnostic de A à Z

### En entretien
- [ ] Expliquer OSI en 2 minutes
- [ ] Donner un exemple de dépannage par couche
- [ ] Répondre à "TCP ou UDP pour la vidéo ?"

---

## 🎓 Message final de votre formateur

**Bravo d'être arrivé(e) jusqu'ici !** 👏

Ce cours était dense, je le sais. Mais vous avez maintenant une **compétence clé** pour votre métier de TSSR.

### Ce que vous avez appris

✅ Le modèle OSI (votre GPS de dépannage)  
✅ Le modèle TCP/IP (la réalité terrain)  
✅ Les protocoles essentiels  
✅ La méthodologie de diagnostic  
✅ Les outils du technicien  

### Mes 3 conseils pour progresser

1. **Pratiquez sur votre PC**  
   Lancez les commandes, cassez des trucs, réparez. C'est en faisant qu'on apprend.

2. **Documentez vos interventions**  
   À chaque problème résolu, notez : Symptôme → Diagnostic → Solution. Créez votre base de connaissance.

3. **Ne paniquez jamais**  
   Avec la méthode OSI, vous avez un processus. Couche par couche. Méthodiquement. Vous ALLEZ trouver.

### La suite

Le cours suivant est **"Adressage IP et Subnetting"**. Il approfondit la couche 3 (Réseau). Vous y apprendrez à calculer des sous-réseaux, comprendre les masques, etc.

Ensuite, vous attaquerez **Active Directory** (couches 5-7). Vous verrez, tout ce qu'on a appris ici prendra son sens.

### Un dernier mot

Le réseau, c'est comme la cuisine : au début, les recettes paraissent compliquées. Mais après avoir fait 10 fois le même plat, ça devient automatique.

Dans 3 mois, vous diagnostiquerez un problème réseau en 2 minutes sans même y réfléchir. Vous verrez.

**Alors on continue ? 💪**

---

<div align="center">

### 🚀 "Dans le doute, pingue. C'est toujours la couche 1."

**Cours suivant :** [Adressage IP et Subnetting](./adressage-ip-subnetting.md)

[⬅️ Retour au sommaire](../../README.md)

---

**Bon courage pour votre reconversion TSSR !**

*"Le réseau n'a pas de secret, juste des couches à explorer."* 🎓

</div>