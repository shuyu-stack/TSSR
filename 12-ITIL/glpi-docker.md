# Installation de Docker et GLPI sur Debian 12

## 📋 Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Prérequis](#prérequis)
- [Étape 1 : Préparation du système](#étape-1--préparation-du-système)
- [Étape 2 : Installation de Docker](#étape-2--installation-de-docker)
- [Étape 3 : Déploiement de GLPI](#étape-3--déploiement-de-glpi)
- [Étape 4 : Configuration de GLPI](#étape-4--configuration-de-glpi)
- [Étape 5 : Commandes utiles](#étape-5--commandes-utiles)
- [Dépannage](#dépannage)
- [Conclusion](#conclusion)

---

## Vue d'ensemble

Ce guide vous accompagne pas à pas pour installer Docker sur Debian 12 (en ligne de commande) et déployer GLPI (Gestionnaire Libre de Parc Informatique) via une image Docker Hub.

### Architecture déployée

```
┌─────────────────────────────────────┐
│     Navigateur Web (Port 80)        │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│       VM Debian 12 (VMware)         │
│  ┌────────────────────────────────┐ │
│  │      Docker Engine             │ │
│  │  ┌──────────┐  ┌─────────────┐│ │
│  │  │  GLPI    │  │  MariaDB    ││ │
│  │  │ :80      │◄─┤  :3306      ││ │
│  │  │          │  │             ││ │
│  │  └──────────┘  └─────────────┘│ │
│  │         glpi-network           │ │
│  └────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### Composants

- **GLPI** : Application web de gestion de parc informatique
- **MariaDB** : Base de données relationnelle
- **Docker Compose** : Orchestrateur de conteneurs
- **Image utilisée** : diouxx/glpi

---

## Prérequis

### Matériel

- VM Debian 12 sur VMware (ou autre hyperviseur)
- 2 Go RAM minimum (4 Go recommandé)
- 20 Go d'espace disque
- Connexion réseau active

### Logiciel

- Debian 12 installé en mode CLI (sans interface graphique)
- Accès root ou utilisateur avec sudo
- SSH activé (optionnel mais recommandé)

---

## Étape 1 : Préparation du système

### 1.1 Connexion et mise à jour

Connectez-vous à votre VM Debian 12 :

```bash
# Se connecter en root
su -

# OU avec votre utilisateur (si sudo configuré)
ssh utilisateur@IP_VM
```

Mettez à jour le système :

```bash
# Mettre à jour la liste des paquets
apt update

# Mettre à jour les paquets installés
apt upgrade -y
```

### 1.2 Installation de sudo (si nécessaire)

Si sudo n'est pas installé :

```bash
# Se connecter en root
su -

# Installer sudo
apt install sudo -y

# Ajouter votre utilisateur au groupe sudo
usermod -aG sudo votre_utilisateur

# Vérifier
groups votre_utilisateur

# Se déconnecter et se reconnecter pour appliquer
exit
```

### 1.3 Installation des outils de base

```bash
sudo apt install -y \
  curl \
  wget \
  gnupg \
  lsb-release \
  ca-certificates \
  apt-transport-https
```

**Explication :**

- `curl / wget` : Téléchargement de fichiers
- `gnupg` : Vérification des signatures GPG
- `ca-certificates` : Certificats SSL
- `apt-transport-https` : Support HTTPS pour APT

---

## Étape 2 : Installation de Docker

### 2.1 Ajout du dépôt officiel Docker

**Créer le répertoire pour les clés GPG**

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

**Télécharger la clé GPG Docker**

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

**Ajouter le dépôt Docker**

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 2.2 Installation de Docker Engine

```bash
# Mettre à jour la liste des paquets
sudo apt update

# Installer Docker et ses composants
sudo apt install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

**Composants installés :**

- `docker-ce` : Docker Engine (moteur principal)
- `docker-ce-cli` : Interface en ligne de commande
- `containerd.io` : Runtime pour gérer les conteneurs
- `docker-buildx-plugin` : Constructeur d'images avancé
- `docker-compose-plugin` : Orchestration multi-conteneurs

### 2.3 Vérification de l'installation

```bash
# Vérifier la version
sudo docker --version

# Tester avec l'image hello-world
sudo docker run hello-world

# Vérifier le statut du service
sudo systemctl status docker
```

**Résultat attendu :**

```
Docker version 25.x.x, build xxxxx

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### 2.4 Utiliser Docker sans sudo (optionnel)

```bash
# Ajouter votre utilisateur au groupe docker
sudo usermod -aG docker $USER

# Appliquer les changements (sans se déconnecter)
newgrp docker

# Tester sans sudo
docker ps
```

**⚠️ Note de sécurité :**
Ajouter un utilisateur au groupe docker lui donne des privilèges équivalents à root. À utiliser avec précaution en production.

---

## Étape 3 : Déploiement de GLPI

### 3.1 Créer la structure du projet

```bash
# Créer un répertoire pour le projet
mkdir -p ~/glpi-docker
cd ~/glpi-docker
```

### 3.2 Télécharger les images (optionnel)

Pour gagner du temps lors de la démo :

```bash
# Télécharger l'image GLPI
docker pull diouxx/glpi:latest

# Télécharger l'image MariaDB
docker pull mariadb:10.11

# Vérifier les images téléchargées
docker images
```

### 3.3 Créer le fichier docker-compose.yml

```bash
nano docker-compose.yml
```

Collez le contenu suivant :

```yaml
version: '3.8'

services:
  # Base de données MariaDB pour GLPI
  mariadb:
    image: mariadb:10.11
    container_name: glpi-mariadb
    hostname: mariadb
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: rootpass123
      MARIADB_DATABASE: glpidb
      MARIADB_USER: glpi_user
      MARIADB_PASSWORD: glpi_pass
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    networks:
      - glpi-network

  # Application GLPI
  glpi:
    image: diouxx/glpi:latest
    container_name: glpi-app
    hostname: glpi
    restart: always
    ports:
      - "80:80"
    environment:
      TIMEZONE: Europe/Paris
    volumes:
      - /var/glpi/:/var/www/html/glpi
    depends_on:
      - mariadb
    networks:
      - glpi-network

# Réseau bridge pour la communication inter-conteneurs
networks:
  glpi-network:
    driver: bridge
```

**Sauvegarder :** Ctrl+O → Entrée → Ctrl+X

### 3.4 Comprendre le fichier docker-compose.yml

**Service MariaDB**

```yaml
mariadb:
  image: mariadb:10.11              # Version stable de MariaDB
  container_name: glpi-mariadb      # Nom du conteneur
  hostname: mariadb                 # Nom réseau (utilisé par GLPI)
  restart: always                   # Redémarre automatiquement
  environment:                      # Variables d'environnement
    MARIADB_ROOT_PASSWORD: rootpass123
    MARIADB_DATABASE: glpidb
    MARIADB_USER: glpi_user
    MARIADB_PASSWORD: glpi_pass
  volumes:                          # Persistance des données
    - /var/lib/mysql:/var/lib/mysql
  networks:                         # Réseau partagé
    - glpi-network
```

**Service GLPI**

```yaml
glpi:
  image: diouxx/glpi:latest         # Image GLPI du Docker Hub
  container_name: glpi-app          # Nom du conteneur
  hostname: glpi                    # Nom réseau
  restart: always                   # Redémarre automatiquement
  ports:
    - "80:80"                       # Port hôte:conteneur
  environment:
    TIMEZONE: Europe/Paris          # Fuseau horaire
  volumes:
    - /var/glpi/:/var/www/html/glpi # Persistance GLPI
  depends_on:
    - mariadb                       # Démarre après MariaDB
  networks:
    - glpi-network
```

**Réseau**

```yaml
networks:
  glpi-network:
    driver: bridge                  # Réseau bridge isolé
```

**Avantages :**

- Isolation réseau entre les conteneurs
- Communication via les hostnames (mariadb, glpi)
- Sécurité accrue

### 3.5 Lancer la stack GLPI

```bash
# Démarrer en mode détaché (background)
docker compose up -d

# Suivre les logs en temps réel
docker compose logs -f
```

Appuyez sur Ctrl+C pour arrêter le suivi des logs (les conteneurs restent actifs).

### 3.6 Vérifier le déploiement

```bash
# Lister les conteneurs actifs
docker ps

# Vérifier les logs de GLPI
docker logs glpi-app

# Vérifier les logs de MariaDB
docker logs glpi-mariadb

# Inspecter le réseau créé
docker network inspect glpi-docker_glpi-network

# Voir l'utilisation des ressources
docker stats --no-stream
```

### 3.7 Obtenir l'IP de la VM

```bash
# Voir toutes les interfaces
ip a

# Ou plus précis
ip -4 addr show | grep inet
```

**Notez l'IP** (exemple : `192.168.1.100`)

---

## Étape 4 : Configuration de GLPI

### 4.1 Accès à l'interface web

Depuis votre navigateur (PC hôte) :

```
http://IP_DE_VOTRE_VM
Exemple : http://192.168.1.100
```

### 4.2 Assistant d'installation

**Étape 1 : Langue**

- Sélectionnez : **Français**

**Étape 2 : Licence**

- Cliquez sur : **Accepter**

**Étape 3 : Type d'installation**

- Sélectionnez : **Installer**

**Étape 4 : Configuration de la base de données**

Remplissez les champs suivants :

| Champ            | Valeur      |
|------------------|-------------|
| Serveur SQL      | mariadb     |
| Utilisateur SQL  | glpi_user   |
| Mot de passe SQL | glpi_pass   |

Cliquez sur **Continuer**

**Étape 5 : Sélection de la base**

- Sélectionnez : **glpidb**
- Cliquez sur **Continuer**

**Étape 6 : Initialisation**

- Laissez GLPI initialiser la base de données
- Cliquez sur **Continuer** jusqu'à la fin

### 4.3 Connexion par défaut

GLPI crée automatiquement 5 comptes de test :

| Utilisateur  | Mot de passe | Profil       |
|--------------|--------------|--------------|
| glpi         | glpi         | Super-Admin  |
| tech         | tech         | Technicien   |
| normal       | normal       | Utilisateur  |
| post-only    | post-only    | Post-only    |
| glpi-system  | -            | Système      |

Connectez-vous avec :

- **Login** : glpi
- **Mot de passe** : glpi

### 4.4 ⚠️ Sécurisation immédiate

**Changer les mots de passe**

1. Cliquez sur **Administration** → **Utilisateurs**
2. Cliquez sur l'utilisateur **glpi**
3. Descendez jusqu'à la section **Mot de passe**
4. Remplissez :
   - **Mot de passe** : [nouveau_mot_de_passe]
   - **Confirmation** : [nouveau_mot_de_passe]
5. Cliquez sur **Actualiser**

Répétez pour les autres comptes actifs.

**Supprimer le dossier d'installation**

```bash
# Entrer dans le conteneur GLPI
docker exec -it glpi-app /bin/sh

# Supprimer le dossier d'installation
rm -rf /var/www/html/glpi/install

# Sortir du conteneur
exit
```

---

## Étape 5 : Commandes utiles

### Gestion des conteneurs

```bash
# Voir les conteneurs actifs
docker ps

# Voir TOUS les conteneurs (même arrêtés)
docker ps -a

# Arrêter la stack
docker compose down

# Démarrer la stack
docker compose up -d

# Redémarrer la stack
docker compose restart

# Reconstruire et redémarrer
docker compose up -d --force-recreate

# Voir les logs en temps réel
docker compose logs -f

# Voir les logs d'un service spécifique
docker logs glpi-app
docker logs glpi-mariadb
```

### Inspection des conteneurs

```bash
# Voir les statistiques en temps réel
docker stats

# Inspecter un conteneur
docker inspect glpi-app

# Voir les processus d'un conteneur
docker top glpi-app

# Voir les fichiers modifiés
docker diff glpi-app
```

### Gestion des images

```bash
# Lister les images locales
docker images

# Supprimer une image
docker rmi diouxx/glpi:latest

# Nettoyer les images inutilisées
docker image prune -a
```

### Gestion des volumes

```bash
# Lister les volumes
docker volume ls

# Inspecter un volume
docker volume inspect glpi-docker_mariadb-data

# Supprimer un volume
docker volume rm glpi-docker_mariadb-data

# Nettoyer les volumes inutilisés
docker volume prune
```

### Gestion des réseaux

```bash
# Lister les réseaux
docker network ls

# Inspecter un réseau
docker network inspect glpi-docker_glpi-network

# Supprimer un réseau
docker network rm glpi-docker_glpi-network

# Nettoyer les réseaux inutilisés
docker network prune
```

### Accéder aux conteneurs

```bash
# Shell interactif dans GLPI
docker exec -it glpi-app /bin/sh

# Shell interactif dans MariaDB
docker exec -it glpi-mariadb /bin/bash

# Exécuter une commande MySQL
docker exec -it glpi-mariadb mysql -u glpi_user -pglpi_pass glpidb
```

### Sauvegardes

**Sauvegarder la base de données**

```bash
# Dump de la base MariaDB
docker exec glpi-mariadb mysqldump -u glpi_user -pglpi_pass glpidb > backup-glpi-$(date +%Y%m%d).sql

# Vérifier la sauvegarde
ls -lh backup-glpi-*.sql
```

**Restaurer la base de données**

```bash
# Restaurer depuis un dump
docker exec -i glpi-mariadb mysql -u glpi_user -pglpi_pass glpidb < backup-glpi-20260216.sql
```

**Sauvegarder les volumes Docker**

```bash
# Arrêter la stack
docker compose down

# Sauvegarder le répertoire des volumes
sudo tar -czf backup-volumes-$(date +%Y%m%d).tar.gz /var/lib/mysql /var/glpi

# Redémarrer
docker compose up -d
```

### Nettoyage complet

```bash
# Arrêter et supprimer conteneurs + réseau
docker compose down

# Supprimer aussi les volumes (⚠️ PERTE DE DONNÉES)
docker compose down -v

# Nettoyer tout le système Docker
docker system prune -a --volumes

# Voir l'espace récupéré
docker system df
```

---

## Dépannage

### Problème : Les conteneurs ne démarrent pas

**Vérifier les logs :**

```bash
docker compose logs
```

**Causes courantes :**

- Port 80 déjà utilisé
- Problème de permissions sur les volumes
- MariaDB n'a pas fini d'initialiser

**Solutions :**

```bash
# Vérifier les ports
sudo netstat -tulpn | grep :80

# Vérifier les permissions
ls -la /var/lib/mysql
ls -la /var/glpi

# Attendre l'initialisation de MariaDB
docker logs glpi-mariadb | grep "ready for connections"
```

### Problème : GLPI ne se connecte pas à MariaDB

**Vérifications :**

```bash
# Vérifier que MariaDB est actif
docker ps | grep mariadb

# Vérifier les logs MariaDB
docker logs glpi-mariadb

# Tester la connexion depuis GLPI
docker exec -it glpi-app ping mariadb

# Vérifier le réseau
docker network inspect glpi-docker_glpi-network
```

**Solution :**

```bash
# Recréer le réseau
docker compose down
docker network rm glpi-docker_glpi-network
docker compose up -d
```

### Problème : Page blanche ou erreur 500

**Vérifier les permissions :**

```bash
# Entrer dans le conteneur
docker exec -it glpi-app /bin/sh

# Vérifier les permissions
ls -la /var/www/html/glpi

# Corriger si nécessaire
chown -R www-data:www-data /var/www/html/glpi
exit
```

### Problème : sudo command not found

**Solution :**

```bash
# Se connecter en root
su -

# Installer sudo
apt install sudo -y

# Ajouter l'utilisateur au groupe sudo
usermod -aG sudo votre_utilisateur

# Se déconnecter et se reconnecter
exit
```

### Problème : Docker compose: command not found

**Vérification :**

```bash
# Vérifier l'installation
docker compose version
```

**Si erreur, installer manuellement :**

```bash
sudo apt update
sudo apt install docker-compose-plugin -y
```

---

## Conclusion

### Ce que vous avez appris

- ✅ Installer Docker sur Debian 12
- ✅ Utiliser Docker Compose pour orchestrer des conteneurs
- ✅ Déployer une application multi-conteneurs (GLPI + MariaDB)
- ✅ Gérer les volumes pour la persistance des données
- ✅ Créer un réseau Docker isolé
- ✅ Sauvegarder et restaurer des données

### Avantages de Docker pour GLPI

| Avantage         | Description                             |
|------------------|-----------------------------------------|
| Rapidité         | Déploiement en 2 minutes vs 30-60 min   |
| Isolation        | Pas de conflit avec d'autres services   |
| Portabilité      | Fonctionne sur Debian, Ubuntu, Windows, macOS |
| Reproductibilité | Un seul fichier docker-compose.yml      |
| Mise à jour facile | Changer la version de l'image         |

### Pour aller plus loin

- Ajouter un reverse proxy (Nginx, Traefik)
- Configurer HTTPS avec Let's Encrypt
- Mettre en place des sauvegardes automatiques
- Monitorer avec Prometheus + Grafana
- Configurer la haute disponibilité
- Déployer sur Kubernetes

### Ressources

- **Documentation Docker** : https://docs.docker.com
- **Documentation GLPI** : https://glpi-project.org/documentation
- **Image Docker GLPI** : https://hub.docker.com/r/diouxx/glpi
- **Docker Compose** : https://docs.docker.com/compose

---

**Auteur**
Guide créé pour une formation TSSR - Février 2026
