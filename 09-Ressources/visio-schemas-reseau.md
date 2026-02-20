# Microsoft Visio - Schémas Réseau et Documentation Technique

> 📚 **Module :** Outils de documentation
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 4-5 heures (pratique)
> 🎯 **Niveau :** Fondamental (important pour la documentation)
> 🎓 **Formateur virtuel :** Architecte réseau avec +20 ans d'expérience
> 🎨 **Focus :** Créer des schémas réseau professionnels

---

## 👨‍🏫 Message de votre formateur

> **"Un schéma vaut mieux que 1000 mots."**
>
> En 20 ans, j'ai vu des infrastructures IT **non documentées** tomber en panne, et personne ne savait comment ça fonctionnait. Résultat : Heures perdues, stress, risque d'erreur.
>
> **La documentation technique (schémas réseau) est ESSENTIELLE.**
>
> **En entreprise, vous allez créer des schémas pour :**
> - ✅ **Documenter l'infrastructure existante** (topologie réseau)
> - ✅ **Proposer de nouvelles architectures** (avant projet)
> - ✅ **Former les nouveaux arrivants** (plus facile à comprendre qu'un long texte)
> - ✅ **Diagnostiquer les pannes** (avoir une vue d'ensemble)
> - ✅ **Présenter aux clients/managers** (communication visuelle)
>
> **À l'examen TSSR :**
> - Il y a environ 30% de chances qu'on vous demande de **créer un schéma réseau**
> - Ou de **compléter un schéma existant**
> - Temps accordé : 15-20 minutes
> - Si vous maîtrisez Visio, c'est un exercice **facile et rapide** (points gratuits !)
>
> **Les recruteurs ADORENT les candidats qui documentent.**
> Pourquoi ? Parce qu'un technicien qui documente = moins de questions aux collègues = gain de temps pour toute l'équipe.
>
> **Ce cours est 80% pratique. Ouvrez Visio et suivez les exercices !**

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Partie 1 : Découvrir Microsoft Visio](#-partie-1--découvrir-microsoft-visio)
- [Partie 2 : Les formes réseau essentielles](#-partie-2--les-formes-réseau-essentielles)
- [Partie 3 : Créer votre premier schéma réseau](#-partie-3--créer-votre-premier-schéma-réseau)
- [Partie 4 : Schémas réseau avancés](#-partie-4--schémas-réseau-avancés)
- [Partie 5 : Bonnes pratiques](#-partie-5--bonnes-pratiques-de-documentation)
- [Exemples de schémas types](#-exemples-de-schémas-types)
- [Alternatives gratuites à Visio](#-alternatives-gratuites-à-visio)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Ouvrir** et naviguer dans Microsoft Visio
- ✅ **Utiliser** les formes réseau (serveur, switch, routeur, PC, etc.)
- ✅ **Créer** un schéma réseau simple (topologie LAN)
- ✅ **Créer** un schéma réseau complexe (multi-sites, VPN, VLAN)
- ✅ **Appliquer** les bonnes pratiques de documentation
- ✅ **Exporter** un schéma en PDF ou PNG
- ✅ **Utiliser** des alternatives gratuites (Draw.io, Lucidchart)
- ✅ **Réussir** l'exercice de schéma réseau à l'examen

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [x] Avoir des notions de réseau (IP, routeur, switch, etc.)
- [x] Avoir suivi les cours TCP/IP et Active Directory
- [ ] **Microsoft Visio** installé (ou utiliser une alternative gratuite)
  - Visio fait partie de Microsoft Office (version Pro ou Entreprise)
  - Si vous n'avez pas Visio : Utilisez **Draw.io** (gratuit, en ligne)

**Matériel nécessaire :**
- 💻 PC Windows ou Mac
- 🖱️ Souris (recommandé pour dessiner)
- 🎨 Microsoft Visio (ou Draw.io gratuit)
- 📝 Exemples de réseaux à schématiser (fournis dans le cours)

---

## 🔷 Partie 1 : Découvrir Microsoft Visio

### Qu'est-ce que Microsoft Visio ?

**Microsoft Visio** est un logiciel de **dessin vectoriel** spécialisé dans la création de **diagrammes professionnels** :
- Schémas réseau / topologies
- Organigrammes
- Diagrammes de flux (flowcharts)
- Plans d'architecture
- Chronologies de projet

**Pourquoi Visio et pas PowerPoint ou Word ?**
- Visio a des **formes réseau préconçues** (serveur, routeur, switch, etc.)
- Les connexions restent liées aux objets (si vous déplacez un routeur, les câbles suivent)
- Export professionnel (PDF, PNG, SVG)
- Reconnu universellement en entreprise

### Ouvrir Visio et créer un schéma réseau

#### Étape 1 : Lancer Visio

1. Ouvrez **Microsoft Visio**
2. Écran d'accueil : Vous voyez des modèles (templates)

#### Étape 2 : Choisir le bon modèle

1. Dans la barre de recherche, tapez : **"Network"** ou **"Réseau"**
2. Sélectionnez : **"Basic Network Diagram"** (Diagramme réseau de base)
3. Cliquez sur **Create** (Créer)

✅ **Visio ouvre un nouveau document avec les formes réseau sur la gauche.**

### Interface de Visio (repères visuels)

```
┌────────────────────────────────────────────────────────────┐
│  Fichier  Accueil  Insertion  Création  Données  Révision  │  ← Ruban (comme Word/Excel)
├──────────┬─────────────────────────────────────────────────┤
│          │                                                  │
│  Formes  │                                                  │
│  ───────  │                  ZONE DE DESSIN                 │
│  □ PC    │             (Glissez-déposez ici)               │
│  ⬓ Server │                                                  │
│  ▭ Switch │                                                  │
│  ◊ Router │                                                  │
│  ─ Cable  │                                                  │
│          │                                                  │
│          │                                                  │
│          │                                                  │
└──────────┴─────────────────────────────────────────────────┘
```

**Les 3 zones importantes :**
1. **Ruban** (en haut) : Outils de dessin, alignement, texte
2. **Volet Formes** (à gauche) : Bibliothèque d'objets réseau
3. **Zone de dessin** (au centre) : Où vous créez votre schéma

---

## 🔷 Partie 2 : Les formes réseau essentielles

### Catégories de formes

Dans le volet **Formes** (à gauche), vous avez plusieurs catégories :

#### **Network and Peripherals** (Réseau et périphériques)
Les formes les plus utilisées :

| Forme | Nom | Utilisation |
|-------|-----|-------------|
| 💻 | **PC / Workstation** | Poste de travail utilisateur |
| 🖥️ | **Server** | Serveur (fichiers, AD, DNS, etc.) |
| 📱 | **Laptop** | Ordinateur portable |
| 🖨️ | **Printer** | Imprimante réseau |
| 🌐 | **Router** | Routeur (connexion internet, WAN) |
| 🔀 | **Switch** | Commutateur réseau (LAN) |
| 🔥 | **Firewall** | Pare-feu |
| ☁️ | **Cloud** | Services cloud (internet, Azure, AWS) |
| 📡 | **Wireless Access Point** | Point d'accès WiFi |
| 💾 | **NAS / Storage** | Serveur de stockage |

#### **Cables** (Câbles)
- **Ethernet Cable** : Câble réseau classique
- **Wireless Link** : Connexion sans fil (WiFi)

### Comment utiliser une forme ?

**Méthode 1 : Glisser-déposer**
1. Dans le volet **Formes**, trouvez la forme souhaitée (ex: PC)
2. **Cliquez et maintenez** la souris
3. **Glissez** sur la zone de dessin
4. **Relâchez**

**Méthode 2 : Double-clic**
1. **Double-cliquez** sur la forme dans le volet
2. Elle apparaît au centre de la zone de dessin

### Redimensionner et déplacer

**Redimensionner :**
- Cliquez sur la forme
- Des **poignées** (petits carrés) apparaissent
- Tirez une poignée pour agrandir/rétrécir

**Déplacer :**
- Cliquez sur la forme
- Maintenez et **glissez** vers la nouvelle position

**Supprimer :**
- Cliquez sur la forme
- Appuyez sur **Suppr** (ou **Delete**)

---

## 🔷 Partie 3 : Créer votre premier schéma réseau

### Exercice guidé : Réseau simple d'une PME

**Contexte :**
PME "TechnoSolaris" avec :
- 1 routeur (connexion internet)
- 1 switch
- 1 serveur (fichiers + AD)
- 5 PC utilisateurs
- 1 imprimante réseau

**Objectif :** Créer le schéma de ce réseau.

#### Étape 1 : Placer le routeur (connexion internet)

1. Dans **Formes** → **Network and Peripherals**, trouvez **Router**
2. **Glissez-déposez** le routeur **en haut au centre** de la zone de dessin
3. Double-cliquez sur le texte sous le routeur
4. Tapez : **Routeur Internet**
5. Cliquez en dehors pour valider

#### Étape 2 : Placer le switch

1. Trouvez **Switch** dans les formes
2. Glissez le switch **sous le routeur** (environ 5 cm en dessous)
3. Renommez-le : **Switch Principal**

#### Étape 3 : Placer le serveur

1. Trouvez **Server**
2. Placez-le **à gauche du switch** (même hauteur)
3. Renommez : **SRV-FILES** et en dessous ajoutez **(192.168.1.10)**

> 💡 **Astuce :** Toujours indiquer l'IP des équipements clés (serveurs, routeur, switch si IP de gestion)

#### Étape 4 : Placer les 5 PC

1. Trouvez **PC / Workstation**
2. Placez 5 PC **en bas**, espacés régulièrement
3. Renommez-les :
   - PC-COMPTA-01
   - PC-COMPTA-02
   - PC-DIR-01
   - PC-ADMIN-01
   - PC-ADMIN-02

#### Étape 5 : Placer l'imprimante

1. Trouvez **Printer**
2. Placez l'imprimante **à droite du switch** (même hauteur que le serveur)
3. Renommez : **Imprimante RDC** (192.168.1.50)

#### Étape 6 : Connecter les équipements (câbles)

Maintenant, on va relier tout ça avec des câbles.

**Connecter le routeur au switch :**
1. Dans **Formes** → **Cables**, trouvez **Ethernet Cable** (ou utilisez l'outil Connecteur)
2. **Méthode automatique (recommandé) :**
   - Cliquez sur l'outil **Connecteur** dans le ruban (icône de ligne)
   - Cliquez sur le **routeur** (un point de connexion apparaît)
   - Cliquez sur le **switch**
   - Une ligne se crée automatiquement !

3. Répétez pour connecter :
   - Switch → Serveur
   - Switch → PC-COMPTA-01
   - Switch → PC-COMPTA-02
   - Switch → PC-DIR-01
   - Switch → PC-ADMIN-01
   - Switch → PC-ADMIN-02
   - Switch → Imprimante

#### Étape 7 : Ajouter des annotations

1. Dans le ruban, cliquez sur **Insertion** → **Zone de texte**
2. Dessinez une zone en haut à gauche
3. Tapez :
   ```
   Réseau TechnoSolaris
   Réseau : 192.168.1.0/24
   Passerelle : 192.168.1.1
   DNS : 192.168.1.10
   ```

4. Changez la police : **Gras, taille 12**

✅ **Votre premier schéma réseau est terminé !**

#### Étape 8 : Exporter en PDF

1. **Fichier** → **Exporter** → **Créer un PDF/XPS**
2. Choisissez l'emplacement
3. Nommez : `Schema-Reseau-TechnoSolaris.pdf`
4. Cliquez sur **Publier**

---

## 🔷 Partie 4 : Schémas réseau avancés

### Schéma avec VLANs

**Contexte :**
Vous devez séparer le réseau en 3 VLANs :
- VLAN 10 : Comptabilité (192.168.10.0/24)
- VLAN 20 : Direction (192.168.20.0/24)
- VLAN 30 : Administration (192.168.30.0/24)

**Astuce pour représenter les VLANs :**

1. **Utilisez des rectangles de couleur pour grouper les équipements**
   - Insertion → **Formes** → **Rectangle**
   - Dessinez un rectangle autour des PC de la compta
   - Clic droit → **Format de la forme** → **Remplissage** : Bleu clair, **Transparence** : 50%
   - Ajoutez un texte : **VLAN 10 - Comptabilité**

2. Répétez pour VLAN 20 (vert clair) et VLAN 30 (orange clair)

### Schéma multi-sites avec VPN

**Contexte :**
Entreprise avec 2 sites :
- Site Paris (siège)
- Site Lyon (agence)
- Connectés par VPN

**Comment représenter :**

```
[Site Paris]                    [Internet]                    [Site Lyon]
┌──────────────┐                   ┌─┐                    ┌──────────────┐
│  Routeur VPN │◄─────VPN─────────►│ │◄─────VPN─────────►│  Routeur VPN │
└──────┬───────┘                   └─┘                    └──────┬───────┘
       │                                                          │
   [Switch]                                                   [Switch]
       │                                                          │
  [Serveurs]                                                 [Serveurs]
       │                                                          │
     [PCs]                                                      [PCs]
```

**Dans Visio :**
1. Créez 2 zones distinctes (Site Paris à gauche, Site Lyon à droite)
2. Placez une **Cloud** au centre (internet)
3. Connectez les routeurs à la cloud avec des lignes
4. **Annotez la ligne VPN** :
   - Double-clic sur la ligne → Ajoutez du texte : "VPN IPsec"
   - Changez le style de ligne : Pointillés (pour indiquer que c'est virtuel)

### Schéma avec DMZ (zone démilitarisée)

**Contexte :**
Réseau avec serveurs publics (web, email) séparés du réseau interne.

**Structure :**
```
Internet
   │
[Firewall externe]
   │
   ├───► [DMZ] : Serveur Web (accès public)
   │
[Firewall interne]
   │
[Réseau interne] : Serveurs AD, fichiers (accès privé)
```

**Représentation dans Visio :**
- Utilisez 2 **Firewalls**
- Zone DMZ : Rectangle orange
- Réseau interne : Rectangle vert

---

## 🔷 Partie 5 : Bonnes pratiques de documentation

### Règle #1 : Utilisez une légende

**Toujours inclure une légende** expliquant :
- Les couleurs utilisées (VLAN 10 = bleu, VLAN 20 = vert)
- Les types de connexion (ligne pleine = Ethernet, pointillés = VPN)
- Les symboles personnalisés

**Exemple de légende :**
```
Légende :
━━━━━  Ethernet 1 Gbps
┄┄┄┄┄  VPN IPsec
🔴 Serveur critique
🟢 Serveur secondaire
```

### Règle #2 : Indiquez les adresses IP

**Sur les équipements clés, notez toujours :**
- Adresse IP
- Masque de sous-réseau (si pertinent)
- Nom d'hôte

**Exemple :**
```
SRV-AD-01
192.168.1.10 /24
DC1.solaris.local
```

### Règle #3 : Organisez le schéma logiquement

**Hiérarchie du haut vers le bas :**
```
Internet / WAN
   ↓
Routeur
   ↓
Firewall
   ↓
Switch core
   ↓
Switches d'accès
   ↓
Équipements terminaux (PC, imprimantes)
```

### Règle #4 : Utilisez des couleurs avec modération

**Bon usage des couleurs :**
- ✅ Différencier les VLANs
- ✅ Distinguer les zones (DMZ, LAN, WAN)
- ✅ Mettre en évidence les équipements critiques

**Mauvais usage :**
- ❌ Arc-en-ciel (trop de couleurs = confus)
- ❌ Couleurs flashy (jaune fluo, rose vif)

### Règle #5 : Ajoutez des métadonnées

**En bas du schéma, ajoutez toujours :**
- Titre du schéma
- Date de création
- Date de dernière mise à jour
- Auteur (votre nom)
- Version (v1.0, v1.1, etc.)

**Exemple :**
```
──────────────────────────────────────────────
Schéma Réseau - TechnoSolaris
Créé le : 12/01/2026
Mis à jour le : 12/01/2026
Auteur : Votre Nom
Version : 1.0
──────────────────────────────────────────────
```

### Règle #6 : Exportez en plusieurs formats

**Toujours exporter en :**
- **PDF** : Pour impression et consultation (universel)
- **PNG** : Pour insérer dans un document Word/PowerPoint
- **VSD / VSDX** : Format Visio natif (pour modifications futures)

---

## 📋 Exemples de schémas types

### 1. Topologie étoile (Star Topology)

**Utilisation :** Réseau d'entreprise standard.

```
           [Switch]
         /  |  |  \
        /   |  |   \
      PC1  PC2 PC3  PC4
```

**Avantages :**
- Si un PC tombe, les autres continuent
- Facile à dépanner

### 2. Topologie bus (Bus Topology)

**Utilisation :** Vieux réseaux (années 1990).

```
PC1───PC2───PC3───PC4 (tous sur le même câble)
```

**Inconvénients :**
- Si le câble principal casse, tout le réseau tombe
- Obsolète aujourd'hui

### 3. Topologie anneau (Ring Topology)

**Utilisation :** Réseaux redondants (FDDI, Token Ring).

```
    PC1
   /   \
 PC4   PC2
   \   /
    PC3
```

**Avantages :**
- Redondance (2 chemins possibles)

### 4. Architecture 3-tiers (Datacenter)

```
Niveau 1 : Core (Routeur principal)
Niveau 2 : Distribution (Switches agrégation)
Niveau 3 : Access (Switches d'accès)
```

---

## 🌐 Alternatives gratuites à Visio

Si vous n'avez pas Microsoft Visio, voici les meilleures alternatives **gratuites** :

### 1. **Draw.io** (diagrams.net) ⭐ Recommandé

**Avantages :**
- ✅ **100% gratuit**
- ✅ Fonctionne en ligne (pas d'installation)
- ✅ Bibliothèque réseau complète
- ✅ Export PDF, PNG, SVG
- ✅ Interface similaire à Visio

**Utilisation :**
1. Aller sur : https://app.diagrams.net/
2. Cliquez sur "Create New Diagram"
3. Choisissez "Network" dans les templates
4. Dessinez votre schéma
5. Fichier → Export as → PDF

### 2. **Lucidchart**

**Avantages :**
- ✅ Version gratuite (limitée à 3 documents)
- ✅ Interface moderne
- ✅ Collaboration en temps réel

**Inconvénient :**
- ⚠️ Gratuit limité (version payante pour plus de documents)

### 3. **Cisco Packet Tracer** (pour les réseaux Cisco)

**Avantages :**
- ✅ Gratuit
- ✅ Simulation réseau complète (testez vos configurations !)
- ✅ Parfait pour étudier CCNA

**Inconvénient :**
- ⚠️ Orienté Cisco (pas généraliste comme Visio)

**Téléchargement :**
https://www.netacad.com/courses/packet-tracer

---

## 🎯 Exercices pratiques

### Exercice 1 : Schéma réseau d'une école (20 min)

**Contexte :**
Vous devez créer le schéma réseau d'une école :
- 1 routeur (connexion internet)
- 1 firewall
- 2 switches (bâtiment A et B)
- 20 PC élèves (10 par bâtiment)
- 2 imprimantes (1 par bâtiment)
- 1 serveur (pédagogique)

**Consignes :**
1. Ouvrez Visio (ou Draw.io)
2. Créez le schéma
3. Ajoutez les adresses IP :
   - Réseau : 10.0.0.0/16
   - Bâtiment A : 10.0.1.0/24
   - Bâtiment B : 10.0.2.0/24
4. Utilisez des couleurs pour différencier les bâtiments
5. Ajoutez une légende
6. Exportez en PDF

<details>
<summary>Cliquez pour voir un exemple de solution</summary>

**Structure attendue :**

```
                    [Internet]
                        │
                   [Routeur]
                        │
                   [Firewall]
                        │
                   [Switch Core]
                   /          \
          [Switch A]          [Switch B]
         /    |    \         /    |    \
    [10 PCs] [Imp] [Serveur]  [10 PCs] [Imp]
  Bâtiment A                 Bâtiment B
```

**Adresses IP exemple :**
- Routeur : 10.0.0.1
- Firewall : 10.0.0.2
- Switch Core : 10.0.0.10
- Switch A : 10.0.1.1
- Switch B : 10.0.2.1
- Serveur : 10.0.1.10
- PCs Bât A : 10.0.1.100-110
- PCs Bât B : 10.0.2.100-110

</details>

---

### Exercice 2 : Schéma avec VPN (15 min)

**Contexte :**
Entreprise avec 2 sites :
- Site Paris (siège) : 192.168.1.0/24
- Site Marseille (agence) : 192.168.2.0/24
- VPN site-à-site entre les 2

**Consignes :**
1. Créez 2 zones distinctes (Paris à gauche, Marseille à droite)
2. Chaque site a : 1 routeur, 1 switch, 5 PCs, 1 serveur
3. Reliez les 2 routeurs par internet avec un VPN
4. Utilisez une ligne en pointillés pour le VPN
5. Annotez "VPN IPsec 256-bit"

---

## ✅ Checklist avant de valider un schéma

Avant de considérer votre schéma terminé, vérifiez :

### Complétude
- [ ] Tous les équipements sont présents
- [ ] Tous les câbles sont connectés
- [ ] Aucun équipement "flottant" (non connecté)

### Clarté
- [ ] Les noms sont lisibles (taille de police correcte)
- [ ] Les équipements ne se chevauchent pas
- [ ] L'organisation est logique (hiérarchie du haut vers le bas)

### Information
- [ ] Les adresses IP sont indiquées sur les équipements clés
- [ ] Une légende est présente si nécessaire
- [ ] Les métadonnées sont en bas (titre, date, auteur)

### Esthétique
- [ ] Utilisation de couleurs cohérente
- [ ] Alignement des objets (utilisez les guides de Visio)
- [ ] Espacement régulier

### Export
- [ ] Exporté en PDF (pour consultation)
- [ ] Exporté en PNG (pour documents)
- [ ] Fichier source sauvegardé (.vsd / .vsdx ou .drawio)

---

## 📝 Message final de votre formateur

> **Félicitations ! Vous savez maintenant créer des schémas réseau professionnels !**
>
> **En 20 ans, j'ai vu 2 types de techniciens :**
> - Ceux qui documentent : Sereins, organisés, respectés par les collègues
> - Ceux qui ne documentent pas : Stressés, "homme-clé" (si absent = personne ne sait rien), mal vus par la direction
>
> **Soyez dans la première catégorie. Documentez TOUT.**
>
> **À l'examen :**
> - Si on vous demande de créer un schéma : Soyez méthodique
> - Commencez par le routeur (en haut), finissez par les PC (en bas)
> - N'oubliez pas les adresses IP et la légende
> - 15 minutes suffisent pour un schéma simple
>
> **En entreprise :**
> - Créez un schéma dès votre arrivée (compréhension de l'infra)
> - Mettez-le à jour à chaque changement
> - Partagez-le avec l'équipe (OneDrive, SharePoint, wiki)
> - Les managers adorent les schémas pour les présentations
>
> **Outils recommandés :**
> - Si vous avez Visio : Utilisez-le (standard en entreprise)
> - Sinon : Draw.io (gratuit, excellent, compatible Visio)
> - Pour apprendre les réseaux Cisco : Packet Tracer
>
> **Exercice pour vous :**
> - Schématisez votre réseau domestique (box, PC, TV, smartphone...)
> - Puis schématisez le réseau de votre école/organisme de formation
> - Plus vous pratiquez, plus ce sera rapide !
>
> **Un bon schéma réseau = Carte au trésor. Vous serez le héros qui sait où tout se trouve ! 🗺️**

---

<div align="center">

**Cours suivant :** [Linux Serveur](../03-Linux/linux-serveur-fondamentaux.md)

[⬅️ Retour au sommaire](../README.md) | [📊 Progression](../progression.md)

---

**💡 Exercice : Créez le schéma de votre réseau domestique !**

**🎨 Alternative gratuite recommandée : https://app.diagrams.net/**

</div>
