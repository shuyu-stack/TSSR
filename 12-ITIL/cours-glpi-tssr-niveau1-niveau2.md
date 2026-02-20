# 🖥️ GLPI — Cours Complet Niveau 1 & Niveau 2
### Formation TSSR — Bac+2 Systèmes et Réseaux

---

> **Formateur** : Architecte Réseau & Directeur DSI  
> **Public** : Techniciens Supérieurs Systèmes et Réseaux (TSSR)  
> **Prérequis** : Notions de base en informatique, administration Windows/Linux  
> **Durée estimée** : 2 jours (Niveau 1 : jour 1 / Niveau 2 : jour 2)  
> **Version GLPI** : 10.x

---

## 📚 Sommaire

1. [Introduction — C'est quoi GLPI ?](#1-introduction--cest-quoi-glpi-)
2. [Architecture GLPI](#2-architecture-glpi)
3. [Les profils utilisateurs](#3-les-profils-utilisateurs)
4. [NIVEAU 1 — L'utilisateur de base](#4-niveau-1--lutilisateur-de-base)
   - 4.1 [Se connecter à GLPI](#41-se-connecter-à-glpi)
   - 4.2 [Créer un ticket d'incident](#42-créer-un-ticket-dincident)
   - 4.3 [Suivre son ticket](#43-suivre-son-ticket)
   - 4.4 [Utiliser la base de connaissances](#44-utiliser-la-base-de-connaissances)
5. [NIVEAU 2 — Le technicien support](#5-niveau-2--le-technicien-support)
   - 5.1 [Gérer les tickets](#51-gérer-les-tickets)
   - 5.2 [L'inventaire du parc](#52-linventaire-du-parc)
   - 5.3 [Gestion des utilisateurs](#53-gestion-des-utilisateurs)
   - 5.4 [Les SLA — Contrats de service](#54-les-sla--contrats-de-service)
   - 5.5 [Rapports et statistiques](#55-rapports-et-statistiques)
   - 5.6 [Administration avancée](#56-administration-avancée)
6. [Cas pratiques](#6-cas-pratiques)
7. [Astuces et bonnes pratiques](#7-astuces-et-bonnes-pratiques)
8. [Lexique](#8-lexique)

---

## 1. Introduction — C'est quoi GLPI ?

### En une phrase simple

> **GLPI, c'est le carnet de bord de toute une DSI.**  
> Il permet de savoir **qui a quoi**, **qui demande quoi**, et **qui a fait quoi**.

### La vraie définition

GLPI signifie **Gestionnaire Libre de Parc Informatique**. C'est un logiciel **open source** (gratuit) développé par la société Teclib' et utilisé par des milliers d'entreprises en France et dans le monde.

Il remplit **deux grandes fonctions** :

| Fonction | Description | Analogie simple |
|----------|-------------|-----------------|
| **ITSM** | Gestion des incidents et demandes | Un centre d'appel organisé |
| **ITAM** | Inventaire du parc informatique | Un registre de tout le matériel |

### Pourquoi c'est indispensable en entreprise ?

Imaginez une DSI sans GLPI :

- Un utilisateur appelle pour signaler que son PC est en panne
- Le technicien intervient mais ne note rien
- 3 semaines plus tard, le PC tombe encore en panne
- Personne ne sait ce qui avait été fait la dernière fois
- Le PC a peut-être une garantie mais on ne sait pas
- Le responsable DSI ne peut pas mesurer la charge de travail de son équipe

**Avec GLPI**, tout est tracé, mesurable et organisé.

### Les chiffres clés

- Utilisé dans **plus de 160 pays**
- Plus de **300 000 installations** dans le monde
- Gratuit et open source (licence GPLv2)
- Disponible en **français** et dans 40+ langues
- Compatible avec des outils comme **FusionInventory** pour l'inventaire automatique

---

## 2. Architecture GLPI

### Comment ça fonctionne techniquement ?

```
┌─────────────────────────────────────────────────────┐
│                    NAVIGATEUR WEB                    │
│          (Chrome, Firefox, Edge...)                  │
└─────────────────────┬───────────────────────────────┘
                      │  HTTP / HTTPS
┌─────────────────────▼───────────────────────────────┐
│                 SERVEUR GLPI                         │
│         PHP + Apache/Nginx + GLPI                    │
│                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  Interface  │  │   Modules   │  │    API      │ │
│  │    Web      │  │  (plugins)  │  │   REST      │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
└─────────────────────┬───────────────────────────────┘
                      │  SQL
┌─────────────────────▼───────────────────────────────┐
│               BASE DE DONNÉES                        │
│              MySQL / MariaDB                         │
│                                                     │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌──────────┐ │
│  │ Tickets │ │ Matériel│ │Utilisat.│ │ Journaux │ │
│  └─────────┘ └─────────┘ └─────────┘ └──────────┘ │
└─────────────────────────────────────────────────────┘
```

### Les modules principaux

```
GLPI
├── 🎫  Assistance          → Tickets, incidents, demandes
├── 💻  Parc                → Ordinateurs, serveurs, imprimantes
├── 💼  Gestion             → Contrats, fournisseurs, budgets
├── 🛠️  Outils              → Base de connaissances, notes, projets
├── 👥  Administration      → Utilisateurs, entités, profils
└── ⚙️  Configuration       → Paramètres, emails, notifications
```

---

## 3. Les profils utilisateurs

> **💡 Astuce formateur :** Commencez toujours par expliquer les profils.  
> Les apprenants comprennent mieux GLPI quand ils savent "depuis quel angle ils regardent".

GLPI fonctionne avec un système de **profils** (rôles). Chaque profil donne accès à certaines fonctionnalités.

### Les 4 profils de base

| Profil | Login par défaut | Qui c'est ? | Ce qu'il peut faire |
|--------|------------------|-------------|---------------------|
| **Super-Admin** | `glpi` / `glpi` | Le responsable DSI, l'admin | Tout, absolument tout |
| **Technicien** | `tech` / `tech` | Le technicien support | Gérer tickets, inventaire, utilisateurs |
| **Normal** | `normal` / `normal` | L'utilisateur lambda | Créer des tickets, consulter ses équipements |
| **Post-only** | `post-only` / `post-only` | L'utilisateur restreint | Créer des tickets uniquement |

> **⚠️ Important en production :**  
> Ces comptes par défaut doivent être **supprimés ou modifiés** après installation !  
> En entreprise, les utilisateurs sont créés individuellement ou importés depuis l'**Active Directory**.

### Analogie simple

Pensez à une grande entreprise :
- **Super-Admin** = Le Directeur DSI (accès à tout)
- **Technicien** = Le technicien support N1/N2 (son périmètre de travail)
- **Normal** = L'employé du bureau comptabilité (il crée des tickets quand son PC plante)
- **Post-only** = Le stagiaire (il peut juste signaler un problème)

---

## 4. NIVEAU 1 — L'utilisateur de base

> **Objectif :** À la fin de ce niveau, l'apprenant sait se connecter, créer un ticket, le suivre, et utiliser la base de connaissances.

---

### 4.1 Se connecter à GLPI

#### Étapes de connexion

1. Ouvrir un navigateur web
2. Aller sur l'URL de GLPI (exemple : `http://glpi.monentreprise.fr` ou `http://192.168.1.100`)
3. Renseigner ses identifiants
4. Choisir la source de connexion :
   - **Base interne GLPI** : compte créé directement dans GLPI
   - **Annuaire LDAP** : compte Active Directory de l'entreprise
5. Cliquer sur **Se connecter**

#### Ce que vous voyez après connexion

```
┌────────────────────────────────────────────────────────────┐
│  GLPI                              👤 Jean DUPONT (Normal) │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  🏠 Accueil                                                │
│                                                            │
│  ┌────────────────────┐  ┌────────────────────────────┐   │
│  │  Mes tickets       │  │  Actualités                │   │
│  │  Ouverts : 2       │  │  ...                       │   │
│  │  En cours : 1      │  │                            │   │
│  └────────────────────┘  └────────────────────────────┘   │
│                                                            │
│  📝 Créer un ticket                                        │
└────────────────────────────────────────────────────────────┘
```

> **💡 Astuce :** Si vous ne voyez pas le menu "Assistance", c'est que votre profil ne vous donne pas accès à cette section. Contactez votre administrateur GLPI.

---

### 4.2 Créer un ticket d'incident

> Un ticket, c'est simplement une **fiche de demande d'aide** que vous envoyez à l'équipe informatique.

#### Les deux types de tickets

| Type | C'est quoi ? | Exemples |
|------|--------------|----------|
| **Incident** | Quelque chose ne fonctionne plus | Mon PC ne démarre plus, Internet ne marche pas |
| **Demande** | Une nouvelle demande de service | Je voudrais un nouvel écran, créer un compte |

#### Créer un ticket pas à pas

**Navigation :**
```
Menu gauche → Assistance → Créer un ticket
```

**Les champs obligatoires :**

| Champ | Description | Exemple |
|-------|-------------|---------|
| **Type** | Incident ou Demande | Incident |
| **Titre** | Résumé court du problème | "PC bureau ne démarre plus" |
| **Description** | Détails complets | "Depuis ce matin, mon PC affiche un écran bleu au démarrage..." |
| **Urgence** | À quel point c'est urgent | Haute |
| **Impact** | Combien de personnes sont affectées | Moyen |

**Les champs optionnels mais utiles :**

| Champ | Description |
|-------|-------------|
| **Catégorie** | Classer le problème (matériel, logiciel, réseau...) |
| **Fichiers joints** | Photo de l'écran d'erreur, capture d'écran |
| **Observateurs** | Autres personnes à tenir informées |

#### Calculer la priorité automatiquement

GLPI calcule la priorité automatiquement selon :

```
Priorité = Urgence × Impact

Urgence :  1 (Très basse)  →  5 (Très haute)
Impact  :  1 (Très bas)    →  5 (Très haut)

Exemple :
- Urgence = Haute (4) + Impact = Très haut (5) = Priorité TRÈS HAUTE
- Urgence = Basse (2) + Impact = Bas (2)        = Priorité BASSE
```

#### Les niveaux d'urgence — En pratique

| Urgence | Exemple concret |
|---------|-----------------|
| **Très basse** | La souris fait un peu de bruit |
| **Basse** | L'imprimante est lente |
| **Moyenne** | Outlook ne se synchronise plus |
| **Haute** | Mon PC ne démarre plus |
| **Très haute** | Le serveur de production est tombé |

> **⚠️ Erreur fréquente des utilisateurs :**  
> Tout mettre en priorité "Très haute". Cela noie les vrais incidents critiques dans la masse.  
> En tant que technicien, apprenez à **re-qualifier** les priorités.

#### Exemple complet de bon ticket

```
Type        : Incident
Titre       : Écran bleu au démarrage - PC-COMPTA-05
Catégorie   : Matériel > Ordinateur

Description :
Bonjour,

Depuis ce matin (lundi 17/02/2026 vers 8h30), mon ordinateur 
affiche un écran bleu au démarrage avec le message suivant :
"CRITICAL_PROCESS_DIED"

J'ai essayé d'éteindre et rallumer 3 fois, sans résultat.
Je travaille sur la comptabilité de fin de mois, c'est urgent.

PC concerné : PC-COMPTA-05
Bureau : Bâtiment B, 2ème étage, bureau 214
Numéro de téléphone : 01 23 45 67 89

Pièce jointe : photo_ecran_bleu.jpg

Urgence : Haute
Impact   : Moyen (seulement moi)
```

> **💡 La règle d'or du bon ticket :**  
> Un technicien qui lit votre ticket doit pouvoir intervenir **sans avoir besoin de vous rappeler**.  
> Qui ? Quoi ? Quand ? Où ? Comment ?

---

### 4.3 Suivre son ticket

#### Où trouver ses tickets ?

```
Menu gauche → Assistance → Tickets
```

Vous voyez tous vos tickets avec leur statut.

#### Les statuts d'un ticket

```
Nouveau → En cours (Assigné) → En cours (Planifié) → En attente → Résolu → Clos

🟡 Nouveau        : Votre ticket vient d'être créé, personne ne l'a encore pris
🔵 En cours       : Un technicien s'en occupe
🟠 En attente     : On attend quelque chose (livraison matériel, réponse fournisseur...)
🟢 Résolu         : Le technicien a traité le problème
⚫ Clos           : Le ticket est terminé et archivé
```

#### Ajouter un commentaire à son ticket

Si vous avez des informations supplémentaires ou si le problème évolue :

1. Cliquer sur votre ticket
2. Descendre jusqu'à la section **"Ajouter une tâche"** ou **"Suivi"**
3. Écrire votre commentaire
4. Cliquer sur **Envoyer**

> **💡 Astuce :** Le technicien reçoit une notification par email à chaque fois que vous ajoutez un suivi. Inutile de le rappeler si vous avez commenté le ticket.

#### Valider ou rejeter une résolution

Quand votre ticket passe en statut "Résolu", vous pouvez :
- **Valider** : Le problème est bien résolu → le ticket se ferme
- **Rejeter** : Le problème n'est pas résolu → le ticket repasse "En cours"

---

### 4.4 Utiliser la base de connaissances

> La base de connaissances, c'est le **FAQ interne** de votre DSI.  
> Avant de créer un ticket, vérifiez si la solution existe déjà !

#### Accéder à la base de connaissances

```
Menu gauche → Outils → Base de connaissances
```

#### Rechercher un article

1. Cliquer sur le champ de recherche
2. Taper des mots-clés (exemple : "reset mot de passe", "VPN connexion", "imprimante")
3. Consulter les articles proposés

#### Exemple d'article type

```
Titre : Comment réinitialiser son mot de passe Windows

Catégorie : Sécurité > Authentification

Solution :
1. Aller sur https://password.monentreprise.fr
2. Cliquer sur "Mot de passe oublié"
3. Entrer votre adresse email professionnelle
4. Suivre les instructions reçues par email

Si cela ne fonctionne pas, créez un ticket de type "Demande" 
avec pour titre "Réinitialisation mot de passe urgente".

Temps de résolution moyen : 5 minutes
```

---

## 5. NIVEAU 2 — Le technicien support

> **Objectif :** À la fin de ce niveau, l'apprenant sait gérer des tickets, maintenir un inventaire, administrer des utilisateurs et produire des rapports.

---

### 5.1 Gérer les tickets

#### Vue d'ensemble du technicien

Quand vous vous connectez avec le profil **Technicien**, votre tableau de bord ressemble à :

```
┌─────────────────────────────────────────────────────────┐
│  Mon équipe  │  Tickets ouverts : 12  │  En retard : 3  │
├─────────────────────────────────────────────────────────┤
│  🔴 Très Haute   │ 2  │ Server prod en panne             │
│  🟠 Haute        │ 5  │ Problèmes divers                 │
│  🟡 Moyenne      │ 4  │ Demandes logiciels               │
│  🟢 Basse        │ 1  │ Réclamation écran                │
└─────────────────────────────────────────────────────────┘
```

#### Traiter un ticket — Étapes complètes

**Étape 1 : Prendre en charge le ticket**

```
Ticket → Acteurs → Assigné à → Choisir son nom
```

Cela indique à tout le monde que **vous** gérez ce ticket.  
L'utilisateur reçoit une notification : "Votre ticket a été pris en charge par [votre nom]."

**Étape 2 : Qualifier le ticket**

Vérifiez et corrigez si besoin :
- Le type (Incident ou Demande) est-il correct ?
- La catégorie est-elle bonne ?
- L'urgence et l'impact sont-ils corrects ?
- L'équipement concerné est-il lié au ticket ?

**Étape 3 : Traiter et documenter**

Après chaque action, ajoutez un **suivi** :

```
Onglet "Suivi" → Ajouter un suivi

Type     : Suivi public (visible par l'utilisateur)
          OU Suivi privé (visible uniquement par les techniciens)
Contenu  : Ce que vous avez fait
```

**Exemple de bons suivis :**
```
Suivi 1 (public) :
"Bonjour, j'ai bien reçu votre demande. 
Je vais intervenir sur votre poste cet après-midi vers 14h00."

Suivi 2 (privé) :
"Diagnostic effectué : disque dur défaillant. 
SMART indique 47 secteurs défectueux. 
Commande SSD Samsung 500Go en cours."

Suivi 3 (public) :
"Le disque dur a été remplacé par un SSD. 
Vos données ont été récupérées et restaurées. 
Windows a été réinstallé. Pouvez-vous vérifier que tout fonctionne ?"
```

**Étape 4 : Résoudre le ticket**

```
Ticket → Statut → Résolu
         Champ "Solution" → Décrire ce qui a été fait
```

> **💡 La règle d'or du technicien :**  
> Un bon ticket résolu, c'est un ticket où **n'importe quel autre technicien** peut comprendre ce qui a été fait, en lisant la solution.

**Étape 5 : Associer à la base de connaissances**

Si la solution est réutilisable :

```
Ticket → Solution → Convertir en article de la base de connaissances
```

#### Le processus de traitement en schéma

```
Nouveau ticket reçu
        │
        ▼
Ticket correctement qualifié ?
   Oui ──────────────────────┐
   Non → Requalifier         │
                             ▼
                    Assigner à un technicien
                             │
                             ▼
                    Traiter le problème
                             │
                    Documenter les actions (suivis)
                             │
                             ▼
                    Problème résolu ?
                   Oui ──────────────────┐
                   Non → Escalader N2/N3 │
                                         ▼
                                Renseigner la solution
                                         │
                                         ▼
                                Passer en "Résolu"
                                         │
                                Utilisateur valide ?
                                Oui → Ticket CLOS ✅
                                Non → Retour en cours
```

#### Gérer les escalades

Quand un problème dépasse vos compétences :

```
Ticket → Acteurs → Groupe assigné → Changer vers N2 ou N3
```

Ajoutez un **suivi privé** expliquant pourquoi vous escaladez.

**Exemples de critères d'escalade :**

| Situation | Action |
|-----------|--------|
| Problème réseau complexe | Escalade vers l'équipe réseau |
| Serveur en panne | Escalade vers l'équipe infrastructure |
| Problème de sécurité | Escalade vers le RSSI |
| SLA dépassé | Escalade automatique ou manuelle au responsable |

#### Les tickets liés

Quand plusieurs utilisateurs signalent le **même problème** (exemple : Internet en panne pour tout le monde) :

1. Créer un **ticket principal** (le problème global)
2. Lier tous les autres tickets à ce ticket principal :

```
Ticket → Onglet "Liens" → Ajouter un lien → "Est lié à"
```

Quand vous résolvez le ticket principal, tous les tickets liés se résolvent automatiquement.

---

### 5.2 L'inventaire du parc

> L'inventaire, c'est le **cadastre** de votre DSI.  
> Tout équipement doit être enregistré : on sait où il est, qui l'utilise, et dans quel état il est.

#### Les catégories d'équipements dans GLPI

```
Parc
├── 💻  Ordinateurs          → PC fixes, portables, serveurs
├── 🖥️  Moniteurs            → Écrans
├── 🖨️  Périphériques        → Imprimantes, scanners, docks
├── 📱  Téléphones           → Téléphones IP, mobiles
├── 🔌  Matériel réseau      → Switchs, routeurs, bornes WiFi
├── 📦  Logiciels            → Licences logicielles
├── 🏢  Racks                → Baies serveurs
└── 🔧  Éléments réseaux     → Câbles, SFP, prises...
```

#### Créer un équipement manuellement

**Navigation :**
```
Parc → Ordinateurs → Ajouter
```

**Les informations importantes à renseigner :**

| Champ | Description | Exemple |
|-------|-------------|---------|
| **Nom** | Identifiant unique de la machine | `PC-COMPTA-05` |
| **Type** | Ordinateur de bureau, portable... | `Ordinateur de bureau` |
| **Fabricant** | Marque | `Dell` |
| **Modèle** | Référence | `OptiPlex 7090` |
| **Numéro de série** | N° unique gravé sur la machine | `SN4521678` |
| **Numéro d'inventaire** | Votre numéro interne | `INV-2024-0542` |
| **Statut** | État de l'équipement | `En production` |
| **Lieu** | Où est la machine | `Bâtiment B > Bureau 214` |
| **Utilisateur** | Qui utilise la machine | `Jean DUPONT` |
| **Groupe** | Département | `Comptabilité` |
| **Date d'achat** | Pour la garantie | `15/03/2024` |
| **Garantie** | Durée et expiration | `3 ans → 14/03/2027` |

#### Les statuts d'un équipement

| Statut | Signification |
|--------|---------------|
| **En production** | En service, utilisé |
| **En stock** | Disponible, non attribué |
| **En réparation** | En maintenance |
| **Réformé** | Retiré du service |
| **Volé** | Signalé comme volé |
| **En prêt** | Prêté temporairement |

> **💡 Astuce :** Les statuts sont personnalisables.  
> Vous pouvez créer vos propres statuts selon les besoins de votre entreprise.

#### Ajouter des composants à un ordinateur

Dans la fiche d'un ordinateur, vous pouvez renseigner les composants :

```
Onglet "Composants"
├── Processeur (CPU)    → Intel Core i7-12700, 4.9GHz
├── Mémoire vive (RAM)  → 2 × 8Go DDR4 3200MHz
├── Disque dur/SSD      → Samsung SSD 500Go
├── Carte graphique     → Intel UHD Graphics 770
├── Carte réseau        → Intel I225-V 2.5Gb
└── Système d'exploitation → Windows 11 Pro
```

#### Associer un ticket à un équipement

C'est une des fonctionnalités les plus puissantes de GLPI !

```
Fiche équipement → Onglet "Tickets" → Voir l'historique des incidents
```

Ainsi, si un PC tombe en panne 3 fois en 6 mois, vous pouvez le voir d'un coup d'œil et décider de le **remplacer** plutôt que de continuer à le réparer.

#### Gérer les logiciels et licences

**Navigation :**
```
Parc → Logiciels → Ajouter
```

**Exemple de fiche logiciel :**

```
Nom            : Microsoft Office 365
Éditeur        : Microsoft
Version        : 365 (2024)
Type de licence: Volume (OLP)
Nombre         : 150 licences
Date d'expiration : 31/12/2026
Renouvellement automatique : Oui
```

> **⚠️ Cas pratique en entreprise :**  
> Un auditeur vient vérifier vos licences logicielles. Avec GLPI, vous imprimez en 2 clics la liste de tous vos logiciels avec leurs licences et leurs postes d'installation. Sans GLPI, c'est une journée de travail pour chercher tout ça.

#### L'inventaire automatique avec l'agent GLPI

En production, on n'ajoute pas les équipements à la main. On utilise un **agent** qui tourne sur chaque poste et remonte automatiquement les informations :

```
Poste client                   Serveur GLPI
┌──────────────┐               ┌──────────────┐
│ Agent GLPI   │──HTTP/HTTPS──▶│ GLPI         │
│ (service     │               │              │
│  Windows)    │               │  Base de     │
│              │◀─ Commandes ──│  données     │
│  Collecte :  │               │              │
│  - CPU       │               │  Inventaire  │
│  - RAM       │               │  automatique │
│  - Logiciels │               │              │
│  - Réseau    │               │              │
└──────────────┘               └──────────────┘
```

Installation de l'agent sur Windows :

```batch
REM Télécharger l'agent GLPI depuis glpi-project.org
REM Installer et configurer avec l'URL du serveur GLPI
glpi-agent-installer.exe /server=http://glpi.monentreprise.fr/
```

---

### 5.3 Gestion des utilisateurs

#### Créer un utilisateur manuellement

**Navigation :**
```
Administration → Utilisateurs → Ajouter
```

**Champs essentiels :**

| Champ | Description | Exemple |
|-------|-------------|---------|
| **Login** | Identifiant de connexion | `jdupont` |
| **Nom** | Nom de famille | `DUPONT` |
| **Prénom** | Prénom | `Jean` |
| **Email** | Pour les notifications | `j.dupont@entreprise.fr` |
| **Mot de passe** | Ou via LDAP | `*****` |
| **Profil** | Technicien, Normal... | `Normal` |
| **Entité** | Périmètre de l'utilisateur | `Siège social` |
| **Localisation** | Bureau de l'utilisateur | `Bâtiment B - Bureau 214` |
| **Téléphone** | Pour le recontacter | `01 23 45 67 89` |

#### Synchronisation Active Directory (LDAP)

En entreprise, les comptes viennent généralement de l'**Active Directory** :

```
Configuration → Authentification → Annuaires LDAP

Paramètres à renseigner :
- Serveur LDAP    : dc01.monentreprise.local
- Port            : 389 (LDAP) ou 636 (LDAPS sécurisé)
- DN de base      : dc=monentreprise,dc=local
- Compte de bind  : cn=glpi-reader,ou=service,dc=...
- Mot de passe    : ****

Filtres :
- Filtre utilisateurs : (objectClass=user)
- Filtre groupes      : (objectClass=group)
```

> **💡 Astuce :** Avec l'intégration LDAP, les utilisateurs se connectent avec leurs identifiants Windows habituels. Plus besoin de gérer deux mots de passe !

#### Les entités — Notion importante

> Une **entité** dans GLPI, c'est comme une **division dans l'organigramme**.

```
Entité Racine (Entreprise ACME)
├── Siège Social (Paris)
│   ├── Direction
│   ├── Comptabilité
│   └── RH
├── Agence Lyon
│   ├── Commercial
│   └── Technique
└── Agence Marseille
    ├── Commercial
    └── Support
```

Chaque technicien peut être limité à son entité : le technicien de Lyon ne voit que les tickets de Lyon.

---

### 5.4 Les SLA — Contrats de service

> Un **SLA** (Service Level Agreement) = un **engagement de délai**.  
> "Nous nous engageons à traiter les incidents critiques en moins de 4 heures."

#### Pourquoi c'est important ?

Sans SLA :
- Les tickets restent ouverts indéfiniment
- Aucun engagement envers les utilisateurs
- Impossible de mesurer la performance du support

Avec SLA :
- Les délais sont définis et mesurés
- GLPI alerte automatiquement quand un délai est dépassé
- Le management peut mesurer la qualité du support

#### Créer un SLA dans GLPI

**Navigation :**
```
Administration → SLA → Ajouter
```

**Exemple de SLA classique :**

| Priorité | Délai de prise en charge | Délai de résolution |
|----------|--------------------------|---------------------|
| Très haute | 30 minutes | 4 heures |
| Haute | 2 heures | 8 heures |
| Moyenne | 4 heures | 24 heures |
| Basse | 8 heures | 72 heures |

**Configuration dans GLPI :**
```
Nom du SLA   : SLA Standard Entreprise
Calendrier   : Lundi-Vendredi 8h00-18h00 (hors jours fériés)
               (Important ! Un délai de 4h ne court pas le week-end)
```

#### Les calendriers

Pensez à définir un calendrier de travail, sinon GLPI compte 24h/24 7j/7 !

```
Configuration → Calendriers → Ajouter

Nom         : Heures ouvrées
Horaires    : Lundi au Vendredi
              8h00 → 12h00
              13h30 → 18h00
Jours fériés : Oui (importer la liste des jours fériés)
```

#### Les escalades automatiques

Quand un délai SLA est dépassé, GLPI peut :
- Envoyer un **email d'alerte** au responsable
- **Changer la priorité** du ticket automatiquement
- **Réassigner** le ticket à un autre technicien

---

### 5.5 Rapports et statistiques

> En tant que technicien N2, vous devez être capable de **produire des rapports** pour votre responsable.

#### Les rapports natifs de GLPI

**Navigation :**
```
Administration → Rapports
```

**Rapports disponibles :**

| Rapport | Contenu |
|---------|---------|
| **Rapport global** | Nombre de tickets par période, par technicien, par catégorie |
| **Rapport par équipement** | Incidents par machine |
| **Rapport par utilisateur** | Tickets par utilisateur |
| **Contrats** | Expiration des contrats et garanties |
| **Réseau** | Inventaire réseau |

#### Les statistiques de tickets

**Navigation :**
```
Assistance → Statistiques
```

Vous pouvez filtrer par :
- Période (ce mois, ce trimestre, cette année)
- Technicien
- Catégorie
- Priorité
- Entité

**Exemple d'analyse utile :**

```
Question : "Quel est notre temps moyen de résolution ?"

Assistance → Statistiques → Par priorité
→ Affiche le temps moyen de résolution pour chaque priorité
→ Comparez avec vos engagements SLA
```

#### Tableau de bord personnalisé

GLPI 10.x permet de créer des **tableaux de bord** personnalisés :

```
Accueil → "+" (Ajouter un widget)

Widgets disponibles :
├── Nombre de tickets ouverts
├── Tickets par technicien
├── Respect des SLA (%)
├── Top 5 des catégories d'incidents
└── Tickets créés cette semaine (graphe)
```

---

### 5.6 Administration avancée

#### Les règles métier — L'automatisation intelligente

> Les règles métier, c'est l'**automatisation** dans GLPI.  
> "Si un ticket correspond à certains critères, alors faire automatiquement quelques chose."

**Navigation :**
```
Administration → Règles → Règles métier pour les tickets
```

**Exemples de règles utiles :**

**Règle 1 : Attribution automatique par catégorie**
```
SI   catégorie = "Réseau"
ALORS assigner au groupe "Équipe Réseau"
```

**Règle 2 : Élévation de priorité automatique**
```
SI   titre contient "production" ou "serveur"
ET   urgence = Haute
ALORS priorité = Très haute
ET   notifier "chef-dsi@entreprise.fr"
```

**Règle 3 : SLA automatique selon le lieu**
```
SI   entité = "Agence Lyon"
ALORS appliquer SLA "SLA Lyon"
```

#### Les modèles de tickets

Pour éviter que les utilisateurs créent des tickets vides ou mal remplis, créez des **modèles** :

**Navigation :**
```
Assistance → Modèles de tickets → Ajouter
```

**Exemple — Modèle "Demande d'accès VPN" :**
```
Type       : Demande (forcé, non modifiable)
Titre      : "Demande d'accès VPN - [NOM UTILISATEUR]"
Catégorie  : Réseau > VPN (forcée)
Description: 
"Merci de remplir les informations suivantes :
- Nom complet : 
- Service : 
- Raison de la demande : 
- Date de début souhaitée : 
- Date de fin (si temporaire) :"

Champs obligatoires : titre, description, lieu
```

#### Les notifications par email

Configurez GLPI pour envoyer des emails automatiques :

**Navigation :**
```
Configuration → Notifications → Ajouter
```

**Événements déclencheurs disponibles :**
- Nouveau ticket
- Ticket assigné
- Nouveau suivi
- Ticket résolu
- SLA dépassé
- Ticket clôturé

**Configuration du serveur email :**
```
Configuration → Notifications → Configuration email

Serveur SMTP  : smtp.monentreprise.fr
Port          : 587 (STARTTLS) ou 465 (SSL)
Email expéditeur : glpi@monentreprise.fr
Authentification : Oui
Login          : glpi-notification@monentreprise.fr
Mot de passe   : ****
```

#### Plugins utiles pour la production

GLPI peut être étendu avec des **plugins** :

| Plugin | Description | Utilité |
|--------|-------------|---------|
| **FusionInventory** | Inventaire automatique | Indispensable en production |
| **OCS Inventory** | Alternative à FusionInventory | Compatible OCS |
| **Monitoring** | Intégration Nagios/Centreon | Liens tickets/alertes |
| **LDAP Synchronization** | Synchro Active Directory | Automatisation |
| **PDF** | Export PDF des objets | Rapports personnalisés |
| **Dasboard** | Tableaux de bord avancés | Reporting |

---

## 6. Cas pratiques

### 🛠️ TP1 — Niveau 1 : Créer et suivre un ticket

**Scénario :**
> Marie MARTIN (service RH) appelle pour signaler que son imprimante réseau `IMP-RH-01` n'imprime plus depuis ce matin. Elle a 15 contrats à imprimer pour une réunion à 14h.

**À faire :**
1. Connectez-vous avec le compte `normal` / `normal`
2. Créez un ticket d'incident avec les bonnes informations
3. Joignez une capture d'écran fictive
4. Vérifiez le statut du ticket après création

**Attendu :**
```
Type        : Incident
Titre       : Imprimante IMP-RH-01 hors service - Besoin avant 14h
Catégorie   : Matériel > Imprimante
Urgence     : Haute
Impact      : Moyen
Description : [Détaillée avec qui, quoi, quand, où]
```

---

### 🛠️ TP2 — Niveau 2 : Traiter et documenter un ticket

**Scénario :**
> Vous êtes technicien. Vous recevez le ticket du TP1. Vous devez intervenir.

**À faire :**
1. Connectez-vous avec le compte `tech` / `tech`
2. Prenez en charge le ticket
3. Requalifiez si nécessaire
4. Ajoutez 2 suivis (un public, un privé)
5. Résolvez le ticket avec une solution détaillée
6. Convertissez la solution en article de base de connaissances

**Solution attendue :**
```
Diagnostic   : Vérification de la file d'impression
               → File bloquée avec 47 documents en attente
Action       : Suppression de la file d'impression
               net stop spooler
               del /Q /F /S "%systemroot%\System32\spool\PRINTERS\*.*"
               net start spooler
Résultat     : Imprimante opérationnelle à 11h30
Temps passé  : 20 minutes
```

---

### 🛠️ TP3 — Inventaire : Créer une fiche équipement complète

**Scénario :**
> Vous venez de recevoir un nouveau PC pour la comptabilité. Enregistrez-le dans GLPI.

**Informations du PC :**
```
Marque       : Lenovo
Modèle       : ThinkCentre M90q Gen 3
Numéro série : LNV-PF4T891K
OS           : Windows 11 Pro 23H2
RAM          : 16 Go DDR5
SSD          : 512 Go NVMe
Utilisateur  : Pierre DURAND
Lieu         : Bâtiment A, Bureau 105
Achat        : 10/02/2026 - 1200€ - Fournisseur : IT Shop France
Garantie     : 3 ans (09/02/2029)
```

**À faire :**
1. Créez la fiche ordinateur complète
2. Ajoutez les composants (CPU, RAM, SSD)
3. Associez l'utilisateur et le lieu
4. Renseignez les informations de garantie
5. Ajoutez le logiciel Windows 11 Pro

---

### 🛠️ TP4 — Analyse : Produire un rapport mensuel

**À faire :**
1. Connectez-vous en Super-Admin
2. Rendez-vous dans Administration → Rapports
3. Générez le rapport du mois en cours
4. Répondez aux questions suivantes :
   - Combien de tickets ont été créés ce mois ?
   - Quel est le temps moyen de résolution ?
   - Quelle est la catégorie la plus fréquente ?
   - Y a-t-il des SLA dépassés ?

---

## 7. Astuces et bonnes pratiques

### 💡 Astuces pour les techniciens

**Astuce 1 — Utiliser les filtres et les vues**
```
Dans la liste des tickets, configurez votre vue :
→ Tickets non assignés → à prendre en charge
→ Mes tickets en cours → ce que je dois traiter
→ Tickets en retard → urgences
```

**Astuce 2 — Les raccourcis clavier**
```
Ctrl + Entrée  → Sauvegarder un formulaire
F5             → Actualiser la liste des tickets
Alt + ←        → Retour à la page précédente
```

**Astuce 3 — L'historique de chaque objet**
```
Sur n'importe quelle fiche (ticket, équipement, utilisateur)
→ Onglet "Historique"
→ Vous voyez tout ce qui a été modifié, par qui, quand
```

**Astuce 4 — La recherche globale**
```
La barre "Chercher dans le menu" en haut à gauche
→ Tapez directement un numéro de ticket, un nom, un numéro de série
→ GLPI cherche partout en une seconde
```

**Astuce 5 — Les modèles pour gagner du temps**
```
Si vous créez souvent le même type de ticket (maintenance mensuelle, etc.)
→ Créez un modèle avec les champs pré-remplis
→ Gain de temps considérable en production
```

**Astuce 6 — Lier équipements et tickets**
```
Toujours associer le ticket à l'équipement concerné
→ Cela construit l'historique des pannes
→ Vous pourrez justifier un remplacement au management
```

---

### 🏆 Bonnes pratiques professionnelles

**Pour les tickets :**

| ✅ Faire | ❌ Ne pas faire |
|---------|----------------|
| Titre clair et précis | Titre vague : "problème PC" |
| Description détaillée | Laisser la description vide |
| Bonne catégorie | Tout mettre dans "Autre" |
| Urgence cohérente | Tout mettre en "Très haute" |
| Suivis réguliers | Laisser un ticket sans nouvelles pendant 5 jours |
| Solution documentée | Résoudre sans expliquer |

**Pour l'inventaire :**

| ✅ Faire | ❌ Ne pas faire |
|---------|----------------|
| Numéro de série systématique | Pas de N/S = pas traçable |
| Mettre à jour le statut | Laisser "en production" un PC réformé |
| Dater les achats | Impossible de gérer les garanties |
| Lier aux utilisateurs | Équipement sans propriétaire |
| Renseigner les garanties | Ne pas savoir si c'est sous garantie |

**Pour l'administration :**

| ✅ Faire | ❌ Ne pas faire |
|---------|----------------|
| Changer les mots de passe par défaut | Laisser glpi/glpi en prod |
| Sauvegarder la BDD régulièrement | Pas de backup |
| Archiver les vieux tickets | Laisser tout dans "en cours" |
| Former les utilisateurs | Laisser les utilisateurs sans guide |
| Documenter les règles métier | Règles sans explication |

---

### 🔒 Sécurité GLPI — Points importants

**En production, vérifiez :**

```
1. ✅ Compte glpi/glpi supprimé ou mot de passe changé
2. ✅ Dossier /install/ supprimé après installation
3. ✅ HTTPS activé (certificat SSL)
4. ✅ Accès admin limité aux IP internes
5. ✅ Sauvegardes automatiques de la base de données
6. ✅ Mises à jour GLPI régulières
7. ✅ Logs de connexion activés
8. ✅ Timeout de session configuré
```

---

## 8. Lexique

| Terme | Définition simple |
|-------|-------------------|
| **GLPI** | Gestionnaire Libre de Parc Informatique |
| **ITSM** | Information Technology Service Management — gestion des services IT |
| **ITAM** | Information Technology Asset Management — gestion des actifs IT |
| **SLA** | Service Level Agreement — contrat de niveau de service (délais) |
| **Incident** | Quelque chose qui ne fonctionne plus normalement |
| **Demande** | Besoin d'un nouveau service ou équipement |
| **Ticket** | Fiche de suivi d'un incident ou d'une demande |
| **Entité** | Division organisationnelle dans GLPI |
| **Profil** | Rôle d'un utilisateur (ce qu'il peut voir/faire) |
| **LDAP** | Protocole pour se connecter à l'Active Directory |
| **FusionInventory** | Plugin pour l'inventaire automatique des postes |
| **Escalade** | Transfert d'un ticket vers un niveau de support supérieur |
| **Base de connaissances** | FAQ interne des solutions aux problèmes courants |
| **Suivi** | Commentaire ajouté sur un ticket (journal d'actions) |
| **Acteur** | Personne liée à un ticket (demandeur, technicien, observateur) |
| **Règle métier** | Automatisation basée sur des conditions |
| **N1 / N2 / N3** | Niveaux de support (N1 = basique, N3 = expert) |
| **Parc** | L'ensemble du matériel et logiciels gérés |
| **Statut** | État d'un ticket ou d'un équipement |
| **Plugin** | Extension qui ajoute des fonctionnalités à GLPI |

---

## 📋 Résumé des accès rapides

| Fonctionnalité | Navigation |
|----------------|------------|
| Créer un ticket | Assistance → Créer un ticket |
| Voir mes tickets | Assistance → Tickets |
| Inventaire PC | Parc → Ordinateurs |
| Inventaire logiciels | Parc → Logiciels |
| Gestion utilisateurs | Administration → Utilisateurs |
| Base de connaissances | Outils → Base de connaissances |
| Statistiques | Assistance → Statistiques |
| Rapports | Administration → Rapports |
| SLA | Administration → SLA |
| Règles métier | Administration → Règles |
| Notifications | Configuration → Notifications |
| Calendriers | Configuration → Calendriers |
| Plugins | Configuration → Plugins |

---

## 🎯 Objectifs de fin de formation

### Niveau 1 — Je suis capable de :
- [ ] Me connecter à GLPI
- [ ] Créer un ticket d'incident bien documenté
- [ ] Créer une demande de service
- [ ] Suivre l'évolution de mes tickets
- [ ] Utiliser la base de connaissances
- [ ] Valider ou rejeter une résolution

### Niveau 2 — Je suis capable de :
- [ ] Gérer et traiter des tickets
- [ ] Qualifier et requalifier des tickets
- [ ] Escalader vers le bon niveau
- [ ] Créer et maintenir l'inventaire du parc
- [ ] Créer et gérer des utilisateurs
- [ ] Configurer des SLA et calendriers
- [ ] Produire des rapports et statistiques
- [ ] Créer des règles métier simples
- [ ] Gérer la base de connaissances
- [ ] Configurer les notifications

---

*Document rédigé pour la formation TSSR — Nextformation*  
*Version 1.0 — Février 2026*  
*🔄 À mettre à jour à chaque nouvelle version de GLPI*
