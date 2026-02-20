# SSH - Connexion à distance sécurisée

> 📚 **Module :** Linux Administration - Administration distante
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 3 heures
> 🎯 **Niveau :** Intermédiaire (N2)
> 👨‍🏫 **Approche :** Admin système → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [SSH - Les bases](#-ssh---les-bases)
- [Authentification par clés](#-authentification-par-clés)
- [Configuration SSH client](#-configuration-ssh-client)
- [Transfert de fichiers](#-transfert-de-fichiers)
- [SSH avancé](#-ssh-avancé)
- [Sécurisation SSH](#-sécurisation-ssh)
- [Troubleshooting SSH](#-troubleshooting-ssh)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**Je gère 50 serveurs Linux.** Ils sont répartis dans 12 datacenters différents en France et en Europe.

**Question :** Comment je les administre ?

**Réponse :** **Depuis mon salon, en pyjama, avec SSH.**

SSH (**S**ecure **SH**ell) est **l'outil n°1** de tout admin système Linux. Je l'utilise **tous les jours, plusieurs dizaines de fois par jour**, depuis 15 ans.

### 🎯 Pourquoi SSH est magique

**Scénario réel (2019) :**

Vendredi 18h, je suis en vacances à Marseille. Mon téléphone sonne.

**"Le serveur web de prod à Paris ne répond plus !"**

- Je sors mon laptop
- Je me connecte en 4G
- `ssh prod-web-01`
- Je diagnostique et corrige en 10 minutes
- Je retourne à la plage

**Sans SSH ?** Il aurait fallu :
- Trouver quelqu'un sur place à Paris
- Lui donner accès physique au datacenter
- Le guider au téléphone
- Perdre 3 heures minimum

**Avec SSH ?** 10 minutes depuis n'importe où dans le monde. 🌍

### 🎯 Ce que vous allez apprendre

Dans ce cours, vous allez maîtriser :
- ✅ La connexion SSH de base
- ✅ L'authentification par clés (plus JAMAIS de mot de passe à taper)
- ✅ Le transfert de fichiers sécurisé
- ✅ Les tunnels SSH (accéder à des services distants)
- ✅ La sécurisation d'un serveur SSH

Allez, on y va ! 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Se connecter** à un serveur distant en SSH
- ✅ **Configurer** l'authentification par clés SSH
- ✅ **Transférer** des fichiers avec scp et rsync
- ✅ **Créer** des tunnels SSH pour accéder à des services distants
- ✅ **Sécuriser** un serveur SSH contre les attaques
- ✅ **Diagnostiquer** des problèmes de connexion SSH

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les commandes de base Linux
- [ ] Comprendre les notions de réseau (IP, port)
- [ ] Avoir accès à au moins 2 machines Linux (ou VM)

**Matériel nécessaire :**
- 💻 2 machines Linux (ou 1 local + 1 serveur distant)
- 🌐 Connexion réseau entre les machines
- 📝 Terminal

---

## 🔐 SSH - Les bases

### Qu'est-ce que SSH ?

**SSH = Secure Shell**

```
┌─────────────────────────────────────────────────────────────┐
│  AVANT SSH : Telnet (années 1980-2000)                      │
├─────────────────────────────────────────────────────────────┤
│  • Connexion en CLAIR (non chiffré)                         │
│  • Mots de passe visibles sur le réseau                     │
│  • N'importe qui peut intercepter                           │
│  • DANGEREUX et OBSOLÈTE                                    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  AVEC SSH (depuis 1995)                                     │
├─────────────────────────────────────────────────────────────┤
│  • Connexion CHIFFRÉE                                       │
│  • Authentification sécurisée (clés + mots de passe)        │
│  • Impossible à intercepter                                 │
│  • STANDARD moderne                                         │
└─────────────────────────────────────────────────────────────┘
```

### Connexion SSH de base

**Syntaxe :**

```bash
ssh utilisateur@serveur
```

**Exemples :**

```bash
# Avec nom d'hôte
ssh john@serveur.example.com

# Avec adresse IP
ssh john@192.168.1.100

# Port non standard
ssh -p 2222 john@serveur.example.com

# Avec commande directe (sans ouvrir un shell)
ssh john@serveur.example.com "uptime"
ssh john@serveur.example.com "df -h"
```

### Première connexion

**Ce qui se passe lors de la première connexion :**

```bash
$ ssh john@192.168.1.100

The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '192.168.1.100' (ECDSA) to the list of known hosts.
john@192.168.1.100's password:
```

**Décryptage :**

1. **Fingerprint** = Empreinte unique du serveur (comme une empreinte digitale)
2. Tapez `yes` pour accepter
3. L'empreinte est sauvée dans `~/.ssh/known_hosts`
4. Tapez le mot de passe

**Pourquoi cette étape ?**

Protection contre les attaques **Man-In-The-Middle** (MITM) :
- Si quelqu'un tente d'intercepter, le fingerprint sera différent
- SSH vous alertera : "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!"

### known_hosts

**Fichier contenant les empreintes des serveurs connus :**

```bash
cat ~/.ssh/known_hosts
```

**Exemple :**

```
192.168.1.100 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTY...
serveur.example.com ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC...
```

**Supprimer une entrée (si fingerprint changé) :**

```bash
ssh-keygen -R 192.168.1.100
ssh-keygen -R serveur.example.com
```

### Se déconnecter

**3 méthodes :**

```bash
exit            # Commande exit
logout          # Commande logout
Ctrl+D          # Raccourci clavier (le plus rapide)
```

---

## 🔑 Authentification par clés

### Pourquoi les clés SSH ?

```
┌─────────────────────────────────────────────────────────────┐
│  AUTHENTIFICATION PAR MOT DE PASSE                          │
├─────────────────────────────────────────────────────────────┤
│  ❌ Il faut le taper à chaque fois                           │
│  ❌ Peut être bruteforcé                                     │
│  ❌ Peut être intercepté (keylogger)                         │
│  ❌ Difficile à gérer sur 50 serveurs                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  AUTHENTIFICATION PAR CLÉ                                   │
├─────────────────────────────────────────────────────────────┤
│  ✅ Connexion automatique (sans taper de mot de passe)       │
│  ✅ Beaucoup plus sécurisé                                   │
│  ✅ Impossible à bruteforcer                                 │
│  ✅ Une clé pour tous les serveurs                           │
└─────────────────────────────────────────────────────────────┘
```

### Le système de clés publique/privée

**Analogie de la boîte aux lettres :**

```
┌─────────────────────────────────────────────────────────────┐
│  CLÉ PUBLIQUE = Boîte aux lettres                          │
│  • Tout le monde peut y DÉPOSER du courrier                 │
│  • Mais seul le propriétaire peut OUVRIR                    │
│  • On peut la copier partout sans risque                    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  CLÉ PRIVÉE = Clé de la boîte aux lettres                  │
│  • Seul le propriétaire la possède                          │
│  • JAMAIS la partager ou la copier                          │
│  • Permet de LIRE le courrier de la boîte                   │
└─────────────────────────────────────────────────────────────┘
```

**Processus d'authentification :**

```
1. Vous générez une PAIRE de clés (publique + privée)
2. Vous copiez la clé PUBLIQUE sur le serveur
3. Quand vous vous connectez :
   - Le serveur vous envoie un défi chiffré avec votre clé publique
   - Seul vous (avec la clé privée) pouvez déchiffrer
   - Vous renvoyez la réponse
   - Connexion acceptée !
```

### Générer une paire de clés

**Commande :**

```bash
ssh-keygen
```

**Processus interactif :**

```bash
$ ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/home/john/.ssh/id_rsa): [Entrée]
Enter passphrase (empty for no passphrase): [Tapez une passphrase ou Entrée]
Enter same passphrase again: [Retapez la passphrase]

Your identification has been saved in /home/john/.ssh/id_rsa
Your public key has been saved in /home/john/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx john@laptop
```

**Résultat :**

```bash
ls -l ~/.ssh/
-rw------- 1 john john 2602 Feb  9 16:00 id_rsa        # CLÉ PRIVÉE
-rw-r--r-- 1 john john  570 Feb  9 16:00 id_rsa.pub    # CLÉ PUBLIQUE
```

> ⚠️ **IMPORTANT :** La clé privée (`id_rsa`) doit TOUJOURS avoir les permissions `600` (lecture/écriture propriétaire uniquement) !

**Générer avec options :**

```bash
# Type de clé plus moderne (Ed25519)
ssh-keygen -t ed25519 -C "john@laptop"

# RSA avec taille spécifique
ssh-keygen -t rsa -b 4096 -C "john@laptop"
```

**Types de clés :**

```
RSA       # Ancien mais compatible partout (2048 ou 4096 bits)
Ed25519   # Moderne, plus rapide, plus sécurisé (recommandé)
ECDSA     # Moderne
```

> 💡 **Conseil :** Utilisez Ed25519 sauf si vous devez vous connecter à des vieux serveurs.

### Copier la clé publique sur le serveur

**Méthode automatique (recommandée) :**

```bash
ssh-copy-id user@serveur
```

**Exemple :**

```bash
$ ssh-copy-id john@192.168.1.100

/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s)
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed
john@192.168.1.100's password: [Tapez votre mot de passe]

Number of key(s) added: 1

Now try logging into the machine with "ssh john@192.168.1.100"
and check to make sure that only the key(s) you wanted were added.
```

**Test :**

```bash
ssh john@192.168.1.100
# Connexion SANS demander le mot de passe ! 🎉
```

**Méthode manuelle (si ssh-copy-id pas disponible) :**

```bash
# Sur votre machine locale
cat ~/.ssh/id_rsa.pub

# Copier le contenu

# Sur le serveur distant
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
# Coller la clé
chmod 600 ~/.ssh/authorized_keys
```

### Passphrase de la clé privée

**Si vous avez mis une passphrase :**

```bash
$ ssh john@serveur
Enter passphrase for key '/home/john/.ssh/id_rsa': [Taper passphrase]
```

**Avantages de la passphrase :**
- ✅ Même si on vole votre clé privée, elle est inutilisable sans la passphrase
- ✅ Protection supplémentaire

**Inconvénient :**
- ❌ Il faut la taper à chaque connexion

**Solution : ssh-agent**

### ssh-agent - Ne plus retaper la passphrase

**Démarrer ssh-agent :**

```bash
eval $(ssh-agent)
```

**Ajouter la clé :**

```bash
ssh-add ~/.ssh/id_rsa
# Enter passphrase for /home/john/.ssh/id_rsa: [Taper UNE FOIS]
```

**Maintenant :**

```bash
ssh john@serveur
# Connexion directe, SANS demander la passphrase ! 🎉
```

**Astuce : Auto-démarrage**

Ajouter dans `~/.bashrc` ou `~/.zshrc` :

```bash
if [ -z "$SSH_AUTH_SOCK" ]; then
   eval $(ssh-agent -s) > /dev/null
   ssh-add ~/.ssh/id_rsa 2>/dev/null
fi
```

---

## ⚙️ Configuration SSH client

### Le fichier ~/.ssh/config

**Permet de créer des alias et configurations personnalisées.**

**Exemple de base :**

```bash
nano ~/.ssh/config
```

**Contenu :**

```
Host prod-web
    HostName 192.168.1.100
    User john
    Port 22
    IdentityFile ~/.ssh/id_rsa

Host dev-db
    HostName dev-db.example.com
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

**Utilisation :**

```bash
# Au lieu de :
ssh john@192.168.1.100

# Vous tapez :
ssh prod-web
```

**Beaucoup plus simple !** 🚀

### Exemple avancé

```
# Serveur de production
Host prod-*
    User admin
    Port 2222
    IdentityFile ~/.ssh/prod_key
    StrictHostKeyChecking yes

Host prod-web
    HostName 192.168.1.100

Host prod-db
    HostName 192.168.1.101

# Serveurs de développement
Host dev-*
    User developer
    Port 22
    IdentityFile ~/.ssh/dev_key
    StrictHostKeyChecking no

Host dev-app
    HostName 10.0.0.50

# Serveur avec rebond (ProxyJump)
Host internal-server
    HostName 10.0.10.50
    User john
    ProxyJump bastion.example.com

# Tous les serveurs (wildcard)
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    Compression yes
```

**Options utiles :**

```
ServerAliveInterval 60    # Envoie un ping toutes les 60s (évite timeout)
ServerAliveCountMax 3     # Déconnecte après 3 pings sans réponse
Compression yes           # Active la compression (utile sur connexion lente)
StrictHostKeyChecking no  # Ne vérifie pas le fingerprint (DANGEREUX en prod !)
ForwardAgent yes          # Permet d'utiliser vos clés SSH à travers le serveur
```

---

## 📁 Transfert de fichiers

### scp - Secure Copy

**Syntaxe :**

```bash
scp source destination
```

**Du local vers le distant :**

```bash
# Copier un fichier
scp fichier.txt john@serveur:/tmp/

# Copier un dossier (récursif)
scp -r dossier/ john@serveur:/backup/

# Avec port non standard
scp -P 2222 fichier.txt john@serveur:/tmp/
```

**Du distant vers le local :**

```bash
# Copier un fichier
scp john@serveur:/var/log/app.log /tmp/

# Copier un dossier
scp -r john@serveur:/var/www/html/ /backup/local/
```

**Entre deux serveurs distants :**

```bash
scp john@serveur1:/data/file.txt admin@serveur2:/backup/
```

**Options utiles :**

```bash
-r      # Récursif (pour les dossiers)
-P      # Port (ATTENTION : majuscule pour scp, minuscule pour ssh)
-v      # Verbose (affiche les détails)
-C      # Compression
-p      # Préserve dates et permissions
-q      # Quiet (silencieux)
```

**Exemple complet :**

```bash
# Copier avec compression, préservation et verbose
scp -rCpv /var/www/html/ john@backup:/backups/www/
```

### rsync - Synchronisation puissante

**rsync est BEAUCOUP plus puissant que scp :**

```
┌─────────────────────────────────────────────────────────────┐
│  scp                                                        │
├─────────────────────────────────────────────────────────────┤
│  • Copie TOUT à chaque fois                                 │
│  • Pas de reprise en cas d'interruption                     │
│  • Simple mais basique                                      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  rsync                                                      │
├─────────────────────────────────────────────────────────────┤
│  • Copie SEULEMENT ce qui a changé (delta sync)             │
│  • Reprise en cas d'interruption                            │
│  • Plein d'options puissantes                               │
│  • Peut supprimer ce qui n'existe plus (--delete)           │
└─────────────────────────────────────────────────────────────┘
```

**Syntaxe de base :**

```bash
rsync -avz source/ destination/
```

**Options importantes :**

```
-a    # Archive (préserve tout : permissions, dates, liens, etc.)
-v    # Verbose
-z    # Compression
-h    # Human-readable (tailles en Ko, Mo, Go)
-P    # Progress + reprise
--delete    # Supprime les fichiers qui n'existent plus dans la source
--dry-run   # Simulation (ne fait rien, affiche ce qui serait fait)
```

**Exemples pratiques :**

**1. Synchroniser un site web vers un serveur :**

```bash
rsync -avzh /var/www/html/ john@serveur:/var/www/html/
```

**2. Backup avec suppression des fichiers obsolètes :**

```bash
rsync -avzh --delete /data/ john@backup:/backups/data/
```

**3. Test avant de vraiment faire :**

```bash
rsync -avzh --delete --dry-run /data/ john@backup:/backups/data/
# Vérifie ce qui serait fait

rsync -avzh --delete /data/ john@backup:/backups/data/
# Lance vraiment la synchro
```

**4. Avec barre de progression :**

```bash
rsync -avzhP /gros-fichier.iso john@serveur:/tmp/
```

**5. Exclure certains fichiers :**

```bash
rsync -avzh --exclude='*.log' --exclude='tmp/' /data/ john@backup:/backups/
```

**Astuce : Slash ou pas slash ?**

```bash
rsync -avz /source/ destination/    # Copie le CONTENU de source/
rsync -avz /source destination/     # Copie le DOSSIER source/ lui-même

# Exemple :
rsync -avz /var/www/ backup/
# Résultat : backup/html, backup/logs

rsync -avz /var/www backup/
# Résultat : backup/www/html, backup/www/logs
```

> 💡 **Mon usage quotidien :** J'utilise rsync pour toutes mes sauvegardes et synchronisations.

---

## 🚀 SSH avancé

### Tunnels SSH (Port Forwarding)

**Permet d'accéder à des services distants de façon sécurisée.**

#### Local Port Forwarding

**Cas d'usage :** Accéder à une base MySQL distante qui n'accepte que les connexions locales.

**Problème :**

```
Vous : 192.168.1.50
Serveur : 192.168.1.100
MySQL sur serveur : Écoute SEULEMENT sur 127.0.0.1:3306 (localhost)

→ Vous ne pouvez PAS vous connecter directement à MySQL depuis votre PC
```

**Solution : Tunnel SSH**

```bash
ssh -L 3307:localhost:3306 john@192.168.1.100
```

**Décryptage :**

```
-L 3307:localhost:3306
   │     │         │
   │     │         └─ Port MySQL sur le serveur
   │     └─ localhost vu DEPUIS le serveur
   └─ Port local sur VOTRE machine
```

**Résultat :**

```
Votre PC:3307 → [Tunnel SSH] → Serveur → localhost:3306 (MySQL)
```

**Utilisation :**

```bash
# Dans un autre terminal
mysql -h 127.0.0.1 -P 3307 -u root -p
# Vous êtes connecté à la MySQL distante ! 🎉
```

**Autre exemple : Accéder à une interface web interne**

```bash
ssh -L 8080:localhost:80 john@serveur
```

Puis dans le navigateur : `http://localhost:8080`

#### Remote Port Forwarding

**Cas d'usage :** Exposer un service local vers un serveur distant.

**Exemple :**

```bash
ssh -R 8080:localhost:3000 john@serveur
```

**Résultat :**

```
serveur:8080 → [Tunnel SSH] → Votre PC:3000
```

**Usage :** Montrer une app en développement (tournant sur votre PC port 3000) à un collègue qui se connecte au serveur.

#### Dynamic Port Forwarding (SOCKS Proxy)

**Créer un proxy SOCKS pour router tout le trafic par SSH :**

```bash
ssh -D 1080 john@serveur
```

**Configuration dans le navigateur :**
- Proxy SOCKS5
- Hôte : localhost
- Port : 1080

**Résultat :** Tout votre trafic web passe par le serveur (utile pour contourner des restrictions réseau).

### ProxyJump - Rebond SSH

**Cas d'usage :** Serveur accessible seulement via un bastion.

```
Vous → [Bastion] → Serveur interne
```

**Méthode classique (2 connexions) :**

```bash
ssh user@bastion
# Puis une fois connecté :
ssh user@serveur-interne
```

**Méthode moderne (1 commande) :**

```bash
ssh -J user@bastion user@serveur-interne
```

**Dans ~/.ssh/config :**

```
Host bastion
    HostName bastion.example.com
    User admin

Host serveur-interne
    HostName 10.0.10.50
    User john
    ProxyJump bastion
```

**Utilisation :**

```bash
ssh serveur-interne
# Connexion automatique via bastion ! 🚀
```

### X11 Forwarding

**Lancer des applications graphiques à distance :**

```bash
ssh -X john@serveur
```

**Puis sur le serveur :**

```bash
firefox &
gedit fichier.txt &
```

**Les fenêtres s'affichent sur VOTRE PC !**

> 💡 **Note :** Nécessite un serveur X sur votre machine (Linux/macOS OK, Windows nécessite VcXsrv ou Xming).

---

## 🔒 Sécurisation SSH

### Configuration du serveur SSH

**Fichier : `/etc/ssh/sshd_config`**

**Modifications recommandées :**

```bash
sudo nano /etc/ssh/sshd_config
```

**1. Changer le port par défaut**

```
# Défaut
Port 22

# Changé (exemple)
Port 2222
```

**Pourquoi ?** Les bots scannent massivement le port 22. Un port non standard réduit drastiquement les tentatives d'intrusion.

**2. Désactiver le login root**

```
PermitRootLogin no
```

**Pourquoi ?** Oblige les attaquants à deviner 2 choses : le login ET le mot de passe.

**3. Désactiver l'authentification par mot de passe**

```
PasswordAuthentication no
```

**Pourquoi ?** Force l'utilisation de clés SSH (impossible à bruteforcer).

> ⚠️ **ATTENTION :** Faites ça APRÈS avoir configuré vos clés SSH ! Sinon vous ne pourrez plus vous connecter !

**4. Autoriser seulement certains utilisateurs**

```
AllowUsers john alice
```

**Ou autoriser seulement un groupe :**

```
AllowGroups ssh-users
```

**5. Timeout des connexions inactives**

```
ClientAliveInterval 300
ClientAliveCountMax 2
```

**Traduction :** Déconnexion après 10 minutes d'inactivité (300s × 2).

**6. Limiter les tentatives de connexion**

```
MaxAuthTries 3
```

**Configuration complète sécurisée :**

```
# Port non standard
Port 2222

# Protocole
Protocol 2

# Root interdit
PermitRootLogin no

# Authentification par clé obligatoire
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no

# Utilisateurs autorisés
AllowUsers john alice

# Timeout
ClientAliveInterval 300
ClientAliveCountMax 2

# Tentatives limitées
MaxAuthTries 3

# X11 et autres
X11Forwarding no
AllowTcpForwarding no
```

**Redémarrer SSH après modif :**

```bash
sudo systemctl restart sshd
# Ou
sudo systemctl restart ssh
```

> ⚠️ **IMPORTANT :** Gardez une connexion SSH ouverte pendant que vous testez les modifs ! Si ça ne marche pas, vous pourrez corriger.

### fail2ban - Protection contre le bruteforce

**Installer fail2ban :**

```bash
sudo apt install fail2ban        # Debian/Ubuntu
sudo yum install fail2ban        # RedHat/CentOS
```

**Configuration :**

```bash
sudo nano /etc/fail2ban/jail.local
```

**Contenu :**

```ini
[DEFAULT]
bantime = 3600            # Bannir pendant 1 heure
findtime = 600            # Fenêtre de détection : 10 min
maxretry = 3              # 3 tentatives max

[sshd]
enabled = true
port = 2222               # Si port changé
logpath = /var/log/auth.log
```

**Démarrer fail2ban :**

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

**Vérifier les bannissements :**

```bash
sudo fail2ban-client status sshd
```

**Résultat :**

```
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     15
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 2
   |- Total banned:     5
   `- Banned IP list:   192.168.1.50 203.0.113.10
```

**Débannir une IP :**

```bash
sudo fail2ban-client set sshd unbanip 192.168.1.50
```

### Whitelist IP (pare-feu)

**Autoriser SEULEMENT certaines IP :**

```bash
# ufw (Ubuntu/Debian)
sudo ufw allow from 192.168.1.0/24 to any port 2222
sudo ufw deny 2222
sudo ufw enable

# iptables (générique)
sudo iptables -A INPUT -p tcp -s 192.168.1.0/24 --dport 2222 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 2222 -j DROP
```

---

## 🔧 Troubleshooting SSH

### Connexion refusée

**Erreur :**

```
ssh: connect to host 192.168.1.100 port 22: Connection refused
```

**Causes possibles :**

1. **SSH n'est pas installé ou pas démarré**

```bash
# Sur le serveur
sudo systemctl status ssh
sudo systemctl start ssh
```

2. **Mauvais port**

```bash
# Vérifier le port d'écoute sur le serveur
sudo ss -tulpn | grep ssh
# tcp  LISTEN  0  128  0.0.0.0:2222  0.0.0.0:*  users:(("sshd",pid=850))

# Se connecter avec le bon port
ssh -p 2222 user@serveur
```

3. **Pare-feu bloqué**

```bash
# Sur le serveur
sudo ufw status
sudo ufw allow 22
```

### Timeout

**Erreur :**

```
ssh: connect to host 192.168.1.100 port 22: Connection timed out
```

**Causes :**

1. Serveur éteint ou injoignable
2. Pare-feu bloquant (côté serveur ou réseau)
3. Mauvaise IP

**Diagnostic :**

```bash
# Tester la connectivité réseau
ping 192.168.1.100

# Tester si le port est ouvert
nc -zv 192.168.1.100 22
telnet 192.168.1.100 22
```

### Permission denied (publickey)

**Erreur :**

```
Permission denied (publickey).
```

**Causes :**

1. **Clé publique pas copiée sur le serveur**

```bash
ssh-copy-id user@serveur
```

2. **Mauvaises permissions**

```bash
# Sur le serveur, vérifier :
ls -ld ~/.ssh
# drwx------ (700)

ls -l ~/.ssh/authorized_keys
# -rw------- (600)

# Corriger si besoin
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

3. **SELinux (RedHat/CentOS)**

```bash
sudo restorecon -R -v ~/.ssh
```

### Mode verbeux pour diagnostiquer

**Connexion avec détails :**

```bash
ssh -v user@serveur         # Verbeux niveau 1
ssh -vv user@serveur        # Verbeux niveau 2
ssh -vvv user@serveur       # Verbeux niveau 3 (très détaillé)
```

**Permet de voir exactement où ça bloque !**

---

## 🎯 Exercices pratiques

### Exercice 1 : Configuration complète

**Objectif :** Configurer SSH avec authentification par clés.

**Consignes :**

1. Générer une paire de clés SSH
2. Copier la clé publique sur un serveur
3. Se connecter sans mot de passe
4. Créer un alias dans ~/.ssh/config

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Générer la clé
ssh-keygen -t ed25519 -C "john@laptop"
# Appuyer sur Entrée pour accepter les valeurs par défaut

# 2. Copier sur le serveur
ssh-copy-id john@192.168.1.100
# Taper le mot de passe

# 3. Tester la connexion
ssh john@192.168.1.100
# ✅ Connexion sans mot de passe !

# 4. Créer l'alias
nano ~/.ssh/config

# Ajouter :
Host myserver
    HostName 192.168.1.100
    User john
    IdentityFile ~/.ssh/id_ed25519

# Tester
ssh myserver
# ✅ Connexion avec l'alias !
```

</details>

---

### Exercice 2 : Tunnel SSH pour MySQL

**Objectif :** Accéder à une base MySQL distante via un tunnel.

**Scénario :**
- Serveur : 192.168.1.100
- MySQL écoute sur localhost:3306 (pas accessible de l'extérieur)
- Vous voulez vous connecter depuis votre PC

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer le tunnel
ssh -L 3307:localhost:3306 john@192.168.1.100

# 2. Dans un autre terminal, se connecter à MySQL
mysql -h 127.0.0.1 -P 3307 -u root -p
# Taper le mot de passe MySQL

# Vous êtes connecté à la MySQL distante !

# Alternative : En background
ssh -fN -L 3307:localhost:3306 john@192.168.1.100
# -f : background
# -N : pas de commande (juste le tunnel)
```

</details>

---

### Exercice 3 : Sécurisation complète

**Objectif :** Sécuriser un serveur SSH.

**Consignes :**

1. Changer le port SSH vers 2222
2. Désactiver le login root
3. Désactiver l'authentification par mot de passe
4. Installer fail2ban

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Modifier la configuration SSH
sudo nano /etc/ssh/sshd_config

# Modifier/ajouter :
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

# 2. Tester la configuration
sudo sshd -t
# (Aucun message = OK)

# 3. Redémarrer SSH (ATTENTION : gardez une connexion ouverte !)
sudo systemctl restart sshd

# 4. Tester la nouvelle connexion (dans un autre terminal)
ssh -p 2222 user@serveur
# ✅ OK

# 5. Installer fail2ban
sudo apt install fail2ban

# 6. Configurer fail2ban
sudo nano /etc/fail2ban/jail.local

[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 3

[sshd]
enabled = true
port = 2222
logpath = /var/log/auth.log

# 7. Démarrer fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# 8. Vérifier
sudo fail2ban-client status sshd
```

</details>

---

## 📚 Ressources

### Documentation officielle

- [OpenSSH Manual](https://www.openssh.com/manual.html)
- [SSH Config File](https://www.ssh.com/academy/ssh/config)
- [fail2ban Documentation](https://www.fail2ban.org/)

### Tutoriels

- [SSH Academy](https://www.ssh.com/academy/ssh)
- [DigitalOcean SSH Tutorials](https://www.digitalocean.com/community/tags/ssh)

### Outils

- [SSH Key Generator](https://8gwifi.org/sshfunctions.jsp)
- [SSH Config Generator](https://www.ssh-config.com/)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Mes serveurs SSH :**
-
-

**Alias à créer :**
-
-

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Je sais me connecter en SSH à un serveur
- [ ] Je maîtrise l'authentification par clés SSH
- [ ] Je sais créer des alias dans ~/.ssh/config
- [ ] Je peux transférer des fichiers avec scp et rsync
- [ ] Je comprends les tunnels SSH (Local Port Forwarding)
- [ ] Je sais sécuriser un serveur SSH
- [ ] Je peux diagnostiquer des problèmes de connexion SSH

---

<div align="center">

**Cours suivant :** [05-logs-depannage-troubleshooting.md](05-logs-depannage-troubleshooting.md)

[⬅️ Retour au sommaire](README.md)

</div>
