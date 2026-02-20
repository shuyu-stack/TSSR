# 🚀 Guide GPO Avancées - Configurations Professionnelles

**Prérequis :** Avoir suivi le guide de base et avoir une infrastructure AD fonctionnelle

---

## 📑 Table des matières

1. [Installation de logiciels via GPO](#1-installation-de-logiciels-via-gpo)
2. [Scripts de démarrage/arrêt/ouverture/fermeture](#2-scripts-de-démarragearrêtouverturefermeture)
3. [Redirection de dossiers (Mes Documents, Bureau, etc.)](#3-redirection-de-dossiers)
4. [Gestion de l'alimentation](#4-gestion-de-lalimentation)
5. [Paramètres de sécurité avancés](#5-paramètres-de-sécurité-avancés)
6. [Déploiement d'imprimantes](#6-déploiement-dimprimantes)
7. [Configuration du pare-feu Windows](#7-configuration-du-pare-feu-windows)
8. [Restrictions logicielles (AppLocker)](#8-restrictions-logicielles-applocker)
9. [Configuration des navigateurs (Edge/Chrome)](#9-configuration-des-navigateurs)
10. [Paramètres de verrouillage automatique](#10-paramètres-de-verrouillage-automatique)

---

## 1️⃣ Installation de logiciels via GPO

### 📌 Méthode 1 : Installation via fichier MSI

**Prérequis :**
- Fichier .MSI du logiciel
- Partage réseau accessible par les postes

**Étape 1 : Créer le partage réseau**
```
1. Sur le serveur, créer un dossier : C:\Logiciels
2. Y placer les fichiers MSI (ex: 7zip.msi, vlc.msi)
3. Clic droit sur le dossier → Propriétés → Partage → Partage avancé

┌────────────────────────────────────────┐
│ ☑ Partager ce dossier                  │
│ Nom de partage : Logiciels             │
│ → Autorisations                        │
│   Groupe : Utilisateurs du domaine     │
│   ☑ Lecture                            │
│ → OK                                   │
└────────────────────────────────────────┘

4. Onglet Sécurité → Modifier
   Ajouter : "Ordinateurs du domaine"
   Permissions : Lecture et exécution
```

**Étape 2 : Créer la GPO d'installation**
```
1. Gestion des stratégies de groupe
2. Créer une nouvelle GPO : GPO_Installation_Logiciels
3. Clic droit → Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres logiciels
          └── Installation de logiciel

4. Clic droit → Nouveau → Package...
5. Parcourir vers : \\serveur01\Logiciels\7zip.msi
   ⚠️ IMPORTANT : Utiliser le chemin UNC (\\serveur\partage), PAS le lecteur local
```

**Étape 3 : Méthode de déploiement**
```
Fenêtre "Déployer le logiciel" :
┌────────────────────────────────────────┐
│ ● Attribué                             │
│   → Installation automatique           │
│      au démarrage du PC                │
│                                        │
│ ○ Publié                               │
│   → Disponible dans "Programmes        │
│      et fonctionnalités"               │
│   (Utilisé pour installations          │
│    optionnelles)                       │
│                                        │
│ ○ Avancé                               │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 4 : Options avancées (optionnel)**
```
1. Double-clic sur le package 7zip dans la liste
2. Onglet "Déploiement"

┌────────────────────────────────────────┐
│ Type de déploiement :                  │
│ ● Attribué                             │
│                                        │
│ Options de déploiement :               │
│ ☑ Désinstaller cette application       │
│   lorsqu'elle n'est plus gérée         │
│ ☑ Ne pas afficher ce package dans      │
│   Ajout/Suppression de programmes      │
│                                        │
│ Options d'installation :               │
│ ● Installation de base                 │
│ ○ Installation maximale                │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 5 : Lier et activer**
```
1. Lier la GPO à "OU=Ordinateurs"
2. Sur un poste client : gpupdate /force puis redémarrer
3. Le logiciel s'installe automatiquement au démarrage
```

### 💡 PowerShell pour gérer les installations

```powershell
# ✅ Créer le partage réseau
New-SmbShare -Name "Logiciels" -Path "C:\Logiciels" -ReadAccess "ENTREPRISE\Utilisateurs du domaine"

# ✅ Vérifier les packages déployés dans une GPO
Get-GPO -Name "GPO_Installation_Logiciels" | Get-GPRegistryValue

# ✅ Voir les logiciels installés sur un poste distant
Get-WmiObject -Class Win32_Product -ComputerName "PC-Test01" | Select-Object Name, Version

# ✅ Forcer la réinstallation d'un package (sur le client)
msiexec /fam {CODE-PRODUIT}

# ✅ Désinstaller un logiciel via GPO (modifier le package)
# Interface : Clic droit sur le package → Toutes les tâches → Supprimer
# Options : "Désinstaller immédiatement le logiciel"
```

### ⚠️ Problèmes courants

```
Problème : Le logiciel ne s'installe pas
Solutions :
1. Vérifier les permissions NTFS et partage
2. Vérifier le chemin UNC (\\serveur\partage, PAS C:\...)
3. Vérifier que le fichier MSI n'est pas corrompu
4. Vérifier les logs : C:\Windows\Debug\UserMode\gpsvc.log
5. Événements : Observateur d'événements → Applications

Problème : Installation en boucle
Solution : Décocher "Installation de base" et cocher "Installation maximale"

Problème : Erreur 1274 (fichier inaccessible)
Solution : Ajouter "Ordinateurs du domaine" aux permissions de lecture
```

---

## 2️⃣ Scripts de démarrage/arrêt/ouverture/fermeture

### 📌 Types de scripts

```
┌─────────────────────────────────────────────────────────────┐
│ TYPE SCRIPT          │ QUAND ?              │ CONTEXTE       │
├──────────────────────┼──────────────────────┼────────────────┤
│ Démarrage (Startup)  │ Au boot du PC        │ SYSTEM         │
│ Arrêt (Shutdown)     │ A l'extinction du PC │ SYSTEM         │
│ Ouverture (Logon)    │ Connexion utilisateur│ Utilisateur    │
│ Fermeture (Logoff)   │ Déconnexion          │ Utilisateur    │
└─────────────────────────────────────────────────────────────┘
```

### 📌 Exemple 1 : Script de mappage de lecteurs (Logon)

**Étape 1 : Créer le script PowerShell**
```powershell
# Fichier : MapLecteurs.ps1
# Emplacement : \\serveur01\netlogon\scripts\MapLecteurs.ps1

# Supprimer les lecteurs existants
Remove-PSDrive -Name P -ErrorAction SilentlyContinue
Remove-PSDrive -Name H -ErrorAction SilentlyContinue

# Mapper le lecteur Public
New-PSDrive -Name "P" -PSProvider FileSystem -Root "\\serveur01\public" -Persist

# Mapper le lecteur personnel (Home)
$username = $env:USERNAME
New-PSDrive -Name "H" -PSProvider FileSystem -Root "\\serveur01\home\$username" -Persist

# Log pour debug
$logFile = "\\serveur01\logs\$username-mappage.log"
"$(Get-Date) - Lecteurs mappés pour $username" | Out-File $logFile -Append
```

**Étape 2 : Placer le script dans NETLOGON**
```
1. Sur le serveur, aller dans : C:\Windows\SYSVOL\sysvol\entreprise.local\scripts
   (ou \\serveur01\netlogon)
2. Créer un sous-dossier : "scripts"
3. Y copier MapLecteurs.ps1
```

**Étape 3 : Créer la GPO de script d'ouverture**
```
1. Créer GPO : GPO_Script_Mappage
2. Modifier la GPO

Configuration Utilisateur
  ├── Stratégies
      ├── Paramètres Windows
          └── Scripts (Ouverture/Fermeture de session)

3. Double-clic sur "Ouverture de session"

┌────────────────────────────────────────┐
│ Onglet "PowerShell Scripts"            │
│ → Ajouter                              │
│                                        │
│ Nom du script :                        │
│ \\serveur01\netlogon\scripts\          │
│   MapLecteurs.ps1                      │
│                                        │
│ Paramètres du script : (vide)         │
│ → OK                                   │
└────────────────────────────────────────┘

4. ⚠️ IMPORTANT : Activer l'exécution PowerShell

Configuration Utilisateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Système
              └── Scripts

5. Double-clic sur "Activer l'exécution des scripts PowerShell"
   → Activé
   Stratégie d'exécution : Autoriser les scripts locaux et les scripts signés distants
```

**Étape 4 : Tester**
```
1. Lier la GPO à OU=Utilisateurs_Test
2. Se connecter avec testuser1
3. Vérifier les lecteurs P: et H: sont mappés
```

### 📌 Exemple 2 : Script de nettoyage (Démarrage)

**Script de nettoyage des fichiers temporaires**
```powershell
# Fichier : Nettoyage.ps1
# Emplacement : \\serveur01\netlogon\scripts\Nettoyage.ps1

# Nettoyer les fichiers temp Windows
Remove-Item -Path "C:\Windows\Temp\*" -Force -Recurse -ErrorAction SilentlyContinue

# Nettoyer les fichiers temp utilisateurs (si profils présents)
$profiles = Get-ChildItem "C:\Users" -Directory | Where-Object { $_.Name -notmatch "Public|Default|All Users" }

foreach ($profile in $profiles) {
    $tempPath = Join-Path $profile.FullName "AppData\Local\Temp"
    if (Test-Path $tempPath) {
        Remove-Item -Path "$tempPath\*" -Force -Recurse -ErrorAction SilentlyContinue
    }
}

# Nettoyer le cache DNS
ipconfig /flushdns | Out-Null

# Log
$logFile = "\\serveur01\logs\nettoyage-$(Get-Date -Format 'yyyy-MM-dd').log"
"$(Get-Date) - Nettoyage effectué sur $env:COMPUTERNAME" | Out-File $logFile -Append
```

**Configuration dans la GPO**
```
1. Créer GPO : GPO_Nettoyage_Demarrage
2. Modifier la GPO

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          └── Scripts (Démarrage/Arrêt)

3. Double-clic sur "Démarrage"
4. Onglet PowerShell Scripts → Ajouter
5. Nom du script : \\serveur01\netlogon\scripts\Nettoyage.ps1
```

### 📌 Exemple 3 : Script de sauvegarde (Fermeture de session)

```powershell
# Fichier : Sauvegarde_Bureau.ps1
# Sauvegarde automatique du Bureau à la déconnexion

$username = $env:USERNAME
$desktopPath = [Environment]::GetFolderPath("Desktop")
$backupPath = "\\serveur01\backups\$username\Desktop_$(Get-Date -Format 'yyyy-MM-dd_HHmm')"

# Créer le dossier de sauvegarde
New-Item -Path $backupPath -ItemType Directory -Force | Out-Null

# Copier les fichiers du Bureau
Copy-Item -Path "$desktopPath\*" -Destination $backupPath -Recurse -Force -ErrorAction SilentlyContinue

# Conserver seulement les 7 dernières sauvegardes
$allBackups = Get-ChildItem "\\serveur01\backups\$username" | Sort-Object CreationTime -Descending
if ($allBackups.Count -gt 7) {
    $allBackups | Select-Object -Skip 7 | Remove-Item -Recurse -Force
}

# Log
$logFile = "\\serveur01\logs\$username-sauvegarde.log"
"$(Get-Date) - Sauvegarde effectuée : $backupPath" | Out-File $logFile -Append
```

### 💡 PowerShell pour gérer les scripts

```powershell
# ✅ Voir les scripts configurés dans une GPO
Get-GPRegistryValue -Name "GPO_Script_Mappage" -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts"

# ✅ Tester un script manuellement
Invoke-Command -ComputerName "PC-Test01" -FilePath "\\serveur01\netlogon\scripts\MapLecteurs.ps1"

# ✅ Voir les logs d'exécution des scripts (sur le client)
Get-EventLog -LogName System -Source "Group Policy Scripts" -Newest 10

# ✅ Forcer l'exécution immédiate des scripts d'ouverture
gpupdate /force /logoff

# ✅ Débugger un script
# Ajouter dans le script :
Start-Transcript -Path "\\serveur01\logs\debug-$env:USERNAME.log"
# ... votre code ...
Stop-Transcript
```

### ⚠️ Bonnes pratiques pour les scripts

```
1. Toujours utiliser le chemin UNC (\\serveur\partage)
2. Gérer les erreurs avec -ErrorAction SilentlyContinue
3. Logger les actions pour le debug
4. Tester les scripts manuellement AVANT de les déployer
5. Ne pas bloquer l'ouverture de session (scripts rapides)
6. Utiliser des scripts PowerShell plutôt que .bat (plus puissants)
7. Signer les scripts en production (pour la sécurité)
```

---

## 3️⃣ Redirection de dossiers

### 📌 Pourquoi rediriger les dossiers ?

```
Avantages :
✅ Sauvegarde centralisée automatique
✅ Profil utilisateur léger (connexion rapide)
✅ Accès aux documents depuis n'importe quel poste
✅ Simplification de la migration de postes
```

### 📌 Prérequis : Créer le partage réseau

**Étape 1 : Créer la structure de dossiers**
```
1. Sur le serveur : Créer C:\Donnees_Utilisateurs
2. Créer les sous-dossiers :

C:\Donnees_Utilisateurs\
├── Mes_Documents\
├── Bureau\
├── Images\
├── Telechargements\
└── AppData_Roaming\

3. Partager le dossier principal

Clic droit sur "Donnees_Utilisateurs" → Propriétés → Partage avancé
┌────────────────────────────────────────┐
│ ☑ Partager ce dossier                  │
│ Nom de partage : Donnees$              │
│ (Le $ le rend caché)                   │
│ → Autorisations                        │
│   Utilisateurs du domaine :            │
│   ☑ Contrôle total                     │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 2 : Permissions NTFS**
```
1. Propriétés → Sécurité → Avancé
2. Désactiver l'héritage → Convertir

3. Ajouter :
┌────────────────────────────────────────┐
│ Principal : SYSTEM                     │
│ Type : Autoriser                       │
│ Permissions : Contrôle total           │
│                                        │
│ Principal : Administrateurs            │
│ Type : Autoriser                       │
│ Permissions : Contrôle total           │
│                                        │
│ Principal : Utilisateurs du domaine    │
│ Type : Autoriser                       │
│ Permissions :                          │
│   ☑ Parcourir le dossier               │
│   ☑ Créer des dossiers                 │
│   (Pas de lecture des autres dossiers) │
│                                        │
│ Principal : CREATOR OWNER              │
│ Type : Autoriser                       │
│ Permissions : Contrôle total           │
│ S'applique à : Sous-dossiers et        │
│                fichiers uniquement     │
└────────────────────────────────────────┘
```

### 📌 Configuration de la redirection

**Étape 3 : Créer la GPO de redirection**
```
1. Nouvelle GPO : GPO_Redirection_Dossiers
2. Modifier

Configuration Utilisateur
  ├── Stratégies
      ├── Paramètres Windows
          └── Redirection de dossiers

3. Clic droit sur "Documents" → Propriétés

┌────────────────────────────────────────┐
│ Onglet Cible :                         │
│ Paramètre : ● De base                  │
│                                        │
│ Emplacement du dossier cible :         │
│ ● Créer un dossier pour chaque        │
│   utilisateur sous le chemin racine    │
│                                        │
│ Chemin racine :                        │
│ \\serveur01\Donnees$\Mes_Documents     │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘

4. Onglet Paramètres :
┌────────────────────────────────────────┐
│ ☑ Accorder à l'utilisateur des droits │
│   exclusifs                            │
│                                        │
│ ☑ Déplacer le contenu de Documents    │
│   vers le nouvel emplacement           │
│                                        │
│ Suppression de stratégie :             │
│ ● Laisser le dossier au nouvel        │
│   emplacement                          │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 4 : Rediriger d'autres dossiers**
```
Répéter pour :
├── Bureau → \\serveur01\Donnees$\Bureau
├── Images → \\serveur01\Donnees$\Images
├── Téléchargements → \\serveur01\Donnees$\Telechargements
└── AppData (Roaming) → \\serveur01\Donnees$\AppData_Roaming
```

### 💡 PowerShell pour la redirection

```powershell
# ✅ Créer le partage réseau
New-SmbShare -Name "Donnees$" -Path "C:\Donnees_Utilisateurs" -FullAccess "ENTREPRISE\Utilisateurs du domaine"

# ✅ Configurer les permissions NTFS
$acl = Get-Acl "C:\Donnees_Utilisateurs"
$acl.SetAccessRuleProtection($true, $false)

# SYSTEM - Contrôle total
$systemRule = New-Object System.Security.AccessControl.FileSystemAccessRule("SYSTEM", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")
$acl.AddAccessRule($systemRule)

# Administrateurs - Contrôle total
$adminRule = New-Object System.Security.AccessControl.FileSystemAccessRule("Administrateurs", "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")
$acl.AddAccessRule($adminRule)

# Utilisateurs du domaine - Créer des dossiers
$userRule = New-Object System.Security.AccessControl.FileSystemAccessRule("Utilisateurs du domaine", "CreateDirectories,Traverse", "ContainerInherit", "None", "Allow")
$acl.AddAccessRule($userRule)

# Appliquer
Set-Acl "C:\Donnees_Utilisateurs" $acl

# ✅ Vérifier la redirection sur un poste client
# (Exécuter sur le client après connexion)
[Environment]::GetFolderPath("MyDocuments")
[Environment]::GetFolderPath("Desktop")

# ✅ Forcer la redirection immédiate
gpupdate /force /logoff
```

### ⚠️ Problèmes courants

```
Problème : "Impossible d'accéder au dossier"
Solutions :
1. Vérifier les permissions NTFS et partage
2. Vérifier que l'utilisateur a bien "Contrôle total" sur SON dossier
3. Vérifier que CREATOR OWNER est bien configuré

Problème : Les fichiers ne se déplacent pas
Solution : Cocher "Déplacer le contenu vers le nouvel emplacement"

Problème : Connexion très lente après redirection
Solutions :
1. Ne PAS rediriger AppData (sauf Roaming si nécessaire)
2. Exclure les fichiers volumineux (.pst, .ost)
3. Utiliser des fichiers hors ligne pour le Bureau

Problème : Profil temporaire après redirection
Solution : Supprimer le cache de profil local
  C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat*
```

---

## 4️⃣ Gestion de l'alimentation

### 📌 Créer un plan d'alimentation personnalisé

**Étape 1 : Créer la GPO**
```
1. Nouvelle GPO : GPO_Alimentation_Bureautique
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Système
              └── Gestion de l'alimentation

3. Ouvrir "Options d'alimentation"
```

**Étape 2 : Configurer les paramètres**
```
Paramètres pour postes de bureau :
┌─────────────────────────────────────────────┐
│ Sélectionner un modèle de gestion de        │
│ l'alimentation actif :                      │
│ → Activé                                    │
│ Modèle : Performances élevées               │
│ → OK                                        │
│                                             │
│ Arrêter l'affichage (sur secteur) :        │
│ → Activé                                    │
│ Délai : 15 minutes                          │
│                                             │
│ Mettre en veille (sur secteur) :           │
│ → Activé                                    │
│ Délai : 30 minutes                          │
│                                             │
│ Arrêter les disques durs (sur secteur) :   │
│ → Activé                                    │
│ Délai : 20 minutes                          │
└─────────────────────────────────────────────┘
```

**Paramètres pour portables :**
```
Créer GPO : GPO_Alimentation_Portables

┌─────────────────────────────────────────────┐
│ Modèle : Utilisation normale                │
│                                             │
│ Sur secteur :                               │
│   Écran : 10 min                            │
│   Veille : 20 min                           │
│                                             │
│ Sur batterie :                              │
│   Écran : 5 min                             │
│   Veille : 10 min                           │
│                                             │
│ Niveau de batterie critique :              │
│   15% → Mise en veille prolongée            │
└─────────────────────────────────────────────┘
```

### 💡 PowerShell pour l'alimentation

```powershell
# ✅ Voir les plans d'alimentation disponibles (sur le client)
powercfg /list

# ✅ Activer le plan "Performances élevées"
powercfg /setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c

# ✅ Configurer le délai de mise en veille
powercfg /change standby-timeout-ac 30    # Sur secteur (30 min)
powercfg /change standby-timeout-dc 10    # Sur batterie (10 min)

# ✅ Configurer l'extinction de l'écran
powercfg /change monitor-timeout-ac 15    # Sur secteur (15 min)
powercfg /change monitor-timeout-dc 5     # Sur batterie (5 min)

# ✅ Désactiver la mise en veille hybride
powercfg /hibernate off

# ✅ Exporter un plan d'alimentation
powercfg /export "C:\plan_personnalise.pow" 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c

# ✅ Importer un plan d'alimentation
powercfg /import "\\serveur01\netlogon\plans\plan_bureautique.pow"

# ✅ Voir tous les paramètres d'alimentation
powercfg /query
```

---

## 5️⃣ Paramètres de sécurité avancés

### 📌 Stratégie de verrouillage de compte

**Configuration via GPO**
```
1. Nouvelle GPO : GPO_Securite_Verrouillage
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Stratégies de comptes
                  └── Stratégie de verrouillage de compte

Paramètres recommandés :
┌─────────────────────────────────────────────┐
│ Seuil de verrouillage du compte :          │
│ → 5 tentatives non valides                  │
│                                             │
│ Durée du verrouillage de compte :          │
│ → 30 minutes                                │
│                                             │
│ Réinitialiser le compteur après :          │
│ → 30 minutes                                │
└─────────────────────────────────────────────┘
```

### 📌 Stratégie d'audit avancée

**Configuration de l'audit**
```
Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Configuration de la stratégie d'audit avancée
                  └── Stratégies d'audit

Activer les audits suivants :
┌─────────────────────────────────────────────┐
│ Ouverture de session de compte :           │
│   ☑ Réussite                                │
│   ☑ Échec                                   │
│                                             │
│ Gestion des comptes :                      │
│   ☑ Réussite                                │
│   ☑ Échec                                   │
│                                             │
│ Accès à l'objet DS :                       │
│   ☑ Réussite                                │
│   ☑ Échec                                   │
│                                             │
│ Événements d'ouverture de session :        │
│   ☑ Réussite                                │
│   ☑ Échec                                   │
│                                             │
│ Modification de stratégie :                │
│   ☑ Réussite                                │
│                                             │
│ Utilisation de privilèges :                │
│   ☑ Échec                                   │
└─────────────────────────────────────────────┘
```

### 📌 Restriction d'exécution de logiciels

**Via Stratégies de restriction logicielle**
```
1. Créer GPO : GPO_Restriction_Logiciels
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Stratégies de restriction logicielle

3. Clic droit → "Créer une nouvelle stratégie"

4. Clic droit sur "Niveaux de sécurité" → "Non autorisé"
   → Définir par défaut
   (Tout est bloqué par défaut)

5. Créer des exceptions :
   Clic droit sur "Règles supplémentaires" → "Nouvelle règle de chemin d'accès"

Exceptions à créer :
┌─────────────────────────────────────────────┐
│ Chemin : C:\Program Files\*                 │
│ Niveau de sécurité : Non restreint          │
│                                             │
│ Chemin : C:\Program Files (x86)\*           │
│ Niveau de sécurité : Non restreint          │
│                                             │
│ Chemin : C:\Windows\*                       │
│ Niveau de sécurité : Non restreint          │
│                                             │
│ Chemin : %AppData%\Microsoft\*              │
│ Niveau de sécurité : Non restreint          │
└─────────────────────────────────────────────┘

⚠️ Attention : BIEN TESTER avant déploiement !
   Risque de bloquer des applications légitimes
```

### 💡 PowerShell pour la sécurité

```powershell
# ✅ Voir les comptes verrouillés
Search-ADAccount -LockedOut | Select-Object Name, SamAccountName, LockedOut, LastLogonDate

# ✅ Déverrouiller un compte
Unlock-ADAccount -Identity "testuser1"

# ✅ Réinitialiser le compteur de tentatives
# (se fait automatiquement après le délai configuré)

# ✅ Voir les événements de sécurité (échecs de connexion)
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4625} -MaxEvents 10 | Format-Table TimeCreated, Message -Wrap

# ✅ Voir les connexions réussies
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4624} -MaxEvents 10

# ✅ Activer l'audit depuis PowerShell
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
auditpol /set /subcategory:"Account Lockout" /success:enable /failure:enable

# ✅ Voir la configuration d'audit actuelle
auditpol /get /category:*
```

---

## 6️⃣ Déploiement d'imprimantes

### 📌 Méthode 1 : Via Préférences GPO

**Étape 1 : Installer l'imprimante sur le serveur**
```
1. Sur le serveur, installer les pilotes de l'imprimante
2. Panneau de configuration → Périphériques et imprimantes
3. Ajouter une imprimante
4. Configurer l'imprimante réseau
5. Clic droit → "Propriétés de l'imprimante" → Partage
   ☑ Partager cette imprimante
   Nom de partage : ImpCompta01
```

**Étape 2 : Déployer via GPO**
```
1. Créer GPO : GPO_Imprimantes_Compta
2. Modifier

Configuration Utilisateur
  ├── Préférences
      ├── Paramètres du Panneau de configuration
          └── Imprimantes

3. Clic droit → Nouveau → Imprimante partagée

┌────────────────────────────────────────┐
│ Action : ● Créer                       │
│                                        │
│ Chemin de partage :                    │
│ \\serveur01\ImpCompta01                │
│                                        │
│ Emplacement : Comptabilité - Bureau 12│
│ Commentaire : Imprimante HP LaserJet   │
│                                        │
│ ☑ Définir comme imprimante par défaut  │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 3 : Ciblage par groupe**
```
1. Dans la fenêtre de configuration, onglet "Commun"
2. Cliquer "Ciblage au niveau de l'élément..."

┌────────────────────────────────────────┐
│ Clic "Nouvel élément" →                │
│ "Groupe de sécurité"                   │
│                                        │
│ Groupe : ENTREPRISE\GRP_Comptabilite   │
│ Utilisateur dans le groupe             │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘

L'imprimante ne sera déployée QUE pour les membres du groupe
```

### 💡 PowerShell pour les imprimantes

```powershell
# ✅ Lister les imprimantes sur le serveur
Get-Printer | Select-Object Name, DriverName, PortName, Shared

# ✅ Partager une imprimante
Set-Printer -Name "HP LaserJet" -Shared $true -ShareName "ImpCompta01"

# ✅ Installer une imprimante réseau sur un client
Add-Printer -ConnectionName "\\serveur01\ImpCompta01"

# ✅ Définir une imprimante par défaut
$printer = Get-CimInstance -ClassName Win32_Printer -Filter "Name='\\\\serveur01\\ImpCompta01'"
Invoke-CimMethod -InputObject $printer -MethodName SetDefaultPrinter

# ✅ Supprimer une imprimante
Remove-Printer -Name "\\serveur01\ImpCompta01"

# ✅ Voir les imprimantes installées sur un poste distant
Get-Printer -ComputerName "PC-Compta01"

# ✅ Déployer une imprimante à tous les membres d'un groupe
$membres = Get-ADGroupMember -Identity "GRP_Comptabilite"
foreach ($membre in $membres) {
    $computerName = (Get-ADUser $membre -Properties *).LastLogonWorkstation
    if ($computerName) {
        Invoke-Command -ComputerName $computerName -ScriptBlock {
            Add-Printer -ConnectionName "\\serveur01\ImpCompta01"
        }
    }
}
```

---

## 7️⃣ Configuration du pare-feu Windows

### 📌 Activer le pare-feu Windows via GPO

**Étape 1 : Créer la GPO**
```
1. Nouvelle GPO : GPO_Pare_Feu_Windows
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Pare-feu Windows Defender avec fonctions avancées de sécurité
```

**Étape 2 : Configurer les profils**
```
1. Clic droit sur "Pare-feu Windows Defender..." → Propriétés

Onglet "Profil de domaine" :
┌────────────────────────────────────────┐
│ État du pare-feu : Activé               │
│ Connexions entrantes : Bloquer          │
│ Connexions sortantes : Autoriser        │
│                                        │
│ Journalisation :                       │
│   Taille maximale : 16384 Ko           │
│   Enregistrer les paquets abandonnés : │
│   Oui                                  │
└────────────────────────────────────────┘

Répéter pour "Profil privé" et "Profil public"
```

**Étape 3 : Créer des règles de pare-feu**
```
1. Développer "Pare-feu Windows Defender..."
2. Clic droit sur "Règles de trafic entrant" → "Nouvelle règle..."

Exemple : Autoriser Bureau à distance (RDP)
┌────────────────────────────────────────┐
│ Type de règle :                        │
│ ● Prédéfinie                           │
│ Sélectionner : Bureau à distance       │
│ → Suivant                              │
│                                        │
│ Sélectionner les règles :              │
│ ☑ Bureau à distance (TCP-Entrant)      │
│ → Suivant                              │
│                                        │
│ Action :                               │
│ ● Autoriser la connexion               │
│ → Terminer                             │
└────────────────────────────────────────┘

Exemple 2 : Autoriser un port personnalisé
┌────────────────────────────────────────┐
│ Type de règle : ● Port                 │
│ → Suivant                              │
│                                        │
│ ● TCP                                  │
│ Ports locaux spécifiques : 8080        │
│ → Suivant                              │
│                                        │
│ Action : ● Autoriser la connexion      │
│ → Suivant                              │
│                                        │
│ Profils : ☑ Domaine                    │
│ → Suivant                              │
│                                        │
│ Nom : Autoriser_App_Interne_8080      │
│ → Terminer                             │
└────────────────────────────────────────┘
```

### 💡 PowerShell pour le pare-feu

```powershell
# ✅ Activer le pare-feu sur tous les profils
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True

# ✅ Créer une règle pour autoriser RDP
New-NetFirewallRule -DisplayName "Autoriser RDP" -Direction Inbound -Protocol TCP -LocalPort 3389 -Action Allow -Profile Domain

# ✅ Créer une règle pour autoriser un port personnalisé
New-NetFirewallRule -DisplayName "App Interne Port 8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow -Profile Domain

# ✅ Créer une règle pour autoriser un programme
New-NetFirewallRule -DisplayName "Autoriser App.exe" -Direction Inbound -Program "C:\Program Files\MonApp\app.exe" -Action Allow

# ✅ Bloquer une IP spécifique
New-NetFirewallRule -DisplayName "Bloquer IP suspecte" -Direction Inbound -RemoteAddress 192.168.1.50 -Action Block

# ✅ Lister toutes les règles actives
Get-NetFirewallRule | Where-Object {$_.Enabled -eq $true} | Select-Object DisplayName, Direction, Action

# ✅ Désactiver une règle
Set-NetFirewallRule -DisplayName "Autoriser RDP" -Enabled False

# ✅ Supprimer une règle
Remove-NetFirewallRule -DisplayName "Autoriser RDP"

# ✅ Voir les connexions bloquées (logs)
Get-WinEvent -FilterHashtable @{LogName='Security';ID=5157} -MaxEvents 20 | Format-Table TimeCreated, Message -Wrap
```

---

## 8️⃣ Restrictions logicielles (AppLocker)

### 📌 Configuration AppLocker via GPO

**Étape 1 : Créer la GPO**
```
1. Nouvelle GPO : GPO_AppLocker
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Stratégies de contrôle d'application
                  └── AppLocker
```

**Étape 2 : Configurer les règles par défaut**
```
1. Clic droit sur "Règles exécutables" → "Créer les règles par défaut"
   (Autorise les fichiers dans Windows et Program Files)

2. Clic droit sur "Règles Windows Installer" → "Créer les règles par défaut"

3. Clic droit sur "Règles de script" → "Créer les règles par défaut"
```

**Étape 3 : Créer une règle de blocage**
```
Exemple : Bloquer l'exécution depuis le Bureau et Téléchargements

1. Clic droit sur "Règles exécutables" → "Créer une nouvelle règle..."

┌────────────────────────────────────────┐
│ Permissions :                          │
│ ● Refuser                              │
│ Groupe : Tout le monde                 │
│ → Suivant                              │
│                                        │
│ Conditions :                           │
│ ● Chemin d'accès                       │
│ → Suivant                              │
│                                        │
│ Chemin : %USERPROFILE%\Desktop\*       │
│ → Suivant                              │
│                                        │
│ Exceptions : (aucune)                  │
│ → Suivant                              │
│                                        │
│ Nom : Bloquer_Execution_Bureau         │
│ → Créer                                │
└────────────────────────────────────────┘

2. Répéter pour %USERPROFILE%\Downloads\*
```

**Étape 4 : Autoriser une application spécifique**
```
1. Nouvelle règle → Autoriser

┌────────────────────────────────────────┐
│ Permissions : ● Autoriser              │
│ Groupe : Tout le monde                 │
│                                        │
│ Conditions : ● Éditeur                 │
│ (Utilise la signature numérique)       │
│                                        │
│ Parcourir : C:\Program Files\7-Zip\    │
│             7zFM.exe                   │
│                                        │
│ Nom : Autoriser_7Zip                   │
└────────────────────────────────────────┘
```

**Étape 5 : Activer AppLocker**
```
⚠️ IMPORTANT : Activer le service "Identity d'application"

Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Services système

1. Double-clic sur "Identity d'application"
2. ☑ Définir ce paramètre de stratégie
3. Mode de démarrage : Automatique
4. → OK
```

### 💡 PowerShell pour AppLocker

```powershell
# ✅ Voir les règles AppLocker configurées
Get-AppLockerPolicy -Effective -Xml | Out-File C:\AppLockerPolicy.xml
Get-AppLockerPolicy -Effective | Select-Object -ExpandProperty RuleCollections

# ✅ Tester si un fichier serait bloqué (mode audit)
Test-AppLockerPolicy -Path "C:\Users\test\Desktop\malware.exe" -User "ENTREPRISE\testuser1"

# ✅ Créer une règle AppLocker par chemin
$rule = New-AppLockerPolicy -RuleType Path -Path "C:\Apps\MonApp\*" -User "Tout le monde" -Action Allow -RuleNamePrefix "Autoriser_MonApp"

# ✅ Appliquer la stratégie
Set-AppLockerPolicy -PolicyObject $rule

# ✅ Exporter la stratégie AppLocker
Get-AppLockerPolicy -Effective -Xml | Out-File "\\serveur01\backup\AppLocker_$(Get-Date -Format 'yyyy-MM-dd').xml"

# ✅ Activer le service Application Identity
Set-Service -Name AppIDSvc -StartupType Automatic
Start-Service -Name AppIDSvc
```

---

## 9️⃣ Configuration des navigateurs

### 📌 Configuration Microsoft Edge via GPO

**Étape 1 : Télécharger les modèles d'administration Edge**
```
1. Télécharger depuis : https://www.microsoft.com/edge/business/download
2. Extraire le fichier .zip
3. Copier les fichiers .admx dans : C:\Windows\PolicyDefinitions\
4. Copier les fichiers .adml dans : C:\Windows\PolicyDefinitions\fr-FR\
```

**Étape 2 : Créer la GPO**
```
1. Nouvelle GPO : GPO_Config_Edge
2. Modifier

Configuration Ordinateur (ou Utilisateur)
  ├── Stratégies
      ├── Modèles d'administration
          └── Microsoft Edge
```

**Étape 3 : Paramètres recommandés**
```
Paramètres de sécurité :
┌────────────────────────────────────────┐
│ Définir la page d'accueil :            │
│ → Activé                               │
│ URL : https://intranet.entreprise.local│
│                                        │
│ Empêcher le contournement des messages│
│ Microsoft Defender SmartScreen :       │
│ → Activé                               │
│                                        │
│ Activer l'authentification pour       │
│ les applications et sites :            │
│ → Activé                               │
│                                        │
│ Bloquer les téléchargements dangereux :│
│ → Activé                               │
└────────────────────────────────────────┘

Paramètres de productivité :
┌────────────────────────────────────────┐
│ Définir les favoris de l'entreprise :  │
│ → Activé                               │
│ URL du fichier JSON de favoris :       │
│ \\serveur01\netlogon\edge_bookmarks.json│
│                                        │
│ Empêcher les modifications de favoris :│
│ → Activé                               │
│                                        │
│ Désactiver la synchronisation :        │
│ → Activé                               │
└────────────────────────────────────────┘
```

### 📌 Configuration Google Chrome via GPO

**Étape 1 : Télécharger les modèles Chrome**
```
1. Télécharger : https://chromeenterprise.google/browser/download/
2. Extraire policy_templates.zip
3. Copier windows\admx\chrome.admx → C:\Windows\PolicyDefinitions\
4. Copier windows\admx\fr-FR\chrome.adml → C:\Windows\PolicyDefinitions\fr-FR\
```

**Étape 2 : Configuration**
```
1. Nouvelle GPO : GPO_Config_Chrome
2. Modifier

Configuration Ordinateur
  ├── Stratégies
      ├── Modèles d'administration
          └── Google Chrome

Paramètres :
┌────────────────────────────────────────┐
│ Page d'accueil :                       │
│ → Activé                               │
│ URL : https://intranet.entreprise.local│
│                                        │
│ Bloquer les extensions sauf            │
│ liste blanche :                        │
│ → Activé                               │
│ ID extensions : <liste des ID>         │
│                                        │
│ Désactiver le mode navigation privée : │
│ → Activé                               │
└────────────────────────────────────────┘
```

### 💡 PowerShell pour les navigateurs

```powershell
# ✅ Voir la version de Edge installée
Get-AppxPackage -Name Microsoft.MicrosoftEdge | Select-Object Name, Version

# ✅ Désinstaller Edge (pas recommandé !)
# N/A - Edge est intégré à Windows 10/11

# ✅ Définir Edge comme navigateur par défaut
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\http\UserChoice" -Name "ProgId" -Value "MSEdgeHTM"
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\https\UserChoice" -Name "ProgId" -Value "MSEdgeHTM"

# ✅ Créer un fichier de favoris Edge (JSON)
$bookmarks = @{
    "Intranet" = "https://intranet.entreprise.local"
    "Webmail" = "https://mail.entreprise.local"
    "Support IT" = "https://support.entreprise.local"
} | ConvertTo-Json

$bookmarks | Out-File "\\serveur01\netlogon\edge_bookmarks.json"
```

---

## 🔟 Paramètres de verrouillage automatique

### 📌 Configuration du verrouillage automatique

**Étape 1 : Créer la GPO**
```
1. Nouvelle GPO : GPO_Verrouillage_Auto
2. Modifier

Configuration Ordinateur (ou Utilisateur)
  ├── Stratégies
      ├── Modèles d'administration
          ├── Système
              └── Ctrl+Alt+Suppr Options
```

**Étape 2 : Paramètres de verrouillage**
```
1. Double-clic sur "Supprimer le verrouillage de l'ordinateur"
   → Désactivé (pour AUTORISER le verrouillage)

Configuration Utilisateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Système
              └── Ctrl+Alt+Suppr Options

2. "Supprimer le verrouillage de l'ordinateur"
   → Désactivé
```

**Étape 3 : Écran de veille avec mot de passe**
```
Configuration Utilisateur
  ├── Stratégies
      ├── Modèles d'administration
          ├── Panneau de configuration
              └── Personnalisation

Paramètres :
┌────────────────────────────────────────┐
│ Activer l'écran de veille :            │
│ → Activé                               │
│                                        │
│ Délai d'expiration de l'écran de       │
│ veille :                               │
│ → Activé                               │
│ Secondes : 900 (15 minutes)            │
│                                        │
│ Protection par mot de passe de         │
│ l'écran de veille :                    │
│ → Activé                               │
│                                        │
│ Forcer un écran de veille spécifique : │
│ → Activé                               │
│ Nom de l'écran : scrnsave.scr          │
│ (Écran noir)                           │
└────────────────────────────────────────┘
```

**Étape 4 : Verrouillage après inactivité**
```
Configuration Ordinateur
  ├── Stratégies
      ├── Paramètres Windows
          ├── Paramètres de sécurité
              └── Stratégies locales
                  └── Options de sécurité

1. "Ouverture de session interactive : Seuil d'inactivité de l'ordinateur"
   → 900 secondes (15 minutes)
```

### 💡 PowerShell pour le verrouillage

```powershell
# ✅ Verrouiller immédiatement le poste
rundll32.exe user32.dll,LockWorkStation

# ✅ Configurer le délai de verrouillage (Registre)
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "InactivityTimeoutSecs" -Value 900

# ✅ Activer l'écran de veille avec mot de passe
Set-ItemProperty -Path "HKCU:\Control Panel\Desktop" -Name "ScreenSaveActive" -Value 1
Set-ItemProperty -Path "HKCU:\Control Panel\Desktop" -Name "ScreenSaverIsSecure" -Value 1
Set-ItemProperty -Path "HKCU:\Control Panel\Desktop" -Name "ScreenSaveTimeOut" -Value 900

# ✅ Forcer le verrouillage après fermeture du capot (portable)
powercfg /SETDCVALUEINDEX SCHEME_CURRENT 4f971e89-eebd-4455-a8de-9e59040e7347 5ca83367-6e45-459f-a27b-476b1d01c936 1
powercfg /SETACVALUEINDEX SCHEME_CURRENT 4f971e89-eebd-4455-a8de-9e59040e7347 5ca83367-6e45-459f-a27b-476b1d01c936 1

# ✅ Désactiver le verrouillage automatique (déconseillé)
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "InactivityTimeoutSecs" -Value 0
```

---

## 📊 Résumé des GPO Avancées

### 📋 Liste des GPO créées

```
GPO_Installation_Logiciels       → Déploiement MSI automatique
GPO_Script_Mappage               → Scripts de connexion (lecteurs réseau)
GPO_Nettoyage_Demarrage          → Scripts de démarrage (nettoyage)
GPO_Redirection_Dossiers         → Redirection Documents/Bureau/etc.
GPO_Alimentation_Bureautique     → Gestion alimentation postes fixes
GPO_Alimentation_Portables       → Gestion alimentation portables
GPO_Securite_Verrouillage        → Verrouillage compte après échecs
GPO_Audit_Securite               → Audit des événements
GPO_Restriction_Logiciels        → Blocage exécution dans certains dossiers
GPO_Imprimantes_Compta           → Déploiement imprimantes par service
GPO_Pare_Feu_Windows             → Configuration pare-feu + règles
GPO_AppLocker                    → Contrôle d'application avancé
GPO_Config_Edge                  → Configuration Microsoft Edge
GPO_Config_Chrome                → Configuration Google Chrome
GPO_Verrouillage_Auto            → Écran de veille + verrouillage auto
```

### ✅ Checklist de déploiement

```
Avant le déploiement :
- [ ] Toutes les GPO testées sur OU=TEST_GPO
- [ ] Scripts testés manuellement
- [ ] Partages réseau créés avec bonnes permissions
- [ ] Sauvegarde de toutes les GPO effectuée
- [ ] Documentation à jour
- [ ] Plan de rollback préparé

Pendant le déploiement :
- [ ] Déploiement progressif (TEST → IT → Services)
- [ ] Surveillance logs événements
- [ ] Test de connexion utilisateur
- [ ] Vérification fonctionnement applications
- [ ] Collecte feedback utilisateurs

Après le déploiement :
- [ ] Rapport de déploiement créé
- [ ] Incidents documentés
- [ ] Ajustements GPO si nécessaire
- [ ] Formation utilisateurs effectuée
- [ ] Monitoring continu en place
```

### 🚨 Commandes de dépannage rapide

```powershell
# Sur le CLIENT :

# Forcer la mise à jour des GPO
gpupdate /force

# Voir les GPO appliquées
gpresult /r
gpresult /h C:\rapport_gpo.html

# Voir les erreurs GPO
Get-WinEvent -LogName "Microsoft-Windows-GroupPolicy/Operational" -MaxEvents 20

# Réinitialiser les GPO (mode sans échec)
rd /s /q "%WinDir%\System32\GroupPolicy"
gpupdate /force

# Sur le SERVEUR :

# Voir toutes les GPO
Get-GPO -All | Format-Table DisplayName, GpoStatus

# Sauvegarder toutes les GPO
Backup-GPO -All -Path "C:\Backup_GPO\$(Get-Date -Format 'yyyy-MM-dd')"

# Restaurer une GPO
Restore-GPO -Name "GPO_Installation_Logiciels" -Path "C:\Backup_GPO\2026-01-14"

# Forcer la réplication AD
repadmin /syncall /AdeP
```

---

## 🎓 Pour aller encore plus loin

### Sujets avancés à explorer :

1. **GPO Loopback Processing** - Appliquer des GPO utilisateur basées sur l'ordinateur
2. **WMI Filters** - Appliquer des GPO selon critères (OS, RAM, etc.)
3. **Starter GPOs** - Modèles de GPO réutilisables
4. **Central Store** - Centraliser les modèles ADMX
5. **GPO Preferences Extensions** - Variables d'environnement, tâches planifiées
6. **Security Baselines Microsoft** - Templates de sécurité préconfigurés
7. **LAPS** (Local Admin Password Solution) - Gestion mots de passe admin locaux
8. **Just Enough Administration (JEA)** - Délégation contrôle PowerShell

---

**📌 Fin du guide GPO Avancées**

Tu as maintenant une bibliothèque complète de configurations GPO professionnelles, toutes avec interface graphique ET commandes PowerShell pour automatisation et dépannage !
