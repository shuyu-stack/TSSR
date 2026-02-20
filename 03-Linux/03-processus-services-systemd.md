# Processus, Services et systemd - Le moteur de Linux

> 📚 **Module :** Linux Administration - Gestion des services
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Intermédiaire (N2)
> 👨‍🏫 **Approche :** Admin système → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Comprendre les processus](#-comprendre-les-processus)
- [Gestion des processus](#-gestion-des-processus)
- [systemd - Le gestionnaire de services](#-systemd---le-gestionnaire-de-services)
- [journalctl - Les logs systemd](#-journalctl---les-logs-systemd)
- [Créer un service personnalisé](#-créer-un-service-personnalisé)
- [TP Pratique : Service custom](#-tp-pratique--service-custom)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**Jeudi, 9h30.** Je bois mon café tranquillement. Mon téléphone sonne.

**Client :** "Le serveur est ULTRA LENT ! Plus personne ne peut travailler !"

Je me connecte en SSH. Je tape **une commande** :

```bash
top
```

**2 secondes pour identifier le problème :**

```
PID   USER  %CPU  %MEM  COMMAND
12847 www   99.8  45.2  /usr/bin/php malicious_script.php
```

Un processus PHP fou consomme **99,8% du CPU**. Je le tue :

```bash
kill -9 12847
```

**Serveur sauvé. Temps total : 2 minutes.**

Le client : "Comment tu as fait si vite ?!"

### 🎯 La réponse

Parce que je **maîtrise la gestion des processus**. Je sais :
- Comment voir ce qui tourne sur le système
- Comment identifier un processus problématique
- Comment le stopper proprement (ou brutalement si besoin)
- Comment gérer les services pour qu'ils démarrent automatiquement

**Ces compétences, vous allez les acquérir dans ce cours.**

Dans 4 heures, vous serez capable de diagnostiquer et résoudre 80% des problèmes de performance serveur. 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Comprendre** ce qu'est un processus Linux
- ✅ **Lister** et analyser les processus en cours
- ✅ **Gérer** les processus (kill, nice, bg/fg)
- ✅ **Maîtriser** systemd et systemctl
- ✅ **Créer** un service systemd personnalisé
- ✅ **Analyser** les logs avec journalctl
- ✅ **Dépanner** un service qui refuse de démarrer
- ✅ **Diagnostiquer** un serveur qui rame

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les commandes de base Linux
- [ ] Savoir utiliser sudo
- [ ] Comprendre les permissions de base
- [ ] Avoir un système Linux avec systemd (Ubuntu 16.04+, Debian 8+, CentOS 7+)

**Matériel nécessaire :**
- 💻 Linux avec systemd (VM Ubuntu/Debian recommandé)
- 🔑 Accès sudo
- 📝 Terminal ouvert

---

## 🔧 Comprendre les processus

### Qu'est-ce qu'un processus ?

**Définition simple :** Un processus = un programme en cours d'exécution.

```
┌─────────────────────────────────────────────────────────────┐
│  PROGRAMME vs PROCESSUS                                     │
├─────────────────────────────────────────────────────────────┤
│  Programme = Fichier sur le disque                          │
│    Exemple : /usr/bin/firefox                               │
│                                                             │
│  Processus = Programme chargé en mémoire et exécuté         │
│    Exemple : firefox (PID 1234) qui tourne actuellement     │
└─────────────────────────────────────────────────────────────┘
```

**Analogie :**
- Programme = Recette de cuisine (livre)
- Processus = Plat en train d'être cuisiné

### Anatomie d'un processus

Chaque processus a :

```
┌─────────────────────────────────────────────────────────────┐
│  PID (Process ID)                                           │
│  = Numéro unique (comme une carte d'identité)               │
│  Exemple : 1234                                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  PPID (Parent Process ID)                                   │
│  = PID du processus parent qui l'a lancé                    │
│  Exemple : bash (PID 1000) lance firefox (PID 1234)         │
│            → firefox a PPID = 1000                          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  USER                                                       │
│  = Utilisateur qui a lancé le processus                     │
│  Exemple : john, root, www-data                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  %CPU                                                       │
│  = Pourcentage d'utilisation du CPU                         │
│  Exemple : 25.3 = utilise 25,3% du CPU                      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  %MEM                                                       │
│  = Pourcentage d'utilisation de la RAM                      │
│  Exemple : 5.2 = utilise 5,2% de la RAM                     │
└─────────────────────────────────────────────────────────────┘
```

### L'arbre des processus

**Tous les processus sont liés entre eux :**

```
systemd (PID 1)  ← Le PÈRE de tous les processus
│
├── sshd (PID 850)
│   └── sshd (PID 1200)  ← Session de john
│       └── bash (PID 1201)
│           ├── vim (PID 1250)
│           └── python script.py (PID 1300)
│
├── apache2 (PID 900)
│   ├── apache2 (PID 901)
│   ├── apache2 (PID 902)
│   └── apache2 (PID 903)
│
└── cron (PID 700)
```

**Visualiser l'arbre :**

```bash
pstree
pstree -p               # Avec les PID
pstree -p john          # Arbre d'un utilisateur
```

---

## 📊 Gestion des processus

### ps - Process Status

**Lister les processus :**

```bash
ps                      # Processus de la session courante
ps -u john              # Processus de l'utilisateur john
ps aux                  # TOUS les processus (format BSD)
ps -ef                  # TOUS les processus (format System V)
```

**La commande la plus utilisée :**

```bash
ps aux
```

**Exemple de sortie :**

```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 169820 13368 ?        Ss   Feb09   0:02 /sbin/init
root       850  0.0  0.0  72296  5852 ?        Ss   Feb09   0:00 /usr/sbin/sshd -D
www-data   900  0.1  1.2 256744 51200 ?        S    Feb09   0:15 /usr/sbin/apache2
john      1201  0.0  0.0  21520  5124 pts/0    Ss   10:30   0:00 -bash
john      1250  0.2  0.5  45612 20480 pts/0    S+   10:31   0:05 vim fichier.txt
```

**Décryptage des colonnes :**

```
USER    = Propriétaire
PID     = Process ID
%CPU    = Utilisation CPU
%MEM    = Utilisation RAM
VSZ     = Mémoire virtuelle (Ko)
RSS     = Mémoire physique (Ko)
TTY     = Terminal (? = pas de terminal)
STAT    = État (voir ci-dessous)
START   = Heure de démarrage
TIME    = Temps CPU cumulé
COMMAND = Commande complète
```

**États des processus (STAT) :**

```
R = Running (en cours d'exécution)
S = Sleeping (en attente)
D = Uninterruptible sleep (attente I/O)
Z = Zombie (terminé mais pas nettoyé)
T = sTopped (arrêté)
s = session leader
+ = foreground (premier plan)
< = haute priorité
N = basse priorité
```

### top - Monitoring temps réel

**L'outil n°1 pour diagnostiquer un serveur lent :**

```bash
top
```

**Interface :**

```
top - 10:45:32 up 1 day,  2:15,  2 users,  load average: 0.52, 0.58, 0.59
Tasks: 185 total,   1 running, 184 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.2 us,  1.3 sy,  0.0 ni, 93.2 id,  0.3 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7842.5 total,    452.2 free,   3250.8 used,   4139.5 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   4123.7 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
 1234 john      20   0 2548924 256744  45612 S  25.3   3.2   0:15.78 firefox
 5678 www-data  20   0  256744  51200  12800 S   5.2   0.6   0:02.15 apache2
```

**Informations importantes :**

```
load average: 0.52, 0.58, 0.59
             │     │     │
             │     │     └─ Charge moyenne sur 15 min
             │     └─ Charge moyenne sur 5 min
             └─ Charge moyenne sur 1 min

Règle : Si load average > nombre de CPUs → Serveur surchargé
Exemple : 4 CPUs, load = 6.2 → Problème !
```

**Raccourcis dans top :**

```
q       # Quitter
k       # Kill un processus (demande le PID)
M       # Trier par utilisation mémoire
P       # Trier par utilisation CPU (défaut)
1       # Afficher chaque CPU séparément
h       # Aide
```

> 💡 **Mon workflow :** Je lance `top` dès que je me connecte à un serveur pour avoir une vue d'ensemble.

### htop - top en mieux

**Version améliorée de top (à installer) :**

```bash
sudo apt install htop    # Debian/Ubuntu
sudo yum install htop    # RedHat/CentOS
```

**Avantages :**
- Interface colorée
- Utilisation de la souris
- Plus visuel et intuitif
- Filtrage facile

```bash
htop
```

**Interface htop :**

```
  1  [|||||||||||                         25.3%]
  2  [||||                                 5.2%]
  3  [||                                   2.1%]
  4  [|                                    1.5%]
  Mem[|||||||||||||||||||||||||      3.2G/7.6G]
  Swp[                                  0K/2.0G]

  PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
 1234 john       20   0 2548M  256M 45.6M S 25.3  3.2  0:15.78 firefox
 5678 www-data   20   0  256M 51.2M 12.8M S  5.2  0.6  0:02.15 apache2
```

**Raccourcis htop :**

```
F3      # Rechercher
F4      # Filtrer
F5      # Vue arbre
F6      # Trier
F9      # Kill
F10     # Quitter
```

### pgrep et pkill

**Trouver un processus par nom :**

```bash
pgrep firefox           # Affiche le PID de firefox
pgrep -u john           # Processus de l'utilisateur john
pgrep -l firefox        # Avec le nom de commande
```

**Tuer par nom :**

```bash
pkill firefox           # Tue tous les processus firefox
pkill -u john           # Tue tous les processus de john
pkill -9 firefox        # Kill brutal (signal SIGKILL)
```

> ⚠️ **ATTENTION :** `pkill` tue TOUS les processus correspondants !

### kill - Envoyer des signaux

**Syntaxe :**

```bash
kill [SIGNAL] PID
```

**Signaux principaux :**

```
┌─────────────────────────────────────────────────────────────┐
│  Signal  │ Numéro │ Action                                  │
├─────────────────────────────────────────────────────────────┤
│  SIGTERM │   15   │ Arrêt propre (défaut)                   │
│                     Permet au processus de se terminer       │
│                     proprement (sauvegardes, nettoyage)      │
│                                                             │
│  SIGKILL │    9   │ Arrêt BRUTAL immédiat                   │
│                     Tue le processus sans lui laisser le     │
│                     temps de rien faire                      │
│                                                             │
│  SIGHUP  │    1   │ Hangup - Recharger la config            │
│                     Souvent utilisé pour relire les configs  │
│                                                             │
│  SIGSTOP │   19   │ Pause le processus                      │
│                                                             │
│  SIGCONT │   18   │ Reprend le processus                    │
└─────────────────────────────────────────────────────────────┘
```

**Exemples :**

```bash
# Arrêt propre (laisse le temps de se terminer)
kill 1234
kill -15 1234           # Équivalent
kill -SIGTERM 1234      # Équivalent

# Arrêt brutal (si le processus refuse de s'arrêter)
kill -9 1234
kill -SIGKILL 1234      # Équivalent

# Recharger la config
kill -HUP 1234
kill -1 1234
```

**Ordre recommandé pour tuer un processus :**

```bash
# 1. Essayer l'arrêt propre
kill 1234

# 2. Attendre 10 secondes
sleep 10

# 3. Si toujours là, kill brutal
kill -9 1234
```

### killall

**Tuer par nom de programme :**

```bash
killall firefox         # Tue tous les firefox
killall -9 firefox      # Kill brutal
```

### nice et renice - Priorité

**Priorité = ordre de passage pour le CPU**

```
Nice value : de -20 (priorité maximale) à +19 (priorité minimale)

-20 = "Je passe en premier !"
  0 = Priorité normale (défaut)
+19 = "Je passe en dernier, pas urgent"
```

**Lancer un processus avec priorité basse :**

```bash
nice -n 10 ./gros_traitement.sh
# Lance le script avec priorité +10 (basse)
```

**Modifier la priorité d'un processus existant :**

```bash
# Voir la priorité actuelle
ps -l 1234

# Changer la priorité
renice -n 5 -p 1234     # Priorité +5
sudo renice -n -10 -p 1234  # Priorité -10 (root uniquement)
```

**Usage réel :**

```bash
# Grosse sauvegarde qui ne doit pas ralentir le serveur
nice -n 15 tar -czf /backup/data.tar.gz /data/
```

### bg, fg, jobs - Gestion foreground/background

**Lancer un processus en arrière-plan :**

```bash
./long_script.sh &      # Le & lance en background
```

**Mettre en pause et reprendre :**

```bash
# Lancer un processus
./long_script.sh

# Ctrl+Z pour mettre en pause
^Z
[1]+  Stopped    ./long_script.sh

# Voir les jobs
jobs
[1]+  Stopped    ./long_script.sh

# Reprendre en background
bg 1

# Reprendre en foreground
fg 1
```

**Usage réel :**

```bash
# Je lance vim
vim fichier.txt

# Mince, j'ai besoin du terminal !
# Ctrl+Z
^Z

# Je fais mes commandes
ls -l
grep "error" /var/log/syslog

# Je reviens dans vim
fg
```

### nohup - Détacher du terminal

**Problème :** Si vous lancez un script en SSH et que vous fermez la connexion, le script meurt.

**Solution : nohup**

```bash
nohup ./long_script.sh &
```

**Ce qui se passe :**
- Le script continue même si vous fermez le terminal
- La sortie est redirigée vers `nohup.out`

**Exemple réel :**

```bash
# Lancer un backup de nuit
nohup /scripts/backup.sh > /var/log/backup.log 2>&1 &

# Vérifier qu'il tourne
ps aux | grep backup.sh

# Je peux me déconnecter, il continue !
exit
```

---

## 🔄 systemd - Le gestionnaire de services

### Qu'est-ce que systemd ?

**systemd = Le système d'init moderne de Linux**

```
┌─────────────────────────────────────────────────────────────┐
│  AVANT systemd (SysV init)                                  │
├─────────────────────────────────────────────────────────────┤
│  • Scripts shell dans /etc/init.d/                          │
│  • Lent (démarrage séquentiel)                              │
│  • Difficile à gérer                                        │
│  • Commandes : service apache2 start                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  AVEC systemd (moderne)                                     │
├─────────────────────────────────────────────────────────────┤
│  • Fichiers .service dans /lib/systemd/system/              │
│  • Rapide (démarrage parallèle)                             │
│  • Gestion uniforme                                         │
│  • Logs intégrés (journalctl)                               │
│  • Commandes : systemctl start apache2                      │
└─────────────────────────────────────────────────────────────┘
```

**Vérifier que vous avez systemd :**

```bash
ps -p 1
#   PID TTY          TIME CMD
#     1 ?        00:00:02 systemd
```

Si PID 1 = systemd, vous avez systemd ! 🎉

### systemctl - Contrôle des services

**Les commandes essentielles :**

```bash
# Démarrer un service
sudo systemctl start apache2

# Arrêter un service
sudo systemctl stop apache2

# Redémarrer un service
sudo systemctl restart apache2

# Recharger la config (sans interruption)
sudo systemctl reload apache2

# Voir le statut d'un service
systemctl status apache2

# Activer au démarrage
sudo systemctl enable apache2

# Désactiver au démarrage
sudo systemctl disable apache2

# Vérifier si activé
systemctl is-enabled apache2

# Vérifier si actif
systemctl is-active apache2
```

### systemctl status - Le diagnostic n°1

**Commande la plus importante pour diagnostiquer :**

```bash
systemctl status apache2
```

**Exemple de sortie (service OK) :**

```
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2026-02-09 10:30:15 UTC; 2h 15min ago
     Docs: https://httpd.apache.org/docs/2.4/
 Main PID: 900 (apache2)
    Tasks: 55 (limit: 4915)
   Memory: 12.3M
   CGroup: /system.slice/apache2.service
           ├─900 /usr/sbin/apache2 -k start
           ├─901 /usr/sbin/apache2 -k start
           └─902 /usr/sbin/apache2 -k start

Feb 09 10:30:15 server systemd[1]: Starting The Apache HTTP Server...
Feb 09 10:30:15 server systemd[1]: Started The Apache HTTP Server.
```

**Décryptage :**

```
Loaded: loaded     → Le fichier .service existe et est valide
Active: active (running)  → Le service tourne
enabled            → Démarre automatiquement au boot
Main PID: 900      → PID du processus principal
```

**Exemple de sortie (service FAIL) :**

```
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled)
   Active: failed (Result: exit-code) since Sat 2026-02-09 12:45:32 UTC
     Docs: https://httpd.apache.org/docs/2.4/
  Process: 1234 ExecStart=/usr/sbin/apachectl start (code=exited, status=1/FAILURE)

Feb 09 12:45:32 server systemd[1]: Starting The Apache HTTP Server...
Feb 09 12:45:32 server apachectl[1234]: Action 'start' failed.
Feb 09 12:45:32 server apachectl[1234]: Syntax error on line 234
Feb 09 12:45:32 server systemd[1]: apache2.service: Control process exited, code=exited status=1
Feb 09 12:45:32 server systemd[1]: apache2.service: Failed with result 'exit-code'.
```

> 💡 **Astuce :** Les dernières lignes de `systemctl status` = début du diagnostic !

### Lister les services

```bash
# Tous les services
systemctl list-units --type=service

# Services actifs
systemctl list-units --type=service --state=active

# Services failed
systemctl list-units --type=service --state=failed

# Tous les services (même inactifs)
systemctl list-unit-files --type=service
```

**Exemple :**

```bash
$ systemctl list-units --type=service --state=failed
  UNIT                LOAD   ACTIVE SUB    DESCRIPTION
● apache2.service     loaded failed failed The Apache HTTP Server
```

**Diagnostic immédiat !**

### Recharger systemd

**Après avoir modifié un fichier .service :**

```bash
sudo systemctl daemon-reload
```

**Quand l'utiliser ?**
- Après création d'un nouveau fichier .service
- Après modification d'un fichier .service existant
- systemd ne voit pas les changements tant que daemon-reload n'est pas fait

---

## 📋 journalctl - Les logs systemd

### Qu'est-ce que journalctl ?

**journalctl = Commande pour lire les logs systemd (journal)**

**Avantages vs logs traditionnels :**
- Centralisé (tous les logs au même endroit)
- Filtrage puissant (par service, date, priorité)
- Recherche rapide
- Rotation automatique

### Commandes de base

```bash
# Voir tous les logs
journalctl

# Voir les logs d'un service
journalctl -u apache2
journalctl -u ssh

# Suivre en temps réel (comme tail -f)
journalctl -f
journalctl -f -u apache2

# Depuis une date
journalctl --since "2026-02-09 10:00:00"
journalctl --since "1 hour ago"
journalctl --since "yesterday"
journalctl --since today

# Entre deux dates
journalctl --since "2026-02-09 08:00" --until "2026-02-09 12:00"

# Par priorité
journalctl -p err        # Seulement les erreurs
journalctl -p warning    # Warning et plus grave
```

**Priorités :**

```
0: emerg   (urgence système)
1: alert   (alerte)
2: crit    (critique)
3: err     (erreur)
4: warning (avertissement)
5: notice  (notification)
6: info    (information)
7: debug   (debug)
```

### Exemples pratiques

**1. Voir les erreurs d'Apache :**

```bash
journalctl -u apache2 -p err
```

**2. Suivre les connexions SSH en temps réel :**

```bash
journalctl -f -u ssh
```

**3. Voir ce qui s'est passé au démarrage :**

```bash
journalctl -b
journalctl -b -1         # Démarrage précédent
```

**4. Logs depuis 1h avec détails :**

```bash
journalctl --since "1 hour ago" -xe
```

**Options importantes :**

```
-x    # Explications supplémentaires
-e    # Saute à la fin (comme less +G)
-n 50 # Affiche les 50 dernières lignes
```

### Gestion de l'espace journal

**Voir l'espace utilisé :**

```bash
journalctl --disk-usage
```

**Exemple de sortie :**

```
Archived and active journals take up 512.0M in the file system.
```

**Nettoyer les logs :**

```bash
# Garder seulement les 3 derniers jours
sudo journalctl --vacuum-time=3d

# Garder seulement 100M
sudo journalctl --vacuum-size=100M

# Supprimer les logs de plus de 7 jours
sudo journalctl --vacuum-time=7d
```

**Configuration permanente :**

```bash
sudo nano /etc/systemd/journald.conf
```

```ini
[Journal]
SystemMaxUse=100M       # Maximum 100M pour les logs
MaxRetentionSec=7day    # Garder max 7 jours
```

```bash
sudo systemctl restart systemd-journald
```

---

## 🛠️ Créer un service personnalisé

### Structure d'un fichier .service

**Emplacement :** `/etc/systemd/system/monservice.service`

**Template de base :**

```ini
[Unit]
Description=Description de mon service
After=network.target

[Service]
Type=simple
User=john
Group=john
WorkingDirectory=/home/john/app
ExecStart=/home/john/app/script.sh
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### Section [Unit]

```ini
[Unit]
Description=Mon super service         # Description courte
Documentation=https://example.com     # Doc (optionnel)
After=network.target                  # Démarre APRÈS le réseau
Requires=mysql.service                # Nécessite MySQL
```

**After vs Requires :**

```
After=mysql.service
→ Attends que MySQL démarre AVANT de démarrer
→ Mais si MySQL fail, je démarre quand même

Requires=mysql.service
→ Si MySQL fail, je fail aussi
```

### Section [Service]

```ini
[Service]
Type=simple                           # Type de service (voir ci-dessous)
User=www-data                         # Utilisateur
Group=www-data                        # Groupe
WorkingDirectory=/var/www             # Dossier de travail
ExecStart=/usr/bin/python3 app.py     # Commande de démarrage
ExecReload=/bin/kill -HUP $MAINPID    # Commande de rechargement
ExecStop=/bin/kill -TERM $MAINPID     # Commande d'arrêt
Restart=on-failure                    # Redémarre si crash
RestartSec=10                         # Attend 10s avant redémarrage
StandardOutput=syslog                 # Sortie vers syslog
StandardError=syslog                  # Erreurs vers syslog
SyslogIdentifier=myapp                # Identifiant dans les logs
```

**Types de services :**

```
simple    # Le processus reste au premier plan (défaut)
forking   # Le processus se met en background (fork)
oneshot   # Exécute une fois et se termine
notify    # Notifie systemd quand il est prêt
```

### Section [Install]

```ini
[Install]
WantedBy=multi-user.target            # Démarre au niveau multi-user
# Équivalent de runlevel 3 (mode texte)
```

**Targets courants :**

```
multi-user.target    # Mode texte (serveur)
graphical.target     # Mode graphique (desktop)
```

---

## 🎯 TP Pratique : Service custom

### Mission

Créer un service qui :
1. Lance un script Python qui écrit dans un log toutes les 10 secondes
2. Démarre automatiquement au boot
3. Redémarre automatiquement en cas de crash

### Étape 1 : Créer le script

```bash
mkdir -p /opt/monapp
nano /opt/monapp/monitor.py
```

**Contenu du script :**

```python
#!/usr/bin/env python3
import time
import datetime

log_file = "/var/log/monapp.log"

while True:
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open(log_file, "a") as f:
        f.write(f"[{timestamp}] Service is running\n")
    time.sleep(10)
```

**Rendre exécutable :**

```bash
chmod +x /opt/monapp/monitor.py
```

### Étape 2 : Créer le fichier service

```bash
sudo nano /etc/systemd/system/monapp.service
```

**Contenu :**

```ini
[Unit]
Description=Mon Application de Monitoring
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/monapp
ExecStart=/usr/bin/python3 /opt/monapp/monitor.py
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal
SyslogIdentifier=monapp

[Install]
WantedBy=multi-user.target
```

### Étape 3 : Activer le service

```bash
# Recharger systemd (voir le nouveau service)
sudo systemctl daemon-reload

# Démarrer le service
sudo systemctl start monapp

# Vérifier le statut
systemctl status monapp
```

**Résultat attendu :**

```
● monapp.service - Mon Application de Monitoring
   Loaded: loaded (/etc/systemd/system/monapp.service; disabled)
   Active: active (running) since Sat 2026-02-09 14:30:15 UTC
 Main PID: 2345 (python3)
    Tasks: 1
   Memory: 8.5M
   CGroup: /system.slice/monapp.service
           └─2345 /usr/bin/python3 /opt/monapp/monitor.py

Feb 09 14:30:15 server systemd[1]: Started Mon Application de Monitoring.
```

### Étape 4 : Activer au démarrage

```bash
sudo systemctl enable monapp
```

**Vérification :**

```bash
systemctl is-enabled monapp
# enabled
```

### Étape 5 : Tester les logs

```bash
# Voir les logs en temps réel
journalctl -f -u monapp

# Voir le fichier de log
tail -f /var/log/monapp.log
```

**Résultat :**

```
[2026-02-09 14:30:25] Service is running
[2026-02-09 14:30:35] Service is running
[2026-02-09 14:30:45] Service is running
```

### Étape 6 : Tester le redémarrage automatique

```bash
# Trouver le PID
systemctl status monapp | grep "Main PID"
# Main PID: 2345

# Tuer le processus
sudo kill -9 2345

# Attendre 10 secondes (RestartSec=10)
sleep 10

# Vérifier qu'il a redémarré
systemctl status monapp
# Active: active (running)
# Main PID: 2456 ← Nouveau PID !
```

**✅ Le service a redémarré automatiquement !**

### Étape 7 : Tester le démarrage au boot

```bash
# Redémarrer le serveur
sudo reboot

# Après redémarrage, se reconnecter
ssh user@server

# Vérifier que le service tourne
systemctl status monapp
# Active: active (running)

# Vérifier le log
tail /var/log/monapp.log
```

**✅ Mission accomplie !**

---

## 🎯 Exercices pratiques

### Exercice 1 : Diagnostic serveur lent

**Objectif :** Identifier et corriger un processus qui consomme trop de CPU.

**Scénario :**

Créer un script qui consomme beaucoup de CPU et le diagnostiquer.

**Consignes :**

1. Créer un script gourmand
2. Le lancer
3. Identifier le problème avec `top` ou `htop`
4. Le tuer

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer un script CPU-intensif
cat > /tmp/cpu_eater.sh << 'EOF'
#!/bin/bash
while true; do
  echo "Eating CPU..." > /dev/null
done
EOF

chmod +x /tmp/cpu_eater.sh

# 2. Lancer en background
/tmp/cpu_eater.sh &

# 3. Diagnostiquer avec top
top

# Vous voyez :
#  PID USER  %CPU %MEM COMMAND
# 3456 john  99.9  0.0 bash

# 4. Identifier le PID et tuer
pkill -9 cpu_eater

# Ou avec le PID exact
kill -9 3456

# 5. Vérifier
top
# Le processus a disparu
```

</details>

---

### Exercice 2 : Service qui refuse de démarrer

**Objectif :** Diagnostiquer un service qui fail au démarrage.

**Scénario :**

Créer un service avec une erreur et la corriger.

**Consignes :**

1. Créer un service avec une erreur dans ExecStart
2. Essayer de le démarrer (échec)
3. Diagnostiquer avec `systemctl status` et `journalctl`
4. Corriger

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer un service avec erreur
sudo nano /etc/systemd/system/buggy.service

# Contenu (ERREUR : /usr/bin/python3 au lieu de /usr/bin/python3)
[Unit]
Description=Service avec bug

[Service]
ExecStart=/usr/bin/pyton3 -c "print('Hello')"

[Install]
WantedBy=multi-user.target

# 2. Recharger et démarrer
sudo systemctl daemon-reload
sudo systemctl start buggy

# 3. Échec ! Diagnostiquer
systemctl status buggy
# ● buggy.service - Service avec bug
#    Loaded: loaded
#    Active: failed (Result: exit-code)
#   Process: 4567 ExecStart=/usr/bin/pyton3 -c print('Hello')
#
# Feb 09 15:30:15 server systemd[1]: Starting Service avec bug...
# Feb 09 15:30:15 server systemd[1]: buggy.service: Main process exited
# Feb 09 15:30:15 server systemd[1]: buggy.service: Failed with result 'exit-code'.

# Plus de détails
journalctl -xe -u buggy
# ...
# Feb 09 15:30:15 server systemd[4567]: buggy.service: Failed to execute command: No such file or directory
# Feb 09 15:30:15 server systemd[4567]: buggy.service: Failed at step EXEC spawning /usr/bin/pyton3
#                                                                                          ^^^^^^
#                                                                                     ERREUR ICI !

# 4. Corriger le fichier
sudo nano /etc/systemd/system/buggy.service
# Changer : pyton3 → python3

# 5. Recharger et démarrer
sudo systemctl daemon-reload
sudo systemctl start buggy

# 6. Vérifier
systemctl status buggy
# ● buggy.service - Service avec bug
#    Active: inactive (dead)
# (Normal, le script s'est exécuté et terminé)
```

</details>

---

### Exercice 3 : Service avec logs rotatifs

**Objectif :** Créer un service qui nettoie automatiquement ses logs.

**Consignes :**

1. Créer un script qui génère des logs
2. Configurer journald pour limiter la taille
3. Vérifier la rotation automatique

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Script générateur de logs
sudo nano /opt/log-generator.sh

# Contenu
#!/bin/bash
while true; do
  logger -t myapp "Log entry at $(date)"
  sleep 1
done

sudo chmod +x /opt/log-generator.sh

# 2. Créer le service
sudo nano /etc/systemd/system/log-generator.service

# Contenu
[Unit]
Description=Log Generator

[Service]
ExecStart=/opt/log-generator.sh
StandardOutput=syslog
SyslogIdentifier=myapp

[Install]
WantedBy=multi-user.target

# 3. Configurer la rotation
sudo nano /etc/systemd/journald.conf

# Ajouter/modifier
[Journal]
SystemMaxUse=50M
MaxFileSec=1day

# 4. Redémarrer journald
sudo systemctl restart systemd-journald

# 5. Démarrer le service
sudo systemctl daemon-reload
sudo systemctl start log-generator

# 6. Voir les logs
journalctl -f -t myapp

# 7. Vérifier l'espace
journalctl --disk-usage
```

</details>

---

## 📚 Ressources

### Documentation officielle

- [systemd Documentation](https://www.freedesktop.org/software/systemd/man/)
- [systemctl Manual](https://www.freedesktop.org/software/systemd/man/systemctl.html)
- [journalctl Manual](https://www.freedesktop.org/software/systemd/man/journalctl.html)

### Tutoriels

- [Understanding systemd Units](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)
- [How To Use Systemctl](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)

### Outils

- [systemd.unit Generator](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Services à surveiller sur mes serveurs :**
-
-

**Commandes que j'utilise le plus :**
-
-

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Je sais utiliser `ps`, `top`, `htop` pour surveiller les processus
- [ ] Je peux tuer un processus avec `kill` et les différents signaux
- [ ] Je maîtrise `systemctl` (start, stop, status, enable, disable)
- [ ] Je sais analyser les logs avec `journalctl`
- [ ] Je peux créer un fichier .service personnalisé
- [ ] Je sais diagnostiquer un service qui refuse de démarrer
- [ ] Je comprends les différences entre les types de services systemd

---

<div align="center">

**Cours suivant :** [04-ssh-connexion-distance.md](04-ssh-connexion-distance.md)

[⬅️ Retour au sommaire](README.md)

</div>
