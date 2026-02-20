# Installation, Mise à Jour et Support des Applications Métiers

> 📚 **Module :** Maître Emplois - Mission 07
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 6-8 heures
> 🎯 **Niveau :** N2 (Intermédiaire)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Types d'applications métiers](#-types-dapplications-métiers)
- [Installation des applications](#-installation-des-applications)
- [Déploiement centralisé](#-déploiement-centralisé)
- [Mise à jour et maintenance](#-mise-à-jour-et-maintenance)
- [Support applicatif](#-support-applicatif)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Installer des applications métiers (MSI, EXE, scripts)
- ✅ Déployer des applications via GPO et SCCM
- ✅ Gérer les mises à jour applicatives
- ✅ Diagnostiquer les problèmes applicatifs courants
- ✅ Utiliser les gestionnaires de paquets (Chocolatey, Winget)

---

## 📦 Types d'applications métiers

### Classification des applications

```
┌─────────────────────────────────────────────────────────────┐
│              TYPES D'APPLICATIONS ENTREPRISE                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  BUREAUTIQUE                                                 │
│  • Microsoft Office 365 / 2021                              │
│  • Adobe Acrobat Reader/Pro                                 │
│  • Navigateurs (Chrome, Edge, Firefox)                      │
│                                                              │
│  MÉTIER SPÉCIFIQUE                                           │
│  • ERP (SAP, Sage, Cegid)                                   │
│  • CRM (Salesforce, Dynamics)                               │
│  • Comptabilité (Sage, EBP, Ciel)                          │
│  • CAO/DAO (AutoCAD, SolidWorks)                           │
│                                                              │
│  COMMUNICATION                                               │
│  • Microsoft Teams                                           │
│  • Zoom, Webex                                              │
│  • Clients messagerie (Outlook)                             │
│                                                              │
│  SÉCURITÉ / IT                                               │
│  • Antivirus (Windows Defender, Kaspersky)                  │
│  • VPN (Cisco AnyConnect, FortiClient)                      │
│  • Outils support (TeamViewer, AnyDesk)                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 💿 Installation des applications

### Installation silencieuse (MSI)

```powershell
# Installation MSI standard
msiexec /i "application.msi" /qn /norestart

# Avec fichier log
msiexec /i "application.msi" /qn /norestart /l*v "C:\Logs\install.log"

# Avec propriétés personnalisées
msiexec /i "application.msi" /qn INSTALLDIR="C:\Program Files\App" ALLUSERS=1

# Désinstallation
msiexec /x "application.msi" /qn

# Désinstallation par GUID
msiexec /x {GUID-PRODUIT} /qn
```

### Installation silencieuse (EXE)

```powershell
# Paramètres courants (varient selon l'application)
# /S, /s, /silent, /quiet, -silent, --silent

# Exemples courants :
# 7-Zip
7z2301-x64.exe /S

# Google Chrome
ChromeSetup.exe /silent /install

# Firefox
Firefox_Setup.exe /S

# Adobe Reader
AdobeReader.exe /sAll /rs /msi EULA_ACCEPT=YES

# VLC
vlc-3.0.exe /S

# Notepad++
npp.Installer.exe /S
```

### Microsoft Office

```powershell
# Office Click-to-Run (Office 365)
# Utiliser l'outil de déploiement Office (ODT)

# 1. Télécharger ODT
# https://www.microsoft.com/download/details.aspx?id=49117

# 2. Créer configuration.xml
@"
<Configuration>
  <Add OfficeClientEdition="64" Channel="Current">
    <Product ID="O365ProPlusRetail">
      <Language ID="fr-fr" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="OneDrive" />
    </Product>
  </Add>
  <Display Level="None" AcceptEULA="TRUE" />
  <Property Name="AUTOACTIVATE" Value="1" />
</Configuration>
"@ | Out-File "configuration.xml"

# 3. Télécharger les fichiers
setup.exe /download configuration.xml

# 4. Installer
setup.exe /configure configuration.xml
```

---

## 🚀 Déploiement centralisé

### Déploiement via GPO

```
Configuration ordinateur > Stratégies > Paramètres du logiciel >
Installation de logiciel

1. Clic droit > Nouveau > Package
2. Sélectionner le .msi sur un partage réseau (\\SRV\Deploy$\)
3. Choisir "Attribué" (installation automatique)
4. Lier la GPO à l'OU cible
```

### Chocolatey (Gestionnaire de paquets)

```powershell
# Installer Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Installer des applications
choco install googlechrome -y
choco install firefox -y
choco install 7zip -y
choco install adobereader -y
choco install notepadplusplus -y
choco install vlc -y
choco install git -y
choco install vscode -y

# Mettre à jour toutes les applications
choco upgrade all -y

# Lister les applications installées
choco list --local-only

# Script de déploiement poste type
$apps = @(
    "googlechrome",
    "firefox",
    "7zip",
    "adobereader",
    "notepadplusplus",
    "vlc"
)

foreach ($app in $apps) {
    choco install $app -y
}
```

### Winget (Windows Package Manager)

```powershell
# Rechercher une application
winget search chrome

# Installer
winget install Google.Chrome --silent
winget install Mozilla.Firefox --silent
winget install 7zip.7zip --silent

# Mettre à jour
winget upgrade --all

# Exporter la liste des applications installées
winget export -o applications.json

# Importer et installer depuis une liste
winget import -i applications.json
```

---

## 🔄 Mise à jour et maintenance

### Gestion des mises à jour

```
┌─────────────────────────────────────────────────────────────┐
│           STRATÉGIE DE MISE À JOUR                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  CRITIQUE (Sécurité) → Déploiement immédiat                │
│  • Failles critiques CVE                                    │
│  • Patches de sécurité urgents                             │
│                                                              │
│  IMPORTANTE → Déploiement sous 7 jours                      │
│  • Corrections de bugs majeurs                              │
│  • Améliorations de stabilité                              │
│                                                              │
│  STANDARD → Déploiement planifié (mensuel)                  │
│  • Nouvelles fonctionnalités                               │
│  • Mises à jour mineures                                   │
│                                                              │
│  PROCESSUS :                                                 │
│  1. Test sur environnement pilote                          │
│  2. Validation par les key users                           │
│  3. Déploiement progressif (par vagues)                    │
│  4. Monitoring et rollback si problème                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Script de mise à jour centralisée

```powershell
# Script de mise à jour quotidienne (via tâche planifiée)
$logFile = "C:\Logs\Updates_$(Get-Date -Format 'yyyyMMdd').log"

# Mise à jour via Chocolatey
"[$(Get-Date)] Début des mises à jour Chocolatey" | Out-File $logFile -Append
choco upgrade all -y | Out-File $logFile -Append

# Mise à jour Windows (optionnel)
"[$(Get-Date)] Vérification Windows Update" | Out-File $logFile -Append
Get-WindowsUpdate | Out-File $logFile -Append

"[$(Get-Date)] Mises à jour terminées" | Out-File $logFile -Append
```

---

## 🔧 Support applicatif

### Diagnostic des problèmes applicatifs

```
┌─────────────────────────────────────────────────────────────┐
│        DIAGNOSTIC APPLICATION NE DÉMARRE PAS                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. MESSAGE D'ERREUR                                         │
│     • Noter le message exact                                │
│     • Capture d'écran                                       │
│                                                              │
│  2. VÉRIFIER LES LOGS                                        │
│     • Event Viewer > Applications                           │
│     • Logs applicatifs (dossier AppData)                   │
│                                                              │
│  3. EXÉCUTION ADMIN                                          │
│     • Clic droit > Exécuter en tant qu'administrateur      │
│                                                              │
│  4. MODE COMPATIBILITÉ                                       │
│     • Propriétés > Compatibilité                           │
│     • Tester Windows 8/7                                    │
│                                                              │
│  5. DÉPENDANCES                                              │
│     • .NET Framework                                        │
│     • Visual C++ Redistributable                           │
│     • Java Runtime                                          │
│                                                              │
│  6. RÉPARATION / RÉINSTALLATION                             │
│     • Programmes > Modifier > Réparer                      │
│     • Désinstaller proprement puis réinstaller             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Outils de diagnostic

```powershell
# Vérifier les applications installées
Get-WmiObject -Class Win32_Product | Select-Object Name, Version

# Plus rapide avec le registre
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher

# Vérifier les versions .NET installées
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse |
    Get-ItemProperty -Name Version -EA 0 |
    Select-Object PSChildName, Version

# Vérifier Visual C++ Redistributable
Get-WmiObject -Query "SELECT * FROM Win32_Product WHERE Name LIKE '%Visual C++%'" |
    Select-Object Name, Version

# Process Monitor pour diagnostic avancé
# Télécharger depuis Sysinternals
# Filtrer sur le processus de l'application
```

---

## 🎯 Exercices pratiques

### Exercice : Script d'installation automatisée

Créez un script qui installe les applications standard d'un poste bureautique.

<details>
<summary>Solution</summary>

```powershell
# install-standard-apps.ps1

$logPath = "C:\Logs"
if (-not (Test-Path $logPath)) { New-Item -ItemType Directory -Path $logPath }
$log = "$logPath\install_$(Get-Date -Format 'yyyyMMdd_HHmmss').log"

function Write-Log {
    param([string]$Message)
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    "$timestamp - $Message" | Tee-Object -FilePath $log -Append
}

Write-Log "=== Début installation applications standard ==="

# Vérifier/Installer Chocolatey
if (-not (Get-Command choco -ErrorAction SilentlyContinue)) {
    Write-Log "Installation de Chocolatey..."
    Set-ExecutionPolicy Bypass -Scope Process -Force
    iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
}

# Liste des applications
$applications = @(
    @{Name="googlechrome"; DisplayName="Google Chrome"},
    @{Name="firefox"; DisplayName="Mozilla Firefox"},
    @{Name="7zip"; DisplayName="7-Zip"},
    @{Name="adobereader"; DisplayName="Adobe Reader"},
    @{Name="notepadplusplus"; DisplayName="Notepad++"},
    @{Name="vlc"; DisplayName="VLC Media Player"}
)

foreach ($app in $applications) {
    Write-Log "Installation de $($app.DisplayName)..."
    $result = choco install $app.Name -y 2>&1
    if ($LASTEXITCODE -eq 0) {
        Write-Log "✓ $($app.DisplayName) installé avec succès"
    } else {
        Write-Log "✗ Erreur installation $($app.DisplayName)"
    }
}

Write-Log "=== Installation terminée ==="
Write-Host "`nRapport disponible : $log" -ForegroundColor Green
```
</details>

---

## 📚 Ressources

- [Chocolatey Packages](https://community.chocolatey.org/packages)
- [Winget Documentation](https://docs.microsoft.com/windows/package-manager/)
- [Office Deployment Tool](https://docs.microsoft.com/deployoffice/overview-office-deployment-tool)

---

## ✅ Checklist de révision

- [ ] Installer des applications en mode silencieux (MSI, EXE)
- [ ] Utiliser Chocolatey et Winget
- [ ] Déployer via GPO
- [ ] Gérer les mises à jour applicatives
- [ ] Diagnostiquer les problèmes d'applications

---

<div align="center">

**Cours suivant :** [Rédaction de procédures et documentation technique](./08-documentation-technique.md)

[⬅️ Retour au sommaire](./README.md)

</div>
