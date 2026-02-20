# Serveur FTP sous Windows Server 2025

> 📚 **Module :** Windows Server  
> 📅 **Date :** Janvier 2026  
> ⏱️ **Durée :** 4 heures  
> 🎯 **Niveau :** Intermédiaire  
> 🔗 **Prérequis :** Active Directory configuré

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Qu'est-ce qu'un serveur FTP ?](#-quest-ce-quun-serveur-ftp)
- [Installation du serveur FTP](#-installation-du-serveur-ftp)
- [Configuration de base](#-configuration-de-base)
- [Authentification avec Active Directory](#-authentification-avec-active-directory)
- [Permissions et sécurité](#-permissions-et-sécurité)
- [Configuration du pare-feu](#-configuration-du-pare-feu)
- [Ports passifs FTP](#-ports-passifs-ftp)
- [Accès depuis les clients](#-accès-depuis-les-clients)
- [Sécurisation avec FTPS](#-sécurisation-avec-ftps)
- [Dépannage](#-dépannage)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Expliquer le rôle d'un serveur FTP en entreprise
- ✅ Installer et configurer IIS avec le rôle FTP
- ✅ Créer un site FTP avec authentification Active Directory
- ✅ Configurer les permissions NTFS pour les utilisateurs FTP
- ✅ Gérer les ports passifs et le pare-feu Windows
- ✅ Tester l'accès FTP depuis différents clients
- ✅ Sécuriser le serveur FTP avec FTPS (SSL/TLS)
- ✅ Diagnostiquer et résoudre les problèmes FTP courants

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir un serveur Windows Server 2025 installé
- [ ] Avoir configuré Active Directory (contrôleur de domaine)
- [ ] Avoir des utilisateurs créés dans AD
- [ ] Comprendre les permissions NTFS
- [ ] Connaître les bases du modèle OSI (couches 3-4-7)

**Matériel nécessaire :**
- 💻 VM Windows Server 2025 (contrôleur de domaine)
- 💻 VM Windows 10 (client pour tester)
- 🌐 Réseau configuré (même subnet)

**Environnement lab :**
```
Serveur : WIN2025TP
IP      : 192.168.230.10
Domaine : solaris.local

Client  : PC-WIN10
IP      : 192.168.230.50
```

---

## 📚 Qu'est-ce qu'un serveur FTP ?

### Définition

**FTP (File Transfer Protocol)** est un protocole de transfert de fichiers entre un serveur et un client. C'est un des plus anciens protocoles Internet (créé en 1971 !).

### Pourquoi utiliser FTP en entreprise ?

| Avantage | Description |
|----------|-------------|
| **Transfert de gros fichiers** | Plus fiable que les emails (limite 25 MB généralement) |
| **Accès distant** | Les utilisateurs peuvent accéder aux fichiers depuis n'importe où |
| **Gestion centralisée** | Tous les fichiers sont au même endroit |
| **Permissions granulaires** | Contrôle précis de qui peut lire/écrire |
| **Automatisation** | Scripts de sauvegarde automatiques |

### Cas d'usage réels

✅ **Échange avec partenaires** : Un cabinet comptable reçoit des documents de ses clients  
✅ **Dépôt de fichiers** : Les commerciaux déposent leurs rapports depuis l'extérieur  
✅ **Sauvegarde** : Backup automatique vers un serveur FTP distant  
✅ **Mise à disposition** : Partage de fichiers volumineux (vidéos, CAO, etc.)  

### FTP vs autres solutions

| Solution | Avantages | Inconvénients |
|----------|-----------|---------------|
| **FTP** | Universel, rapide, automatisable | Non sécurisé par défaut |
| **FTPS** | FTP sécurisé (SSL/TLS) | Plus complexe à configurer |
| **SFTP** | Sécurisé (SSH), pas de port passif | Nécessite serveur SSH (Linux) |
| **Partage SMB** | Intégré Windows, simple | Difficile via Internet |
| **Cloud** | Accessible partout | Coût récurrent, dépendance |

> 💡 **Conseil TSSR :** En 2026, privilégiez **FTPS** (FTP sécurisé) plutôt que FTP classique. On va voir les deux dans ce cours.

---

## ⚙️ Installation du serveur FTP

### Comprendre IIS

Le serveur FTP sous Windows fait partie d'**IIS (Internet Information Services)**, le serveur web de Microsoft.

```
IIS = Serveur Web + Serveur FTP + Autres services
```

### Méthode 1 : Installation via le Gestionnaire de serveur

**Étape par étape :**

1. Ouvrez le **Gestionnaire de serveur**
2. Cliquez sur **Gérer** → **Ajouter des rôles et fonctionnalités**
3. Cliquez sur **Suivant** jusqu'à **Sélection des rôles de serveurs**
4. Cochez **Serveur Web (IIS)**
5. Dans la popup, cliquez sur **Ajouter les fonctionnalités**
6. Cliquez sur **Suivant** jusqu'à **Services de rôle**
7. Développez **Serveur Web (IIS)** → **Serveur FTP**
8. Cochez :
   - ☑ **Serveur FTP**
   - ☑ **Extensibilité FTP**
9. Cliquez sur **Suivant** puis **Installer**
10. Attendez la fin de l'installation (2-5 minutes)

### Méthode 2 : Installation via PowerShell (rapide !)

```powershell
# Installer IIS et le serveur FTP
Install-WindowsFeature -Name Web-Server,Web-Ftp-Server -IncludeManagementTools

# Vérifier l'installation
Get-WindowsFeature | Where-Object {$_.Name -like "*FTP*"}

# Résultat attendu :
# [X] Web-FTP-Server         Serveur FTP
# [X] Web-Ftp-Service        Service FTP
# [X] Web-Ftp-Ext            Extensibilité FTP
```

### Vérification de l'installation

```powershell
# Vérifier que le service FTP est installé
Get-Service -Name ftpsvc

# Statut attendu :
# Status : Stopped (normal, on n'a pas encore créé de site)
# StartType : Automatic

# Démarrer le service FTP
Start-Service -Name ftpsvc

# Vérifier le statut
Get-Service -Name ftpsvc
# Status : Running ✅
```

> 💡 **Astuce :** Le service FTP ne démarre vraiment que quand vous créez votre premier site FTP.

---

## 🔧 Configuration de base

### Créer le dossier FTP

**Méthode 1 : Interface graphique**

1. Ouvrez l'**Explorateur de fichiers**
2. Créez un nouveau dossier : **C:\FTP**
3. *Facultatif :* Créez des sous-dossiers pour organiser :
   ```
   C:\FTP\
   ├── Uploads\      (pour les dépôts)
   ├── Downloads\    (pour les téléchargements)
   └── Partage\      (pour les fichiers communs)
   ```

**Méthode 2 : PowerShell**

```powershell
# Créer le dossier FTP principal
New-Item -Path "C:\FTP" -ItemType Directory

# Créer des sous-dossiers (optionnel)
New-Item -Path "C:\FTP\Uploads" -ItemType Directory
New-Item -Path "C:\FTP\Downloads" -ItemType Directory
New-Item -Path "C:\FTP\Partage" -ItemType Directory

# Vérifier
Get-ChildItem -Path "C:\FTP"
```

### Configurer les permissions NTFS de base

**Important :** Les permissions FTP = **Permissions NTFS** × **Permissions FTP**  
(La plus restrictive gagne)

```powershell
# Donner le contrôle total aux Administrateurs
$acl = Get-Acl "C:\FTP"
$permission = "BUILTIN\Administrateurs","FullControl","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP" $acl

# Vérifier les permissions
Get-Acl "C:\FTP" | Format-List
```

### Créer le site FTP dans IIS

**Méthode 1 : Interface graphique**

1. Appuyez sur **Windows + R** → Tapez **inetmgr** → OK
2. Le **Gestionnaire des services Internet (IIS)** s'ouvre
3. Développez votre serveur dans l'arborescence à gauche
4. Clic droit sur **Sites** → **Ajouter un site FTP...**

**Configuration du site :**

| Paramètre | Valeur | Explication |
|-----------|--------|-------------|
| **Nom du site FTP** | FTP-Solaris | Nom interne (libre) |
| **Chemin d'accès physique** | C:\FTP | Dossier racine |

Cliquez sur **Suivant**

**Configuration de la liaison :**

| Paramètre | Valeur | Explication |
|-----------|--------|-------------|
| **Adresse IP** | Toutes non attribuées | Ou sélectionnez 192.168.230.10 |
| **Port** | 21 | Port FTP standard |
| **Activer l'hôte virtuel** | ☐ Non coché | Pas nécessaire pour un lab |
| **SSL** | Aucun | On verra FTPS plus tard |
| **Démarrer le site FTP automatiquement** | ☑ Coché | Important ! |

Cliquez sur **Suivant**

**Configuration de l'authentification et de l'autorisation :**

| Paramètre | Valeur | Explication |
|-----------|--------|-------------|
| **Anonyme** | ☐ NON coché | On veut identifier les utilisateurs |
| **De base** | ☑ Coché | Utilise les comptes Windows/AD |
| **Autoriser l'accès à** | Utilisateurs spécifiés | Contrôle précis |
| **Utilisateurs** | mcurie | Ou un groupe AD |
| **Permissions** | ☑ Lecture + ☑ Écriture | L'utilisateur peut tout faire |

Cliquez sur **Terminer**

### Méthode 2 : PowerShell (création automatisée)

```powershell
# Importer le module WebAdministration
Import-Module WebAdministration

# Créer le site FTP
New-WebFtpSite -Name "FTP-Solaris" `
    -Port 21 `
    -PhysicalPath "C:\FTP"

# Configurer l'authentification de base
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.security.authentication.basicAuthentication.enabled `
    -Value $true

# Désactiver l'authentification anonyme
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.security.authentication.anonymousAuthentication.enabled `
    -Value $false

# Démarrer le site
Start-Website -Name "FTP-Solaris"

# Vérifier
Get-Website -Name "FTP-Solaris"
```

### Vérification du site FTP

```powershell
# Vérifier que le service FTP est démarré
Get-Service -Name ftpsvc
# Status : Running ✅

# Vérifier que le site FTP existe
Get-IISSite | Where-Object {$_.Name -like "*FTP*"}

# Tester localement (depuis le serveur)
Test-NetConnection -ComputerName localhost -Port 21
# TcpTestSucceeded : True ✅
```

---

## 🔐 Authentification avec Active Directory

### Pourquoi utiliser AD ?

✅ **Centralisation** : Un seul compte pour FTP, réseau, email, etc.  
✅ **Sécurité** : Politique de mots de passe complexes  
✅ **Groupes** : Gérer les permissions par département  
✅ **Audit** : Savoir qui a accédé à quoi  

### Configurer les permissions pour un utilisateur AD

**Scénario :** Marie Curie (mcurie@solaris.local) doit accéder au FTP

**Étape 1 : Donner les permissions NTFS**

```powershell
# Ajouter l'utilisateur aux permissions NTFS
$acl = Get-Acl "C:\FTP"
$permission = "SOLARIS\mcurie","Modify","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP" $acl

# Vérifier
Get-Acl "C:\FTP" | Select-Object -ExpandProperty Access | Where-Object {$_.IdentityReference -like "*mcurie*"}
```

**Étape 2 : Autoriser l'utilisateur dans IIS**

**Interface graphique :**

1. Ouvrez **IIS Manager** (inetmgr)
2. Développez **Sites** → **FTP-Solaris**
3. Double-cliquez sur **Autorisation FTP**
4. Dans le panneau Actions (à droite), cliquez sur **Ajouter une règle d'autorisation...**
5. Configuration :
   - ☑ **Utilisateurs spécifiés**
   - Noms : **mcurie**
   - Permissions : ☑ **Lecture** + ☑ **Écriture**
6. Cliquez sur **OK**

**PowerShell :**

```powershell
# Ajouter une règle d'autorisation FTP
Add-WebConfiguration "/system.ftpServer/security/authorization" `
    -Value @{accessType='Allow'; users='mcurie'; permissions='Read,Write'} `
    -PSPath "IIS:\" `
    -Location "FTP-Solaris"

# Vérifier les règles
Get-WebConfiguration "/system.ftpServer/security/authorization" `
    -PSPath "IIS:\" `
    -Location "FTP-Solaris" | Select-Object -ExpandProperty Collection
```

### Utiliser des groupes AD (recommandé)

**Meilleure pratique :** Créer un groupe AD pour le FTP

```powershell
# Créer un groupe AD pour le FTP
New-ADGroup -Name "G_FTP_Users" `
    -GroupScope Global `
    -GroupCategory Security `
    -Path "OU=Solaris_Corp,DC=solaris,DC=local" `
    -Description "Utilisateurs ayant accès au serveur FTP"

# Ajouter Marie au groupe
Add-ADGroupMember -Identity "G_FTP_Users" -Members "mcurie"

# Vérifier
Get-ADGroupMember -Identity "G_FTP_Users" | Select-Object Name,SamAccountName
```

**Puis donner les permissions au groupe :**

```powershell
# Permissions NTFS pour le groupe
$acl = Get-Acl "C:\FTP"
$permission = "SOLARIS\G_FTP_Users","Modify","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP" $acl

# Autorisation FTP pour le groupe
Add-WebConfiguration "/system.ftpServer/security/authorization" `
    -Value @{accessType='Allow'; roles='G_FTP_Users'; permissions='Read,Write'} `
    -PSPath "IIS:\" `
    -Location "FTP-Solaris"
```

> 💡 **Avantage des groupes :** Ajoutez un utilisateur au groupe → Il a automatiquement accès au FTP !

---

## 🛡️ Permissions et sécurité

### Comprendre les permissions FTP

**2 niveaux de permissions :**

```
Accès FTP = Permissions NTFS ∩ Permissions FTP
            (la plus restrictive gagne)
```

**Exemple :**

| NTFS | FTP | Résultat |
|------|-----|----------|
| Lecture seule | Lecture + Écriture | **Lecture seule** (NTFS gagne) |
| Lecture + Écriture | Lecture seule | **Lecture seule** (FTP gagne) |
| Lecture + Écriture | Lecture + Écriture | **Lecture + Écriture** ✅ |

### Créer des dossiers avec permissions différentes

**Scénario réel :** 

- **Uploads/** : Tout le monde peut déposer des fichiers
- **Downloads/** : Tout le monde peut télécharger (lecture seule)
- **Admin/** : Seulement les administrateurs

```powershell
# Créer la structure
New-Item -Path "C:\FTP\Uploads" -ItemType Directory
New-Item -Path "C:\FTP\Downloads" -ItemType Directory
New-Item -Path "C:\FTP\Admin" -ItemType Directory

# Uploads : Écriture pour G_FTP_Users
$acl = Get-Acl "C:\FTP\Uploads"
$permission = "SOLARIS\G_FTP_Users","Write","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP\Uploads" $acl

# Downloads : Lecture seule pour G_FTP_Users
$acl = Get-Acl "C:\FTP\Downloads"
$permission = "SOLARIS\G_FTP_Users","Read","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP\Downloads" $acl

# Admin : Contrôle total pour Administrateurs uniquement
$acl = Get-Acl "C:\FTP\Admin"
$permission = "BUILTIN\Administrateurs","FullControl","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
# Supprimer les autres permissions (optionnel)
Set-Acl "C:\FTP\Admin" $acl
```

### Isolation des utilisateurs (répertoires personnels)

**Concept :** Chaque utilisateur ne voit que son dossier

**Configuration IIS :**

1. Dans IIS Manager, sélectionnez le site **FTP-Solaris**
2. Double-cliquez sur **Isolation des utilisateurs FTP**
3. Sélectionnez : **Nom d'utilisateur du répertoire (restreindre les utilisateurs à leur répertoire)**
4. Cliquez sur **Appliquer**

**Structure des dossiers :**

```
C:\FTP\LocalUser\
├── mcurie\         (Marie ne voit que ça)
├── jdupont\        (Jean ne voit que ça)
└── asmith\         (Alice ne voit que ça)
```

**PowerShell pour créer les dossiers utilisateurs :**

```powershell
# Créer le dossier parent
New-Item -Path "C:\FTP\LocalUser" -ItemType Directory

# Créer les dossiers utilisateurs
$users = @("mcurie", "jdupont", "asmith")
foreach ($user in $users) {
    $userPath = "C:\FTP\LocalUser\$user"
    New-Item -Path $userPath -ItemType Directory
    
    # Donner le contrôle total à l'utilisateur sur SON dossier
    $acl = Get-Acl $userPath
    $permission = "SOLARIS\$user","FullControl","Allow"
    $accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
    $acl.SetAccessRule($accessRule)
    Set-Acl $userPath $acl
}
```

---

## 🔥 Configuration du pare-feu

### Pourquoi configurer le pare-feu ?

Par défaut, Windows bloque **TOUS** les ports entrants (sécurité).  
Il faut autoriser :
- **Port 21** : Connexion FTP (commandes)
- **Ports 5000-5020** : Transfert de données en mode passif

### Méthode 1 : Interface graphique

**Ouvrir le port 21 :**

1. Ouvrez **Pare-feu Windows Defender avec fonctions avancées de sécurité**
2. Cliquez sur **Règles de trafic entrant** dans le panneau gauche
3. Clic droit → **Nouvelle règle...**
4. Type de règle : **Port** → Suivant
5. Protocole : **TCP** → Ports locaux spécifiques : **21** → Suivant
6. Action : **Autoriser la connexion** → Suivant
7. Profils : Cochez **Domaine**, **Privé**, **Public** → Suivant
8. Nom : **FTP Server (Port 21)** → Terminer

**Ouvrir les ports passifs (5000-5020) :**

Répétez la procédure avec :
- Ports : **5000-5020**
- Nom : **FTP Passive Ports**

### Méthode 2 : PowerShell (rapide)

```powershell
# Autoriser le port 21 (commandes FTP)
New-NetFirewallRule -DisplayName "FTP Server (Port 21)" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 21 `
    -Action Allow `
    -Profile Domain,Private,Public

# Autoriser les ports passifs (5000-5020)
New-NetFirewallRule -DisplayName "FTP Passive Ports" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 5000-5020 `
    -Action Allow `
    -Profile Domain,Private,Public

# Vérifier les règles créées
Get-NetFirewallRule | Where-Object {$_.DisplayName -like "*FTP*"} | Select-Object DisplayName,Enabled,Direction
```

### Vérification

```powershell
# Tester le port 21 localement
Test-NetConnection -ComputerName localhost -Port 21
# TcpTestSucceeded : True ✅

# Tester depuis le client Windows 10
Test-NetConnection -ComputerName 192.168.230.10 -Port 21
# TcpTestSucceeded : True ✅
```

---

## 🔄 Ports passifs FTP

### Comprendre le mode passif

**Problème FTP classique :**  
FTP utilise **2 connexions** :
1. **Port 21** : Commandes (LIST, GET, PUT)
2. **Port 20** : Données (transfert réel des fichiers)

En mode **actif**, le serveur initie la connexion données → Bloqué par les pare-feu !

En mode **passif**, le client initie TOUT → Fonctionne avec les pare-feu ✅

### Configurer les ports passifs dans IIS

**Interface graphique :**

1. Ouvrez **IIS Manager**
2. Sélectionnez votre **serveur** (racine, pas le site !)
3. Double-cliquez sur **Pare-feu de support FTP** (FTP Firewall Support)
4. Configuration :
   - **Plage de ports de données :** 5000-5020
   - **Adresse IP externe du pare-feu :** 192.168.230.10
5. Dans le panneau Actions (droite), cliquez sur **Appliquer**

**PowerShell :**

```powershell
# Configurer la plage de ports passifs
Set-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "lowDataChannelPort" -Value 5000

Set-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "highDataChannelPort" -Value 5020

# Configurer l'IP externe
Set-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "externalIp4Address" -Value "192.168.230.10"

# Redémarrer le service FTP pour appliquer
Restart-Service ftpsvc

# Vérifier la configuration
Get-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "lowDataChannelPort" | Select-Object Value

Get-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "highDataChannelPort" | Select-Object Value
```

> 💡 **Pourquoi 5000-5020 ?** Cette plage de 20 ports permet **20 connexions FTP simultanées**. Adaptez selon vos besoins (ex: 5000-5050 pour 50 connexions).

---

## 💻 Accès depuis les clients

### Méthode 1 : Explorateur Windows

**Sur le PC Windows 10 client :**

1. Ouvrez l'**Explorateur de fichiers**
2. Dans la barre d'adresse, tapez : **ftp://192.168.230.10**
3. Appuyez sur **Entrée**
4. Une fenêtre de connexion apparaît
5. Nom d'utilisateur : **mcurie** (ou **SOLARIS\mcurie**)
6. Mot de passe : [mot de passe de Marie]
7. Vous voyez le contenu de C:\FTP ! ✅

**Mapper comme lecteur réseau :**

1. Dans l'Explorateur, clic droit sur **Ce PC**
2. **Connecter un lecteur réseau**
3. Lecteur : **F:** (ou autre lettre libre)
4. Dossier : **ftp://192.168.230.10**
5. ☑ Cochez **Se reconnecter lors de l'ouverture de session**
6. Cliquez sur **Terminer**
7. Entrez les identifiants

### Méthode 2 : Client FTP en ligne de commandes

**Windows intègre un client FTP basique :**

```cmd
# Se connecter au serveur
ftp 192.168.230.10

# Entrer les identifiants quand demandé
User: mcurie
Password: [votre_mot_de_passe]

# Commandes FTP de base
ls              # Lister les fichiers
dir             # Lister avec détails
cd dossier      # Changer de dossier
pwd             # Afficher le dossier actuel

# Télécharger un fichier (GET)
get fichier.txt

# Uploader un fichier (PUT)
put monFichier.doc

# Télécharger plusieurs fichiers
mget *.txt

# Uploader plusieurs fichiers
mput *.doc

# Créer un dossier
mkdir nouveau_dossier

# Supprimer un fichier
delete fichier.txt

# Quitter
bye
```

### Méthode 3 : FileZilla (client graphique professionnel)

**Installation :**
1. Téléchargez FileZilla Client sur [filezilla-project.org](https://filezilla-project.org/)
2. Installez-le sur le PC Windows 10

**Connexion :**

| Paramètre | Valeur |
|-----------|--------|
| **Hôte** | 192.168.230.10 |
| **Nom d'utilisateur** | mcurie |
| **Mot de passe** | [mot_de_passe] |
| **Port** | 21 |

Cliquez sur **Connexion rapide**

**Avantages FileZilla :**
- ✅ Interface graphique intuitive
- ✅ Transferts multiples simultanés
- ✅ Reprendre les transferts interrompus
- ✅ Enregistrer les connexions (Gestionnaire de sites)
- ✅ Synchronisation de dossiers

### Méthode 4 : PowerShell (avancé)

```powershell
# Télécharger un fichier depuis FTP
$url = "ftp://192.168.230.10/fichier.txt"
$output = "C:\Temp\fichier.txt"
$username = "mcurie"
$password = "MonMotDePasse"

$request = [System.Net.FtpWebRequest]::Create($url)
$request.Method = [System.Net.WebRequestMethods+Ftp]::DownloadFile
$request.Credentials = New-Object System.Net.NetworkCredential($username, $password)
$response = $request.GetResponse()

$responseStream = $response.GetResponseStream()
$file = [System.IO.File]::Create($output)
$buffer = New-Object byte[] 1024

do {
    $count = $responseStream.Read($buffer, 0, $buffer.Length)
    $file.Write($buffer, 0, $count)
} while ($count -gt 0)

$file.Close()
$responseStream.Close()
$response.Close()

Write-Host "Fichier téléchargé : $output" -ForegroundColor Green
```

---

## 🔒 Sécurisation avec FTPS

### Pourquoi FTPS ?

**FTP classique = NON SÉCURISÉ** :
- ❌ Mots de passe en clair
- ❌ Fichiers transférés en clair
- ❌ Risque d'interception (Man-in-the-Middle)

**FTPS = FTP sécurisé avec SSL/TLS** :
- ✅ Chiffrement des identifiants
- ✅ Chiffrement des données
- ✅ Authentification du serveur (certificat)

### Générer un certificat SSL auto-signé

**Interface graphique :**

1. Ouvrez **IIS Manager**
2. Sélectionnez votre **serveur** (racine)
3. Double-cliquez sur **Certificats de serveur**
4. Dans le panneau Actions (droite), cliquez sur **Créer un certificat auto-signé...**
5. Nom convivial : **FTP-Solaris-SSL**
6. Magasin de certificats : **Personnel**
7. Cliquez sur **OK**

**PowerShell :**

```powershell
# Créer un certificat auto-signé
$cert = New-SelfSignedCertificate `
    -DnsName "WIN2025TP.solaris.local" `
    -CertStoreLocation "Cert:\LocalMachine\My" `
    -FriendlyName "FTP-Solaris-SSL" `
    -NotAfter (Get-Date).AddYears(5)

# Afficher l'empreinte du certificat (thumbprint)
$cert.Thumbprint
```

### Activer FTPS sur le site FTP

**Interface graphique :**

1. Dans IIS Manager, sélectionnez le site **FTP-Solaris**
2. Double-cliquez sur **Paramètres SSL FTP**
3. Configuration :
   - **Liaison SSL** : Sélectionnez le certificat **FTP-Solaris-SSL**
   - **Connexions SSL** : Sélectionnez **Exiger des connexions SSL**
4. Cliquez sur **Appliquer**

**PowerShell :**

```powershell
# Récupérer l'empreinte du certificat
$certThumbprint = (Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.FriendlyName -eq "FTP-Solaris-SSL"}).Thumbprint

# Activer FTPS sur le site FTP
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.security.ssl.controlChannelPolicy -Value 1  # 1 = Require SSL
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.security.ssl.dataChannelPolicy -Value 1
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.security.ssl.serverCertHash -Value $certThumbprint

# Redémarrer le site
Stop-Website -Name "FTP-Solaris"
Start-Website -Name "FTP-Solaris"
```

### Connexion FTPS depuis FileZilla

1. Ouvrez FileZilla
2. Configuration :
   - **Hôte** : ftps://192.168.230.10
   - **Nom d'utilisateur** : mcurie
   - **Mot de passe** : [mot_de_passe]
   - **Port** : 21
3. Cliquez sur **Connexion rapide**
4. Une popup apparaît : **Certificat inconnu**
5. ☑ Cochez **Toujours faire confiance à ce certificat**
6. Cliquez sur **OK**
7. Connexion établie avec chiffrement SSL ! ✅

> ⚠️ **Certificat auto-signé :** En production, utilisez un certificat d'une **autorité de certification** reconnue (Let's Encrypt, DigiCert, etc.).

---

## 🔧 Dépannage

### Problème 1 : "Connexion refusée"

**Symptôme :** Le client ne peut pas se connecter au port 21

**Diagnostic :**

```powershell
# Sur le serveur : Vérifier que le service FTP est démarré
Get-Service -Name ftpsvc
# Si Stopped :
Start-Service -Name ftpsvc

# Vérifier que le site FTP est démarré
Get-Website -Name "FTP-Solaris"
# State : Started ✅

# Vérifier le pare-feu
Get-NetFirewallRule | Where-Object {$_.DisplayName -like "*FTP*" -and $_.Enabled -eq $true}

# Tester localement
Test-NetConnection -ComputerName localhost -Port 21
```

**Solutions :**
1. Démarrer le service FTP
2. Démarrer le site FTP dans IIS
3. Vérifier les règles de pare-feu

---

### Problème 2 : "Authentification échouée"

**Symptôme :** Le client entre les identifiants mais ça ne marche pas

**Diagnostic :**

```powershell
# Vérifier que l'utilisateur existe dans AD
Get-ADUser -Identity "mcurie"

# Vérifier le mot de passe (tester une connexion)
runas /user:solaris\mcurie cmd
# Si ça demande le mot de passe → Compte OK
# Si "mot de passe incorrect" → Réinitialiser

# Vérifier les autorisations FTP dans IIS
Get-WebConfiguration "/system.ftpServer/security/authorization" `
    -PSPath "IIS:\" `
    -Location "FTP-Solaris"
```

**Solutions :**
1. Réinitialiser le mot de passe de l'utilisateur
2. Vérifier que l'utilisateur est autorisé dans IIS
3. Vérifier les permissions NTFS sur C:\FTP

---

### Problème 3 : "Connexion établie mais liste impossible"

**Symptôme :** Le client se connecte mais ne peut pas lister les fichiers

**Cause :** Problème de mode passif (ports 5000-5020 bloqués)

**Diagnostic :**

```powershell
# Vérifier les ports passifs configurés
Get-WebConfigurationProperty -PSPath "IIS:\\" `
    -Filter "/system.ftpServer/firewallSupport" `
    -Name "lowDataChannelPort"

# Vérifier le pare-feu
Get-NetFirewallRule -DisplayName "FTP Passive Ports"
```

**Solutions :**
1. Configurer les ports passifs dans IIS
2. Ouvrir les ports 5000-5020 dans le pare-feu
3. Sur le client, forcer le mode passif (FileZilla : Édition → Paramètres → Connexion → FTP → Mode de transfert : Passif)

---

### Problème 4 : "Permission refusée"

**Symptôme :** L'utilisateur se connecte mais ne peut pas télécharger/uploader

**Diagnostic :**

```powershell
# Vérifier les permissions NTFS
Get-Acl "C:\FTP" | Select-Object -ExpandProperty Access

# Vérifier les autorisations FTP
Get-WebConfiguration "/system.ftpServer/security/authorization" `
    -PSPath "IIS:\" `
    -Location "FTP-Solaris"
```

**Solutions :**
1. Ajouter l'utilisateur aux permissions NTFS (Modify)
2. Ajouter l'utilisateur aux autorisations FTP (Read + Write)
3. Vérifier que l'utilisateur n'est pas dans le groupe "Invités"

---

### Tableau de dépannage rapide

| Symptôme | Couche OSI | Commande diagnostic | Solution |
|----------|-----------|---------------------|----------|
| Connexion refusée | 4 (Transport) | `Test-NetConnection -Port 21` | Pare-feu / Service arrêté |
| Authentification échouée | 7 (Application) | `Get-ADUser` | Mot de passe / Autorisations |
| Liste impossible | 4 (Transport) | Vérifier ports passifs | Ports 5000-5020 bloqués |
| Permission refusée | 7 (Application) | `Get-Acl` | Permissions NTFS / FTP |
| Transfert lent | 1-3 (Physique/Réseau) | `ping`, `tracert` | Problème réseau |

---

## 🎯 Exercices pratiques

### Exercice 1 : Installation complète

**Objectif :** Installer et configurer un serveur FTP de A à Z

**Consignes :**
1. Installez IIS avec le rôle FTP
2. Créez un dossier C:\FTP_Lab
3. Créez un site FTP nommé "FTP-Test"
4. Autorisez l'utilisateur "mcurie" avec lecture + écriture
5. Ouvrez les ports dans le pare-feu
6. Testez la connexion depuis un client

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# 1. Installation
Install-WindowsFeature -Name Web-Server,Web-Ftp-Server -IncludeManagementTools

# 2. Créer le dossier
New-Item -Path "C:\FTP_Lab" -ItemType Directory

# 3. Permissions NTFS
$acl = Get-Acl "C:\FTP_Lab"
$permission = "SOLARIS\mcurie","Modify","Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
$acl.SetAccessRule($accessRule)
Set-Acl "C:\FTP_Lab" $acl

# 4. Créer le site FTP
New-WebFtpSite -Name "FTP-Test" -Port 21 -PhysicalPath "C:\FTP_Lab"

# 5. Configurer l'authentification
Set-ItemProperty "IIS:\Sites\FTP-Test" `
    -Name ftpServer.security.authentication.basicAuthentication.enabled -Value $true
Set-ItemProperty "IIS:\Sites\FTP-Test" `
    -Name ftpServer.security.authentication.anonymousAuthentication.enabled -Value $false

# 6. Autoriser mcurie
Add-WebConfiguration "/system.ftpServer/security/authorization" `
    -Value @{accessType='Allow'; users='mcurie'; permissions='Read,Write'} `
    -PSPath "IIS:\" -Location "FTP-Test"

# 7. Pare-feu
New-NetFirewallRule -DisplayName "FTP Test" -Direction Inbound -Protocol TCP -LocalPort 21 -Action Allow

# 8. Démarrer
Start-Website -Name "FTP-Test"

# 9. Test
Test-NetConnection -ComputerName localhost -Port 21
```

</details>

---

### Exercice 2 : Isolation des utilisateurs

**Objectif :** Créer des répertoires personnels pour chaque utilisateur

**Consignes :**
1. Créez 3 utilisateurs AD : user1, user2, user3
2. Créez la structure C:\FTP\LocalUser\user1, user2, user3
3. Configurez l'isolation des utilisateurs dans IIS
4. Testez que chaque utilisateur ne voit que son dossier

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```powershell
# 1. Créer les utilisateurs AD
$users = @("user1", "user2", "user3")
foreach ($user in $users) {
    New-ADUser -Name $user -SamAccountName $user `
        -UserPrincipalName "$user@solaris.local" `
        -AccountPassword (ConvertTo-SecureString "P@ssw0rd123" -AsPlainText -Force) `
        -Enabled $true
}

# 2. Créer la structure de dossiers
New-Item -Path "C:\FTP\LocalUser" -ItemType Directory
foreach ($user in $users) {
    $userPath = "C:\FTP\LocalUser\$user"
    New-Item -Path $userPath -ItemType Directory
    
    # Permissions NTFS
    $acl = Get-Acl $userPath
    $permission = "SOLARIS\$user","FullControl","Allow"
    $accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule $permission
    $acl.SetAccessRule($accessRule)
    Set-Acl $userPath $acl
}

# 3. Configurer l'isolation (dans IIS Manager manuellement)
# Ou via PowerShell :
Set-ItemProperty "IIS:\Sites\FTP-Solaris" `
    -Name ftpServer.userIsolation.mode -Value 3  # 3 = Isolate users

# 4. Test : Connectez-vous avec user1 → Vous ne voyez que C:\FTP\LocalUser\user1
```

</details>

---

### Exercice 3 : FTPS avec certificat

**Objectif :** Sécuriser le serveur FTP avec SSL/TLS

**Consignes :**
1. Générez un certificat auto-signé
2. Configurez le site FTP pour utiliser FTPS
3. Testez la connexion sécurisée avec FileZilla

*Solution laissée en exercice (suivez les instructions de la section FTPS)*

---

## 📚 Ressources complémentaires

### Documentation officielle

- [Microsoft Docs - IIS FTP](https://docs.microsoft.com/en-us/iis/publish/using-the-ftp-service)
- [RFC 959 - FTP Protocol](https://tools.ietf.org/html/rfc959)
- [RFC 4217 - FTPS](https://tools.ietf.org/html/rfc4217)

### Outils recommandés

- **FileZilla Client** (gratuit) : [filezilla-project.org](https://filezilla-project.org/)
- **WinSCP** (alternatif) : [winscp.net](https://winscp.net/)
- **Cyberduck** (Mac/Windows) : [cyberduck.io](https://cyberduck.io/)

### Tutoriels vidéo

- [IIS FTP Setup - YouTube](https://www.youtube.com/results?search_query=iis+ftp+setup)
- [FTPS Configuration](https://www.youtube.com/results?search_query=ftps+configuration+windows)

---

## ✅ Checklist de révision

Avant de passer au cours suivant, vous devez maîtriser :

- [ ] Expliquer le rôle du FTP en entreprise
- [ ] Installer IIS avec le rôle FTP
- [ ] Créer un site FTP dans IIS Manager
- [ ] Configurer l'authentification avec AD
- [ ] Gérer les permissions NTFS et FTP
- [ ] Ouvrir les ports dans le pare-feu Windows
- [ ] Configurer les ports passifs FTP
- [ ] Se connecter depuis l'Explorateur Windows
- [ ] Utiliser FileZilla pour gérer les fichiers
- [ ] Sécuriser avec FTPS (certificat SSL)
- [ ] Diagnostiquer les problèmes FTP courants

---

<div align="center">

**Cours suivant :** [FSRM - Quotas et Filtrage](./fsrm-quotas.md)

**Cours précédent :** [GPO - Mappage de lecteurs](./gpo-mappage-lecteurs.md)

[⬅️ Retour au sommaire](../../README.md)

---

### 💾 "FTP : File Transfer Protocol - Simple, efficace, universel !"

*Bon courage pour la mise en place de votre serveur FTP !* 🚀

</div>
