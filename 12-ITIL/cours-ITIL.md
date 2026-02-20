# ITIL - Le guide pratique du terrain

> 📚 **Module :** Gestion des Services IT
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4-5 heures
> 🎯 **Niveau :** TSSR - Technicien Supérieur

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction](#-introduction)
- [Les 5 phases du cycle de vie ITIL](#-les-5-phases-du-cycle-de-vie-itil)
- [Les processus essentiels pour un TSSR](#-les-processus-essentiels-pour-un-tssr)
- [ITIL dans la vraie vie](#-itil-dans-la-vraie-vie)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Comprendre ITIL sans le jargon commercial - version terrain
- ✅ Appliquer les processus ITIL dans votre quotidien de technicien
- ✅ Parler le langage des managers IT et des auditeurs
- ✅ Documenter vos interventions selon les bonnes pratiques ITIL
- ✅ Vous démarquer en entretien grâce à une vision pragmatique d'ITIL

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir une expérience de base en support informatique
- [ ] Comprendre la différence entre un incident et un problème (on va approfondir)
- [ ] Savoir ce qu'est un ticket d'incident

**Matériel nécessaire :**
- 💻 Aucun logiciel spécifique
- 🌐 Optionnel : accès à un système de ticketing (GLPI, ServiceNow, Jira Service Desk...)

---

## 📚 Introduction

### Qu'est-ce qu'ITIL vraiment ?

**La définition officielle ennuyeuse :**
ITIL (Information Technology Infrastructure Library) est un référentiel de bonnes pratiques pour la gestion des services informatiques.

**La vraie définition après 20 ans sur le terrain :**
ITIL, c'est simplement **une façon organisée de gérer l'informatique** pour éviter le chaos. Point.

Imaginez votre quotidien sans ITIL :
- Le même problème revient toutes les semaines, personne ne sait pourquoi
- Les collègues résolvent les incidents différemment, impossible de former les nouveaux
- Aucune trace de qui a fait quoi, quand et comment
- Les utilisateurs appellent directement celui qu'ils préfèrent au lieu de suivre un processus
- Impossible de prioriser : "tout est urgent !"

**ITIL, c'est la solution à ce bordel.**

### Pourquoi c'est important pour un TSSR ?

✅ **En entretien :** Dire "je connais ITIL" ne suffit plus. Dire "j'applique le processus de gestion des incidents ITIL en qualifiant, priorisant et escaladant selon la matrice de criticité" = vous êtes recruté.

✅ **Au quotidien :** ITIL structure votre travail. Vous ne gérez plus des "problèmes", vous gérez des incidents, des demandes et des problèmes (oui, ce sont 3 choses différentes).

✅ **Pour l'évolution :** Comprendre ITIL, c'est comprendre comment fonctionne une DSI moderne. Indispensable pour évoluer vers responsable support, admin système senior, ou chef de projet.

---

## ⚙️ Les 5 phases du cycle de vie ITIL

ITIL v3 et v4 organisent les services IT en **cycle de vie**. Voici la version simple :

### 1. 📋 Stratégie des Services (Service Strategy)

**En gros :** C'est le "POURQUOI" on fait les choses.

**Votre rôle de TSSR :**
Pas grand-chose directement, c'est le job des managers. MAIS vous devez comprendre que chaque service IT a un objectif métier.

**Exemple concret :**
Pourquoi on a un serveur de fichiers ? Pas "pour stocker des fichiers" (évident), mais pour "permettre aux équipes de collaborer et respecter les obligations légales de conservation des données".

> 💡 **Astuce terrain :** Quand vous proposez une solution technique, pensez "impact métier". Ça change tout en réunion.

---

### 2. 🎨 Conception des Services (Service Design)

**En gros :** C'est le "QUOI" on met en place.

**Votre rôle de TSSR :**
Participer aux specs techniques, documenter l'architecture, prévoir la capacité.

**Exemple concret :**
On doit déployer un nouveau serveur applicatif :
- Combien d'utilisateurs simultanés ?
- Quelle disponibilité nécessaire (99% ? 99.9% ?) ?
- Quelle procédure de sauvegarde ?
- Quel plan de reprise si ça plante ?

**Les 5 aspects de la conception (les fameux "5 P") :**
1. **Performance** : Ça doit être rapide
2. **Protection** : Ça doit être sécurisé
3. **Personnel** : Qui gère quoi ?
4. **Produits** : Quels outils/logiciels ?
5. **Processus** : Comment on fait tourner tout ça ?

> 💡 **Astuce terrain :** En entretien, citer les "5 P de la conception ITIL" vous démarque immédiatement. 99% des candidats ne les connaissent pas.

---

### 3. 🔄 Transition des Services (Service Transition)

**En gros :** C'est le "COMMENT" on passe de la théorie à la prod.

**Votre rôle de TSSR :**
**C'EST VOTRE QUOTIDIEN.** Tests, recette, déploiements, migration de données.

**Exemple concret :**
Migration de Windows 10 vers Windows 11 :
1. Tests en labo
2. Déploiement pilote (10 utilisateurs)
3. Validation et ajustements
4. Déploiement progressif par vagues
5. Documentation et formation

**Processus clé : Gestion des Changements (Change Management)**

C'est LE processus qui évite les catastrophes. Avant TOUT changement :
- Remplir une demande de changement (RFC = Request For Change)
- Évaluation des risques
- Planification (date, rollback)
- Validation par le CAB (Change Advisory Board)
- Exécution
- Vérification post-changement

> ⚠️ **IMPORTANT :** Dans la vraie vie, 80% des pannes critiques viennent de changements mal gérés. Pas de malveillance, juste quelqu'un qui "a juste redémarré un service".

---

### 4. 🚀 Exploitation des Services (Service Operation)

**En gros :** C'est le "MAINTENANT", le run quotidien.

**Votre rôle de TSSR :**
**C'EST VOTRE CŒUR DE MÉTIER.** 90% de votre temps est ici.

Les processus essentiels (détaillés dans la section suivante) :
- **Gestion des Incidents** : Rétablir le service le plus vite possible
- **Gestion des Demandes** : Traiter les demandes standards (création compte, reset mot de passe...)
- **Gestion des Problèmes** : Trouver la cause racine pour que ça ne se reproduise plus
- **Gestion des Événements** : Supervision et alerting (Nagios, Zabbix, PRTG...)
- **Gestion des Accès** : Qui a le droit à quoi ?

---

### 5. 📊 Amélioration Continue (Continual Service Improvement - CSI)

**En gros :** C'est le "MIEUX" - on s'améliore en permanence.

**Votre rôle de TSSR :**
Proposer des améliorations basées sur votre expérience terrain.

**Exemple concret :**
Vous constatez que 30% des tickets sont "mot de passe oublié". Au lieu de juste les traiter :
1. **Mesurer** : Combien de temps ça prend ? Quel coût ?
2. **Analyser** : Pourquoi les users oublient leur MDP ?
3. **Proposer** : Mettre en place un self-service de reset
4. **Implémenter** : Déployer la solution
5. **Vérifier** : Les tickets ont-ils baissé ?

**Le cycle de Deming (PDCA) :**
- **Plan** : Planifier l'amélioration
- **Do** : Faire les changements
- **Check** : Vérifier les résultats
- **Act** : Ajuster et standardiser

> 💡 **Astuce terrain :** Tenir un journal des "irritants récurrents" et proposer des solutions = valeur ajoutée énorme. Vous passez de "technicien qui exécute" à "expert qui améliore".

---

## 🚀 Les processus essentiels pour un TSSR

### 1. Gestion des Incidents (Incident Management)

**Définition ITIL :**
Un incident est une interruption **non planifiée** d'un service ou une dégradation de la qualité du service.

**Exemples d'incidents :**
- ❌ Imprimante en panne
- ❌ Serveur web inaccessible
- ❌ Application lente
- ❌ Email qui ne part pas

**Objectif :** Rétablir le service le plus vite possible (même avec un contournement temporaire).

#### Le cycle de vie d'un incident

```
1. DÉTECTION
   ↓
2. ENREGISTREMENT (création du ticket)
   ↓
3. QUALIFICATION (c'est quoi le problème exactement ?)
   ↓
4. CLASSIFICATION & PRIORISATION (urgent ? critique ?)
   ↓
5. DIAGNOSTIC & INVESTIGATION
   ↓
6. RÉSOLUTION & RÉTABLISSEMENT
   ↓
7. CLÔTURE (validation utilisateur + documentation)
```

#### Matrice de priorisation (à connaître PAR CŒUR)

| Impact / Urgence | 🔥 Urgent | ⚠️ Moyen | ⏰ Faible |
|------------------|-----------|----------|-----------|
| **💥 Critique** | P1 (1h) | P2 (4h) | P3 (1j) |
| **⚡ Élevé** | P2 (4h) | P3 (1j) | P4 (3j) |
| **📊 Moyen** | P3 (1j) | P4 (3j) | P5 (1sem) |
| **📝 Faible** | P4 (3j) | P5 (1sem) | P5 (2sem) |

**Exemple de qualification :**

❌ **Mauvais :** "L'imprimante marche pas"

✅ **Bon (ITIL) :**
- **Service** : Impression RH
- **Impact** : Élevé (tout le service RH est bloqué)
- **Urgence** : Urgent (paies à éditer aujourd'hui)
- **Priorité** : P2 - Résolution sous 4h
- **Solution de contournement** : Redirection vers imprimante Direction

> 💡 **Astuce terrain :** Toujours proposer un workaround (contournement) même moche. Un directeur qui peut imprimer via USB en attendant > un directeur qui ne peut pas imprimer.

#### Escalade hiérarchique vs technique

**Escalade technique :** Tu passes le ticket au niveau 2/3 car tu n'as pas les compétences/droits.
**Escalade hiérarchique :** Tu remontes à ton chef car le délai va être dépassé ou client VIP mécontent.

---

### 2. Gestion des Demandes (Request Fulfillment)

**Définition ITIL :**
Une demande est une requête d'un utilisateur pour quelque chose de **standard et prévisible**.

**Exemples de demandes :**
- ✅ Création de compte utilisateur
- ✅ Installation logiciel standard (Office, Adobe Reader...)
- ✅ Reset mot de passe
- ✅ Ajout d'imprimante
- ✅ Demande d'accès à un dossier partagé

**Différence avec un incident :**

| Incident | Demande |
|----------|---------|
| Quelque chose est **cassé** | Quelque chose est **demandé** |
| Résoudre vite | Réaliser selon planning |
| Peut être critique | Rarement urgent |
| Exemple : "Le serveur est down" | Exemple : "Je voudrais Office" |

**Catalogue de services :**

Dans une DSI ITIL, vous avez un **catalogue de services** avec :
- Ce qu'on peut demander
- Le délai de réalisation (SLA)
- Qui valide (manager ? DSI ?)
- Le coût (refacturation interne ou pas)

> 💡 **Astuce terrain :** Créer des modèles de tickets pour les demandes récurrentes vous fait gagner 70% du temps.

---

### 3. Gestion des Problèmes (Problem Management)

**Définition ITIL :**
Un problème est la **cause racine** d'un ou plusieurs incidents.

**Exemple concret :**

- **Incidents** : Tous les lundis matin, les utilisateurs du site de Lyon ne peuvent pas accéder à l'ERP pendant 30 minutes.
- **Problème** : Pourquoi ça se produit tous les lundis ?
- **Investigation** : Le backup de la BDD se lance à 8h au lieu de 6h → surcharge serveur
- **Solution permanente** : Modifier la planification du backup à 3h du matin

#### Les types de problèmes

1. **Problème réactif** : On constate des incidents récurrents → on cherche la cause
2. **Problème proactif** : On identifie une faiblesse avant que ça pète (ex: serveur à 90% de disque)

#### Known Error Database (KEDB)

C'est votre **Bible** de technicien. Une base qui documente :
- Le problème identifié
- La cause racine
- La solution de contournement
- La solution permanente (si disponible)

**Exemple :**

```markdown
PROBLÈME : Blue Screen 0x0000007B après mise à jour Windows 11
CAUSE : Incompatibilité driver RAID Intel ancienne génération
CONTOURNEMENT : Booter en mode sans échec, désinstaller KB5034441
SOLUTION PERMANENTE : Mise à jour driver Intel RAID v19.5.1.1040
FRÉQUENCE : 15 incidents sur 200 postes
STATUT : Résolu - Procédure automatisée
```

> 💡 **Astuce terrain :** Alimenter la KEDB fait de vous un technicien senior. Les juniors résolvent, les seniors capitalisent.

---

### 4. Gestion des Changements (Change Management)

**Définition ITIL :**
Processus pour gérer tous les changements de manière contrôlée afin de minimiser les risques.

**Types de changements :**

1. **Changement Standard** : Pré-approuvé, low-risk, répétitif
   - Exemple : Reset mot de passe, ajout imprimante
   - Pas besoin de RFC, procédure documentée

2. **Changement Normal** : Nécessite évaluation et approbation
   - Exemple : Migration serveur, upgrade version applicative
   - RFC complète + validation CAB

3. **Changement Urgent** : Critique, processus accéléré
   - Exemple : Patch de sécurité critique zero-day
   - Emergency CAB (e-CAB) en urgence

#### Contenu d'une RFC (Request For Change)

```markdown
TITRE : Migration serveur fichiers SRVFILE01 de Win2016 vers Win2022

DEMANDEUR : Jean Dupont (Admin Sys)
DATE SOUHAITÉE : 15/03/2026
FENÊTRE DE MAINTENANCE : Samedi 15/03 de 20h à 23h

JUSTIFICATION :
- Windows Server 2016 fin de support en 2027
- Anticiper la migration avant fin de support
- Bénéficier des améliorations de sécurité de 2022

DESCRIPTION :
1. Sauvegarde complète SRVFILE01
2. Installation Win2022 sur nouveau serveur SRVFILE02
3. Migration des partages et permissions NTFS
4. Tests de validation
5. Bascule DNS vers SRVFILE02
6. Mise en observation SRVFILE01 (30 jours)

IMPACT :
- Services affectés : Partages réseau département COMPTA, RH, DIRECTION
- Utilisateurs impactés : 45 utilisateurs
- Interruption de service : 30 minutes max (bascule DNS)

RISQUES :
- Risque faible : Perte de permissions → Mitigé par tests préalables
- Risque moyen : Incompatibilité applicative → Rollback possible sous 15 min

PLAN DE ROLLBACK :
En cas d'échec :
1. Rebascule DNS vers SRVFILE01
2. Analyse des erreurs
3. Report de la migration

VALIDATION :
Chef de projet : ✅
Responsable Infrastructure : ✅
CAB : En attente
```

> ⚠️ **IMPORTANT :** 90% des changements foireux = pas de plan de rollback. TOUJOURS prévoir comment revenir en arrière.

---

### 5. Gestion des Niveaux de Service (Service Level Management)

**Définition ITIL :**
Négocier, documenter et suivre les engagements de service (SLA).

#### SLA, OLA, UC : Les 3 niveaux

**SLA (Service Level Agreement) :**
Contrat entre IT et client/utilisateur.

Exemple :
- Incident P1 (critique) : Prise en charge en 15 min, résolution en 2h
- Incident P2 (important) : Prise en charge en 1h, résolution en 4h
- Disponibilité serveurs de prod : 99.5% (hors maintenance planifiée)

**OLA (Operational Level Agreement) :**
Accord interne entre équipes IT.

Exemple :
- L'équipe Système s'engage à fournir une VM en 48h à l'équipe Dev
- L'équipe Réseau s'engage à provisionner un VLAN en 24h

**UC (Underpinning Contract) :**
Contrat avec fournisseur externe.

Exemple :
- Le prestataire maintenance garantit une intervention sur site en 4h ouvrées
- L'opérateur télécom garantit 99.9% de disponibilité lien fibre

> 💡 **Astuce terrain :** Savoir différencier SLA/OLA/UC en entretien impressionne. 95% des candidats confondent tout.

---

## 🎯 ITIL dans la vraie vie

### Scénario 1 : La panne du lundi matin

**08h15 - Lundi matin**

Votre téléphone explose : le serveur de messagerie est down, 200 utilisateurs ne peuvent pas accéder à leurs emails.

#### ❌ Réaction non-ITIL (chaos)

1. Tu te précipites sur le serveur
2. Tu redémarres des services au hasard
3. Tu ne documentes rien
4. Ça remarche par miracle
5. Tu ne sais pas ce qui s'est passé ni comment tu as résolu
6. La semaine suivante : rebelote

#### ✅ Réaction ITIL (professionnel)

**1. DÉTECTION & ENREGISTREMENT (2 min)**
```
Incident #2025-0234
Objet : Serveur messagerie SRVMAIL01 inaccessible
Date/Heure : 15/02/2026 08:15
Reporter : Multiple (via supervision + appels users)
```

**2. QUALIFICATION & PRIORISATION (1 min)**
```
Service : Messagerie Exchange
Impact : Critique (200 users, toute l'entreprise)
Urgence : Urgent (activité métier bloquée)
Priorité : P1 → Résolution requise sous 1h
```

**3. COMMUNICATION (1 min)**
- Email auto sur liste-diffusion : "Incident en cours sur messagerie, équipe IT mobilisée, point dans 30 min"
- Escalade hiérarchique responsable IT

**4. DIAGNOSTIC (10 min)**
```bash
# Vérification services
Get-Service | Where-Object {$_.DisplayName -like "*Exchange*"}
# Service "Microsoft Exchange Information Store" : Stopped

# Vérification logs
Get-EventLog -LogName Application -Source "MSExchangeIS" -Newest 20
# Erreur : Database dismounted - Insufficient disk space
```

**Cause identifiée :** Disque E: (base Exchange) plein à 100% → Base dismount auto

**5. RÉSOLUTION (15 min)**
```powershell
# Solution immédiate : Suppression logs anciens
Remove-Item "E:\Exchange\Logs\*.log" -Force | Where-Object {$_.LastWriteTime -lt (Get-Date).AddDays(-7)}

# Libéré 45 Go

# Remount database
Mount-Database -Identity "Mailbox Database 01"

# Vérification
Get-MailboxDatabaseCopyStatus
# Status : Healthy
```

**6. VÉRIFICATION (5 min)**
- Test connexion Outlook : OK
- Test webmail : OK
- Appel à 3 utilisateurs échantillon : OK

**7. COMMUNICATION (2 min)**
- Email : "Service rétabli à 08:48, cause identifiée : saturation disque. Mesures préventives en cours."

**8. CLÔTURE (10 min)**
```
Incident #2025-0234 - RÉSOLU
Durée interruption : 33 minutes
Cause : Disque E: saturé (logs Exchange non purgés)
Résolution : Purge manuelle logs + remount base
Actions préventives :
- Création RFC #2025-0089 : Mise en place purge auto logs Exchange
- Création Problème #2025-0012 : Gestion capacité disques serveurs critiques
```

**9. POST-MORTEM (le lendemain)**
- Réunion 30 min avec l'équipe
- Documentation KEDB
- Mise en place script PowerShell purge auto
- Ajout monitoring alerte disque > 80%

> 💡 **Leçon terrain :** ITIL ne vous ralentit pas, il vous structure. Ici, 36 minutes pour résoudre ET prévenir. Sans ITIL : 10 minutes pour "résoudre" et reproductible la semaine d'après.

---

### Scénario 2 : La demande de nouvel utilisateur

**Contexte :** Le service RH recrute Marie Dubois, développeuse, arrivée prévue le 01/03.

#### ❌ Gestion artisanale (risquée)

- Le RH t'appelle directement la veille
- Tu crées le compte AD à l'arrache
- Tu oublies de l'ajouter aux bons groupes
- Elle ne peut pas accéder au GitLab, perte de temps J+1
- Pas de trace de qui a demandé, pourquoi, quand

#### ✅ Gestion ITIL (processus)

**1. DEMANDE STANDARDISÉE (J-10)**

RH remplit un formulaire web (catalogue de services) :

```
DEMANDE DE CRÉATION COMPTE UTILISATEUR
Formulaire #REQ-2025-0567

Nom : DUBOIS
Prénom : Marie
Poste : Développeuse Full-Stack
Service : DSI - Équipe Dev
Manager : Pierre Martin
Date d'arrivée : 01/03/2026
Type de contrat : CDI

ACCÈS REQUIS :
☑ Compte Active Directory
☑ Boîte email Exchange
☑ Accès VPN
☑ Accès GitLab (groupe : dev-team)
☑ Accès Jira (rôle : développeur)
☑ PC portable (MacBook Pro M3)
☑ Licence JetBrains

VALIDATION :
Manager (Pierre Martin) : ✅ Approuvé le 20/02
Responsable DSI : ✅ Approuvé le 21/02
```

**2. EXÉCUTION (J-1)**

Checklist automatique assignée au technicien :

```powershell
# Script standardisé création user
.\New-UserOnboarding.ps1 -FirstName "Marie" -LastName "Dubois" -Department "DSI" -Title "Developpeuse" -Manager "Pierre.Martin"

# Le script fait automatiquement :
# 1. Création compte AD dans OU=Users,OU=DSI
# 2. Ajout groupes : DSI-Users, Dev-Team, VPN-Users
# 3. Création boîte mail Exchange
# 4. Envoi email bienvenue avec login provisoire
# 5. Création ticket équipement PC
# 6. Demande licence JetBrains à l'admin licences
# 7. Ajout GitLab (API)
# 8. Ajout Jira (API)
```

**3. VÉRIFICATION (J-1)**

Checklist manuelle :
- [ ] Compte AD créé : marie.dubois
- [ ] Email fonctionnel : marie.dubois@entreprise.fr
- [ ] Accès VPN testé
- [ ] GitLab : membre groupe dev-team
- [ ] Jira : rôle développeur assigné
- [ ] PC configuré et prêt
- [ ] Badge d'accès commandé (RH)

**4. LIVRAISON (J+0 : 01/03)**

- Marie arrive
- Package complet prêt
- Document "Guide de démarrage" personnalisé
- Temps d'attente : 0
- Productivité immédiate

> 💡 **Résultat :** Avec processus ITIL = 15 min de travail IT. Sans = 2h + oublis + frustration.

---

### Scénario 3 : Le problème récurrent mystérieux

**Contexte :** Depuis 3 semaines, 5-6 utilisateurs par jour signalent "Internet très lent le matin".

#### Étape 1 : De l'incident au problème

```
Incidents traités :
#INC-2025-0234 : User lent internet - 08/02
#INC-2025-0245 : Lenteur navigation - 09/02
#INC-2025-0251 : Débit faible - 10/02
#INC-2025-0267 : Internet rame - 11/02
#INC-2025-0278 : Slow network - 12/02
[...15 autres incidents similaires...]

Action à chaque fois : "Redémarré le PC de l'utilisateur → Résolu"
```

**Constat :** Résoudre les symptômes ne résout pas le problème racine.

#### Étape 2 : Création d'un problème

```
PROBLÈME #PRB-2025-0034
Titre : Lenteur internet récurrente matinée (08h-10h)
Impacte : 20 incidents en 3 semaines, 5-8 utilisateurs/jour
Priorité : Moyenne (dégrade productivité mais pas bloquant)

SYMPTÔMES :
- Plage horaire : 08h00 - 10h00 précisément
- Utilisateurs affectés : aléatoire mais surtout site principal
- Symptômes : Lenteur navigation web, timeouts applications cloud
- Workaround actuel : Redémarrage PC (efficace mais temporaire)

HYPOTHÈSES :
1. Problème bande passante : saturation lien Internet ?
2. Problème DNS : résolution lente ?
3. Problème proxy/firewall : surcharge ?
4. Malware/cryptominer : activité malveillante ?
```

#### Étape 3 : Investigation méthodique

**Jour 1 - Analyse trafic réseau**

```bash
# Monitoring lien Internet via PRTG
# Graphique 08h-10h : pic à 980 Mbps (lien 1 Gbps saturé à 98%)

# Analyse top consommateurs (NetFlow)
# Source IP : 192.168.50.23 → 450 Mbps à elle seule !
```

**Identification du poste :** PC-COMPTA-05 (Poste Marie Legrand, service compta)

**Jour 2 - Analyse du poste incriminé**

```powershell
# Connexion en remote sur PC-COMPTA-05
# Analyse processus
Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 10

# Processus suspect : "OneDriveSetup.exe" - CPU 85%, Network 450 Mbps
```

**CAUSE RACINE IDENTIFIÉE :**
Marie a configuré OneDrive pour synchroniser un NAS entier (2,5 To) tous les matins à 8h via tâche planifiée personnelle. Le poste upload vers OneDrive cloud = saturation totale du lien Internet.

#### Étape 4 : Résolution

**Solution immédiate :**
- Arrêt de la synchro OneDrive sur PC-COMPTA-05
- Explication à Marie des impacts

**Solution pérenne :**
- Mise en place règle QoS sur firewall : OneDrive limité à 100 Mbps max
- Politique d'usage OneDrive formalisée (pas de sync NAS entiers)
- Formation utilisateurs sur stockage cloud

**Documentation KEDB :**

```markdown
PROBLÈME : Saturation lien Internet 08h-10h
CAUSE RACINE : OneDrive sync massive non contrôlée
SOLUTION : QoS OneDrive + Politique d'usage
PRÉVENTION : Monitoring per-user bandwidth (alerte > 200 Mbps/user)
STATUT : Résolu - Aucun incident similaire depuis 15 jours
```

> 💡 **Leçon terrain :** Gestion des problèmes = passer de pompier à architecte. Vous ne gérez plus les symptômes, vous éliminez les causes.

---

## 🎯 Exercices pratiques

### Exercice 1 : Qualifier et prioriser des incidents

**Objectif :**
Apprendre à qualifier correctement des incidents et leur attribuer la bonne priorité selon la matrice ITIL.

**Consignes :**

Pour chaque situation ci-dessous, déterminez :
1. S'il s'agit d'un **Incident**, d'une **Demande** ou d'un **Problème**
2. L'**Impact** (Critique / Élevé / Moyen / Faible)
3. L'**Urgence** (Urgent / Moyen / Faible)
4. La **Priorité résultante** (P1 à P5)
5. Le **délai de résolution** selon le SLA

**Situations :**

**A)** Le directeur financier appelle : "Mon Excel ne s'ouvre plus, j'ai une réunion conseil d'administration dans 1h avec mes tableaux de bord, c'est catastrophique !"

**B)** Un commercial écrit : "Bonjour, je voudrais installer Zoom sur mon PC pour faire des visios clients. Pas urgent, d'ici la semaine prochaine c'est bon."

**C)** La supervision alerte : Le serveur de base de données de production est à 95% CPU depuis 20 minutes. Certaines requêtes commencent à timeout.

**D)** Un utilisateur signale : "Depuis ce matin, mon imprimante fait des bourrages papier 1 fois sur 3. Je peux quand même imprimer, c'est juste chiant."

**E)** 15 tickets ouverts en 2 jours : "Impossible d'accéder au dossier partagé \\SRVFILE\PROJETS tous les après-midis entre 14h et 15h".

---

<details>
<summary>Cliquez pour voir la solution</summary>

**A) Excel du directeur financier**

- **Type** : Incident (service dégradé/cassé)
- **Impact** : Élevé (directeur bloqué, réunion stratégique)
- **Urgence** : Urgent (deadline 1h)
- **Priorité** : **P2** (résolution sous 4h, mais ici deadline plus courte)
- **SLA** : 4h max
- **Action** : Escalade immédiate, traiter en priorité absolue
- **Workaround** : Ouvrir Excel en mode sans échec / Utiliser Excel Online / Récupérer fichiers sur autre PC

---

**B) Installation Zoom**

- **Type** : **Demande** (pas un incident, c'est une requête standard)
- **Impact** : Faible (1 utilisateur, pas bloquant)
- **Urgence** : Faible (délai 1 semaine OK pour lui)
- **Priorité** : **P5**
- **SLA** : 1-2 semaines
- **Action** : Validation manager si nécessaire, planifier installation

---

**C) Serveur BDD 95% CPU**

- **Type** : Incident (dégradation service, risque de panne imminente)
- **Impact** : **Critique** (serveur de prod, timeouts = impact métier direct)
- **Urgence** : **Urgent** (situation qui empire, risque crash)
- **Priorité** : **P1** (résolution sous 1h)
- **SLA** : 1h max
- **Action** :
  - Diagnostic immédiat (requête lente ? Processus bloquant ?)
  - Analyser qui/quoi consomme (top, activity monitor BDD)
  - Kill processus si nécessaire
  - Escalade DBA si compétence insuffisante

---

**D) Imprimante bourrages papier**

- **Type** : Incident (dysfonctionnement équipement)
- **Impact** : Faible (1 utilisateur, service toujours fonctionnel)
- **Urgence** : Faible (pas bloquant, workaround existe)
- **Priorité** : **P5**
- **SLA** : 1-2 semaines
- **Action** :
  - Nettoyer rouleaux d'entraînement
  - Vérifier qualité papier
  - Si récurrent : prévoir remplacement imprimante

---

**E) Inaccessibilité dossier partagé 14h-15h**

- **Type** : **PROBLÈME** (pas un incident isolé, c'est récurrent avec pattern)
- **Priorité** : Moyenne
- **Action** :
  - Créer ticket Problème (PRB-XXX)
  - Lier les 15 incidents au problème
  - Investigation :
    - Tâche planifiée/backup qui se lance à 14h ?
    - Script qui lock le dossier ?
    - Saturation réseau à cette heure ?
  - Analyser logs serveur fichiers sur créneau 14h-15h
  - Identifier cause racine
  - Résolution définitive

**Analyse attendue :** C'est un piège classique ! Beaucoup traitent ça comme 15 incidents. Un technicien ITIL voit le pattern et ouvre un PROBLÈME pour traiter la cause racine.

</details>

---

### Exercice 2 : Rédiger une RFC (Request For Change)

**Objectif :**
Rédiger une demande de changement complète et professionnelle selon les standards ITIL.

**Contexte :**

Vous êtes technicien dans une PME de 150 personnes. L'antivirus actuel (version gratuite) ne suffit plus. Vous devez migrer vers une solution professionnelle (Microsoft Defender for Endpoint).

Le déploiement concernera :
- 120 PC Windows 10/11
- 15 serveurs Windows Server
- 10 MacBooks (équipe Marketing)

**Consignes :**

Rédigez une RFC complète incluant :
1. Titre, demandeur, date
2. Justification métier (pourquoi c'est nécessaire ?)
3. Description technique du changement
4. Impact (qui/quoi est affecté ?)
5. Risques et stratégies de mitigation
6. Plan de rollback
7. Planning de déploiement (par phases)

---

<details>
<summary>Cliquez pour voir un exemple de solution</summary>

```markdown
═══════════════════════════════════════════════════
REQUEST FOR CHANGE (RFC)
RFC-2026-0045
═══════════════════════════════════════════════════

INFORMATIONS GÉNÉRALES
────────────────────────────────────────────────────
Titre : Migration antivirus vers Microsoft Defender for Endpoint
Demandeur : Jean MARTIN - Technicien Support N2
Service : DSI
Date de demande : 15/02/2026
Date souhaitée : 15/03/2026
Type de changement : Normal (nécessite approbation CAB)

JUSTIFICATION MÉTIER
────────────────────────────────────────────────────
Contexte :
L'antivirus actuel (Avast Free) présente des limitations critiques :
- Pas de gestion centralisée (120 PC gérés manuellement)
- Pas de reporting de sécurité
- Définitions parfois obsolètes (MAJ manuelle par users)
- Non-conformité RGPD (absence traçabilité incidents sécurité)
- Incompatible avec notre futur Azure AD (roadmap 2026)

Bénéfices attendus :
✅ Gestion centralisée via Microsoft 365 Defender
✅ Conformité réglementaire (logs, audits, RGPD)
✅ Détection avancée menaces (EDR)
✅ Intégration native écosystème Microsoft
✅ Réduction temps de gestion : -10h/mois estimées

ROI : Licence M365 E5 déjà payée (fonctionnalité incluse) → Pas de coût additionnel

DESCRIPTION DU CHANGEMENT
────────────────────────────────────────────────────

Phase 1 - PRÉPARATION (J-15 à J-7)
• Configuration tenant Microsoft 365 Defender
• Création stratégies de sécurité (3 profils : Standard, Serveurs, MacOS)
• Tests en laboratoire sur 3 VM (Win10, Win11, WinServer)
• Documentation procédures

Phase 2 - PILOTE (J-5 à J-3)
• Déploiement sur 10 PC volontaires (équipe IT)
• Monitoring pendant 3 jours
• Ajustements si nécessaire
• Validation go/no-go

Phase 3 - DÉPLOIEMENT PROGRESSIF (J à J+10)
• Vague 1 (J) : 30 PC administratifs (faible criticité)
• Vague 2 (J+3) : 50 PC production (criticité moyenne)
• Vague 3 (J+7) : 40 PC commerciaux/direction (criticité élevée)
• Vague 4 (J+10) : 15 serveurs Windows (maintenance)
• Vague 5 (J+12) : 10 MacBooks (équipe Marketing)

Phase 4 - DÉCOMMISSIONNEMENT AVAST (J+15)
• Désinstallation Avast sur tous les postes
• Vérification absence résidus
• Clôture licences Avast

Méthode de déploiement :
- PC : GPO Active Directory (installation silencieuse)
- Serveurs : Installation manuelle en fenêtre de maintenance
- MacBooks : Intune (MDM)

IMPACT
────────────────────────────────────────────────────

Systèmes affectés :
• 120 PC utilisateurs Windows 10/11
• 15 serveurs Windows Server 2016/2019/2022
• 10 MacBooks équipe Marketing
• Active Directory (nouvelle GPO)
• Microsoft 365 (activation Defender)

Utilisateurs impactés :
• 145 utilisateurs (totalité entreprise)

Interruption de service :
• PC utilisateurs : Redémarrage requis (~5 min) → Planifié hors heures bureau
• Serveurs : Redémarrage requis (~10 min) → Fenêtre maintenance samedi 22/03 20h-23h
• Services métier : Aucune interruption attendue

RISQUES & MITIGATION
────────────────────────────────────────────────────

RISQUE 1 : Incompatibilité logicielle (Probabilité : Faible | Impact : Moyen)
Description : Un logiciel métier pourrait être bloqué par Defender
Mitigation :
- Tests en labo sur applications critiques identifiées
- Liste d'exclusions préparée si nécessaire
- Rollback possible sous 30 min

RISQUE 2 : Problème de performance (Probabilité : Faible | Impact : Faible)
Description : Defender consomme plus de ressources qu'Avast
Mitigation :
- Tests sur vieux PC (Intel i3, 4 Go RAM) validés OK
- Ajustement scan temps réel si nécessaire
- Monitoring CPU/RAM post-déploiement

RISQUE 3 : Problème réseau/déploiement GPO (Probabilité : Moyenne | Impact : Moyen)
Description : GPO ne se propage pas correctement
Mitigation :
- Test GPO sur OU pilote avant déploiement massif
- Script PowerShell de secours pour installation manuelle
- Support IT disponible J+0 à J+3 pour interventions

RISQUE 4 : Faux positifs massifs (Probabilité : Faible | Impact : Élevé)
Description : Defender bloque à tort des fichiers légitimes
Mitigation :
- Configuration basée sur best practices Microsoft
- Liste d'exclusions standards préparée (profils roaming, outils dev)
- Procédure d'escalade pour déblocage rapide

PLAN DE ROLLBACK
────────────────────────────────────────────────────

En cas d'échec critique (blocage massif, incompatibilité majeure) :

ROLLBACK IMMÉDIAT (sous 2h) :
1. Suspension déploiement GPO (désactivation OU concernée)
2. Désinstallation Defender via script PowerShell
   ```powershell
   Uninstall-WindowsFeature -Name Windows-Defender
   ```
3. Réinstallation Avast via package MSI de secours
4. Redémarrage postes
5. Vérification protection active

CRITÈRES DE ROLLBACK :
- Plus de 10% des postes rencontrent des problèmes bloquants
- Application métier critique bloquée sans solution rapide
- Performance système dégradée de plus de 30%

TESTS & VALIDATION
────────────────────────────────────────────────────

Critères de succès :
✅ 100% des postes protégés (reporting Defender)
✅ Définitions à jour automatiquement
✅ Aucune alerte critique non traitée
✅ Aucune plainte utilisateur sur performance
✅ Applications métier fonctionnelles
✅ Temps de boot < 10% d'augmentation

Tests post-déploiement :
- Scan complet sur 10 postes échantillon
- Test détection malware (fichier EICAR)
- Vérification reporting central
- Audit conformité RGPD

PLANNING DÉTAILLÉ
────────────────────────────────────────────────────

05/03 (J-10) : Configuration tenant Defender
08/03 (J-7)  : Tests laboratoire
10/03 (J-5)  : Déploiement pilote (10 PC IT)
13/03 (J-2)  : Validation pilote - Go/No-Go
15/03 (J)    : Vague 1 - 30 PC admin
18/03 (J+3)  : Vague 2 - 50 PC prod
22/03 (J+7)  : Vague 3 - 40 PC commerciaux + Serveurs (maintenance samedi)
25/03 (J+10) : Vague 4 - 10 MacBooks
30/03 (J+15) : Décommissionnement Avast
05/04 (J+21) : Revue post-implémentation (REX)

RESSOURCES NÉCESSAIRES
────────────────────────────────────────────────────

Humaines :
- Jean MARTIN (Tech Support) : 20h
- Sophie DURAND (Admin Sys) : 10h
- Support N1 disponible J à J+3 : 2 personnes

Techniques :
- Accès admin Microsoft 365 Defender
- Droits GPO Active Directory
- Accès Intune (MacBooks)

Financières :
- Aucun coût (inclus licence M365 E5 existante)

APPROBATIONS REQUISES
────────────────────────────────────────────────────

Responsable DSI : ☐ En attente
RSSI (si applicable) : ☐ En attente
CAB (Change Advisory Board) : ☐ Prévu réunion 20/02
Direction (info uniquement) : ☐ En attente

COMMUNICATION
────────────────────────────────────────────────────

J-7 : Email informatif à tous les utilisateurs
J-1 (par vague) : Rappel redémarrage requis
J+0 (par vague) : Email confirmation déploiement
J+15 : Communication fin de projet

SUIVI POST-CHANGEMENT
────────────────────────────────────────────────────

KPI à surveiller (30 jours) :
- Taux de détection menaces
- Nombre de faux positifs
- Tickets support liés à l'antivirus
- Performance postes (CPU/RAM moyen)
- Satisfaction utilisateurs (sondage J+30)

REX (Retour d'expérience) prévu : 05/04/2026
```

</details>

---

### Exercice 3 : Problème ou Incident ?

**Objectif :**
Différencier un incident d'un problème et savoir quand basculer de l'un à l'autre.

**Consignes :**

Pour chaque scénario, indiquez :
1. Faut-il ouvrir un **Incident** ou un **Problème** ?
2. Justifiez votre réponse
3. Si c'est un problème, proposez une méthode d'investigation

---

**Scénario A :**
L'imprimante du 3e étage affiche "Bourrage papier". C'est la première fois que ça arrive.

**Scénario B :**
Sur les 30 derniers jours, vous avez traité 47 tickets "Imprimante HP LaserJet 4015 - Bourrage papier", toutes sur la même imprimante.

**Scénario C :**
Un utilisateur appelle : "Mon PC a planté, écran bleu". Vous redémarrez, tout remarche.

**Scénario D :**
Vous constatez 12 tickets "Blue Screen STOP 0x0000007B" sur des PC Dell Optiplex 7080, tous après la dernière mise à jour Windows KB5035858.

**Scénario E :**
Le serveur web est down depuis 10 minutes. Les clients ne peuvent plus commander sur le e-commerce.

**Scénario F :**
C'est la 4e fois en 2 mois que le serveur web plante exactement le 1er du mois entre 00h et 01h.

---

<details>
<summary>Cliquez pour voir la solution</summary>

**Scénario A : Imprimante bourrage papier (1re fois)**

→ **INCIDENT**

Justification :
- C'est un événement isolé
- Pas de récurrence connue
- Objectif : rétablir le service rapidement

Action :
- Ouvrir ticket incident
- Débourrer l'imprimante
- Vérifier qualité papier
- Tester impression
- Clôturer ticket

---

**Scénario B : 47 tickets bourrage papier même imprimante**

→ **PROBLÈME**

Justification :
- Récurrence évidente (47 fois !)
- Cause racine non identifiée
- Traiter les symptômes ne résout rien

Action :
- Ouvrir ticket PROBLÈME
- Lier les 47 incidents au problème
- Investigation :
  - Âge de l'imprimante ? (usure rouleaux)
  - Type de papier utilisé ? (humidité, grammage)
  - Fréquence d'usage ? (surcharge)
  - Maintenance préventive faite ? (nettoyage rouleaux)
- Causes probables :
  1. Rouleaux d'entraînement usés → Remplacement
  2. Papier inadapté → Formation utilisateurs
  3. Imprimante en fin de vie → Remplacement

Solution permanente :
- Si rouleaux : Commande kit maintenance HP
- Si récurrent même après : Remplacement imprimante
- Documentation KEDB

---

**Scénario C : Blue Screen isolé**

→ **INCIDENT**

Justification :
- Événement unique
- Résolu par redémarrage
- Aucun pattern

Action :
- Ticket incident
- Résolution : Redémarrage
- Note : Surveiller si récurrence
- Si ça se reproduit → Analyser dump mémoire

---

**Scénario D : 12 Blue Screen après KB5035858**

→ **PROBLÈME**

Justification :
- Pattern clair : Même erreur + Même modèle PC + Même mise à jour
- Cause racine probable : Incompatibilité KB5035858 avec Dell Optiplex 7080

Action :
- Ouvrir PROBLÈME
- Investigation :
  1. Vérifier forums Microsoft / Dell : Problème connu ?
  2. Analyser dump mémoire (BlueScreenView)
  3. Tester désinstallation KB5035858 sur 1 PC

Cause probable :
- Driver incompatible (souvent : Intel RAID, chipset, réseau)

Solution permanente :
- Bloquer KB5035858 via WSUS/GPO
- Attendre patch correctif Microsoft
- OU mettre à jour drivers Dell avant nouvelle tentative MAJ

Documentation KEDB :
```
PROBLÈME : BSOD 0x0000007B post-KB5035858 sur Dell Optiplex 7080
CAUSE : Incompatibilité driver Intel RAID v18.x avec KB5035858
SOLUTION : Update driver Intel RAID v19.5 PUIS installation KB5035858
WORKAROUND : Désinstallation KB5035858 en attendant driver
```

---

**Scénario E : Serveur web down depuis 10 min**

→ **INCIDENT** (CRITIQUE - P1)

Justification :
- Interruption de service en cours
- Impact métier majeur (e-commerce = perte CA)
- Urgence absolue

Action :
- Incident P1
- Diagnostic immédiat (service arrêté ? Crash ? Saturation ?)
- Résolution rapide
- Communication client/management
- Post-mortem après résolution

---

**Scénario F : Serveur web crash 4x en 2 mois (toujours le 1er du mois 00h-01h)**

→ **PROBLÈME** (+ Incidents associés)

Justification :
- Récurrence avec pattern temporel TRÈS précis
- Cause racine évidente : Quelque chose se passe le 1er du mois à minuit

Investigation :
1. **Tâches planifiées serveur :**
   - Y a-t-il un job programmé le 1er à 00h ?
   - Script de facturation ?
   - Sauvegarde BDD ?
   - Batch de stats mensuelles ?

2. **Logs applicatifs :**
   - Analyser logs autour de 00h le 1er du mois
   - Exception ? Out of memory ?

3. **Logs système :**
   - Événements Windows/Linux à 00h

Cause probable trouvée (exemple réel vécu) :
- Script comptable génère rapport mensuel le 1er à 00h
- Requête SQL mal optimisée → Timeout 30 min → Crash MySQL → Crash app web

Solution :
- Optimiser la requête SQL (index manquant)
- OU décaler le script à 03h du matin (hors peak)
- Monitoring spécifique ce créneau

</details>

---

## 📚 Ressources

### Documentation officielle

- [ITIL 4 Foundation - Axelos (officiel)](https://www.axelos.com/certifications/itil-service-management/itil-4-foundation)
- [ITIL 4 - Guide pratique ITIL Foundation (FR)](https://www.itilfoundation.fr/)

### Outils ITSM (IT Service Management) compatibles ITIL

**Open Source :**
- [GLPI](https://glpi-project.org/) - Le plus populaire en France, complet, gratuit
- [iTop](https://www.combodo.com/itop) - Très ITIL-compliant, interface moderne
- [OTRS](https://otrs.com/) - Puissant mais complexe

**Propriétaires (leaders du marché) :**
- **ServiceNow** - Le Rolls des ITSM, très cher, utilisé dans les grandes entreprises
- **Jira Service Management** - Simple, intégré écosystème Atlassian
- **BMC Remedy** - Historique, lourd mais complet
- **Freshservice** - Cloud, simple, bon rapport qualité/prix

> 💡 **Conseil pour TSSR :** Apprenez GLPI (gratuit) pour comprendre les concepts. En entreprise, vous aurez ServiceNow ou Jira, mais les principes restent identiques.

### Certifications ITIL

**ITIL 4 Foundation** (certification d'entrée)
- Durée : 2-3 jours de formation
- Examen : QCM 40 questions (26/40 pour réussir)
- Coût : 300-800€ selon organisme
- Validité : À vie
- **Recommandation :** Passez-la ! C'est un + sur le CV et souvent exigée pour postes admin sys.

**ITIL 4 Practitioner / Managing Professional** (niveau expert)
- Pour plus tard, une fois plusieurs années d'expérience

### Livres recommandés

- **"ITIL 4 Foundation" (officiel Axelos)** - La Bible, un peu aride mais complet
- **"Processus ITIL en 100 questions" - Christian Dumont** - Format digest, très bien pour réviser

### Vidéos YouTube

- [Chaîne "IT Process Maps"](https://www.youtube.com/@ITProcessMaps) - Templates ITIL gratuits
- [Tutoriel GLPI complet](https://www.youtube.com/results?search_query=glpi+tutorial) - Pour pratiquer

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Exemples de notes utiles :**
- Différences entre outils ticketing de votre formation vs ce cours
- Questions à poser au formateur
- Exemples spécifiques de votre stage/alternance
- Idées d'amélioration pour votre entreprise actuelle

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] Différencier Incident / Demande / Problème sur des cas concrets
- [ ] Qualifier et prioriser un incident avec la matrice Impact/Urgence
- [ ] Expliquer les 5 phases du cycle de vie ITIL en moins de 2 minutes
- [ ] Rédiger une RFC simple mais complète
- [ ] Comprendre l'importance de la KEDB (Known Error Database)
- [ ] Savoir quand escalader (technique vs hiérarchique)
- [ ] Connaître les "5 P" de la conception des services
- [ ] Différencier SLA / OLA / UC
- [ ] Expliquer le cycle de Deming (PDCA)
- [ ] Être capable de parler ITIL en entretien sans réciter du par-cœur

---

## 🎯 Les phrases qui tuent en entretien

**Quand le recruteur demande : "Connaissez-vous ITIL ?"**

❌ **Réponse faible :**
"Oui, j'ai vu ça en cours, c'est pour gérer les tickets."

✅ **Réponse qui démarque :**
"Oui, j'applique quotidiennement les processus ITIL de gestion des incidents et des demandes. Par exemple, je qualifie systématiquement chaque ticket avec impact et urgence pour déterminer la priorité selon la matrice P1 à P5, ce qui garantit le respect des SLA. J'ai aussi participé à la documentation de notre KEDB pour capitaliser sur les résolutions et réduire le MTTR. Je connais également le processus de gestion des changements - je ne fais jamais de modif en prod sans RFC validée."

**Effet :** Le recruteur comprend que vous ne récitez pas, vous PRATIQUEZ.

---

**Quand on vous demande : "Racontez-moi une situation difficile que vous avez gérée"**

❌ **Réponse faible :**
"Un jour le serveur était en panne, j'ai redémarré et ça a remarché."

✅ **Réponse ITIL qui démarque :**
"On avait un incident P1 : serveur de messagerie down, 200 utilisateurs impactés. J'ai d'abord qualifié l'incident (impact critique, urgence haute), prévenu le management, puis diagnostiqué méthodiquement : logs événements → disque saturé → purge logs → remount base Exchange. Service rétabli en 35 minutes. Mais je n'ai pas juste résolu : j'ai créé un problème pour traiter la cause racine, ce qui a abouti à un script de purge automatique. Résultat : aucune récurrence depuis 6 mois. J'ai documenté ça dans notre KEDB."

**Effet :** Vous montrez que vous comprenez la différence entre "réparer" et "améliorer".

---

## 🚀 Derniers conseils du terrain (20 ans d'expérience)

### 1. ITIL n'est pas une religion

ITIL propose des **bonnes pratiques**, pas des règles absolues. Dans une TPE de 10 personnes, faire une RFC pour changer un câble réseau, c'est ridicule. Adaptez le niveau de formalisme à la taille et maturité de votre organisation.

### 2. Commencez simple

Vous débutez ? Concentrez-vous sur 3 processus :
1. **Gestion des incidents** (bien qualifier et prioriser)
2. **Gestion des demandes** (processus standard)
3. **Gestion des changements** (RFC pour tout ce qui est risqué)

Le reste viendra avec l'expérience.

### 3. Documentez TOUT

Votre mémoire vous trahira. La KEDB est votre meilleure amie. Un problème résolu et non documenté = un problème qui reviendra.

### 4. ITIL ≠ Bureaucratie

Si vos processus ralentissent plus qu'ils n'aident, c'est mal implémenté. ITIL bien fait accélère le travail, pas l'inverse.

### 5. Parlez le langage métier

Ne dites jamais "Le serveur SRVMAIL01 est down". Dites "La messagerie est inaccessible, 200 utilisateurs impactés, impossible d'envoyer/recevoir emails, impact commercial direct".

Les managers s'en foutent du nom du serveur. Ils veulent savoir l'impact métier.

### 6. La certification aide, mais la pratique prime

En entretien, raconter comment vous avez appliqué ITIL sur un projet réel vaut 10x plus qu'un diplôme.

---

## 🎓 Pour aller plus loin après le TSSR

Une fois TSSR acquis et ITIL maîtrisé, les évolutions possibles :

1. **Responsable Support / Service Desk Manager**
   Gérer une équipe de techniciens, les processus ITIL, les SLA.

2. **Administrateur Systèmes Senior**
   Infrastructure + processus ITIL de changement/problème.

3. **DevOps Engineer**
   ITIL + automatisation + culture DevOps (ITIL 4 intègre DevOps).

4. **ITSM Consultant**
   Aider les entreprises à implémenter ITIL et les outils ITSM.

5. **Architecte Infrastructure**
   Concevoir des SI en intégrant dès le départ les principes ITIL.

**Point commun :** Tous nécessitent une maîtrise d'ITIL.

---

<div align="center">

**Prochains cours recommandés :**
- Pratique GLPI (mise en situation ITIL)
- Scripting PowerShell pour automatisation (ITIL Amélioration Continue)
- Supervision & Monitoring (ITIL Gestion des Événements)

[⬅️ Retour au sommaire](../README.md)

---

*"ITIL ne fait pas de vous un meilleur technicien. ITIL fait de vous un professionnel qui structure son expertise pour la rendre scalable, traçable et améliorer en continu."*

**— Un admin réseau qui a survécu 20 ans sans péter les plombs grâce à ITIL**

</div>
