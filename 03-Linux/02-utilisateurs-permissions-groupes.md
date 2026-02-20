# Utilisateurs, Permissions et Groupes - La sécurité avant tout

> 📚 **Module :** Linux Administration - Sécurité de base
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Débutant/Intermédiaire (N1/N2)
> 👨‍🏫 **Approche :** Admin système → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Gestion des utilisateurs](#-gestion-des-utilisateurs)
- [Gestion des groupes](#-gestion-des-groupes)
- [Permissions Unix](#-permissions-unix)
- [Permissions spéciales](#-permissions-spéciales)
- [Sudo et élévation de privilèges](#-sudo-et-élévation-de-privilèges)
- [TP Pratique : Structure d'entreprise](#-tp-pratique--structure-dentreprise)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**Mars 2013. 3h du matin.** Je reçois un appel d'un client paniqué :

**"Notre serveur de fichiers est inaccessible ! Plus personne ne peut travailler !"**

Je me connecte. Je regarde. **Horreur.**

Un stagiaire, **la veille**, a voulu "arranger" les permissions d'un dossier qui posait problème. Il a tapé :

```bash
chmod 777 -R /
```

**Traduction :** "Donne TOUS les droits à TOUT LE MONDE sur TOUT LE SYSTÈME."

**Résultat :**
- Le système ne bootait plus
- SSH refusait de démarrer (permissions /etc/ssh trop ouvertes)
- Les mots de passe étaient lisibles par tous (/etc/shadow en 777)

**6 heures de restauration depuis backup. 30 000€ de perte pour le client.**

### 🎯 La leçon

**Les permissions Linux, ce n'est PAS optionnel.**

C'est la **BASE de la sécurité** de votre système. Mal comprises, mal configurées, elles peuvent :
- ❌ Détruire votre système
- ❌ Exposer des données confidentielles
- ❌ Créer des failles de sécurité énormes

**Mais bien maîtrisées :**
- ✅ Elles protègent votre infrastructure
- ✅ Elles permettent la collaboration sécurisée
- ✅ Elles structurent l'organisation

**Je vais vous apprendre à les maîtriser.** Pas juste à les comprendre. À les **maîtriser**.

Allez, on démarre ! 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Créer et gérer** des utilisateurs et groupes Linux
- ✅ **Comprendre** le système de permissions Unix (rwx)
- ✅ **Appliquer** les bonnes permissions selon le contexte
- ✅ **Utiliser** les permissions spéciales (SUID, SGID, Sticky bit)
- ✅ **Configurer** sudo de façon sécurisée
- ✅ **Diagnostiquer** des problèmes de droits d'accès
- ✅ **Créer** une structure d'entreprise avec utilisateurs et permissions

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les commandes de base Linux (cours 01)
- [ ] Savoir naviguer dans l'arborescence
- [ ] Comprendre ce qu'est un fichier, un dossier
- [ ] Avoir un accès root ou sudo sur un système Linux

**Matériel nécessaire :**
- 💻 Linux (VM Ubuntu/Debian recommandé)
- 🔑 Accès administrateur (sudo)
- 📝 De quoi prendre des notes

---

## 👤 Gestion des utilisateurs

### Les fichiers système

**Linux stocke les infos utilisateurs dans des fichiers texte :**

```
┌─────────────────────────────────────────────────────────────┐
│  /etc/passwd - Informations des utilisateurs               │
├─────────────────────────────────────────────────────────────┤
│  Format : login:x:UID:GID:commentaire:home:shell            │
│                                                             │
│  Exemple :                                                  │
│  john:x:1000:1000:John Doe:/home/john:/bin/bash             │
│                                                             │
│  • login : nom d'utilisateur                                │
│  • x : mot de passe (stocké dans /etc/shadow)               │
│  • UID : User ID (identifiant numérique)                    │
│  • GID : Group ID (groupe principal)                        │
│  • commentaire : nom complet, infos                         │
│  • home : répertoire personnel                              │
│  • shell : shell par défaut                                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  /etc/shadow - Mots de passe chiffrés                      │
├─────────────────────────────────────────────────────────────┤
│  Format : login:hash:lastchange:min:max:warn:inactive:expire│
│                                                             │
│  Exemple :                                                  │
│  john:$6$random$hashedpassword:19387:0:99999:7:::           │
│                                                             │
│  ⚠️ Fichier SENSIBLE - Permissions 640 (root:shadow)        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  /etc/group - Informations des groupes                     │
├─────────────────────────────────────────────────────────────┤
│  Format : nom:x:GID:membres                                 │
│                                                             │
│  Exemple :                                                  │
│  developers:x:1001:john,alice,bob                           │
└─────────────────────────────────────────────────────────────┘
```

### Créer un utilisateur

**Syntaxe complète :**

```bash
sudo useradd [options] nom_utilisateur
```

**Options principales :**

```bash
-m              # Créer le home (/home/utilisateur)
-d /path        # Spécifier un home custom
-s /bin/bash    # Définir le shell
-c "commentaire" # Ajouter un commentaire
-G groupe1,groupe2 # Ajouter à des groupes secondaires
-e 2026-12-31   # Date d'expiration du compte
```

**Exemple pratique :**

```bash
# Créer un utilisateur complet
sudo useradd -m -s /bin/bash -c "Jean Dupont - Développeur" jdupont

# Définir son mot de passe
sudo passwd jdupont
# Taper le mot de passe 2 fois
```

**Vérification :**

```bash
# Voir l'entrée dans /etc/passwd
grep jdupont /etc/passwd
# jdupont:x:1001:1001:Jean Dupont - Développeur:/home/jdupont:/bin/bash

# Vérifier que le home existe
ls -ld /home/jdupont
# drwxr-x--- 2 jdupont jdupont 4096 Feb  9 15:30 /home/jdupont
```

### adduser vs useradd

**Sur Debian/Ubuntu, il existe aussi `adduser` (script interactif) :**

```bash
sudo adduser jdupont
```

**Comparaison :**

```
┌──────────────────────────────────────────────────────────┐
│  useradd (commande de bas niveau)                       │
├──────────────────────────────────────────────────────────┤
│  • Disponible sur toutes les distributions               │
│  • Nécessite de spécifier toutes les options            │
│  • Ne crée PAS le home par défaut (besoin de -m)        │
│  • Ne demande PAS le mot de passe                        │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  adduser (script interactif Debian/Ubuntu)              │
├──────────────────────────────────────────────────────────┤
│  • Spécifique Debian/Ubuntu                              │
│  • Interface interactive                                 │
│  • Crée automatiquement le home                          │
│  • Demande le mot de passe                               │
│  • Copie les fichiers squelette (/etc/skel)             │
└──────────────────────────────────────────────────────────┘
```

> 💡 **Mon conseil :** Sur Debian/Ubuntu, utilisez `adduser` (plus simple). Sur RedHat/CentOS, utilisez `useradd -m`.

### Modifier un utilisateur

```bash
# Changer le shell
sudo usermod -s /bin/zsh jdupont

# Ajouter à un groupe secondaire (sans supprimer les autres)
sudo usermod -aG sudo jdupont

# Changer le home
sudo usermod -d /home/nouveau_home jdupont

# Verrouiller un compte (désactiver)
sudo usermod -L jdupont

# Déverrouiller
sudo usermod -U jdupont
```

> ⚠️ **ATTENTION :** `usermod -G` (sans -a) **ÉCRASE** tous les groupes secondaires !

**MAUVAIS :**
```bash
sudo usermod -G developers jdupont  # Supprime tous les autres groupes !
```

**BON :**
```bash
sudo usermod -aG developers jdupont # Ajoute developers sans supprimer les autres
```

### Supprimer un utilisateur

```bash
# Supprimer l'utilisateur (garde le home)
sudo userdel jdupont

# Supprimer l'utilisateur ET son home
sudo userdel -r jdupont
```

> 💡 **Astuce :** Avant de supprimer, vérifiez s'il possède des fichiers ailleurs :

```bash
sudo find / -user jdupont 2>/dev/null
```

### Changer de mot de passe

```bash
# Changer SON PROPRE mot de passe
passwd

# Changer le mot de passe d'un autre user (root uniquement)
sudo passwd jdupont

# Forcer le changement au prochain login
sudo passwd -e jdupont
```

### Informations utilisateur

```bash
# Qui suis-je ?
whoami

# Mon UID, GID et groupes
id

# Détails sur un utilisateur
id jdupont

# Qui est connecté ?
who

# Historique de connexion
last
```

**Exemple :**

```bash
$ id
uid=1000(john) gid=1000(john) groups=1000(john),27(sudo),999(docker)
```

### su - Switch User

**Changer d'utilisateur :**

```bash
su jdupont              # Change vers jdupont (garde l'environnement)
su - jdupont            # Change vers jdupont (nouvel environnement complet)
su                      # Devient root (déconseillé)
```

**Différence su vs su - :**

```bash
# Sans tiret : garde votre PWD et variables d'environnement
$ pwd
/home/john
$ su jdupont
$ pwd
/home/john              # Toujours dans le home de john !

# Avec tiret : charge l'environnement complet du user
$ pwd
/home/john
$ su - jdupont
$ pwd
/home/jdupont           # Maintenant dans le home de jdupont
```

> 💡 **Conseil :** Utilisez **TOUJOURS** `su -` pour éviter les problèmes d'environnement.

---

## 👥 Gestion des groupes

### Pourquoi des groupes ?

**Scénario sans groupes :**

```
Projet "Alpha" avec 10 développeurs
→ Il faut donner les droits à CHAQUE développeur individuellement
→ Un nouveau dev arrive ? Modifier les droits de 50 dossiers
→ CAUCHEMAR de maintenance
```

**Scénario avec groupes :**

```
Groupe "developers"
→ On donne les droits au GROUPE
→ On ajoute les utilisateurs au groupe
→ Un nouveau dev ? 1 commande : usermod -aG developers nouveau_dev
→ SIMPLE et maintenable
```

### Créer un groupe

```bash
sudo groupadd developers
sudo groupadd compta
sudo groupadd direction
```

**Vérification :**

```bash
grep developers /etc/group
# developers:x:1001:
```

### Ajouter un utilisateur à un groupe

```bash
# Ajouter jdupont au groupe developers
sudo usermod -aG developers jdupont

# Vérifier
groups jdupont
# jdupont : jdupont developers

# Ou avec id
id jdupont
```

> ⚠️ **IMPORTANT :** L'utilisateur doit se **déconnecter/reconnecter** pour que les changements prennent effet !

**Test :**

```bash
# Avant déconnexion
$ groups
john sudo

# Ajouter au groupe
$ sudo usermod -aG developers john

# Toujours pareil (normal)
$ groups
john sudo

# Se déconnecter (Ctrl+D ou exit)
$ exit

# Se reconnecter
$ ssh john@localhost

# Maintenant c'est bon !
$ groups
john sudo developers
```

### Groupe primaire vs secondaires

```
┌─────────────────────────────────────────────────────────────┐
│  GROUPE PRIMAIRE                                            │
├─────────────────────────────────────────────────────────────┤
│  • Défini dans /etc/passwd (4e champ)                       │
│  • Utilisé par défaut pour les NOUVEAUX fichiers créés      │
│  • Un seul groupe primaire par utilisateur                  │
│                                                             │
│  Exemple : john crée un fichier                             │
│  $ touch test.txt                                           │
│  $ ls -l test.txt                                           │
│  -rw-r--r-- 1 john john 0 Feb  9 15:45 test.txt             │
│                        ^^^^ groupe primaire                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  GROUPES SECONDAIRES                                        │
├─────────────────────────────────────────────────────────────┤
│  • Définis dans /etc/group                                  │
│  • Permettent l'ACCÈS aux fichiers de ces groupes           │
│  • Peuvent être multiples                                   │
│                                                             │
│  Exemple : john est dans le groupe "developers"             │
│  → Il peut lire/écrire les fichiers du groupe developers    │
└─────────────────────────────────────────────────────────────┘
```

**Changer le groupe primaire :**

```bash
sudo usermod -g developers jdupont
```

### Supprimer un groupe

```bash
sudo groupdel developers
```

> ⚠️ **Attention :** Ne peut pas supprimer un groupe qui est le groupe primaire d'un utilisateur !

---

## 🔐 Permissions Unix

### Le système rwx

**Chaque fichier/dossier a 3 niveaux de permissions :**

```
-rw-r--r--  1  john  developers  1234  Feb 9 15:45  fichier.txt
│││││││││││
││││││││││└─ Autres (Others) : lecture
│││││││││└─ Autres : PAS d'écriture
││││││││└─ Autres : PAS d'exécution
│││││││└─ Groupe : lecture
││││││└─ Groupe : PAS d'écriture
│││││└─ Groupe : PAS d'exécution
││││└─ Propriétaire (Owner) : lecture
│││└─ Propriétaire : écriture
││└─ Propriétaire : PAS d'exécution
│└─ Type de fichier (- = fichier normal)
```

**Les 3 permissions :**

```
r = Read (lecture)           = 4
w = Write (écriture)         = 2
x = eXecute (exécution)      = 1
```

**Les 3 catégories :**

```
Owner (propriétaire)         = u (user)
Group (groupe)               = g (group)
Others (autres)              = o (others)
```

### Signification selon le type

```
┌─────────────────────────────────────────────────────────────┐
│  POUR UN FICHIER                                            │
├─────────────────────────────────────────────────────────────┤
│  r = Lire le contenu                                        │
│  w = Modifier le contenu                                    │
│  x = Exécuter (si c'est un script/programme)                │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  POUR UN DOSSIER                                            │
├─────────────────────────────────────────────────────────────┤
│  r = Lister le contenu (ls)                                 │
│  w = Créer/supprimer des fichiers dedans                    │
│  x = Traverser le dossier (cd dedans)                       │
│                                                             │
│  ⚠️ x est OBLIGATOIRE pour accéder à un dossier !           │
└─────────────────────────────────────────────────────────────┘
```

**Exemple important :**

```bash
# Dossier avec r-x (pas de w)
drwxr-xr-x 2 john john 4096 Feb 9 fichiers/

$ cd fichiers/            # ✅ OK (x = peut traverser)
$ ls                      # ✅ OK (r = peut lister)
$ touch test.txt          # ❌ ERREUR (pas de w = pas de création)
```

### chmod - Changer les permissions

**2 syntaxes : symbolique et octale**

#### Syntaxe symbolique (débutant-friendly)

```bash
chmod u+x script.sh        # Ajoute exécution pour le propriétaire
chmod g+w fichier.txt      # Ajoute écriture pour le groupe
chmod o-r secret.txt       # Retire lecture pour les autres
chmod a+r public.txt       # Ajoute lecture pour ALL (tous)
```

**Lettres :**
- `u` = user (propriétaire)
- `g` = group
- `o` = others
- `a` = all (tout le monde)

**Opérateurs :**
- `+` = ajouter
- `-` = retirer
- `=` = définir exactement

**Exemples :**

```bash
# Rendre un script exécutable
chmod u+x script.sh

# Donner tous les droits au propriétaire, lecture seule aux autres
chmod u=rwx,go=r fichier.txt

# Retirer TOUS les droits aux autres
chmod o= secret.txt
```

#### Syntaxe octale (pro)

**Plus rapide mais demande de calculer :**

```
r = 4
w = 2
x = 1

Exemples :
rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0
```

**Format : 3 chiffres (owner, group, others)**

```bash
chmod 755 script.sh
# 7 (owner) = rwx
# 5 (group) = r-x
# 5 (others) = r-x
# Résultat : -rwxr-xr-x

chmod 644 fichier.txt
# 6 (owner) = rw-
# 4 (group) = r--
# 4 (others) = r--
# Résultat : -rw-r--r--

chmod 600 secret.txt
# 6 (owner) = rw-
# 0 (group) = ---
# 0 (others) = ---
# Résultat : -rw-------
```

**Permissions courantes :**

```
755 = rwxr-xr-x   # Scripts, dossiers publics
644 = rw-r--r--   # Fichiers normaux
600 = rw-------   # Fichiers privés (clés SSH, configs sensibles)
700 = rwx------   # Dossiers privés
640 = rw-r-----   # Logs, fichiers groupe
```

> 💡 **Astuce mnémotechnique :**
> - 7 = tout
> - 6 = lecture+écriture
> - 5 = lecture+exécution
> - 4 = lecture seule
> - 0 = rien

### chown - Changer le propriétaire

```bash
# Changer le propriétaire
sudo chown john fichier.txt

# Changer propriétaire ET groupe
sudo chown john:developers fichier.txt

# Récursif (tout un dossier)
sudo chown -R john:developers projet/
```

**Syntaxes possibles :**

```bash
chown user fichier            # Change juste le user
chown user:group fichier      # Change user et group
chown :group fichier          # Change juste le group
chown user: fichier           # Change user, groupe = groupe primaire du user
```

### chgrp - Changer le groupe

```bash
# Changer le groupe
sudo chgrp developers fichier.txt

# Récursif
sudo chgrp -R developers projet/
```

> 💡 **Note :** `chgrp` est équivalent à `chown :groupe`

### umask - Masque de création

**Le umask définit les permissions PAR DÉFAUT des nouveaux fichiers.**

```bash
# Voir le umask actuel
umask
# 0022

# Définir un nouveau umask
umask 0027
```

**Calcul :**

```
Permissions max :
  Fichier : 666 (rw-rw-rw-)
  Dossier : 777 (rwxrwxrwx)

Umask : 0022

Permissions effectives :
  Fichier : 666 - 022 = 644 (rw-r--r--)
  Dossier : 777 - 022 = 755 (rwxr-xr-x)
```

**Umasks courants :**

```
0022    # Défaut : fichiers 644, dossiers 755
0027    # Plus sécurisé : fichiers 640, dossiers 750
0077    # Très sécurisé : fichiers 600, dossiers 700 (privé total)
```

---

## 🔒 Permissions spéciales

### SUID - Set User ID (4xxx)

**Permet d'exécuter un fichier avec les droits de son propriétaire.**

**Exemple typique : `passwd`**

```bash
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 68208 Feb  9 2025 /usr/bin/passwd
   ^
   └─ s = SUID
```

**Pourquoi ?**

Un utilisateur normal doit pouvoir changer **son** mot de passe. Mais `/etc/shadow` appartient à root et n'est pas lisible par les autres.

**Solution :** Le programme `passwd` a le SUID. Quand vous l'exécutez, il s'exécute **avec les droits de root**, donc peut modifier `/etc/shadow`.

**Définir le SUID :**

```bash
sudo chmod u+s programme
sudo chmod 4755 programme     # Octal
```

> ⚠️ **DANGER :** Un SUID mal placé est une ÉNORME faille de sécurité !

**Exemple dangereux :**

```bash
# NE JAMAIS FAIRE ÇA
sudo chmod u+s /bin/bash
# → N'importe qui peut obtenir un shell root !
```

### SGID - Set Group ID (2xxx)

**2 usages différents :**

#### 1. Sur un fichier exécutable

Exécute avec les droits du **groupe** propriétaire (rare).

#### 2. Sur un dossier (USAGE PRINCIPAL)

**Tous les fichiers créés dedans auront le même groupe que le dossier.**

**Exemple pratique :**

```bash
# Créer un dossier partagé
sudo mkdir /projets/alpha
sudo chown :developers /projets/alpha
sudo chmod 2775 /projets/alpha
   │
   └─ 2 = SGID

# Vérification
$ ls -ld /projets/alpha
drwxrwsr-x 2 root developers 4096 Feb  9 16:00 /projets/alpha
       ^
       └─ s = SGID
```

**Résultat :**

```bash
# Alice (groupe developers) crée un fichier
$ touch /projets/alpha/test.txt
$ ls -l /projets/alpha/test.txt
-rw-r--r-- 1 alice developers 0 Feb  9 16:01 test.txt
                    ^^^^^^^^^^
                    Groupe = developers (du dossier, pas d'alice !)
```

**Sans SGID :**

```bash
# Le fichier aurait le groupe primaire d'alice
-rw-r--r-- 1 alice alice 0 Feb  9 16:01 test.txt
```

**Définir le SGID :**

```bash
sudo chmod g+s dossier/
sudo chmod 2775 dossier/      # Octal
```

> 💡 **Usage :** Dossiers partagés entre équipes !

### Sticky Bit (1xxx)

**Sur un dossier : seul le propriétaire d'un fichier peut le supprimer.**

**Exemple typique : `/tmp`**

```bash
$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 Feb  9 16:05 /tmp
         ^
         └─ t = Sticky bit
```

**Pourquoi ?**

`/tmp` est accessible en écriture par **tout le monde**. Sans sticky bit, n'importe qui pourrait supprimer les fichiers des autres !

**Avec le sticky bit :**

```bash
# Alice crée un fichier
alice$ touch /tmp/alice.txt

# Bob ne peut PAS le supprimer
bob$ rm /tmp/alice.txt
rm: cannot remove '/tmp/alice.txt': Operation not permitted

# Seul alice (ou root) peut le supprimer
alice$ rm /tmp/alice.txt
# ✅ OK
```

**Définir le sticky bit :**

```bash
sudo chmod +t dossier/
sudo chmod 1777 dossier/      # Octal
```

**Récapitulatif des permissions spéciales :**

```
┌──────────────────────────────────────────────────────────┐
│  Bit  │ Symbolique │ Octal │ Usage                       │
├──────────────────────────────────────────────────────────┤
│ SUID  │ u+s        │ 4xxx  │ Exécute comme propriétaire  │
│ SGID  │ g+s        │ 2xxx  │ Héritage groupe (dossier)   │
│ Sticky│ +t         │ 1xxx  │ Protection suppression      │
└──────────────────────────────────────────────────────────┘
```

**Combinaisons :**

```bash
chmod 4755 fichier    # SUID + rwxr-xr-x
chmod 2775 dossier    # SGID + rwxrwxr-x
chmod 1777 dossier    # Sticky + rwxrwxrwx
chmod 6755 fichier    # SUID+SGID + rwxr-xr-x
```

---

## 🔓 Sudo et élévation de privilèges

### Pourquoi sudo ?

**Avant sudo :**

```
Besoin d'une commande root ?
→ Se connecter en root (su)
→ Faire la commande
→ Oublier de se déconnecter
→ Continuer à bosser en root
→ Faire une bêtise en root
→ 💥 CATASTROPHE
```

**Avec sudo :**

```
Besoin d'une commande root ?
→ sudo commande
→ Tape ton mot de passe
→ Commande exécutée en root
→ Retour immédiat en user normal
→ ✅ SÉCURISÉ
```

### Configuration de sudo

**Fichier principal : `/etc/sudoers`**

> ⚠️ **NE JAMAIS ÉDITER DIRECTEMENT !** Utilisez `visudo`.

```bash
sudo visudo
```

**Pourquoi visudo ?**

- Vérifie la syntaxe avant de sauvegarder
- Évite de bloquer sudo en cas d'erreur
- Verrouille le fichier pendant l'édition

### Syntaxe du fichier sudoers

```
user    host=(runas)    commands
```

**Exemples :**

```bash
# Donner tous les droits sudo à john
john    ALL=(ALL:ALL) ALL

# Permettre à alice de redémarrer le serveur web sans mot de passe
alice   ALL=(ALL) NOPASSWD: /usr/sbin/service apache2 restart

# Groupe developers peut tout faire
%developers ALL=(ALL:ALL) ALL
```

**Décryptage :**

```
john    ALL=(ALL:ALL) ALL
│       │   │    │    │
│       │   │    │    └─ Peut exécuter TOUTES les commandes
│       │   │    └─ En tant que n'importe quel groupe
│       │   └─ En tant que n'importe quel utilisateur
│       └─ Sur toutes les machines
└─ Utilisateur concerné

%developers ALL=(ALL:ALL) ALL
│
└─ % = Groupe (pas un utilisateur)
```

### Utilisation de sudo

```bash
# Exécuter une commande en root
sudo apt update

# Exécuter en tant qu'un autre user
sudo -u www-data touch /var/www/fichier.txt

# Devenir root temporairement
sudo -i                     # Login shell
sudo -s                     # Shell actuel

# Éditer un fichier protégé
sudo nano /etc/hosts

# Relancer sudo sans retaper le mot de passe (cache 15 min)
sudo apt install vim
sudo apt install git        # Pas de mot de passe redemandé
```

### Logs sudo

**Toutes les commandes sudo sont loguées !**

```bash
# Voir les logs sudo
sudo grep sudo /var/log/auth.log

# Exemple de sortie :
Feb  9 16:30:12 server sudo: john : TTY=pts/0 ; PWD=/home/john ; USER=root ; COMMAND=/usr/bin/apt update
```

> 💡 **Traçabilité :** C'est pour ça qu'on utilise sudo au lieu de se connecter en root !

### Donner sudo à un utilisateur

**Méthode 1 : Ajouter au groupe sudo (Debian/Ubuntu)**

```bash
sudo usermod -aG sudo john
```

**Méthode 2 : Modifier /etc/sudoers**

```bash
sudo visudo

# Ajouter :
john ALL=(ALL:ALL) ALL
```

**Méthode 3 : Créer un fichier dans /etc/sudoers.d/**

```bash
sudo nano /etc/sudoers.d/john

# Contenu :
john ALL=(ALL:ALL) ALL

# Permissions importantes :
sudo chmod 440 /etc/sudoers.d/john
```

> 💡 **Bonne pratique :** Utilisez `/etc/sudoers.d/` pour garder `/etc/sudoers` propre.

### Exemples avancés

**Permettre des commandes spécifiques :**

```bash
# Alice peut redémarrer nginx
alice ALL=(ALL) NOPASSWD: /usr/sbin/service nginx restart, /usr/sbin/service nginx reload

# Bob peut lire les logs
bob ALL=(ALL) NOPASSWD: /usr/bin/tail /var/log/*

# Groupe support peut gérer les services
%support ALL=(ALL) NOPASSWD: /usr/sbin/service * start, /usr/sbin/service * stop, /usr/sbin/service * restart
```

---

## 🏢 TP Pratique : Structure d'entreprise

### Scénario

Vous êtes admin système d'une PME de 30 personnes avec 3 services :
- **Comptabilité** : 10 personnes
- **Développement** : 15 personnes
- **Direction** : 5 personnes

**Objectif :** Créer une structure sécurisée avec utilisateurs, groupes et permissions adaptées.

### Étape 1 : Créer les groupes

```bash
sudo groupadd compta
sudo groupadd dev
sudo groupadd direction
```

### Étape 2 : Créer les utilisateurs

```bash
# Comptabilité
sudo useradd -m -G compta -c "Marie Comptable" mcomptable
sudo useradd -m -G compta -c "Paul Comptable" pcomptable

# Développement
sudo useradd -m -G dev -c "Alice Dev" adev
sudo useradd -m -G dev -c "Bob Dev" bdev
sudo useradd -m -G dev -c "Charlie Dev" cdev

# Direction
sudo useradd -m -G direction -c "Jean Directeur" jdirecteur

# Définir les mots de passe
sudo passwd mcomptable
sudo passwd pcomptable
sudo passwd adev
sudo passwd bdev
sudo passwd cdev
sudo passwd jdirecteur
```

### Étape 3 : Créer la structure de dossiers

```bash
# Créer la racine
sudo mkdir -p /entreprise/{compta,dev,direction,commun}
```

### Étape 4 : Configurer les permissions

```bash
# Dossier compta : accessible uniquement par le groupe compta
sudo chown :compta /entreprise/compta
sudo chmod 2770 /entreprise/compta
#          │││└─ others : aucun droit
#          ││└─ group : rwx (lecture, écriture, traverser)
#          │└─ owner : rwx
#          └─ SGID : nouveaux fichiers → groupe compta

# Dossier dev : accessible uniquement par le groupe dev
sudo chown :dev /entreprise/dev
sudo chmod 2770 /entreprise/dev

# Dossier direction : accessible uniquement par le groupe direction
sudo chown :direction /entreprise/direction
sudo chmod 2770 /entreprise/direction

# Dossier commun : accessible par tout le monde
sudo chmod 1777 /entreprise/commun
#          │││└─ others : rwx
#          ││└─ group : rwx
#          │└─ owner : rwx
#          └─ Sticky bit : chacun supprime que ses fichiers
```

### Étape 5 : Vérification

```bash
$ ls -l /entreprise/
drwxrws---  2 root compta     4096 Feb  9 17:00 compta
drwxrwxrwt  2 root root       4096 Feb  9 17:00 commun
drwxrws---  2 root dev        4096 Feb  9 17:00 dev
drwxrws---  2 root direction  4096 Feb  9 17:00 direction
```

### Étape 6 : Tests

**Test 1 : Alice (dev) crée un fichier dans /entreprise/dev**

```bash
# Se connecter en alice
su - adev

$ cd /entreprise/dev
$ touch projet_alpha.txt
$ ls -l
-rw-r--r-- 1 adev dev 0 Feb  9 17:05 projet_alpha.txt
                    ^^^ Groupe = dev (grâce au SGID)

# Bob (aussi dev) peut le modifier
$ su - bdev
$ echo "Hello" >> /entreprise/dev/projet_alpha.txt
# ✅ OK
```

**Test 2 : Alice ne peut PAS accéder au dossier compta**

```bash
su - adev

$ ls /entreprise/compta
ls: cannot open directory '/entreprise/compta': Permission denied
# ✅ Correct
```

**Test 3 : Le directeur peut tout voir (optionnel)**

```bash
# Ajouter le directeur à tous les groupes
sudo usermod -aG compta,dev jdirecteur

$ su - jdirecteur
$ ls /entreprise/compta
# ✅ OK

$ ls /entreprise/dev
# ✅ OK
```

**Test 4 : Dossier commun avec sticky bit**

```bash
# Alice crée un fichier
su - adev
$ touch /entreprise/commun/alice.txt

# Bob ne peut PAS le supprimer
su - bdev
$ rm /entreprise/commun/alice.txt
rm: cannot remove '/entreprise/commun/alice.txt': Operation not permitted
# ✅ Correct
```

### Résultat final

```
/entreprise/
├── compta/          [drwxrws--- root:compta]
│                     Seul groupe compta peut accéder
│
├── dev/             [drwxrws--- root:dev]
│                     Seul groupe dev peut accéder
│
├── direction/       [drwxrws--- root:direction]
│                     Seul groupe direction peut accéder
│
└── commun/          [drwxrwxrwt root:root]
                      Tout le monde peut créer
                      Mais chacun supprime que ses fichiers
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Diagnostic de permissions

**Objectif :** Comprendre et corriger un problème de permissions.

**Scénario :**

Un développeur vous dit : "Je ne peux pas écrire dans /var/www/html !"

```bash
$ ls -ld /var/www/html
drwxr-xr-x 2 root root 4096 Feb  9 18:00 /var/www/html

$ id jdev
uid=1001(jdev) gid=1001(jdev) groups=1001(jdev),33(www-data)
```

**Question :** Quelle est la solution ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Analyse :**

```
drwxr-xr-x 2 root root 4096 /var/www/html
│││││││││││
│││└─ Group : r-x (peut lire et traverser, mais pas écrire)
││└─ Others : r-x
│└─ Owner (root) : rwx
```

Le dossier appartient à `root:root`. Le groupe n'a que `r-x` (pas de `w`).

**Solutions possibles :**

**Solution 1 : Changer le groupe + permissions**

```bash
# Changer le groupe vers www-data
sudo chown :www-data /var/www/html

# Donner écriture au groupe
sudo chmod g+w /var/www/html

# Vérification
ls -ld /var/www/html
drwxrwxr-x 2 root www-data 4096 /var/www/html

# Maintenant jdev (qui est dans www-data) peut écrire
```

**Solution 2 : Ajouter le SGID (recommandé)**

```bash
sudo chown :www-data /var/www/html
sudo chmod 2775 /var/www/html

# Avantage : tous les nouveaux fichiers seront groupe www-data
```

**Test :**

```bash
su - jdev
$ touch /var/www/html/test.php
$ ls -l /var/www/html/test.php
-rw-r--r-- 1 jdev www-data 0 Feb  9 18:10 test.php
# ✅ Groupe = www-data (grâce au SGID)
```

</details>

---

### Exercice 2 : Script avec SUID (DANGER)

**Objectif :** Comprendre les risques du SUID.

**Scénario :**

Créer un script qui lit /etc/shadow (normalement interdit).

**Consignes :**

1. Créer un script bash simple qui affiche /etc/shadow
2. Le rendre exécutable
3. Tester en tant qu'utilisateur normal (échec)
4. Mettre le SUID
5. Tester à nouveau (succès)
6. **SUPPRIMER IMMÉDIATEMENT** (sécurité)

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer le script
cat > /tmp/read_shadow.sh << 'EOF'
#!/bin/bash
cat /etc/shadow
EOF

# 2. Rendre exécutable
chmod +x /tmp/read_shadow.sh

# 3. Tester en user normal
$ /tmp/read_shadow.sh
cat: /etc/shadow: Permission denied
# ✅ Normal

# 4. Mettre le SUID et chown root
sudo chown root:root /tmp/read_shadow.sh
sudo chmod u+s /tmp/read_shadow.sh

# Vérifier
$ ls -l /tmp/read_shadow.sh
-rwsr-xr-x 1 root root 29 Feb  9 18:15 /tmp/read_shadow.sh
   ^
   └─ SUID actif

# 5. Tester à nouveau
$ /tmp/read_shadow.sh
root:$6$random$hashblablabla:19387:0:99999:7:::
john:$6$random$hashblablabla:19387:0:99999:7:::
# ✅ Ça marche ! (exécuté avec droits root)

# 6. SUPPRIMER IMMÉDIATEMENT
sudo rm /tmp/read_shadow.sh
```

**POURQUOI C'EST DANGEREUX :**

Avec ce script, **n'importe quel utilisateur** peut lire `/etc/shadow` qui contient les mots de passe chiffrés !

> ⚠️ **NE JAMAIS mettre SUID sur des scripts bash !** C'est une faille de sécurité énorme.

</details>

---

### Exercice 3 : Audit de sécurité

**Objectif :** Trouver les fichiers avec SUID/SGID (potentiels risques).

**Consignes :**

1. Trouver tous les fichiers avec SUID sur le système
2. Trouver tous les fichiers avec SGID
3. Identifier ceux qui sont légitimes vs suspects

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Trouver les SUID (4000)
sudo find / -type f -perm -4000 -ls 2>/dev/null

# Sortie typique (légitime) :
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/su
/usr/bin/chsh
/usr/bin/mount
/usr/bin/umount

# 2. Trouver les SGID (2000)
sudo find / -type f -perm -2000 -ls 2>/dev/null

# 3. Vérifier les suspects
# Si vous trouvez :
/home/john/mysteryapp  ← SUSPECT !
/tmp/script.sh         ← TRÈS SUSPECT !

# Analyser
ls -l /chemin/fichier/suspect
file /chemin/fichier/suspect
```

**Fichiers SUID légitimes :**
- `/usr/bin/passwd` (changer son mot de passe)
- `/usr/bin/sudo` (élévation privilèges)
- `/usr/bin/su` (changer d'utilisateur)
- `/usr/bin/mount` (monter des volumes)

**Fichiers SUID SUSPECTS :**
- Dans `/tmp/`
- Dans `/home/`
- Scripts bash
- Fichiers récemment modifiés

**Action si suspect :**

```bash
# Retirer le SUID
sudo chmod u-s /chemin/fichier/suspect

# Ou supprimer
sudo rm /chemin/fichier/suspect
```

</details>

---

### Exercice 4 : Scénario réel - Log rotation

**Objectif :** Configurer les permissions pour qu'un script puisse gérer les logs.

**Scénario :**

Vous avez une application qui écrit dans `/var/log/myapp/app.log`. Vous voulez qu'un utilisateur `logmanager` puisse :
- Lire les logs
- Archiver les logs (copier)
- Mais PAS supprimer les logs

**Consignes :**

1. Créer l'utilisateur `logmanager`
2. Créer le dossier `/var/log/myapp/`
3. Configurer les bonnes permissions
4. Tester

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```bash
# 1. Créer l'utilisateur
sudo useradd -m -s /bin/bash logmanager
sudo passwd logmanager

# 2. Créer le dossier
sudo mkdir -p /var/log/myapp

# 3. Créer un groupe dédié
sudo groupadd logadmin
sudo usermod -aG logadmin logmanager

# 4. Configurer les permissions
sudo chown root:logadmin /var/log/myapp
sudo chmod 750 /var/log/myapp
#             ││└─ others : aucun accès
#             │└─ group : r-x (lire, traverser, pas écrire)
#             └─ owner : rwx (tout)

# 5. Créer un fichier de log test
sudo touch /var/log/myapp/app.log
sudo chown root:logadmin /var/log/myapp/app.log
sudo chmod 640 /var/log/myapp/app.log
#             ││└─ others : rien
#             │└─ group : r-- (lecture seule)
#             └─ owner : rw-

# 6. Test en tant que logmanager
su - logmanager

# Peut lire
$ cat /var/log/myapp/app.log
# ✅ OK

# Peut copier
$ cp /var/log/myapp/app.log /tmp/backup.log
# ✅ OK

# Ne peut PAS supprimer
$ rm /var/log/myapp/app.log
rm: cannot remove '/var/log/myapp/app.log': Permission denied
# ✅ Correct

# Ne peut PAS modifier
$ echo "test" >> /var/log/myapp/app.log
bash: /var/log/myapp/app.log: Permission denied
# ✅ Correct
```

**Résumé des permissions :**

```
/var/log/myapp/
├── Dossier : 750 (root:logadmin)
│   → logadmin peut lister et traverser
│
└── app.log : 640 (root:logadmin)
    → logadmin peut lire uniquement
```

</details>

---

## 📚 Ressources

### Documentation officielle

- [Linux Users and Groups](https://www.kernel.org/doc/html/latest/admin-guide/README.html)
- [File Permissions](https://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html)
- [Sudo Manual](https://www.sudo.ws/man/sudo.man.html)

### Tutoriels

- [Understanding Linux File Permissions](https://www.linux.com/training-tutorials/understanding-linux-file-permissions/)
- [Linux Users and Groups Tutorial](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-ubuntu)

### Outils pratiques

- [Permission Calculator](https://chmod-calculator.com/) - Calculateur de chmod
- [Explain Shell](https://explainshell.com/) - Explique les commandes

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Points importants à retenir :**
-
-
-

**Questions :**
-
-

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Je sais créer et supprimer des utilisateurs
- [ ] Je comprends la différence entre groupe primaire et secondaires
- [ ] Je maîtrise chmod en symbolique ET en octal
- [ ] Je connais les permissions courantes (755, 644, 600, etc.)
- [ ] Je comprends le SUID, SGID et Sticky bit
- [ ] Je sais configurer sudo de façon sécurisée
- [ ] Je peux diagnostiquer un problème de permissions
- [ ] J'ai compris les dangers du chmod 777 et du SUID

---

<div align="center">

**Cours suivant :** [03-processus-services-systemd.md](03-processus-services-systemd.md)

[⬅️ Retour au sommaire](README.md)

</div>
