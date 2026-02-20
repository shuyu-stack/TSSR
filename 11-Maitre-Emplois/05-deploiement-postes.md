# Déploiement et Configuration des Postes de Travail

> 📚 **Module :** Maître Emplois - Mission 05
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 8-10 heures
> 🎯 **Niveau :** N2-N3 (Intermédiaire à Avancé)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction au déploiement](#-introduction-au-déploiement)
- [Imaging et Masterisation](#-imaging-et-masterisation)
- [MDT - Microsoft Deployment Toolkit](#-mdt---microsoft-deployment-toolkit)
- [WDS - Windows Deployment Services](#-wds---windows-deployment-services)
- [Migration de données](#-migration-de-données)
- [Configuration post-déploiement](#-configuration-post-déploiement)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Créer une image master Windows personnalisée
- ✅ Déployer des postes via MDT et WDS
- ✅ Configurer Sysprep correctement
- ✅ Migrer les données utilisateurs
- ✅ Automatiser les déploiements avec des scripts
- ✅ Intégrer un poste au domaine Active Directory

---

## 📋 Prérequis

- [ ] Connaître Windows Server (AD, DNS, DHCP)
- [ ] Maîtriser Windows 10/11
- [ ] Comprendre le partitionnement disque
- [ ] Avoir des bases en scripting (PowerShell)

**Environnement nécessaire :**
- 💻 Windows Server avec AD/DNS/DHCP
- 🖥️ MDT installé
- 🌐 WDS configuré

---

## 📚 Introduction au déploiement

### Méthodes de déploiement

```
┌─────────────────────────────────────────────────────────────┐
│                 MÉTHODES DE DÉPLOIEMENT                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  MANUELLE (High Touch)                                       │
│  • Installation depuis USB/DVD                              │
│  • Configuration manuelle                                   │
│  • Long et non reproductible                                │
│  → Pour 1-10 postes                                         │
│                                                              │
│  SEMI-AUTOMATIQUE (Lite Touch - LTI)                        │
│  • Image master + MDT                                        │
│  • Intervention minimale du technicien                      │
│  • Séquence de tâches personnalisée                         │
│  → Pour 10-500 postes                                       │
│                                                              │
│  AUTOMATIQUE (Zero Touch - ZTI)                             │
│  • SCCM/MECM + MDT                                          │
│  • Déploiement sans intervention                            │
│  • Gestion centralisée complète                             │
│  → Pour 500+ postes                                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Workflow de déploiement

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   CRÉATION   │────▶│  DÉPLOIEMENT │────▶│   CONFIG     │
│   MASTER     │     │   IMAGE      │     │   POST-INST  │
└──────────────┘     └──────────────┘     └──────────────┘
       │                    │                    │
       ▼                    ▼                    ▼
 • Install Windows    • Boot PXE/USB       • Jonction domaine
 • Install Apps       • Chargement image   • Install apps
 • Config système     • Partitionnement    • Config user
 • Sysprep            • Application image  • Activation
```

---

## 🖼️ Imaging et Masterisation

### Création d'un Master Windows

#### Étape 1 : Installation de référence

```
┌─────────────────────────────────────────────────────────────┐
│  INSTALLATION DU POSTE DE RÉFÉRENCE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Installer Windows proprement                            │
│     • Dernière version Windows 10/11                        │
│     • Compte admin local temporaire                         │
│                                                              │
│  2. Configurer le système                                    │
│     • Désactiver les apps inutiles                          │
│     • Configurer les paramètres par défaut                  │
│     • Appliquer les optimisations                           │
│                                                              │
│  3. Installer les applications                               │
│     • Microsoft Office                                       │
│     • Navigateurs (Chrome, Edge)                            │
│     • Applications métier                                    │
│     • Outils IT (TeamViewer, etc.)                          │
│                                                              │
│  4. Mettre à jour                                            │
│     • Windows Update complet                                │
│     • Mises à jour applications                             │
│     • Drivers à jour                                        │
│                                                              │
│  5. Nettoyer                                                 │
│     • cleanmgr /d C:                                        │
│     • Supprimer fichiers temporaires                        │
│     • Vider corbeille                                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### Étape 2 : Sysprep

Sysprep prépare Windows pour le clonage en supprimant les identifiants uniques.

```powershell
# Emplacement de Sysprep
C:\Windows\System32\Sysprep\sysprep.exe

# Commande Sysprep pour généralisation
sysprep /generalize /oobe /shutdown

# Options importantes :
# /generalize : Supprime les infos spécifiques (SID, nom PC)
# /oobe : Démarre l'OOBE au prochain boot
# /shutdown : Éteint le PC après Sysprep
# /quit : Ne redémarre pas (pour capture)
# /unattend:fichier.xml : Fichier de réponses
```

**Fichier de réponses unattend.xml (exemple) :**

```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="specialize">
        <component name="Microsoft-Windows-Shell-Setup">
            <ComputerName>*</ComputerName>
            <TimeZone>Romance Standard Time</TimeZone>
        </component>
    </settings>
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Shell-Setup">
            <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <HideLocalAccountScreen>true</HideLocalAccountScreen>
                <HideOnlineAccountScreens>true</HideOnlineAccountScreens>
                <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
                <ProtectYourPC>3</ProtectYourPC>
            </OOBE>
            <UserAccounts>
                <LocalAccounts>
                    <LocalAccount wcm:action="add">
                        <Name>AdminLocal</Name>
                        <Group>Administrators</Group>
                        <Password>
                            <Value>MotDePasse123!</Value>
                            <PlainText>true</PlainText>
                        </Password>
                    </LocalAccount>
                </LocalAccounts>
            </UserAccounts>
        </component>
    </settings>
</unattend>
```

#### Étape 3 : Capture de l'image

```powershell
# Avec DISM (depuis WinPE)
# Capturer l'image
dism /Capture-Image /ImageFile:D:\Images\Windows10-Master.wim /CaptureDir:C:\ /Name:"Windows 10 Master v1.0"

# Vérifier l'image
dism /Get-ImageInfo /ImageFile:D:\Images\Windows10-Master.wim

# Avec MDT
# La capture se fait via une séquence de tâches "Capture"
```

---

## 🛠️ MDT - Microsoft Deployment Toolkit

### Installation de MDT

```
┌─────────────────────────────────────────────────────────────┐
│  INSTALLATION MDT                                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  PRÉREQUIS :                                                 │
│  • Windows Server 2019/2022                                 │
│  • Windows ADK (Assessment and Deployment Kit)              │
│  • Windows PE add-on pour ADK                               │
│                                                              │
│  INSTALLATION :                                              │
│  1. Télécharger Windows ADK                                 │
│  2. Installer ADK (Deployment Tools + USMT)                 │
│  3. Installer Windows PE add-on                             │
│  4. Télécharger et installer MDT                            │
│                                                              │
│  CONFIGURATION :                                             │
│  1. Ouvrir Deployment Workbench                             │
│  2. Créer un nouveau Deployment Share                       │
│  3. Importer le système d'exploitation                      │
│  4. Ajouter les applications                                │
│  5. Créer les séquences de tâches                           │
│  6. Générer le boot media                                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Structure du Deployment Share

```
DeploymentShare$
├── Applications/          # Applications à déployer
├── Boot/                  # Images de boot WinPE
├── Captures/              # Images capturées
├── Control/               # Fichiers de configuration
│   ├── Bootstrap.ini      # Config boot
│   ├── CustomSettings.ini # Config déploiement
│   └── TaskSequences/     # Définition des séquences
├── Operating Systems/     # Images Windows
├── Out-of-Box Drivers/    # Drivers
├── Packages/              # Mises à jour, packs
├── Scripts/               # Scripts MDT
└── Tools/                 # Outils supplémentaires
```

### Fichier CustomSettings.ini

```ini
[Settings]
Priority=Default
Properties=MyCustomProperty

[Default]
OSInstall=Y
SkipCapture=YES
SkipAdminPassword=YES
SkipProductKey=YES
SkipComputerBackup=YES
SkipBitLocker=YES
SkipLocaleSelection=YES
SkipTimeZone=YES

; Configuration régionale
KeyboardLocale=fr-FR
UserLocale=fr-FR
UILanguage=fr-FR
TimeZoneName=Romance Standard Time

; Configuration domaine
JoinDomain=ENTREPRISE.LOCAL
DomainAdmin=AdminDomaine
DomainAdminDomain=ENTREPRISE
DomainAdminPassword=MotDePasse123!
MachineObjectOU=OU=Postes,DC=ENTREPRISE,DC=LOCAL

; Applications à installer
Applications001={GUID-App1}
Applications002={GUID-App2}

; Compte admin local
AdminPassword=AdminLocal123!

; Nom du PC (formulaire)
SkipComputerName=NO
```

### Création d'une séquence de tâches

```
┌─────────────────────────────────────────────────────────────┐
│  SÉQUENCE DE TÂCHES - DÉPLOIEMENT STANDARD                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  PHASE 1 : Initialisation                                    │
│  ├── Gather (collecte infos)                                │
│  ├── Format and Partition Disk                              │
│  └── Inject Drivers                                         │
│                                                              │
│  PHASE 2 : Installation                                      │
│  ├── Install Operating System                               │
│  ├── Apply Windows Settings                                 │
│  └── Apply Network Settings                                 │
│                                                              │
│  PHASE 3 : Post-installation                                 │
│  ├── Install Applications                                   │
│  ├── Install Windows Updates                                │
│  └── Run Custom Scripts                                     │
│                                                              │
│  PHASE 4 : Finalisation                                      │
│  ├── Configure Windows                                      │
│  ├── Enable BitLocker (si requis)                          │
│  └── Cleanup and Restart                                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🌐 WDS - Windows Deployment Services

### Configuration de WDS

```powershell
# Installer le rôle WDS
Install-WindowsFeature -Name WDS -IncludeManagementTools

# Initialiser WDS
WDSUtil /Initialize-Server /RemInstall:"D:\RemoteInstall"

# Configurer le serveur WDS
WDSUtil /Set-Server /AnswerClients:All

# Ajouter une image de boot
WDSUtil /Add-Image /ImageFile:"D:\Images\boot.wim" /ImageType:Boot

# Ajouter une image d'installation
WDSUtil /Add-Image /ImageFile:"D:\Images\install.wim" /ImageType:Install

# Démarrer le service WDS
Start-Service WDSServer
```

### Boot PXE

```
┌─────────────────────────────────────────────────────────────┐
│  PROCESSUS BOOT PXE                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. PC démarre et demande une IP via DHCP                   │
│  2. Le serveur DHCP fournit l'IP + options PXE              │
│     Option 66 : Adresse du serveur WDS                      │
│     Option 67 : Fichier de boot (boot\x64\wdsnbp.com)       │
│  3. Le PC télécharge le boot loader via TFTP                │
│  4. Le boot loader charge WinPE                             │
│  5. WinPE démarre et lance le wizard MDT                    │
│                                                              │
│  CONFIGURATION DHCP :                                        │
│  Option 66 (Boot Server) : 192.168.1.10                     │
│  Option 67 (Boot File) : boot\x64\wdsnbp.com               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 📦 Migration de données

### USMT - User State Migration Tool

```powershell
# Emplacement USMT (après installation ADK)
# C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\User State Migration Tool

# Scanner le profil utilisateur (estimation)
scanstate.exe /i:migdocs.xml /i:migapp.xml /o /c /v:13 /l:scan.log D:\Migration\User1

# Capturer les données utilisateur
scanstate.exe D:\Migration\User1 /i:migdocs.xml /i:migapp.xml /o /c /v:13 /l:scan.log

# Restaurer les données utilisateur
loadstate.exe D:\Migration\User1 /i:migdocs.xml /i:migapp.xml /c /v:13 /l:load.log
```

### Migration manuelle

```powershell
# Script de migration simple
$source = "C:\Users\OldUser"
$destination = "\\SERVEUR\Migration$\OldUser"

# Dossiers à migrer
$folders = @(
    "Desktop",
    "Documents",
    "Downloads",
    "Pictures",
    "Favorites",
    "AppData\Local\Google\Chrome\User Data\Default",
    "AppData\Local\Microsoft\Outlook"
)

foreach ($folder in $folders) {
    $src = Join-Path $source $folder
    $dst = Join-Path $destination $folder

    if (Test-Path $src) {
        Copy-Item -Path $src -Destination $dst -Recurse -Force
        Write-Host "Copié : $folder"
    }
}

Write-Host "Migration terminée !"
```

---

## ⚙️ Configuration post-déploiement

### Jonction au domaine

```powershell
# Joindre le domaine via PowerShell
Add-Computer -DomainName "ENTREPRISE.LOCAL" -OUPath "OU=Postes,DC=ENTREPRISE,DC=LOCAL" -Credential (Get-Credential) -Restart

# Ou via netdom
netdom join %COMPUTERNAME% /domain:ENTREPRISE.LOCAL /UserD:AdminDomaine /PasswordD:* /OU:"OU=Postes,DC=ENTREPRISE,DC=LOCAL"
```

### Script de configuration post-install

```powershell
# PostInstall.ps1 - Configuration post-déploiement

# 1. Renommer le PC selon convention
$newName = "PC-" + (Get-WmiObject Win32_BIOS).SerialNumber.Substring(0,8)
Rename-Computer -NewName $newName

# 2. Configurer les paramètres d'alimentation
powercfg /setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c  # High Performance

# 3. Désactiver l'hibernation
powercfg /hibernate off

# 4. Configurer le pare-feu
# Autoriser le ping
netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=icmpv4:8,any dir=in action=allow

# 5. Activer Bureau à distance
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

# 6. Installer les outils IT
# Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

choco install googlechrome -y
choco install 7zip -y
choco install notepadplusplus -y

# 7. Mettre à jour Windows
Install-Module PSWindowsUpdate -Force
Get-WindowsUpdate -AcceptAll -Install -AutoReboot

Write-Host "Configuration post-installation terminée !"
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Créer un fichier unattend.xml

Créez un fichier de réponses qui :
- Configure le fuseau horaire sur Paris
- Crée un compte admin local "Support"
- Accepte automatiquement l'EULA
- Désactive les écrans OOBE inutiles

<details>
<summary>Solution</summary>

```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="specialize">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" language="neutral">
            <TimeZone>Romance Standard Time</TimeZone>
        </component>
    </settings>
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" language="neutral">
            <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <HideLocalAccountScreen>true</HideLocalAccountScreen>
                <HideOnlineAccountScreens>true</HideOnlineAccountScreens>
                <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
                <ProtectYourPC>3</ProtectYourPC>
            </OOBE>
            <UserAccounts>
                <LocalAccounts>
                    <LocalAccount wcm:action="add">
                        <Name>Support</Name>
                        <Group>Administrators</Group>
                        <Password>
                            <Value>Support2026!</Value>
                            <PlainText>true</PlainText>
                        </Password>
                    </LocalAccount>
                </LocalAccounts>
            </UserAccounts>
        </component>
    </settings>
</unattend>
```
</details>

### Exercice 2 : Script de migration utilisateur

Créez un script PowerShell qui migre les données d'un utilisateur.

<details>
<summary>Solution</summary>

```powershell
param(
    [string]$UserName,
    [string]$DestinationServer = "\\SRV-FILES\Migration$"
)

$source = "C:\Users\$UserName"
$destination = "$DestinationServer\$UserName"

if (-not (Test-Path $source)) {
    Write-Error "Profil utilisateur non trouvé : $source"
    exit 1
}

# Créer le dossier destination
New-Item -ItemType Directory -Path $destination -Force | Out-Null

# Dossiers à migrer
$folders = @("Desktop", "Documents", "Downloads", "Pictures", "Favorites")

foreach ($folder in $folders) {
    $src = Join-Path $source $folder
    $dst = Join-Path $destination $folder

    if (Test-Path $src) {
        Write-Host "Migration de $folder..." -ForegroundColor Yellow
        Copy-Item -Path $src -Destination $dst -Recurse -Force
        Write-Host "OK" -ForegroundColor Green
    }
}

Write-Host "`nMigration terminée ! Données dans : $destination" -ForegroundColor Cyan
```
</details>

---

## 📚 Ressources

- [Microsoft Deployment Toolkit](https://docs.microsoft.com/mem/configmgr/mdt/)
- [Windows ADK](https://docs.microsoft.com/windows-hardware/get-started/adk-install)
- [Sysprep Documentation](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-process-overview)

---

## ✅ Checklist de révision

- [ ] Comprendre les différentes méthodes de déploiement (LTI, ZTI)
- [ ] Savoir utiliser Sysprep pour généraliser une image
- [ ] Configurer MDT (Deployment Share, séquences de tâches)
- [ ] Configurer WDS pour le boot PXE
- [ ] Créer des fichiers unattend.xml
- [ ] Utiliser USMT pour la migration de données
- [ ] Joindre un poste au domaine via script

---

<div align="center">

**Cours suivant :** [Gestion des droits et accès (AD/GPO)](./06-gestion-droits-ad.md)

[⬅️ Retour au sommaire](./README.md)

</div>
