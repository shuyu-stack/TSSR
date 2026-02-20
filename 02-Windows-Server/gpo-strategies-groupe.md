# GPO - Stratégies de Groupe (Group Policy Objects)

> 📚 **Module :** Windows Server - Administration Active Directory
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 8-10 heures
> 🎯 **Niveau :** Fondamental (CRITIQUE pour examen + emploi)
> 🎓 **Formateur virtuel :** Architecte réseau avec +20 ans d'expérience
> 🖱️ **Méthode pédagogique :** Interface graphique en priorité

---

## 👨‍🏫 Message de votre formateur

> **Écoutez-moi bien, c'est LA compétence N°1 qu'on vous demandera en entreprise :**
>
> **Les GPO (Group Policy Objects) = Votre super-pouvoir en tant que TSSR.**
>
> Imaginez : Vous devez installer un logiciel sur 500 postes. Sans GPO, vous allez mettre **1 semaine** à faire le tour. Avec GPO ? **5 minutes** de configuration, vous prenez un café, et c'est fait automatiquement pendant la nuit.
>
> **En 20 ans, j'ai vu des techniciens se tuer à la tâche en faisant tout manuellement**, alors qu'une simple GPO aurait économisé 90% du temps.
>
> **Les GPO permettent de :**
> - ✅ Mapper des lecteurs réseau automatiquement (plus de tickets "je ne vois pas le lecteur Z:")
> - ✅ Installer des logiciels sans bouger de votre bureau
> - ✅ Configurer la sécurité (pare-feu, mots de passe complexes, verrouillage écran)
> - ✅ Personnaliser les bureaux Windows (fond d'écran, démarrage, etc.)
> - ✅ Déployer des scripts PowerShell au démarrage/arrêt
>
> **À l'examen TSSR, il y a 95% de chances** d'avoir un exercice pratique GPO. C'est quasi-certain. Généralement : "Créer une GPO qui mappe un lecteur réseau" ou "Déployer un fond d'écran d'entreprise".
>
> **En entretien d'embauche**, si vous dites "Je maîtrise les GPO", les recruteurs IT savent que vous allez leur faire gagner un temps MONSTRE. C'est un argument massue.

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction - Qu'est-ce qu'une GPO ?](#-introduction)
- [Partie 1 : Concepts fondamentaux](#-partie-1--concepts-fondamentaux)
- [Partie 2 : Créer votre première GPO](#-partie-2--créer-votre-première-gpo)
- [Partie 3 : Mappage de lecteurs réseau](#-partie-3--mappage-de-lecteurs-réseau-le-cas-dusage-1)
- [Partie 4 : Configuration du Bureau Windows](#-partie-4--configuration-du-bureau-windows)
- [Partie 5 : Sécurité avec les GPO](#-partie-5--sécurité-avec-les-gpo)
- [Partie 6 : Déploiement de scripts](#-partie-6--déploiement-de-scripts)
- [Partie 7 : Diagnostic et dépannage](#-partie-7--diagnostic-et-dépannage-gpo)
- [Cas d'usage réels en entreprise](#-cas-dusage-réels-en-entreprise)
- [Pièges à éviter](#-pièges-à-éviter)
- [Exercices pratiques](#-exercices-pratiques)
- [Checklist pour l'examen](#-checklist-pour-lexamen)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** ce qu'est une GPO et pourquoi elle est essentielle
- ✅ **Créer** une GPO depuis la console GPMC (Group Policy Management Console)
- ✅ **Lier** une GPO à une OU (Organizational Unit)
- ✅ **Mapper** automatiquement des lecteurs réseau avec une GPO
- ✅ **Configurer** le Bureau Windows (fond d'écran, économiseur d'écran, etc.)
- ✅ **Appliquer** des paramètres de sécurité (mots de passe, pare-feu)
- ✅ **Déployer** des scripts PowerShell ou Batch au démarrage
- ✅ **Diagnostiquer** pourquoi une GPO ne s'applique pas
- ✅ **Forcer** l'application immédiate d'une GPO
- ✅ **Réussir** l'exercice pratique à l'examen TSSR

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [x] Avoir un contrôleur de domaine Active Directory opérationnel
- [x] Avoir créé des Unités d'Organisation (OUs)
- [x] Avoir des utilisateurs et/ou ordinateurs dans les OUs
- [x] Avoir les droits d'administrateur du domaine
- [ ] *Recommandé :* Avoir suivi le cours "Active Directory Domain Services"

**Matériel nécessaire :**
- 💻 Windows Server 2019/2022/2025 (contrôleur de domaine)
- 🖥️ 1-2 postes clients Windows 10/11 joints au domaine
- 🌐 Partage réseau créé (pour le mappage de lecteurs)
- 📝 Patience et envie de pratiquer !

---

## 📚 Introduction

### Qu'est-ce qu'une GPO (Group Policy Object) ?

Une **GPO** est un **ensemble de règles** que vous créez une seule fois et qui s'applique automatiquement à des centaines/milliers d'utilisateurs ou d'ordinateurs.

**Analogie simple :**

Imaginez une école avec 1000 élèves :

❌ **Sans GPO (méthode manuelle) :**
```
Directeur : "Tous les élèves doivent porter l'uniforme bleu"
→ Le directeur doit aller voir CHAQUE élève, un par un
→ Temps : 1 semaine
→ Erreurs : certains élèves oubliés
```

✅ **Avec GPO (automatisation) :**
```
Directeur : "Règle : Tous les élèves de l'école portent l'uniforme bleu"
→ La règle s'applique automatiquement à tous les élèves
→ Temps : 5 minutes pour créer la règle
→ Nouveaux élèves : ils reçoivent automatiquement l'uniforme
```

**En informatique, c'est pareil :**

| Tâche | Sans GPO | Avec GPO |
|-------|----------|----------|
| Mapper le lecteur Z: sur 200 postes | 200 fois manuellement (2-3 jours) | 1 GPO (10 minutes) |
| Installer un logiciel sur 500 PC | Faire le tour de l'entreprise (1 semaine) | 1 GPO (30 minutes) |
| Changer le fond d'écran de tous les postes | Mission impossible manuellement | 1 GPO (5 minutes) |
| Appliquer une politique de mot de passe complexe | Impossible à contrôler manuellement | 1 GPO (2 minutes) |

### Pourquoi les GPO sont-elles essentielles ?

#### 💰 **Gain de temps = Gain d'argent**

**Exemple réel vécu :**
```
Entreprise : 300 employés
Tâche : Mapper 3 lecteurs réseau (Commun, Personnel, Service)

Sans GPO :
- 5 min par poste × 300 postes = 1500 minutes = 25 heures = 3 jours de travail
- Coût technicien : 3 jours × 200€/jour = 600€

Avec GPO :
- Créer 3 GPO : 30 minutes
- Coût : 0,5h × 25€/h = 12,50€

Économie : 587,50€ + des employés contents dès le premier jour
```

#### 🎯 **Standardisation**

Tous les postes ont la même configuration. Pas de "chez moi ça marche pas pareil".

#### 🔒 **Sécurité**

Appliquer des règles de sécurité (pare-feu, antivirus, mots de passe) de manière uniforme.

#### 🚀 **Évolutivité**

Vous embauchez 50 personnes demain ? Ils héritent automatiquement de toutes les GPO. Rien à faire !

---

## 🔷 Partie 1 : Concepts fondamentaux

### Les 3 composants d'une GPO

```
┌─────────────────────────────────────────┐
│         1. CRÉER la GPO                 │  "Règle : Mapper lecteur Z:"
│    (Group Policy Management Console)   │
└─────────────────┬───────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────┐
│         2. LIER la GPO à une OU         │  "Cette règle s'applique au service Compta"
│         (Group Policy Management)       │
└─────────────────┬───────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────┐
│      3. APPLIQUER automatiquement       │  "Tous les PC de Compta reçoivent le lecteur Z:"
│    (au démarrage du PC ou connexion)    │
└─────────────────────────────────────────┘
```

### Configuration Ordinateur vs Configuration Utilisateur

Une GPO contient **2 sections** :

#### 🖥️ **Computer Configuration** (Configuration Ordinateur)
- S'applique à la **machine** (peu importe qui se connecte)
- Moment : Au **démarrage de l'ordinateur**
- Exemples :
  - Installer un logiciel (ex: antivirus)
  - Configurer le pare-feu
  - Paramètres réseau
  - Scripts de démarrage

#### 👤 **User Configuration** (Configuration Utilisateur)
- S'applique à l'**utilisateur** (peu importe sur quel PC il se connecte)
- Moment : À la **connexion de l'utilisateur**
- Exemples :
  - Mapper des lecteurs réseau
  - Fond d'écran personnalisé
  - Raccourcis bureau
  - Stratégie de mot de passe

**Quelle section choisir ?**

| Besoin | Section |
|--------|---------|
| Installer un antivirus sur tous les PC | 🖥️ Computer |
| Mapper un lecteur personnel pour chaque utilisateur | 👤 User |
| Configurer le pare-feu Windows | 🖥️ Computer |
| Changer le fond d'écran | 👤 User (ou Computer si pour tous) |
| Script de nettoyage au démarrage du PC | 🖥️ Computer |

> 💡 **Astuce de pro :**
> En cas de doute, demandez-vous : "Est-ce que ça concerne la machine ou la personne ?" Si c'est la personne → User. Si c'est la machine → Computer.

### L'ordre d'application des GPO (LSDOU)

Les GPO s'appliquent dans un ordre précis :

```
1. Local (GPO locale de la machine)
   ↓
2. Site (GPO du site Active Directory)
   ↓
3. Domain (GPO du domaine)
   ↓
4. OU (GPO de l'Unité d'Organisation)
   ↓
   La DERNIÈRE appliquée gagne (sauf si "Enforced")
```

**Mnémotechnique : LSDOU**
- **L**ocal
- **S**ite
- **D**omain
- **OU**

> ⚠️ **Important :** La GPO liée à l'OU la plus basse dans l'arborescence a la priorité. Si deux GPO configurent le même paramètre, c'est la dernière appliquée qui gagne.

### Héritage et blocage

**Héritage :** Par défaut, une GPO s'applique à l'OU et à toutes les sous-OUs.

```
OU : Entreprise (GPO : Fond d'écran entreprise)
  ├─ OU : Comptabilité (hérite du fond d'écran)
  └─ OU : Direction (hérite du fond d'écran)
```

**Bloquer l'héritage :** Vous pouvez empêcher une OU de recevoir les GPO des OUs parents.

**Enforced (Appliqué en force) :** Une GPO "Enforced" s'applique même si l'OU bloque l'héritage. C'est le mode "dictateur" des GPO 😄

---

## 🔷 Partie 2 : Créer votre première GPO

### Étape 1 : Ouvrir la console de gestion des GPO (GPMC)

**Sur votre contrôleur de domaine Windows Server :**

1. Appuyez sur la touche **Windows** (ou cliquez sur le menu Démarrer)
2. Tapez : `gpmc.msc`
3. Appuyez sur **Entrée**

**Alternative :**
1. **Server Manager** (Gestionnaire de serveur)
2. En haut à droite : **Tools** (Outils)
3. Cliquez sur : **Group Policy Management** (Gestion des stratégies de groupe)

✅ **La console GPMC s'ouvre.**

Vous devriez voir :
```
Group Policy Management
├─ Forest: solaris.local
│  └─ Domains
│     └─ solaris.local
│        ├─ Domain Controllers (OU)
│        ├─ Group Policy Objects (dossier des GPO)
│        └─ Vos OUs (Comptabilité, Direction, etc.)
```

### Étape 2 : Créer une nouvelle GPO (Test)

Pour apprendre, on va créer une GPO de test simple : afficher un message au démarrage de Windows.

**Procédure :**

1. Dans le volet de gauche (arborescence), dépliez :
   - **Forest: solaris.local**
   - **Domains**
   - **solaris.local**

2. Faites un **clic droit** sur **Group Policy Objects**

3. Cliquez sur **New** (Nouveau)

4. Une fenêtre s'ouvre :
   - **Name** (Nom) : `TEST - Message Bienvenue`
   - **Source Starter GPO** : Laissez vide (None)

5. Cliquez sur **OK**

✅ **Votre première GPO est créée !**

Elle apparaît maintenant dans le dossier "Group Policy Objects".

> 💡 **Conseil :** Utilisez un système de nommage clair. Exemples :
> - `GPO - Mappage Lecteurs Compta`
> - `GPO - Fond Écran Entreprise`
> - `TEST - [Description]` (pour les tests)

### Étape 3 : Éditer la GPO

Maintenant qu'elle est créée, il faut la **configurer** (définir ce qu'elle fait).

1. Dans le dossier **Group Policy Objects**, trouvez votre GPO : `TEST - Message Bienvenue`

2. Faites un **clic droit** dessus

3. Cliquez sur **Edit** (Modifier)

✅ **L'éditeur de stratégie de groupe s'ouvre.**

Vous voyez deux grandes sections :
```
Group Policy Management Editor
├─ Computer Configuration (Configuration Ordinateur)
│  ├─ Policies (Stratégies)
│  └─ Preferences (Préférences)
└─ User Configuration (Configuration Utilisateur)
   ├─ Policies (Stratégies)
   └─ Preferences (Préférences)
```

### Étape 4 : Configurer un message de bienvenue

On va afficher un message au démarrage de Windows (comme "Bienvenue sur le réseau SOLARIS").

**Navigation dans l'éditeur :**

1. Dépliez : **Computer Configuration** (Configuration Ordinateur)
2. Dépliez : **Policies** (Stratégies)
3. Dépliez : **Windows Settings** (Paramètres Windows)
4. Dépliez : **Security Settings** (Paramètres de sécurité)
5. Cliquez sur : **Local Policies** → **Security Options** (Options de sécurité)

6. Dans le volet de droite, cherchez :
   - **Interactive logon: Message text for users attempting to log on**

7. **Double-cliquez** dessus

8. Une fenêtre s'ouvre :
   - Cochez : **Define this policy setting** (Définir ce paramètre)
   - Dans la zone de texte, tapez :
     ```
     Bienvenue sur le réseau SOLARIS
     Veuillez respecter la charte informatique
     ```
   - Cliquez **OK**

9. Juste en dessous, configurez aussi :
   - **Interactive logon: Message title for users attempting to log on**
   - Double-cliquez
   - Cochez **Define this policy setting**
   - Titre : `🏢 Réseau SOLARIS`
   - Cliquez **OK**

10. **Fermez** l'éditeur de stratégie de groupe

✅ **Votre GPO est maintenant configurée !**

### Étape 5 : Lier la GPO à une OU

Pour que la GPO s'applique, il faut la **lier** à une Unité d'Organisation (OU).

**Retour dans la console GPMC :**

1. Dans le volet de gauche, sous **solaris.local**, trouvez votre OU
   - Par exemple : **Comptabilité**

2. Faites un **clic droit** sur l'OU **Comptabilité**

3. Cliquez sur **Link an Existing GPO...** (Lier un objet de stratégie de groupe existant)

4. Une fenêtre s'ouvre avec la liste de toutes vos GPO

5. Sélectionnez : **TEST - Message Bienvenue**

6. Cliquez **OK**

✅ **La GPO est maintenant liée à l'OU Comptabilité !**

Vous devriez voir sous l'OU "Comptabilité" votre GPO avec une petite icône de chaîne 🔗.

### Étape 6 : Tester l'application de la GPO

**Sur un poste client joint au domaine (dans l'OU Comptabilité) :**

1. Ouvrez une **invite de commandes** (cmd) en administrateur

2. Tapez :
   ```cmd
   gpupdate /force
   ```

3. Cette commande force l'application immédiate des GPO (normalement elles s'appliquent au redémarrage)

4. Attendez le message : "User Policy update has completed successfully."

5. **Redémarrez** le PC client

6. Au démarrage de Windows, **AVANT** l'écran de connexion, vous devriez voir :
   ```
   ┌─────────────────────────────────────┐
   │      🏢 Réseau SOLARIS              │
   ├─────────────────────────────────────┤
   │ Bienvenue sur le réseau SOLARIS     │
   │ Veuillez respecter la charte        │
   │ informatique                         │
   │                                      │
   │              [ OK ]                  │
   └─────────────────────────────────────┘
   ```

✅ **Bravo ! Votre première GPO fonctionne !** 🎉

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Créer une GPO en PowerShell
> New-GPO -Name "TEST - Message Bienvenue"
>
> # Lier la GPO à une OU
> New-GPLink -Name "TEST - Message Bienvenue" -Target "OU=Comptabilité,DC=solaris,DC=local"
>
> # Forcer la mise à jour sur un PC distant
> Invoke-GPUpdate -Computer "PC-COMPTA-01" -Force
> ```
> Utile quand vous devez créer 50 GPO d'un coup !

---

## 🔷 Partie 3 : Mappage de lecteurs réseau (le cas d'usage N°1)

Le **mappage de lecteurs réseau** est **LA raison N°1** pour laquelle vous créerez des GPO en entreprise.

### Scénario réel

**Contexte :**
- Entreprise SOLARIS avec 100 employés
- Serveur de fichiers : `\\SRV-FICHIERS\`
- Partages réseau existants :
  - `\\SRV-FICHIERS\Commun` → Accessible à tous (lecteur P:)
  - `\\SRV-FICHIERS\Comptabilite` → Réservé à la compta (lecteur K:)
  - `\\SRV-FICHIERS\Users\%username%` → Dossier personnel de chaque utilisateur (lecteur H:)

**Objectif :** Mapper automatiquement ces lecteurs pour tous les utilisateurs concernés.

### Créer la GPO de mappage

#### Étape 1 : Créer une nouvelle GPO

1. Console **GPMC** (gpmc.msc)
2. Clic droit sur **Group Policy Objects** → **New**
3. Nom : `GPO - Mappage Lecteurs Réseau`
4. OK

#### Étape 2 : Éditer la GPO

1. Clic droit sur la GPO **GPO - Mappage Lecteurs Réseau**
2. **Edit** (Modifier)

#### Étape 3 : Naviguer vers "Drive Maps" (Mappages de lecteurs)

**Important :** Le mappage de lecteurs se fait dans **User Configuration** (car c'est lié à l'utilisateur, pas à la machine).

**Navigation :**
1. Dépliez : **User Configuration**
2. Dépliez : **Preferences** (⚠️ PAS "Policies", mais "Preferences" !)
3. Dépliez : **Windows Settings**
4. Cliquez sur : **Drive Maps** (Mappages de lecteurs)

> 💡 **Pourquoi "Preferences" et pas "Policies" ?**
> - **Policies** : Paramètres strictement imposés (utilisateur ne peut pas modifier)
> - **Preferences** : Paramètres recommandés (utilisateur peut les modifier s'il veut)
> Pour les lecteurs réseau, on utilise généralement "Preferences".

#### Étape 4 : Créer le mappage du lecteur P: (Commun)

1. Dans le volet de droite (vide pour l'instant), faites un **clic droit**

2. Sélectionnez : **New** → **Mapped Drive** (Nouveau lecteur mappé)

3. Une fenêtre **New Drive Properties** s'ouvre. Remplissez :

   **Onglet "General" :**
   - **Action** : Sélectionnez **Create** (Créer)
   - **Location** : Tapez le chemin UNC du partage :
     ```
     \\SRV-FICHIERS\Commun
     ```
   - **Reconnect** : Cochez ✅ (pour que le lecteur se reconnecte au démarrage)
   - **Label as** : (optionnel) Tapez : `Dossier Commun`
   - **Drive Letter** : Sélectionnez **P:**
   - **Show this drive** : Laissez coché
   - **Show all drives** : Laissez coché

   **Onglet "Common" :**
   - Vous pouvez laisser par défaut (on verra plus tard pour le ciblage avancé)

4. Cliquez **OK**

✅ **Le lecteur P: est configuré !**

Vous voyez maintenant dans le volet de droite :
```
Name: P:
Action: Create
Path: \\SRV-FICHIERS\Commun
```

#### Étape 5 : Créer le mappage du lecteur H: (Personnel)

On va utiliser une **variable** pour que chaque utilisateur ait son propre dossier.

1. Clic droit dans le volet → **New** → **Mapped Drive**

2. Remplissez :
   - **Action** : **Create**
   - **Location** :
     ```
     \\SRV-FICHIERS\Users\%username%
     ```
     > La variable `%username%` sera automatiquement remplacée par le nom de l'utilisateur !
   - **Reconnect** : ✅
   - **Label as** : `Mes Documents`
   - **Drive Letter** : **H:**

3. **OK**

✅ **Le lecteur H: est configuré !**

**Résultat :**
- Si l'utilisateur "jdupont" se connecte → Il aura `H: = \\SRV-FICHIERS\Users\jdupont`
- Si l'utilisateur "mmartin" se connecte → Il aura `H: = \\SRV-FICHIERS\Users\mmartin`

> 💡 **Variables utiles :**
> - `%username%` : Nom de l'utilisateur (ex: jdupont)
> - `%userdomain%` : Domaine (ex: SOLARIS)
> - `%computername%` : Nom du PC
> - `%logonserver%` : Serveur de connexion

#### Étape 6 : Créer le mappage du lecteur K: (Comptabilité uniquement)

Ce lecteur ne doit être visible QUE pour les utilisateurs du service Comptabilité.

**Option 1 : Créer une GPO séparée et la lier uniquement à l'OU Comptabilité** (recommandé)

**Option 2 : Utiliser le ciblage de niveau élément (Item-Level Targeting)**

Voyons l'Option 2 (plus avancée mais puissante) :

1. Clic droit → **New** → **Mapped Drive**

2. Remplissez :
   - **Location** : `\\SRV-FICHIERS\Comptabilite`
   - **Drive Letter** : **K:**
   - **Label as** : `Compta`

3. Onglet **Common**
4. Cochez : **Item-level targeting** (Ciblage de niveau élément)
5. Cliquez sur le bouton **Targeting...** (Ciblage)

6. Une fenêtre **Targeting Editor** s'ouvre
7. Cliquez sur **New Item** → **Security Group** (Groupe de sécurité)

8. Remplissez :
   - **Group** : Cliquez sur le bouton **"..."**
   - Tapez : `GRP-Comptabilite` (le groupe de sécurité des comptables)
   - Cliquez **Check Names** → **OK**
   - **User in the group** : Laissez coché

9. Cliquez **OK** dans toutes les fenêtres

✅ **Le lecteur K: ne sera mappé QUE pour les membres du groupe "GRP-Comptabilite" !**

#### Étape 7 : Lier la GPO

1. Fermez l'éditeur
2. Dans GPMC, faites un clic droit sur votre OU racine (ex: "Utilisateurs")
3. **Link an Existing GPO**
4. Sélectionnez **GPO - Mappage Lecteurs Réseau**
5. OK

#### Étape 8 : Tester

**Sur un poste client :**

1. Connectez-vous avec un compte utilisateur du domaine

2. Ouvrez une invite de commandes :
   ```cmd
   gpupdate /force
   ```

3. **Déconnectez-vous** et **reconnectez-vous** (les lecteurs se mappent à la connexion)

4. Ouvrez **l'Explorateur de fichiers** (Windows + E)

5. Vous devriez voir :
   ```
   Ce PC
   ├─ (C:) Disque local
   ├─ (H:) Mes Documents (\\SRV-FICHIERS\Users\jdupont)
   ├─ (K:) Compta (\\SRV-FICHIERS\Comptabilite)  ← Si vous êtes dans le groupe Compta
   └─ (P:) Dossier Commun (\\SRV-FICHIERS\Commun)
   ```

✅ **Les lecteurs sont mappés automatiquement !** 🎉

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Créer une GPO de mappage de lecteur via PowerShell
> $GPO = New-GPO -Name "GPO - Mappage Lecteurs"
>
> # (La configuration du mappage nécessite des commandes avancées)
> # En pratique, on utilise l'interface pour créer le premier mappage
> # Puis on peut dupliquer/modifier via PowerShell
>
> # Forcer la mise à jour
> Invoke-GPUpdate -Computer "PC-CLIENT-01" -RandomDelayInMinutes 0
> ```

---

## 🔷 Partie 4 : Configuration du Bureau Windows

Vous voulez que tous les postes de l'entreprise aient le même fond d'écran (logo de l'entreprise, charte graphique) ? Les GPO font ça en 5 minutes.

### Cas d'usage : Déployer un fond d'écran d'entreprise

#### Étape 1 : Préparer l'image

1. Créez ou récupérez l'image de fond d'écran (ex: `logo-solaris.jpg`)
2. Placez-la dans un **partage réseau** accessible par tous :
   ```
   \\SRV-FICHIERS\Ressources\Fonds-Ecran\logo-solaris.jpg
   ```
   > Pensez à donner les permissions en lecture à "Domain Users" !

#### Étape 2 : Créer la GPO

1. GPMC → Clic droit **Group Policy Objects** → **New**
2. Nom : `GPO - Fond Écran Entreprise`
3. OK

#### Étape 3 : Éditer la GPO

1. Clic droit sur la GPO → **Edit**

2. Navigation :
   - **User Configuration**
   - **Policies** (cette fois-ci, pas Preferences)
   - **Administrative Templates** (Modèles d'administration)
   - **Desktop** (Bureau)
   - **Desktop** (encore)

3. Dans le volet de droite, trouvez :
   - **Desktop Wallpaper** (Papier peint du Bureau)

4. **Double-cliquez** dessus

5. Une fenêtre s'ouvre :
   - Sélectionnez : **Enabled** (Activé)
   - **Wallpaper Name** : Tapez le chemin UNC :
     ```
     \\SRV-FICHIERS\Ressources\Fonds-Ecran\logo-solaris.jpg
     ```
   - **Wallpaper Style** : Sélectionnez **Fill** (Remplir) ou **Fit** (Ajuster)
   - Cliquez **OK**

6. Fermez l'éditeur

#### Étape 4 : Lier la GPO

1. Clic droit sur votre OU (ex: "Utilisateurs") → **Link an Existing GPO**
2. Sélectionnez **GPO - Fond Écran Entreprise**
3. OK

#### Étape 5 : Tester

```cmd
gpupdate /force
```

Déconnectez-vous et reconnectez-vous.

✅ **Le fond d'écran de l'entreprise s'affiche !**

> ⚠️ **Note :** L'utilisateur ne pourra PAS changer le fond d'écran (c'est imposé par GPO). Si vous voulez qu'il puisse le modifier, il faut utiliser Preferences au lieu de Policies.

### Autres configurations Bureau utiles

**Dans le même dossier "Desktop", vous pouvez configurer :**

| Paramètre | Utilité |
|-----------|---------|
| **Screen saver timeout** | Activer l'économiseur d'écran après X minutes (sécurité) |
| **Password protect the screen saver** | Demander le mot de passe au retour (sécurité) |
| **Remove Recycle Bin icon** | Masquer la corbeille du bureau |
| **Hide all icons on desktop** | Bureau vide (environnement kiosque) |

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Créer une GPO de fond d'écran
> $GPO = New-GPO -Name "GPO - Fond Écran"
> Set-GPRegistryValue -Name "GPO - Fond Écran" `
>     -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" `
>     -ValueName "Wallpaper" `
>     -Type String `
>     -Value "\\SRV-FICHIERS\Ressources\logo.jpg"
> ```

---

## 🔷 Partie 5 : Sécurité avec les GPO

Les GPO permettent d'imposer des **règles de sécurité** uniformes.

### Cas d'usage 1 : Politique de mot de passe forte

**Contexte :** Vous voulez obliger tous les utilisateurs à avoir un mot de passe complexe (12 caractères minimum, majuscules, chiffres, caractères spéciaux).

#### Configuration

1. Ouvrez **GPMC**

2. Clic droit sur **Group Policy Objects** → **New**
3. Nom : `GPO - Politique Mots de Passe`

4. Clic droit sur la GPO → **Edit**

5. Navigation :
   - **Computer Configuration**
   - **Policies**
   - **Windows Settings**
   - **Security Settings**
   - **Account Policies** (Stratégies de compte)
   - **Password Policy** (Stratégie de mot de passe)

6. Configurez les paramètres suivants (double-clic sur chaque) :

| Paramètre | Valeur recommandée | Explication |
|-----------|-------------------|-------------|
| **Enforce password history** | 5 passwords remembered | Empêche de réutiliser les 5 derniers mots de passe |
| **Maximum password age** | 90 days | Changer le mot de passe tous les 90 jours |
| **Minimum password age** | 1 day | Empêche de changer le mot de passe immédiatement |
| **Minimum password length** | 12 characters | Minimum 12 caractères |
| **Password must meet complexity requirements** | **Enabled** | Oblige majuscules + minuscules + chiffres + symboles |

7. Fermez l'éditeur

8. **Liez** la GPO au **domaine** (pas à une OU) :
   - Clic droit sur **solaris.local** (le domaine)
   - **Link an Existing GPO**
   - Sélectionnez **GPO - Politique Mots de Passe**

✅ **Tous les utilisateurs du domaine doivent maintenant avoir un mot de passe fort !**

> ⚠️ **Important :** Cette GPO s'applique à TOUS les utilisateurs du domaine. Testez d'abord dans une OU de test !

### Cas d'usage 2 : Activer le pare-feu Windows

**Contexte :** Vous voulez vous assurer que le pare-feu Windows est activé sur tous les postes.

#### Configuration

1. Nouvelle GPO : `GPO - Pare-feu Windows`

2. Éditer :
   - **Computer Configuration**
   - **Policies**
   - **Windows Settings**
   - **Security Settings**
   - **Windows Defender Firewall with Advanced Security**
   - **Windows Defender Firewall with Advanced Security** (encore)
   - Clic droit → **Properties**

3. Onglets **Domain Profile**, **Private Profile**, **Public Profile** :
   - Pour chacun, définissez :
     - **Firewall state** : **On (recommended)**
     - **Inbound connections** : **Block (default)**
     - **Outbound connections** : **Allow (default)**

4. OK → Fermez

5. Liez à l'OU des ordinateurs

✅ **Le pare-feu est maintenant activé sur tous les postes !**

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Activer le pare-feu via GPO (nécessite des commandes avancées)
> # En pratique, utilisez l'interface pour la première fois
> # Puis vous pouvez exporter/importer la config :
>
> # Exporter la config d'une GPO
> Backup-GPO -Name "GPO - Pare-feu Windows" -Path "C:\GPO-Backups"
>
> # Importer dans une nouvelle GPO
> Import-GPO -BackupId <GUID> -TargetName "Nouvelle GPO" -Path "C:\GPO-Backups"
> ```

---

## 🔷 Partie 6 : Déploiement de scripts

Vous pouvez exécuter des **scripts PowerShell ou Batch** automatiquement au démarrage du PC ou à la connexion de l'utilisateur.

### Cas d'usage : Script de nettoyage au démarrage

**Scénario :** Vous voulez exécuter un script qui nettoie les fichiers temporaires à chaque démarrage.

#### Étape 1 : Créer le script

1. Créez un fichier `nettoyage-temp.bat` :
   ```batch
   @echo off
   REM Script de nettoyage des fichiers temporaires

   del /q /f /s %TEMP%\* 2>nul
   del /q /f /s C:\Windows\Temp\* 2>nul

   echo Nettoyage effectué le %date% %time% >> C:\Logs\nettoyage.log
   ```

2. Placez-le dans un partage réseau :
   ```
   \\SRV-FICHIERS\Scripts\nettoyage-temp.bat
   ```

> ⚠️ Donnez les permissions en **lecture + exécution** à "Domain Computers" (pour que les PC puissent l'exécuter).

#### Étape 2 : Créer la GPO

1. Nouvelle GPO : `GPO - Script Nettoyage Démarrage`

2. Éditer :
   - **Computer Configuration** (car c'est au démarrage du PC)
   - **Policies**
   - **Windows Settings**
   - **Scripts (Startup/Shutdown)** (Scripts Démarrage/Arrêt)
   - **Startup** (Démarrage)

3. Double-cliquez sur **Startup**

4. Une fenêtre **Startup Properties** s'ouvre
5. Cliquez sur **Add...** (Ajouter)

6. Remplissez :
   - **Script Name** : Tapez le chemin UNC :
     ```
     \\SRV-FICHIERS\Scripts\nettoyage-temp.bat
     ```
   - **Script Parameters** : (laissez vide)

7. OK → OK

8. Fermez l'éditeur

#### Étape 3 : Lier et tester

1. Liez la GPO à l'OU des ordinateurs

2. Sur un PC client :
   ```cmd
   gpupdate /force
   ```

3. **Redémarrez** le PC

✅ **Le script s'exécute automatiquement au démarrage !**

> 💡 **Pour les scripts PowerShell :**
> Utilisez le même principe, mais dans :
> - **Computer Configuration** → **Policies** → **Windows Settings** → **Scripts (Startup/Shutdown)**
> - Onglet **PowerShell Scripts** (au lieu de Scripts)

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Ajouter un script de démarrage à une GPO
> Set-GPStartupScript -Name "GPO - Script Nettoyage" `
>     -ScriptType "Batch" `
>     -Path "\\SRV-FICHIERS\Scripts\nettoyage-temp.bat"
> ```

---

## 🔷 Partie 7 : Diagnostic et dépannage GPO

### Problème N°1 : "Ma GPO ne s'applique pas !"

**Méthodologie de diagnostic (méthode de PRO) :**

#### Étape 1 : Vérifier que la GPO est bien liée

1. Ouvrez **GPMC**
2. Naviguez vers votre OU
3. Vérifiez que la GPO apparaît sous l'OU (avec l'icône de lien 🔗)

#### Étape 2 : Vérifier que l'ordinateur/utilisateur est bien dans l'OU

1. Ouvrez **Active Directory Users and Computers** (dsa.msc)
2. Vérifiez que le PC ou l'utilisateur est dans la bonne OU
3. Si ce n'est pas le cas, déplacez-le

#### Étape 3 : Vérifier l'ordre d'application

1. GPMC → Sélectionnez votre OU
2. Onglet **Group Policy Inheritance** (Héritage des stratégies de groupe)
3. Vérifiez que votre GPO apparaît dans la liste
4. L'ordre d'application est de haut en bas (Link Order 1 = priorité la plus haute)

#### Étape 4 : Forcer la mise à jour

**Sur le PC client :**

```cmd
gpupdate /force
```

**Attendez le message :** "Computer Policy update has completed successfully."

> Si la GPO configure des paramètres utilisateur, **déconnectez-vous et reconnectez-vous**.
> Si la GPO configure des paramètres ordinateur, **redémarrez le PC**.

#### Étape 5 : Vérifier quelles GPO s'appliquent réellement

**Sur le PC client :**

```cmd
gpresult /r
```

**Sortie :** Liste de toutes les GPO appliquées.

Cherchez votre GPO dans la section "Applied Group Policy Objects".

**Version plus détaillée (génère un rapport HTML) :**

```cmd
gpresult /h C:\rapport-gpo.html
```

Ouvrez `C:\rapport-gpo.html` dans un navigateur. Vous voyez TOUT : GPO appliquées, refusées, paramètres configurés, etc.

> 💡 **Astuce de pro :** Utilisez `gpresult /h` SYSTÉMATIQUEMENT quand vous dépannez. C'est l'outil N°1 du diagnostic GPO.

#### Étape 6 : Vérifier les permissions

Une GPO ne s'applique QUE si l'utilisateur/ordinateur a les permissions "Read" ET "Apply Group Policy".

1. GPMC → Sélectionnez votre GPO
2. Onglet **Delegation** (Délégation)
3. Vérifiez que "Authenticated Users" (ou un groupe spécifique) a les permissions :
   - ✅ Read
   - ✅ Apply Group Policy

Si manquant :
4. Cliquez **Advanced**
5. **Add** → Tapez `Authenticated Users` → OK
6. Cochez **Read** et **Apply Group Policy**
7. OK

### Problème N°2 : "Ma GPO s'applique, mais le paramètre ne change pas"

**Causes possibles :**

1. **Conflit avec une autre GPO** : Une GPO de priorité supérieure configure le même paramètre différemment
   - Solution : Utilisez "Enforced" ou réorganisez l'ordre des GPO

2. **Paramètre configuré en local** : L'utilisateur a modifié le paramètre localement
   - Solution : Utilisez "Policies" au lieu de "Preferences" (les policies sont imposées)

3. **Cache GPO corrompu** :
   - Solution : Supprimez le cache :
     ```cmd
     rd /s /q %windir%\System32\GroupPolicy
     gpupdate /force
     ```

### Problème N°3 : "Ma GPO met trop de temps à s'appliquer"

**Par défaut, les GPO se mettent à jour :**
- Toutes les **90 minutes** (avec un décalage aléatoire de 0-30 minutes)
- Au **démarrage** de l'ordinateur
- À la **connexion** de l'utilisateur

**Pour forcer immédiatement :**
```cmd
gpupdate /force
```

**Pour réduire l'intervalle de mise à jour (déconseillé, sauf pour les tests) :**

1. Créez une GPO
2. Computer Configuration → Policies → Administrative Templates → System → Group Policy
3. **Set Group Policy refresh interval for computers** : Définissez (ex: 30 minutes)

### Commandes de diagnostic essentielles

| Commande | Utilité |
|----------|---------|
| `gpupdate /force` | Force l'application immédiate des GPO |
| `gpresult /r` | Affiche les GPO appliquées (résumé) |
| `gpresult /h rapport.html` | Génère un rapport HTML complet |
| `gpresult /scope user /v` | Détails des GPO utilisateur |
| `gpresult /scope computer /v` | Détails des GPO ordinateur |
| `rsop.msc` | Interface graphique pour voir les GPO (Resultant Set of Policy) |

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Forcer la mise à jour sur un PC distant
> Invoke-GPUpdate -Computer "PC-CLIENT-01" -Force -RandomDelayInMinutes 0
>
> # Obtenir les GPO appliquées sur un PC distant
> Get-GPResultantSetOfPolicy -Computer "PC-CLIENT-01" -ReportType Html -Path "C:\rapport.html"
> ```

---

## 💼 Cas d'usage réels en entreprise

Voici les GPO que j'ai créées des centaines de fois en 20 ans (et que vous créerez aussi) :

### 1. Mappage de lecteurs réseau (90% des demandes)
✅ Déjà vu plus haut

### 2. Installation automatique d'imprimantes réseau

**Navigation :**
- User Configuration → Preferences → Control Panel Settings → **Printers**

**Configuration :**
- New → Shared Printer
- Path : `\\SRV-PRINT\HP-Compta-RDC`
- Action : Create
- Set as default : Coché (si c'est l'imprimante par défaut)

### 3. Déploiement de logiciels (MSI)

**Navigation :**
- Computer Configuration → Policies → Software Settings → **Software installation**

**Configuration :**
- Clic droit → New → Package
- Sélectionnez le fichier .msi (dans un partage réseau)
- Assignment type : "Assigned" (attribué)
- Le logiciel s'installe au prochain démarrage

**Exemple :** Déployer Google Chrome, 7-Zip, Adobe Reader, etc.

### 4. Redirection des dossiers (Documents, Bureau, etc.)

**Utilité :** Enregistrer les documents sur le serveur au lieu du PC local (sauvegarde automatique, accessible de partout)

**Navigation :**
- User Configuration → Policies → Windows Settings → **Folder Redirection**

**Configuration :**
- Clic droit sur "Documents" → Properties
- Setting : Basic
- Target folder location : `\\SRV-FICHIERS\Users\%username%\Documents`
- OK

✅ Les documents de l'utilisateur sont maintenant enregistrés sur le serveur !

### 5. Désactiver le panneau de configuration

**Utilité :** Empêcher les utilisateurs de modifier les paramètres système (environnement sécurisé)

**Navigation :**
- User Configuration → Policies → Administrative Templates → Control Panel
- **Prohibit access to Control Panel and PC settings** : Enabled

### 6. Verrouillage automatique après X minutes

**Utilité :** Sécurité (verrouiller la session si l'utilisateur s'absente)

**Navigation :**
- User Configuration → Policies → Administrative Templates → Control Panel → Personalization
- **Enable screen saver** : Enabled
- **Screen saver timeout** : 600 seconds (10 minutes)
- **Password protect the screen saver** : Enabled

### 7. Désactiver l'USB (sécurité)

**Utilité :** Empêcher les utilisateurs de brancher des clés USB (prévenir les fuites de données / virus)

**Navigation :**
- Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access
- **All Removable Storage classes: Deny all access** : Enabled

⚠️ À utiliser avec précaution !

---

## ⚠️ Pièges à éviter

### 1. Lier une GPO au mauvais endroit

**Erreur classique :**
- Vous créez une GPO pour les utilisateurs
- Vous la liez à l'OU des **ordinateurs** au lieu de l'OU des **utilisateurs**
- Résultat : ça ne marche pas !

**Règle :**
- GPO avec "User Configuration" → Lier à une OU d'**utilisateurs**
- GPO avec "Computer Configuration" → Lier à une OU d'**ordinateurs**

### 2. Tester sans faire gpupdate /force

Les GPO ne s'appliquent pas instantanément. Il faut :
- Soit faire `gpupdate /force`
- Soit redémarrer le PC (pour Computer Config)
- Soit se déconnecter/reconnecter (pour User Config)

### 3. Oublier les permissions sur les partages réseau

Si vous mappez un lecteur vers `\\SRV\Partage`, vérifiez que :
- Les utilisateurs ont les permissions NTFS ET SMB
- Le serveur est joignable
- Le nom DNS fonctionne

### 4. Conflits entre GPO

Si 2 GPO configurent le même paramètre différemment :
- C'est la **dernière appliquée** qui gagne (ordre LSDOU)
- Sauf si une GPO est en mode "Enforced" (elle a toujours la priorité)

**Solution :** Organisez bien vos GPO, utilisez des noms clairs, et documentez !

### 5. Ne pas tester avant de déployer

**Règle d'or :**
1. Créez une OU de test
2. Testez la GPO sur 1-2 postes/utilisateurs de test
3. Vérifiez que ça marche
4. Seulement APRÈS, déployez en production

> 💡 En 20 ans, j'ai vu des GPO mal testées **bloquer toute l'entreprise** (exemple : GPO qui désactive le réseau par erreur). Testez TOUJOURS !

---

## 🎯 Exercices pratiques (pour l'examen)

### Exercice 1 : Mappage de lecteurs (15 min) - Niveau examen

**Contexte :**
Entreprise **InnoTech**, 50 employés. Vous devez mapper les lecteurs suivants :

- **P:** → `\\SRV-FILES\Public` (pour tous)
- **S:** → `\\SRV-FILES\Services\%departement%` (par service)
- **H:** → `\\SRV-FILES\Users\%username%` (personnel)

**Consignes :**
1. Créez la GPO "GPO - Mappage Lecteurs InnoTech"
2. Configurez les 3 mappages
3. Liez la GPO à l'OU "Utilisateurs"
4. Testez sur un poste client

<details>
<summary>Cliquez pour voir la solution</summary>

**Solution :**

1. **GPMC** → Clic droit **Group Policy Objects** → New
   - Nom : `GPO - Mappage Lecteurs InnoTech`

2. Clic droit sur la GPO → **Edit**

3. Navigation :
   - User Configuration → Preferences → Windows Settings → **Drive Maps**

4. **Lecteur P:** (Public)
   - Clic droit → New → Mapped Drive
   - Location : `\\SRV-FILES\Public`
   - Drive Letter : **P:**
   - Reconnect : ✅
   - Label : `Dossier Public`
   - OK

5. **Lecteur S:** (Service)
   - New → Mapped Drive
   - Location : `\\SRV-FILES\Services\%departement%`
   - Drive Letter : **S:**
   - Reconnect : ✅
   - Label : `Mon Service`
   - OK

   > Note : La variable `%departement%` doit être un attribut AD rempli pour chaque utilisateur.

6. **Lecteur H:** (Personnel)
   - New → Mapped Drive
   - Location : `\\SRV-FILES\Users\%username%`
   - Drive Letter : **H:**
   - Reconnect : ✅
   - Label : `Mes Documents`
   - OK

7. Fermez l'éditeur

8. Dans GPMC, clic droit sur l'OU "Utilisateurs" → **Link an Existing GPO**
   - Sélectionnez : `GPO - Mappage Lecteurs InnoTech`
   - OK

9. **Test :**
   ```cmd
   gpupdate /force
   ```
   Déconnectez-vous et reconnectez-vous.

   Vérifiez dans l'Explorateur : P:, S:, H: sont présents.

</details>

---

### Exercice 2 : Fond d'écran + Économiseur d'écran (10 min)

**Contexte :**
Déployez le fond d'écran de l'entreprise (`\\SRV-FILES\Ressources\wallpaper.jpg`) ET activez l'économiseur d'écran après 10 minutes avec mot de passe.

**Consignes :**
1. Créez la GPO "GPO - Bureau Standard"
2. Configurez le fond d'écran
3. Configurez l'économiseur d'écran (10 min, avec mot de passe)
4. Liez et testez

<details>
<summary>Cliquez pour voir la solution</summary>

**Solution :**

1. Nouvelle GPO : `GPO - Bureau Standard`

2. Edit :
   - **User Configuration** → Policies → Administrative Templates → Desktop → Desktop

3. **Desktop Wallpaper** :
   - Double-clic → **Enabled**
   - Wallpaper Name : `\\SRV-FILES\Ressources\wallpaper.jpg`
   - Wallpaper Style : **Fill**
   - OK

4. Retour dans Desktop → Allez dans **Control Panel → Personalization**

5. **Enable screen saver** :
   - **Enabled**
   - OK

6. **Screen saver timeout** :
   - **Enabled**
   - Seconds : `600` (10 minutes)
   - OK

7. **Password protect the screen saver** :
   - **Enabled**
   - OK

8. Fermez, liez à l'OU, testez.

</details>

---

### Exercice 3 : Diagnostic de panne (15 min) - Niveau examen

**Contexte :**
Un utilisateur vous appelle : "Je ne vois pas le lecteur P: alors que mes collègues l'ont !"

**Informations :**
- Utilisateur : Marie Martin (mmartin)
- PC : PC-COMPTA-05
- OU attendue : Comptabilité
- GPO concernée : "GPO - Mappage Lecteurs"

**Consignes :**
1. Diagnostiquez le problème (sans solutions toutes faites !)
2. Trouvez la cause
3. Proposez une solution

<details>
<summary>Cliquez pour voir la méthodologie de diagnostic</summary>

**Méthodologie :**

**Étape 1 : Vérifier que l'utilisateur est dans la bonne OU**

```cmd
# Sur le poste de l'utilisateur
whoami /fqdn
```

Résultat attendu : `SOLARIS\mmartin`

Vérifiez dans **Active Directory Users and Computers** :
- L'utilisateur mmartin est-il dans l'OU "Comptabilité" ?
- Ou est-il ailleurs (ex: dans "Users" par défaut) ?

**Si dans la mauvaise OU :**
→ **Solution :** Déplacez l'utilisateur dans l'OU "Comptabilité"

---

**Étape 2 : Vérifier que la GPO est liée à l'OU**

1. GPMC → OU "Comptabilité"
2. Vérifiez que "GPO - Mappage Lecteurs" apparaît sous l'OU

**Si la GPO n'est pas liée :**
→ **Solution :** Liez la GPO à l'OU

---

**Étape 3 : Vérifier que la GPO s'applique réellement**

Sur le PC de l'utilisateur :

```cmd
gpresult /r
```

Cherchez dans la section "Applied Group Policy Objects" : est-ce que "GPO - Mappage Lecteurs" apparaît ?

**Si la GPO n'apparaît pas :**
→ Vérifiez les permissions (étape 4)

---

**Étape 4 : Vérifier les permissions**

1. GPMC → Sélectionnez "GPO - Mappage Lecteurs"
2. Onglet **Delegation**
3. Vérifiez que "Authenticated Users" a les permissions :
   - ✅ Read
   - ✅ Apply Group Policy

**Si les permissions manquent :**
→ **Solution :** Ajoutez les permissions

---

**Étape 5 : Forcer la mise à jour**

```cmd
gpupdate /force
```

Déconnectez-vous et reconnectez-vous.

Vérifiez si le lecteur P: apparaît maintenant.

---

**Étape 6 : Vérifier la configuration de la GPO**

Si tout le reste est OK mais le lecteur n'apparaît toujours pas :

1. GPMC → Edit la GPO
2. User Configuration → Preferences → Windows Settings → Drive Maps
3. Vérifiez que le mappage P: existe et est correctement configuré
4. Vérifiez le chemin UNC : `\\SRV-FICHIERS\Public`
5. Testez manuellement le chemin dans l'Explorateur

**Si le chemin UNC ne fonctionne pas :**
→ **Problème réseau ou permissions sur le partage** (pas lié à la GPO)

---

**Causes les plus fréquentes (par ordre) :**
1. 🥇 Utilisateur dans la mauvaise OU (60%)
2. 🥈 GPO pas mise à jour (20%) → solution : gpupdate /force
3. 🥉 Permissions manquantes sur la GPO (10%)
4. Chemin UNC incorrect ou partage inaccessible (10%)

</details>

---

## ✅ Checklist pour l'examen

Avant de passer au module suivant, assurez-vous de maîtriser :

### Concepts
- [ ] Expliquer ce qu'est une GPO et à quoi elle sert
- [ ] Différencier "Computer Configuration" et "User Configuration"
- [ ] Expliquer l'ordre d'application LSDOU
- [ ] Comprendre l'héritage et le blocage
- [ ] Savoir quand utiliser "Enforced"

### Manipulation (interface graphique)
- [ ] Ouvrir la console GPMC (gpmc.msc)
- [ ] Créer une nouvelle GPO
- [ ] Éditer une GPO
- [ ] Lier une GPO à une OU
- [ ] Délier une GPO

### Cas d'usage pratiques
- [ ] Mapper un lecteur réseau (avec chemin fixe)
- [ ] Mapper un lecteur personnel (avec %username%)
- [ ] Configurer un fond d'écran
- [ ] Activer l'économiseur d'écran avec mot de passe
- [ ] Configurer une politique de mot de passe
- [ ] Déployer un script de démarrage

### Diagnostic
- [ ] Utiliser `gpupdate /force` pour forcer la mise à jour
- [ ] Utiliser `gpresult /r` pour voir les GPO appliquées
- [ ] Générer un rapport HTML avec `gpresult /h`
- [ ] Identifier pourquoi une GPO ne s'applique pas
- [ ] Vérifier les permissions sur une GPO
- [ ] Vérifier que l'utilisateur/PC est dans la bonne OU

### Bonus PowerShell (si vous voulez aller plus loin)
- [ ] Créer une GPO avec `New-GPO`
- [ ] Lier une GPO avec `New-GPLink`
- [ ] Forcer la mise à jour à distance avec `Invoke-GPUpdate`

---

## 📚 Ressources complémentaires

### Documentation officielle Microsoft
- [Group Policy Overview](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831791(v=ws.11))
- [Group Policy Settings Reference](https://www.microsoft.com/en-us/download/details.aspx?id=25250)

### Outils utiles
- **GPMC** (gpmc.msc) : Console de gestion des GPO
- **RSOP** (rsop.msc) : Voir les GPO appliquées (GUI)
- **gpresult** : Diagnostic GPO (CLI)

### Commandes essentielles (mémo)

```cmd
REM Forcer la mise à jour des GPO
gpupdate /force

REM Voir les GPO appliquées (résumé)
gpresult /r

REM Générer un rapport HTML complet
gpresult /h C:\rapport-gpo.html

REM Voir les GPO appliquées (interface graphique)
rsop.msc
```

> 💡 **Astuce de pro (PowerShell) - Mémo :**
> ```powershell
> # Créer une GPO
> New-GPO -Name "Ma GPO"
>
> # Lier une GPO à une OU
> New-GPLink -Name "Ma GPO" -Target "OU=Utilisateurs,DC=solaris,DC=local"
>
> # Forcer la mise à jour sur un PC distant
> Invoke-GPUpdate -Computer "PC-CLIENT-01" -Force
>
> # Obtenir les GPO liées à une OU
> Get-GPInheritance -Target "OU=Comptabilité,DC=solaris,DC=local"
>
> # Sauvegarder une GPO
> Backup-GPO -Name "GPO - Mappage Lecteurs" -Path "C:\GPO-Backups"
>
> # Restaurer une GPO
> Restore-GPO -Name "GPO - Mappage Lecteurs" -Path "C:\GPO-Backups"
> ```

---

## 📝 Message final de votre formateur

> **Félicitations ! Vous venez de maîtriser LES GPO - la compétence N°1 du TSSR !** 🎉
>
> **En 20 ans, j'ai vu la différence entre :**
> - Les techniciens qui configurent tout manuellement (épuisés, inefficaces, stressés)
> - Les techniciens qui automatisent avec les GPO (sereins, efficaces, valorisés)
>
> **Vous faites maintenant partie de la 2ème catégorie.** 💪
>
> **À l'examen :**
> - Il y aura quasi-certainement un exercice GPO (95% de chances)
> - Généralement : mappage de lecteurs ou fond d'écran
> - Temps accordé : 15-20 minutes
> - **Si vous savez faire ça, vous avez 20-30% des points de l'examen !**
>
> **En entreprise :**
> - Les GPO vous feront économiser **80% de votre temps** sur les tâches répétitives
> - Les recruteurs ADORENT les candidats qui maîtrisent les GPO
> - C'est ce qui différencie un technicien junior d'un technicien confirmé
>
> **Mes conseils pour progresser :**
> 1. **Pratiquez !** Créez 10 GPO différentes sur votre lab
> 2. **Cassez tout !** Testez ce qui se passe si vous configurez mal une GPO
> 3. **Documentez !** Notez vos GPO dans un fichier Excel (nom, OU liée, fonction)
> 4. **Automatisez !** Une fois à l'aise avec l'interface, passez à PowerShell
>
> **Prochaine étape :** Pratiquez le mappage de lecteurs (c'est CE que vous ferez le plus en entreprise).
>
> **Vous êtes maintenant un expert GPO. Foncez !** 🚀

---

<div align="center">

**Cours suivant :** [DNS et DHCP](./dns-dhcp-windows-server.md) | [Windows Client 10/11](./windows-client-10-11.md)

[⬅️ Retour au sommaire](../README.md) | [📊 Progression](../progression.md)

---

**💡 Une question sur les GPO ? Relisez la section "Diagnostic" - 90% des solutions y sont !**

**🎯 Exercice pour vous entraîner :** Créez une GPO qui mappe 3 lecteurs différents avec des variables (%username%, %computername%, etc.)

</div>
