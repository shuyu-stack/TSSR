# Commandes Linux essentielles de survie - Votre kit de premier secours

> 📚 **Module :** Linux Administration - Fondamentaux
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Débutant (N1/N2)
> 👨‍🏫 **Approche :** Admin système → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Navigation dans le système](#-navigation-dans-le-système)
- [Manipulation de fichiers](#-manipulation-de-fichiers)
- [Lecture et recherche](#-lecture-et-recherche)
- [Les éditeurs de texte](#-les-éditeurs-de-texte)
- [TP Pratique : Dépannage Apache](#-tp-pratique--dépannage-apache)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**2h du matin, un samedi de juillet 2012.** Mon téléphone sonne. Le serveur web de prod de mon plus gros client est down. 50 000€ de CA/jour. Je me connecte en SSH depuis mon salon, en pyjama, café à la main.

**Pas d'interface graphique. Juste un terminal noir avec un curseur qui clignote.**

En **7 minutes**, j'ai :
1. Identifié le problème (Apache crashé)
2. Trouvé la cause (fichier de config corrompu)
3. Restauré la sauvegarde
4. Redémarré le service

**Total : 7 minutes. Pas une fenêtre graphique ouverte.**

**Le client m'a dit :** "Comment tu fais ça ? C'est de la magie ?"

**Non, ce n'est pas de la magie.** Ce sont **15 commandes Linux** que je connais par cœur. Les mêmes que vous allez apprendre aujourd'hui.

### 🎯 Ma promesse

À la fin de ces 4 heures, vous serez capable de :
- ✅ Vous déplacer dans un système Linux sans avoir peur
- ✅ Trouver n'importe quel fichier en quelques secondes
- ✅ Lire et analyser des logs pour diagnostiquer une panne
- ✅ Éditer un fichier de configuration sans tout casser

**Ces commandes vont vous sauver la vie.** Littéralement. Je les utilise **tous les jours** depuis 15 ans.

Allez, on y va ! 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Naviguer** dans l'arborescence Linux sans interface graphique
- ✅ **Créer, copier, déplacer, supprimer** des fichiers et dossiers
- ✅ **Lire** des fichiers de configuration et des logs
- ✅ **Rechercher** des fichiers par nom, taille ou date
- ✅ **Utiliser** les éditeurs nano et vim (bases)
- ✅ **Dépanner** un problème réel sur un serveur web

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir des notions de base en informatique
- [ ] Comprendre ce qu'est un fichier, un dossier, un chemin
- [ ] Avoir un accès à un terminal Linux (VM, WSL, ou serveur distant)

**Matériel nécessaire :**
- 💻 PC avec Linux, WSL ou connexion SSH à un serveur
- 📝 De quoi prendre des notes
- ☕ Du café (optionnel mais recommandé)

---

## 🧭 Navigation dans le système

### L'arborescence Linux vs Windows

```
┌─────────────────────────────────────────────────────────────┐
│  WINDOWS                                                    │
├─────────────────────────────────────────────────────────────┤
│  C:\                    (racine du lecteur C)               │
│  ├── Windows\           (système)                           │
│  ├── Program Files\     (applications)                      │
│  └── Users\             (utilisateurs)                      │
│      └── John\                                              │
│          ├── Desktop\                                       │
│          └── Documents\                                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  LINUX                                                      │
├─────────────────────────────────────────────────────────────┤
│  /                      (racine UNIQUE)                     │
│  ├── bin/               (commandes de base)                 │
│  ├── etc/               (fichiers de configuration)         │
│  ├── home/              (dossiers des utilisateurs)         │
│  │   └── john/                                              │
│  ├── var/               (données variables)                 │
│  │   └── log/           (LOGS - vos meilleurs amis !)      │
│  ├── tmp/               (fichiers temporaires)              │
│  └── usr/               (programmes utilisateurs)           │
└─────────────────────────────────────────────────────────────┘
```

**Différence clé :** Linux a **UNE SEULE racine** (`/`), pas de lecteurs C:, D:, etc.

### pwd - Print Working Directory

**Traduction :** "Où suis-je ?"

```bash
pwd
# Affiche : /home/john/documents
```

> 💡 **Astuce :** Perdu ? Tapez `pwd`. Toujours.

### cd - Change Directory

**Les commandes essentielles :**

```bash
cd /home/john/documents    # Chemin absolu (depuis la racine /)
cd documents               # Chemin relatif (depuis où je suis)
cd ..                      # Remonter d'un niveau (parent)
cd ~                       # Aller dans MON répertoire personnel
cd /                       # Aller à la racine
cd -                       # Retourner au dossier précédent
```

**Exemple pratique :**

```bash
pwd                        # Je suis dans /home/john
cd /var/log                # Je vais dans /var/log
pwd                        # Affiche : /var/log
cd -                       # Je retourne d'où je viens
pwd                        # Affiche : /home/john
```

> 💡 **Astuce de PRO :** Appuyez sur **Tab** pour auto-compléter. C'est votre meilleur ami !

```bash
cd /var/lo<TAB>            # Se complète en : cd /var/log/
cd /etc/apa<TAB>           # Se complète en : cd /etc/apache2/
```

### ls - List

**Lister le contenu d'un dossier :**

```bash
ls                         # Liste simple
ls -l                      # Liste détaillée (long format)
ls -a                      # Affiche les fichiers cachés (commençant par .)
ls -la                     # Les deux combinés
ls -lh                     # Tailles lisibles (K, M, G)
ls -ltr                    # Trié par date (le plus récent en bas)
```

**Exemple de sortie :**

```bash
$ ls -lh
drwxr-xr-x 2 john john 4.0K Feb  9 10:30 documents
-rw-r--r-- 1 john john 1.2M Feb  9 09:15 rapport.pdf
-rwxr-xr-x 1 john john  512 Feb  8 14:22 script.sh
```

**Décryptage :**

```
-rw-r--r--  1  john  john  1.2M  Feb 9 09:15  rapport.pdf
│││││││││││  │   │     │    │      │           │
│││││││││││  │   │     │    │      │           └─ Nom du fichier
│││││││││││  │   │     │    │      └─ Date de modification
│││││││││││  │   │     │    └─ Taille
│││││││││││  │   │     └─ Groupe propriétaire
│││││││││││  │   └─ Propriétaire
│││││││││││  └─ Nombre de liens
│││││││││││
││││││││││└─ Autres : lecture
│││││││││└─ Autres : écriture
││││││││└─ Autres : exécution
│││││││└─ Groupe : lecture
││││││└─ Groupe : écriture
│││││└─ Groupe : exécution
││││└─ Proprio : lecture
│││└─ Proprio : écriture
││└─ Proprio : exécution
│└─ Type : - = fichier, d = dossier, l = lien
```

> 💡 **Mon alias préféré :** `alias ll='ls -lha'` (je gagne 5 secondes par jour, soit 30 minutes par an !)

### Les chemins spéciaux

```bash
.          # Répertoire courant
..         # Répertoire parent
~          # Mon répertoire personnel (/home/john)
/          # Racine du système
```

**Exemple pratique :**

```bash
cd /var/log/apache2
ls .                       # Liste le contenu courant (apache2)
ls ..                      # Liste le contenu du parent (log)
cd ~/documents             # Va dans /home/john/documents
```

---

## 📁 Manipulation de fichiers

### touch - Créer un fichier vide

```bash
touch fichier.txt          # Crée un fichier vide
touch file1.txt file2.txt  # Crée plusieurs fichiers
```

**Usage réel :** Souvent utilisé pour mettre à jour la date de modification d'un fichier existant.

### mkdir - Make Directory

```bash
mkdir documents            # Crée un dossier
mkdir -p projet/src/main   # Crée toute l'arborescence (-p = parents)
```

**Sans -p :**
```bash
mkdir projet/src/main      # ERREUR si projet/ n'existe pas
```

**Avec -p :**
```bash
mkdir -p projet/src/main   # Crée projet/, puis src/, puis main/
```

### cp - Copy

```bash
cp fichier.txt copie.txt              # Copie simple
cp fichier.txt /tmp/                  # Copie vers un dossier
cp -r dossier/ /backup/               # Copie récursive (-r)
cp -p fichier.txt copie.txt           # Préserve permissions (-p)
cp -v fichier.txt copie.txt           # Mode verbeux (-v)
```

**Exemple réel :**

```bash
# Sauvegarder un fichier de config avant modif
cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.backup
```

> ⚠️ **IMPORTANT :** `cp` écrase sans demander ! Utilisez `cp -i` pour demander confirmation.

### mv - Move (et renommer)

```bash
mv ancien.txt nouveau.txt             # Renommer
mv fichier.txt /tmp/                  # Déplacer
mv *.txt documents/                   # Déplacer tous les .txt
```

**Astuce :** `mv` est AUSSI utilisé pour renommer !

```bash
mv mon_fichier_avec_un_nom_trop_long.txt court.txt
```

### rm - Remove

```bash
rm fichier.txt                        # Supprimer un fichier
rm -r dossier/                        # Supprimer un dossier (-r = récursif)
rm -f fichier.txt                     # Forcer (-f = force, pas de confirmation)
rm -rf dossier/                       # Supprimer dossier + contenu (DANGER !)
```

> ⚠️ **ATTENTION EXTRÊME DANGER :**

```bash
rm -rf /         # DÉTRUIT TOUT LE SYSTÈME
rm -rf /*        # PAREIL - NE JAMAIS FAIRE
```

**Histoire vraie (2015) :** Un stagiaire a tapé `rm -rf /tmp/*` mais a **fait un espace** : `rm -rf / tmp/*`

**Résultat :** Il a commencé à supprimer **la racine** avant qu'on coupe le serveur. 3 heures de restauration.

> 💡 **Conseil de PRO :** Avant un `rm -rf`, faites un `ls` pour vérifier que vous êtes au bon endroit.

### Wildcards (caractères jokers)

```bash
*              # N'importe quelle chaîne de caractères
?              # Un seul caractère
[abc]          # a, b ou c
[0-9]          # Chiffre de 0 à 9
```

**Exemples :**

```bash
ls *.txt                  # Tous les fichiers .txt
ls file?.txt              # file1.txt, file2.txt, fileA.txt
ls [Rr]apport*            # Rapport.pdf, rapport_2025.docx
rm test[1-3].log          # test1.log, test2.log, test3.log
```

### ln - Liens symboliques

**Lien symbolique = raccourci Windows**

```bash
ln -s /var/log/apache2/access.log ~/access.log
```

**Vérification :**

```bash
$ ls -l ~/access.log
lrwxrwxrwx 1 john john 29 Feb  9 10:45 access.log -> /var/log/apache2/access.log
```

**Usage réel :** Créer des raccourcis vers des dossiers fréquents.

```bash
ln -s /var/www/html ~/www
cd ~/www                  # Équivaut à : cd /var/www/html
```

---

## 📖 Lecture et recherche

### cat - Concatenate

**Afficher tout le contenu d'un fichier :**

```bash
cat fichier.txt
cat /etc/hosts
```

**Concaténer plusieurs fichiers :**

```bash
cat file1.txt file2.txt > fusion.txt
```

> ⚠️ **Attention :** `cat` affiche TOUT. Sur un gros fichier, ça défile trop vite !

### less et more

**Pour lire un fichier page par page :**

```bash
less /var/log/syslog      # Lecture avec navigation
more /var/log/syslog      # Lecture simple (moins puissant)
```

**Navigation dans less :**

```
Espace          # Page suivante
b               # Page précédente
/motif          # Rechercher "motif"
n               # Occurrence suivante
q               # Quitter
```

> 💡 **Conseil :** Utilisez **less**, pas **more**. Less is more ! 😄

### head et tail

**Lire le début d'un fichier :**

```bash
head /var/log/syslog           # 10 premières lignes
head -n 20 fichier.txt         # 20 premières lignes
```

**Lire la fin d'un fichier :**

```bash
tail /var/log/syslog           # 10 dernières lignes
tail -n 50 fichier.txt         # 50 dernières lignes
```

**LA COMMANDE LA PLUS UTILE DE VOTRE CARRIÈRE :**

```bash
tail -f /var/log/apache2/error.log
```

**`tail -f` = suivre en temps réel !**

**Scénario réel :**

```bash
# Terminal 1 : Vous regardez le log en temps réel
tail -f /var/log/apache2/error.log

# Terminal 2 : Vous testez votre site web
curl http://localhost

# Terminal 1 : Les erreurs apparaissent EN DIRECT !
[Sat Feb 09 11:23:45] [error] File not found: /var/www/html/test.php
```

> 💡 **Astuce :** `tail -f` est votre **arme n°1** pour débugger !

### grep - Recherche dans le contenu

**Rechercher un mot dans un fichier :**

```bash
grep "error" /var/log/syslog
grep -i "error" /var/log/syslog         # Insensible à la casse (-i)
grep -r "TODO" /var/www/html/           # Recherche récursive (-r)
grep -n "function" script.sh            # Affiche les numéros de ligne (-n)
```

**Combinaison puissante :**

```bash
grep -i "failed" /var/log/auth.log | less
```

**Exemple réel : Trouver toutes les tentatives de connexion SSH échouées**

```bash
grep "Failed password" /var/log/auth.log
```

**Sortie :**

```
Feb  9 08:15:23 server sshd[12345]: Failed password for invalid user admin from 192.168.1.50
Feb  9 08:15:28 server sshd[12347]: Failed password for root from 192.168.1.50
```

> 💡 **Combo magique :** `tail -f log.txt | grep --color "ERROR"`

---

## 🔍 Recherche de fichiers

### find - Recherche par critères

**Syntaxe :** `find [où] [critère] [action]`

**Par nom :**

```bash
find /var/log -name "*.log"                    # Tous les .log
find /etc -name "apache*"                      # Commence par apache
find . -name "test.txt"                        # Dans le dossier courant
```

**Par taille :**

```bash
find /var/log -size +100M                      # Fichiers > 100 Mo
find /tmp -size +1G                            # Fichiers > 1 Go
find . -size -10k                              # Fichiers < 10 Ko
```

**Par date :**

```bash
find /var/log -mtime -1                        # Modifiés depuis 24h
find /tmp -mtime +30                           # Modifiés il y a +30 jours
find . -mtime 0                                # Modifiés aujourd'hui
```

**Combinaisons :**

```bash
# Trouver les .log de plus de 100 Mo
find /var/log -name "*.log" -size +100M

# Supprimer les fichiers temporaires de +7 jours
find /tmp -type f -mtime +7 -delete
```

**Histoire vraie (2018) :**

Un client m'appelle : "Le serveur est plein, impossible de travailler !"

```bash
df -h                      # Disque plein à 100%
find / -size +1G           # Cherche les gros fichiers

# Résultat : Un fichier de log de 50 Go !
/var/log/apache2/access.log    50G
```

**Solution :**

```bash
> /var/log/apache2/access.log  # Vide le fichier (sans le supprimer)
```

**5 minutes. Problème résolu.**

### locate - Recherche rapide

**Plus rapide que find (utilise une base de données indexée) :**

```bash
locate apache2.conf
locate "*.pdf"
```

**Mettre à jour la base :**

```bash
sudo updatedb
```

> 💡 **Astuce :** `locate` est ultra-rapide, mais ne trouve que les fichiers existants lors du dernier `updatedb`.

### which et whereis

**Trouver où est une commande :**

```bash
which python               # /usr/bin/python
which ls                   # /bin/ls
whereis apache2            # apache2: /usr/sbin/apache2 /etc/apache2
```

---

## ✏️ Les éditeurs de texte

### nano - L'éditeur pour débutants

**Lancer nano :**

```bash
nano fichier.txt
```

**Raccourcis essentiels :**

```
Ctrl + O        # Enregistrer (Write Out)
Ctrl + X        # Quitter
Ctrl + K        # Couper la ligne
Ctrl + U        # Coller
Ctrl + W        # Rechercher
```

**Exemple pratique :**

```bash
sudo nano /etc/hosts
# Ajouter une ligne :
192.168.1.100   monserveur.local
# Ctrl+O → Entrée → Ctrl+X
```

> 💡 **Conseil :** nano est parfait pour débuter. Simple, intuitif, les raccourcis sont affichés en bas.

### vim - L'éditeur des pros

**Pourquoi apprendre vim ?**

- Sur 90% des serveurs, c'est LE SEUL éditeur installé
- Ultra-puissant une fois maîtrisé
- Léger et rapide

**Les 3 modes de vim :**

```
┌─────────────────────────────────────────────────────────────┐
│  MODE NORMAL (par défaut)                                   │
│  → Déplacement, copier, coller, supprimer                   │
│  → Appuyer sur i pour passer en mode Insertion              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  MODE INSERTION (après avoir appuyé sur i)                  │
│  → Écrire du texte normalement                              │
│  → Appuyer sur Esc pour retourner en mode Normal            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  MODE COMMANDE (après avoir tapé : en mode Normal)          │
│  → :w = enregistrer                                         │
│  → :q = quitter                                             │
│  → :wq = enregistrer et quitter                             │
│  → :q! = quitter sans enregistrer                           │
└─────────────────────────────────────────────────────────────┘
```

**Survie vim en 10 commandes :**

```bash
vim fichier.txt            # Ouvrir le fichier

i                          # Passer en mode Insertion
<tapez votre texte>
Esc                        # Retour mode Normal

:w                         # Enregistrer
:q                         # Quitter
:wq                        # Enregistrer et quitter
:q!                        # Quitter SANS enregistrer (force)

/motif                     # Rechercher "motif"
n                          # Occurrence suivante
```

**Histoire drôle :** Il y a une blague dans le monde Linux :

> "Comment générer une chaîne aléatoire ?"
> "Mettez un débutant devant vim et demandez-lui de quitter."

**Résultat typique :** `:wq:q!exitquithelp:exit^C^C^C`

> 💡 **Mon conseil :** Apprenez au minimum à **ouvrir, éditer, sauver et quitter** avec vim. Un jour, vous serez bloqué sur un serveur sans nano.

---

## 🚨 TP Pratique : Dépannage Apache

### Scénario

Vous êtes admin système. Un développeur vous appelle à 14h :

**"Le site web ne marche plus ! J'ai juste modifié le fichier de config Apache ce matin..."**

**Votre mission :** Trouver et corriger le problème.

### Étape 1 : Connexion au serveur

```bash
ssh admin@192.168.1.100
# Mot de passe : ********
```

### Étape 2 : Vérifier l'état du service

```bash
sudo systemctl status apache2
```

**Résultat :**

```
● apache2.service - The Apache HTTP Server
   Loaded: loaded
   Active: failed (Result: exit-code)
```

**Analyse :** Apache a planté. Il faut savoir pourquoi.

### Étape 3 : Consulter les logs

```bash
cd /var/log/apache2
ls -ltr                    # Les logs les plus récents en bas
tail -n 50 error.log
```

**Vous trouvez :**

```
[Sat Feb 09 09:15:32] [error] Syntax error on line 234 of /etc/apache2/apache2.conf:
Invalid command 'ServerNam', perhaps misspelled or defined by a module not included
```

**Bingo !** Ligne 234, faute de frappe : `ServerNam` au lieu de `ServerName`.

### Étape 4 : Naviguer vers le fichier

```bash
cd /etc/apache2
ls -l apache2.conf
```

### Étape 5 : Faire une sauvegarde

```bash
sudo cp apache2.conf apache2.conf.backup-$(date +%Y%m%d-%H%M%S)
ls -l
```

**Résultat :**

```
-rw-r--r-- 1 root root 7224 Feb  9 09:15 apache2.conf
-rw-r--r-- 1 root root 7224 Feb  9 14:23 apache2.conf.backup-20260209-142315
```

### Étape 6 : Éditer le fichier

```bash
sudo nano apache2.conf
```

**Aller à la ligne 234 :** Ctrl+W (recherche) → tapez `ServerNam` → Entrée

**Corriger :**

```
Avant : ServerNam www.example.com
Après : ServerName www.example.com
```

**Enregistrer :** Ctrl+O → Entrée → Ctrl+X

### Étape 7 : Tester la configuration

```bash
sudo apache2ctl configtest
```

**Résultat :**

```
Syntax OK
```

**Parfait !** La syntaxe est bonne.

### Étape 8 : Redémarrer Apache

```bash
sudo systemctl restart apache2
sudo systemctl status apache2
```

**Résultat :**

```
● apache2.service - The Apache HTTP Server
   Loaded: loaded
   Active: active (running)
```

**✅ SUCCÈS !**

### Étape 9 : Tester dans le navigateur

```bash
curl http://localhost
```

**Résultat :**

```html
<html>
<h1>It works!</h1>
</html>
```

### Récapitulatif des commandes utilisées

```bash
ssh admin@serveur                               # Connexion
sudo systemctl status apache2                   # État service
cd /var/log/apache2                             # Aller dans les logs
tail -n 50 error.log                            # Lire les erreurs
cd /etc/apache2                                 # Aller dans la config
sudo cp apache2.conf apache2.conf.backup        # Sauvegarde
sudo nano apache2.conf                          # Édition
sudo apache2ctl configtest                      # Test syntaxe
sudo systemctl restart apache2                  # Redémarrage
```

**Temps total : 7 minutes.**

**Vous venez de sauver la journée.** 🎉

---

## 🎯 Exercices pratiques

### Exercice 1 : Créer une arborescence

**Objectif :** Créer une structure de projet web.

**Consignes :**

1. Créer cette arborescence dans votre home :
```
~/monprojet/
├── src/
│   ├── css/
│   ├── js/
│   └── images/
├── config/
└── logs/
```

2. Créer un fichier `index.html` dans `src/`
3. Vérifier avec `tree` ou `ls -R`

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# Créer toute l'arborescence d'un coup
mkdir -p ~/monprojet/{src/{css,js,images},config,logs}

# Créer le fichier index.html
touch ~/monprojet/src/index.html

# Vérifier
cd ~/monprojet
ls -R

# Ou avec tree (si installé)
tree
```

**Résultat :**

```
monprojet/
├── config
├── logs
└── src
    ├── css
    ├── images
    ├── index.html
    └── js
```

</details>

---

### Exercice 2 : Copier avec préservation

**Objectif :** Sauvegarder un fichier de config en préservant les permissions.

**Consignes :**

1. Créer un fichier `/tmp/config.ini` avec des permissions spécifiques :
   ```bash
   touch /tmp/config.ini
   chmod 640 /tmp/config.ini
   ```

2. Le copier dans `/tmp/backup/` en **préservant** les permissions
3. Vérifier que les permissions sont identiques

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# Créer le fichier avec permissions
touch /tmp/config.ini
chmod 640 /tmp/config.ini

# Vérifier les permissions initiales
ls -l /tmp/config.ini
# -rw-r----- 1 john john 0 Feb  9 14:30 /tmp/config.ini

# Créer le dossier backup
mkdir -p /tmp/backup

# Copier avec préservation des permissions
cp -p /tmp/config.ini /tmp/backup/

# Vérifier
ls -l /tmp/backup/config.ini
# -rw-r----- 1 john john 0 Feb  9 14:30 /tmp/backup/config.ini
```

**Explication :** L'option `-p` préserve :
- Les permissions (mode)
- Le propriétaire (si possible)
- Les dates (modification, accès)

</details>

---

### Exercice 3 : Trouver les gros fichiers

**Objectif :** Libérer de l'espace disque en trouvant les fichiers volumineux.

**Consignes :**

1. Créer des fichiers de test de différentes tailles :
   ```bash
   dd if=/dev/zero of=/tmp/small.dat bs=1M count=5
   dd if=/dev/zero of=/tmp/medium.dat bs=1M count=50
   dd if=/dev/zero of=/tmp/large.dat bs=1M count=150
   ```

2. Trouver tous les fichiers de `/tmp` supérieurs à 100 Mo
3. Lister leur taille de façon lisible
4. Les supprimer

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# Créer les fichiers de test
dd if=/dev/zero of=/tmp/small.dat bs=1M count=5
dd if=/dev/zero of=/tmp/medium.dat bs=1M count=50
dd if=/dev/zero of=/tmp/large.dat bs=1M count=150

# Trouver les fichiers > 100 Mo
find /tmp -type f -size +100M

# Résultat :
# /tmp/large.dat

# Afficher avec taille lisible
find /tmp -type f -size +100M -exec ls -lh {} \;

# Résultat :
# -rw-r--r-- 1 john john 150M Feb  9 14:35 /tmp/large.dat

# Supprimer
find /tmp -type f -size +100M -delete

# Vérifier
find /tmp -type f -size +100M
# (aucun résultat)
```

**Bonus : Trouver les 10 plus gros fichiers du système**

```bash
sudo find / -type f -exec du -h {} + 2>/dev/null | sort -rh | head -n 10
```

</details>

---

### Exercice 4 : Analyser un log

**Objectif :** Extraire des informations d'un fichier de log.

**Consignes :**

1. Créer un fichier de log de test :
   ```bash
   cat > /tmp/access.log << 'EOF'
   192.168.1.10 - [09/Feb/2026:10:15:23] "GET /index.html HTTP/1.1" 200
   192.168.1.15 - [09/Feb/2026:10:16:45] "GET /about.html HTTP/1.1" 200
   192.168.1.10 - [09/Feb/2026:10:17:12] "GET /login.php HTTP/1.1" 404
   192.168.1.20 - [09/Feb/2026:10:18:33] "POST /api/data HTTP/1.1" 500
   192.168.1.15 - [09/Feb/2026:10:19:01] "GET /contact.html HTTP/1.1" 200
   192.168.1.10 - [09/Feb/2026:10:20:22] "GET /admin.php HTTP/1.1" 403
   EOF
   ```

2. Trouver toutes les erreurs (codes 4xx et 5xx)
3. Compter combien de fois 192.168.1.10 apparaît
4. Extraire uniquement les URLs demandées

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer le fichier (commande fournie)
cat > /tmp/access.log << 'EOF'
192.168.1.10 - [09/Feb/2026:10:15:23] "GET /index.html HTTP/1.1" 200
192.168.1.15 - [09/Feb/2026:10:16:45] "GET /about.html HTTP/1.1" 200
192.168.1.10 - [09/Feb/2026:10:17:12] "GET /login.php HTTP/1.1" 404
192.168.1.20 - [09/Feb/2026:10:18:33] "POST /api/data HTTP/1.1" 500
192.168.1.15 - [09/Feb/2026:10:19:01] "GET /contact.html HTTP/1.1" 200
192.168.1.10 - [09/Feb/2026:10:20:22] "GET /admin.php HTTP/1.1" 403
EOF

# 2. Trouver les erreurs (4xx et 5xx)
grep -E " [45][0-9]{2}$" /tmp/access.log

# Résultat :
# 192.168.1.10 - [09/Feb/2026:10:17:12] "GET /login.php HTTP/1.1" 404
# 192.168.1.20 - [09/Feb/2026:10:18:33] "POST /api/data HTTP/1.1" 500
# 192.168.1.10 - [09/Feb/2026:10:20:22] "GET /admin.php HTTP/1.1" 403

# 3. Compter les occurrences de 192.168.1.10
grep -c "192.168.1.10" /tmp/access.log

# Résultat : 3

# 4. Extraire les URLs
grep -o '"[A-Z]* /[^"]*' /tmp/access.log | cut -d' ' -f2

# Résultat :
# /index.html
# /about.html
# /login.php
# /api/data
# /contact.html
# /admin.php
```

</details>

---

## 📚 Ressources

### Documentation officielle

- [GNU Core Utils Manual](https://www.gnu.org/software/coreutils/manual/)
- [Linux Command Line Basics](https://ubuntu.com/tutorials/command-line-for-beginners)
- [Vim Documentation](https://www.vim.org/docs.php)

### Cheat Sheets

- [Linux Command Cheat Sheet](https://cheatography.com/davechild/cheat-sheets/linux-command-line/)
- [Vim Cheat Sheet](https://vim.rtorr.com/)

### Tutoriels interactifs

- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) - Apprendre en jouant
- [Linux Journey](https://linuxjourney.com/) - Tutoriel progressif

### Livres recommandés

- "The Linux Command Line" par William Shotts (gratuit en PDF)
- "Unix and Linux System Administration Handbook" par Evi Nemeth

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Commandes que je dois pratiquer :**
-
-
-

**Questions à poser :**
-
-

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Je sais naviguer avec `cd`, `pwd`, `ls`
- [ ] Je comprends les chemins absolus vs relatifs
- [ ] Je maîtrise `cp`, `mv`, `rm` (avec prudence !)
- [ ] Je sais créer des dossiers avec `mkdir -p`
- [ ] Je peux lire des fichiers avec `cat`, `less`, `tail`
- [ ] Je connais `tail -f` pour suivre les logs
- [ ] Je sais chercher avec `grep` et `find`
- [ ] Je peux éditer un fichier avec `nano`
- [ ] Je connais les bases de survie de `vim`
- [ ] J'ai résolu le TP Apache avec succès

---

<div align="center">

**Cours suivant :** [02-utilisateurs-permissions-groupes.md](02-utilisateurs-permissions-groupes.md)

[⬅️ Retour au sommaire](README.md)

</div>
