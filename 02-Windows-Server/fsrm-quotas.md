# FSRM - Gestion des Quotas et Filtrage de Fichiers

> 📚 **Module :** Windows Server  
> 📅 **Date :** Janvier 2026  
> ⏱️ **Durée :** 5 heures  
> 🎯 **Niveau :** Intermédiaire  
> 🔗 **Prérequis :** Active Directory + Partages réseau configurés

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Qu'est-ce que FSRM ?](#-quest-ce-que-fsrm)
- [Installation de FSRM](#-installation-de-fsrm)
- [Quotas de stockage](#-quotas-de-stockage)
- [Filtrage de fichiers](#-filtrage-de-fichiers)
- [Rapports de stockage](#-rapports-de-stockage)
- [Gestion des fichiers](#-gestion-des-fichiers)
- [Notifications et alertes](#-notifications-et-alertes)
- [Cas pratiques d'entreprise](#-cas-pratiques-dentreprise)
- [Dépannage](#-dépannage)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Expliquer le rôle de FSRM dans la gestion du stockage
- ✅ Installer et configurer File Server Resource Manager
- ✅ Créer des quotas (durs et souples) sur des dossiers
- ✅ Filtrer des types de fichiers (bloquer .mp3, .exe, etc.)
- ✅ Générer des rapports de stockage automatiques
- ✅ Configurer des alertes par email
- ✅ Appliquer des modèles de quotas prédéfinis
- ✅ Résoudre les problèmes courants liés aux quotas

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir un serveur Windows Server 2025 configuré
- [ ] Avoir Active Directory fonctionnel
- [ ] Avoir des partages réseau créés (ex: \\SERVEUR\Donnees_Compta)
- [ ] Comprendre les permissions NTFS
- [ ] Connaître les bases de PowerShell

**Matériel nécessaire :**
- 💻 VM Windows Server 2025 (contrôleur de domaine)
- 💻 VM Windows 10 (client pour tester)
- 📁 Partages réseau existants
- 📧 Serveur SMTP configuré (optionnel, pour les notifications)

---

## 📚 Qu'est-ce que FSRM ?

### Définition

**FSRM (File Server Resource Manager)** est un rôle Windows Server qui permet de :

1. **Gérer les quotas** : Limiter l'espace disque par utilisateur/dossier
2. **Filtrer les fichiers** : Bloquer certains types de fichiers
3. **Générer des rapports** : Analyser l'utilisation du stockage
4. **Automatiser** : Tâches de gestion du stockage

### Pourquoi utiliser FSRM ?

#### Problèmes courants en entreprise :

```
❌ "Le serveur de fichiers est plein !"
❌ "Qui stocke des films sur le serveur ?"
❌ "Quelqu'un a mis un virus .exe dans le partage !"
❌ "On manque d'espace, qui utilise le plus ?"
```

#### Avec FSRM, vous résolvez tout ça :

```
✅ Quotas : Limite de 5 GB par utilisateur
✅ Filtrage : Bloque .mp3, .avi, .exe automatiquement
✅ Rapports : "Top 10 des gros fichiers"
✅ Alertes : Email quand quota à 80%
```

### Cas d'usage réels

| Scénario | Solution FSRM |
|----------|---------------|
| Utilisateurs qui stockent trop | **Quota souple** : Alerte à 5 GB, bloque à 10 GB |
| Fichiers piratés sur le serveur | **Filtrage** : Bloque .mp3, .avi, .mkv |
| Ransomware | **Filtrage** : Bloque extensions suspectes (.encrypted, .locked) |
| Audit nécessaire | **Rapports** : Liste complète des fichiers par département |
| Gestion proactive | **Alertes** : Email automatique quand espace < 20% |

---

## ⚙️ Installation de FSRM

### Méthode 1 : Interface graphique (Gestionnaire de serveur)

**Étape par étape :**

1. Ouvrez le **Gestionnaire de serveur**
2. Cliquez sur **Gérer** → **Ajouter des rôles et fonctionnalités**
3. Cliquez sur **Suivant** jusqu'à **Rôles de serveurs**
4. Développez **Services de fichiers et de stockage**
5. Développez **Services de fichiers et iSCSI**
6. Cochez **Gestionnaire de ressources du serveur de fichiers**
7. Cliquez sur **Ajouter les fonctionnalités** dans la popup
8. Cliquez sur **Suivant** jusqu'à **Installer**
9. Attendez la fin de l'installation (2-3 minutes)
10. Cliquez sur **Fermer**

### Méthode 2 : PowerShell (rapide !)

```powershell
# Installer FSRM
Install-WindowsFeature -Name FS-Resource-Manager -IncludeManagementTools

# Vérifier l'installation
Get-WindowsFeature -Name FS-Resource-Manager

# Résultat attendu :
# [X] FS-Resource-Manager    Gestionnaire de ressources du serveur de fichiers
```

### Ouvrir la console FSRM

**Méthode 1 :** Windows + R → Tapez **fsrm.msc** → OK

**Méthode 2 :** Gestionnaire de serveur → Outils → **Gestionnaire de ressources du serveur de fichiers**

### Première configuration

```powershell
# Configurer le serveur SMTP pour les notifications (optionnel)
Set-FsrmSetting -SmtpServer "smtp.solaris.local" `
    -AdminEmailAddress "admin@solaris.local" `
    -FromEmailAddress "fsrm@solaris.local"

# Vérifier la configuration
Get-FsrmSetting | Select-Object SmtpServer,AdminEmailAddress,FromEmailAddress
```

---

## 💾 Quotas de stockage

### Comprendre les quotas

Un **quota** limite l'espace disque disponible pour un dossier ou un volume.

**2 types de quotas :**

| Type | Description | Usage |
|------|-------------|-------|
| **Quota souple (Soft)** | Alerte mais n'empêche pas d'écrire | Surveillance, avertissement |
| **Quota dur (Hard)** | Bloque l'écriture à la limite | Limite stricte |

### Créer un quota souple (Soft Quota)

**Scénario :** Limiter le dossier Comptabilité à 5 GB avec alerte

**Interface graphique :**

1. Ouvrez **FSRM** (fsrm.msc)
2. Développez **Gestion de quota** dans le panneau gauche
3. Clic droit sur **Quotas** → **Créer un quota...**
4. Configuration :
   - **Chemin du quota** : C:\Donnees_Compta (ou S:\Donnees_Compta)
   - **Créer un quota sur le chemin** : ☑ Coché
   - **Propriétés de quota** :
     - ☑ Définir des propriétés de quota personnalisées
     - Limite : **5 GB**
     - Type : **Quota souple** (Soft quota)
5. Cliquez sur l'onglet **Seuils d'alerte**
6. Ajoutez un seuil :
   - Pourcentage : **80%** (alerte à 4 GB)
   - ☑ Envoyer un message électronique
   - À : admin@solaris.local
   - Objet : "Quota Comptabilité à 80%"
7. Cliquez sur **OK**

**PowerShell :**

```powershell
# Créer un quota souple de 5 GB
New-FsrmQuota -Path "C:\Donnees_Compta" `
    -Size 5GB `
    -SoftLimit `
    -Description "Quota souple Comptabilité"

# Ajouter un seuil d'alerte à 80%
$action = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "Quota Comptabilité à 80%" `
    -Body "Le dossier Comptabilité a atteint 80% de sa limite (4 GB sur 5 GB)."

New-FsrmQuotaThreshold -Percentage 80 -Action $action

# Vérifier
Get-FsrmQuota -Path "C:\Donnees_Compta"
```

### Créer un quota dur (Hard Quota)

**Scénario :** Bloquer l'écriture dans le dossier Utilisateurs à 10 GB

**PowerShell :**

```powershell
# Créer un quota dur de 10 GB
New-FsrmQuota -Path "C:\Users_Data" `
    -Size 10GB `
    -Description "Quota dur utilisateurs - Bloque à 10 GB"

# Note : Pas de -SoftLimit = quota dur par défaut

# Ajouter une alerte à 90%
$action = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "URGENT : Quota utilisateurs à 90%" `
    -Body "Le dossier utilisateurs a atteint 90% (9 GB sur 10 GB). Espace sera bloqué à 10 GB."

New-FsrmQuotaThreshold -Percentage 90 -Action $action
```

### Utiliser des modèles de quota

FSRM inclut des **modèles prédéfinis** très pratiques.

**Modèles intégrés :**

| Modèle | Limite | Type | Usage |
|--------|--------|------|-------|
| 100 MB Limit | 100 MB | Dur | Test, petits dossiers |
| 200 MB Limit | 200 MB | Dur | Dossiers personnels limités |
| Monitor 200 GB Volume Usage | 200 GB | Souple | Surveillance disques |
| Monitor 500 GB Volume Usage | 500 GB | Souple | Surveillance gros disques |

**Appliquer un modèle :**

```powershell
# Lister les modèles disponibles
Get-FsrmQuotaTemplate | Select-Object Name,Size,SoftLimit

# Créer un quota basé sur un modèle
New-FsrmQuota -Path "C:\Test_Quota" `
    -Template "200 MB Limit"

# Vérifier
Get-FsrmQuota -Path "C:\Test_Quota"
```

**Créer votre propre modèle :**

```powershell
# Créer un modèle personnalisé
New-FsrmQuotaTemplate -Name "Quota_5GB_Souple" `
    -Size 5GB `
    -SoftLimit `
    -Description "Modèle 5 GB souple pour départements"

# Ajouter des seuils au modèle
$action80 = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "Alerte : Quota à 80%" `
    -Body "Le dossier a atteint 80% de sa limite."

New-FsrmQuotaTemplateThreshold -Name "Quota_5GB_Souple" `
    -Percentage 80 -Action $action80

# Appliquer le modèle à un dossier
New-FsrmQuota -Path "C:\Direction" -Template "Quota_5GB_Souple"
New-FsrmQuota -Path "C:\IT" -Template "Quota_5GB_Souple"
```

### Quota automatique (Auto-apply)

**Concept :** Appliquer automatiquement un quota à chaque sous-dossier créé.

**Exemple :** Chaque utilisateur a son dossier dans C:\Users_Data\, quota automatique de 2 GB.

```powershell
# Créer un quota automatique
New-FsrmAutoQuota -Path "C:\Users_Data" `
    -Template "200 MB Limit"

# Maintenant, chaque dossier créé dans C:\Users_Data\ 
# aura automatiquement un quota de 200 MB

# Test : Créer un dossier
New-Item -Path "C:\Users_Data\mcurie" -ItemType Directory

# Vérifier que le quota est appliqué
Get-FsrmQuota -Path "C:\Users_Data\mcurie"
```

---

## 🚫 Filtrage de fichiers

### Comprendre le filtrage

Le **filtrage de fichiers** permet de **bloquer** certains types de fichiers sur le serveur.

**Cas d'usage :**

| Problème | Types de fichiers à bloquer |
|----------|----------------------------|
| Fichiers piratés | .mp3, .avi, .mkv, .mp4 |
| Exécutables dangereux | .exe, .bat, .cmd, .vbs |
| Fichiers temporaires | .tmp, .temp, ~ |
| Fichiers système | .dll, .sys |
| Ransomware | .encrypted, .locked, .crypto |

### Groupes de fichiers prédéfinis

FSRM inclut des **groupes de fichiers** prêts à l'emploi.

```powershell
# Lister les groupes de fichiers
Get-FsrmFileGroup | Select-Object Name,IncludePattern

# Exemples :
# Audio and Video Files    : *.mp3, *.avi, *.mp4, *.wmv, etc.
# Executable Files         : *.exe, *.dll, *.bat, *.cmd
# E-mail Files             : *.pst, *.ost
# Office Files             : *.doc, *.xls, *.ppt
# Compressed Files         : *.zip, *.rar, *.7z
```

### Créer un filtre de fichiers

**Scénario :** Bloquer les fichiers audio/vidéo sur le partage Comptabilité

**Interface graphique :**

1. Ouvrez **FSRM** (fsrm.msc)
2. Développez **Gestion du filtrage de fichiers**
3. Clic droit sur **Filtres de fichiers** → **Créer un filtre de fichiers...**
4. Configuration :
   - **Chemin du filtre** : C:\Donnees_Compta
   - ☑ **Bloquer l'enregistrement de types de fichiers**
   - Groupes de fichiers : Cochez **Audio and Video Files**
5. Onglet **Notification par courrier électronique** :
   - ☑ Envoyer un message à l'administrateur
   - À : admin@solaris.local
6. Cliquez sur **OK**

**PowerShell :**

```powershell
# Créer un filtre de fichiers
New-FsrmFileScreen -Path "C:\Donnees_Compta" `
    -IncludeGroup "Audio and Video Files" `
    -Description "Bloque les fichiers multimédia"

# Vérifier
Get-FsrmFileScreen -Path "C:\Donnees_Compta"

# Test : Essayer de copier un .mp3
# → Accès refusé ! ✅
```

### Créer un groupe de fichiers personnalisé

**Exemple :** Bloquer les fichiers suspects de ransomware

```powershell
# Créer un groupe de fichiers
New-FsrmFileGroup -Name "Ransomware_Extensions" `
    -IncludePattern @("*.encrypted","*.locked","*.crypto","*.crypt","*.WNCRY","*.zzzzz","*.locky") `
    -Description "Extensions typiques de ransomware"

# Appliquer le filtre
New-FsrmFileScreen -Path "C:\Donnees_Compta" `
    -IncludeGroup "Ransomware_Extensions" `
    -Description "Protection anti-ransomware"

# Avec notification email
$notification = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "ALERTE : Tentative de ransomware détectée !" `
    -Body "Un fichier suspect a été bloqué sur le serveur."

New-FsrmFileScreen -Path "C:\Donnees_Compta" `
    -IncludeGroup "Ransomware_Extensions" `
    -Notification $notification
```

### Exceptions au filtrage

**Scénario :** Bloquer les .exe partout SAUF dans le dossier IT

```powershell
# 1. Créer le filtre général
New-FsrmFileScreen -Path "C:\Partages" `
    -IncludeGroup "Executable Files"

# 2. Créer une exception pour le dossier IT
New-FsrmFileScreenException -Path "C:\Partages\IT" `
    -IncludeGroup "Executable Files" `
    -Description "Les IT peuvent mettre des .exe"

# Résultat :
# C:\Partages\**       → .exe bloqués ❌
# C:\Partages\IT\**    → .exe autorisés ✅
```

### Modèles de filtrage

**Créer un modèle réutilisable :**

```powershell
# Créer un modèle de filtrage
New-FsrmFileScreenTemplate -Name "Blocage_Multimedia" `
    -IncludeGroup "Audio and Video Files" `
    -Description "Bloque audio/vidéo"

# Appliquer le modèle
New-FsrmFileScreen -Path "C:\Direction" -Template "Blocage_Multimedia"
New-FsrmFileScreen -Path "C:\Compta" -Template "Blocage_Multimedia"
New-FsrmFileScreen -Path "C:\RH" -Template "Blocage_Multimedia"

# Modifier le modèle (se répercute partout)
Set-FsrmFileScreenTemplate -Name "Blocage_Multimedia" `
    -IncludeGroup "Audio and Video Files","Compressed Files"
```

---

## 📊 Rapports de stockage

### Types de rapports disponibles

FSRM peut générer **7 types de rapports** :

| Rapport | Description |
|---------|-------------|
| **Fichiers volumineux** | Top 50/100 des plus gros fichiers |
| **Fichiers récents** | Fichiers créés/modifiés récemment |
| **Fichiers en double** | Fichiers identiques (nom, taille) |
| **Fichiers par propriétaire** | Qui possède quels fichiers |
| **Fichiers par groupe** | Fichiers par type (.doc, .xls, etc.) |
| **Quota le moins utilisé** | Quotas avec le plus d'espace libre |
| **Quota le plus utilisé** | Quotas presque pleins |

### Générer un rapport manuel

**Interface graphique :**

1. Ouvrez **FSRM** (fsrm.msc)
2. Clic droit sur **Gestion des rapports de stockage** → **Générer des rapports maintenant...**
3. Configuration :
   - **Volumes ou dossiers** : C:\Donnees_Compta
   - **Types de rapports** : Cochez **Fichiers volumineux** et **Quota le plus utilisé**
   - **Formats de rapport** : HTML (pour visualisation)
4. Cliquez sur **OK**
5. Le rapport s'ouvre dans votre navigateur ! 🎉

**PowerShell :**

```powershell
# Générer un rapport de fichiers volumineux
New-FsrmStorageReport -Name "Rapport_Gros_Fichiers" `
    -Namespace "C:\Donnees_Compta" `
    -ReportType LargeFiles `
    -LargeFileMinimum 10MB `
    -LargeFilePattern "*" `
    -Interactive

# Le rapport s'ouvre automatiquement
```

### Planifier des rapports automatiques

**Scénario :** Rapport hebdomadaire tous les lundis à 8h

**Interface graphique :**

1. Clic droit sur **Gestion des rapports de stockage** → **Planifier une nouvelle tâche de rapport...**
2. Configuration :
   - **Nom** : Rapport_Hebdomadaire_Stockage
   - **Volumes** : C:\, S:\
   - **Types de rapports** :
     - ☑ Fichiers volumineux
     - ☑ Quota le plus utilisé
     - ☑ Fichiers par propriétaire
   - **Format** : HTML
3. Onglet **Remise** :
   - ☑ Envoyer les rapports par courrier électronique
   - À : admin@solaris.local
4. Onglet **Planification** :
   - Chaque semaine : Lundi
   - Heure : 08:00
5. Cliquez sur **OK**

**PowerShell :**

```powershell
# Créer une tâche de rapport planifiée
$task = New-FsrmScheduledTask -Time "08:00" -Weekly Monday

New-FsrmStorageReport -Name "Rapport_Hebdomadaire" `
    -Namespace "C:\","S:\" `
    -ReportType @("LargeFiles","QuotaUsage","FilesByOwner") `
    -Schedule $task `
    -MailTo "admin@solaris.local"

# Vérifier les tâches planifiées
Get-FsrmScheduledTask
```

### Personnaliser les rapports

```powershell
# Rapport avancé : Fichiers > 100 MB, créés dans les 30 derniers jours
New-FsrmStorageReport -Name "Rapport_Personnalisé" `
    -Namespace "C:\Partages" `
    -ReportType LargeFiles `
    -LargeFileMinimum 100MB `
    -FileAge 30 `
    -Interactive

# Rapport de fichiers par extension
New-FsrmStorageReport -Name "Rapport_Extensions" `
    -Namespace "C:\Partages" `
    -ReportType FilesByFileGroup `
    -Interactive
```

---

## 🔔 Notifications et alertes

### Types de notifications

FSRM peut envoyer **4 types de notifications** :

| Type | Description | Usage |
|------|-------------|-------|
| **Email** | Envoyer un email | Alerte administrateur |
| **Journal des événements** | Écrire dans Event Viewer | Monitoring centralisé |
| **Commande** | Exécuter un script | Automatisation |
| **Rapport** | Générer un rapport | Analyse détaillée |

### Configurer les notifications par email

```powershell
# Configuration SMTP globale
Set-FsrmSetting -SmtpServer "smtp.solaris.local" `
    -AdminEmailAddress "admin@solaris.local" `
    -FromEmailAddress "fsrm@solaris.local"

# Tester l'envoi d'email
Test-FsrmEmail -ToEmailAddress "admin@solaris.local" `
    -Subject "Test FSRM" `
    -Body "Si vous recevez ceci, les notifications fonctionnent !"
```

### Exemple : Alerte quota à 90%

```powershell
# Créer une action email
$emailAction = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "[URGENT] Quota à 90% - [Source Folder]" `
    -Body "Le dossier [Source Folder Path] a atteint 90% de sa limite ([Quota Limit]). 
    Espace utilisé : [Quota Used]
    Espace libre : [Quota Available]
    
    Action requise : Libérer de l'espace ou augmenter le quota."

# Créer un quota avec cette alerte
New-FsrmQuota -Path "C:\Direction" -Size 10GB
New-FsrmQuotaThreshold -Path "C:\Direction" -Percentage 90 -Action $emailAction
```

**Variables disponibles dans les emails :**

| Variable | Description |
|----------|-------------|
| [Source Folder] | Nom du dossier |
| [Source Folder Path] | Chemin complet |
| [Quota Limit] | Limite du quota |
| [Quota Used] | Espace utilisé |
| [Quota Available] | Espace libre |
| [Quota Used Percent] | Pourcentage utilisé |

### Exemple : Exécuter un script à 95%

```powershell
# Créer un script de nettoyage
$scriptPath = "C:\Scripts\Cleanup_TempFiles.ps1"
$scriptContent = @"
# Script de nettoyage automatique
Remove-Item "C:\Direction\*.tmp" -Force -ErrorAction SilentlyContinue
Remove-Item "C:\Direction\~*" -Force -ErrorAction SilentlyContinue
Write-Host "Fichiers temporaires supprimés"
"@

Set-Content -Path $scriptPath -Value $scriptContent

# Créer une action de commande
$commandAction = New-FsrmAction -Type Command `
    -Command "powershell.exe" `
    -CommandParameters "-File $scriptPath" `
    -RunLimitInterval 60

# Appliquer au quota
New-FsrmQuotaThreshold -Path "C:\Direction" -Percentage 95 -Action $commandAction
```

---

## 🏢 Cas pratiques d'entreprise

### Cas 1 : Entreprise de 50 employés

**Besoin :**
- Chaque utilisateur a un dossier personnel limité à 5 GB
- Bloquer les fichiers multimédias
- Rapport mensuel

**Solution :**

```powershell
# 1. Créer le modèle de quota
New-FsrmQuotaTemplate -Name "Quota_Utilisateur_5GB" `
    -Size 5GB `
    -Description "Quota par utilisateur"

# 2. Appliquer automatiquement aux dossiers utilisateurs
New-FsrmAutoQuota -Path "D:\Users" -Template "Quota_Utilisateur_5GB"

# 3. Créer le filtre multimédia
New-FsrmFileScreenTemplate -Name "Blocage_Multimedia" `
    -IncludeGroup "Audio and Video Files"

New-FsrmFileScreen -Path "D:\Users" -Template "Blocage_Multimedia"

# 4. Rapport mensuel
$task = New-FsrmScheduledTask -Time "08:00" -Monthly 1  # 1er du mois
New-FsrmStorageReport -Name "Rapport_Mensuel_Users" `
    -Namespace "D:\Users" `
    -ReportType @("QuotaUsage","FilesByOwner") `
    -Schedule $task `
    -MailTo "admin@solaris.local"
```

---

### Cas 2 : Cabinet comptable

**Besoin :**
- Dossier Clients limité à 50 GB (souple)
- Bloquer les .exe (sécurité)
- Alerte à 40 GB
- Rapport hebdomadaire des gros fichiers

**Solution :**

```powershell
# 1. Quota souple 50 GB
New-FsrmQuota -Path "E:\Clients" -Size 50GB -SoftLimit

# 2. Alerte à 80% (40 GB)
$emailAction = New-FsrmAction -Type Email `
    -MailTo "admin@cabinet.com" `
    -Subject "Dossier Clients à 80%" `
    -Body "Le dossier Clients a atteint 40 GB sur 50 GB autorisés."

New-FsrmQuotaThreshold -Path "E:\Clients" -Percentage 80 -Action $emailAction

# 3. Bloquer les .exe
New-FsrmFileScreen -Path "E:\Clients" -IncludeGroup "Executable Files"

# 4. Rapport hebdomadaire
$task = New-FsrmScheduledTask -Time "07:00" -Weekly Monday
New-FsrmStorageReport -Name "Rapport_Clients" `
    -Namespace "E:\Clients" `
    -ReportType LargeFiles `
    -LargeFileMinimum 50MB `
    -Schedule $task `
    -MailTo "admin@cabinet.com"
```

---

### Cas 3 : Protection anti-ransomware

**Besoin :**
- Détecter et bloquer les extensions de ransomware
- Notification immédiate
- Enregistrer dans Event Viewer

**Solution :**

```powershell
# 1. Créer le groupe de fichiers ransomware
$ransomwareExtensions = @(
    "*.encrypted","*.locked","*.crypto","*.crypt",
    "*.WNCRY","*.locky","*.cerber","*.zepto",
    "*.zzzzz","*.micro","*.cryptolocker","*.vault"
)

New-FsrmFileGroup -Name "Ransomware" `
    -IncludePattern $ransomwareExtensions `
    -Description "Extensions typiques de ransomware"

# 2. Notification email immédiate
$emailAction = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local","security@solaris.local" `
    -Subject "[ALERTE SÉCURITÉ] Ransomware détecté !" `
    -Body "ATTENTION : Une tentative de ransomware a été détectée.
    
    Fichier : [File Path]
    Utilisateur : [Source Io Owner]
    Heure : [Violation Time]
    
    ACTION IMMÉDIATE REQUISE :
    1. Déconnecter le poste utilisateur du réseau
    2. Analyser avec antivirus
    3. Restaurer depuis sauvegarde si nécessaire"

# 3. Enregistrer dans Event Viewer
$eventAction = New-FsrmAction -Type Event `
    -EventType Warning `
    -Body "Ransomware détecté : [File Path]"

# 4. Exécuter un script d'isolation
$isolateScript = "C:\Scripts\Isolate_User.ps1"
$cmdAction = New-FsrmAction -Type Command `
    -Command "powershell.exe" `
    -CommandParameters "-File $isolateScript -User [Source Io Owner]"

# 5. Créer le filtre avec toutes les actions
New-FsrmFileScreen -Path "C:\Partages" `
    -IncludeGroup "Ransomware" `
    -Notification $emailAction,$eventAction,$cmdAction `
    -Description "Protection anti-ransomware"
```

---

## 🔧 Dépannage

### Problème 1 : "Le quota ne fonctionne pas"

**Symptôme :** L'utilisateur dépasse le quota sans être bloqué

**Diagnostic :**

```powershell
# Vérifier que le quota existe
Get-FsrmQuota -Path "C:\Dossier"

# Vérifier le type de quota
Get-FsrmQuota -Path "C:\Dossier" | Select-Object Size,SoftLimit
# Si SoftLimit = True → C'est un quota souple (alerte seulement)
# Si SoftLimit = False → Quota dur (bloque)

# Vérifier l'utilisation actuelle
Get-FsrmQuota -Path "C:\Dossier" | Select-Object Size,Usage,Percent
```

**Solutions :**
1. Si quota souple → Normal, c'est juste une alerte
2. Convertir en quota dur : `Set-FsrmQuota -Path "C:\Dossier" -SoftLimit:$false`
3. Vérifier que le service SrmSvc est démarré : `Get-Service SrmSvc`

---

### Problème 2 : "Le filtrage ne bloque pas les fichiers"

**Symptôme :** Les utilisateurs peuvent toujours copier des .mp3

**Diagnostic :**

```powershell
# Vérifier que le filtre existe
Get-FsrmFileScreen -Path "C:\Partages"

# Vérifier les groupes de fichiers inclus
Get-FsrmFileScreen -Path "C:\Partages" | Select-Object -ExpandProperty IncludeGroup

# Vérifier le groupe "Audio and Video Files"
Get-FsrmFileGroup -Name "Audio and Video Files" | Select-Object -ExpandProperty IncludePattern
```

**Solutions :**
1. Vérifier que *.mp3 est bien dans le groupe
2. Appliquer le filtre au bon chemin (pas de sous-dossier)
3. Redémarrer le service : `Restart-Service SrmSvc`

---

### Problème 3 : "Les emails ne sont pas envoyés"

**Symptôme :** Aucune notification par email

**Diagnostic :**

```powershell
# Vérifier la configuration SMTP
Get-FsrmSetting | Select-Object SmtpServer,AdminEmailAddress,FromEmailAddress

# Tester l'envoi d'email
Test-FsrmEmail -ToEmailAddress "admin@solaris.local"
```

**Solutions :**
1. Configurer le serveur SMTP : `Set-FsrmSetting -SmtpServer "smtp.domain.com"`
2. Vérifier que le serveur SMTP autorise les relais depuis le serveur de fichiers
3. Vérifier le pare-feu (port 25)

---

### Problème 4 : "Les rapports ne se génèrent pas"

**Symptôme :** Les rapports planifiés ne sont pas créés

**Diagnostic :**

```powershell
# Vérifier les tâches planifiées
Get-FsrmScheduledTask

# Vérifier dans le Planificateur de tâches Windows
# Ouvrir : taskschd.msc
# Chercher : Microsoft\Windows\FileServerResourceManager

# Vérifier les logs
Get-EventLog -LogName Application -Source "FSRM" -Newest 50
```

**Solutions :**
1. Vérifier que le service Planificateur de tâches est démarré
2. Re-créer la tâche planifiée
3. Tester manuellement : `Start-FsrmStorageReport -Name "Nom_Rapport"`

---

## 🎯 Exercices pratiques

### Exercice 1 : Quotas de base

**Objectif :** Créer un quota dur de 1 GB sur un dossier test

**Consignes :**
1. Créez le dossier C:\Test_Quota
2. Créez un quota dur de 1 GB
3. Ajoutez une alerte à 80%
4. Testez en copiant des fichiers jusqu'à atteindre la limite

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# 1. Créer le dossier
New-Item -Path "C:\Test_Quota" -ItemType Directory

# 2. Créer le quota dur de 1 GB
New-FsrmQuota -Path "C:\Test_Quota" -Size 1GB

# 3. Ajouter une alerte à 80%
$emailAction = New-FsrmAction -Type Email `
    -MailTo "admin@solaris.local" `
    -Subject "Quota Test à 80%" `
    -Body "Le dossier Test_Quota a atteint 800 MB sur 1 GB."

New-FsrmQuotaThreshold -Path "C:\Test_Quota" -Percentage 80 -Action $emailAction

# 4. Test : Copier un gros fichier
# fsutil file createnew C:\Test_Quota\test.bin 1073741824  # 1 GB
# → Erreur : Quota dépassé ! ✅

# Vérifier l'utilisation
Get-FsrmQuota -Path "C:\Test_Quota" | Select-Object Path,Size,Usage,Percent
```

</details>

---

### Exercice 2 : Filtrage anti-piratage

**Objectif :** Bloquer les fichiers multimédias sur un partage

**Consignes :**
1. Créez un dossier C:\Partage_Entreprise
2. Bloquez les fichiers audio et vidéo
3. Testez en essayant de copier un .mp3

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# 1. Créer le dossier
New-Item -Path "C:\Partage_Entreprise" -ItemType Directory

# 2. Créer le filtre
New-FsrmFileScreen -Path "C:\Partage_Entreprise" `
    -IncludeGroup "Audio and Video Files" `
    -Description "Bloque multimédia"

# 3. Test : Essayer de copier un fichier .mp3
# Copy-Item "C:\Music\song.mp3" "C:\Partage_Entreprise\"
# → Erreur : L'opération n'a pas pu se terminer ! ✅

# Vérifier le filtre
Get-FsrmFileScreen -Path "C:\Partage_Entreprise"
```

</details>

---

### Exercice 3 : Rapport personnalisé

**Objectif :** Générer un rapport des fichiers de plus de 10 MB

**Consignes :**
1. Générez un rapport de fichiers volumineux (> 10 MB)
2. Sur le dossier C:\
3. Format HTML

*Solution laissée en exercice*

---

## 📚 Ressources complémentaires

### Documentation officielle

- [Microsoft Docs - FSRM](https://docs.microsoft.com/fr-fr/windows-server/storage/fsrm/fsrm-overview)
- [PowerShell FSRM Cmdlets](https://docs.microsoft.com/en-us/powershell/module/fileserverresourcemanager/)

### Scripts utiles

- [FSRM Anti-Ransomware](https://github.com/search?q=fsrm+ransomware) (GitHub)
- [Quota Automation Scripts](https://github.com/search?q=fsrm+quota) (GitHub)

### Outils

- **Storage Reports Viewer** : Inclus avec FSRM
- **Event Viewer** : Pour les logs FSRM
- **PowerShell ISE** : Pour créer des scripts

---

## ✅ Checklist de révision

Avant de passer au module suivant, vous devez maîtriser :

- [ ] Installer FSRM sur Windows Server
- [ ] Créer des quotas souples et durs
- [ ] Utiliser des modèles de quotas
- [ ] Appliquer des quotas automatiques
- [ ] Créer des groupes de fichiers personnalisés
- [ ] Bloquer des types de fichiers spécifiques
- [ ] Créer des exceptions au filtrage
- [ ] Générer des rapports de stockage manuels
- [ ] Planifier des rapports automatiques
- [ ] Configurer des notifications par email
- [ ] Diagnostiquer les problèmes de quotas
- [ ] Mettre en place une protection anti-ransomware

---

<div align="center">

**Cours suivant :** [Déduplication des données](./deduplication-donnees.md)

**Cours précédent :** [Serveur FTP](./serveur-ftp.md)

[⬅️ Retour au sommaire](../../README.md)

---

### 💾 "FSRM : Gérer l'espace disque avant qu'il ne soit trop tard !"

*Bon courage pour la mise en place de vos quotas et filtres !* 🚀

</div>
