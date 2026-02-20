# 🖥️ Guide VMware - Lab Windows Server pour TSSR

**Objectif :** Configurer un environnement VMware optimal pour pratiquer Active Directory, DNS, DHCP et GPO

---

## 📑 Table des matières

1. [Architecture du Lab recommandée](#1-architecture-du-lab-recommandée)
2. [Configuration VMware Workstation/Player](#2-configuration-vmware-workstationplayer)
3. [Création de la VM Windows Server](#3-création-de-la-vm-windows-server)
4. [Configuration réseau VMware](#4-configuration-réseau-vmware)
5. [Installation Windows Server](#5-installation-windows-server)
6. [Configuration IP statique dans VMware](#6-configuration-ip-statique-dans-vmware)
7. [Snapshots - Points de restauration](#7-snapshots---points-de-restauration)
8. [Clonage de VMs (postes clients)](#8-clonage-de-vms-postes-clients)
9. [Optimisation des performances](#9-optimisation-des-performances)
10. [Troubleshooting VMware](#10-troubleshooting-vmware)

---

## 1️⃣ Architecture du Lab recommandée

### 📌 Configuration minimale pour un lab fonctionnel

```
┌─────────────────────────────────────────────────────────────────┐
│ MACHINE HÔTE (Votre PC physique)                                │
│ RAM : 16 Go minimum (32 Go recommandé)                          │
│ CPU : 4 cœurs minimum (8 cœurs recommandé)                      │
│ Disque : 200 Go libres (SSD recommandé)                         │
│                                                                 │
│  ┌────────────────────────────────────────────────────────┐    │
│  │ VMware Workstation Pro / Player                        │    │
│  │                                                        │    │
│  │  ┌──────────────────┐  ┌──────────────────┐          │    │
│  │  │ VM1: Server2022  │  │ VM2: Win10/11    │          │    │
│  │  │ Rôle: DC + DNS   │  │ Rôle: Client     │          │    │
│  │  │      + DHCP      │  │ Membre du domaine│          │    │
│  │  │                  │  │                  │          │    │
│  │  │ RAM: 4 Go        │  │ RAM: 4 Go        │          │    │
│  │  │ CPU: 2 cœurs     │  │ CPU: 2 cœurs     │          │    │
│  │  │ Disque: 60 Go    │  │ Disque: 60 Go    │          │    │
│  │  │ IP: 192.168.10.10│  │ IP: DHCP         │          │    │
│  │  └──────────────────┘  └──────────────────┘          │    │
│  │           │                      │                    │    │
│  │           └──────────┬───────────┘                    │    │
│  │                      │                                │    │
│  │            ┌─────────▼─────────┐                      │    │
│  │            │ VMnet8 (NAT)      │                      │    │
│  │            │ 192.168.10.0/24   │                      │    │
│  │            │ Passerelle: .2    │                      │    │
│  │            └───────────────────┘                      │    │
│  └────────────────────────────────────────────────────────┘    │
│                         │                                      │
│                         ▼ (Internet via NAT)                   │
│                    [Internet]                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 📌 Ressources recommandées par VM

```
┌──────────────────────────────────────────────────────────────┐
│ TYPE VM          │ RAM    │ CPU    │ DISQUE │ RÉSEAU         │
├──────────────────┼────────┼────────┼────────┼────────────────┤
│ Server 2022 DC   │ 4 Go   │ 2 cœurs│ 60 Go  │ NAT (VMnet8)   │
│ Server 2022 Core │ 2 Go   │ 2 cœurs│ 40 Go  │ NAT (VMnet8)   │
│ Windows 10/11    │ 4 Go   │ 2 cœurs│ 60 Go  │ NAT (VMnet8)   │
│ (Clients)        │        │        │        │                │
└──────────────────────────────────────────────────────────────┘

⚠️ IMPORTANT : Ne pas allouer TOUTE la RAM de l'hôte !
   Laisser au moins 8 Go pour le système hôte
```

### 📌 Architecture réseau recommandée

```
Option 1 : VMnet8 (NAT) - RECOMMANDÉ pour débuter
┌──────────────────────────────────────────────┐
│ ✅ Avantages :                               │
│   - Accès Internet automatique               │
│   - Isolation du réseau physique             │
│   - Facile à configurer                      │
│                                              │
│ ❌ Inconvénients :                           │
│   - Non accessible depuis l'hôte par défaut  │
│   - Nécessite configuration pour accès RDP   │
└──────────────────────────────────────────────┘

Option 2 : VMnet1 (Host-Only)
┌──────────────────────────────────────────────┐
│ ✅ Avantages :                               │
│   - Accessible depuis l'hôte (RDP facile)    │
│   - Isolation totale                         │
│                                              │
│ ❌ Inconvénients :                           │
│   - Pas d'accès Internet direct              │
│   - Nécessite 2ème carte réseau pour Internet│
└──────────────────────────────────────────────┘

Option 3 : Bridge (Pont)
┌──────────────────────────────────────────────┐
│ ✅ Avantages :                               │
│   - VMs sur le même réseau que l'hôte        │
│   - Accessible depuis tout le réseau local   │
│                                              │
│ ❌ Inconvénients :                           │
│   - Risque de conflit IP                     │
│   - Exposition au réseau physique            │
│   - Problèmes si DHCP serveur existe déjà    │
└──────────────────────────────────────────────┘

🎯 RECOMMANDATION POUR LE LAB :
   VMnet8 (NAT) pour Internet + VMnet1 (Host-Only) pour management
```

---

## 2️⃣ Configuration VMware Workstation/Player

### 📌 Configuration de l'éditeur de réseau virtuel

**Étape 1 : Ouvrir l'Éditeur de réseau virtuel**
```
1. VMware Workstation → Édition → Éditeur de réseau virtuel
2. Si demandé, cliquer "Modifier les paramètres" (droits admin)
```

**Étape 2 : Configurer VMnet8 (NAT)**
```
┌────────────────────────────────────────┐
│ Sélectionner : VMnet8                  │
│                                        │
│ Type : NAT                             │
│ ☑ Connecter une carte réseau hôte     │
│   à ce réseau                          │
│ ☑ Utiliser l'adressage local DHCP     │
│                                        │
│ Paramètres de sous-réseau :           │
│   IP du sous-réseau : 192.168.10.0    │
│   Masque : 255.255.255.0               │
│                                        │
│ Passerelle : 192.168.10.2              │
│ (Définie automatiquement par VMware)   │
│                                        │
│ → Appliquer                            │
└────────────────────────────────────────┘
```

**Étape 3 : Configurer le DHCP VMware (temporaire)**
```
1. Cliquer "Paramètres DHCP" sur VMnet8

┌────────────────────────────────────────┐
│ Plage IP de début : 192.168.10.100    │
│ Plage IP de fin   : 192.168.10.200    │
│                                        │
│ Durée de bail : 1800 secondes          │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘

⚠️ NOTE : Ce DHCP VMware sera utilisé TEMPORAIREMENT
   jusqu'à ce que votre serveur DHCP Windows soit actif
```

**Étape 4 : Configurer VMnet1 (Host-Only) - OPTIONNEL**
```
┌────────────────────────────────────────┐
│ Sélectionner : VMnet1                  │
│                                        │
│ Type : Host-only                       │
│ ☑ Connecter une carte réseau hôte     │
│                                        │
│ Paramètres de sous-réseau :           │
│   IP du sous-réseau : 192.168.20.0    │
│   Masque : 255.255.255.0               │
│                                        │
│ → Appliquer → OK                       │
└────────────────────────────────────────┘
```

### 💡 PowerShell pour vérifier VMware (sur l'hôte Windows)

```powershell
# ✅ Lister les cartes réseau VMware sur l'hôte
Get-NetAdapter | Where-Object {$_.Name -like "VMware*"} | Select-Object Name, Status, LinkSpeed

# ✅ Voir la configuration IP de VMnet8 sur l'hôte
Get-NetIPAddress -InterfaceAlias "VMware Network Adapter VMnet8"

# ✅ Tester la connectivité vers une VM
Test-Connection -ComputerName 192.168.10.10 -Count 2

# ✅ Voir les services VMware en cours
Get-Service | Where-Object {$_.Name -like "VMware*"} | Select-Object Name, Status, StartType
```

---

## 3️⃣ Création de la VM Windows Server

### 📌 Création de la VM serveur

**Étape 1 : Créer une nouvelle VM**
```
1. VMware → Fichier → Nouvelle machine virtuelle
2. Configuration : Personnalisée (recommandé)
3. → Suivant
```

**Étape 2 : Compatibilité matérielle**
```
┌────────────────────────────────────────┐
│ Version de compatibilité :             │
│ Workstation 17.x (ou la plus récente)  │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 3 : Installer le système d'exploitation**
```
┌────────────────────────────────────────┐
│ ● J'installerai le système            │
│   d'exploitation ultérieurement        │
│ (Recommandé pour contrôler l'install)  │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 4 : Système d'exploitation invité**
```
┌────────────────────────────────────────┐
│ Système : ● Microsoft Windows          │
│ Version : Windows Server 2022          │
│           (ou 2019/2016)               │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 5 : Nom et emplacement**
```
┌────────────────────────────────────────┐
│ Nom de la machine :                    │
│   SRV-DC01                             │
│                                        │
│ Emplacement :                          │
│   D:\VMware\LAB-TSSR\SRV-DC01          │
│ (Choisir un disque avec beaucoup      │
│  d'espace, de préférence SSD)          │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 6 : Configuration du processeur**
```
┌────────────────────────────────────────┐
│ Nombre de processeurs : 1              │
│ Nombre de cœurs par processeur : 2    │
│                                        │
│ Total : 2 cœurs                        │
│                                        │
│ ☑ Virtualiser Intel VT-x/EPT ou       │
│   AMD-V/RVI                            │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 7 : Mémoire**
```
┌────────────────────────────────────────┐
│ Mémoire pour cette machine virtuelle : │
│                                        │
│ 4096 Mo (4 Go)                         │
│                                        │
│ ⚠️ Minimum absolu : 2048 Mo            │
│    Recommandé : 4096 Mo                │
│    Optimal : 8192 Mo                   │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 8 : Type de réseau**
```
┌────────────────────────────────────────┐
│ Type de connexion réseau :             │
│                                        │
│ ● Utiliser la traduction d'adresses   │
│   réseau (NAT)                         │
│   → VMnet8                             │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 9 : Contrôleur SCSI**
```
┌────────────────────────────────────────┐
│ Type de contrôleur SCSI :              │
│ ● LSI Logic SAS (recommandé)           │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 10 : Type de disque**
```
┌────────────────────────────────────────┐
│ Type de disque :                       │
│ ● NVMe (recommandé, plus rapide)       │
│   OU                                   │
│ ● SCSI (compatible)                    │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 11 : Disque**
```
┌────────────────────────────────────────┐
│ ● Créer un nouveau disque virtuel     │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 12 : Taille du disque**
```
┌────────────────────────────────────────┐
│ Capacité maximale : 60 Go              │
│                                        │
│ ● Allouer tout l'espace maintenant    │
│   (Meilleure performance - RECOMMANDÉ) │
│                                        │
│ ● Stocker le disque virtuel en un     │
│   seul fichier                         │
│   (Meilleure performance)              │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 13 : Fichier de disque**
```
┌────────────────────────────────────────┐
│ Nom du fichier :                       │
│   SRV-DC01.vmdk                        │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘
```

**Étape 14 : Finalisation**
```
┌────────────────────────────────────────┐
│ ☑ Personnaliser le matériel            │
│   (IMPORTANT pour ajouter l'ISO)       │
│                                        │
│ → Terminer                             │
└────────────────────────────────────────┘
```

**Étape 15 : Personnalisation matérielle**
```
Dans la fenêtre "Matériel" :

1. Lecteur CD/DVD (SATA) :
   ☑ Connecter au démarrage
   ● Utiliser le fichier image ISO
   Parcourir → Sélectionner :
     Windows_Server_2022.iso

2. Périphérique USB :
   ● USB 3.1 (recommandé)

3. Affichage :
   ☑ Accélérer les graphiques 3D
   Mémoire graphique : 1 Go (si disponible)

4. → Fermer
```

### 💡 Fichiers VMware créés

```
D:\VMware\LAB-TSSR\SRV-DC01\
├── SRV-DC01.vmx           ← Fichier de configuration (XML)
├── SRV-DC01.vmdk          ← Descripteur du disque
├── SRV-DC01-flat.vmdk     ← Fichier disque réel (60 Go)
├── SRV-DC01.vmxf          ← Configuration supplémentaire
├── SRV-DC01.nvram         ← BIOS virtuel
└── vmware.log             ← Logs VMware

⚠️ NE JAMAIS SUPPRIMER les fichiers .vmdk manuellement !
```

---

## 4️⃣ Configuration réseau VMware

### 📌 Vérifier la configuration réseau de la VM

**Étape 1 : Paramètres de la VM**
```
1. Clic droit sur la VM → Paramètres
2. Onglet "Matériel" → Carte réseau

┌────────────────────────────────────────┐
│ Connexion réseau :                     │
│ ● NAT : Utilisé pour partager l'IP    │
│   de l'hôte                            │
│                                        │
│ Type de carte :                        │
│ ● VMXNET3 (recommandé, plus rapide)   │
│   OU                                   │
│ ● E1000E (compatible Windows Server)   │
│                                        │
│ ☑ Connecter au démarrage               │
│ ☑ Répliquer l'état de la connexion    │
│   physique du réseau                   │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘
```

### 📌 Ajouter une 2ème carte réseau (OPTIONNEL)

**Pour séparer Management et Production :**
```
1. Paramètres VM → Ajouter... → Carte réseau → Suivant

Carte 1 (Production) :
┌────────────────────────────────────────┐
│ Type : NAT (VMnet8)                    │
│ IP : 192.168.10.10 (statique)          │
│ Usage : AD, DNS, DHCP, Clients         │
└────────────────────────────────────────┘

Carte 2 (Management) - OPTIONNEL :
┌────────────────────────────────────────┐
│ Type : Host-Only (VMnet1)              │
│ IP : 192.168.20.10 (statique)          │
│ Usage : RDP, SSH, Administration       │
└────────────────────────────────────────┘

🎯 Avantage :
   - Séparer le trafic AD du trafic admin
   - RDP depuis l'hôte via 192.168.20.10
   - Internet/AD via 192.168.10.10
```

### 💡 Commandes réseau dans VMware

```powershell
# ✅ Lister les cartes réseau dans la VM
Get-NetAdapter | Select-Object Name, Status, LinkSpeed, MacAddress

# ✅ Renommer les cartes pour plus de clarté
Rename-NetAdapter -Name "Ethernet0" -NewName "LAN-Production"
Rename-NetAdapter -Name "Ethernet1" -NewName "LAN-Management"

# ✅ Voir les paramètres de la carte VMware
Get-NetAdapter | Get-NetAdapterAdvancedProperty

# ✅ Désactiver IPv6 (si non utilisé)
Disable-NetAdapterBinding -Name "LAN-Production" -ComponentID ms_tcpip6
```

---

## 5️⃣ Installation Windows Server

### 📌 Démarrer l'installation

**Étape 1 : Démarrer la VM**
```
1. Sélectionner SRV-DC01
2. Cliquer "Mettre sous tension cette machine virtuelle"
3. La VM boot sur l'ISO Windows Server 2022
```

**Étape 2 : Installation de Windows Server**
```
┌────────────────────────────────────────┐
│ Langue : Français                      │
│ Format heure : Français (France)       │
│ Clavier : Français                     │
│ → Suivant → Installer maintenant       │
└────────────────────────────────────────┘

Édition à installer :
┌────────────────────────────────────────┐
│ ● Windows Server 2022 Standard         │
│   (Expérience de bureau)               │
│                                        │
│ ⚠️ NE PAS choisir "Server Core"        │
│    (pas d'interface graphique)         │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘

Type d'installation :
┌────────────────────────────────────────┐
│ ● Personnalisé : Installer Windows     │
│   uniquement                           │
│                                        │
│ → Suivant                              │
└────────────────────────────────────────┘

Où installer Windows :
┌────────────────────────────────────────┐
│ Lecteur 0 : 60 Go                      │
│ (Tout l'espace disponible)             │
│                                        │
│ → Suivant                              │
│ (L'installation démarre)               │
└────────────────────────────────────────┘
```

**⏱️ Attendre l'installation (10-20 minutes)**

**Étape 3 : Configuration initiale**
```
Mot de passe Administrateur :
┌────────────────────────────────────────┐
│ Mot de passe : P@ssw0rd2024!           │
│ Confirmer : P@ssw0rd2024!              │
│                                        │
│ ⚠️ CONSERVER ce mot de passe           │
│    dans un endroit sûr !               │
│                                        │
│ → Terminer                             │
└────────────────────────────────────────┘
```

**Étape 4 : Première connexion**
```
1. Appuyer sur Ctrl+Alt+Insert (dans VMware)
   (Équivalent de Ctrl+Alt+Suppr)

2. Entrer le mot de passe administrateur

3. Le Gestionnaire de serveur s'ouvre automatiquement
```

### 📌 Installation VMware Tools (IMPORTANT !)

**Pourquoi installer VMware Tools ?**
```
✅ Améliore les performances graphiques
✅ Partage du presse-papier hôte ↔ VM
✅ Glisser-déposer de fichiers
✅ Synchronisation de l'heure
✅ Meilleure gestion de la souris
```

**Installation :**
```
1. Dans VMware → VM → Installer VMware Tools
2. Dans la VM, ouvrir l'Explorateur de fichiers
3. Double-clic sur le lecteur CD (VMware Tools)
4. Exécuter : setup64.exe

┌────────────────────────────────────────┐
│ Installation VMware Tools              │
│ → Suivant → Suivant                    │
│ Type : ● Complet                       │
│ → Suivant → Installer                  │
│                                        │
│ Redémarrer la VM après installation    │
└────────────────────────────────────────┘
```

### 💡 Vérifier VMware Tools

```powershell
# ✅ Vérifier que VMware Tools est installé
Get-Service -Name "VMTools" | Select-Object Name, Status, StartType

# ✅ Voir la version de VMware Tools
C:\Program Files\VMware\VMware Tools\vmtoolsd.exe -v

# ✅ Synchroniser l'heure avec l'hôte
w32tm /resync /force

# ✅ Activer le partage de fichiers (Drag & Drop)
# (Déjà activé par VMware Tools)
```

---

## 6️⃣ Configuration IP statique dans VMware

### 📌 Définir l'IP statique dans Windows Server

**Étape 1 : Ouvrir les paramètres réseau**
```
1. Dans la VM Windows Server
2. Panneau de configuration → Réseau et Internet → Centre Réseau et partage
3. Modifier les paramètres de la carte
4. Clic droit sur "Ethernet0" → Propriétés
5. Double-clic sur "Protocole Internet version 4 (TCP/IPv4)"
```

**Étape 2 : Configuration IP statique**
```
Pour VMnet8 (NAT) - Configuration serveur DC :
┌────────────────────────────────────────┐
│ ● Utiliser l'adresse IP suivante :     │
│                                        │
│ Adresse IP     : 192.168.10.10        │
│ Masque         : 255.255.255.0         │
│ Passerelle     : 192.168.10.2          │
│ (Passerelle NAT de VMware)             │
│                                        │
│ ● Utiliser l'adresse de serveur DNS   │
│   suivante :                           │
│                                        │
│ DNS préféré    : 127.0.0.1             │
│ (Lui-même, une fois DNS installé)      │
│                                        │
│ DNS auxiliaire : 192.168.10.2          │
│ (Passerelle VMware pour Internet)      │
│                                        │
│ → OK → OK                              │
└────────────────────────────────────────┘
```

**Étape 3 : Renommer l'ordinateur**
```
1. Gestionnaire de serveur → Serveur local
2. Cliquer sur le nom actuel (ex: WIN-XXXXX)

┌────────────────────────────────────────┐
│ Nom de l'ordinateur : SRV-DC01         │
│                                        │
│ → OK → Redémarrer maintenant           │
└────────────────────────────────────────┘
```

### 💡 Configuration IP via PowerShell

```powershell
# ✅ Configuration IP statique complète en une commande
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.10.10 -PrefixLength 24 -DefaultGateway 192.168.10.2

# ✅ Configurer les DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses ("127.0.0.1","192.168.10.2")

# ✅ Renommer la carte réseau
Rename-NetAdapter -Name "Ethernet0" -NewName "LAN-Production"

# ✅ Renommer l'ordinateur
Rename-Computer -NewName "SRV-DC01" -Restart

# ✅ Vérifier la configuration
Get-NetIPConfiguration
Get-NetIPAddress -InterfaceAlias "LAN-Production"
Test-Connection 192.168.10.2 -Count 2
Test-Connection google.com -Count 2
```

---

## 7️⃣ Snapshots - Points de restauration

### 📌 Pourquoi utiliser les snapshots ?

```
✅ Sauvegarde instantanée de l'état de la VM
✅ Possibilité de revenir en arrière en cas d'erreur
✅ Tester des configurations sans risque
✅ Indispensable avant toute modification majeure

⚠️ Snapshots à créer OBLIGATOIREMENT :
   1. Après installation de Windows Server
   2. Après installation VMware Tools
   3. Avant installation AD DS
   4. Après installation AD DS (avant GPO)
   5. Avant chaque GPO importante
   6. Avant tout changement de configuration réseau
```

### 📌 Créer un snapshot

**Méthode 1 : Via l'interface VMware**
```
1. VM → Snapshot → Prendre un instantané...

┌────────────────────────────────────────┐
│ Nom : 01_Windows_Server_Installe       │
│                                        │
│ Description :                          │
│ Windows Server 2022 installé           │
│ VMware Tools installé                  │
│ IP statique configurée (192.168.10.10) │
│ Nom : SRV-DC01                         │
│ Date : 2026-01-14                      │
│                                        │
│ ☑ Capturer la mémoire de la machine   │
│   virtuelle                            │
│                                        │
│ → Prendre l'instantané                 │
└────────────────────────────────────────┘
```

**Méthode 2 : Snapshot rapide**
```
Raccourci clavier : Ctrl + Alt + S
(Dans VMware Workstation)
```

### 📌 Gestionnaire de snapshots

**Voir tous les snapshots :**
```
1. VM → Snapshot → Gestionnaire d'instantanés...

Vue arborescente :
┌────────────────────────────────────────┐
│ Vous êtes ici                          │
│  │                                     │
│  ├── 01_Windows_Server_Installe        │
│  │                                     │
│  ├── 02_Avant_Installation_AD          │
│  │                                     │
│  ├── 03_AD_Installe                    │
│  │   │                                 │
│  │   ├── 04_Avant_GPO_Securite         │
│  │   │                                 │
│  │   └── 05_GPO_Securite_Active        │
│  │                                     │
│  └── ...                                │
└────────────────────────────────────────┘
```

### 📌 Restaurer un snapshot

**Méthode 1 : Restauration simple**
```
1. VM → Snapshot → Gestionnaire d'instantanés
2. Sélectionner le snapshot souhaité
3. Cliquer "Accéder à"

⚠️ ATTENTION : Vous perdrez TOUTES les modifications
   effectuées après ce snapshot !
```

**Méthode 2 : Cloner avant de restaurer (RECOMMANDÉ)**
```
1. Créer un snapshot de l'état actuel (au cas où)
2. Restaurer le snapshot désiré
3. Si problème, revenir au snapshot récent
```

### 📌 Supprimer un snapshot

```
⚠️ NE PAS supprimer les snapshots trop vite !

Quand supprimer :
✅ Quand vous êtes SÛR que vous n'en avez plus besoin
✅ Quand l'espace disque est critique
✅ Après validation complète d'une config

1. Gestionnaire d'instantanés
2. Sélectionner le snapshot
3. Cliquer "Supprimer"
4. → Les modifications sont fusionnées dans le disque parent
```

### 💡 Bonnes pratiques snapshots

```
✅ Nommer les snapshots de manière descriptive
   Exemple : "02_Avant_GPO_Restriction_USB_2026-01-14"

✅ Ajouter une description détaillée
   Exemple : "Avant application GPO restriction USB.
             Configuration testée : OK
             Utilisateurs de test créés : testuser1-5"

✅ Ne pas garder trop de snapshots (max 5-10)
   → Impact sur les performances

✅ Faire des snapshots AVANT les changements importants
   → Pas après !

❌ Ne JAMAIS copier/déplacer les fichiers .vmdk
   avec des snapshots actifs
   → Corruption garantie

❌ Ne pas utiliser les snapshots comme sauvegarde
   → Utiliser les clones complets pour ça
```

### 📌 Plan de snapshots recommandé

```
Lab Active Directory :
┌──────────────────────────────────────────────────────┐
│ SNAPSHOT                      │ QUAND LE CRÉER       │
├───────────────────────────────┼──────────────────────┤
│ 01_OS_Installe                │ Après install Windows│
│ 02_VMTools_IP_Config          │ Après config réseau  │
│ 03_Avant_AD                   │ Avant promo DC       │
│ 04_AD_DNS_DHCP_OK             │ Après AD/DNS/DHCP    │
│ 05_OU_Structure_Creee         │ Après création OU    │
│ 06_Utilisateurs_Test_Crees    │ Avant GPO            │
│ 07_GPO_Base_Configurees       │ Après GPO de base    │
│ 08_GPO_Avancees_Test          │ Avant déploiement    │
│ 09_Lab_Complet_Fonctionnel    │ État stable final    │
└──────────────────────────────────────────────────────┘

🎯 Garder 09_Lab_Complet_Fonctionnel comme "base propre"
   pour recommencer rapidement
```

---

## 8️⃣ Clonage de VMs (postes clients)

### 📌 Pourquoi cloner ?

```
✅ Créer rapidement plusieurs VMs identiques
✅ Gagner du temps (pas besoin de réinstaller Windows)
✅ Créer des postes clients pour tester les GPO
✅ Simuler un environnement d'entreprise

Types de clones :
┌────────────────────────────────────────┐
│ Clone lié (Linked Clone) :             │
│ ✅ Prend peu d'espace disque            │
│ ✅ Rapide à créer                       │
│ ❌ Dépend de la VM parente              │
│ → Idéal pour tests temporaires         │
│                                        │
│ Clone complet (Full Clone) :           │
│ ✅ Indépendant de la VM parente         │
│ ✅ Peut être déplacé ailleurs           │
│ ❌ Prend autant de place que l'original │
│ → Idéal pour environnement permanent   │
└────────────────────────────────────────┘
```

### 📌 Créer une VM Windows 10/11 (Template)

**Étape 1 : Créer une VM Windows 10/11**
```
1. Créer une nouvelle VM (voir section 3)
   Nom : WIN10-TEMPLATE
   RAM : 4 Go
   CPU : 2 cœurs
   Disque : 60 Go
   Réseau : NAT (VMnet8)

2. Installer Windows 10/11
3. Installer VMware Tools
4. Mettre à jour Windows (Windows Update)
5. NE PAS joindre au domaine
6. NE PAS créer d'utilisateurs spécifiques
```

**Étape 2 : Sysprep (Généraliser la VM)**
```
⚠️ IMPORTANT pour éviter les conflits SID !

1. Dans la VM Windows 10/11
2. Ouvrir CMD en administrateur
3. Exécuter :

cd C:\Windows\System32\Sysprep
sysprep.exe /oobe /generalize /shutdown

┌────────────────────────────────────────┐
│ La VM va :                             │
│ 1. Supprimer les infos spécifiques     │
│ 2. Générer un nouveau SID au prochain  │
│    démarrage                           │
│ 3. S'éteindre                          │
└────────────────────────────────────────┘

⚠️ NE PAS REDÉMARRER cette VM !
   Elle sera utilisée comme template
```

**Étape 3 : Créer un snapshot du template**
```
1. VM → Snapshot → Prendre un instantané
   Nom : Template_Win10_Sysprep_Ready

2. Ce template ne doit JAMAIS être démarré
   → Toujours cloner avant d'utiliser
```

### 📌 Cloner la VM

**Méthode 1 : Clone lié (recommandé pour lab)**
```
1. Clic droit sur WIN10-TEMPLATE → Gérer → Cloner...

┌────────────────────────────────────────┐
│ État actuel ou snapshot :              │
│ ● Instantané existant :                │
│   Template_Win10_Sysprep_Ready         │
│ → Suivant                              │
│                                        │
│ Type de clone :                        │
│ ● Créer un clone lié                   │
│ → Suivant                              │
│                                        │
│ Nom : PC-CLIENT01                      │
│ Emplacement :                          │
│   D:\VMware\LAB-TSSR\PC-CLIENT01       │
│ → Terminer                             │
└────────────────────────────────────────┘
```

**Méthode 2 : Clone complet**
```
Même processus, mais :
┌────────────────────────────────────────┐
│ Type de clone :                        │
│ ● Créer un clone complet               │
│                                        │
│ ⏱️ Temps de clonage : 5-10 minutes     │
│ 💾 Espace requis : 60 Go               │
└────────────────────────────────────────┘
```

### 📌 Configuration du clone

**Étape 4 : Première configuration du clone**
```
1. Démarrer PC-CLIENT01
2. Windows démarre en mode OOBE (comme première installation)

┌────────────────────────────────────────┐
│ Région : France                        │
│ Disposition clavier : Français         │
│                                        │
│ Nom du PC : PC-CLIENT01                │
│                                        │
│ Compte local :                         │
│   Utilisateur : UserLocal              │
│   Mot de passe : P@ssw0rd123!          │
│                                        │
│ → Terminer                             │
└────────────────────────────────────────┘
```

**Étape 5 : Joindre au domaine**
```
1. Configurer DNS → 192.168.10.10 (le DC)

PowerShell (Admin) :
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses "192.168.10.10"

2. Joindre le domaine

Add-Computer -DomainName "entreprise.local" -Credential ENTREPRISE\Administrateur -Restart

OU via GUI :
Système → Modifier les paramètres → Modifier → Domaine : entreprise.local
```

### 📌 Créer plusieurs clones rapidement

**Pour créer 5 postes clients :**
```
1. Cloner WIN10-TEMPLATE → PC-CLIENT01
2. Cloner WIN10-TEMPLATE → PC-CLIENT02
3. Cloner WIN10-TEMPLATE → PC-CLIENT03
4. Cloner WIN10-TEMPLATE → PC-CLIENT04
5. Cloner WIN10-TEMPLATE → PC-CLIENT05

Puis pour chaque :
- Démarrer
- Configurer nom unique
- Joindre au domaine
- Déplacer dans l'OU appropriée (via AD)
```

### 💡 Script PowerShell pour configuration rapide

```powershell
# ✅ Script à exécuter sur chaque clone (après premier boot)
# Adapter les valeurs selon le PC

# Configuration réseau
$IPAddress = "192.168.10.101"  # Changer pour chaque PC
$ComputerName = "PC-CLIENT01"   # Changer pour chaque PC

# Renommer le PC
Rename-Computer -NewName $ComputerName -Force

# Configurer DNS vers le DC
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses "192.168.10.10"

# Joindre au domaine
$Credential = Get-Credential -Message "Entrer les identifiants domaine (ENTREPRISE\Administrateur)"
Add-Computer -DomainName "entreprise.local" -Credential $Credential -Restart
```

---

## 9️⃣ Optimisation des performances

### 📌 Paramètres VMware pour meilleures performances

**Étape 1 : Paramètres VM**
```
1. Clic droit sur la VM → Paramètres
2. Onglet "Options"

┌────────────────────────────────────────┐
│ Options avancées :                     │
│ ☑ Désactiver la mise en veille         │
│   automatique de la VM                 │
│                                        │
│ ☑ Activer la technologie de            │
│   virtualisation assistée par le       │
│   matériel (VT-x/AMD-V)                │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Étape 2 : Fichier .vmx (avancé)**
```
⚠️ FERMER la VM avant de modifier

1. Aller dans le dossier de la VM
2. Éditer SRV-DC01.vmx avec un éditeur de texte

Ajouter ces lignes à la fin :
┌────────────────────────────────────────┐
│ # Optimisations performances           │
│ mainMem.useNamedFile = "FALSE"         │
│ MemTrimRate = "0"                      │
│ prefvmx.useRecommendedLockedMemSize    │
│   = "TRUE"                             │
│ MemAllowAutoScaleDown = "FALSE"        │
│ sched.mem.pshare.enable = "FALSE"      │
│                                        │
│ # Optimisation disque                  │
│ disk.EnableUUID = "TRUE"               │
│                                        │
│ # Désactiver le défragmentation auto   │
│ mainMem.backing = "swap"               │
└────────────────────────────────────────┘

3. Sauvegarder et redémarrer la VM
```

### 📌 Optimisations dans Windows Server

**Désactiver les services inutiles :**
```powershell
# ✅ Désactiver les services non nécessaires en VM

# Windows Search (indexation)
Set-Service -Name "WSearch" -StartupType Disabled
Stop-Service -Name "WSearch" -Force

# Superfetch (inutile en VM)
Set-Service -Name "SysMain" -StartupType Disabled
Stop-Service -Name "SysMain" -Force

# Windows Update (contrôler manuellement)
Set-Service -Name "wuauserv" -StartupType Manual

# Défragmentation planifiée (inutile sur disque virtuel)
Disable-ScheduledTask -TaskName "Microsoft\Windows\Defrag\ScheduledDefrag"
```

**Optimiser les effets visuels :**
```
1. Système → Paramètres système avancés
2. Performances → Paramètres

┌────────────────────────────────────────┐
│ Effets visuels :                       │
│ ● Ajuster afin d'obtenir les          │
│   meilleures performances              │
│                                        │
│ OU personnaliser :                     │
│ ☐ Animer les fenêtres                  │
│ ☐ Afficher les ombres                  │
│ ☑ Lisser les polices d'écran           │
│   (Garder pour lisibilité)             │
│                                        │
│ → OK                                   │
└────────────────────────────────────────┘
```

**Désactiver la mise en veille :**
```powershell
# ✅ Désactiver toutes les mises en veille
powercfg /change standby-timeout-ac 0
powercfg /change standby-timeout-dc 0
powercfg /change monitor-timeout-ac 0
powercfg /change hibernate-timeout-ac 0

# ✅ Activer le plan Hautes performances
powercfg /setactive 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
```

### 📌 Optimisations sur l'hôte

**Allouer plus de RAM si possible :**
```
⚠️ Règle générale :
   RAM hôte = RAM VMs + 8 Go pour l'hôte

Exemple avec 32 Go de RAM sur l'hôte :
- VM Serveur : 8 Go
- VM Client 1 : 4 Go
- VM Client 2 : 4 Go
- Hôte : 16 Go restants
→ Configuration confortable
```

**Utiliser un SSD :**
```
✅ SSD = Performances × 5-10 par rapport à un HDD
✅ Temps de boot : 10s au lieu de 1 minute
✅ Installation AD : 5 min au lieu de 15 min

Si possible :
- Installer VMware sur le SSD
- Stocker les VMs sur le SSD
- Garder les ISOs sur HDD (pas besoin de vitesse)
```

---

## 🔟 Troubleshooting VMware

### 📌 Problèmes réseau courants

**Problème 1 : Pas d'accès Internet dans la VM**
```
Diagnostic :
1. Dans la VM : ping 192.168.10.2 (passerelle VMware)
   ❌ Échec → Problème carte réseau VM
   ✅ OK → Continuer

2. ping 8.8.8.8
   ❌ Échec → Problème NAT VMware
   ✅ OK → Continuer

3. ping google.com
   ❌ Échec → Problème DNS
   ✅ OK → Tout fonctionne

Solutions :
- Vérifier que la carte réseau est en NAT (VMnet8)
- Redémarrer les services VMware sur l'hôte :
  Services → VMware NAT Service → Redémarrer
  Services → VMware DHCP Service → Redémarrer

- Vérifier le pare-feu de l'hôte (autoriser VMware)
```

**Problème 2 : Impossible de joindre le domaine**
```
Diagnostic :
1. Sur le client : ping 192.168.10.10 (DC)
   ❌ Échec → Vérifier IP/réseau

2. nslookup entreprise.local
   ❌ Échec → DNS mal configuré

Solutions :
- Vérifier que DNS client pointe vers 192.168.10.10
- Vérifier que le serveur DNS fonctionne sur le DC
- Vérifier que les deux VMs sont sur le même réseau (VMnet8)
```

### 📌 Problèmes de performances

**VM très lente :**
```
Causes possibles :
1. Pas assez de RAM allouée
   → Augmenter à 4-8 Go pour le serveur

2. Trop de VMs démarrées simultanément
   → Éteindre les VMs non utilisées

3. Snapshots multiples actifs
   → Consolider les snapshots anciens

4. Disque dur hôte saturé
   → Utiliser un SSD
   → Défragmenter le disque hôte

5. VMware Tools non installé
   → Installer VMware Tools
```

**Vérifications :**
```powershell
# Dans la VM, vérifier l'utilisation ressources
Get-Counter '\Memory\Available MBytes'
Get-Counter '\Processor(_Total)\% Processor Time'

# Voir les processus gourmands
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```

### 📌 Problèmes de snapshots

**Erreur : "Snapshot delta file too large"**
```
Cause : Le snapshot a trop grandi (plusieurs Go)

Solution :
1. Consolider le snapshot :
   VM → Snapshot → Gestionnaire d'instantanés
   → Supprimer le snapshot

2. Ou créer un nouveau clone complet de la VM
```

**Impossible de supprimer un snapshot :**
```
Solution :
1. Éteindre complètement la VM
2. VM → Snapshot → Gestionnaire → Supprimer
3. Attendre (peut prendre 10-30 minutes)
4. NE PAS interrompre le processus
```

### 📌 Problèmes VMware Tools

**VMware Tools non détecté :**
```
1. Vérifier le service dans la VM :
   Get-Service VMTools

2. Réinstaller VMware Tools :
   VM → Réinstaller VMware Tools

3. Redémarrer la VM
```

### 💡 Commandes de diagnostic

```powershell
# ✅ Dans la VM Windows :

# Vérifier la connectivité réseau
Test-NetConnection -ComputerName google.com -InformationLevel Detailed

# Vérifier les services critiques
Get-Service | Where-Object {$_.Status -eq "Running"} | Select-Object Name, DisplayName

# Vérifier les logs événements
Get-WinEvent -LogName System -MaxEvents 20 | Where-Object {$_.LevelDisplayName -eq "Error"}

# Espace disque
Get-PSDrive -PSProvider FileSystem

# Performances réseau
Test-Connection -ComputerName 192.168.10.2 -Count 10 | Measure-Object -Property ResponseTime -Average

# ✅ Sur l'hôte Windows :

# Voir l'utilisation ressources des VMs
Get-Process -Name vmware* | Select-Object Name, CPU, WorkingSet

# Services VMware
Get-Service | Where-Object {$_.Name -like "VM*"} | Select-Object Name, Status
```

---

## 📊 Récapitulatif de la configuration

### ✅ Checklist complète du lab

```
INFRASTRUCTURE VMWARE :
- [ ] VMware Workstation installé
- [ ] Éditeur de réseau virtuel configuré (VMnet8)
- [ ] Services VMware démarrés

VM SERVEUR (SRV-DC01) :
- [ ] VM créée (4 Go RAM, 2 CPU, 60 Go disque)
- [ ] Windows Server 2022 installé
- [ ] VMware Tools installé
- [ ] IP statique : 192.168.10.10
- [ ] Nom : SRV-DC01
- [ ] Snapshot "Base propre" créé

VM CLIENTS (PC-CLIENT01...) :
- [ ] Template Windows 10/11 créé
- [ ] Sysprep effectué
- [ ] Clones créés
- [ ] Joints au domaine
- [ ] Dans les bonnes OU

SNAPSHOTS :
- [ ] Snapshot après install OS
- [ ] Snapshot après config réseau
- [ ] Snapshot avant AD
- [ ] Snapshot après AD/DNS/DHCP
- [ ] Snapshot avant GPO

RÉSEAU :
- [ ] Toutes les VMs sur VMnet8
- [ ] Ping DC ↔ Clients fonctionne
- [ ] Ping vers Internet fonctionne
- [ ] Résolution DNS fonctionne
```

### 🎯 Schéma final du lab

```
┌─────────────────────────────────────────────────────────┐
│ MACHINE HÔTE                                            │
│ IP sur VMnet8 : 192.168.10.1                            │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ VMnet8 (NAT) - 192.168.10.0/24                  │   │
│  │ Passerelle : 192.168.10.2                       │   │
│  │                                                 │   │
│  │  ┌──────────────┐  ┌──────────┐  ┌──────────┐  │   │
│  │  │ SRV-DC01     │  │ CLIENT01 │  │ CLIENT02 │  │   │
│  │  │ .10          │  │ DHCP     │  │ DHCP     │  │   │
│  │  │ DC+DNS+DHCP  │  │ Domaine  │  │ Domaine  │  │   │
│  │  └──────────────┘  └──────────┘  └──────────┘  │   │
│  └─────────────────────────────────────────────────┘   │
│                    ↓ NAT                                │
│              [Internet]                                 │
└─────────────────────────────────────────────────────────┘

Domaine : entreprise.local
Plage DHCP : 192.168.10.100 - 192.168.10.200
DNS : 192.168.10.10
Passerelle : 192.168.10.2
```

---

**📌 Fin du guide VMware**

Tu as maintenant un guide complet pour créer et gérer ton lab Windows Server dans VMware ! Ce guide complète parfaitement les guides de configuration AD et GPO.

**Prochaine étape :** Suivre le "Guide_Configuration_GPO_Etape_Par_Etape.md" pour configurer ton infrastructure AD dans ce lab VMware.
