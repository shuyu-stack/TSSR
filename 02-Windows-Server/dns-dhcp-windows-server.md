# DNS et DHCP sous Windows Server

> 📚 **Module :** Windows Server - Services réseau fondamentaux
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 6-8 heures
> 🎯 **Niveau :** Fondamental (CRITIQUE pour examen + emploi)
> 🎓 **Formateur virtuel :** Architecte réseau avec +20 ans d'expérience

---

## 👨‍🏫 Message de votre formateur

> **Écoutez bien, c'est VITAL :**
>
> En 20 ans de métier, j'ai vu des milliers de tickets de support. **80% des problèmes réseau = problème DNS**. Pas de connexion internet ? DNS. Le serveur ne répond pas ? DNS. Les emails ne partent pas ? DNS.
>
> **DNS et DHCP = les 2 piliers de TOUT réseau moderne.** Si vous ne maîtrisez pas ces 2 services, vous allez galérer TOUS LES JOURS en poste.
>
> À l'examen TSSR, il y a **90% de chances** d'avoir un exercice sur DNS/DHCP. C'est quasi-certain.
>
> Ce cours va vous apprendre l'essentiel pour :
> - ✅ Configurer DNS et DHCP en moins de 30 minutes
> - ✅ Diagnostiquer et corriger 95% des problèmes courants
> - ✅ Éviter les erreurs classiques qui bloquent tout le réseau
> - ✅ Réussir l'exercice pratique de l'examen

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction - Pourquoi DNS et DHCP sont essentiels](#-introduction)
- [Partie 1 : DNS (Domain Name System)](#-partie-1--dns-domain-name-system)
  - [Les bases du DNS](#les-bases-du-dns)
  - [Installation du rôle DNS](#installation-du-rôle-dns)
  - [Configuration d'une zone DNS](#configuration-dune-zone-dns)
  - [Les enregistrements DNS](#les-enregistrements-dns)
  - [Diagnostic DNS](#diagnostic-dns)
- [Partie 2 : DHCP (Dynamic Host Configuration Protocol)](#-partie-2--dhcp)
  - [Les bases du DHCP](#les-bases-du-dhcp)
  - [Installation du rôle DHCP](#installation-du-rôle-dhcp)
  - [Configuration d'un scope DHCP](#configuration-dun-scope-dhcp)
  - [Réservations DHCP](#réservations-dhcp)
  - [Diagnostic DHCP](#diagnostic-dhcp)
- [Partie 3 : Intégration DNS + DHCP + Active Directory](#-partie-3--intégration-dns--dhcp--active-directory)
- [Dépannage courant](#-dépannage-courant)
- [Astuces de pro](#-astuces-de-pro)
- [Pièges à éviter](#-pièges-à-éviter)
- [Exercices pratiques](#-exercices-pratiques)
- [Checklist pour l'examen](#-checklist-pour-lexamen)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** le rôle du DNS et du DHCP dans un réseau d'entreprise
- ✅ **Installer** et **configurer** un serveur DNS sous Windows Server
- ✅ **Créer** des zones DNS (directes et inverses)
- ✅ **Gérer** les enregistrements DNS (A, AAAA, CNAME, MX, PTR, SRV)
- ✅ **Installer** et **configurer** un serveur DHCP
- ✅ **Créer** des scopes DHCP avec plages IP, réservations, options
- ✅ **Diagnostiquer** et **corriger** les problèmes DNS/DHCP les plus courants
- ✅ **Intégrer** DNS et DHCP dans un domaine Active Directory
- ✅ **Réussir** l'exercice pratique à l'examen TSSR

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [x] Avoir installé Windows Server 2019/2022/2025
- [x] Avoir configuré une adresse IP statique sur votre serveur
- [x] Comprendre les bases du modèle TCP/IP
- [x] Comprendre le subnetting et les masques de sous-réseau
- [ ] *Recommandé :* Avoir un contrôleur de domaine Active Directory configuré

**Matériel nécessaire :**
- 💻 Windows Server 2019/2022/2025 (VM ou physique)
- 🌐 Réseau local fonctionnel (peut être virtuel)
- 🖥️ 1-2 postes clients Windows 10/11 pour tester
- 📝 Bloc-notes pour noter les commandes importantes

---

## 📚 Introduction

### Pourquoi DNS et DHCP sont les services les plus importants ?

Imaginez votre réseau comme une ville :

🏙️ **DNS = L'annuaire téléphonique de la ville**
- Vous connaissez le nom de votre ami (www.google.com)
- Mais vous avez besoin de son adresse postale (IP : 142.250.185.78)
- Le DNS fait cette traduction nom → IP

🏠 **DHCP = Le bureau d'attribution des adresses**
- Quand un nouveau résident arrive en ville (un PC se connecte au réseau)
- Il a besoin d'une adresse postale (adresse IP)
- Le DHCP lui attribue automatiquement une adresse disponible

### En entreprise, voici ce qui se passe SANS DNS/DHCP :

❌ **Sans DNS :**
- Vous devez mémoriser les IP de tous les serveurs (impossible !)
- Pas d'accès aux sites web (vous tapez google.com → ça ne marche pas)
- Active Directory ne fonctionne pas (il utilise massivement le DNS)
- Les emails ne partent/arrivent pas

❌ **Sans DHCP :**
- Vous devez configurer MANUELLEMENT l'IP de chaque PC (1000 postes = cauchemar)
- Conflits d'IP fréquents (2 machines avec la même IP = plantage)
- Erreur de configuration = pas de réseau
- Perte de temps monstre (5-10 min par poste)

### Pourquoi c'est CRITIQUE pour l'examen ?

À l'examen TSSR, vous aurez **quasi-certainement** :
- ✅ Un exercice : "Installez et configurez DNS/DHCP sur ce serveur"
- ✅ Un problème de réseau à diagnostiquer (souvent lié au DNS)
- ✅ Des questions théoriques sur les enregistrements DNS

**Si vous ratez cette partie, vous perdez 20-30% des points. C'est énorme.**

### Pourquoi c'est CRITIQUE en entreprise ?

**Statistique personnelle (20 ans de terrain) :**
- 📊 **80% des tickets réseau** = problème DNS
- 📊 **50% des tickets support niveau 1** = "pas de réseau" → souvent DHCP
- 📊 **100% des infrastructures Windows** utilisent DNS + DHCP

**Exemples réels de tickets que j'ai traités :**

| Ticket | Cause réelle | Solution |
|--------|--------------|----------|
| "Internet ne marche plus sur tout le bâtiment" | Serveur DNS planté | Redémarrer service DNS (30 secondes) |
| "Les nouveaux PC n'ont pas de réseau" | Scope DHCP plein | Étendre la plage IP (5 minutes) |
| "Les utilisateurs ne peuvent plus se connecter au domaine" | Enregistrement SRV DNS manquant | Forcer l'enregistrement AD dans DNS |
| "Les emails ne partent plus" | Enregistrement MX DNS incorrect | Corriger l'enregistrement MX |

> 💡 **Conseil d'expert :**
> En tant que TSSR junior, si vous maîtrisez DNS et DHCP, vous allez résoudre **80% de vos tickets** plus vite que vos collègues. Ça fait la différence en entretien d'embauche !

---

## 🔷 Partie 1 : DNS (Domain Name System)

### Les bases du DNS

#### Qu'est-ce que le DNS ?

Le **DNS** (Domain Name System) est un **annuaire distribué** qui traduit les noms de domaine (lisibles par les humains) en adresses IP (utilisées par les machines).

**Analogie simple :**
- Vous tapez **www.microsoft.com** dans votre navigateur
- Votre PC demande au DNS : "C'est quoi l'IP de microsoft.com ?"
- Le DNS répond : "C'est **20.112.52.29**"
- Votre PC se connecte à cette IP

#### Les composants du DNS

1. **Serveur DNS** : Machine qui répond aux requêtes DNS
2. **Zone DNS** : Base de données contenant les enregistrements pour un domaine
3. **Enregistrements DNS** : Lignes dans la base (ex: "serveur1 = 192.168.1.10")
4. **Client DNS** : Votre PC qui fait des requêtes DNS

#### Hiérarchie DNS

```
                    . (racine)
                    |
        ┌───────────┼───────────┐
        |           |           |
       .com        .fr         .org
        |
    microsoft.com
        |
    www.microsoft.com
```

#### Types de zones DNS

1. **Zone de recherche directe** (Forward Lookup Zone)
   - Nom → IP
   - Exemple : "serveur1.entreprise.local" → "192.168.1.10"

2. **Zone de recherche inversée** (Reverse Lookup Zone)
   - IP → Nom
   - Exemple : "192.168.1.10" → "serveur1.entreprise.local"
   - Moins utilisée mais importante pour certains services (emails notamment)

---

### Installation du rôle DNS

#### Méthode 1 : Interface graphique (Server Manager)

**Étape 1 : Ouvrir Server Manager**
1. Ouvrez **Server Manager** (se lance automatiquement au démarrage)
2. Cliquez sur **Manage** (en haut à droite)
3. Sélectionnez **Add Roles and Features**

**Étape 2 : Installation**
1. Cliquez **Next** jusqu'à "Server Roles"
2. Cochez **DNS Server**
3. Cliquez **Add Features** (pour ajouter les outils)
4. Cliquez **Next** → **Next** → **Install**
5. Attendez la fin de l'installation (2-3 minutes)
6. Cliquez **Close**

**Étape 3 : Vérification**
1. Dans Server Manager, cliquez sur **Tools** (en haut à droite)
2. Vous devez voir **DNS** dans la liste
3. Cliquez sur **DNS** pour ouvrir la console de gestion

✅ **C'est fait ! Votre serveur DNS est installé.**

#### Méthode 2 : PowerShell (RAPIDE - méthode de pro)

```powershell
# Installer le rôle DNS (une seule commande !)
Install-WindowsFeature -Name DNS -IncludeManagementTools

# Vérifier l'installation
Get-WindowsFeature DNS
```

**Résultat attendu :**
```
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[X] DNS Server                                          DNS                        Installed
```

> 💡 **Astuce de pro :**
> En production, on utilise TOUJOURS PowerShell. C'est plus rapide et ça peut être automatisé. Apprenez les commandes PowerShell dès maintenant, vous gagnerez un temps fou !

---

### Configuration d'une zone DNS

Maintenant que DNS est installé, on va créer notre première **zone de recherche directe**.

**Scénario :** Vous gérez le réseau de l'entreprise **SOLARIS.local**. Vous devez créer la zone DNS pour ce domaine.

#### Méthode 1 : Interface graphique

**Étape 1 : Ouvrir la console DNS**
1. Server Manager → **Tools** → **DNS**
2. Dans le volet de gauche, dépliez le nom de votre serveur

**Étape 2 : Créer une zone de recherche directe**
1. Clic droit sur **Forward Lookup Zones**
2. Cliquez sur **New Zone...**
3. Assistant de création :
   - Type : **Primary zone** (zone principale)
   - Cochez **Store the zone in Active Directory** (si vous avez AD)
   - Zone Name : Tapez **SOLARIS.local**
   - Dynamic Update : Sélectionnez **Allow only secure dynamic updates** (si AD) ou **Do not allow dynamic updates** (sinon)
4. Cliquez **Finish**

✅ **Votre zone DNS est créée !**

**Étape 3 : Vérification**
- Dépliez **Forward Lookup Zones**
- Vous devez voir **SOLARIS.local**
- À l'intérieur, vous voyez déjà 2 enregistrements automatiques :
  - `SOA` (Start of Authority)
  - `NS` (Name Server)

#### Méthode 2 : PowerShell

```powershell
# Créer une zone DNS primaire
Add-DnsServerPrimaryZone -Name "SOLARIS.local" -ReplicationScope "Forest" -DynamicUpdate "Secure"

# Vérifier la création
Get-DnsServerZone -Name "SOLARIS.local"
```

> 💡 **Astuce de pro :**
> Dans un environnement Active Directory, utilisez **TOUJOURS** "Allow only secure dynamic updates". Ça permet aux machines du domaine d'enregistrer automatiquement leur nom dans le DNS. C'est magique !

---

### Les enregistrements DNS

Les **enregistrements DNS** sont les lignes de votre "annuaire". Chaque type d'enregistrement a un rôle spécifique.

#### Les enregistrements DNS les plus importants

| Type | Nom complet | Utilité | Exemple |
|------|-------------|---------|---------|
| **A** | Address | Nom d'hôte → IPv4 | serveur1.solaris.local → 192.168.1.10 |
| **AAAA** | IPv6 Address | Nom d'hôte → IPv6 | serveur1.solaris.local → 2001:db8::1 |
| **CNAME** | Canonical Name | Alias (nom alternatif) | www → serveur1.solaris.local |
| **MX** | Mail Exchange | Serveur de messagerie | mail.solaris.local (priorité 10) |
| **PTR** | Pointer | IP → Nom (zone inversée) | 192.168.1.10 → serveur1.solaris.local |
| **SRV** | Service | Localisation de services | _ldap._tcp → DC1.solaris.local |
| **NS** | Name Server | Serveur DNS autoritaire | solaris.local → ns1.solaris.local |
| **SOA** | Start of Authority | Info sur la zone | Zone principale, TTL, etc. |

#### 🎯 Focus sur les 4 enregistrements que vous utiliserez à 90%

##### 1. Enregistrement A (Address) - Le plus utilisé

**C'est quoi ?** Associe un nom d'hôte à une adresse IPv4.

**Exemple concret :**
```
serveur1.solaris.local  →  192.168.1.10
pc-comptable.solaris.local  →  192.168.1.50
```

**Créer un enregistrement A (GUI) :**
1. Console DNS → Forward Lookup Zones → SOLARIS.local
2. Clic droit dans le volet de droite → **New Host (A or AAAA)...**
3. Name : `serveur1`
4. IP Address : `192.168.1.10`
5. Cochez **Create associated pointer (PTR) record** (recommandé)
6. Cliquez **Add Host**

**Créer un enregistrement A (PowerShell) :**
```powershell
# Ajouter un enregistrement A
Add-DnsServerResourceRecordA -Name "serveur1" -ZoneName "SOLARIS.local" -IPv4Address "192.168.1.10"

# Vérifier
Get-DnsServerResourceRecord -ZoneName "SOLARIS.local" -Name "serveur1"
```

##### 2. Enregistrement CNAME (Canonical Name) - Les alias

**C'est quoi ?** Crée un "surnom" pour un serveur existant.

**Exemple concret :**
```
www.solaris.local  →  serveur1.solaris.local
intranet.solaris.local  →  serveur1.solaris.local
```

**Pourquoi c'est utile ?**
- Un serveur, plusieurs noms (www, intranet, ftp...)
- Si vous changez l'IP du serveur, vous ne modifiez que l'enregistrement A (pas tous les CNAME)

**Créer un CNAME (GUI) :**
1. Console DNS → SOLARIS.local
2. Clic droit → **New Alias (CNAME)...**
3. Alias name : `www`
4. FQDN : `serveur1.solaris.local`
5. OK

**Créer un CNAME (PowerShell) :**
```powershell
Add-DnsServerResourceRecordCName -Name "www" -ZoneName "SOLARIS.local" -HostNameAlias "serveur1.solaris.local"
```

##### 3. Enregistrement MX (Mail Exchange) - Les emails

**C'est quoi ?** Indique quel serveur gère les emails du domaine.

**Exemple concret :**
```
SOLARIS.local  →  mail.solaris.local (priorité 10)
```

**Créer un MX (GUI) :**
1. Console DNS → SOLARIS.local
2. Clic droit → **New Mail Exchanger (MX)...**
3. Host or child domain : laissez vide (pour le domaine entier)
4. Mail server : `mail.solaris.local`
5. Priority : `10` (plus bas = plus prioritaire)
6. OK

##### 4. Enregistrement PTR (Pointer) - Résolution inverse

**C'est quoi ?** L'inverse de A : traduit une IP en nom.

**Exemple concret :**
```
192.168.1.10  →  serveur1.solaris.local
```

**Pourquoi c'est important ?**
- Requis pour certains serveurs email (anti-spam)
- Utilisé pour les logs et l'audit

**Créer une zone de recherche inversée :**
1. DNS → Clic droit sur **Reverse Lookup Zones** → **New Zone...**
2. Primary zone
3. IPv4 Reverse Lookup Zone
4. Network ID : `192.168.1` (les 3 premiers octets)
5. Dynamic Update : Secure
6. Finish

Les PTR se créent automatiquement quand vous cochez "Create PTR" lors de la création d'un enregistrement A.

---

### Diagnostic DNS

En entreprise, vous passerez **50% de votre temps** à diagnostiquer des problèmes DNS. Voici les commandes essentielles.

#### 🔧 Commandes de diagnostic Windows

##### 1. `ipconfig /all` - Voir la config DNS du PC

```cmd
ipconfig /all
```

**Sortie importante :**
```
DNS Servers . . . . . . . . . . . : 192.168.1.10
                                     8.8.8.8
```

> 💡 Le premier DNS doit être votre serveur interne !

##### 2. `ipconfig /flushdns` - Vider le cache DNS

```cmd
ipconfig /flushdns
```

**Quand l'utiliser ?**
- Vous avez modifié un enregistrement DNS
- Le PC utilise encore l'ancienne IP
- → Vider le cache force une nouvelle requête

> ⚠️ **90% des problèmes DNS "bizarres" = cache pas vidé !**

##### 3. `nslookup` - Interroger le DNS

```cmd
# Résoudre un nom
nslookup serveur1.solaris.local

# Résoudre avec un DNS spécifique
nslookup serveur1.solaris.local 192.168.1.10

# Recherche inversée
nslookup 192.168.1.10
```

**Exemple de sortie :**
```
Server:  dns1.solaris.local
Address:  192.168.1.10

Name:    serveur1.solaris.local
Address:  192.168.1.10
```

##### 4. `Test-NetConnection` - PowerShell (plus puissant)

```powershell
# Tester la résolution DNS + connexion
Test-NetConnection serveur1.solaris.local -Port 80

# Résoudre seulement le DNS
Resolve-DnsName serveur1.solaris.local
```

#### 🔧 Commandes de diagnostic sur le serveur DNS

##### Vérifier que le service DNS tourne

```powershell
# Voir le statut du service
Get-Service DNS

# Redémarrer le service DNS (si problème)
Restart-Service DNS
```

##### Lister tous les enregistrements d'une zone

```powershell
Get-DnsServerResourceRecord -ZoneName "SOLARIS.local"
```

##### Vérifier les logs DNS

**GUI :**
1. Console DNS → Serveur → Monitoring
2. Cochez "A simple query against this DNS server"
3. Cliquez **Test Now**
4. Résultat : PASS (✅) ou FAIL (❌)

**PowerShell :**
```powershell
# Tester les requêtes DNS
Resolve-DnsName google.com -Server localhost
```

---

## 🔶 Partie 2 : DHCP (Dynamic Host Configuration Protocol)

### Les bases du DHCP

#### Qu'est-ce que le DHCP ?

Le **DHCP** attribue automatiquement une **configuration réseau** aux machines qui se connectent.

**Sans DHCP :**
```
Nouvel employé arrive avec son PC
→ Vous devez :
  1. Trouver une IP libre (192.168.1.???)
  2. Configurer manuellement : IP, masque, passerelle, DNS
  3. Risque d'erreur = pas de réseau
  4. Temps : 5-10 minutes
```

**Avec DHCP :**
```
Nouvel employé branche son PC
→ DHCP fait tout automatiquement en 5 secondes
→ Le PC obtient : IP, masque, passerelle, DNS
→ Ça marche du premier coup
```

#### Le processus DHCP (DORA)

Le DHCP utilise un échange en 4 étapes appelé **DORA** :

```
Client                                 Serveur DHCP
  |                                          |
  |------- DISCOVER (broadcast) ----------->|  "Je cherche un serveur DHCP !"
  |                                          |
  |<------- OFFER (unicast) ----------------|  "J'ai une IP pour toi : 192.168.1.50"
  |                                          |
  |------- REQUEST (broadcast) ------------>|  "OK, je prends cette IP !"
  |                                          |
  |<------- ACK (unicast) ------------------|  "Confirmé, l'IP est à toi pour 8 jours"
  |                                          |
```

- **D**iscover : Le client cherche un serveur DHCP
- **O**ffer : Le serveur propose une IP
- **R**equest : Le client demande officiellement cette IP
- **A**cknowledge : Le serveur confirme

> 💡 **Astuce de pro :**
> En dépannage, si un PC n'obtient pas d'IP, vérifiez l'étape où ça bloque :
> - DISCOVER pas reçu → problème réseau (câble, switch)
> - OFFER pas envoyé → serveur DHCP éteint ou scope plein
> - ACK pas reçu → conflit d'IP

#### Les composants du DHCP

1. **Serveur DHCP** : Machine qui distribue les IP
2. **Scope** : Plage d'adresses IP disponibles (ex: 192.168.1.100 à 192.168.1.200)
3. **Bail** (Lease) : Durée de validité d'une IP (ex: 8 jours)
4. **Réservation** : IP fixe attribuée à une machine spécifique (via adresse MAC)
5. **Options DHCP** : Paramètres supplémentaires (passerelle, DNS, domaine...)

---

### Installation du rôle DHCP

#### Méthode 1 : Interface graphique

**Étape 1 : Installation**
1. Server Manager → **Manage** → **Add Roles and Features**
2. Sélectionnez **DHCP Server**
3. **Add Features** → **Next** → **Install**
4. Attendez la fin (2-3 min)
5. **Close**

**Étape 2 : Configuration post-installation (IMPORTANT !)**

Après l'installation, vous devez "autoriser" le serveur DHCP dans Active Directory.

1. Server Manager → Notification (drapeau jaune en haut)
2. Cliquez sur **Complete DHCP configuration**
3. **Commit** (si vous êtes admin du domaine)
4. **Close**

> ⚠️ **PIÈGE CLASSIQUE :**
> Beaucoup de débutants oublient cette étape. Résultat : le serveur DHCP est installé mais ne distribue PAS d'IP ! Pensez toujours à "autoriser" le serveur après installation.

#### Méthode 2 : PowerShell

```powershell
# Installer DHCP
Install-WindowsFeature DHCP -IncludeManagementTools

# Autoriser le serveur DHCP dans AD (remplacez par votre nom de serveur)
Add-DhcpServerInDC -DnsName "DC1.solaris.local" -IPAddress 192.168.1.10

# Vérifier
Get-DhcpServerInDC
```

---

### Configuration d'un scope DHCP

Un **scope** = une plage d'adresses IP que le DHCP va distribuer.

**Scénario :** Vous gérez le réseau 192.168.1.0/24. Vous voulez :
- Plage DHCP : 192.168.1.100 à 192.168.1.200 (101 adresses)
- IP statiques manuelles : 192.168.1.1 à 192.168.1.99 (pour serveurs, imprimantes)
- Passerelle : 192.168.1.1
- DNS : 192.168.1.10
- Durée de bail : 8 jours

#### Méthode 1 : Interface graphique

**Étape 1 : Ouvrir la console DHCP**
1. Server Manager → **Tools** → **DHCP**
2. Dépliez le nom de votre serveur
3. Dépliez **IPv4**

**Étape 2 : Créer un nouveau scope**
1. Clic droit sur **IPv4** → **New Scope...**
2. Assistant :
   - **Name** : `LAN Principal 192.168.1.0/24`
   - **Description** : `Plage DHCP pour le réseau principal`
   - **Start IP** : `192.168.1.100`
   - **End IP** : `192.168.1.200`
   - **Length** : 24 (masque 255.255.255.0)
   - **Subnet mask** : 255.255.255.0
3. **Next**

**Étape 3 : Exclusions (optionnel)**
- Si vous voulez exclure certaines IP de la plage (ex: pour des réservations futures)
- Exemple : exclure 192.168.1.100 à 192.168.1.110
- **Next**

**Étape 4 : Durée du bail**
- Par défaut : 8 jours (c'est bien)
- **Next**

**Étape 5 : Configurer les options DHCP**
- Sélectionnez **Yes, I want to configure these options now**
- **Next**

**Étape 6 : Passerelle (Router)**
- IP Address : `192.168.1.1`
- **Add**
- **Next**

**Étape 7 : DNS**
- Server name : `DC1` (si c'est votre serveur DNS)
- **Resolve** (ça trouve l'IP automatiquement)
- Ou tapez directement l'IP : `192.168.1.10`
- **Add**
- **Next**

**Étape 8 : WINS** (optionnel, généralement on saute)
- **Next**

**Étape 9 : Activer le scope**
- Sélectionnez **Yes, I want to activate this scope now**
- **Next** → **Finish**

✅ **Votre scope DHCP est créé et actif !**

#### Méthode 2 : PowerShell (RAPIDE)

```powershell
# Créer le scope
Add-DhcpServerv4Scope `
    -Name "LAN Principal" `
    -StartRange 192.168.1.100 `
    -EndRange 192.168.1.200 `
    -SubnetMask 255.255.255.0 `
    -LeaseDuration 8.00:00:00 `
    -State Active

# Ajouter les options (passerelle, DNS)
Set-DhcpServerv4OptionValue `
    -ScopeId 192.168.1.0 `
    -Router 192.168.1.1 `
    -DnsServer 192.168.1.10 `
    -DnsDomain "SOLARIS.local"

# Vérifier
Get-DhcpServerv4Scope
```

> 💡 **Astuce de pro :**
> Nommez vos scopes clairement : "Bureau 3ème étage 192.168.3.0/24", "WiFi invités 10.0.10.0/24", etc. Dans 6 mois, vous ne vous souviendrez plus de ce que c'est !

---

### Réservations DHCP

Une **réservation** associe une adresse IP à une adresse MAC spécifique. La machine obtient TOUJOURS la même IP via DHCP.

**Cas d'usage :**
- Imprimantes réseau
- Serveurs (si vous ne voulez pas configurer IP statique)
- Caméras IP
- Équipements réseau (switch managé, point d'accès WiFi)

#### Créer une réservation (GUI)

**Étape 1 : Trouver l'adresse MAC de la machine**

Sur Windows :
```cmd
ipconfig /all
```
Cherchez "Physical Address" : `00-15-5D-01-23-45`

**Étape 2 : Créer la réservation**
1. Console DHCP → IPv4 → Scope → **Reservations**
2. Clic droit → **New Reservation...**
3. Remplissez :
   - **Reservation name** : `Imprimante-RDC`
   - **IP address** : `192.168.1.50`
   - **MAC address** : `00-15-5D-01-23-45`
   - **Description** : `Imprimante HP bureau RDC`
4. **Add**

✅ Cette machine obtiendra TOUJOURS l'IP 192.168.1.50 via DHCP.

#### Créer une réservation (PowerShell)

```powershell
Add-DhcpServerv4Reservation `
    -ScopeId 192.168.1.0 `
    -IPAddress 192.168.1.50 `
    -ClientId "00-15-5D-01-23-45" `
    -Name "Imprimante-RDC" `
    -Description "Imprimante HP bureau RDC"
```

> 💡 **Conseil d'expert :**
> Pour les imprimantes, **toujours utiliser une réservation DHCP** plutôt qu'une IP statique. Pourquoi ?
> - Centralisé : toutes les IP gérées au même endroit (serveur DHCP)
> - Si vous changez le DNS ou la passerelle, ça se met à jour automatiquement
> - Plus facile à documenter

---

### Diagnostic DHCP

#### 🔧 Côté client : Problème pour obtenir une IP

##### 1. Vérifier si le client a une IP

```cmd
ipconfig
```

**Cas 1 : IP en 169.254.x.x** (APIPA)
```
IPv4 Address. . . . . . . . . . . : 169.254.123.45
```
→ **Problème : Le client n'a pas pu contacter le serveur DHCP**

**Solutions :**
- Vérifier le câble réseau
- Vérifier que le serveur DHCP est allumé
- Vérifier que le scope est activé
- Vérifier les paramètres du pare-feu (port UDP 67/68)

**Cas 2 : Pas d'IP du tout**
→ Carte réseau désactivée ou driver manquant

##### 2. Forcer le renouvellement DHCP

```cmd
# Libérer l'IP actuelle
ipconfig /release

# Demander une nouvelle IP
ipconfig /renew
```

> 💡 Cette commande résout 50% des problèmes DHCP !

##### 3. Voir les détails du bail DHCP

```cmd
ipconfig /all
```

Cherchez :
```
DHCP Enabled. . . . . . . . . . . : Yes
DHCP Server . . . . . . . . . . . : 192.168.1.10
Lease Obtained. . . . . . . . . . : Sunday, January 12, 2026 9:00:00 AM
Lease Expires . . . . . . . . . . : Monday, January 20, 2026 9:00:00 AM
```

#### 🔧 Côté serveur : Vérifier les baux actifs

##### Console DHCP (GUI)

1. DHCP → IPv4 → Scope → **Address Leases**
2. Vous voyez toutes les IP distribuées avec :
   - Nom du client
   - Adresse IP
   - Type (Bail actif, Réservation)
   - Expiration

##### PowerShell

```powershell
# Lister tous les baux actifs
Get-DhcpServerv4Lease -ScopeId 192.168.1.0

# Voir les statistiques du scope
Get-DhcpServerv4ScopeStatistics

# Vérifier l'état du service DHCP
Get-Service DHCPServer
```

**Exemple de statistiques :**
```
ScopeId       : 192.168.1.0
Free          : 85
InUse         : 16
Percentage    : 15.84%
```

> ⚠️ **ALERTE :** Si le scope est plein (Percentage > 95%), étendez la plage d'IP !

#### 🔧 Problèmes courants et solutions

| Symptôme | Cause probable | Solution |
|----------|----------------|----------|
| Client obtient IP 169.254.x.x | Serveur DHCP injoignable | Vérifier réseau, serveur allumé, scope actif |
| Scope vide (0 baux) | Scope désactivé | Clic droit scope → Activate |
| "IP address already in use" | Conflit d'IP | Identifier la machine en double, corriger IP statique |
| Scope plein (100% utilisé) | Trop de machines / scope trop petit | Étendre la plage d'IP ou réduire durée bail |
| Client n'obtient pas les bonnes options (DNS, passerelle) | Options mal configurées | Vérifier Set-DhcpServerv4OptionValue |

---

## 🔷 Partie 3 : Intégration DNS + DHCP + Active Directory

Dans un environnement Active Directory, DNS et DHCP travaillent ensemble de manière magique.

### DNS dynamique intégré à Active Directory

**Qu'est-ce que c'est ?**

Quand un client obtient une IP via DHCP, le serveur DHCP **enregistre automatiquement** le nom de la machine dans le DNS.

**Exemple :**
```
1. PC-COMPTABLE se connecte au réseau
2. DHCP lui donne l'IP 192.168.1.150
3. DHCP enregistre automatiquement dans le DNS :
   - pc-comptable.solaris.local → 192.168.1.150 (enregistrement A)
   - 192.168.1.150 → pc-comptable.solaris.local (enregistrement PTR)
```

**Résultat :** Vous pouvez faire `ping pc-comptable` et ça marche automatiquement !

### Configuration de l'intégration DHCP → DNS

**PowerShell (recommandé) :**
```powershell
# Activer l'enregistrement DNS dynamique pour un scope
Set-DhcpServerv4DnsSetting `
    -ScopeId 192.168.1.0 `
    -DynamicUpdates Always `
    -DeleteDnsRROnLeaseExpiry $True
```

**GUI :**
1. Console DHCP → Scope → Clic droit → **Properties**
2. Onglet **DNS**
3. Cochez :
   - ✅ Enable DNS dynamic updates according to the settings below
   - ✅ Always dynamically update DNS records
   - ✅ Discard A and PTR records when lease is deleted
4. **OK**

> 💡 **Conseil d'expert :**
> Avec cette config, votre DNS est toujours à jour automatiquement. Vous n'avez plus JAMAIS à créer manuellement un enregistrement A pour un poste client. C'est un gain de temps ÉNORME !

### Enregistrements SRV pour Active Directory

Active Directory utilise massivement les enregistrements **SRV** pour localiser les contrôleurs de domaine.

**Enregistrements SRV importants :**
- `_ldap._tcp.SOLARIS.local` : Service LDAP (annuaire AD)
- `_kerberos._tcp.SOLARIS.local` : Service Kerberos (authentification)
- `_gc._tcp.SOLARIS.local` : Global Catalog

**Ces enregistrements sont créés automatiquement** quand vous installez Active Directory.

**Vérification :**
```powershell
# Lister les enregistrements SRV
Get-DnsServerResourceRecord -ZoneName "SOLARIS.local" -RRType Srv
```

**Si manquants (ça arrive après un crash) :**
```cmd
# Forcer l'enregistrement du DC dans le DNS
ipconfig /registerdns

# Ou redémarrer le service Netlogon
net stop netlogon && net start netlogon
```

> ⚠️ **PIÈGE EXAMEN :**
> Si on vous donne un problème "les clients ne peuvent pas se connecter au domaine", vérifiez TOUJOURS en premier les enregistrements SRV dans le DNS. C'est un classique !

---

## 🔧 Dépannage courant

### 🚨 Problème 1 : "Je ne peux pas accéder à internet"

**Méthodologie de diagnostic (méthode de PRO) :**

```cmd
# Étape 1 : Vérifier l'IP
ipconfig

# ✅ IP valide (192.168.x.x) ? → Étape 2
# ❌ IP 169.254.x.x ? → Problème DHCP (voir section DHCP)

# Étape 2 : Vérifier la connectivité locale
ping 192.168.1.1   (passerelle)

# ✅ Ça répond ? → Étape 3
# ❌ Pas de réponse ? → Problème réseau physique ou passerelle

# Étape 3 : Vérifier le DNS
ping google.com

# ✅ Ça répond ? → Internet fonctionne !
# ❌ "Ping request could not find host" ? → Problème DNS → Étape 4

# Étape 4 : Vérifier le DNS avec une IP publique
ping 8.8.8.8   (DNS de Google)

# ✅ Ça répond ? → DNS défaillant, internet OK
# ❌ Pas de réponse ? → Problème routage/internet

# Étape 5 : Vérifier la config DNS
ipconfig /all
# Vérifier "DNS Servers" : doit pointer vers votre serveur interne (192.168.1.10)

# Étape 6 : Tester la résolution DNS
nslookup google.com

# ❌ Erreur ? → Serveur DNS HS ou mal configuré
```

**Solution 80% des cas :**
```cmd
ipconfig /flushdns
ipconfig /release
ipconfig /renew
```

### 🚨 Problème 2 : "Le serveur XXX ne répond pas"

```cmd
# Étape 1 : Résoudre le nom
nslookup serveur1.solaris.local

# ✅ Retourne une IP ? → Étape 2
# ❌ "Non-existent domain" ? → Enregistrement DNS manquant

# Étape 2 : Tester la connectivité vers l'IP
ping 192.168.1.10   (l'IP trouvée)

# ✅ Ça répond ? → Le serveur est joignable, problème applicatif
# ❌ Pas de réponse ? → Serveur éteint ou pare-feu bloque
```

**Solution si DNS manquant :**
```powershell
# Créer l'enregistrement A manuellement
Add-DnsServerResourceRecordA -Name "serveur1" -ZoneName "SOLARIS.local" -IPv4Address "192.168.1.10"
```

### 🚨 Problème 3 : Conflit d'IP

**Symptôme :** Un PC affiche "IP address conflict"

**Cause :** Deux machines ont la même IP (généralement une IP statique en double)

**Diagnostic :**
```cmd
# Sur le PC en conflit, noter l'IP
ipconfig

# Trouver qui d'autre a cette IP
arp -a | findstr "192.168.1.50"
```

**Solution :**
1. Identifier la machine en double
2. Si c'est une IP statique mal configurée : corriger l'IP statique
3. Si c'est dans le scope DHCP : créer une exclusion ou réservation

### 🚨 Problème 4 : Serveur DNS ne répond plus

**Symptôme :** Plus personne ne peut résoudre les noms

**Diagnostic :**
```powershell
# Vérifier le service DNS
Get-Service DNS

# Si Status = Stopped → Redémarrer
Restart-Service DNS
```

**Causes fréquentes :**
- Serveur DNS surchargé (manque de RAM)
- Zone DNS corrompue
- Disque plein

**Solution temporaire :**
```powershell
# Redémarrer le service
Restart-Service DNS

# Vérifier les logs
Get-EventLog -LogName "DNS Server" -Newest 50
```

---

## 💡 Astuces de pro (20 ans d'expérience)

### ✅ 1. Toujours avoir 2 serveurs DNS

**En prod, configurez TOUJOURS 2 serveurs DNS sur les clients :**

```
DNS primaire : 192.168.1.10 (votre serveur interne)
DNS secondaire : 8.8.8.8 (Google DNS) ou 1.1.1.1 (Cloudflare)
```

**Pourquoi ?**
- Si votre DNS interne plante, les clients peuvent encore accéder à internet
- Les noms internes (solaris.local) ne marcheront plus, mais au moins ils ont internet

**Comment configurer (PowerShell) :**
```powershell
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -DnsServer 192.168.1.10,8.8.8.8
```

### ✅ 2. Utilisez des noms descriptifs partout

**❌ Mauvais :**
```
Scope : "Scope 1"
Enregistrement A : "srv1"
Réservation : "Reservation 001"
```

**✅ Bon :**
```
Scope : "Bureau Comptabilité - 192.168.2.0/24"
Enregistrement A : "serveur-fichiers-principal"
Réservation : "Imprimante-HP-RDC"
```

Dans 6 mois, vous ne vous souviendrez plus de ce que c'est !

### ✅ 3. Documentez vos plages IP

**Créez un fichier Excel/Markdown :**

| Plage | Usage | DHCP ? |
|-------|-------|--------|
| 192.168.1.1-99 | IP statiques (serveurs) | Non |
| 192.168.1.100-200 | DHCP postes utilisateurs | Oui |
| 192.168.1.201-220 | Réservations DHCP (imprimantes) | Oui (réservé) |
| 192.168.1.221-254 | Libres (extension future) | Non |

### ✅ 4. Automatisez avec PowerShell

**Script utile : Créer 10 enregistrements DNS d'un coup**

```powershell
# Créer des enregistrements pour 10 PCs
1..10 | ForEach-Object {
    Add-DnsServerResourceRecordA `
        -Name "PC-Compta-$_" `
        -ZoneName "SOLARIS.local" `
        -IPv4Address "192.168.2.$_"
}
```

### ✅ 5. Surveillez l'utilisation de vos scopes DHCP

**Script de monitoring :**

```powershell
# Vérifier si un scope est presque plein
Get-DhcpServerv4ScopeStatistics | Where-Object {$_.PercentageInUse -gt 80} | Format-Table

# Recevoir une alerte par email (à scripter)
```

**Si un scope dépasse 80% :**
- Étendez la plage d'IP
- Réduisez la durée de bail (ex: 8 jours → 2 jours)
- Nettoyez les baux expirés

### ✅ 6. Sauvegardez vos zones DNS régulièrement

**Backup manuel :**
```powershell
# Exporter toutes les zones DNS
Export-DnsServerZone -Name "SOLARIS.local" -FileName "SOLARIS.local.backup"
```

**Les fichiers de zone sont ici :**
```
C:\Windows\System32\dns\
```

**Sauvegardez ce dossier régulièrement !**

---

## ⚠️ Pièges à éviter (erreurs classiques)

### ❌ 1. Oublier d'autoriser le serveur DHCP dans AD

**Symptôme :** DHCP installé mais ne distribue aucune IP.

**Solution :**
```powershell
Add-DhcpServerInDC -DnsName "DC1.solaris.local" -IPAddress 192.168.1.10
```

### ❌ 2. Configurer un client avec le DNS public en premier

**❌ Mauvais :**
```
DNS 1 : 8.8.8.8 (Google)
DNS 2 : 192.168.1.10 (votre serveur)
```

**Pourquoi c'est mauvais ?**
- Les noms internes (serveur1.solaris.local) ne seront JAMAIS résolus
- Active Directory ne fonctionnera pas

**✅ Bon :**
```
DNS 1 : 192.168.1.10 (votre serveur)
DNS 2 : 8.8.8.8 (Google)
```

### ❌ 3. Créer un scope DHCP qui chevauche les IP statiques

**Exemple d'erreur :**
```
IP statiques serveurs : 192.168.1.1-50
Scope DHCP : 192.168.1.10-200
→ Conflit ! Le DHCP va distribuer des IP déjà utilisées !
```

**Solution :** Toujours exclure les IP statiques du scope ou utiliser des plages séparées.

### ❌ 4. Ne pas vider le cache DNS après une modification

**Vous modifiez un enregistrement DNS :**
- serveur1.solaris.local : 192.168.1.10 → 192.168.1.20

**Mais les clients utilisent encore l'ancienne IP !**

**Solution :**
```cmd
# Sur CHAQUE client
ipconfig /flushdns

# Sur le serveur DNS
dnscmd /clearcache
```

### ❌ 5. Oublier de tester la résolution inverse (PTR)

**Symptôme :** Les emails sortants sont rejetés (anti-spam)

**Cause :** Pas d'enregistrement PTR

**Vérification :**
```cmd
nslookup 192.168.1.10
```

**Doit retourner le nom du serveur, pas "Non-existent domain".**

**Solution :** Créer une zone de recherche inversée et cocher "Create PTR" quand vous créez un enregistrement A.

---

## 🎯 Exercices pratiques (pour l'examen)

### Exercice 1 : Configuration complète DNS + DHCP (30 min)

**Contexte :**
Vous êtes technicien chez **TechnoSolaris**, une PME de 50 employés. Votre mission : installer et configurer DNS et DHCP.

**Informations réseau :**
- Domaine : `technosolaris.local`
- Réseau : `10.0.10.0/24`
- Serveur : `SRV1` - IP statique : `10.0.10.1`
- Passerelle internet : `10.0.10.254`
- Plage DHCP : `10.0.10.50` à `10.0.10.150` (101 adresses)
- Durée bail : 4 jours

**Consignes :**

**Partie 1 : DNS (15 min)**
1. Installez le rôle DNS sur SRV1
2. Créez la zone de recherche directe `technosolaris.local`
3. Créez les enregistrements DNS suivants :
   - `srv1.technosolaris.local` → `10.0.10.1`
   - `web` → alias vers `srv1.technosolaris.local`
   - `mail.technosolaris.local` → `10.0.10.5`
   - Enregistrement MX pour le domaine pointant vers `mail.technosolaris.local` (priorité 10)
4. Créez la zone de recherche inversée pour le réseau `10.0.10.0/24`
5. Testez la résolution avec `nslookup`

**Partie 2 : DHCP (15 min)**
1. Installez le rôle DHCP sur SRV1
2. Autorisez le serveur DHCP dans Active Directory
3. Créez un scope nommé "Réseau principal" avec :
   - Plage : `10.0.10.50` à `10.0.10.150`
   - Masque : `255.255.255.0`
   - Passerelle : `10.0.10.254`
   - DNS : `10.0.10.1` (SRV1)
   - Domaine DNS : `technosolaris.local`
   - Durée bail : 4 jours
4. Créez une réservation pour l'imprimante HP (MAC: 00-11-22-33-44-55) → IP `10.0.10.200`
5. Activez le scope
6. Sur un poste client, testez l'obtention d'une IP via DHCP

<details>
<summary>Cliquez pour voir la solution</summary>

**Solution Partie 1 : DNS**

```powershell
# 1. Installer DNS
Install-WindowsFeature DNS -IncludeManagementTools

# 2. Créer la zone directe
Add-DnsServerPrimaryZone -Name "technosolaris.local" -ReplicationScope "Forest" -DynamicUpdate "Secure"

# 3. Créer les enregistrements
Add-DnsServerResourceRecordA -Name "srv1" -ZoneName "technosolaris.local" -IPv4Address "10.0.10.1"
Add-DnsServerResourceRecordCName -Name "web" -ZoneName "technosolaris.local" -HostNameAlias "srv1.technosolaris.local"
Add-DnsServerResourceRecordA -Name "mail" -ZoneName "technosolaris.local" -IPv4Address "10.0.10.5"
Add-DnsServerResourceRecordMX -Name "." -ZoneName "technosolaris.local" -MailExchange "mail.technosolaris.local" -Preference 10

# 4. Créer la zone inversée
Add-DnsServerPrimaryZone -NetworkId "10.0.10.0/24" -ReplicationScope "Forest" -DynamicUpdate "Secure"

# 5. Test
nslookup srv1.technosolaris.local
nslookup web.technosolaris.local
```

**Solution Partie 2 : DHCP**

```powershell
# 1. Installer DHCP
Install-WindowsFeature DHCP -IncludeManagementTools

# 2. Autoriser le serveur
Add-DhcpServerInDC -DnsName "SRV1.technosolaris.local" -IPAddress 10.0.10.1

# 3. Créer le scope
Add-DhcpServerv4Scope `
    -Name "Réseau principal" `
    -StartRange 10.0.10.50 `
    -EndRange 10.0.10.150 `
    -SubnetMask 255.255.255.0 `
    -LeaseDuration 4.00:00:00 `
    -State Active

# Configurer les options
Set-DhcpServerv4OptionValue `
    -ScopeId 10.0.10.0 `
    -Router 10.0.10.254 `
    -DnsServer 10.0.10.1 `
    -DnsDomain "technosolaris.local"

# 4. Créer la réservation imprimante
Add-DhcpServerv4Reservation `
    -ScopeId 10.0.10.0 `
    -IPAddress 10.0.10.200 `
    -ClientId "00-11-22-33-44-55" `
    -Name "Imprimante-HP" `
    -Description "Imprimante HP bureau"

# 6. Test sur le client
ipconfig /release
ipconfig /renew
ipconfig /all
```

</details>

---

### Exercice 2 : Diagnostic de panne (15 min)

**Contexte :**
Les utilisateurs du service comptabilité vous appellent : "On ne peut plus accéder au serveur de fichiers !"

**Informations :**
- Serveur de fichiers : `fichiers.solaris.local`
- IP attendue : `192.168.1.20`
- Utilisateurs obtiennent : "Host not found"

**Consignes :**
1. Identifiez le problème avec les commandes de diagnostic
2. Proposez une solution
3. Testez que la solution fonctionne

<details>
<summary>Cliquez pour voir la solution</summary>

**Diagnostic :**

```cmd
# Étape 1 : Tester la résolution DNS
nslookup fichiers.solaris.local
# Résultat : "Non-existent domain" → Enregistrement DNS manquant

# Étape 2 : Tester avec l'IP directe
ping 192.168.1.20
# Résultat : Ça répond ! → Le serveur est joignable, c'est bien un problème DNS

# Étape 3 : Vérifier dans le DNS
# Console DNS → Zone SOLARIS.local → Chercher "fichiers"
# Résultat : Enregistrement absent
```

**Solution :**

```powershell
# Ajouter l'enregistrement manquant
Add-DnsServerResourceRecordA -Name "fichiers" -ZoneName "solaris.local" -IPv4Address "192.168.1.20"

# Test
nslookup fichiers.solaris.local
# Résultat : Retourne 192.168.1.20 ✅

# Test connexion
ping fichiers.solaris.local
# Résultat : Ça répond ✅
```

**Sur les postes clients, vider le cache :**
```cmd
ipconfig /flushdns
```

</details>

---

## ✅ Checklist pour l'examen

Avant de passer au module suivant, assurez-vous de maîtriser :

### DNS
- [ ] Expliquer le rôle du DNS (traduction nom → IP)
- [ ] Installer le rôle DNS en moins de 3 minutes
- [ ] Créer une zone de recherche directe
- [ ] Créer une zone de recherche inversée
- [ ] Créer un enregistrement A (nom → IP)
- [ ] Créer un CNAME (alias)
- [ ] Créer un enregistrement MX (email)
- [ ] Diagnostiquer un problème DNS avec `nslookup`
- [ ] Vider le cache DNS (`ipconfig /flushdns`)
- [ ] Vérifier le service DNS (PowerShell)

### DHCP
- [ ] Expliquer le rôle du DHCP (attribution automatique IP)
- [ ] Expliquer le processus DORA (Discover, Offer, Request, Ack)
- [ ] Installer le rôle DHCP
- [ ] Autoriser le serveur DHCP dans AD
- [ ] Créer un scope DHCP (plage, masque, passerelle, DNS)
- [ ] Créer une réservation DHCP (MAC → IP fixe)
- [ ] Diagnostiquer un problème DHCP (IP 169.254.x.x)
- [ ] Forcer le renouvellement DHCP (`ipconfig /release` + `renew`)
- [ ] Vérifier les baux actifs
- [ ] Étendre un scope plein

### Intégration
- [ ] Activer l'enregistrement DNS dynamique via DHCP
- [ ] Expliquer le rôle des enregistrements SRV pour Active Directory
- [ ] Vérifier les enregistrements SRV d'un DC

### Diagnostic
- [ ] Suivre la méthodologie de diagnostic réseau (IP → Passerelle → DNS → Internet)
- [ ] Diagnostiquer "pas d'internet" en 5 étapes
- [ ] Identifier et corriger un conflit d'IP
- [ ] Redémarrer les services DNS/DHCP

---

## 📚 Ressources complémentaires

### Documentation officielle Microsoft
- [DNS Server Overview](https://docs.microsoft.com/en-us/windows-server/networking/dns/dns-top)
- [DHCP Server Overview](https://docs.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top)

### Tutoriels recommandés
- [IT-Connect : DNS sous Windows Server](https://www.it-connect.fr)
- [TechNet : DHCP Best Practices](https://technet.microsoft.com)

### Commandes essentielles (mémo)

**DNS :**
```powershell
# Installer DNS
Install-WindowsFeature DNS -IncludeManagementTools

# Créer zone
Add-DnsServerPrimaryZone -Name "domain.local" -ReplicationScope "Forest" -DynamicUpdate "Secure"

# Ajouter enregistrement A
Add-DnsServerResourceRecordA -Name "server1" -ZoneName "domain.local" -IPv4Address "192.168.1.10"

# Lister enregistrements
Get-DnsServerResourceRecord -ZoneName "domain.local"
```

**DHCP :**
```powershell
# Installer DHCP
Install-WindowsFeature DHCP -IncludeManagementTools

# Autoriser serveur
Add-DhcpServerInDC -DnsName "DC1.domain.local" -IPAddress 192.168.1.10

# Créer scope
Add-DhcpServerv4Scope -Name "LAN" -StartRange 192.168.1.100 -EndRange 192.168.1.200 -SubnetMask 255.255.255.0

# Options
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -Router 192.168.1.1 -DnsServer 192.168.1.10
```

**Diagnostic :**
```cmd
ipconfig /all
ipconfig /flushdns
ipconfig /release
ipconfig /renew
nslookup server.domain.local
ping server.domain.local
```

---

## 📝 Message final de votre formateur

> **Félicitations !** Vous venez de terminer le module le plus important de votre formation TSSR.
>
> **DNS et DHCP = 80% de votre quotidien en poste.** Si vous maîtrisez ces 2 services, vous allez résoudre la majorité des problèmes réseau plus vite que vos collègues.
>
> **À l'examen :**
> - Entraînez-vous à créer DNS + DHCP en moins de 30 minutes
> - Connaissez les commandes de diagnostic par cœur
> - Sachez diagnostiquer "pas d'internet" les yeux fermés
>
> **En entreprise :**
> - Documentez vos plages IP et vos zones DNS
> - Automatisez avec PowerShell
> - Surveillez l'utilisation de vos scopes DHCP
>
> **Prochaine étape :** Pratique, pratique, PRATIQUE ! Installez une VM, cassez tout, réparez. C'est comme ça qu'on apprend vraiment.
>
> 💪 **Vous avez le niveau pour réussir l'examen. Maintenant, il faut pratiquer !**

---

<div align="center">

**Cours suivant :** [GPO - Stratégies de groupe](./gpo-strategies-groupe.md)

[⬅️ Retour au sommaire](../README.md) | [📊 Progression](../progression.md)

---

**💡 Une question ? Relisez la section "Dépannage courant" - 90% des réponses y sont !**

</div>
