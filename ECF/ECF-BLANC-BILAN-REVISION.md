# 📝 ECF Blanc - Bilan et Révisions

> 📅 **Date du jour :** 11 février 2026
> 🎯 **ECF blanc prévu :** ~25 février 2026 (dans 2 semaines)
> ⏱️ **Durée de l'épreuve :** 4 heures
> ⏱️ **Temps de préparation :** 14 jours
> 📚 **Formation :** TSSR Nextformation

---

## ⚡ FORMAT DE L'ECF (4 HEURES)

### Structure de l'épreuve

L'ECF est composé de **DEUX parties** :

#### 📌 **PARTIE 1 : PRATIQUE (Windows/Linux)** (~2h30)
**Format :** Manipulations sur machines virtuelles VMware
**Livrables :** Document PDF avec screenshots demandés (6 à 10 screenshots)

**Exemples de manipulations :**
```
✅ Installation Windows 10/11 Pro
   - Configuration réseau en DHCP
   - Renommer la machine (ex: EPCF-0622)

✅ Gestion de disques
   - Ajouter un disque dur (ex: 80 Go)
   - Formater en NTFS
   - Nommer le lecteur (ex: "Backup")

✅ Sauvegarde/Restauration
   - Créer une image système sur disque Backup
   - Supprimer le disque principal
   - Restaurer sur un nouveau disque

✅ Installation de logiciels
   - Installer Firefox/Chrome dernière version
   - Installer des applications métier

✅ Configuration réseau
   - Configurer IP statique/DHCP
   - Joindre un domaine
   - Tester la connectivité
```

#### 📌 **PARTIE 2 : THÉORIQUE** (~1h30)
**Format :** Questionnaire à répondre sur le document (conversion en PDF)
**Exemples de questions :** (basé sur le vrai ECF)

**Questions Windows :**
1. Quelle commande pour atteindre "Connexions réseau" ?
   → `ncpa.cpl`

2. Où changer le nom de l'ordinateur et le groupe de travail ?
   → Panneau de configuration → Système → Renommer ce PC

3. Qu'est-ce qu'un domaine Windows Server ?
   → Environnement sécurisé centralisé géré par un contrôleur de domaine via AD DS

4. Que sont les "objets" dans Windows Server ?
   → Utilisateurs, groupes, OU, ordinateurs, imprimantes, etc.

5. Rôles pour un WDS fonctionnel ?
   → AD DS, DNS, DHCP, WDS

6. Quels sont les 2 fichiers .wim pour le déploiement ?
   → `boot.wim` et `install.wim`

**Questions Linux :**
7. Système de fichiers natif Linux ?
   → ext4

8. Que trouve-t-on dans ces dossiers ?
   - `/` → Racine du système de fichiers
   - `/root` → Répertoire personnel de l'administrateur
   - `/home` → Répertoires personnels des utilisateurs
   - `/etc` → Configurations système
   - `/var` → Données variables (logs, caches)

9. À quel OS ressemble MacOS ?
   → Linux (MacOS est dérivé d'UNIX, comme Linux)

**Questions Sécurité :**
10. Précautions avant intervention matérielle ?
    → Débrancher alimentation, décharger électricité (bouton power), gants antistatiques

11. Logiciel de cryptage Windows ?
    → BitLocker

12. Précautions de sécurité pour :
    - **Réseau :** Mises à jour, antivirus, EDR, firewall
    - **Postes :** Mots de passe complexes, verrouillage automatique
    - **Données :** Sauvegardes régulières, GPO, droits NTFS
    - **Mails :** Formation anti-phishing
    - **Clés USB :** Bloquer les clés externes par GPO

### 🎯 Stratégie pour réussir l'ECF (4h)

**⏰ Gestion du temps recommandée :**

| Timing | Activité | Conseil |
|--------|----------|---------|
| **0:00-0:15** | Lecture complète | Lire TOUT avant de commencer |
| **0:15-2:45** | Partie pratique | Lancer les VM en premier, travailler pendant install |
| **2:45-4:00** | Partie théorique | Répondre aux questions, relecture |

**💡 Conseils clés :**
1. ✅ **Lancer les installations de VM immédiatement** (pendant qu'elles installent, avancer sur autre chose)
2. ✅ **Commencer par ce que vous maîtrisez** (confiance + rapidité)
3. ✅ **Prendre des screenshots clairs** avec votre nom/date visible
4. ✅ **Ne pas rester bloqué** → Passer à la suite, revenir après
5. ✅ **Garder 30 min pour relecture** et finalisation PDF

**📸 Screenshots à prendre :**
- ⚠️ Chaque fois que vous voyez un symbole 📷, prenez un screenshot
- ✅ Nommer vos fichiers : `EPCF_VotreNom_Question1.png`
- ✅ Tout regrouper dans UN SEUL PDF : `EPCF_VotreNom.pdf`

---

## 📊 Analyse de votre situation

### Vous avez dit : "J'ai eu un peu de tout"

C'est normal ! Un ECF blanc teste **toutes les compétences vues depuis le début** de la formation. Voici ce que vous avez déjà étudié et ce que vous devez réviser.

---

## 🎯 Modules déjà vus (à réviser en priorité)

### ✅ DÉCEMBRE 2025

#### 1. Adressage IP et Subnetting (12h)
**Ce qu'on peut vous demander :**
- Calculer des sous-réseaux
- Déterminer l'adresse réseau, broadcast, première/dernière IP
- Trouver le masque adapté à un besoin
- Classes d'adresses (A, B, C)
- CIDR et notation /24, /25, etc.

**Questions types :**
```
Q: Combien d'hôtes dans un réseau 192.168.1.0/26 ?
R: 2^(32-26) - 2 = 62 hôtes

Q: Quelle est l'adresse broadcast de 10.0.0.0/8 ?
R: 10.255.255.255

Q: Quel masque pour 50 postes ?
R: /26 (255.255.255.192) = 62 hôtes utilisables
```

**📚 Cours disponible :** `01-Reseaux/adressage-ip-subnetting.md`

---

### ✅ JANVIER 2026

#### 2. Active Directory (30h)
**Ce qu'on peut vous demander :**
- Rôle d'un contrôleur de domaine
- Installation et configuration AD DS
- Création d'utilisateurs, groupes, OU
- Stratégies de mots de passe
- Différence entre groupes de sécurité et distribution
- Réplication entre DC
- FSMO (rôles de maître d'opérations)

**Questions types :**
```
Q: Quelle commande pour promouvoir un serveur en DC ?
R: dcpromo (Windows Server 2012) ou Install-WindowsFeature AD-Domain-Services +
   Install-ADDSForest (PowerShell)

Q: Quelle différence entre OU et groupe ?
R: OU = conteneur pour organiser les objets + appliquer GPO
   Groupe = attribuer des permissions à plusieurs utilisateurs

Q: Citez les 5 rôles FSMO.
R: Schema Master, Domain Naming Master, RID Master, PDC Emulator, Infrastructure Master
```

**📚 Cours disponible :** `02-Windows-Server/active-directory.md`

#### 3. DNS, DHCP, WINS (25h)
**Ce qu'on peut vous demander :**
- Rôle du DNS (résolution de noms)
- Types d'enregistrements DNS (A, AAAA, CNAME, MX, PTR, SRV)
- Configuration d'un serveur DHCP
- Étendue, plage d'exclusion, réservation
- Bail DHCP (lease time)
- Rôle de WINS (obsolète, résolution NetBIOS)

**Questions types :**
```
Q: Quel enregistrement DNS pour un serveur mail ?
R: MX (Mail eXchanger)

Q: Qu'est-ce qu'une réservation DHCP ?
R: Attribution d'une IP fixe à une adresse MAC spécifique

Q: Différence entre DNS et WINS ?
R: DNS = résolution de noms d'hôtes FQDN (moderne)
   WINS = résolution de noms NetBIOS (obsolète)
```

#### 4. GPO - Stratégies de groupe (20h)
**Ce qu'on peut vous demander :**
- Créer une GPO
- Lier une GPO à une OU
- Ordre d'application des GPO (LSDOU)
- Stratégies ordinateur vs utilisateur
- Héritage et blocage
- Déploiement de logiciels par GPO
- Mappage de lecteurs réseau

**Questions types :**
```
Q: Dans quel ordre s'appliquent les GPO ?
R: Local → Site → Domain → OU (LSDOU)

Q: Comment forcer l'application immédiate d'une GPO ?
R: gpupdate /force

Q: Où créer une GPO pour mapper un lecteur Z: pour tous les utilisateurs ?
R: Computer Configuration\Preferences\Windows Settings\Drive Maps
   OU User Configuration si spécifique à l'utilisateur
```

**⚠️ À créer :** Cours GPO détaillé

#### 5. Windows Client (15h)
**Ce qu'on peut vous demander :**
- Joindre un poste à un domaine
- Profils utilisateurs (local, itinérant, obligatoire)
- Gestion des disques (partitions, volumes)
- Permissions NTFS vs partage
- Outils d'administration (MMC, gpedit.msc, etc.)
- Dépannage Windows (mode sans échec, restauration système)

**Questions types :**
```
Q: Quelle commande pour joindre un domaine ?
R: Panneau de configuration → Système → Modifier les paramètres
   OU PowerShell: Add-Computer -DomainName "contoso.local"

Q: Différence entre permission NTFS et partage ?
R: NTFS = s'applique localement et à distance (cumulatif)
   Partage = uniquement à distance (plus restrictif si les deux)

Q: Qu'est-ce qu'un profil itinérant ?
R: Profil utilisateur stocké sur un serveur, disponible sur n'importe quel poste du domaine
```

#### 6. Introduction réseau local (15h)
**Ce qu'on peut vous demander :**
- Topologies réseau (bus, étoile, anneau)
- Câblage (droit, croisé)
- Équipements (hub, switch, routeur)
- Protocoles de la couche 2 (Ethernet)
- Adresse MAC
- Domaine de collision vs domaine de diffusion

**Questions types :**
```
Q: Différence entre hub et switch ?
R: Hub = répète le signal sur tous les ports (domaine de collision unique)
   Switch = commute intelligemment selon adresse MAC (un domaine par port)

Q: Qu'est-ce qu'une adresse MAC ?
R: Adresse physique unique de la carte réseau (6 octets, ex: 00:1A:2B:3C:4D:5E)

Q: Quand utiliser un câble croisé ?
R: Pour connecter directement 2 PC, ou 2 switchs (sauf si Auto-MDIX)
```

---

### ✅ FÉVRIER 2026 (en cours)

#### 7. Linux serveur (40h) - Terminé le 06/02
**Ce qu'on peut vous demander :**
- Commandes de base (ls, cd, mkdir, rm, cp, mv, cat, grep, etc.)
- Gestion des permissions (chmod, chown)
- Gestion des utilisateurs (useradd, usermod, passwd)
- Arborescence Linux (/etc, /var, /home, /usr, /bin)
- Installation de paquets (apt, yum)
- Services (systemctl start/stop/restart/status)
- Éditeur vi/vim ou nano
- Logs système (/var/log)

**Questions types :**
```
Q: Comment donner tous les droits à un fichier ?
R: chmod 777 fichier (ou chmod a+rwx fichier)
   ⚠️ Dangereux ! Préférer chmod 755 ou 644

Q: Où se trouvent les configurations système ?
R: /etc

Q: Comment redémarrer le service SSH ?
R: systemctl restart sshd
   OU service sshd restart (ancien)

Q: Comment voir les logs système ?
R: /var/log/syslog (Debian/Ubuntu)
   /var/log/messages (RedHat/CentOS)
   OU journalctl (systemd)
```

**📚 Cours disponibles :**
- `03-Linux/README.md` (guide complet)
- `03-Linux/01-commandes-essentielles.md`
- `03-Linux/03-gestion-utilisateurs-groupes.md`
- `03-Linux/05-gestion-paquets-logiciels.md`
- `03-Linux/troubleshooting-logs-diagnostics.md`

#### 8. ToIP - Téléphonie sur IP (20h) - En cours (08-11/02)
**Ce qu'on peut vous demander :**
- Différence entre VoIP et téléphonie classique
- Conversion voix analogique en numérique
- Protocoles SIP vs RTP
- Ports utilisés (SIP 5060/5061, RTP 16384-32767)
- Codecs (G.711, G.729, G.722)
- Qu'est-ce qu'un IPBX
- Rôle de la QoS en VoIP
- RTCP pour la supervision

**Questions types :**
```
Q: Quel protocole transporte la voix ?
R: RTP (Real-time Transport Protocol)
   SIP = signalisation uniquement !

Q: Quel codec pour un lien WAN lent ?
R: G.729 (8 Kbps, compressé)

Q: À quoi sert la QoS en VoIP ?
R: Prioriser les paquets de voix pour éviter latence, jitter et pertes

Q: Ports à ouvrir sur un firewall pour VoIP ?
R: UDP 5060 (SIP), UDP 16384-32767 (RTP)
```

**📚 Cours disponibles :**
- `10-Telephonie-VoIP/01-fondamentaux-voip.md`
- `10-Telephonie-VoIP/02-protocoles-voip.md`
- `10-Telephonie-VoIP/03-questions-examens-voip.md` ✅ **NOUVEAU !**

---

### 📅 Ce qui arrive AVANT votre ECF (à étudier rapidement)

#### 9. GPO Approfondissement (14-15 février)
- Stratégies avancées
- Déploiement de logiciels
- Scripts de démarrage/arrêt
- Préférences GPO

#### 10. WSUS (16 février)
- Windows Server Update Services
- Gestion centralisée des mises à jour
- Groupes d'ordinateurs
- Approbation des mises à jour

#### 11. GLP - Gestion de Parc (17-19 février)
- Inventaire matériel/logiciel
- Outils (GLPI, OCS Inventory)
- Gestion des licences
- Suivi des interventions

#### 12. Messagerie (20 février)
- Exchange Server
- Boîtes aux lettres
- Groupes de distribution
- Outlook

#### 13. Modèle OSI et TCP/IP (22-23 février)
- 7 couches du modèle OSI
- Correspondance avec TCP/IP
- Protocoles par couche
- Encapsulation

#### 14. Support utilisateur (24-25 février)
- Méthodologie de dépannage
- Helpdesk niveau 1-2
- Ticketing
- Communication avec l'utilisateur

**📚 Cours disponible :**
- `01-Reseaux/modele-osi-tcpip.md`

---

## 🛠️ Compétences PRATIQUES à maîtriser (PARTIE 1 de l'ECF)

### 🖥️ Windows - Manipulations VMware

#### ✅ Installation Windows 10/11 Pro
**Prérequis :** Savoir créer une VM dans VMware
```
1. Créer nouvelle VM :
   - Nom : EPCF-0622 (ou nom demandé)
   - RAM : 4 Go minimum
   - Disque : 65 Go
   - Réseau : NAT ou Bridged

2. Installer Windows 10/11 Pro :
   - Langue : Français
   - Clavier : Français (AZERTY)
   - Version : Pro (PAS Home !)

3. Configuration initiale :
   - Nom du PC : EPCF-0622
   - Compte local (si pas de domaine)
   - Réseau : DHCP activé

📷 Screenshot : Bureau Windows avec nom du PC visible
```

#### ✅ Gestion des disques
**Compétence clé :** Ajouter, formater, nommer des disques
```
1. Ajouter un disque dans VMware :
   - VM éteinte
   - VM Settings → Add → Hard Disk
   - Taille : 80 Go (exemple)
   - Type : SCSI (recommandé)
   - Format : Thick provisioned

2. Initialiser le disque dans Windows :
   - Win + X → Disk Management
   - Clic droit sur nouveau disque → Initialize Disk
   - Style : GPT (moderne) ou MBR (ancien)

3. Créer une partition :
   - Clic droit sur espace non alloué → New Simple Volume
   - Taille : Maximum
   - Lettre : E: ou autre (exemple : Backup)
   - Format : NTFS
   - Nom : "Backup" (ou nom demandé)

4. Vérifier :
   - Ouvrir l'explorateur
   - Voir le nouveau lecteur

📷 Screenshot : Disk Management avec disque ajouté et formaté
```

#### ✅ Sauvegarde et restauration d'image système
**⚠️ TRÈS IMPORTANT : Souvent demandé à l'ECF**
```
1. Créer une image système :
   - Panneau de configuration
   - Système et sécurité
   - Sauvegarder et restaurer (Windows 7)
   - Créer une image système
   - Emplacement : Disque E: (Backup)
   - Disques à sauvegarder : Cocher C:
   - Démarrer la sauvegarde
   - ATTENDRE (peut prendre 15-30 min !)

📷 Screenshot : Sauvegarde terminée avec succès

2. Simuler une panne (supprimer disque principal) :
   - Éteindre la VM
   - VM Settings → Supprimer le disque C: (principal)
   - ⚠️ NE PAS supprimer le disque Backup !

3. Ajouter un nouveau disque :
   - Add → Hard Disk → 70 Go (exemple)

4. Restaurer l'image :
   - Démarrer la VM (va planter, normal)
   - Boot sur DVD Windows ou Recovery
   - Repair your computer
   - Troubleshoot → Advanced → System Image Recovery
   - Sélectionner l'image sur disque Backup
   - Restaurer vers nouveau disque
   - ATTENDRE (15-30 min)

5. Vérifier :
   - Redémarrer
   - Windows doit démarrer normalement
   - Toutes les données doivent être là

📷 Screenshot : Windows restauré et fonctionnel
```

#### ✅ Installation de logiciels
```
1. Télécharger Firefox/Chrome :
   - Ouvrir navigateur (Edge par défaut)
   - Aller sur mozilla.org ou google.com/chrome
   - Télécharger dernière version

2. Installer :
   - Exécuter le .exe
   - Installation standard
   - Autoriser les autorisations UAC

3. Vérifier :
   - Ouvrir le logiciel
   - Vérifier la version (Aide → À propos)

📷 Screenshot : Logiciel installé et version visible
```

#### ✅ Configuration réseau
```
1. Vérifier IP actuelle (DHCP) :
   - Win + R → cmd
   - ipconfig /all
   - Noter l'adresse IP

2. Configurer IP statique (si demandé) :
   - Win + R → ncpa.cpl
   - Clic droit sur Ethernet → Propriétés
   - IPv4 → Propriétés
   - Utiliser l'adresse IP suivante :
     IP : 192.168.1.100 (exemple)
     Masque : 255.255.255.0
     Passerelle : 192.168.1.1
     DNS : 192.168.1.10 (ou 8.8.8.8)
   - OK

3. Tester :
   - ping 192.168.1.1 (passerelle)
   - ping 8.8.8.8 (Internet)
   - nslookup google.com (DNS)

📷 Screenshot : ipconfig /all avec nouvelle config
```

#### ✅ Joindre un domaine
```
1. Prérequis :
   - DNS doit pointer vers le DC (192.168.1.10 par exemple)
   - Ping du DC doit fonctionner

2. Joindre le domaine :
   - Win + Pause → Modifier les paramètres
   - Modifier → Domaine : contoso.local (exemple)
   - Entrer identifiants administrateur du domaine
   - Redémarrer

3. Vérifier :
   - Connexion avec compte du domaine
   - whoami → doit afficher DOMAINE\utilisateur

📷 Screenshot : whoami montrant le domaine
```

---

### 🐧 Linux - Manipulations de base

#### ✅ Navigation et fichiers
```bash
# Se déplacer
cd /home              # Aller dans /home
cd /etc               # Aller dans /etc
pwd                   # Afficher chemin actuel
ls -la                # Lister avec détails

# Créer/supprimer
mkdir /home/test      # Créer répertoire
touch fichier.txt     # Créer fichier vide
rm fichier.txt        # Supprimer fichier
rm -rf dossier/       # Supprimer dossier

# Copier/déplacer
cp source dest        # Copier
mv ancien nouveau     # Renommer/déplacer

📷 Screenshot : Commandes exécutées avec résultats
```

#### ✅ Gestion des permissions
```bash
# Voir les permissions
ls -l fichier.txt     # -rw-r--r--

# Modifier (numérique)
chmod 755 script.sh   # rwxr-xr-x
chmod 644 fichier.txt # rw-r--r--
chmod 777 fichier     # rwxrwxrwx (⚠️ dangereux)

# Modifier (symbolique)
chmod u+x script.sh   # Ajouter exécution pour user
chmod g-w fichier     # Retirer écriture pour groupe
chmod o-r fichier     # Retirer lecture pour others

# Changer propriétaire
chown user:group fichier
chown -R user:group /dossier/

📷 Screenshot : ls -l montrant les permissions
```

#### ✅ Gestion des utilisateurs
```bash
# Créer utilisateur
useradd jdupont            # Créer
passwd jdupont             # Définir mot de passe

# Modifier utilisateur
usermod -aG sudo jdupont   # Ajouter au groupe sudo
usermod -s /bin/bash jdup  # Changer shell

# Supprimer utilisateur
userdel jdupont            # Supprimer (garde /home)
userdel -r jdupont         # Supprimer avec /home

# Voir les utilisateurs
cat /etc/passwd            # Liste utilisateurs
id jdupont                 # Infos utilisateur

📷 Screenshot : Utilisateur créé et visible dans /etc/passwd
```

---

## 📝 Questions THÉORIQUES attendues (PARTIE 2 de l'ECF)

### Commandes Windows
```
Q: Commande pour ouvrir "Connexions réseau" ?
R: ncpa.cpl

Q: Commande pour renommer un PC et changer le groupe de travail ?
R: Win + Pause → Modifier les paramètres → Modifier

Q: Commande pour gérer les disques ?
R: diskmgmt.msc OU Disk Management

Q: Commande pour voir les services ?
R: services.msc

Q: Commande ipconfig pour tout voir ?
R: ipconfig /all
```

### Windows Server
```
Q: Qu'est-ce qu'un domaine Windows Server ?
R: Environnement sécurisé centralisé géré par un contrôleur de domaine via AD DS

Q: Que sont les "objets" dans AD ?
R: Utilisateurs, groupes, OU, ordinateurs, imprimantes

Q: Rôles nécessaires pour WDS (Windows Deployment Services) ?
R: AD DS, DNS, DHCP, WDS

Q: Quels fichiers .wim pour le déploiement Windows ?
R: boot.wim (image de démarrage) et install.wim (image d'installation)

Q: Ports Active Directory ?
R: 389 (LDAP), 636 (LDAPS), 88 (Kerberos), 53 (DNS)
```

### Linux
```
Q: Système de fichiers natif Linux ?
R: ext4 (aussi ext3, xfs, btrfs)

Q: Arborescence Linux :
/      → Racine du système
/root  → Répertoire de l'administrateur (root)
/home  → Répertoires des utilisateurs normaux
/etc   → Fichiers de configuration système
/var   → Données variables (logs, caches, mails)
/usr   → Applications et programmes
/bin   → Binaires essentiels (ls, cd, etc.)
/tmp   → Fichiers temporaires

Q: À quel OS ressemble MacOS ?
R: Linux/UNIX (MacOS est basé sur BSD Unix)
```

### Sécurité
```
Q: Précautions avant intervention matérielle ?
R:
- Débrancher l'alimentation secteur
- Appuyer sur bouton power pour décharger
- Porter des gants antistatiques
- Travailler sur surface non conductrice

Q: Logiciel de cryptage Windows ?
R: BitLocker (version Pro/Entreprise)

Q: Précautions de sécurité :
RÉSEAU :
- Mises à jour régulières
- Firewall activé et configuré
- Antivirus/EDR actif
- Segmentation (VLANs)
- Monitoring/supervision

POSTES :
- Mots de passe complexes (12+ caractères, mixte)
- Changement régulier des mots de passe
- Verrouillage automatique (5-10 min)
- Chiffrement BitLocker
- Mises à jour automatiques

DONNÉES :
- Sauvegardes régulières (3-2-1 rule)
- Permissions NTFS restrictives
- GPO pour contrôle d'accès
- Chiffrement des données sensibles
- Audit des accès

MAILS :
- Formation anti-phishing des utilisateurs
- Filtrage anti-spam
- Bannière d'avertissement mails externes
- Vérification liens et pièces jointes
- Authentification SPF/DKIM/DMARC

CLÉS USB :
- Bloquer par GPO les clés non autorisées
- Liste blanche de périphériques
- Scan antivirus automatique
- Chiffrement obligatoire
- Journalisation des accès
```

---

## 📝 Planning de révision recommandé (14 jours)

### Semaine 1 (12-18 février)

| Jour | Matin (2h) | Après-midi (2h) | Soir (1h) |
|------|------------|-----------------|-----------|
| **Mar 12** | Adressage IP (exercices) | Active Directory (TP) | Réviser DNS/DHCP |
| **Mer 13** | Linux commandes | Linux permissions | Réviser ToIP questions |
| **Jeu 14** | Cours GPO (prévu) | Cours GPO (prévu) | Revoir GPO janvier |
| **Ven 15** | TP GPO (prévu) | TP GPO (prévu) | Faire exercices GPO |
| **Sam 16** | Cours WSUS (prévu) | Réviser Windows Client | Joindre domaine (TP) |
| **Dim 17** | REPOS | Réviser points faibles | QCM blancs |

### Semaine 2 (19-25 février)

| Jour | Matin (2h) | Après-midi (2h) | Soir (1h) |
|------|------------|-----------------|-----------|
| **Mer 19** | TP GLP (prévu) | TP GLP (prévu) | Réviser tous les cours |
| **Jeu 20** | Messagerie (prévu) | Réviser Active Directory | Faire exercices AD |
| **Ven 21** | Révisions générales | Révisions générales | QCM blancs |
| **Sam 22** | Modèle OSI (prévu) | Modèle OSI (prévu) | Mémoriser OSI |
| **Dim 23** | Modèle OSI (prévu) | Exercices réseaux | Fiches de révision |
| **Lun 24** | Support (prévu) | Support (prévu) | RÉVISION TOTALE |
| **Mar 25** | **ECF BLANC** 🎯 | **ECF BLANC** 🎯 | Débrief |

---

## 🎯 Checklist de préparation

### ✅ Connaissances techniques à maîtriser

#### Réseau
- [ ] Calculer un sous-réseau rapidement
- [ ] Connaître les classes d'adresses
- [ ] Différencier hub, switch, routeur
- [ ] Expliquer le modèle OSI (7 couches)
- [ ] Connaître les ports courants (80, 443, 22, 3389, 53, 389, etc.)

#### Windows Server
- [ ] Installer et configurer AD DS
- [ ] Créer des utilisateurs, groupes, OU
- [ ] Créer et lier une GPO
- [ ] Configurer DNS et DHCP
- [ ] Joindre un poste au domaine

#### Linux
- [ ] Naviguer dans l'arborescence
- [ ] Gérer fichiers et répertoires
- [ ] Gérer les permissions (chmod, chown)
- [ ] Créer/modifier un utilisateur
- [ ] Redémarrer un service
- [ ] Lire les logs

#### VoIP
- [ ] Différence SIP et RTP
- [ ] Ports VoIP (5060, 16384-32767)
- [ ] Codecs (G.711 vs G.729)
- [ ] Rôle de la QoS
- [ ] Qu'est-ce qu'un IPBX

#### Support
- [ ] Méthodologie de dépannage
- [ ] Communication avec l'utilisateur
- [ ] Gestion des priorités

### ✅ Compétences pratiques

- [ ] Faire un calcul de sous-réseau en moins de 2 minutes
- [ ] Créer un utilisateur AD en moins de 5 minutes
- [ ] Créer une GPO simple en moins de 10 minutes
- [ ] Naviguer dans Linux sans hésiter
- [ ] Diagnostiquer un problème réseau méthodiquement

---

## 🚨 Points de vigilance (erreurs courantes)

### ❌ Erreurs à éviter

1. **Adressage IP**
   - ❌ Oublier le /masque (toujours noter 192.168.1.0/24)
   - ❌ Confondre adresse réseau et première IP utilisable
   - ❌ Ne pas soustraire 2 pour les hôtes (réseau + broadcast)

2. **Active Directory**
   - ❌ Confondre OU et groupe
   - ❌ Oublier que le DNS doit pointer vers le DC
   - ❌ Ne pas respecter la casse dans les noms de domaine

3. **GPO**
   - ❌ Confondre Computer Configuration et User Configuration
   - ❌ Oublier de lier la GPO à une OU
   - ❌ Ne pas faire gpupdate /force après création

4. **Linux**
   - ❌ Confondre / et \\ (Linux utilise /)
   - ❌ Oublier sudo pour les commandes admin
   - ❌ Ne pas connaître vi/vim (au moins :wq pour quitter)

5. **VoIP**
   - ❌ Confondre SIP (signalisation) et RTP (voix)
   - ❌ Dire que QoS supervise RTP (c'est RTCP !)
   - ❌ Oublier que RTP utilise UDP

---

## 📚 Ressources pour réviser

### Dans votre dépôt Git

```
✅ DISPONIBLES :
├── 01-Reseaux/
│   ├── modele-osi-tcpip.md
│   └── adressage-ip-subnetting.md
│
├── 02-Windows-Server/
│   ├── active-directory.md
│   ├── serveur-ftp.md
│   └── fsrm-quotas.md
│
├── 03-Linux/
│   ├── README.md
│   ├── 01-commandes-essentielles.md
│   ├── 03-gestion-utilisateurs-groupes.md
│   ├── 05-gestion-paquets-logiciels.md
│   └── troubleshooting-logs-diagnostics.md
│
├── 10-Telephonie-VoIP/
│   ├── 01-fondamentaux-voip.md
│   ├── 02-protocoles-voip.md
│   └── 03-questions-examens-voip.md ✅ NOUVEAU
│
└── Document-revision/
    └── ECF-BLANC-BILAN-REVISION.md ← CE FICHIER
```

### Outils en ligne

- **Calculateur IP :** https://www.subnet-calculator.com/
- **Quizz réseau :** https://www.subnetting.net/
- **Pratique Linux :** https://overthewire.org/wargames/bandit/
- **Documentation Microsoft :** https://learn.microsoft.com/

---

## 🎯 Conseils pour le jour J

### Avant l'épreuve
- ✅ Bien dormir la veille (8h)
- ✅ Prendre un bon petit-déjeuner
- ✅ Arriver 15 minutes en avance
- ✅ Avoir une bouteille d'eau
- ✅ Relire vos fiches de révision

### Pendant l'épreuve
- ✅ **Lire TOUTES les questions** avant de commencer
- ✅ Commencer par les questions faciles (confiance !)
- ✅ Gérer son temps (ex: 2h = 120 min / 40 questions = 3 min/question)
- ✅ Vérifier ses calculs (subnetting notamment)
- ✅ Relire ses réponses si temps restant
- ✅ Ne pas rester bloqué (passer à la suivante, revenir après)

### Gestion du stress
- 🧘 Respirer profondément si stress
- 🧘 Se dire "Je connais mes cours, je vais y arriver"
- 🧘 L'ECF blanc est fait pour APPRENDRE, pas pour sanctionner
- 🧘 Les erreurs = opportunités de progresser

### Après l'épreuve
- 📝 Noter les questions difficiles pour réviser
- 📝 Demander les corrections
- 📝 Analyser ses erreurs
- 📝 Refaire les exercices ratés

---

## 💪 Message de motivation

Vous êtes à **2 mois** de formation, vous avez déjà vu énormément de choses :

✅ Active Directory
✅ DNS / DHCP
✅ GPO
✅ Linux serveur
✅ ToIP
✅ Adressage IP
✅ Réseau local

C'est **NORMAL** d'avoir l'impression "d'avoir eu un peu de tout" et de ne pas tout maîtriser parfaitement. L'ECF blanc est justement là pour :

1. **Identifier vos points faibles** → Pour les réviser
2. **Vous mettre en condition d'examen** → Pour ne pas stresser au vrai examen
3. **Valider vos acquis** → Vous verrez que vous savez plus que vous ne pensez !

### 🎯 Objectif réaliste pour l'ECF blanc

**Ne visez PAS 20/20 !** L'ECF blanc est un **outil de diagnostic**, pas une note définitive.

- ✅ **10-12/20** : Très bon pour un ECF blanc (2 mois de formation)
- ✅ **13-15/20** : Excellent, vous êtes bien parti
- ✅ **> 16/20** : Exceptionnel, continuez comme ça !
- ⚠️ **< 10/20** : Points à retravailler identifiés (c'est fait pour ça !)

**L'important :** Comprendre vos erreurs et progresser d'ici le vrai examen en juin.

---

## 📞 Actions immédiates (aujourd'hui)

### Ce soir (11/02/2026) :

1. ✅ **Lire ce document en entier** ← Vous y êtes !
2. ✅ **Relire le cours sur la VoIP** (vous avez cours demain)
   - `10-Telephonie-VoIP/03-questions-examens-voip.md`
3. ✅ **Faire 5 exercices de subnetting** pour ne pas perdre la main
4. ✅ **Revoir les commandes Linux essentielles**
   - `03-Linux/01-commandes-essentielles.md`

### Demain (12/02/2026) :

1. ✅ Suivre le cours d'anglais (12/02)
2. ✅ Réviser Active Directory (1h)
3. ✅ Faire un TP : créer 5 utilisateurs + 2 groupes + 1 OU
4. ✅ Préparer les questions pour le cours GPO (14-15/02)

---

## ✅ Checklist finale avant ECF

**3 jours avant (22/02) :**
- [ ] Revoir TOUS les cours
- [ ] Refaire les exercices ratés
- [ ] Faire des QCM blancs
- [ ] Créer des fiches de révision (1 fiche = 1 sujet)

**Veille (24/02) :**
- [ ] Relire ses fiches
- [ ] Faire un dernier TP simple (confiance)
- [ ] Préparer son matériel
- [ ] SE COUCHER TÔT (23h max)

**Jour J (25/02) :**
- [ ] Petit-déjeuner copieux
- [ ] Relire fiches 30 min avant
- [ ] Arriver en avance
- [ ] RESTER CALME et FAIRE DE SON MIEUX 💪

---

## 📊 Grille d'auto-évaluation

Évaluez-vous sur chaque sujet (1 = faible, 5 = excellent) :

| Sujet | Note /5 | À retravailler ? |
|-------|---------|------------------|
| Adressage IP / Subnetting | ___/5 | [ ] Oui |
| Active Directory | ___/5 | [ ] Oui |
| DNS / DHCP | ___/5 | [ ] Oui |
| GPO | ___/5 | [ ] Oui |
| Windows Client | ___/5 | [ ] Oui |
| Linux commandes | ___/5 | [ ] Oui |
| Linux permissions | ___/5 | [ ] Oui |
| VoIP / ToIP | ___/5 | [ ] Oui |
| Réseau local | ___/5 | [ ] Oui |
| Modèle OSI | ___/5 | [ ] Oui |
| Support utilisateur | ___/5 | [ ] Oui |

**Priorité de révision :** Les sujets notés ≤ 3/5

---

## 🔗 Liens utiles

- **Planning complet :** `planning-formation-tssr.md`
- **Tous vos cours :** Dossiers `01-Reseaux/`, `02-Windows-Server/`, `03-Linux/`, etc.
- **Questions VoIP :** `10-Telephonie-VoIP/03-questions-examens-voip.md`

---

<div align="center">

# 💪 VOUS ALLEZ Y ARRIVER !

**"Le succès, c'est 10% d'inspiration et 90% de transpiration."**
— Thomas Edison

**Travaillez régulièrement, restez motivé, et l'ECF blanc ne sera qu'une étape vers votre titre TSSR !** 🎓

---

📚 **Bonne chance pour vos révisions !**
🎯 **Rendez-vous le 25 février pour l'ECF blanc !**

---

[⬅️ Retour au dossier Document-revision](./README.md)

</div>
