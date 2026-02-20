# Active Directory Domain Services (AD DS)

> 📚 **Module :** Windows Server  
> 📅 **Date :** Janvier 2026  
> ⏱️ **Durée :** 4 heures  
> 🎯 **Niveau :** Débutant

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Qu'est-ce qu'Active Directory ?](#-quest-ce-quactive-directory)
- [Installation AD DS](#-installation-ad-ds)
- [Promotion en contrôleur de domaine](#-promotion-en-contrôleur-de-domaine)
- [Structure organisationnelle](#-structure-organisationnelle)
- [Gestion des utilisateurs](#-gestion-des-utilisateurs)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Expliquer le rôle d'Active Directory dans un réseau Windows
- ✅ Installer et configurer un contrôleur de domaine
- ✅ Créer une structure d'unités d'organisation (OUs)
- ✅ Gérer des utilisateurs et groupes de sécurité
- ✅ Comprendre les concepts de domaine, forêt et arbre

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir installé Windows Server 2025
- [ ] Configurer une IP statique sur le serveur
- [ ] Comprendre les bases du réseau (TCP/IP, DNS)
- [ ] Avoir accès à une VM ou un serveur physique

**Matériel nécessaire :**
- 💻 VM Windows Server 2025 (4 GB RAM, 4 cœurs)
- 💻 VM Windows 10 (2 GB RAM, 2 cœurs)
- 🌐 Réseau VMware en mode Bridged ou NAT

---

## 📚 Qu'est-ce qu'Active Directory ?

### Définition

**Active Directory (AD)** est un service d'annuaire développé par Microsoft pour les réseaux Windows. Il permet de gérer centralement les **utilisateurs, ordinateurs et ressources** d'un réseau.

### Concepts clés

| Concept | Description |
|---------|-------------|
| **Domaine** | Unité d'organisation logique (ex: solaris.local) |
| **Contrôleur de domaine (DC)** | Serveur qui gère l'authentification et les autorisations |
| **Forêt** | Ensemble de domaines qui partagent la même configuration |
| **Arbre** | Hiérarchie de domaines dans une forêt |
| **OU (Unité d'Organisation)** | Conteneur pour organiser objets (users, computers) |
| **Groupe** | Ensemble d'utilisateurs avec permissions communes |

### Avantages d'Active Directory

✅ **Centralisation** : Gestion unique de tous les comptes  
✅ **Sécurité** : Authentification et autorisation centralisées  
✅ **GPO** : Application de stratégies de groupe automatiques  
✅ **SSO** : Single Sign-On (un mot de passe pour tout)  
✅ **Scalabilité** : Support de milliers d'utilisateurs  

### Architecture

```
Forêt : solaris.com
│
├── Domaine racine : solaris.local
│   │
│   ├── Contrôleur de domaine : WIN2025TP
│   │
│   ├── OU : Solaris_Corp
│   │   ├── OU : Direction
│   │   │   ├── User : Jean Dupont
│   │   │   └── User : Marie Martin
│   │   │
│   │   └── OU : Comptabilite
│   │       ├── User : Marie Curie
│   │       └── Group : G_Compta
│   │
│   └── Ordinateurs
│       ├── PC-WIN10-01
│       └── PC-WIN10-02
```

---

## ⚙️ Installation AD DS

### Méthode 1 : Interface graphique

1. Ouvrez le **Gestionnaire de serveur**
2. Cliquez sur **Gérer** → **Ajouter des rôles et fonctionnalités**
3. Cliquez sur **Suivant** jusqu'à **Sélection des rôles de serveurs**
4. Cochez **Services de domaine Active Directory**
5. Cliquez sur **Ajouter les fonctionnalités**
6. Continuez jusqu'à **Installer**
7. Attendez la fin de l'installation

### Méthode 2 : PowerShell

```powershell
# Installation du rôle AD DS
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# Vérification
Get-WindowsFeature -Name AD-Domain-Services
```

> 💡 **Astuce :** L'installation du rôle ne crée pas encore le domaine. C'est juste la préparation !

---

## 🚀 Promotion en contrôleur de domaine

### Pourquoi promouvoir ?

La promotion transforme votre serveur Windows en **contrôleur de domaine** Active Directory. C'est cette étape qui crée réellement le domaine.

### Méthode 1 : Interface graphique

1. Dans le **Gestionnaire de serveur**, cliquez sur le **drapeau** (coin supérieur droit)
2. Cliquez sur **Promouvoir ce serveur en contrôleur de domaine**
3. Sélectionnez **Ajouter une nouvelle forêt**
4. Nom de domaine racine : **solaris.local**
5. Cliquez sur **Suivant**
6. Définissez un **mot de passe DSRM** (ex: P@ssw0rdDSRM2026!)
7. ⚠️ **NOTEZ ce mot de passe** - il sert à restaurer AD
8. Continuez avec les valeurs par défaut
9. Cliquez sur **Installer**
10. Le serveur redémarrera automatiquement

### Méthode 2 : PowerShell

```powershell
# Promotion en DC avec création d'une nouvelle forêt
Install-ADDSForest `
    -DomainName "solaris.local" `
    -DomainNetbiosName "SOLARIS" `
    -ForestMode "WinThreshold" `
    -DomainMode "WinThreshold" `
    -InstallDns:$true `
    -Force:$true

# Le système demandera le mot de passe DSRM
# Le serveur redémarrera automatiquement
```

### Vérification après redémarrage

```powershell
# Vérifier que les services AD sont démarrés
Get-Service ADWS,KDC,NETLOGON,DNS | Format-Table Name,Status,StartType

# Vérifier le domaine
Get-ADDomain | Select-Object Name,DomainMode,Forest

# Diagnostic complet
dcdiag /q

# Tester la résolution DNS
nslookup solaris.local
```

**Résultats attendus :**
- ✅ Tous les services en "Running"
- ✅ `nslookup solaris.local` retourne l'IP du serveur
- ✅ La fenêtre de connexion affiche `SOLARIS\` avant le nom

> ⚠️ **IMPORTANT :** Le mot de passe DSRM est DIFFÉRENT du mot de passe Administrateur !

---

## 📂 Structure organisationnelle

### Qu'est-ce qu'une OU ?

Une **Unité d'Organisation (OU)** est un conteneur qui permet d'organiser les objets AD (utilisateurs, ordinateurs, groupes) de manière hiérarchique.

### Pourquoi utiliser des OUs ?

✅ **Organisation logique** : Reflète la structure de l'entreprise  
✅ **Délégation** : Permet de déléguer des droits d'administration  
✅ **GPO ciblées** : Application de stratégies spécifiques par OU  
✅ **Sécurité** : Protection contre la suppression accidentelle  

### Création d'OUs

**Interface graphique :**

1. Ouvrez **Utilisateurs et ordinateurs Active Directory**
2. Développez **solaris.local**
3. Clic droit sur **solaris.local** → **Nouveau** → **Unité d'organisation**
4. Nom : **Solaris_Corp**
5. ☑ Cochez **Protéger contre la suppression accidentelle**
6. Répétez pour créer les sous-OUs (Direction, Comptabilite)

**PowerShell :**

```powershell
# Créer l'OU principale
New-ADOrganizationalUnit -Name "Solaris_Corp" `
    -Path "DC=solaris,DC=local" `
    -ProtectedFromAccidentalDeletion $true

# Créer les sous-OUs
New-ADOrganizationalUnit -Name "Direction" `
    -Path "OU=Solaris_Corp,DC=solaris,DC=local"

New-ADOrganizationalUnit -Name "Comptabilite" `
    -Path "OU=Solaris_Corp,DC=solaris,DC=local"

# Vérifier
Get-ADOrganizationalUnit -Filter * | Select-Object Name,DistinguishedName
```

---

## 👤 Gestion des utilisateurs

### Créer un utilisateur

**Interface graphique :**

1. Naviguez vers l'OU appropriée (ex: Comptabilite)
2. Clic droit → **Nouveau** → **Utilisateur**
3. Remplissez les informations :
   - Prénom : Marie
   - Nom : Curie
   - Nom d'ouverture : mcurie
4. Définissez un mot de passe
5. ☐ Décochez "L'utilisateur doit changer..." (pour tests)

**PowerShell :**

```powershell
# Créer un utilisateur
New-ADUser -Name "Marie Curie" `
    -GivenName "Marie" `
    -Surname "Curie" `
    -SamAccountName "mcurie" `
    -UserPrincipalName "mcurie@solaris.local" `
    -Path "OU=Comptabilite,OU=Solaris_Corp,DC=solaris,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force) `
    -Enabled $true `
    -ChangePasswordAtLogon $false

# Vérifier
Get-ADUser -Identity "mcurie" | Select-Object Name,Enabled,DistinguishedName
```

### Créer un groupe de sécurité

**PowerShell :**

```powershell
# Créer un groupe
New-ADGroup -Name "G_Compta" `
    -SamAccountName "G_Compta" `
    -GroupCategory Security `
    -GroupScope Global `
    -Path "OU=Comptabilite,OU=Solaris_Corp,DC=solaris,DC=local"

# Ajouter Marie au groupe
Add-ADGroupMember -Identity "G_Compta" -Members "mcurie"

# Vérifier les membres
Get-ADGroupMember -Identity "G_Compta" | Select-Object Name,SamAccountName
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Créer une structure complète

**Objectif :** Créer une structure AD pour une entreprise fictive

**Consignes :**
1. Créez la structure suivante :
   ```
   solaris.local
   └── MonEntreprise
       ├── Direction
       ├── RH
       ├── IT
       └── Commercial
   ```
2. Créez 2 utilisateurs dans chaque OU
3. Créez un groupe de sécurité par OU

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# Créer les OUs
New-ADOrganizationalUnit -Name "MonEntreprise" -Path "DC=solaris,DC=local"
New-ADOrganizationalUnit -Name "Direction" -Path "OU=MonEntreprise,DC=solaris,DC=local"
New-ADOrganizationalUnit -Name "RH" -Path "OU=MonEntreprise,DC=solaris,DC=local"
New-ADOrganizationalUnit -Name "IT" -Path "OU=MonEntreprise,DC=solaris,DC=local"
New-ADOrganizationalUnit -Name "Commercial" -Path "OU=MonEntreprise,DC=solaris,DC=local"

# Créer les utilisateurs IT
New-ADUser -Name "John Doe" -SamAccountName "jdoe" `
    -UserPrincipalName "jdoe@solaris.local" `
    -Path "OU=IT,OU=MonEntreprise,DC=solaris,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force) `
    -Enabled $true

New-ADUser -Name "Jane Smith" -SamAccountName "jsmith" `
    -UserPrincipalName "jsmith@solaris.local" `
    -Path "OU=IT,OU=MonEntreprise,DC=solaris,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force) `
    -Enabled $true

# Créer le groupe IT
New-ADGroup -Name "G_IT" -GroupScope Global -GroupCategory Security `
    -Path "OU=IT,OU=MonEntreprise,DC=solaris,DC=local"

# Ajouter les utilisateurs au groupe
Add-ADGroupMember -Identity "G_IT" -Members "jdoe","jsmith"
```

</details>

### Exercice 2 : Réinitialisation de mot de passe

**Objectif :** Réinitialiser le mot de passe d'un utilisateur

**Consignes :**
1. Réinitialisez le mot de passe de l'utilisateur "mcurie"
2. Nouveau mot de passe : `NewP@ss2026!`
3. L'utilisateur ne doit PAS être obligé de changer le mot de passe

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# Réinitialiser le mot de passe
Set-ADAccountPassword -Identity "mcurie" -Reset `
    -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ss2026!" -Force)

# Désactiver l'obligation de changement
Set-ADUser -Identity "mcurie" -ChangePasswordAtLogon $false

# Vérifier
Get-ADUser -Identity "mcurie" -Properties PasswordLastSet,PasswordNeverExpires | `
    Select-Object Name,PasswordLastSet,PasswordNeverExpires
```

</details>

---

## 📚 Ressources

### Documentation officielle
- [Microsoft Docs - Active Directory](https://docs.microsoft.com/fr-fr/windows-server/identity/ad-ds/active-directory-domain-services)
- [PowerShell AD Module](https://docs.microsoft.com/fr-fr/powershell/module/activedirectory/)

### Tutoriels
- [IT-Connect - Active Directory](https://www.it-connect.fr/cours/active-directory/)
- [TechNet - Best Practices AD](https://social.technet.microsoft.com/wiki/contents/articles/52587.active-directory-best-practices.aspx)

### Vidéos
- [Cours complet Active Directory (YouTube)](https://www.youtube.com/watch?v=YykmeqS2ZFM&list=PLSuzYIVSEUT5ZXGVVP8LBw2B7GS2uaNhJ)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Installation du rôle AD DS
- [ ] Promotion en contrôleur de domaine
- [ ] Création de structure d'OUs
- [ ] Gestion des utilisateurs (création, modification, suppression)
- [ ] Gestion des groupes de sécurité
- [ ] Commandes PowerShell de base pour AD
- [ ] Résolution des problèmes courants (DNS, réplication)

---

<div align="center">

**Cours suivant :** [Stratégies de groupe (GPO)](./gpo-mappage-lecteurs.md)

[⬅️ Retour au sommaire](../README.md)

</div>
