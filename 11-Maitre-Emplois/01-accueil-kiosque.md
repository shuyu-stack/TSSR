# Accueil et Prise en Charge des Utilisateurs en Kiosque

> 📚 **Module :** Maître Emplois - Mission 01
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 4-6 heures
> 🎯 **Niveau :** N1 (Débutant)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction](#-introduction)
- [L'environnement Kiosque IT](#-lenvironnement-kiosque-it)
- [Accueil et communication](#-accueil-et-communication)
- [Processus de prise en charge](#-processus-de-prise-en-charge)
- [Gestion des priorités](#-gestion-des-priorités)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Accueillir professionnellement les utilisateurs au kiosque IT
- ✅ Qualifier rapidement les demandes et incidents
- ✅ Créer et documenter un ticket de support correctement
- ✅ Gérer les priorités et le flux d'utilisateurs
- ✅ Communiquer efficacement avec des utilisateurs non-techniques
- ✅ Escalader correctement les incidents complexes

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Connaître les bases de Windows 10/11
- [ ] Comprendre l'organisation d'une entreprise (services, hiérarchie)
- [ ] Avoir des notions de base en réseau (IP, connexion)

**Matériel nécessaire :**
- 💻 Accès à un outil de ticketing (GLPI, ServiceNow ou simulateur)
- 🌐 Documentation des procédures internes

---

## 📚 Introduction

### Qu'est-ce qu'un Kiosque IT ?

Le **kiosque IT** (ou IT Bar, Genius Bar, Help Desk physique) est un point d'accueil physique où les utilisateurs peuvent venir directement rencontrer un technicien pour :

- Signaler un problème
- Demander de l'aide
- Récupérer/déposer du matériel
- Poser des questions techniques

```
┌─────────────────────────────────────────────────────────────┐
│                      KIOSQUE IT                              │
│                                                              │
│   ┌─────────┐    ┌─────────────────────────────────────┐    │
│   │         │    │  TECHNICIEN                          │    │
│   │  FILE   │    │  - Accueil                           │    │
│   │ D'ATTENTE│───▶│  - Diagnostic rapide                │    │
│   │         │    │  - Résolution ou escalade            │    │
│   └─────────┘    │  - Documentation                     │    │
│                  └─────────────────────────────────────┘    │
│                                                              │
│   🖥️ Poste de diagnostic    📱 Scanner code-barres          │
│   🖨️ Imprimante étiquettes  📋 Formulaires                  │
└─────────────────────────────────────────────────────────────┘
```

### Pourquoi c'est important ?

✅ **Premier contact** : Vous êtes le visage de l'équipe IT pour les utilisateurs
✅ **Satisfaction utilisateur** : Un bon accueil améliore la perception du service IT
✅ **Efficacité** : Une bonne qualification réduit le temps de résolution
✅ **Image de marque** : Vous représentez le professionnalisme de l'entreprise

---

## ⚙️ L'environnement Kiosque IT

### Configuration typique d'un kiosque

**Matériel du technicien :**
- Poste de travail avec double écran
- Scanner code-barres (inventaire)
- Téléphone IP
- Imprimante d'étiquettes
- Tiroir de matériel de prêt (souris, claviers, câbles)

**Outils logiciels :**
- Outil ITSM (ServiceNow, GLPI, Jira)
- Active Directory Users and Computers
- Outil de prise en main à distance (TeamViewer, Quick Assist)
- Base de connaissances (Wiki, Confluence)
- Inventaire parc (GLPI, Lansweeper)

### Les types de demandes au kiosque

| Type | Exemples | Fréquence |
|------|----------|-----------|
| **Incident** | PC ne démarre pas, écran bleu, virus | 40% |
| **Demande de service** | Nouveau compte, accès réseau, logiciel | 30% |
| **Question** | Comment faire pour..., Où trouver... | 20% |
| **Matériel** | Prêt souris, échange clavier, récupération PC | 10% |

---

## 🗣️ Accueil et communication

### Les règles d'or de l'accueil

#### 1. Le SBAM : Sourire, Bonjour, Au revoir, Merci

```
┌─────────────────────────────────────────────────────────────┐
│  😊 SOURIRE     → Même au téléphone, ça s'entend !          │
│  👋 BONJOUR     → "Bonjour, comment puis-je vous aider ?"   │
│  🚪 AU REVOIR   → "Bonne journée, n'hésitez pas à revenir"  │
│  🙏 MERCI       → "Merci pour votre patience"               │
└─────────────────────────────────────────────────────────────┘
```

#### 2. L'écoute active

- **Regardez** l'utilisateur (pas votre écran)
- **Acquiescez** pour montrer que vous suivez
- **Reformulez** pour confirmer votre compréhension
- **Ne coupez pas** la parole

**Exemple de reformulation :**
> Utilisateur : "Mon PC rame, je peux plus travailler, c'est urgent !"
>
> Technicien : "Je comprends, votre ordinateur est très lent et cela bloque votre travail. Depuis quand avez-vous ce problème ?"

#### 3. Adapter son langage

| ❌ Jargon technique | ✅ Langage adapté |
|---------------------|-------------------|
| "Votre DNS ne résout pas" | "Votre PC n'arrive pas à trouver les adresses des sites" |
| "Il faut flusher le cache" | "On va vider la mémoire temporaire" |
| "Votre profil est corrompu" | "Vos paramètres personnels ont un problème" |
| "Le contrôleur de domaine est down" | "Le serveur qui gère les connexions est indisponible" |

### Gérer les utilisateurs difficiles

#### L'utilisateur stressé/pressé

```
┌─────────────────────────────────────────────────────────────┐
│  TECHNIQUE : Rassurer et donner un délai                    │
├─────────────────────────────────────────────────────────────┤
│  1. Reconnaître l'urgence : "Je comprends que c'est urgent" │
│  2. Montrer l'action : "Je m'en occupe immédiatement"       │
│  3. Donner un délai : "Dans 10 minutes ce sera résolu"      │
│  4. Tenir parole : Respectez le délai annoncé               │
└─────────────────────────────────────────────────────────────┘
```

#### L'utilisateur mécontent

```
┌─────────────────────────────────────────────────────────────┐
│  TECHNIQUE : LISA                                            │
├─────────────────────────────────────────────────────────────┤
│  L - Laisser parler (ne pas interrompre)                    │
│  I - Identifier le problème réel                             │
│  S - Sympathiser (montrer de la compréhension)              │
│  A - Agir et proposer une solution                          │
└─────────────────────────────────────────────────────────────┘
```

**Phrases à utiliser :**
- "Je comprends votre frustration..."
- "C'est effectivement gênant, voyons comment résoudre ça..."
- "Je vais faire tout mon possible pour..."

**Phrases à ÉVITER :**
- ❌ "C'est pas ma faute"
- ❌ "C'est comme ça"
- ❌ "Vous avez dû faire une mauvaise manipulation"
- ❌ "Ce n'est pas mon service"

#### L'utilisateur VIP (Direction)

```
┌─────────────────────────────────────────────────────────────┐
│  ATTENTION : Certains utilisateurs sont prioritaires        │
├─────────────────────────────────────────────────────────────┤
│  - Membres de la direction                                   │
│  - Managers avec réunions critiques                         │
│  - Utilisateurs avec deadline projet                        │
│                                                              │
│  → Traiter en priorité mais rester équitable                │
│  → Informer les autres utilisateurs en attente              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 Processus de prise en charge

### Étape 1 : Accueil et identification

```
┌─────────────────────────────────────────────────────────────┐
│  CHECKLIST ACCUEIL                                           │
├─────────────────────────────────────────────────────────────┤
│  □ Saluer l'utilisateur                                      │
│  □ Demander son nom et service                               │
│  □ Vérifier son identité dans l'AD si nécessaire            │
│  □ Noter le numéro de poste/asset                           │
└─────────────────────────────────────────────────────────────┘
```

**Script d'accueil :**
```
"Bonjour ! Je suis [Prénom], technicien support.
Comment puis-je vous aider aujourd'hui ?"

[Après explication de l'utilisateur]

"D'accord, pouvez-vous me donner votre nom et votre service
pour que je crée un ticket de suivi ?"
```

### Étape 2 : Qualification de la demande

#### La méthode QQOQCP

| Question | Objectif | Exemple |
|----------|----------|---------|
| **Q**ui ? | Identifier l'utilisateur | "Quel est votre nom, service ?" |
| **Q**uoi ? | Comprendre le problème | "Que se passe-t-il exactement ?" |
| **O**ù ? | Localiser | "Sur quel poste ? Quel logiciel ?" |
| **Q**uand ? | Temporalité | "Depuis quand ? Après quelle action ?" |
| **C**omment ? | Circonstances | "Comment cela s'est produit ?" |
| **P**ourquoi ? | Impact | "En quoi cela vous bloque ?" |

**Exemple de qualification :**

```
Technicien : "Bonjour, comment puis-je vous aider ?"

Utilisateur : "Mon Outlook ne marche plus !"

Technicien : "D'accord, que se passe-t-il exactement avec Outlook ?"

Utilisateur : "Il ne s'ouvre plus du tout."

Technicien : "Depuis quand avez-vous ce problème ?"

Utilisateur : "Depuis ce matin."

Technicien : "Avez-vous un message d'erreur quand vous essayez de l'ouvrir ?"

Utilisateur : "Oui, il dit quelque chose sur le profil."

Technicien : "Parfait, je vais créer un ticket et regarder ça
             immédiatement. Pouvez-vous me donner le nom de votre PC ?"
```

### Étape 3 : Création du ticket

#### Structure d'un bon ticket

```
┌─────────────────────────────────────────────────────────────┐
│  TICKET #INC0012345                                          │
├─────────────────────────────────────────────────────────────┤
│  Demandeur : Jean DUPONT (Comptabilité)                     │
│  Contact : jean.dupont@entreprise.fr | Poste 1234           │
│  Asset : PC-COMPTA-001                                       │
├─────────────────────────────────────────────────────────────┤
│  Catégorie : Incident > Messagerie > Outlook                │
│  Priorité : P3 - Moyen                                       │
│  Impact : Utilisateur unique                                 │
├─────────────────────────────────────────────────────────────┤
│  Description :                                               │
│  L'utilisateur ne peut plus ouvrir Outlook depuis ce matin. │
│  Message d'erreur : "Impossible de charger le profil"       │
│  Aucune modification récente connue.                        │
│  L'utilisateur doit envoyer des factures avant 14h.         │
├─────────────────────────────────────────────────────────────┤
│  Actions effectuées :                                        │
│  - Vérifié la connexion réseau : OK                         │
│  - Redémarrage Outlook : KO (même erreur)                   │
│  - Escalade N2 pour réparation profil                       │
└─────────────────────────────────────────────────────────────┘
```

#### Les erreurs à éviter dans un ticket

| ❌ Mauvais ticket | ✅ Bon ticket |
|-------------------|---------------|
| "Outlook marche pas" | "Outlook ne démarre pas - erreur profil corrompu" |
| "User en galère" | "Utilisateur Jean DUPONT (Compta) bloqué" |
| "Fait le nécessaire" | "Actions : redémarrage, vérif réseau, escalade N2" |
| "Urgent !!!!!" | "Impact : Factures à envoyer avant 14h (deadline)" |

### Étape 4 : Diagnostic rapide

#### Checklist diagnostic kiosque (5 minutes max)

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC EXPRESS - À faire au kiosque                    │
├─────────────────────────────────────────────────────────────┤
│  □ Vérifier si le problème est connu (base de connaissances)│
│  □ Poser les questions QQOQCP                               │
│  □ Vérifier connexion réseau (ping, ipconfig)               │
│  □ Vérifier les services de base (AD, DNS)                  │
│  □ Tenter un redémarrage si applicable                      │
│  □ Vérifier les droits utilisateur                          │
└─────────────────────────────────────────────────────────────┘
```

#### Commandes de diagnostic rapide

```powershell
# Vérifier la connectivité réseau
ping 8.8.8.8
ping serveur-ad

# Vérifier la configuration IP
ipconfig /all

# Vérifier la connexion au domaine
nltest /sc_query:DOMAINE

# Vérifier les informations utilisateur
whoami /all

# Vérifier l'état du PC
systeminfo | findstr /C:"System Boot Time"
```

### Étape 5 : Résolution ou escalade

#### Arbre de décision

```
                    ┌─────────────────┐
                    │ Problème reçu   │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │ Connu et simple?│
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
          ┌───▼───┐     ┌────▼────┐   ┌─────▼─────┐
          │  OUI  │     │  NON    │   │ COMPLEXE  │
          └───┬───┘     └────┬────┘   └─────┬─────┘
              │              │              │
       ┌──────▼──────┐ ┌─────▼─────┐ ┌──────▼──────┐
       │ Résoudre    │ │ Diagnostic│ │ Escalade N2 │
       │ immédiat    │ │ approfondi│ │ ou N3       │
       └──────┬──────┘ └─────┬─────┘ └──────┬──────┘
              │              │              │
              └──────────────┼──────────────┘
                             │
                    ┌────────▼────────┐
                    │ Documenter et   │
                    │ fermer ticket   │
                    └─────────────────┘
```

#### Critères d'escalade

**Escalader vers N2 si :**
- Problème nécessitant des droits admin
- Installation/configuration logicielle complexe
- Problème matériel nécessitant intervention
- Diagnostic dépassant 15 minutes

**Escalader vers N3 si :**
- Problème d'infrastructure (serveur, réseau)
- Bug applicatif nécessitant l'éditeur
- Incident de sécurité
- Problème impactant plusieurs utilisateurs

---

## ⚡ Gestion des priorités

### Matrice de priorité ITIL

```
                    URGENCE
                    Haute    Moyenne   Basse
                ┌─────────┬─────────┬─────────┐
         Élevé  │   P1    │   P2    │   P3    │
                │ Critique│  Haute  │ Moyenne │
  IMPACT        ├─────────┼─────────┼─────────┤
         Moyen  │   P2    │   P3    │   P4    │
                │  Haute  │ Moyenne │  Basse  │
                ├─────────┼─────────┼─────────┤
         Faible │   P3    │   P4    │   P5    │
                │ Moyenne │  Basse  │Planifiée│
                └─────────┴─────────┴─────────┘
```

### Temps de réponse cibles

| Priorité | Description | Temps de réponse | Temps de résolution |
|----------|-------------|------------------|---------------------|
| **P1** | Critique - Service arrêté | 15 min | 4h |
| **P2** | Haute - Dégradation majeure | 30 min | 8h |
| **P3** | Moyenne - Impact limité | 2h | 24h |
| **P4** | Basse - Gêne mineure | 4h | 48h |
| **P5** | Planifiée - Amélioration | 8h | À planifier |

### Gérer la file d'attente

#### Technique du FIFO amélioré

```
┌─────────────────────────────────────────────────────────────┐
│  GESTION DE FILE D'ATTENTE                                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. TRIER par urgence/impact (pas uniquement ordre arrivée) │
│                                                              │
│  2. TRAITER les demandes rapides (<5min) immédiatement      │
│     → Reset mot de passe, question simple, prêt matériel    │
│                                                              │
│  3. PLANIFIER les demandes longues                          │
│     → "Je crée un ticket, un collègue vous recontacte"      │
│                                                              │
│  4. INFORMER les utilisateurs en attente                    │
│     → "Il y a 2 personnes avant vous, environ 10 minutes"   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

> 💡 **Astuce :** Gardez toujours quelques tickets "rapides" sous la main pour les traiter entre deux demandes complexes.

---

## 🎯 Exercices pratiques

### Exercice 1 : Simulation d'accueil

**Objectif :**
Pratiquer l'accueil et la qualification d'une demande.

**Scénario :**
Un utilisateur arrive au kiosque visiblement stressé : "J'ai une réunion importante dans 30 minutes et mon PowerPoint ne s'ouvre plus ! Il me dit qu'il y a une erreur !"

**Consignes :**
1. Accueillez l'utilisateur de manière professionnelle
2. Calmez-le et qualifiez sa demande (QQOQCP)
3. Déterminez la priorité du ticket
4. Rédigez le ticket complet

<details>
<summary>Cliquez pour voir la solution</summary>

**Script d'accueil :**
```
"Bonjour, je comprends que c'est urgent avec votre réunion.
Je vais m'en occuper tout de suite.
Pouvez-vous me donner votre nom et le nom du fichier PowerPoint ?"
```

**Qualification QQOQCP :**
- Qui : [Nom utilisateur]
- Quoi : PowerPoint ne s'ouvre pas, message d'erreur
- Où : Sur son PC, fichier spécifique ou tous les .pptx ?
- Quand : Depuis quand ? Premier essai aujourd'hui ?
- Comment : Quel message d'erreur exactement ?
- Pourquoi urgent : Réunion dans 30 min

**Priorité :** P2 (Haute) - Urgence élevée + Impact moyen

**Ticket :**
```
Titre : PowerPoint - Impossible d'ouvrir fichier - Réunion urgente
Demandeur : [Nom] - [Service]
Priorité : P2
Description :
- Utilisateur ne peut pas ouvrir son fichier PowerPoint
- Message d'erreur : [à compléter]
- Réunion prévue dans 30 minutes
- Fichier stocké sur : [local/réseau]
Actions :
- Diagnostic en cours
- Vérification emplacement fichier
```

</details>

### Exercice 2 : Rédaction de tickets

**Objectif :**
Améliorer la qualité de rédaction des tickets.

**Consignes :**
Transformez ces mauvais tickets en bons tickets :

1. "Outlook bug"
2. "Ordi lent user méchant"
3. "Imprimante marche pas 3ème étage"

<details>
<summary>Cliquez pour voir la solution</summary>

**Ticket 1 - Corrigé :**
```
Titre : Outlook - Plantage à l'ouverture - Erreur profil
Demandeur : [Nom] - [Service]
Priorité : P3
Description :
- Outlook se ferme immédiatement après ouverture
- Message d'erreur : "Impossible de charger le profil"
- Problème apparu ce matin sans modification connue
Actions effectuées :
- Redémarrage Outlook : KO
- Redémarrage PC : KO
→ Escalade N2 pour réparation profil
```

**Ticket 2 - Corrigé :**
```
Titre : Lenteurs PC - Performance dégradée
Demandeur : [Nom] - [Service]
Priorité : P3
Description :
- PC très lent depuis 2 jours
- Temps de démarrage : ~10 minutes
- Applications longues à ouvrir
- Utilisateur frustré, impacte sa productivité
Diagnostic :
- Espace disque : 90% utilisé
- RAM : 95% utilisée au repos
Actions effectuées :
- Nettoyage fichiers temporaires
- Désinstallation programmes inutiles
→ Escalade N2 pour analyse approfondie
```

**Ticket 3 - Corrigé :**
```
Titre : Imprimante HP-3EME-001 - Hors service
Demandeur : [Signalé par Nom] - [Service]
Priorité : P3
Localisation : 3ème étage, open space comptabilité
Description :
- Imprimante ne répond plus depuis ce matin
- Voyant orange clignotant
- Files d'attente bloquées (15 documents)
- Impact : 8 utilisateurs du service comptabilité
Actions effectuées :
- Vérification connexion réseau : OK
- Redémarrage imprimante : KO
- Bourrage papier vérifié : Aucun
→ Escalade N2 pour intervention sur site
```

</details>

### Exercice 3 : Gestion de conflit

**Objectif :**
Apprendre à gérer un utilisateur mécontent.

**Scénario :**
Un utilisateur arrive furieux : "Ça fait 3 fois que je viens pour le même problème ! Vous êtes vraiment nuls à l'informatique ! Mon PC plante tous les jours et personne ne fait rien !"

**Consignes :**
1. Appliquez la méthode LISA
2. Proposez une réponse professionnelle
3. Identifiez les actions à entreprendre

<details>
<summary>Cliquez pour voir la solution</summary>

**Application de LISA :**

**L - Laisser parler :**
Ne pas interrompre, laisser l'utilisateur exprimer sa frustration.

**I - Identifier :**
"Je vois dans l'historique que vous avez eu 3 tickets pour des plantages. Pouvez-vous me décrire exactement ce qui se passe ?"

**S - Sympathiser :**
"Je comprends parfaitement votre frustration. C'est effectivement inacceptable d'avoir un problème récurrent qui n'est pas résolu. Je vais personnellement m'assurer qu'on trouve une solution définitive."

**A - Agir :**
"Voilà ce que je vous propose :
1. Je consulte immédiatement l'historique de vos tickets
2. J'escalade directement au responsable technique
3. Je vous tiens informé par email dans l'heure
4. Si nécessaire, nous prévoyons un remplacement de votre PC"

**Réponse professionnelle :**
```
"Monsieur/Madame [Nom], je suis vraiment désolé pour cette
situation. Vous avez raison d'être mécontent, avoir le même
problème 3 fois sans solution n'est pas normal.

Je vais regarder l'historique de vos tickets tout de suite
pour comprendre pourquoi le problème n'a pas été résolu
définitivement.

Je m'engage personnellement à suivre ce dossier et à vous
donner une réponse avant [heure].

Est-ce que je peux prendre votre numéro de téléphone direct
pour vous tenir informé ?"
```

**Actions à entreprendre :**
- Consulter l'historique des 3 tickets
- Identifier pourquoi le problème récurrent
- Escalader au N2/N3 avec mention "récurrence"
- Proposer un RDV pour intervention approfondie
- Mettre en place un suivi personnalisé

</details>

---

## 📚 Ressources

### Documentation officielle
- [ITIL Foundation - Gestion des incidents](https://www.axelos.com/certifications/itil-certifications)
- [Microsoft - Support technique](https://docs.microsoft.com/fr-fr/learn/paths/support/)

### Tutoriels
- [IT-Connect - Méthodologie support](https://www.it-connect.fr)
- [HDI - Help Desk Institute](https://www.thinkhdi.com)

### Outils ITSM (pour s'entraîner)
- [GLPI Demo](https://demo.glpi-project.org)
- [ServiceNow Developer](https://developer.servicenow.com)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] L'accueil professionnel des utilisateurs (SBAM)
- [ ] La qualification des demandes (QQOQCP)
- [ ] La rédaction de tickets de qualité
- [ ] La gestion des priorités (matrice ITIL)
- [ ] La communication avec les utilisateurs difficiles (LISA)
- [ ] Les critères d'escalade N1/N2/N3
- [ ] Les commandes de diagnostic rapide

---

<div align="center">

**Cours suivant :** [Support de proximité sur site](./02-support-proximite.md)

[⬅️ Retour au sommaire](./README.md)

</div>
