# 🖥️ Guide Complet : Configuration Infrastructure AD + GPO (Interface Graphique)

**Objectif :** Configurer un environnement Active Directory complet AVANT d'activer les GPO

---

## 📑 Table des matières

1. [Configuration IP Statique](#1-configuration-ip-statique)
2. [Installation et Configuration DNS](#2-installation-et-configuration-dns)
3. [Installation et Configuration DHCP](#3-installation-et-configuration-dhcp)
4. [Installation Active Directory](#4-installation-active-directory)
5. [Création de la Structure OU](#5-création-de-la-structure-ou)
6. [Création des GPO (Mode Préparation)](#6-création-des-gpo-mode-préparation)
7. [Tests et Activation Progressive](#7-tests-et-activation-progressive)

---

## 1️⃣ Configuration IP Statique

### 📌 Via Interface Graphique

**Étape 1 : Ouvrir les paramètres réseau**
```
Panneau de configuration
  → Centre Réseau et partage
  → Modifier les paramètres de la carte
```

**Étape 2 : Configuration de la carte réseau**
```
1. Clic droit sur "Ethernet" → Propriétés
2. Double-clic sur "Protocole Internet version 4 (TCP/IPv4)"
3. Sélectionner "Utiliser l'adresse IP suivante"

Configuration exemple :
┌─────────────────────────────────────┐
│ Adresse IP     : 192.168.1.10      │
│ Masque         : 255.255.255.0      │
│ Passerelle     : 192.168.1.1        │
│ DNS préféré    : 127.0.0.1          │
│ DNS auxiliaire : 192.168.1.1        │
└─────────────────────────────────────┘

4. Cliquer OK → OK
```

### 💡 Astuces PowerShell

```powershell
# ✅ Vérifier la configuration IP actuelle
Get-NetIPAddress -AddressFamily IPv4

# ✅ Configurer IP statique en PowerShell (alternative)
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.10 -PrefixLength 24 -DefaultGateway 192.168.1.1

# ✅ Configurer DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("127.0.0.1","192.168.1.1")

# ✅ Vérifier la connectivité
Test-Connection -ComputerName 8.8.8.8 -Count 2
Test-Connection -ComputerName google.com -Count 2
```

**✔️ Validation :**
- [ ] IP configurée correctement
- [ ] Ping vers la passerelle fonctionne
- [ ] Ping vers Internet fonctionne

---

## 2️⃣ Installation et Configuration DNS

### 📌 Installation du rôle DNS (Interface Graphique)

**Étape 1 : Gestionnaire de serveur**
```
1. Ouvrir "Gestionnaire de serveur"
2. Cliquer "Gérer" → "Ajouter des rôles et fonctionnalités"
3. Suivant → Suivant → Suivant
4. Cocher "Serveur DNS"
5. Cliquer "Ajouter des fonctionnalités"
6. Suivant → Suivant → Suivant → Installer
7. Attendre la fin → Fermer
```

### 📌 Configuration Zone de Recherche Directe

**Étape 2 : Ouvrir le Gestionnaire DNS**
```
1. Gestionnaire de serveur → Outils → DNS
2. Développer le nom du serveur
```

**Étape 3 : Créer la zone directe**
```
1. Clic droit sur "Zones de recherche directes" → "Nouvelle zone..."

Assistant Nouvelle Zone :
┌──────────────────────────────────────────┐
│ Type de zone : Zone principale          │
│ → Suivant                                │
│                                          │
│ Nom de la zone : entreprise.local       │
│ → Suivant                                │
│                                          │
│ Fichier de zone : entreprise.local.dns  │
│ → Suivant                                │
│                                          │
│ Mise à jour dynamique :                  │
│ ☑ Autoriser les mises à jour sécurisées │
│   et non sécurisées (temporaire)         │
│ → Suivant → Terminer                     │
└──────────────────────────────────────────┘
```

**Étape 4 : Ajouter un enregistrement A (Hôte)**
```
1. Développer "Zones de recherche directes" → "entreprise.local"
2. Clic droit dans le panneau → "Nouvel hôte (A ou AAAA)..."

┌────────────────────────────────┐
│ Nom : serveur01                │
│ Adresse IP : 192.168.1.10      │
│ ☑ Créer l'enregistrement PTR   │
│ → Ajouter un hôte              │
└────────────────────────────────┘
```

### 📌 Configuration Zone de Recherche Inversée

**Étape 5 : Créer la zone inversée**
```
1. Clic droit sur "Zones de recherche inversées" → "Nouvelle zone..."

Assistant Nouvelle Zone :
┌──────────────────────────────────────────┐
│ Type de zone : Zone principale          │
│ → Suivant                                │
│                                          │
│ Zone de recherche inversée IPv4          │
│ → Suivant                                │
│                                          │
│ ID réseau : 192.168.1                    │
│ → Suivant                                │
│                                          │
│ Mise à jour dynamique :                  │
│ ☑ Autoriser les mises à jour sécurisées │
│   et non sécurisées                      │
│ → Suivant → Terminer                     │
└──────────────────────────────────────────┘
```

### 💡 Astuces PowerShell

```powershell
# ✅ Vérifier les zones DNS
Get-DnsServerZone

# ✅ Créer une zone directe (alternative)
Add-DnsServerPrimaryZone -Name "entreprise.local" -ZoneFile "entreprise.local.dns" -DynamicUpdate Secure

# ✅ Créer une zone inversée (alternative)
Add-DnsServerPrimaryZone -NetworkID "192.168.1.0/24" -ZoneFile "1.168.192.in-addr.arpa.dns"

# ✅ Ajouter un enregistrement A
Add-DnsServerResourceRecordA -Name "serveur01" -ZoneName "entreprise.local" -IPv4Address "192.168.1.10" -CreatePtr

# ✅ Tester la résolution DNS
nslookup serveur01.entreprise.local
nslookup 192.168.1.10

# ✅ Vérifier tous les enregistrements de la zone
Get-DnsServerResourceRecord -ZoneName "entreprise.local"

# ✅ Tester depuis PowerShell
Resolve-DnsName serveur01.entreprise.local
```

**✔️ Validation :**
- [ ] Zone directe créée
- [ ] Zone inversée créée
- [ ] `nslookup serveur01.entreprise.local` retourne 192.168.1.10
- [ ] `nslookup 192.168.1.10` retourne serveur01.entreprise.local

---

## 3️⃣ Installation et Configuration DHCP

### 📌 Installation du rôle DHCP

**Étape 1 : Gestionnaire de serveur**
```
1. Gestionnaire de serveur → "Gérer" → "Ajouter des rôles et fonctionnalités"
2. Suivant × 3
3. Cocher "Serveur DHCP"
4. Ajouter des fonctionnalités → Suivant × 3
5. Installer → Fermer
```

**Étape 2 : Configuration post-installation**
```
1. Dans Gestionnaire de serveur, cliquer sur le drapeau (🚩)
2. Cliquer "Terminer la configuration DHCP"
3. Suivant
4. Utiliser les informations d'identification suivantes (compte admin)
5. Suivant → Valider → Fermer
```

### 📌 Configuration de l'étendue DHCP

**Étape 3 : Créer une nouvelle étendue**
```
1. Gestionnaire de serveur → Outils → DHCP
2. Développer le nom du serveur
3. Clic droit sur "IPv4" → "Nouvelle étendue..."

Assistant Nouvelle Étendue :
┌─────────────────────────────────────────────┐
│ Nom : Réseau_Local_Principal                │
│ → Suivant                                   │
│                                             │
│ Plage d'adresses IP :                       │
│   IP de début  : 192.168.1.100             │
│   IP de fin    : 192.168.1.200             │
│   Masque       : 255.255.255.0             │
│ → Suivant                                   │
│                                             │
│ Exclusions (optionnel) :                    │
│   IP de début  : 192.168.1.1               │
│   IP de fin    : 192.168.1.20              │
│   (Réserver pour serveurs/imprimantes)      │
│ → Ajouter → Suivant                         │
│                                             │
│ Durée du bail : 8 heures (par défaut)      │
│ → Suivant                                   │
│                                             │
│ Configurer les options DHCP : Oui           │
│ → Suivant                                   │
│                                             │
│ Passerelle (routeur) : 192.168.1.1         │
│ → Ajouter → Suivant                         │
│                                             │
│ Nom de domaine : entreprise.local           │
│ Serveurs DNS : 192.168.1.10                │
│ → Ajouter → Suivant                         │
│                                             │
│ Serveurs WINS : (laisser vide)              │
│ → Suivant                                   │
│                                             │
│ Activer cette étendue : Oui                 │
│ → Suivant → Terminer                        │
└─────────────────────────────────────────────┘
```

**Étape 4 : Activer l'étendue**
```
1. Dans DHCP, développer "IPv4" → "Étendue"
2. Si icône rouge, clic droit → "Activer"
3. Icône devient verte ✅
```

### 💡 Astuces PowerShell

```powershell
# ✅ Vérifier le service DHCP
Get-Service DHCPServer

# ✅ Créer une étendue DHCP (alternative)
Add-DhcpServerv4Scope -Name "Réseau_Local" -StartRange 192.168.1.100 -EndRange 192.168.1.200 -SubnetMask 255.255.255.0 -State Active

# ✅ Ajouter des exclusions
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.1.0 -StartRange 192.168.1.1 -EndRange 192.168.1.20

# ✅ Configurer les options DHCP (passerelle)
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -Router 192.168.1.1

# ✅ Configurer les options DHCP (DNS)
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -DnsServer 192.168.1.10 -DnsDomain "entreprise.local"

# ✅ Voir toutes les étendues
Get-DhcpServerv4Scope

# ✅ Voir les baux actifs
Get-DhcpServerv4Lease -ScopeId 192.168.1.0

# ✅ Voir les statistiques DHCP
Get-DhcpServerv4ScopeStatistics

# ✅ Autoriser le serveur DHCP dans AD (après installation AD)
Add-DhcpServerInDC -DnsName serveur01.entreprise.local -IPAddress 192.168.1.10
```

**✔️ Validation :**
- [ ] Étendue créée et activée (icône verte)
- [ ] Options DHCP configurées (passerelle + DNS)
- [ ] Service DHCP en cours d'exécution

---

## 4️⃣ Installation Active Directory

### 📌 Installation du rôle AD DS

**Étape 1 : Ajouter le rôle**
```
1. Gestionnaire de serveur → Gérer → Ajouter des rôles et fonctionnalités
2. Suivant × 3
3. Cocher "Services AD DS"
4. Ajouter des fonctionnalités → Suivant × 3
5. Installer → Fermer
```

**Étape 2 : Promouvoir en contrôleur de domaine**
```
1. Cliquer sur le drapeau (🚩) → "Promouvoir ce serveur en contrôleur de domaine"

Assistant Configuration AD DS :
┌─────────────────────────────────────────────────┐
│ Configuration du déploiement :                  │
│ ● Ajouter une nouvelle forêt                    │
│ Nom de domaine racine : entreprise.local        │
│ → Suivant                                       │
│                                                 │
│ Options du contrôleur de domaine :             │
│ Niveau fonctionnel forêt : Windows Server 2016  │
│ Niveau fonctionnel domaine : Windows Server 2016│
│ ☑ Serveur DNS                                   │
│ ☑ Catalogue global (GC)                         │
│ Mot de passe DSRM : ************                │
│ → Suivant                                       │
│                                                 │
│ Options DNS : (Ignorer l'avertissement)         │
│ → Suivant                                       │
│                                                 │
│ Nom NetBIOS : ENTREPRISE                        │
│ → Suivant × 3                                   │
│                                                 │
│ Installer → Le serveur va redémarrer            │
└─────────────────────────────────────────────────┘
```

**⏱️ Attendre le redémarrage (5-10 minutes)**

### 💡 Astuces PowerShell

```powershell
# ✅ Installer AD DS (alternative)
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# ✅ Promouvoir en DC (alternative)
Install-ADDSForest `
  -DomainName "entreprise.local" `
  -DomainNetbiosName "ENTREPRISE" `
  -ForestMode "WinThreshold" `
  -DomainMode "WinThreshold" `
  -InstallDns:$true `
  -SafeModeAdministratorPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
  -Force:$true

# ✅ Vérifier l'installation AD
Get-ADDomain
Get-ADForest

# ✅ Vérifier les contrôleurs de domaine
Get-ADDomainController

# ✅ Tester la réplication AD
repadmin /showrepl

# ✅ Vérifier les rôles FSMO
netdom query fsmo

# ✅ Vérifier la santé AD
dcdiag /v

# ✅ Vérifier DNS après installation AD
nslookup entreprise.local
nslookup _ldap._tcp.entreprise.local
```

**✔️ Validation :**
- [ ] Serveur redémarré automatiquement
- [ ] Connexion avec ENTREPRISE\Administrateur fonctionne
- [ ] Outils AD disponibles (Utilisateurs et ordinateurs AD)

---

## 5️⃣ Création de la Structure OU

### 📌 Création des Unités Organisationnelles

**Étape 1 : Ouvrir Utilisateurs et Ordinateurs AD**
```
1. Gestionnaire de serveur → Outils → "Utilisateurs et ordinateurs Active Directory"
2. Développer "entreprise.local"
```

**Étape 2 : Créer la structure OU**
```
Structure recommandée :
entreprise.local
├── 📁 ENTREPRISE_Ressources
│   ├── 📁 Utilisateurs
│   │   ├── 📁 Direction
│   │   ├── 📁 Comptabilite
│   │   ├── 📁 Commercial
│   │   └── 📁 IT
│   ├── 📁 Ordinateurs
│   │   ├── 📁 Postes_Direction
│   │   ├── 📁 Postes_Compta
│   │   ├── 📁 Postes_Commercial
│   │   └── 📁 Postes_IT
│   ├── 📁 Groupes
│   └── 📁 Serveurs
└── 📁 TEST_GPO (pour les tests)
    ├── 📁 Utilisateurs_Test
    └── 📁 Ordinateurs_Test
```

**Création manuelle :**
```
1. Clic droit sur "entreprise.local" → Nouveau → Unité d'organisation
2. Nom : ENTREPRISE_Ressources
3. ☑ Protéger le conteneur contre la suppression accidentelle
4. OK

Répéter pour chaque OU de la structure
```

### 💡 Astuces PowerShell

```powershell
# ✅ Créer toute la structure en une commande
# Créer les OU principales
New-ADOrganizationalUnit -Name "ENTREPRISE_Ressources" -Path "DC=entreprise,DC=local" -ProtectedFromAccidentalDeletion $true
New-ADOrganizationalUnit -Name "TEST_GPO" -Path "DC=entreprise,DC=local" -ProtectedFromAccidentalDeletion $true

# Créer les sous-OU Utilisateurs
New-ADOrganizationalUnit -Name "Utilisateurs" -Path "OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Direction" -Path "OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Comptabilite" -Path "OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Commercial" -Path "OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "IT" -Path "OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"

# Créer les sous-OU Ordinateurs
New-ADOrganizationalUnit -Name "Ordinateurs" -Path "OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Postes_Direction" -Path "OU=Ordinateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Postes_Compta" -Path "OU=Ordinateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Postes_Commercial" -Path "OU=Ordinateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Postes_IT" -Path "OU=Ordinateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"

# Créer les autres OU
New-ADOrganizationalUnit -Name "Groupes" -Path "OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Serveurs" -Path "OU=ENTREPRISE_Ressources,DC=entreprise,DC=local"

# Créer les OU de test
New-ADOrganizationalUnit -Name "Utilisateurs_Test" -Path "OU=TEST_GPO,DC=entreprise,DC=local"
New-ADOrganizationalUnit -Name "Ordinateurs_Test" -Path "OU=TEST_GPO,DC=entreprise,DC=local"

# ✅ Lister toutes les OU
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName

# ✅ Vérifier la protection contre suppression
Get-ADOrganizationalUnit -Filter * -Properties ProtectedFromAccidentalDeletion | Select-Object Name, ProtectedFromAccidentalDeletion
```

### 📌 Création d'utilisateurs de test

**Via interface graphique :**
```
1. Clic droit sur "OU=Utilisateurs_Test" → Nouveau → Utilisateur

┌────────────────────────────────────┐
│ Prénom : Test                      │
│ Nom : User1                        │
│ Nom complet : Test User1           │
│ Nom d'ouverture de session :      │
│   testuser1                        │
│ → Suivant                          │
│                                    │
│ Mot de passe : P@ssw0rd123!       │
│ Confirmer : P@ssw0rd123!          │
│ ☑ L'utilisateur doit changer le   │
│   mot de passe à la prochaine     │
│   ouverture de session (DÉCOCHER) │
│ ☑ Le mot de passe n'expire jamais │
│ → Suivant → Terminer               │
└────────────────────────────────────┘
```

### 💡 PowerShell pour créer des utilisateurs

```powershell
# ✅ Créer un utilisateur de test
New-ADUser -Name "Test User1" `
  -GivenName "Test" `
  -Surname "User1" `
  -SamAccountName "testuser1" `
  -UserPrincipalName "testuser1@entreprise.local" `
  -Path "OU=Utilisateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" `
  -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
  -Enabled $true `
  -PasswordNeverExpires $true

# ✅ Créer plusieurs utilisateurs en une fois
1..5 | ForEach-Object {
    New-ADUser -Name "Test User$_" `
      -SamAccountName "testuser$_" `
      -UserPrincipalName "testuser$_@entreprise.local" `
      -Path "OU=Utilisateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" `
      -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
      -Enabled $true `
      -PasswordNeverExpires $true
}

# ✅ Lister tous les utilisateurs
Get-ADUser -Filter * | Select-Object Name, SamAccountName

# ✅ Vérifier les utilisateurs dans une OU spécifique
Get-ADUser -Filter * -SearchBase "OU=Utilisateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local"
```

**✔️ Validation :**
- [ ] Structure OU créée
- [ ] OU TEST_GPO créée
- [ ] Au moins 2 utilisateurs de test créés

---

## 6️⃣ Création des GPO (Mode Préparation)

### 📌 Ouvrir la Gestion des Stratégies de Groupe

**Étape 1 :**
```
Gestionnaire de serveur → Outils → "Gestion des stratégies de groupe"
```

### 📌 Exemple 1 : GPO de Sécurité (DÉSACTIVÉE)

**Étape 2 : Créer une nouvelle GPO**
```
1. Développer "Forêt: entreprise.local" → "Domaines" → "entreprise.local"
2. Clic droit sur "Objets de stratégie de groupe" → "Nouveau"

┌────────────────────────────────────┐
│ Nom : GPO_Securite_Postes          │
│ Objet GPO de départ : (aucun)      │
│ → OK                               │
└────────────────────────────────────┘
```

**Étape 3 : Configurer la GPO (SANS la lier)**
```
1. Clic droit sur "GPO_Securite_Postes" → "Modifier"

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              ├── Stratégies de compte
                  └── Stratégie de mot de passe

Paramètres à configurer :
┌─────────────────────────────────────────────────┐
│ Durée de vie maximale du mot de passe :        │
│   90 jours                                      │
│                                                 │
│ Durée de vie minimale du mot de passe :        │
│   1 jour                                        │
│                                                 │
│ Longueur minimale du mot de passe :            │
│   12 caractères                                 │
│                                                 │
│ Le mot de passe doit respecter des exigences   │
│ de complexité : Activé                          │
│                                                 │
│ Conserver l'historique des mots de passe :     │
│   24 mots de passe                              │
└─────────────────────────────────────────────────┘

2. Fermer l'Éditeur de gestion des stratégies de groupe
```

**Étape 4 : DÉSACTIVER la GPO**
```
1. Clic droit sur "GPO_Securite_Postes" → "Etat de la GPO"
2. Sélectionner "Tous les paramètres désactivés"
```

### 📌 Exemple 2 : GPO Mappage Lecteurs Réseau (DÉSACTIVÉE)

**Étape 5 : Créer la GPO**
```
1. Clic droit sur "Objets de stratégie de groupe" → "Nouveau"
2. Nom : GPO_Mappage_Lecteurs
3. → OK
```

**Étape 6 : Configurer les lecteurs réseau**
```
1. Clic droit sur "GPO_Mappage_Lecteurs" → "Modifier"

Configuration Utilisateur
  ├── Préférences
      ├── Paramètres Windows
          └── Mappages de lecteurs

2. Clic droit dans le panneau → "Nouveau" → "Lecteur mappé"

Premier lecteur (Partage public) :
┌────────────────────────────────────┐
│ Général :                          │
│ Action : Créer                     │
│ Emplacement : \\serveur01\public   │
│ Reconnecter : ☑                    │
│ Lettre de lecteur : P:             │
│ Afficher ce lecteur : ☑            │
│                                    │
│ Commun :                           │
│ ☐ Exécuter dans le contexte de    │
│   l'utilisateur connecté           │
│ → OK                               │
└────────────────────────────────────┘

3. Créer un autre lecteur (Dossier personnel)

Deuxième lecteur (Home) :
┌────────────────────────────────────┐
│ Action : Créer                     │
│ Emplacement :                      │
│   \\serveur01\home\%username%      │
│ Lettre de lecteur : H:             │
│ → OK                               │
└────────────────────────────────────┘

4. Fermer l'éditeur
```

**Étape 7 : DÉSACTIVER la GPO**
```
Clic droit sur "GPO_Mappage_Lecteurs" → "Etat de la GPO" → "Tous les paramètres désactivés"
```

### 📌 Exemple 3 : GPO Paramètres Bureau (DÉSACTIVÉE)

**Étape 8 : Créer la GPO**
```
Nom : GPO_Parametres_Bureau
```

**Étape 9 : Configurer les paramètres**
```
1. Modifier la GPO

Configuration Utilisateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Bureau
              └── Suppression des icônes du Bureau

Paramètres à activer :
┌────────────────────────────────────────────┐
│ ☑ Masquer l'icône Poste de travail        │
│   → Activé                                 │
│                                            │
│ ☑ Supprimer la Corbeille du Bureau        │
│   → Activé                                 │
└────────────────────────────────────────────┘

Configuration Utilisateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Menu Démarrer et Barre des tâches

┌────────────────────────────────────────────┐
│ Supprimer le menu Exécuter du menu Démarrer│
│   → Activé                                 │
└────────────────────────────────────────────┘

2. Fermer l'éditeur
3. Désactiver la GPO (Etat de la GPO → Tous les paramètres désactivés)
```

### 💡 Astuces PowerShell pour les GPO

```powershell
# ✅ Lister toutes les GPO
Get-GPO -All | Select-Object DisplayName, GpoStatus

# ✅ Créer une GPO (alternative)
New-GPO -Name "GPO_Securite_Postes" -Comment "Paramètres de sécurité des postes"

# ✅ Désactiver une GPO
(Get-GPO -Name "GPO_Securite_Postes").GpoStatus = "AllSettingsDisabled"

# ✅ Voir les détails d'une GPO
Get-GPO -Name "GPO_Securite_Postes" | Select-Object *

# ✅ Sauvegarder une GPO
Backup-GPO -Name "GPO_Securite_Postes" -Path "C:\Backup_GPO"

# ✅ Sauvegarder TOUTES les GPO
Backup-GPO -All -Path "C:\Backup_GPO"

# ✅ Générer un rapport HTML d'une GPO
Get-GPOReport -Name "GPO_Securite_Postes" -ReportType HTML -Path "C:\Rapports\GPO_Securite.html"

# ✅ Voir où est liée une GPO (après liaison)
Get-GPO -Name "GPO_Securite_Postes" | Select-Object -ExpandProperty DisplayName
(Get-GPInheritance -Target "OU=TEST_GPO,DC=entreprise,DC=local").GpoLinks

# ✅ Forcer la mise à jour des GPO sur un poste
Invoke-GPUpdate -Computer "PC-Test01" -Force
```

**✔️ Validation :**
- [ ] 3 GPO créées et configurées
- [ ] Toutes les GPO sont en état "Tous les paramètres désactivés"
- [ ] Aucune GPO n'est liée à une OU

---

## 7️⃣ Tests et Activation Progressive

### 📌 Étape 1 : Lier une GPO à l'OU de test (LIEN DÉSACTIVÉ)

**Via interface graphique :**
```
1. Dans "Gestion des stratégies de groupe"
2. Naviguer vers "TEST_GPO" → "Ordinateurs_Test"
3. Clic droit sur "Ordinateurs_Test" → "Lier un objet de stratégie de groupe existant..."
4. Sélectionner "GPO_Parametres_Bureau"
5. → OK

6. Dans le panneau, vous voyez maintenant le lien
7. Clic droit sur le lien "GPO_Parametres_Bureau" → DÉCOCHER "Lien activé"
   → Le lien apparaît grisé (désactivé)
```

### 💡 PowerShell pour lier/délier

```powershell
# ✅ Lier une GPO à une OU (lien actif)
New-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" -LinkEnabled Yes

# ✅ Lier une GPO avec lien DÉSACTIVÉ
New-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" -LinkEnabled No

# ✅ Désactiver un lien existant
Set-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" -LinkEnabled No

# ✅ Activer un lien existant
Set-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local" -LinkEnabled Yes

# ✅ Supprimer un lien GPO
Remove-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local"

# ✅ Voir tous les liens d'une OU
Get-GPInheritance -Target "OU=Ordinateurs_Test,OU=TEST_GPO,DC=entreprise,DC=local"
```

### 📌 Étape 2 : Activation pour les tests

**Scénario : Tester sur 1 utilisateur/ordinateur**

**Méthode A : Activer le lien GPO**
```
1. Dans "Gestion des stratégies de groupe"
2. Naviguer vers l'OU "Ordinateurs_Test"
3. Clic droit sur le lien "GPO_Parametres_Bureau" → ☑ Cocher "Lien activé"
```

**Méthode B : Activer la GPO elle-même**
```
1. Clic droit sur la GPO "GPO_Parametres_Bureau"
2. "Etat de la GPO" → "Activé"
```

**Joindre un poste au domaine (client Windows 10/11) :**
```
1. Sur le poste client, configurer DNS → 192.168.1.10
2. Panneau de configuration → Système → Modifier les paramètres
3. Bouton "Modifier" → Sélectionner "Domaine"
4. Entrer : entreprise.local
5. Identifiants : ENTREPRISE\Administrateur
6. Redémarrer le poste
7. Déplacer le compte ordinateur dans "OU=Ordinateurs_Test"
```

**Forcer l'application des GPO sur le poste client :**
```
1. Se connecter au poste client avec testuser1
2. Ouvrir CMD en tant qu'administrateur
3. Taper : gpupdate /force
4. Redémarrer ou rouvrir la session
```

### 💡 PowerShell pour tester les GPO

```powershell
# ✅ Sur le poste CLIENT, vérifier les GPO appliquées
gpresult /r

# ✅ Rapport HTML détaillé des GPO appliquées (CLIENT)
gpresult /h C:\rapport_gpo.html

# ✅ Voir les GPO appliquées pour un utilisateur spécifique (SERVEUR)
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\GPO_testuser1.html -User "entreprise\testuser1"

# ✅ Forcer la mise à jour des GPO depuis le serveur
Invoke-GPUpdate -Computer "PC-Test01" -Force -RandomDelayInMinutes 0

# ✅ Vérifier la dernière application des GPO (CLIENT)
gpresult /scope computer /v
gpresult /scope user /v
```

### 📌 Étape 3 : Validation et déploiement progressif

**Plan de déploiement :**
```
Semaine 1 : Tests OU TEST_GPO (2-5 utilisateurs)
┌────────────────────────────────────────────┐
│ ✅ GPO appliquée correctement              │
│ ✅ Aucun problème signalé                  │
│ ✅ Rapport de test validé                  │
└────────────────────────────────────────────┘
         ↓
Semaine 2 : Déploiement partiel (IT uniquement)
┌────────────────────────────────────────────┐
│ Lier GPO à OU "IT"                         │
│ Surveiller pendant 48h                     │
└────────────────────────────────────────────┘
         ↓
Semaine 3 : Déploiement progressif
┌────────────────────────────────────────────┐
│ Lier GPO à "Commercial"                    │
│ Puis "Comptabilité"                        │
│ Puis "Direction"                           │
└────────────────────────────────────────────┘
         ↓
Semaine 4 : Déploiement complet
┌────────────────────────────────────────────┐
│ Lier GPO à racine "ENTREPRISE_Ressources"  │
│ Surveillance continue                      │
└────────────────────────────────────────────┘
```

### 📌 En cas de problème : Rollback

**Désactiver rapidement une GPO :**
```
1. Clic droit sur le lien GPO → Décocher "Lien activé"
   OU
2. Clic droit sur la GPO → "Etat de la GPO" → "Tous les paramètres désactivés"

3. Sur les postes affectés : gpupdate /force
```

### 💡 PowerShell pour rollback

```powershell
# ✅ Désactiver IMMÉDIATEMENT une GPO
Set-GPLink -Name "GPO_Parametres_Bureau" -Target "OU=Commercial,OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local" -LinkEnabled No

# ✅ Désactiver la GPO complètement
(Get-GPO -Name "GPO_Parametres_Bureau").GpoStatus = "AllSettingsDisabled"

# ✅ Forcer la mise à jour sur TOUS les postes d'une OU
Get-ADComputer -Filter * -SearchBase "OU=Commercial,OU=Utilisateurs,OU=ENTREPRISE_Ressources,DC=entreprise,DC=local" | ForEach-Object {
    Invoke-GPUpdate -Computer $_.Name -Force -RandomDelayInMinutes 0
}

# ✅ Restaurer une GPO depuis une sauvegarde
Restore-GPO -Name "GPO_Parametres_Bureau" -Path "C:\Backup_GPO" -BackupId <GUID>
```

---

## 📊 Résumé des Commandes PowerShell Essentielles

### 🔍 Vérifications quotidiennes

```powershell
# Vérifier l'état des services critiques
Get-Service DNS, DHCP, ADWS, KDC, Netlogon | Format-Table Name, Status, StartType

# Vérifier la santé AD
dcdiag /q

# Vérifier la réplication AD
repadmin /replsummary

# Voir toutes les GPO et leur état
Get-GPO -All | Select-Object DisplayName, GpoStatus, CreationTime | Format-Table -AutoSize

# Voir les liens GPO sur une OU
Get-GPInheritance -Target "OU=TEST_GPO,DC=entreprise,DC=local"

# Statistiques DHCP
Get-DhcpServerv4ScopeStatistics

# Vérifier les zones DNS
Get-DnsServerZone | Select-Object ZoneName, ZoneType, DynamicUpdate
```

### 💾 Sauvegardes régulières

```powershell
# Sauvegarder toutes les GPO
$BackupPath = "C:\Backup_GPO\$(Get-Date -Format 'yyyy-MM-dd')"
New-Item -Path $BackupPath -ItemType Directory -Force
Backup-GPO -All -Path $BackupPath

# Sauvegarder l'état du système (AD inclus)
wbadmin start systemstatebackup -backupTarget:E: -quiet
```

---

## ✅ Checklist Finale

### Infrastructure de base
- [ ] IP statique configurée (192.168.1.10)
- [ ] DNS installé et zones créées
- [ ] DHCP installé et étendue active
- [ ] Tests réseau OK (ping, nslookup)

### Active Directory
- [ ] AD DS installé et serveur promu
- [ ] Structure OU créée
- [ ] Utilisateurs de test créés
- [ ] Réplication AD fonctionnelle

### GPO en préparation
- [ ] Minimum 3 GPO créées et configurées
- [ ] Toutes les GPO désactivées ou non liées
- [ ] Sauvegarde des GPO effectuée
- [ ] Documentation des paramètres

### Tests
- [ ] OU TEST_GPO créée avec utilisateurs/ordinateurs
- [ ] GPO testée sur 1-2 postes
- [ ] Rapport gpresult validé
- [ ] Procédure de rollback testée

### Déploiement
- [ ] Plan de déploiement progressif défini
- [ ] Surveillance mise en place
- [ ] Formation utilisateurs effectuée

---

## 🎓 Pour aller plus loin

### Commandes de dépannage avancées

```powershell
# Voir TOUTES les GPO appliquées à un utilisateur
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\rsop.html -User "entreprise\testuser1" -Computer "PC-Test01"

# Simuler l'application des GPO (mode WhatIf)
Get-GPOReport -All -ReportType Html -Path "C:\All_GPO_Report.html"

# Trouver où une GPO spécifique est liée
Get-GPO -Name "GPO_Securite_Postes" | Select-Object DisplayName
Get-ADOrganizationalUnit -Filter * | Get-GPInheritance | Where-Object {$_.GpoLinks.DisplayName -contains "GPO_Securite_Postes"}

# Vérifier les permissions sur une GPO
Get-GPPermission -Name "GPO_Securite_Postes" -All

# Log des événements GPO (sur le client)
Get-WinEvent -LogName "Microsoft-Windows-GroupPolicy/Operational" -MaxEvents 50 | Format-Table TimeCreated, Message -Wrap
```

---

**📌 Fin du guide**

Ce guide te permet de configurer une infrastructure AD complète en interface graphique, avec les commandes PowerShell pour vérifier et automatiser.

**Prochaines étapes recommandées :**
1. Pratiquer la création de GPO plus complexes (redirection de dossiers, installation logiciels)
2. Mettre en place un monitoring des GPO
3. Créer des scripts de sauvegarde automatiques
4. Documenter les GPO en production
