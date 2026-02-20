🎯 TUTORIEL ECF BLANC - PARTIE 1 : PRATIQUE WINDOWS
📋 Préparation
Matériel nécessaire :

VMware Workstation (que tu as déjà)
ISO Windows 10/11 Pro
Environ 2h30

Structure du livrable :

Document PDF avec 6 à 10 screenshots
Nommage clair des captures
Annotations si nécessaire


✅ ÉTAPE 1 : Installation Windows 10/11 Pro
1.1 Créer la VM dans VMware
1. Ouvrir VMware Workstation
2. File → New Virtual Machine → Typical
3. Installer from → ISO (sélectionner ton ISO Windows)
4. Configuration :
   - RAM : 4 Go (4096 MB)
   - Processeurs : 2
   - Disque dur : 60 Go (Thin Provisioned)
   - Network Adapter : NAT ou Bridged
📸 Screenshot 1 : Configuration de la VM (résumé avant création)
1.2 Installation de Windows
1. Démarrer la VM
2. Suivre l'assistant d'installation
3. Choisir "Windows 10/11 Pro"
4. Installation personnalisée
5. Sélectionner le disque
6. Attendre l'installation (~15-20 min)
1.3 Configuration réseau en DHCP
powershell# Vérifier la configuration DHCP (par défaut)
ipconfig /all
```

**📸 Screenshot 2 : Résultat de `ipconfig /all` montrant l'IP DHCP**

### 1.4 Renommer la machine

**Méthode 1 - Interface graphique :**
```
1. Clic droit sur "Ce PC" → Propriétés
2. Renommer ce PC
3. Nom : EPCF-0622
4. Redémarrer
Méthode 2 - PowerShell (plus rapide) :
powershellRename-Computer -NewName "EPCF-0622" -Restart
```

**📸 Screenshot 3 : Propriétés système montrant le nouveau nom**

---

## ✅ ÉTAPE 2 : Gestion de disques

### 2.1 Ajouter un disque dur virtuel (80 Go)
```
1. Éteindre la VM
2. VM → Settings → Add → Hard Disk
3. SCSI (Recommended)
4. Create a new virtual disk
5. Taille : 80 Go
6. Store as single file
7. Nom : Backup.vmdk
8. Démarrer la VM
```

**📸 Screenshot 4 : Gestionnaire de disques montrant le nouveau disque (non alloué)**

### 2.2 Initialiser et formater le disque

**Ouvrir Disk Management :**
```
Win + X → Disk Management
```

**Initialiser le disque :**
```
1. Clic droit sur "Disk 1" (non initialisé)
2. Initialize Disk → GPT
3. OK
```

**Créer une partition :**
```
1. Clic droit sur l'espace non alloué
2. New Simple Volume
3. Next → Next (utiliser tout l'espace)
4. Assigner lettre : D:
5. Format :
   - File system : NTFS
   - Volume label : Backup
   - Quick format : coché
6. Finish
```

**📸 Screenshot 5 : Disk Management avec le disque D: "Backup" formaté en NTFS**

---

## ✅ ÉTAPE 3 : Sauvegarde/Restauration (Image Système)

### 3.1 Créer une image système
```
1. Panneau de configuration
2. System and Security → Backup and Restore (Windows 7)
3. Create a system image (à gauche)
4. Destination : On a hard disk → D: (Backup)
5. Sélectionner C: (System)
6. Start backup
7. Créer un disque de réparation (Skip si pas de lecteur CD)
8. Attendre la création (~10-15 min selon taille)
```

**📸 Screenshot 6 : Progression ou confirmation de la sauvegarde**

### 3.2 Supprimer le disque principal (simulation de panne)
```
1. Éteindre la VM
2. VM Settings → Hard Disk (C:) → Remove
3. OK
```

### 3.3 Ajouter un nouveau disque
```
1. VM Settings → Add → Hard Disk
2. SCSI
3. Create new virtual disk
4. Taille : 60 Go (même taille que l'original)
5. OK
```

### 3.4 Restaurer l'image système
```
1. Démarrer la VM
2. Elle ne bootera pas → appuyer sur une touche pour boot sur CD/DVD
3. Si pas de média de récupération :
   - Éteindre la VM
   - VM Settings → CD/DVD → Use ISO → Windows ISO
   - Démarrer
4. Choose language → Next
5. Repair your computer (en bas à gauche)
6. Troubleshoot → Advanced Options
7. System Image Recovery
8. Sélectionner l'image sur D:
9. Next → Finish → Yes
10. Attendre la restauration (~10-15 min)
11. Restart
```

**📸 Screenshot 7 : Windows démarré après restauration (montrer que le système fonctionne)**

---

## ✅ ÉTAPE 4 : Installation de logiciels

### 4.1 Installer Firefox/Chrome

**Méthode manuelle :**
```
1. Ouvrir Edge (navigateur par défaut)
2. Télécharger Firefox : https://www.mozilla.org/firefox/
3. Télécharger Chrome : https://www.google.com/chrome/
4. Installer les deux
Méthode PowerShell (winget) - plus pro :
powershell# Vérifier si winget est disponible
winget --version

# Installer Firefox
winget install Mozilla.Firefox

# Installer Chrome
winget install Google.Chrome
📸 Screenshot 8 : Menu Démarrer montrant Firefox et Chrome installés
4.2 Installer des applications métier (exemple)
powershell# Exemples d'applications métier courantes
winget install Adobe.Acrobat.Reader.64-bit
winget install 7zip.7zip
winget install VideoLAN.VLC
```

---

## ✅ ÉTAPE 5 : Configuration réseau

### 5.1 Configurer IP statique

**Interface graphique :**
```
1. Win + X → Network Connections
2. Clic droit sur Ethernet0 → Properties
3. Internet Protocol Version 4 (TCP/IPv4) → Properties
4. Use the following IP address :
   - IP : 192.168.1.100
   - Subnet : 255.255.255.0
   - Gateway : 192.168.1.1
   - DNS : 192.168.1.1 (ou 8.8.8.8)
5. OK → OK
PowerShell (méthode rapide) :
powershell# Trouver le nom de l'interface
Get-NetAdapter

# Configurer IP statique
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1

# Configurer DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses "192.168.1.1"
📸 Screenshot 9 : ipconfig /all montrant l'IP statique configurée
5.2 Revenir en DHCP (si besoin)
powershellSet-NetIPInterface -InterfaceAlias "Ethernet0" -Dhcp Enabled
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ResetServerAddresses
5.3 Tester la connectivité
powershell# Test de connectivité locale
ping 192.168.1.1

# Test Internet
ping 8.8.8.8

# Test DNS
ping google.com

# Diagnostic complet
ipconfig /all
nslookup google.com
tracert 8.8.8.8
```

**📸 Screenshot 10 : Résultats des tests de connectivité (ping et ipconfig)**

---

## 📄 STRUCTURE DU PDF À RENDRE
```
📑 ECF Blanc - Partie Pratique Windows
   Candidat : [Ton nom]
   Date : [Date de l'examen]

1. Installation Windows 10/11 Pro
   [Screenshot 1 : Configuration VM]
   [Screenshot 2 : ipconfig DHCP]
   [Screenshot 3 : Nom de la machine]

2. Gestion de disques
   [Screenshot 4 : Nouveau disque non alloué]
   [Screenshot 5 : Disque Backup formaté]

3. Sauvegarde/Restauration
   [Screenshot 6 : Image système créée]
   [Screenshot 7 : Système restauré fonctionnel]

4. Installation de logiciels
   [Screenshot 8 : Firefox et Chrome installés]

5. Configuration réseau
   [Screenshot 9 : IP statique configurée]
   [Screenshot 10 : Tests de connectivité]

⏱️ TIMING RECOMMANDÉ

Installation Windows : 30 min
Gestion disques : 15 min
Sauvegarde/Restauration : 45 min
Installation logiciels : 15 min
Configuration réseau : 15 min
Screenshots et PDF : 30 min

Total : ~2h30

💡 CONSEILS POUR L'ECF

Prends des screenshots au fur et à mesure (ne pas attendre la fin)
Nomme bien tes fichiers : screenshot_01_config_vm.png
Annote si nécessaire (flèches, encadrés rouges)
Vérifie que tous les screenshots sont lisibles
Sauvegarde régulièrement ta VM (snapshots VMware)