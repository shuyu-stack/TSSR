# 🎯 ECF BLANC - ASTUCES ET BONNES PRATIQUES

**Auteur** : Guide de préparation ECF TSSR
**Version** : 2025-2026

## 📚 TABLE DES MATIÈRES

- Comprendre ce que tu fais
- **Fondamentaux Réseau (Vision Senior)**
  - Adressage IP et calcul
  - Subnetting pratique
  - DNS expliqué
  - DHCP en détail
  - HTTP vs HTTPS
  - TCP vs UDP
  - Modèle OSI
- Commandes à retenir
- Checklist ECF complète
- Les 10 erreurs à ÉVITER
- Astuces de rapidité
- Planning de l'ECF (3h)
- Structure du PDF à rendre
- Pour aller plus loin

---

## 🧠 Comprendre ce que tu fais

### L'analogie du réseau d'entreprise

Imagine une entreprise comme un immeuble :

```plaintext
🏢 Immeuble = Domaine Active Directory (ENTREPRISE.LOCAL)

👮 Réception = Serveur AD (SRV-DC01)
   → Vérifie les badges (authentification)
   → Donne les clés des bureaux (autorisations)

📋 Annuaire = DNS
   → "Où est le bureau de Marie ?" → "3ème étage, bureau 310"
   → "Où est le serveur ?" → "192.168.10.10"

🎫 Distributeur de badges temporaires = DHCP
   → Donne un badge/IP aux visiteurs automatiquement
   → "Voici ton badge numéro 192.168.10.105 pour la journée"

🗂️ Départements = OU (Organizational Units)
   → Direction = 3ème étage
   → Compta = 2ème étage
   → RH = 1er étage

📜 Règlement intérieur = GPO (Group Policy)
   → "Au 3ème étage (Direction), le fond d'écran doit être bleu"
   → "Au 2ème étage (Compta), Excel s'ouvre automatiquement"
```

**💡 En vrai** :
- **Active Directory** = Base de données centralisée des utilisateurs
- **DNS** = Résout les noms en adresses IP
- **DHCP** = Distribue automatiquement les IP
- **OU** = Dossiers pour organiser les utilisateurs
- **GPO** = Règles appliquées automatiquement

---

## 🌐 Fondamentaux Réseau (Vision Senior)

### Adressage IP - L'essentiel

Une IP IPv4 = 4 octets (32 bits) = 4 nombres de 0 à 255

```plaintext
192.168.10.50
 └─┬──┘ └─┬──┘
   Réseau  Hôte (dépend du masque)
```

**Les 3 masques que tu DOIS connaître :**

```plaintext
/24 = 255.255.255.0   → 254 hôtes (1 réseau classe C)
/16 = 255.255.0.0     → 65534 hôtes (256 réseaux C)
/8  = 255.0.0.0       → 16M hôtes (65536 réseaux C)
```

**💡 Astuce de calcul rapide :**

```plaintext
192.168.10.50/24
              └─ /24 = les 3 premiers octets sont le réseau
Réseau  : 192.168.10.0
Hôtes   : 192.168.10.1 → 192.168.10.254
Broadcast: 192.168.10.255
```

**Règle d'or** : Dans un réseau /24, tu as 256 adresses dont :
- `.0` = adresse réseau (interdit aux hôtes)
- `.1 → .254` = 254 adresses utilisables
- `.255` = broadcast (interdit aux hôtes)

---

### Subnetting - Méthode simple

**Question ECF classique** : "Divise 192.168.10.0/24 en 4 sous-réseaux"

```plaintext
Solution rapide :
/24 = 256 adresses
4 sous-réseaux = 256 ÷ 4 = 64 adresses chacun
64 = 2^6 → nouveau masque = /26

Résultat :
Sous-réseau 1 : 192.168.10.0/26   (.0 → .63)
Sous-réseau 2 : 192.168.10.64/26  (.64 → .127)
Sous-réseau 3 : 192.168.10.128/26 (.128 → .191)
Sous-réseau 4 : 192.168.10.192/26 (.192 → .255)
```

**Tableau des masques courants :**

```plaintext
/30 = 255.255.255.252 → 2 hôtes  (liens point-à-point)
/29 = 255.255.255.248 → 6 hôtes  (petit groupe)
/28 = 255.255.255.240 → 14 hôtes (département)
/27 = 255.255.255.224 → 30 hôtes
/26 = 255.255.255.192 → 62 hôtes
/25 = 255.255.255.128 → 126 hôtes
/24 = 255.255.255.0   → 254 hôtes (réseau standard PME)
```

**Formule magique** : 2^(32-masque) - 2 = nombre d'hôtes
- Exemple : /26 → 2^(32-26) - 2 = 2^6 - 2 = 64 - 2 = 62 hôtes

---

### DNS - Ce qu'un senior doit savoir

**DNS = Annuaire téléphonique d'Internet**

```plaintext
Tu tapes    : www.google.com
DNS traduit : 142.250.74.206
```

**Fonctionnement réel dans ton labo :**

```plaintext
1. Client WIN10 → tape ENTREPRISE.LOCAL
2. Client regarde son DNS configuré → 192.168.10.10 (le DC)
3. DC répond : "ENTREPRISE.LOCAL = 192.168.10.10 (moi)"
4. Client se connecte au serveur AD
```

**Types d'enregistrements DNS critiques :**

```plaintext
A     → Nom → IPv4 (www.site.com → 1.2.3.4)
AAAA  → Nom → IPv6
CNAME → Alias (www → webserver)
MX    → Serveur mail (gmail.com → smtp.google.com)
SRV   → Services (Active Directory utilise SRV !)
PTR   → IP → Nom (reverse DNS)
```

**💡 Pourquoi AD ne fonctionne PAS sans DNS :**

```plaintext
Active Directory stocke ses services dans DNS via des enregistrements SRV :
_ldap._tcp.ENTREPRISE.LOCAL → SRV → SRV-DC01:389

Sans DNS, le client ne trouve JAMAIS le contrôleur de domaine !
```

**Commandes de diagnostic :**

```cmd
nslookup ENTREPRISE.LOCAL        → Test résolution simple
nslookup -type=SRV _ldap._tcp.ENTREPRISE.LOCAL  → Vérifier SRV AD
ping ENTREPRISE.LOCAL            → Test IP + connectivité
```

---

### DHCP - Distribution automatique d'IP

**DHCP = Serveur qui loue des IP aux clients**

**Processus DORA (à connaître pour l'exam) :**

```plaintext
D - Discover  : Client broadcast "J'ai besoin d'une IP !"
O - Offer     : Serveur DHCP répond "Prends 192.168.10.105"
R - Request   : Client dit "OK, je prends cette IP"
A - Acknowledge: Serveur confirme "C'est validé pour 8 jours"
```

**Configuration DHCP complète (scope) :**

```plaintext
Étendue : 192.168.10.100 → 192.168.10.200
Options obligatoires :
  003 - Routeur (passerelle)    : 192.168.10.1
  006 - Serveurs DNS            : 192.168.10.10
  015 - Nom de domaine DNS      : ENTREPRISE.LOCAL
  051 - Durée du bail           : 8 jours (691200 sec)
```

**Exclusions** : Réserver des IP pour serveurs

```plaintext
Exclusions : 192.168.10.10 → 192.168.10.50
Pourquoi ? Les serveurs doivent avoir des IP FIXES
```

**Réservation DHCP** : Toujours la même IP pour un client spécifique

```plaintext
Basé sur l'adresse MAC :
MAC 00:0C:29:3F:4A:5B → toujours 192.168.10.150
Utilisé pour : imprimantes, serveurs de dev, caméras IP
```

**Diagnostic DHCP :**

```cmd
ipconfig /release    → Libérer l'IP actuelle
ipconfig /renew      → Redemander une IP au DHCP
ipconfig /all        → Voir l'IP obtenue et le serveur DHCP
```

---

### HTTP vs HTTPS - Protocoles Web

**HTTP (Port 80) - Non sécurisé**

```plaintext
Client → Serveur : GET /index.html HTTP/1.1
Serveur → Client : HTTP/1.1 200 OK + page HTML

Problème : Tout est en CLAIR (mots de passe visibles !)
```

**HTTPS (Port 443) - Sécurisé avec TLS/SSL**

```plaintext
1. Client → Serveur : "Bonjour, je veux une connexion sécurisée"
2. Serveur → Client : Certificat SSL (clé publique)
3. Échange de clés cryptées
4. Communication chiffrée AES-256

Résultat : Impossible de lire les données en transit
```

**Codes HTTP à connaître :**

```plaintext
2xx - Succès
  200 OK             → Requête réussie
  201 Created        → Ressource créée

3xx - Redirection
  301 Moved Permanently → Page déplacée (permanent)
  302 Found          → Redirection temporaire

4xx - Erreur client
  400 Bad Request    → Requête malformée
  401 Unauthorized   → Authentification requise
  403 Forbidden      → Accès refusé (même authentifié)
  404 Not Found      → Page introuvable

5xx - Erreur serveur
  500 Internal Server Error → Erreur serveur
  502 Bad Gateway    → Proxy/gateway en erreur
  503 Service Unavailable → Serveur surchargé
```

**💡 Analogie dev :**

```javascript
// HTTP = Envoyer une lettre sans enveloppe (lisible par tous)
fetch('http://api.example.com/data')

// HTTPS = Lettre dans enveloppe scellée + signature
fetch('https://api.example.com/data')
```

---

### TCP vs UDP - Les 2 protocoles de transport

**TCP (Transmission Control Protocol) - Fiable**

```plaintext
Caractéristiques :
✅ Connexion établie (handshake 3-way)
✅ Garantie de livraison
✅ Ordre des paquets respecté
✅ Contrôle d'erreur et retransmission
❌ Plus lent (overhead)

Utilisation :
- HTTP/HTTPS (Web)
- FTP (Transfert fichiers)
- SSH (Connexion sécurisée)
- SMTP (Email)
- Active Directory (LDAP sur TCP 389)
```

**3-way Handshake TCP :**

```plaintext
Client → Serveur : SYN (Synchronize)
Serveur → Client : SYN-ACK (Synchronize-Acknowledge)
Client → Serveur : ACK (Acknowledge)
→ Connexion établie !
```

**UDP (User Datagram Protocol) - Rapide**

```plaintext
Caractéristiques :
✅ Pas de connexion (fire and forget)
✅ Ultra rapide
✅ Faible overhead
❌ Pas de garantie de livraison
❌ Paquets peuvent arriver dans le désordre
❌ Pas de correction d'erreur

Utilisation :
- DNS (requêtes rapides)
- DHCP (découverte réseau)
- VoIP (appels vidéo/audio)
- Streaming vidéo
- Jeux en ligne (position joueurs)
```

**Tableau comparatif :**

```plaintext
┌─────────────┬─────────────┬─────────────┐
│ Critère     │ TCP         │ UDP         │
├─────────────┼─────────────┼─────────────┤
│ Connexion   │ Oui         │ Non         │
│ Fiabilité   │ 100%        │ Best effort │
│ Vitesse     │ Moyen       │ Rapide      │
│ Ordre       │ Garanti     │ Non garanti │
│ Overhead    │ Élevé       │ Faible      │
│ Use case    │ Fichiers    │ Streaming   │
└─────────────┴─────────────┴─────────────┘
```

**💡 Analogie du senior :**

```plaintext
TCP = Courrier recommandé avec accusé de réception
  → Tu es SÛR que le destinataire reçoit
  → Mais c'est plus lent et coûteux

UDP = Crier dans la rue
  → Rapide, pas cher
  → Mais pas sûr que tout le monde entende
```

**Ports courants à retenir pour l'ECF :**

```plaintext
TCP/UDP 53   → DNS
UDP 67-68    → DHCP
TCP 80       → HTTP
TCP 443      → HTTPS
TCP 22       → SSH
TCP 3389     → RDP (Remote Desktop)
TCP 389      → LDAP (Active Directory)
TCP 445      → SMB (Partage fichiers Windows)
TCP 3306     → MySQL
TCP 5432     → PostgreSQL
```

**Commande pour voir les ports ouverts :**

```cmd
netstat -an | findstr LISTENING    → Ports TCP en écoute
netstat -an | findstr ESTABLISHED  → Connexions actives
```

---

### Architecture réseau - Modèle OSI simplifié

**Les 7 couches (de bas en haut) :**

```plaintext
7 - Application  → HTTP, DNS, DHCP (ce que tu utilises)
6 - Présentation → Chiffrement SSL/TLS, compression
5 - Session      → Établir/maintenir connexions
4 - Transport    → TCP, UDP (fiabilité vs vitesse)
3 - Réseau       → IP, routage (adressage)
2 - Liaison      → MAC, switches (Ethernet, WiFi)
1 - Physique     → Câbles, signaux électriques

💡 Moyen mnémotechnique :
   A - All
   P - People
   S - Seem
   T - To
   N - Need
   D - Data
   P - Processing
```

**Encapsulation (comment ça marche vraiment) :**

```plaintext
Application : "Envoie ce fichier"
   ↓
Transport (TCP) : Découpe en segments + port 80
   ↓
Réseau (IP) : Ajoute IP source/dest
   ↓
Liaison (Ethernet) : Ajoute MAC source/dest
   ↓
Physique : Envoie sur le câble
```

---

## 🔧 Commandes à retenir (GUI + CMD simple)

### Réseau :

```cmd
ipconfig                  → Voir mon IP
ipconfig /all             → Voir TOUT (DNS, passerelle, etc.)
ipconfig /release         → Libérer mon IP DHCP
ipconfig /renew           → Redemander une IP DHCP
ping 192.168.10.10        → Tester si le serveur répond
nslookup ENTREPRISE.LOCAL → Tester le DNS
```

### Active Directory :

```cmd
systeminfo | findstr Domaine  → Voir si je suis dans un domaine
gpupdate /force               → Forcer la mise à jour des GPO
gpresult /R                   → Voir les GPO appliquées
whoami                        → Qui suis-je ? (ENTREPRISE\m.dupont)
```

---

## 📋 Checklist ECF complète

### Avant de commencer

```markdown
- [ ] VMware Workstation installé
- [ ] ISO Windows 10/11 Pro téléchargé
- [ ] ISO Windows Server 2019/2022 téléchargé
- [ ] Au moins 8 Go de RAM dispo sur ton PC
- [ ] 150 Go d'espace disque libre
```

### Partie 1 : Workstation

```markdown
- [ ] VM WIN10-CLIENT créée
- [ ] Windows 10/11 Pro installé
- [ ] Machine renommée
- [ ] IP configurée (DHCP ou fixe)
- [ ] Logiciels installés (Firefox, Chrome)
```

### Partie 2 : Disques et Sauvegarde

```markdown
- [ ] Disque D: (Backup) ajouté et formaté
- [ ] Image système créée
- [ ] Restauration testée avec succès
```

### Partie 3 : Active Directory

```markdown
- [ ] VM SRV-DC01 créée
- [ ] Windows Server installé (Desktop Experience)
- [ ] IP fixe configurée (192.168.10.10)
- [ ] Serveur renommé (SRV-DC01)
- [ ] AD DS installé
- [ ] Serveur promu en DC (domaine ENTREPRISE.LOCAL)
- [ ] DNS fonctionne
- [ ] DHCP installé et autorisé
- [ ] Étendue DHCP créée (100-200)
- [ ] OU créées (DIRECTION, COMPTA, RH)
- [ ] 3 utilisateurs créés
- [ ] 3 groupes créés
- [ ] GPO créée et liée
- [ ] Client joint au domaine
- [ ] Connexion avec utilisateur du domaine OK
```

### Screenshots

```markdown
- [ ] 20-22 screenshots pris
- [ ] Screenshots numérotés (01, 02, etc.)
- [ ] Screenshots lisibles
- [ ] PDF créé avec légendes
```

---

## 🎯 Les 10 erreurs à ÉVITER absolument

### 1. DNS mal configuré

```plaintext
❌ Client pointe vers 8.8.8.8 ou 192.168.1.1
✅ Client pointe vers 192.168.10.10 (le DC)

Sans ça, impossible de joindre le domaine !
```

### 2. Oublier de renommer avant AD DS

```plaintext
❌ Installer AD DS puis renommer
✅ Renommer PUIS installer AD DS

Renommer après AD DS = galère technique
```

### 3. Choisir "Standard" au lieu de "Desktop Experience"

```plaintext
❌ Windows Server Standard
✅ Windows Server Standard (Desktop Experience)

Sans Desktop Experience = pas d'interface graphique !
```

### 4. Utiliser un domaine .COM

```plaintext
❌ ENTREPRISE.COM (conflit avec Internet)
✅ ENTREPRISE.LOCAL (domaine interne)

Ou .LAN, .INTERNAL, .CORP
```

### 5. Oublier d'autoriser DHCP dans AD

```plaintext
Après installation DHCP :
- Cliquer sur "Complete DHCP configuration"
- Suivre l'assistant

Sinon, triangle jaune dans DHCP Manager !
```

### 6. Mettre le client et le serveur sur des réseaux différents

```plaintext
❌ Serveur sur VMnet2, Client sur VMnet8
✅ TOUS sur VMnet2

Ils doivent être sur le MÊME réseau virtuel !
```

### 7. Mot de passe trop simple

```plaintext
❌ password123
✅ P@ssw0rd123!

Requis :
- 8 caractères minimum
- Majuscule + minuscule
- Chiffre
- Caractère spécial
```

### 8. Créer les utilisateurs dans le mauvais OU

```plaintext
❌ Créer dans "Users" (OU par défaut)
✅ Créer dans DIRECTION, COMPTA, RH

Les GPO ne s'appliquent qu'aux bonnes OU !
```

### 9. Ne pas tester au fur et à mesure

```plaintext
❌ Tout faire d'un coup, tester à la fin
✅ Tester chaque étape :
   - DNS ok ? → ping, nslookup
   - DHCP ok ? → ipconfig /renew
   - Domaine ok ? → systeminfo
```

### 10. Oublier de prendre des screenshots

```plaintext
❌ Faire tout l'ECF puis se rendre compte qu'il manque des screenshots
✅ Prendre les screenshots AU FUR ET À MESURE

Note sur un papier : "Screenshot 5 : OK ✓"
```

---

## 💡 Astuces de rapidité

### Raccourcis clavier utiles

```plaintext
Win + X           → Menu rapide (Disk Management, etc.)
Win + R           → Exécuter (cmd, diskmgmt.msc, etc.)
Win + E           → Explorateur de fichiers
Win + I           → Paramètres Windows
Win + Pause       → Propriétés système (pour renommer)
Ctrl + Shift + Esc → Gestionnaire des tâches
```

### Snapshot VMware = Sauvegardes express

Avant chaque grosse étape :

```plaintext
1. VM → Snapshot → Take Snapshot
2. Nom : "Avant installation AD DS"
3. Si problème → VM → Snapshot → Revert

C'est comme un checkpoint dans un jeu vidéo !
```

### Organisation des screenshots

Pendant l'ECF, crée un dossier :

```plaintext
C:\ECF_Screenshots\
```

À chaque screenshot :

```plaintext
1. Win + Shift + S (Outil Capture d'écran)
2. Sélectionner la zone
3. Ctrl + V dans Paint
4. Enregistrer : 01_config_vm.png

Alternative : Outil Capture d'écran Windows (tapez "Capture" dans le menu Démarrer)
```

---

## 📊 Planning de l'ECF (3h)

| Temps | Tâche |
|-------|-------|
| 0:00 - 0:25 | Installation Win10 + config réseau |
| 0:25 - 0:40 | Ajout disque + formatage |
| 0:40 - 1:10 | Sauvegarde + restauration |
| 1:10 - 1:30 | Installation Win Server |
| 1:30 - 1:50 | Installation AD DS + Promotion DC |
| 1:50 - 2:05 | Configuration DHCP |
| 2:05 - 2:25 | Création OU + Users + Groupes |
| 2:25 - 2:40 | GPO + Join domain |
| 2:40 - 2:55 | Vérifications finales |
| 2:55 - 3:00 | Assemblage PDF |

**💡 Conseil** : Prends 5 min de pause toutes les heures !

---

## 📝 Structure du PDF à rendre

### Page de garde

```markdown
═══════════════════════════════════════
    ECF BLANC - PARTIE PRATIQUE
    WINDOWS SERVER & WORKSTATION
═══════════════════════════════════════

Candidat : [Ton Prénom Nom]
Formation : TSSR - Nextformation
Date : [Date de l'examen]
Durée : 3h00

═══════════════════════════════════════
```

### Structure du contenu

```markdown
TABLE DES MATIÈRES
1. Installation et configuration Workstation .......... p.2
2. Gestion de disques et sauvegarde ................... p.5
3. Active Directory - Infrastructure complète ......... p.8
4. Vérifications finales .............................. p.15

═══════════════════════════════════════════════════════

1. INSTALLATION ET CONFIGURATION WORKSTATION

1.1 Création de la VM
[Screenshot 1 : Configuration VM WIN10-CLIENT]
> Paramètres : 4 Go RAM, 2 CPU, 60 Go disque, VMnet2

1.2 Installation Windows 10 Pro
[Screenshot 2 : Bureau Windows ouvert]
> Version installée : Windows 10 Pro 21H2

1.3 Configuration réseau DHCP
[Screenshot 3 : ipconfig /all]
> IP obtenue : 192.168.x.x
> Passerelle : 192.168.x.2

[... etc pour tous les screenshots ...]
```

### Conseils pour un PDF pro

```markdown
✅ Numéroter les pages
✅ Mettre un titre à chaque screenshot
✅ Ajouter une légende explicative
✅ Utiliser une police lisible (Arial, Calibri)
✅ Taille de police : 11-12pt minimum
✅ Exporter en PDF (pas Word ou PowerPoint)

Outil recommandé : Word ou LibreOffice Writer → Exporter en PDF
```

---

## 🎓 Pour aller plus loin

### Après l'ECF

Si tu veux approfondir :

1. **Créer des utilisateurs en masse**
   - Utiliser un fichier CSV
   - Importer dans AD via PowerShell (tu verras ça plus tard)

2. **GPO avancées**
   - Bloquer l'accès au Panneau de configuration
   - Mapper des lecteurs réseau automatiquement
   - Déployer des logiciels via GPO

3. **Sécurité**
   - Stratégies de mot de passe complexes
   - Verrouillage de compte après X tentatives
   - Audit des connexions

4. **DHCP avancé**
   - Réservations DHCP (toujours la même IP pour une machine)
   - Failover DHCP (2 serveurs DHCP en redondance)

### Ressources

- Documentation Microsoft : https://docs.microsoft.com/fr-fr/windows-server/
- Formip : Tes exercices quotidiens
- Professor Messer : Vidéos CCNA/Network+ (YouTube)
- Ces guides : À relire avant l'ECF !

---

## 💪 Message de motivation

**Tu as tout ce qu'il faut pour réussir cet ECF !**

Rappelle-toi :

- 🧠 **Comprendre > Mémoriser** : Pose-toi toujours la question "pourquoi ?"
- 🔄 **Pratique > Théorie** : Refais ces manips 2-3 fois avant l'ECF
- 📸 **Screenshots = Preuve** : Capture TOUT au fur et à mesure
- 💾 **Snapshots = Sécurité** : Avant chaque grosse étape

Le jour J :

- 😌 Reste calme, tu connais la procédure
- ⏱️ Gère ton temps (check la checklist toutes les 30 min)
- ✅ Vérifie que chaque étape fonctionne avant de passer à la suivante
- 🆘 Si blocage : snapshot → revenir en arrière → réessayer

**Tu vas assurer ! 🚀**

---

## 📝 Checklist finale avant l'ECF

### La veille :

```markdown
- [ ] Relire ce guide (au moins les titres)
- [ ] Vérifier que VMware fonctionne
- [ ] Vérifier que tu as les ISO
- [ ] Préparer un dossier pour les screenshots
- [ ] Dormir tôt (important !)
```

### Le jour J :

```markdown
- [ ] Arriver 10 min en avance
- [ ] Ouvrir ce guide en PDF sur ton téléphone ou imprimé
- [ ] Respirer un coup
- [ ] C'est parti !
```

### Pendant l'ECF :

```markdown
- [ ] Lire TOUTE la consigne avant de commencer
- [ ] Suivre l'ordre du guide
- [ ] Prendre les screenshots au fur et à mesure
- [ ] Tester chaque étape avant de continuer
- [ ] Garder 15 min pour le PDF à la fin
```

---

**Good luck ! Tu vas cartonner ! 💪🔥**

*Guide créé pour la préparation ECF TSSR - Nextformation 2025-2026*
