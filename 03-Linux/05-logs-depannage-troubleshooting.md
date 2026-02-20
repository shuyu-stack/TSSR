# Logs et Dépannage - L'art du troubleshooting

> 📚 **Module :** Linux Administration - Diagnostic et résolution de problèmes
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Intermédiaire (N2)
> 👨‍🏫 **Approche :** Admin système → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [L'importance des logs](#-limportance-des-logs)
- [Où sont les logs ?](#-où-sont-les-logs-)
- [journalctl - Logs systemd](#-journalctl---logs-systemd)
- [Analyse de logs](#-analyse-de-logs)
- [Outils de diagnostic](#-outils-de-diagnostic)
- [Méthodologie de dépannage](#-méthodologie-de-dépannage)
- [TP Pratique : 5 pannes à résoudre](#-tp-pratique--5-pannes-à-résoudre)
- [Cheat sheet dépannage](#-cheat-sheet-dépannage)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**Vendredi 18h30.** Je suis en route pour le weekend. Mon téléphone sonne.

**"Le site e-commerce est complètement DOWN ! Panique totale !"**

C'est le Black Friday. Chaque minute = 10 000€ de perte.

Je me gare sur une aire d'autoroute. Je sors mon laptop. Je me connecte en 4G.

```bash
ssh prod-web-01
tail -n 100 /var/log/apache2/error.log
```

**Ligne 87 :**

```
[Fri Nov 25 18:15:32] PHP Fatal error: Allowed memory size exhausted
```

**Diagnostic : 3 minutes.**

```bash
sudo nano /etc/php/8.1/apache2/php.ini
# memory_limit = 128M → 256M
sudo systemctl restart apache2
```

**Site rétabli. Temps total : 10 minutes.**

### 🎯 La leçon

**Les logs sont vos meilleurs amis.** Ils contiennent **TOUTES** les réponses.

Un bon admin système passe **50% de son temps** à lire des logs. Pas à coder. Pas à installer. À **LIRE**.

**Dans ce cours, vous allez apprendre :**
- ✅ Où chercher selon le problème
- ✅ Comment extraire l'information utile dans 10 000 lignes
- ✅ Les commandes pour diagnostiquer en 2 minutes
- ✅ La méthodologie systématique de troubleshooting

**Après ce cours, vous serez capable de résoudre 80% des pannes serveur.** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Localiser** les bons fichiers de logs selon le problème
- ✅ **Analyser** des logs avec grep, awk, sed
- ✅ **Utiliser** journalctl efficacement
- ✅ **Diagnostiquer** les problèmes courants (disque plein, CPU saturé, mémoire)
- ✅ **Appliquer** une méthodologie systématique de dépannage
- ✅ **Résoudre** des pannes réelles en moins de 10 minutes

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les commandes de base Linux
- [ ] Savoir utiliser grep et les pipes
- [ ] Comprendre les processus et services
- [ ] Avoir accès à un système Linux

**Matériel nécessaire :**
- 💻 Linux avec systemd
- 🔑 Accès root/sudo
- 📝 Terminal

---

## 📜 L'importance des logs

### Qu'est-ce qu'un log ?

**Log = Journal de bord du système**

Comme un journal intime, mais pour votre serveur :
- Qui s'est connecté et quand
- Quels services ont démarré/planté
- Quelles erreurs se sont produites
- Quelles actions ont été effectuées

### Les 3 types de logs

```
┌─────────────────────────────────────────────────────────────┐
│  LOGS SYSTÈME                                               │
├─────────────────────────────────────────────────────────────┤
│  • Démarrage/arrêt                                          │
│  • Services systemd                                         │
│  • Kernel (noyau)                                           │
│  • Authentifications                                        │
│                                                             │
│  Emplacement : /var/log/syslog, journalctl                  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LOGS APPLICATIFS                                           │
├─────────────────────────────────────────────────────────────┤
│  • Apache/Nginx (serveur web)                               │
│  • MySQL/PostgreSQL (bases de données)                      │
│  • PHP, Python, Node.js                                     │
│  • Applications métier                                      │
│                                                             │
│  Emplacement : /var/log/<application>/                      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LOGS DE SÉCURITÉ                                           │
├─────────────────────────────────────────────────────────────┤
│  • Connexions SSH                                           │
│  • Tentatives d'authentification                            │
│  • sudo                                                     │
│  • Pare-feu                                                 │
│                                                             │
│  Emplacement : /var/log/auth.log, /var/log/secure          │
└─────────────────────────────────────────────────────────────┘
```

---

## 📂 Où sont les logs ?

### Structure de /var/log/

```bash
ls -lh /var/log/
```

**Les fichiers essentiels :**

```
┌─────────────────────────────────────────────────────────────┐
│  LOGS GÉNÉRAUX                                              │
├─────────────────────────────────────────────────────────────┤
│  /var/log/syslog          # Journal système global (Debian) │
│  /var/log/messages        # Journal système (RedHat)        │
│  /var/log/dmesg           # Messages du kernel au boot      │
│  /var/log/kern.log        # Messages du kernel              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  SÉCURITÉ                                                   │
├─────────────────────────────────────────────────────────────┤
│  /var/log/auth.log        # Authentifications (Debian)      │
│  /var/log/secure          # Authentifications (RedHat)      │
│  /var/log/faillog         # Échecs de connexion             │
│  /var/log/lastlog         # Dernières connexions            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  SERVEUR WEB                                                │
├─────────────────────────────────────────────────────────────┤
│  /var/log/apache2/access.log    # Requêtes HTTP Apache     │
│  /var/log/apache2/error.log     # Erreurs Apache           │
│  /var/log/nginx/access.log      # Requêtes HTTP Nginx      │
│  /var/log/nginx/error.log       # Erreurs Nginx            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  BASE DE DONNÉES                                            │
├─────────────────────────────────────────────────────────────┤
│  /var/log/mysql/error.log       # Erreurs MySQL            │
│  /var/log/postgresql/           # Logs PostgreSQL          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  AUTRES                                                     │
├─────────────────────────────────────────────────────────────┤
│  /var/log/mail.log              # Serveur mail             │
│  /var/log/cron.log              # Tâches cron              │
│  /var/log/dpkg.log              # Installations APT        │
│  /var/log/yum.log               # Installations YUM        │
└─────────────────────────────────────────────────────────────┘
```

### Rotation des logs

**Les logs ne grossissent pas indéfiniment. Ils sont "tournés" (rotated).**

```bash
ls -lh /var/log/syslog*
```

**Résultat :**

```
-rw-r----- 1 syslog adm  2.5M Feb  9 18:00 syslog
-rw-r----- 1 syslog adm  8.2M Feb  8 23:59 syslog.1
-rw-r----- 1 syslog adm  3.1M Feb  7 23:59 syslog.2.gz
-rw-r----- 1 syslog adm  2.8M Feb  6 23:59 syslog.3.gz
```

**Explication :**

```
syslog         # Fichier actuel
syslog.1       # Rotation d'hier
syslog.2.gz    # Il y a 2 jours (compressé)
syslog.3.gz    # Il y a 3 jours (compressé)
```

**Configuration de logrotate :**

```bash
cat /etc/logrotate.d/rsyslog
```

**Exemple :**

```
/var/log/syslog
{
    rotate 7             # Garder 7 rotations
    daily                # Rotation quotidienne
    missingok            # Pas d'erreur si fichier absent
    compress             # Compresser les anciennes versions
    delaycompress        # Compresse à partir de .2
    notifempty           # Ne pas tourner si vide
}
```

---

## 📋 journalctl - Logs systemd

### Avantages de journalctl

```
┌─────────────────────────────────────────────────────────────┐
│  FICHIERS LOGS CLASSIQUES                                   │
├─────────────────────────────────────────────────────────────┤
│  ❌ Éparpillés dans /var/log/                                │
│  ❌ Formats différents                                       │
│  ❌ Recherche difficile                                      │
│  ❌ Rotation manuelle                                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  JOURNALCTL (systemd)                                       │
├─────────────────────────────────────────────────────────────┤
│  ✅ Centralisé                                               │
│  ✅ Format unifié                                            │
│  ✅ Filtrage puissant                                        │
│  ✅ Rotation automatique                                     │
│  ✅ Recherche rapide                                         │
└─────────────────────────────────────────────────────────────┘
```

### Commandes essentielles

**Voir tous les logs :**

```bash
journalctl
```

**Suivre en temps réel :**

```bash
journalctl -f
```

**Logs d'un service spécifique :**

```bash
journalctl -u apache2
journalctl -u ssh
journalctl -u nginx
```

**Filtrer par date :**

```bash
# Depuis une heure
journalctl --since "1 hour ago"

# Depuis hier
journalctl --since yesterday

# Aujourd'hui
journalctl --since today

# Date précise
journalctl --since "2026-02-09 10:00:00"

# Entre deux dates
journalctl --since "2026-02-09 08:00" --until "2026-02-09 12:00"
```

**Filtrer par priorité :**

```bash
journalctl -p err          # Erreurs uniquement
journalctl -p warning      # Warnings et plus grave
journalctl -p crit         # Critique et plus grave
```

**Priorités disponibles :**

```
0: emerg    (urgence)
1: alert    (alerte)
2: crit     (critique)
3: err      (erreur)
4: warning  (avertissement)
5: notice   (notification)
6: info     (information)
7: debug    (debug)
```

**Logs du dernier boot :**

```bash
journalctl -b              # Boot actuel
journalctl -b -1           # Boot précédent
journalctl -b -2           # Avant-avant-dernier boot
```

**Lister les boots :**

```bash
journalctl --list-boots
```

**Résultat :**

```
-2 abc123... Tue 2026-02-07 09:00:15 CET—Tue 2026-02-07 18:30:22 CET
-1 def456... Wed 2026-02-08 08:45:30 CET—Wed 2026-02-08 23:15:45 CET
 0 ghi789... Thu 2026-02-09 09:12:05 CET—Thu 2026-02-09 18:45:12 CET
```

**Options pratiques :**

```bash
-n 50           # Dernières 50 lignes
-r              # Ordre inverse (du plus récent au plus ancien)
-x              # Explications supplémentaires
-e              # Sauter à la fin (comme less +G)
-o json-pretty  # Format JSON lisible
```

**Exemples combinés :**

```bash
# Erreurs Apache depuis 1h
journalctl -u apache2 -p err --since "1 hour ago"

# Dernières 100 lignes du service MySQL
journalctl -u mysql -n 100

# Suivre les logs SSH en temps réel
journalctl -f -u ssh
```

### Exemples pratiques

**1. Voir pourquoi le système a redémarré :**

```bash
journalctl -b -1 -p err
```

**2. Analyser un service qui plante :**

```bash
journalctl -u myapp --since "10 minutes ago" -xe
```

**3. Trouver les connexions SSH échouées :**

```bash
journalctl -u ssh --since today | grep "Failed password"
```

**4. Logs du kernel :**

```bash
journalctl -k
```

---

## 🔍 Analyse de logs

### grep - L'outil indispensable

**Rechercher un motif :**

```bash
grep "error" /var/log/syslog
grep -i "error" /var/log/syslog         # Insensible à la casse
grep -n "error" /var/log/syslog         # Avec numéros de ligne
grep -r "error" /var/log/               # Récursif dans tous les fichiers
```

**Compter les occurrences :**

```bash
grep -c "Failed password" /var/log/auth.log
```

**Afficher le contexte :**

```bash
grep -A 5 "error" log.txt     # 5 lignes APRÈS
grep -B 5 "error" log.txt     # 5 lignes AVANT
grep -C 5 "error" log.txt     # 5 lignes AVANT et APRÈS
```

**Inverser la recherche :**

```bash
grep -v "INFO" log.txt        # Tout SAUF les lignes avec INFO
```

**Expressions régulières :**

```bash
# Trouver les erreurs 4xx et 5xx dans Apache
grep -E " [45][0-9]{2} " /var/log/apache2/access.log

# Trouver les IP
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' /var/log/auth.log
```

### awk - Extraction de colonnes

**Extraire des colonnes spécifiques :**

```bash
# Exemple de log Apache :
# 192.168.1.50 - - [09/Feb/2026:10:15:23] "GET /index.html HTTP/1.1" 200

# Extraire les IP (colonne 1)
awk '{print $1}' /var/log/apache2/access.log

# Extraire IP et code de statut (colonnes 1 et 9)
awk '{print $1, $9}' /var/log/apache2/access.log

# Filtrer et afficher
awk '$9 >= 400 {print $1, $9}' /var/log/apache2/access.log
# Affiche IP et code pour toutes les erreurs 4xx/5xx
```

**Compter les occurrences :**

```bash
# Top 10 des IP qui se connectent le plus
awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -10
```

**Résultat :**

```
    523 192.168.1.50
    312 192.168.1.75
    156 192.168.1.100
    ...
```

### sed - Modification à la volée

**Remplacer du texte :**

```bash
# Afficher en remplaçant "error" par "ERROR"
sed 's/error/ERROR/g' log.txt

# Afficher seulement les lignes 10 à 20
sed -n '10,20p' log.txt

# Supprimer les lignes vides
sed '/^$/d' log.txt
```

### Combinaisons puissantes

**1. Trouver les IP qui attaquent en SSH :**

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn
```

**Décryptage :**
- `grep "Failed password"` : Filtre les échecs
- `awk '{print $(NF-3)}'` : Extrait l'IP (3e colonne en partant de la fin)
- `sort` : Trie
- `uniq -c` : Compte les doublons
- `sort -rn` : Trie par nombre décroissant

**Résultat :**

```
     45 203.0.113.25
     32 198.51.100.50
     12 192.0.2.100
```

**2. Analyser les codes HTTP :**

```bash
awk '{print $9}' /var/log/apache2/access.log | sort | uniq -c | sort -rn
```

**Résultat :**

```
  15234 200
   2341 304
    523 404
    156 500
```

**3. Trouver les requêtes lentes (> 5 secondes) :**

```bash
awk '$NF > 5 {print $0}' /var/log/nginx/access.log
```

---

## 🛠️ Outils de diagnostic

### df - Espace disque

**Voir l'espace disque :**

```bash
df -h
```

**Résultat :**

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   45G  2.5G  95% /
/dev/sdb1       100G   23G   72G  25% /data
tmpfs           7.7G  1.2M  7.7G   1% /run
```

> ⚠️ **ALERTE :** Si Use% > 90%, c'est problématique !

**Problème fréquent :** Disque plein = services qui plantent.

### du - Taille des dossiers

**Trouver ce qui prend de la place :**

```bash
du -sh /*                          # Taille de chaque dossier à la racine
du -sh /var/*                      # Dans /var
du -h /var/log | sort -rh | head -10   # Top 10 des logs les plus gros
```

**Exemple :**

```bash
$ du -sh /var/log/*
2.5G    /var/log/apache2
1.8G    /var/log/mysql
523M    /var/log/syslog
45M     /var/log/auth.log
```

**Trouver les fichiers de plus de 100 Mo :**

```bash
find /var/log -type f -size +100M -exec ls -lh {} \;
```

### free - Mémoire RAM

**Voir la RAM utilisée :**

```bash
free -h
```

**Résultat :**

```
              total        used        free      shared  buff/cache   available
Mem:          7.7Gi       3.2Gi       1.5Gi       256Mi       3.0Gi       4.0Gi
Swap:         2.0Gi          0B       2.0Gi
```

**Interprétation :**

```
used     = RAM réellement utilisée par les applications
free     = RAM totalement libre (inutilisée)
buff/cache = Cache du système (NORMAL et BÉNÉFIQUE !)
available = RAM disponible pour de nouvelles applications

⚠️ Regarder "available", pas "free" !
```

**Si available < 10% du total → Problème de mémoire !**

### uptime - Charge système

```bash
uptime
```

**Résultat :**

```
18:45:12 up 5 days, 9:30, 2 users, load average: 2.50, 1.80, 1.25
                                                  │     │     │
                                                  │     │     └─ 15 min
                                                  │     └─ 5 min
                                                  └─ 1 min
```

**Interprétation de load average :**

```
Load average = Nombre moyen de processus en attente d'exécution

Règle simple :
• Load < Nombre de CPUs = OK ✅
• Load = Nombre de CPUs = Limite ⚠️
• Load > Nombre de CPUs = Problème ❌

Exemple :
• 4 CPUs, load = 2.5 → OK
• 4 CPUs, load = 6.8 → Problème !
```

**Voir le nombre de CPUs :**

```bash
nproc
lscpu | grep "^CPU(s)"
```

### iostat - I/O disque

**Installer :**

```bash
sudo apt install sysstat    # Debian/Ubuntu
```

**Utiliser :**

```bash
iostat -x 2
```

**Résultat :**

```
Device   r/s   w/s    rkB/s    wkB/s  %util
sda     15.2  42.5   512.3   1854.2   45.2
sdb      2.1   8.3    64.5    256.8    8.5
```

**Interprétation :**

```
r/s      = Lectures par seconde
w/s      = Écritures par seconde
%util    = % d'utilisation du disque

⚠️ Si %util proche de 100% → Goulet d'étranglement I/O !
```

### netstat / ss - Connexions réseau

**Voir les ports ouverts :**

```bash
# netstat (ancien)
netstat -tulpn

# ss (moderne)
ss -tulpn
```

**Options :**

```
-t    # TCP
-u    # UDP
-l    # Listening (en écoute)
-p    # Programme
-n    # Numérique (pas de résolution DNS)
```

**Résultat :**

```
State   Recv-Q Send-Q  Local Address:Port  Peer Address:Port  Process
LISTEN  0      128     0.0.0.0:22          0.0.0.0:*          users:(("sshd",pid=850))
LISTEN  0      128     0.0.0.0:80          0.0.0.0:*          users:(("apache2",pid=900))
LISTEN  0      80      127.0.0.1:3306      0.0.0.0:*          users:(("mysqld",pid=1200))
```

**Vérifier qu'un port est ouvert :**

```bash
ss -tulpn | grep :80
# Si résultat → Port 80 est ouvert
```

### lsof - Fichiers ouverts

**Voir qui utilise un fichier :**

```bash
lsof /var/log/syslog
```

**Voir les fichiers ouverts par un processus :**

```bash
lsof -p 1234
```

**Trouver quel processus écoute sur un port :**

```bash
lsof -i :80
lsof -i :3306
```

**Résultat :**

```
COMMAND  PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
apache2  900     root    4u  IPv6  12345      0t0  TCP *:80 (LISTEN)
```

---

## 🔧 Méthodologie de dépannage

### La méthode en 5 étapes

```
┌─────────────────────────────────────────────────────────────┐
│  1. IDENTIFIER LE SYMPTÔME                                  │
├─────────────────────────────────────────────────────────────┤
│  • Que se passe-t-il exactement ?                           │
│  • Depuis quand ?                                           │
│  • Ça affecte qui/quoi ?                                    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  2. VÉRIFIER LES LOGS                                       │
├─────────────────────────────────────────────────────────────┤
│  • Logs système : journalctl, /var/log/syslog              │
│  • Logs applicatifs : /var/log/<app>/                      │
│  • Logs de sécurité : /var/log/auth.log                    │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  3. ISOLER LA CAUSE                                         │
├─────────────────────────────────────────────────────────────┤
│  • Service arrêté ?                                         │
│  • Disque plein ?                                           │
│  • Mémoire saturée ?                                        │
│  • Erreur de configuration ?                                │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  4. TESTER UNE SOLUTION                                     │
├─────────────────────────────────────────────────────────────┤
│  • Faire UNE modification à la fois                         │
│  • Tester                                                   │
│  • Si ça ne marche pas, annuler et essayer autre chose      │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  5. DOCUMENTER                                              │
├─────────────────────────────────────────────────────────────┤
│  • Qu'est-ce qui a causé le problème ?                      │
│  • Quelle solution a fonctionné ?                           │
│  • Comment éviter que ça se reproduise ?                    │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚨 TP Pratique : 5 pannes à résoudre

### Panne 1 : Serveur web inaccessible

**Symptôme :** Le site web ne répond pas.

**Diagnostic :**

```bash
# 1. Tester la connexion
curl http://localhost
# curl: (7) Failed to connect to localhost port 80

# 2. Vérifier le service
systemctl status apache2
# ● apache2.service
#    Active: inactive (dead)

# 3. Voir pourquoi il est arrêté
journalctl -u apache2 -n 50
```

**Cause identifiée :** Service arrêté.

**Solution :**

```bash
sudo systemctl start apache2
sudo systemctl status apache2
# Active: active (running) ✅

# Test
curl http://localhost
# <html>It works!</html> ✅
```

---

### Panne 2 : Disque plein

**Symptôme :** Applications plantent, erreurs "No space left on device".

**Diagnostic :**

```bash
# 1. Vérifier l'espace disque
df -h
# /dev/sda1        50G   50G     0  100% /

# 2. Trouver ce qui prend de la place
du -sh /var/log/*
# 45G     /var/log/apache2

# 3. Regarder en détail
ls -lh /var/log/apache2/
# -rw-r--r-- 1 root root 45G Feb  9 18:00 access.log
```

**Cause identifiée :** Log Apache de 45 Go !

**Solution :**

```bash
# OPTION 1 : Vider le fichier (sans le supprimer)
sudo truncate -s 0 /var/log/apache2/access.log

# OPTION 2 : Compresser et archiver
sudo gzip /var/log/apache2/access.log
sudo mv /var/log/apache2/access.log.gz /backup/

# OPTION 3 : Supprimer les vieux logs
sudo find /var/log -name "*.log.*" -mtime +30 -delete

# Vérifier
df -h
# /dev/sda1        50G   5G   42G   11% / ✅
```

**Prévention :**

```bash
# Configurer logrotate
sudo nano /etc/logrotate.d/apache2

/var/log/apache2/*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 640 root adm
}
```

---

### Panne 3 : Serveur lent

**Symptôme :** Toutes les applications sont lentes, le serveur rame.

**Diagnostic :**

```bash
# 1. Voir la charge
uptime
# load average: 8.52, 7.80, 6.25
# (Sur un serveur 4 CPUs → Problème !)

# 2. Voir ce qui consomme
top
# PID  USER  %CPU  %MEM  COMMAND
# 1234 www   99.8  45.2  /usr/bin/php script.php

# 3. Voir les détails
ps aux | grep 1234
```

**Cause identifiée :** Script PHP qui consomme 100% CPU.

**Solution :**

```bash
# Tuer le processus
sudo kill 1234

# Vérifier
top
# load average: 0.52, 1.80, 3.25 ✅
```

**Investigation :**

```bash
# Regarder le log PHP
tail -n 100 /var/log/php-fpm/error.log

# Trouver le script problématique
ps aux | grep php
```

---

### Panne 4 : Impossible de se connecter en SSH

**Symptôme :** `ssh user@serveur` ne fonctionne pas.

**Diagnostic :**

```bash
# 1. Tester la connexion
ssh user@192.168.1.100
# ssh: connect to host 192.168.1.100 port 22: Connection refused

# 2. Vérifier si le serveur répond (depuis un autre terminal)
ping 192.168.1.100
# OK

# 3. Vérifier le service SSH (sur le serveur)
systemctl status ssh
# ● ssh.service
#    Active: inactive (dead)

# 4. Voir pourquoi
journalctl -u ssh -n 50
# Feb 09 18:00:00 server sshd[850]: Server listening on 0.0.0.0 port 22.
# Feb 09 18:15:23 server systemd[1]: Stopping OpenBSD Secure Shell server...
# Feb 09 18:15:23 server systemd[1]: ssh.service: Succeeded.
```

**Cause identifiée :** Service SSH arrêté.

**Solution :**

```bash
sudo systemctl start ssh
sudo systemctl enable ssh    # Pour démarrer au boot

# Vérifier
systemctl status ssh
# Active: active (running) ✅

# Tester
ssh user@192.168.1.100
# Connexion OK ✅
```

**Alternative : Port bloqué par le pare-feu**

```bash
# Vérifier le pare-feu
sudo ufw status
# Status: active
# To                         Action      From
# --                         ------      ----
# 80/tcp                     ALLOW       Anywhere
# 443/tcp                    ALLOW       Anywhere
# (SSH port 22 absent !)

# Ouvrir le port
sudo ufw allow 22
# Ou
sudo ufw allow ssh

# Tester
ssh user@192.168.1.100
# Connexion OK ✅
```

---

### Panne 5 : Permission denied

**Symptôme :** Un utilisateur ne peut pas écrire dans `/var/www/html`.

**Diagnostic :**

```bash
# 1. Tenter d'écrire (en tant que l'utilisateur)
su - jdev
touch /var/www/html/test.php
# touch: cannot touch '/var/www/html/test.php': Permission denied

# 2. Vérifier les permissions
ls -ld /var/www/html
# drwxr-xr-x 2 root root 4096 Feb  9 18:00 /var/www/html
#           ││└─ Others : r-x (lecture, pas écriture)
#           │└─ Group : r-x
#           └─ Owner : rwx

# 3. Vérifier à quels groupes appartient l'utilisateur
id jdev
# uid=1001(jdev) gid=1001(jdev) groups=1001(jdev),33(www-data)
```

**Cause identifiée :** Le dossier appartient à `root:root`, le groupe n'a que `r-x`.

**Solution :**

```bash
# Changer le groupe vers www-data
sudo chown :www-data /var/www/html

# Donner écriture au groupe
sudo chmod g+w /var/www/html

# Bonus : Ajouter SGID pour que tous les nouveaux fichiers héritent du groupe
sudo chmod g+s /var/www/html

# Vérifier
ls -ld /var/www/html
# drwxrwsr-x 2 root www-data 4096 /var/www/html ✅

# Tester
su - jdev
touch /var/www/html/test.php
ls -l /var/www/html/test.php
# -rw-r--r-- 1 jdev www-data 0 Feb  9 18:45 test.php ✅
```

---

## 📋 Cheat sheet dépannage

### Tableau symptôme → diagnostic → solution

| Symptôme | Commandes diagnostic | Solution probable |
|----------|---------------------|-------------------|
| **Site web ne répond pas** | `systemctl status apache2`<br>`journalctl -u apache2`<br>`curl localhost` | `systemctl start apache2`<br>Vérifier config |
| **Serveur lent** | `top` / `htop`<br>`uptime`<br>`iostat` | `kill` processus gourmand<br>Augmenter ressources |
| **Disque plein** | `df -h`<br>`du -sh /var/log/*`<br>`find / -size +1G` | Nettoyer logs<br>Configurer logrotate |
| **Service ne démarre pas** | `systemctl status service`<br>`journalctl -u service -xe` | Corriger config<br>Vérifier permissions |
| **SSH refuse connexion** | `systemctl status ssh`<br>`ss -tulpn \| grep 22`<br>`ufw status` | Démarrer SSH<br>Ouvrir port pare-feu |
| **Permission denied** | `ls -l fichier`<br>`id utilisateur`<br>`groups utilisateur` | `chmod` / `chown`<br>Ajouter au groupe |
| **Mémoire pleine** | `free -h`<br>`ps aux --sort=-%mem \| head` | Tuer processus<br>Redémarrer service |
| **Application plante** | `journalctl -u app`<br>`tail /var/log/app.log`<br>`dmesg` | Vérifier logs<br>Augmenter ressources |

### Commandes rapides par situation

**Diagnostic général (1 minute) :**

```bash
uptime                           # Charge
df -h                            # Disque
free -h                          # RAM
systemctl list-units --failed   # Services en échec
journalctl -p err --since today # Erreurs du jour
```

**Diagnostic réseau :**

```bash
ping <host>                      # Connectivité
ss -tulpn                        # Ports ouverts
curl -I http://localhost         # Test HTTP
traceroute <host>                # Route réseau
```

**Diagnostic performances :**

```bash
top                              # Vue générale
ps aux --sort=-%cpu | head       # Top CPU
ps aux --sort=-%mem | head       # Top RAM
iostat -x 2                      # I/O disque
```

---

## 📚 Ressources

### Documentation officielle

- [systemd Journal](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html)
- [Linux Performance Tools](https://www.brendangregg.com/linuxperf.html)
- [Logrotate Manual](https://linux.die.net/man/8/logrotate)

### Guides

- [Linux Troubleshooting Guide](https://www.tecmint.com/linux-performance-monitoring-and-file-system-statistics-reports/)
- [Server Monitoring Best Practices](https://www.digitalocean.com/community/tutorials/an-introduction-to-metrics-monitoring-and-alerting)

### Outils graphiques

- [Grafana](https://grafana.com/) - Dashboards de monitoring
- [Prometheus](https://prometheus.io/) - Collecte de métriques
- [Netdata](https://www.netdata.cloud/) - Monitoring temps réel

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Pannes que j'ai résolues :**
-
-

**Astuces apprises :**
-
-

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Je sais où trouver les logs selon le type de problème
- [ ] Je maîtrise journalctl pour analyser les logs systemd
- [ ] Je peux utiliser grep, awk, sed pour filtrer les logs
- [ ] Je connais les outils de diagnostic (df, free, top, etc.)
- [ ] Je sais diagnostiquer un serveur lent
- [ ] Je peux résoudre un problème de disque plein
- [ ] J'applique une méthodologie systématique de dépannage
- [ ] J'ai résolu avec succès les 5 pannes du TP

---

<div align="center">

**🎉 Félicitations ! Vous avez terminé le module Linux N1/N2 !**

[⬅️ Retour au sommaire](README.md)

</div>
