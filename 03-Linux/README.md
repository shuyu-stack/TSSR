# 🐧 Module 03 - Linux Administration

> 📚 **Module :** Linux - De technicien N1 à Admin Junior
> 🎯 **Niveau :** Débutant → Intermédiaire
> ⏱️ **Durée totale :** 40 heures (2 semaines intensives)
> 👨‍🏫 **Formateur :** Admin système Linux depuis 15 ans (Debian, Ubuntu, RHEL, CentOS)

---

## 💬 Message de ton formateur

**La vérité sur Linux en formation TSSR :**

La plupart des formations TSSR survolent Linux en 2-3 jours. Résultat ? Tu sais faire `ls`, `cd`, et c'est tout. En entreprise, tu vas galérer.

**Mon constat après 15 ans :**
- 70% des serveurs web tournent sous Linux
- 90% des infrastructures cloud = Linux
- Les meilleures offres d'emploi demandent Linux
- **MAIS** : Peu de TSSR maîtrisent vraiment Linux

**C'est TA chance de te démarquer !**

Dans ce module, je vais te former comme j'aurais aimé qu'on me forme. Pas de théorie sèche, que du **concret**, du **terrain**, des **commandes que tu utiliseras TOUS LES JOURS**.

---

## 🎯 Plan de formation

### 🔰 NIVEAU 1 : Technicien Support N1/N2 (Essentiels)

#### Module 1 : Commandes de base - La survie Linux (4h)
**[01-commandes-essentielles-survie.md](01-commandes-essentielles-survie.md)**
- Navigation système de fichiers (cd, ls, pwd, find)
- Manipulation fichiers/dossiers (cp, mv, rm, mkdir, touch)
- Lecture de fichiers (cat, less, more, head, tail)
- Recherche et filtrage (grep, find, locate)
- Éditeurs de texte (nano, vim basics)
- **TP** : Dépanner un serveur web inaccessible

#### Module 2 : Utilisateurs et permissions (4h)
**[02-utilisateurs-permissions-groupes.md](02-utilisateurs-permissions-groupes.md)**
- Création/suppression utilisateurs (useradd, userdel, usermod)
- Gestion des groupes
- Permissions Unix (chmod, chown, chgrp)
- SUID, SGID, Sticky bit
- sudo et sudoers
- **TP** : Créer une structure d'équipe avec permissions

#### Module 3 : Processus et services (4h)
**[03-processus-services-systemd.md](03-processus-services-systemd.md)**
- Gestion des processus (ps, top, htop, kill)
- systemd (systemctl, journalctl)
- Démarrage/arrêt services
- Création de services personnalisés
- Logs système
- **TP** : Créer un service qui démarre au boot

#### Module 4 : SSH et connexion à distance (3h)
**[04-ssh-connexion-distance.md](04-ssh-connexion-distance.md)**
- Configuration SSH serveur/client
- Authentification par clés (SSH keys)
- SCP et RSYNC (transfert fichiers)
- Tunnels SSH
- Sécurisation SSH
- **TP** : Connexion sécurisée sans mot de passe

#### Module 5 : Logs et dépannage (4h)
**[05-logs-depannage-troubleshooting.md](05-logs-depannage-troubleshooting.md)**
- Où sont les logs ? (/var/log/)
- journalctl (logs systemd)
- Analyse de logs (grep, awk, sed)
- Méthodologie de dépannage Linux
- Outils de diagnostic (df, du, free, netstat, ss)
- **TP** : Diagnostiquer 5 pannes courantes

---

### 🚀 NIVEAU 2 : Admin Junior (Se démarquer)

#### Module 6 : Scripting Bash - Automatisation (6h)
**[06-scripting-bash-automatisation.md](06-scripting-bash-automatisation.md)**
- Variables, conditions, boucles
- Fonctions et arguments
- Scripts de maintenance
- Automatisation tâches répétitives
- Cron et crontab
- **TP** : Script de sauvegarde automatique

#### Module 7 : Serveurs Web - Apache & Nginx (5h)
**[07-serveurs-web-apache-nginx.md](07-serveurs-web-apache-nginx.md)**
- Installation Apache/Nginx
- Virtual Hosts (héberger plusieurs sites)
- SSL/TLS (HTTPS avec Let's Encrypt)
- Reverse Proxy
- Performance et optimisation
- **TP** : Héberger 3 sites web sécurisés

#### Module 8 : Réseau Linux avancé (4h)
**[08-reseau-linux-avance.md](08-reseau-linux-avance.md)**
- Configuration IP (netplan, NetworkManager)
- Routage statique et dynamique
- DNS (configuration clients/serveur)
- DHCP serveur
- Firewall (iptables, ufw, firewalld)
- **TP** : Configurer un routeur Linux

#### Module 9 : Sécurité Linux (4h)
**[09-securite-linux-hardening.md](09-securite-linux-hardening.md)**
- Hardening système (désactiver services inutiles)
- Fail2ban (protection SSH)
- SELinux / AppArmor
- Détection intrusions
- Audit de sécurité
- **TP** : Sécuriser un serveur accessible Internet

#### Module 10 : Sauvegardes et restauration (3h)
**[10-sauvegardes-restauration.md](10-sauvegardes-restauration.md)**
- Stratégies de sauvegarde (3-2-1)
- tar, gzip, bzip2
- rsync incrémental
- Timeshift, Déduplication
- Plan de reprise d'activité
- **TP** : Restaurer un système corrompu

---

## 💼 Pourquoi ce plan va te démarquer

### ✅ Ce que 90% des TSSR savent faire
```bash
ls
cd /home
mkdir dossier
nano fichier.txt
```

### 🚀 Ce que TU vas savoir faire (et pas eux)

```bash
# Automatiser une sauvegarde quotidienne
0 2 * * * /usr/local/bin/backup.sh

# Analyser les logs d'attaques SSH
journalctl -u ssh | grep "Failed password" | awk '{print $11}' | sort | uniq -c | sort -nr

# Créer un reverse proxy pour 10 sites
# Configurer fail2ban pour bloquer les attaques
# Scripter l'installation complète d'un serveur

# Diagnostiquer un serveur lent en 2 minutes
top, htop, iotop, netstat -tulpn, df -h, free -h
```

**En entretien, ça change TOUT.**

---

## 🎯 Les compétences qui font la différence

### Technicien N1/N2 classique
- Sait redémarrer un service
- Lit un fichier de log
- Crée un utilisateur

### TOI après cette formation
- ✅ Automatise avec des scripts Bash
- ✅ Configure un serveur web de A à Z
- ✅ Sécurise un système (fail2ban, iptables)
- ✅ Diagnostique une panne en 5 minutes
- ✅ Explique ce qui se passe sous le capot

**Résultat : Tu passes de 28k€ à 35k€ de salaire.**

---

## 🛠️ Environnement de lab requis

### VMs à créer (VMware/VirtualBox)

```
┌─────────────────────────────────────────────────────┐
│  VM1 : Debian 13 (sans interface graphique)        │
│  ────────────────────────────────────────────────   │
│  • 2 GB RAM, 20 GB disque                           │
│  • SSH activé                                       │
│  • Rôle : Serveur de production                     │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  VM2 : Ubuntu 24.04 Server (sans GUI)              │
│  ────────────────────────────────────────────────   │
│  • 2 GB RAM, 20 GB disque                           │
│  • SSH activé                                       │
│  • Rôle : Serveur web (Apache/Nginx)                │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  VM3 : Debian 13 (avec interface XFCE) - OPTIONNEL │
│  ────────────────────────────────────────────────   │
│  • 2 GB RAM, 25 GB disque                           │
│  • Interface graphique pour débuter                 │
│  • Rôle : Poste de travail / tests                  │
└─────────────────────────────────────────────────────┘
```

### Outils indispensables

**Sur ton PC Windows :**
- ✅ **MobaXterm** ou **PuTTY** (SSH)
- ✅ **WinSCP** ou **FileZilla** (transfert fichiers)
- ✅ **Notepad++** avec coloration syntaxe Bash

**Sur tes VMs Linux :**
- ✅ **htop** (monitoring)
- ✅ **vim** ou **nano** (éditeurs)
- ✅ **curl**, **wget** (téléchargement)
- ✅ **net-tools**, **iproute2** (réseau)

---

## 📋 Prérequis

Avant de commencer, tu dois :

- [ ] Savoir ce qu'est une VM
- [ ] Avoir installé Debian 13 et Ubuntu 24 (✅ tu l'as fait)
- [ ] Savoir te connecter en SSH (on va approfondir)
- [ ] Être à l'aise avec Windows (pour la comparaison)
- [ ] **Vouloir vraiment progresser** (le plus important)

**Aucune connaissance Linux avancée requise !** On part de zéro.

---

## 🎓 Méthode d'apprentissage

### Ma méthode éprouvée (15 ans d'expérience)

```
1️⃣ LIRE le cours (30 min)
   → Prendre des notes MANUSCRITES (mémorisation x3)

2️⃣ REPRODUIRE les commandes (1h)
   → Taper TOUTES les commandes dans ta VM
   → Ne PAS copier-coller, TAPER à la main

3️⃣ CASSER ton système (30 min)
   → Supprimer un fichier important
   → Mal configurer un service
   → Voir ce qui se passe

4️⃣ RÉPARER (1h)
   → Utiliser les commandes apprises
   → Consulter les logs
   → Restaurer

5️⃣ CRÉER ton propre TP (1h)
   → Inventer un scénario
   → L'appliquer
   → Documenter

Total : 4h par module
```

**Répète ça sur 10 modules = Tu deviens opérationnel.**

---

## 💡 Mes conseils de pro

### ✅ À FAIRE

1. **Pratique TOUS LES JOURS** (même 30 min)
   - Le Linux, c'est comme le vélo : faut pratiquer

2. **Utilise Linux en VRAI**
   - Monte un serveur web perso
   - Héberge un truc (blog, portfolio)
   - Automatise des tâches

3. **CASSE des trucs**
   - Supprime /etc/passwd (sur une VM !)
   - Remplis le disque à 100%
   - Tue des processus critiques
   - Apprends à réparer

4. **DOCUMENTE tout**
   - Note tes commandes
   - Crée tes propres cheat sheets
   - Tiens un journal de bord

5. **Demande de l'AIDE**
   - Forums : r/linuxquestions, Unix StackExchange
   - IRC : #debian, #ubuntu sur Libera.Chat
   - Communautés : LinuxFR, developpez.com

### ❌ À ÉVITER

1. **Copier-coller sans comprendre**
   - Tu dois COMPRENDRE chaque commande

2. **Apprendre par cœur**
   - Comprends la LOGIQUE, pas la syntaxe

3. **Avoir peur de casser**
   - C'est une VM, elle se recrée en 10 min

4. **Comparer à Windows**
   - Linux a sa propre philosophie, accepte-la

5. **Abandonner trop vite**
   - Les 2 premières semaines sont dures
   - Après, ça devient naturel

---

## 📚 Ressources complémentaires

### 📖 Documentation officielle
- [Debian Administrator's Handbook](https://debian-handbook.info/)
- [Ubuntu Server Guide](https://ubuntu.com/server/docs)
- [TLDP - Linux Documentation Project](https://tldp.org/)

### 🎥 Vidéos recommandées
- [Learn Linux TV](https://www.youtube.com/@LearnLinuxTV)
- [NetworkChuck](https://www.youtube.com/@NetworkChuck) (Linux en anglais)
- [Cocadmin](https://www.youtube.com/@cocadmin) (Français)

### 📝 Cheat Sheets
- [Linux Command Line Cheat Sheet](https://cheatography.com/davechild/cheat-sheets/linux-command-line/)
- [Vim Cheat Sheet](https://vim.rtorr.com/)

### 💬 Communautés
- Reddit : r/linux, r/linuxadmin, r/debian
- Discord : Serveur "Linux Fr"
- Forum : LinuxFR.org

---

## 🏆 Projet final

À la fin de ce module, tu réaliseras un **projet complet** :

### 🎯 Énoncé : Déploiement Infrastructure Web Sécurisée

**Contexte :**
Une PME te demande de déployer un serveur web Linux pour héberger :
- Son site vitrine (WordPress)
- Son application interne (Node.js)
- Un reverse proxy (Nginx)

**Contraintes :**
- Budget minimal (VPS OVH à 5€/mois)
- Sécurité maximale (HTTPS, fail2ban, firewall)
- Automatisation (scripts de sauvegarde, monitoring)
- Haute disponibilité (monitoring, alertes)

**Livrables :**
- ✅ Serveur configuré de A à Z
- ✅ Documentation complète (installation, maintenance)
- ✅ Scripts Bash d'automatisation
- ✅ Plan de reprise d'activité (PRA)
- ✅ Présentation orale (15 min)

**Ce projet = Ta carte de visite pour les entretiens.**

---

## 📊 Progression recommandée

### Planning 2 semaines intensives

```
Semaine 1 : BASES (Technicien N1/N2)
─────────────────────────────────────
Lundi    : Module 1 - Commandes essentielles
Mardi    : Module 2 - Utilisateurs et permissions
Mercredi : Module 3 - Processus et services
Jeudi    : Module 4 - SSH
Vendredi : Module 5 - Logs et dépannage

Semaine 2 : AVANCÉ (Admin Junior)
──────────────────────────────────
Lundi    : Module 6 - Scripting Bash
Mardi    : Module 7 - Apache/Nginx
Mercredi : Module 8 - Réseau avancé
Jeudi    : Module 9 - Sécurité
Vendredi : Module 10 - Sauvegardes + PROJET FINAL
```

**40h de formation = Tu passes de débutant à opérationnel.**

---

## ✅ Checklist avant de commencer

- [ ] J'ai installé Debian 13 (sans GUI) ✅
- [ ] J'ai installé Ubuntu 24.04 Server ✅
- [ ] Je sais me connecter en SSH
- [ ] J'ai MobaXterm ou PuTTY installé
- [ ] J'ai 2 semaines devant moi pour bosser sérieusement
- [ ] Je suis MOTIVÉ à devenir bon en Linux 🔥

---

## 🎯 Ton objectif final

**Avant cette formation :**
```
"Linux ? Euh... je sais faire ls et cd..."
```

**Après cette formation :**
```
"Linux ? Je gère. Debian, Ubuntu, RHEL, pas de souci.
Je configure un serveur web sécurisé en 30 minutes,
j'automatise avec Bash, je diagnostique une panne
réseau en 5 minutes. Tu veux que je te montre ?"
```

**C'est ÇA la différence entre un TSSR lambda et TOI.**

---

<div align="center">

**🚀 Prêt à devenir un pro Linux ?**

**Commence par :** [01-commandes-essentielles-survie.md](01-commandes-essentielles-survie.md)

[⬅️ Retour au sommaire principal](../README.md)

</div>
