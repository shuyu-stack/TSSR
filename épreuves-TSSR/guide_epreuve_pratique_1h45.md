# Guide de l'Épreuve Pratique (1h45)

## 📋 Informations générales

**Durée** : 1h45  
**Type** : Épreuve pratique sur machines virtuelles  
**Accès Internet** : ✅ **OUI, vous avez accès à Internet !**

---

## ⚠️ CONSIGNES CRITIQUES À RESPECTER

### 🔴 Règles d'or

1. **Si vous ne savez pas répondre à une question, PASSEZ À LA SUIVANTE !**
   - Ne perdez pas de temps sur une question bloquante
   - Revenez-y plus tard si vous avez le temps

2. **Captures d'écran obligatoires**
   - Si vous n'avez pas le temps de faire les captures
   - **Expliquez dans la case prévue ce que vous saviez faire**
   - C'est mieux que de laisser vide !

3. **Gestion du temps**
   - 1h45 = 105 minutes pour 3 parties + questions
   - Ne passez pas 30 minutes sur une seule question
   - Visez 30-35 min par partie technique

---

## 🖥️ Configuration de l'environnement

### Matériel fourni

**Tous dans la même salle**

**Toutes les machines sont installées et prêtes :**
- **VM Active Directory** (ADx1) - Windows Server
- **VM Téléphonie** (telephonex1) - IPBX (probablement Asterisk ou 3CX)
- **VM Linux** (Linux1) - Serveur Linux
- **VM Client** (clientx1) - Poste de travail Windows

### Accès aux VM

Les VM sont déjà démarrées et accessibles. Tu devras probablement :
- Te connecter avec des identifiants fournis
- Utiliser un client de virtualisation (VMware, VirtualBox, Hyper-V)
- Éventuellement SSH pour Linux

---

## 📝 Structure de l'épreuve

### 2 questions distinctes

1. **Une question sur papier** 📄
2. **Une question virtuelle** (sur les VM) 💻

### 3 parties techniques

1. 🪟 **Windows** (Active Directory / Scripts PowerShell)
2. 🐧 **Linux** (Commandes de base / Manipulation fichiers / SSH)
3. 🌐 **Réseaux / Téléphonie** (Schémas / IPBX / Configuration)

---

## 🪟 PARTIE 1 : WINDOWS (Active Directory & PowerShell)

### Types de questions attendues

#### 1. Active Directory (ADDS)

**Création d'utilisateur**
```powershell
# Méthode GUI
Active Directory Users and Computers
→ Clic droit sur l'OU
→ New → User
→ Remplir les informations

# Méthode PowerShell
New-ADUser -Name "Jean Dupont" `
    -GivenName "Jean" `
    -Surname "Dupont" `
    -SamAccountName "jdupont" `
    -UserPrincipalName "jdupont@domain.local" `
    -Path "OU=Utilisateurs,DC=domain,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
    -Enabled $true
```

**Création d'Unité Organisationnelle (OU)**
```powershell
# GUI
Active Directory Users and Computers
→ Clic droit sur le domaine
→ New → Organizational Unit

# PowerShell
New-ADOrganizationalUnit -Name "Service_IT" -Path "DC=domain,DC=local"
```

**Création de groupe**
```powershell
# GUI
Active Directory Users and Computers
→ Clic droit sur l'OU
→ New → Group

# PowerShell
New-ADGroup -Name "Groupe_Admins" `
    -GroupScope Global `
    -GroupCategory Security `
    -Path "OU=Groupes,DC=domain,DC=local"

# Ajouter un utilisateur au groupe
Add-ADGroupMember -Identity "Groupe_Admins" -Members "jdupont"
```

**Modification d'utilisateur**
```powershell
# Changer le mot de passe
Set-ADAccountPassword -Identity jdupont -Reset -NewPassword (ConvertTo-SecureString "NouveauP@ss!" -AsPlainText -Force)

# Forcer le changement au prochain login
Set-ADUser -Identity jdupont -ChangePasswordAtLogon $true

# Désactiver un compte
Disable-ADAccount -Identity jdupont

# Activer un compte
Enable-ADAccount -Identity jdupont

# Modifier les propriétés
Set-ADUser -Identity jdupont -Title "Administrateur Système" -Department "IT" -Office "Paris"
```

**Recherche et listing**
```powershell
# Lister tous les utilisateurs
Get-ADUser -Filter *

# Lister les utilisateurs d'une OU
Get-ADUser -Filter * -SearchBase "OU=Service_IT,DC=domain,DC=local"

# Rechercher un utilisateur spécifique
Get-ADUser -Filter {Name -like "*Dupont*"}

# Lister les membres d'un groupe
Get-ADGroupMember -Identity "Groupe_Admins"

# Lister les groupes d'un utilisateur
Get-ADPrincipalGroupMembership -Identity jdupont
```

#### 2. Scripts PowerShell

**Création en masse d'utilisateurs (depuis CSV)**
```powershell
# Fichier CSV : users.csv
# prenom,nom,login,motdepasse,ou
# Jean,Dupont,jdupont,P@ss123,OU=IT,DC=domain,DC=local
# Marie,Martin,mmartin,P@ss456,OU=RH,DC=domain,DC=local

# Script
Import-Csv -Path "C:\users.csv" | ForEach-Object {
    New-ADUser -Name "$($_.prenom) $($_.nom)" `
        -GivenName $_.prenom `
        -Surname $_.nom `
        -SamAccountName $_.login `
        -UserPrincipalName "$($_.login)@domain.local" `
        -Path $_.ou `
        -AccountPassword (ConvertTo-SecureString $_.motdepasse -AsPlainText -Force) `
        -Enabled $true
    
    Write-Host "Utilisateur $($_.login) créé avec succès" -ForegroundColor Green
}
```

**Script de gestion des groupes**
```powershell
# Ajouter plusieurs utilisateurs à un groupe
$users = @("jdupont", "mmartin", "pdurand")
$groupe = "Groupe_Projet"

foreach ($user in $users) {
    Add-ADGroupMember -Identity $groupe -Members $user
    Write-Host "$user ajouté au groupe $groupe"
}
```

**Script de nettoyage / désactivation**
```powershell
# Désactiver les comptes inactifs depuis 90 jours
$date = (Get-Date).AddDays(-90)
$inactifs = Get-ADUser -Filter {LastLogonDate -lt $date -and Enabled -eq $true} -Properties LastLogonDate

foreach ($user in $inactifs) {
    Disable-ADAccount -Identity $user.SamAccountName
    Write-Host "Compte $($user.SamAccountName) désactivé (inactif depuis $($user.LastLogonDate))"
}
```

**Script de rapport**
```powershell
# Générer un rapport des utilisateurs
Get-ADUser -Filter * -Properties * | 
    Select-Object Name, SamAccountName, EmailAddress, Department, Enabled, LastLogonDate |
    Export-Csv -Path "C:\rapport_users.csv" -NoTypeInformation

Write-Host "Rapport généré : C:\rapport_users.csv"
```

#### 3. Commandes PowerShell de base essentielles

```powershell
# Navigation
Get-Location  # pwd
Set-Location C:\Users  # cd
Get-ChildItem  # ls / dir

# Gestion de fichiers
Copy-Item source.txt destination.txt
Move-Item fichier.txt C:\Temp\
Remove-Item fichier.txt
New-Item -ItemType Directory -Path "C:\MonDossier"

# Services Windows
Get-Service
Start-Service -Name "spooler"
Stop-Service -Name "spooler"
Restart-Service -Name "spooler"

# Processus
Get-Process
Stop-Process -Name "notepad"
Get-Process | Where-Object {$_.CPU -gt 100}

# Réseau
Test-Connection google.com  # ping
Get-NetIPAddress
Get-NetAdapter
Test-NetConnection -ComputerName serveur.local -Port 80

# Aide
Get-Help Get-ADUser
Get-Help Get-ADUser -Examples
Get-Command *AD*
```

### Méthodologie pour la partie Windows

**Checklist rapide :**
```
☐ Vérifier l'accès à la VM AD
☐ Ouvrir PowerShell en tant qu'administrateur
☐ Tester les commandes AD (Get-ADUser, Get-ADDomain)
☐ Lire attentivement la question
☐ Faire une capture AVANT et APRÈS chaque action
☐ Vérifier que la commande/action a bien fonctionné
☐ Noter les commandes utilisées dans la case réponse
```

**Captures d'écran importantes :**
1. État initial (avant modification)
2. Commande exécutée ou interface utilisée
3. État final (après modification)
4. Vérification (Get-ADUser, propriétés, etc.)

---

## 🐧 PARTIE 2 : LINUX

### Types de questions attendues

#### 1. Déplacement et navigation

**Commandes essentielles**
```bash
# Afficher le répertoire courant
pwd

# Lister les fichiers
ls
ls -l      # Liste détaillée
ls -la     # Liste avec fichiers cachés
ls -lh     # Tailles lisibles (human-readable)
ls -ltr    # Tri par date de modification

# Changer de répertoire
cd /home/user
cd ..      # Répertoire parent
cd ~       # Répertoire personnel
cd -       # Répertoire précédent

# Créer un répertoire
mkdir mon_dossier
mkdir -p dossier1/dossier2/dossier3  # Créer parents

# Afficher le contenu d'un fichier
cat fichier.txt
less fichier.txt  # Avec navigation
head fichier.txt  # 10 premières lignes
tail fichier.txt  # 10 dernières lignes
tail -f /var/log/syslog  # Suivi en temps réel
```

#### 2. Copie et manipulation de fichiers

**Commandes de copie**
```bash
# Copier un fichier
cp source.txt destination.txt

# Copier un répertoire
cp -r dossier_source/ dossier_destination/

# Copier avec préservation des attributs
cp -p fichier.txt copie.txt

# Copier plusieurs fichiers
cp fichier1.txt fichier2.txt fichier3.txt /destination/

# Copier avec confirmation
cp -i source.txt destination.txt  # Demande confirmation si existe
```

**Commandes de déplacement/renommage**
```bash
# Déplacer un fichier
mv fichier.txt /nouveau/chemin/

# Renommer un fichier
mv ancien_nom.txt nouveau_nom.txt

# Déplacer plusieurs fichiers
mv fichier1.txt fichier2.txt /destination/
```

**Commandes de suppression**
```bash
# Supprimer un fichier
rm fichier.txt

# Supprimer avec confirmation
rm -i fichier.txt

# Supprimer un répertoire
rm -r dossier/

# Supprimer de force (ATTENTION !)
rm -rf dossier/

# Supprimer les fichiers d'un type
rm *.txt
```

**Création de fichiers**
```bash
# Créer un fichier vide
touch nouveau_fichier.txt

# Créer un fichier avec contenu
echo "Contenu" > fichier.txt

# Ajouter du contenu
echo "Nouvelle ligne" >> fichier.txt

# Créer avec éditeur
nano fichier.txt
vi fichier.txt
```

#### 3. Permissions et propriétaires

**Gestion des permissions**
```bash
# Afficher les permissions
ls -l fichier.txt
# Résultat : -rw-r--r-- 1 user group 1234 Jan 15 10:00 fichier.txt
#            ↑ type et permissions

# Changer les permissions (symbolique)
chmod u+x script.sh      # Ajouter exécution pour user
chmod g-w fichier.txt    # Retirer écriture pour group
chmod o+r fichier.txt    # Ajouter lecture pour others
chmod a+x script.sh      # Ajouter exécution pour all

# Changer les permissions (numérique)
chmod 755 script.sh      # rwxr-xr-x
chmod 644 fichier.txt    # rw-r--r--
chmod 777 fichier.txt    # rwxrwxrwx (DANGEREUX !)

# Permissions récursives
chmod -R 755 /var/www/html/
```

**Comprendre les permissions numériques**
```
r (read)    = 4
w (write)   = 2
x (execute) = 1

755 = rwxr-xr-x
      ↓   ↓   ↓
      7   5   5
    user group others

7 = 4+2+1 = rwx (lecture+écriture+exécution)
5 = 4+0+1 = r-x (lecture+exécution)
4 = 4+0+0 = r-- (lecture seulement)
```

**Gestion des propriétaires**
```bash
# Changer le propriétaire
sudo chown user fichier.txt

# Changer le groupe
sudo chown :group fichier.txt

# Changer propriétaire et groupe
sudo chown user:group fichier.txt

# Récursif
sudo chown -R www-data:www-data /var/www/html/
```

#### 4. SSH et connexions distantes

**Connexion SSH**
```bash
# Connexion simple
ssh user@192.168.1.100
ssh user@serveur.local

# Connexion avec port spécifique
ssh -p 2222 user@serveur.local

# Connexion avec clé privée
ssh -i ~/.ssh/ma_cle_privee user@serveur.local

# Exécuter une commande à distance
ssh user@serveur.local "ls -la /var/log"

# Copier des fichiers via SSH (SCP)
scp fichier.txt user@serveur.local:/home/user/
scp user@serveur.local:/home/user/fichier.txt ./

# Copier un répertoire
scp -r dossier/ user@serveur.local:/home/user/

# Alternative : SFTP
sftp user@serveur.local
sftp> put fichier.txt
sftp> get fichier_distant.txt
sftp> exit
```

**Gestion des clés SSH**
```bash
# Générer une paire de clés
ssh-keygen -t rsa -b 4096
# Fichiers créés :
# ~/.ssh/id_rsa (clé privée - GARDER SECRÈTE)
# ~/.ssh/id_rsa.pub (clé publique - à partager)

# Copier la clé publique sur un serveur
ssh-copy-id user@serveur.local

# Vérifier les connexions
cat ~/.ssh/known_hosts
```

#### 5. Gestion des processus et services

**Processus**
```bash
# Lister les processus
ps aux
ps aux | grep apache

# Processus en temps réel
top
htop  # Plus convivial (si installé)

# Tuer un processus
kill PID
kill -9 PID  # Force kill

# Processus par nom
pkill apache2
killall apache2
```

**Services (systemd)**
```bash
# Statut d'un service
sudo systemctl status apache2

# Démarrer un service
sudo systemctl start apache2

# Arrêter un service
sudo systemctl stop apache2

# Redémarrer un service
sudo systemctl restart apache2

# Recharger la configuration sans redémarrage
sudo systemctl reload apache2

# Activer au démarrage
sudo systemctl enable apache2

# Désactiver au démarrage
sudo systemctl disable apache2

# Lister tous les services
systemctl list-units --type=service
```

#### 6. Réseau et diagnostic

**Configuration réseau**
```bash
# Afficher les interfaces
ip addr show
ip a  # Version courte

# Ancienne commande (si disponible)
ifconfig

# Afficher la table de routage
ip route show
route -n

# Configurer une IP (temporaire)
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up

# Tester la connectivité
ping google.com
ping -c 4 192.168.1.1  # 4 paquets seulement

# Tracer la route
traceroute google.com

# Ports ouverts
sudo netstat -tulpn
sudo ss -tulpn  # Version moderne

# Résolution DNS
nslookup google.com
dig google.com
host google.com
```

#### 7. Recherche de fichiers

**Commande find**
```bash
# Rechercher par nom
find /home -name "*.txt"

# Rechercher par type
find /var -type f  # Fichiers
find /var -type d  # Répertoires

# Rechercher par taille
find /home -size +100M  # Plus de 100 Mo

# Rechercher par date de modification
find /var/log -mtime -7  # Modifiés dans les 7 derniers jours

# Exécuter une commande sur les résultats
find /tmp -name "*.tmp" -delete
find /home -name "*.log" -exec ls -lh {} \;
```

**Commande locate**
```bash
# Mise à jour de la base de données
sudo updatedb

# Recherche rapide
locate fichier.txt
locate "*.conf" | grep apache
```

**Commande grep (recherche dans fichiers)**
```bash
# Rechercher dans un fichier
grep "erreur" /var/log/syslog

# Recherche récursive
grep -r "TODO" /home/user/projet/

# Ignorer la casse
grep -i "error" fichier.log

# Compter les occurrences
grep -c "ERROR" fichier.log

# Afficher les numéros de ligne
grep -n "function" script.js
```

#### 8. Archives et compression

**tar**
```bash
# Créer une archive
tar -cvf archive.tar fichiers/

# Créer une archive compressée (gzip)
tar -czvf archive.tar.gz fichiers/

# Créer une archive compressée (bzip2)
tar -cjvf archive.tar.bz2 fichiers/

# Extraire une archive
tar -xvf archive.tar
tar -xzvf archive.tar.gz

# Lister le contenu sans extraire
tar -tvf archive.tar.gz
```

**zip/unzip**
```bash
# Créer un zip
zip archive.zip fichier1.txt fichier2.txt
zip -r archive.zip dossier/

# Extraire un zip
unzip archive.zip
unzip archive.zip -d /destination/

# Lister le contenu
unzip -l archive.zip
```

### Méthodologie pour la partie Linux

**Checklist rapide :**
```
☐ Vérifier l'accès à la VM Linux
☐ Ouvrir un terminal
☐ Tester les commandes de base (pwd, ls, whoami)
☐ Lire attentivement la question
☐ Faire des captures d'écran AVANT et APRÈS
☐ Utiliser ls -l pour vérifier les permissions/propriétaires
☐ Tester la connectivité si SSH demandé
☐ Noter les commandes exactes utilisées
```

**Script de vérification rapide**
```bash
#!/bin/bash
# Tester que tout fonctionne

echo "=== Informations système ==="
hostname
whoami
pwd

echo -e "\n=== Réseau ==="
ip addr show | grep inet
ping -c 2 8.8.8.8

echo -e "\n=== Services ==="
systemctl list-units --type=service --state=running | head

echo -e "\n=== Disque ==="
df -h

echo "Système opérationnel ✓"
```

---

## 🌐 PARTIE 3 : RÉSEAUX & TÉLÉPHONIE

### A. Concepts réseaux globaux

**Questions possibles :**
- Schéma réseau à analyser ou compléter
- Calcul de sous-réseaux (subnetting)
- Configuration de VLAN
- Routage statique/dynamique

#### Analyse de schéma réseau

**Ce qu'on attend :**
1. Identifier les équipements (routeurs, switchs, firewalls)
2. Comprendre la segmentation (VLANs, sous-réseaux)
3. Identifier les flux de données
4. Repérer les points de sécurité

**Exemple de réponse structurée :**
```
Schéma analysé :
- 1 routeur (passerelle vers Internet)
- 2 switchs de distribution (redondance)
- 3 VLANs :
  * VLAN 10 : Serveurs (192.168.10.0/24)
  * VLAN 20 : Utilisateurs (192.168.20.0/24)
  * VLAN 30 : Invités (192.168.30.0/24)
- 1 pare-feu entre le routeur et les switchs
- Topologie en étoile avec redondance
```

#### Calcul de sous-réseaux rapide

**Exemple de question :**
"Diviser 192.168.10.0/24 en 4 sous-réseaux égaux"

**Réponse :**
```
Réseau de base : 192.168.10.0/24 = 256 adresses
4 sous-réseaux → /26 (64 adresses chacun)

Sous-réseau 1 : 192.168.10.0/26
  - Première IP : 192.168.10.1
  - Dernière IP : 192.168.10.62
  - Broadcast : 192.168.10.63

Sous-réseau 2 : 192.168.10.64/26
  - Première IP : 192.168.10.65
  - Dernière IP : 192.168.10.126
  - Broadcast : 192.168.10.127

Sous-réseau 3 : 192.168.10.128/26
  - Première IP : 192.168.10.129
  - Dernière IP : 192.168.10.190
  - Broadcast : 192.168.10.191

Sous-réseau 4 : 192.168.10.192/26
  - Première IP : 192.168.10.193
  - Dernière IP : 192.168.10.254
  - Broadcast : 192.168.10.255
```

**Tableau de référence rapide :**
```
/24 = 256 adresses (254 utilisables)
/25 = 128 adresses (126 utilisables)
/26 = 64 adresses (62 utilisables)
/27 = 32 adresses (30 utilisables)
/28 = 16 adresses (14 utilisables)
/29 = 8 adresses (6 utilisables)
/30 = 4 adresses (2 utilisables) - Liens point-à-point
```

### B. Téléphonie IP (IPBX)

**Types de questions :**
1. Création d'utilisateurs sur l'IPBX
2. Configuration de renvoi d'appels
3. Configuration de numéros internes

#### 1. Création d'utilisateurs sur IPBX

**Interfaces possibles :**
- **Asterisk** (FreePBX) - Interface web
- **3CX** - Interface graphique
- **Cisco UCM** - Interface CUCM

**Exemple avec FreePBX (Asterisk) :**

**Via interface web :**
```
1. Se connecter à l'interface FreePBX
   http://192.168.1.100/admin

2. Aller dans : Applications → Extensions

3. Cliquer sur "Add Extension" → Add New SIP Extension

4. Configurer l'extension :
   - Extension Number: 1001 (numéro interne)
   - Display Name: Jean Dupont
   - Outbound CID: Jean Dupont <1001>
   - Secret (mot de passe): P@ssw0rd123!
   - Device Options:
     * NAT: Yes
     * Qualify: Yes
     * DTMF Mode: RFC2833

5. Submit et Apply Config
```

**Informations à noter pour configuration téléphone :**
```
Serveur SIP : 192.168.1.100
Compte : 1001
Mot de passe : P@ssw0rd123!
Port : 5060 (SIP standard)
```

**Configuration sur un téléphone IP (softphone) :**
```
1. Ouvrir le softphone (ex: Zoiper, MicroSIP)

2. Ajouter un compte :
   - Username: 1001
   - Domain/Proxy: 192.168.1.100
   - Password: P@ssw0rd123!
   - Transport: UDP
   - Port: 5060

3. Sauvegarder et vérifier l'enregistrement
   → Status: Registered (vert)
```

#### 2. Configuration de renvoi d'appels

**Scénario typique :**
"Renvoyer les appels de l'utilisateur 1001 vers l'utilisateur 1002"

**Via FreePBX :**

**Méthode 1 : Follow Me**
```
1. Applications → Extensions → 1001

2. Aller dans l'onglet "Follow Me"

3. Activer Follow Me :
   ☑ Enable Follow Me

4. Follow Me List :
   1001#      (sonne d'abord le poste principal)
   1002#      (puis renvoie vers 1002)

5. Ring Strategy : ringall (sonne tous en même temps)
   ou ringallv2 (sonne dans l'ordre)

6. Ring Time: 20 secondes

7. Submit et Apply Config
```

**Méthode 2 : Call Forward**
```
1. Applications → Extensions → 1001

2. Dans les options de l'extension :
   - Call Forward Not Reachable: 1002
   - Call Forward Busy: 1002
   - Call Forward No Answer: 1002
   - Call Forward Unavailable: 1002

3. Submit et Apply Config
```

**Méthode 3 : Via code téléphone (feature codes)**
```
Depuis le téléphone 1001 :

*72 1002   → Active le renvoi vers 1002
*73        → Désactive le renvoi

Vérifier : appeler 1001 → doit sonner sur 1002
```

**Types de renvoi d'appel :**
```
- Renvoi immédiat : Tous les appels → autre poste
- Renvoi sur non-réponse : Après X secondes → autre poste
- Renvoi sur occupé : Si en communication → autre poste
- Renvoi sur injoignable : Si téléphone éteint → autre poste
```

#### 3. Configuration avancée IPBX

**Groupe d'appels (Ring Group)**
```
1. Applications → Ring Groups

2. Add Ring Group :
   - Group Number: 2000
   - Group Name: Support
   - Ring Strategy: ringall (tous sonnent)
   - Extension List: 1001, 1002, 1003
   - Ring Time: 30 secondes
   - Destination if no answer: Voicemail 2000

3. Submit et Apply Config

Résultat : Appeler 2000 → sonne sur 1001, 1002 et 1003
```

**File d'attente (Queue)**
```
1. Applications → Queues

2. Add Queue :
   - Queue Number: 3000
   - Queue Name: Service Client
   - Static Agents: 1001, 1002
   - Max Wait Time: 300 secondes
   - Join Announcement: Bienvenue, merci de patienter

3. Submit et Apply Config
```

**Boîte vocale**
```
1. Applications → Voicemail

2. Add Voicemail :
   - Mailbox: 1001
   - Name: Jean Dupont
   - Email: jdupont@domain.com
   - Pager Email: (optionnel)
   - Email Attachment: Yes (envoyer le fichier audio par email)

3. Dans l'extension 1001, associer la voicemail 1001

4. *97 depuis le téléphone pour consulter la boîte vocale
```

### Captures d'écran importantes pour IPBX

**À faire absolument :**
1. **Liste des extensions créées** (capture de la page Extensions)
2. **Configuration de l'extension** (détails de 1001)
3. **Configuration Follow Me** ou Call Forward
4. **Téléphone enregistré** (status "Registered")
5. **Test d'appel** (capture pendant un appel 1001 → 1002)
6. **Renvoi actif** (capture montrant que l'appel sonne sur le bon poste)

### Tests de validation

**Checklist de tests :**
```
☐ Extension 1001 créée → vérifier dans la liste
☐ Téléphone enregistré → status "Registered"
☐ Appel interne 1001 → 1002 → fonctionne
☐ Appel interne 1002 → 1001 → fonctionne
☐ Renvoi configuré → tester en appelant 1001
☐ Vérifier que ça sonne bien sur 1002
☐ Consulter les logs (Reports → Asterisk Logfiles)
```

---

## ⏱️ GESTION DU TEMPS (1h45 = 105 minutes)

### Répartition suggérée

```
00:00 - 00:05 : Lecture de toutes les questions (5 min)
                Identifier les plus faciles

00:05 - 00:35 : Partie Windows (30 min)
                AD + Scripts PowerShell

00:35 - 01:05 : Partie Linux (30 min)
                Commandes + SSH

01:05 - 01:35 : Partie Réseaux/Téléphonie (30 min)
                Schémas + IPBX

01:35 - 01:45 : Relecture et questions laissées (10 min)
                Vérifications finales
```

### Stratégie de priorisation

**Questions FACILES (faire en premier) :**
- Création utilisateur AD (GUI)
- Commandes Linux basiques (ls, cd, cp)
- Créer une extension IPBX

**Questions MOYENNES :**
- Script PowerShell simple
- Configuration SSH
- Renvoi d'appels

**Questions DIFFICILES (faire en dernier) :**
- Script PowerShell complexe
- Schéma réseau avec calculs
- Configuration avancée IPBX

---

## 📸 STRATÉGIE DE CAPTURES D'ÉCRAN

### Principe

**Chaque manipulation = 3 captures minimum :**
1. **AVANT** : État initial
2. **PENDANT** : Action/commande en cours
3. **APRÈS** : Résultat final

### Outils de capture

**Windows :**
- `Win + Shift + S` : Outil Capture d'écran
- `Snipping Tool` : Outil de capture
- `Win + Print Screen` : Capture complète

**Linux :**
- `Scrot` : Capture d'écran
- `GNOME Screenshot` : Interface graphique
- `Spectacle` (KDE)

### Nommage des fichiers

**Convention recommandée :**
```
01_windows_avant_creation_user.png
02_windows_commande_creation_user.png
03_windows_apres_verification_user.png

04_linux_avant_copie_fichiers.png
05_linux_commande_cp.png
06_linux_apres_verification_ls.png

07_ipbx_liste_extensions_avant.png
08_ipbx_creation_extension_1001.png
09_ipbx_liste_extensions_apres.png
10_ipbx_telephone_registered.png
```

### Si pas le temps pour les captures

**Dans la case prévue, écrire clairement :**

```
EXPLICATION (capture non fournie par manque de temps) :

Question : Créer l'utilisateur Jean Dupont (jdupont) dans l'OU Service_IT

Commande PowerShell utilisée :
New-ADUser -Name "Jean Dupont" -GivenName "Jean" -Surname "Dupont" `
    -SamAccountName "jdupont" -UserPrincipalName "jdupont@domain.local" `
    -Path "OU=Service_IT,DC=domain,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
    -Enabled $true

Vérification :
Get-ADUser -Identity jdupont

Résultat attendu :
L'utilisateur jdupont est créé dans l'OU Service_IT,
activé, avec le mot de passe configuré.

Cette manipulation a été réalisée avec succès,
mais je n'ai pas eu le temps de faire la capture d'écran.
```

**C'est mieux que rien ! Le jury verra que tu sais faire.**

---

## 🎯 CHECKLIST FINALE AVANT L'ÉPREUVE

### Préparation technique

```
☐ Réviser les commandes PowerShell AD (New-ADUser, Get-ADUser, Set-ADUser)
☐ Réviser les commandes Linux essentielles (cp, mv, rm, chmod, chown)
☐ Réviser SSH (connexion, clés, scp)
☐ Comprendre le subnetting (/24, /26, /27, /28)
☐ Réviser la création d'extensions IPBX
☐ Comprendre le renvoi d'appels
☐ Tester sur des VM à la maison si possible
```

### État d'esprit

```
☐ Lire TOUTES les questions avant de commencer
☐ Commencer par les questions faciles
☐ Ne pas perdre de temps sur une question bloquante
☐ Si pas de capture, EXPLIQUER dans la case
☐ Utiliser Internet si besoin (documentation officielle)
☐ Garder du temps pour relire (10 min)
```

### Pendant l'épreuve

```
☐ Respirer calmement
☐ Lire attentivement chaque question
☐ Faire une capture AVANT/PENDANT/APRÈS
☐ Vérifier que chaque action a fonctionné
☐ Noter les commandes utilisées
☐ Passer à la suivante si bloqué
☐ Revenir sur les questions laissées à la fin
```

---

## 💡 ASTUCES IMPORTANTES

### Utiliser Internet efficacement

**Sites autorisés et utiles :**
- Documentation officielle Microsoft PowerShell
- Man pages Linux en ligne
- Documentation Asterisk/FreePBX
- Calculateur de sous-réseaux

**Recherches efficaces :**
```
"powershell create ad user"
"linux chmod command examples"
"freepbx call forwarding"
"subnet calculator /24 to /26"
```

### Commandes rapides de vérification

**Windows (PowerShell) :**
```powershell
# Vérifier le domaine AD
Get-ADDomain

# Lister les OUs
Get-ADOrganizationalUnit -Filter *

# Tester la connexion
Test-Connection DC01

# Vérifier un utilisateur
Get-ADUser jdupont -Properties *
```

**Linux :**
```bash
# Vérifier qu'on est bien connecté
whoami && hostname

# Tester les commandes de base
ls -la && pwd

# Vérifier SSH
systemctl status sshd

# Tester la connectivité
ping -c 2 8.8.8.8
```

**IPBX :**
```bash
# Se connecter en SSH au serveur IPBX
ssh admin@192.168.1.100

# Vérifier les extensions enregistrées
asterisk -rx "sip show peers"

# Vérifier les appels en cours
asterisk -rx "core show channels"
```

---

## 📝 MODÈLES DE RÉPONSES

### Modèle pour partie Windows

```
=== QUESTION : Créer l'utilisateur Jean Dupont ===

AVANT :
[Capture : Liste des utilisateurs avant création]

COMMANDE :
New-ADUser -Name "Jean Dupont" -GivenName "Jean" -Surname "Dupont" `
    -SamAccountName "jdupont" -UserPrincipalName "jdupont@domain.local" `
    -Path "OU=Service_IT,DC=domain,DC=local" `
    -AccountPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
    -Enabled $true

[Capture : Exécution de la commande PowerShell]

VÉRIFICATION :
Get-ADUser -Identity jdupont -Properties *

APRÈS :
[Capture : Propriétés de l'utilisateur créé]
[Capture : Liste des utilisateurs après création]

RÉSULTAT : ✓ Utilisateur jdupont créé avec succès dans l'OU Service_IT
```

### Modèle pour partie Linux

```
=== QUESTION : Copier tous les fichiers .txt vers /backup ===

AVANT :
[Capture : ls -la du répertoire source]

COMMANDE :
cp *.txt /backup/

[Capture : Exécution de la commande]

VÉRIFICATION :
ls -la /backup/

APRÈS :
[Capture : Contenu du répertoire /backup]

RÉSULTAT : ✓ Tous les fichiers .txt ont été copiés vers /backup
```

### Modèle pour partie IPBX

```
=== QUESTION : Créer l'extension 1001 (Jean Dupont) ===

AVANT :
[Capture : Liste des extensions avant création]

CRÉATION :
Applications → Extensions → Add New SIP Extension

Configuration :
- Extension: 1001
- Display Name: Jean Dupont
- Secret: P@ssw0rd123!

[Capture : Formulaire de création rempli]

APRÈS :
[Capture : Extension 1001 dans la liste]
[Capture : Téléphone enregistré (status: Registered)]

TEST :
[Capture : Appel de 1001 vers 1002 réussi]

RÉSULTAT : ✓ Extension 1001 créée et fonctionnelle
```

---

## 🚀 MESSAGE FINAL

### Tu es prêt si tu maîtrises :

**Windows :**
- ✅ Création/modification d'utilisateurs AD (GUI + PowerShell)
- ✅ Script PowerShell basique (boucles, Import-Csv)
- ✅ Vérification avec Get-ADUser

**Linux :**
- ✅ Navigation (cd, ls, pwd)
- ✅ Copie/déplacement (cp, mv, rm)
- ✅ Permissions (chmod, chown)
- ✅ SSH (connexion, scp)

**IPBX :**
- ✅ Création d'extension
- ✅ Configuration de renvoi d'appels
- ✅ Test d'appel entre postes

### Stratégie gagnante

1. **LIRE toutes les questions d'abord**
2. **PRIORISER** (facile → difficile)
3. **CAPTURER** (avant/pendant/après)
4. **VÉRIFIER** (chaque action)
5. **EXPLIQUER** (si pas de capture)
6. **PASSER** (si bloqué)
7. **REVENIR** (en fin d'épreuve)

### Rappel des règles d'or

🔴 **SI TU NE SAIS PAS → PASSE À LA SUIVANTE**  
🔴 **PAS DE TEMPS POUR CAPTURE → EXPLIQUE CE QUE TU AS FAIT**  
🔴 **INTERNET EST AUTORISÉ → UTILISE-LE INTELLIGEMMENT**

---

## 💪 Tu as tout ce qu'il faut pour réussir !

- Tu connais les commandes de base
- Tu as pratiqué sur les VM
- Tu as une logique de développeur (avantage !)
- Tu sais chercher sur Internet

**Maintenant, c'est le moment de montrer ce que tu vaux !**

**Bonne chance pour l'épreuve pratique ! 🎯🚀**

---

*Document créé pour Rimk - TSSR Nextformation 2024-2025*
*Épreuve pratique - 1h45 - Partie 1/3 du cursus*
