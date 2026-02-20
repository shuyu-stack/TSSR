# Suivi et Mise à Jour des Tickets dans l'Outil ITSM

> 📚 **Module :** Maître Emplois - Mission 10
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 4-6 heures
> 🎯 **Niveau :** N1-N3 (Tous niveaux)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Introduction à l'ITSM](#-introduction-à-litsm)
- [Cycle de vie d'un ticket](#-cycle-de-vie-dun-ticket)
- [Outils ITSM courants](#-outils-itsm-courants)
- [Bonnes pratiques ticketing](#-bonnes-pratiques-ticketing)
- [KPI et reporting](#-kpi-et-reporting)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Comprendre les concepts ITIL de gestion des incidents
- ✅ Créer et qualifier des tickets correctement
- ✅ Suivre et mettre à jour les tickets tout au long de leur cycle
- ✅ Utiliser les outils ITSM (GLPI, ServiceNow, Jira)
- ✅ Analyser les KPI de support

---

## 📚 Introduction à l'ITSM

### Qu'est-ce que l'ITSM ?

**ITSM** (IT Service Management) = Gestion des Services Informatiques

C'est l'ensemble des processus et outils pour gérer la fourniture de services IT aux utilisateurs.

```
┌─────────────────────────────────────────────────────────────┐
│                    PROCESSUS ITIL CLÉS                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  GESTION DES INCIDENTS                                       │
│  Restaurer le service le plus rapidement possible           │
│  Ex: "Mon PC ne démarre plus" → Dépanner                    │
│                                                              │
│  GESTION DES DEMANDES                                        │
│  Traiter les demandes de service standard                   │
│  Ex: "J'ai besoin d'un accès VPN" → Créer l'accès          │
│                                                              │
│  GESTION DES PROBLÈMES                                       │
│  Identifier la cause racine des incidents récurrents        │
│  Ex: "Les PC du 3ème plantent souvent" → Investigation     │
│                                                              │
│  GESTION DES CHANGEMENTS                                     │
│  Contrôler les modifications de l'infrastructure            │
│  Ex: "Mise à jour serveur" → Planifier, valider, exécuter  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Incident vs Demande de service

| Incident | Demande de service |
|----------|-------------------|
| Interruption non planifiée | Demande standard |
| Dégradation de service | Demande d'information |
| Urgence variable | Planifiable |
| Ex: Outlook ne marche plus | Ex: Besoin d'une souris |

---

## 🔄 Cycle de vie d'un ticket

### États d'un ticket

```
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│ NOUVEAU │──▶│ ASSIGNÉ │──▶│EN COURS │──▶│ RÉSOLU  │──▶│  FERMÉ  │
└─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘
                  │              │              │
                  │              │              ▼
                  │              │         ┌─────────┐
                  │              └────────▶│ EN      │
                  │                        │ ATTENTE │
                  │                        └─────────┘
                  │                             │
                  └─────────────────────────────┘
```

### Cycle détaillé

| État | Description | Actions |
|------|-------------|---------|
| **Nouveau** | Ticket créé, non traité | Qualifier, prioriser |
| **Assigné** | Attribué à un technicien | Prise en charge |
| **En cours** | Travail en cours | Diagnostic, résolution |
| **En attente** | Attente info/tiers | Utilisateur, fournisseur |
| **Résolu** | Solution appliquée | Validation utilisateur |
| **Fermé** | Ticket clôturé | Archivé |

### SLA (Service Level Agreement)

```
┌─────────────────────────────────────────────────────────────┐
│                    MATRICE SLA EXEMPLE                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  PRIORITÉ 1 - CRITIQUE                                       │
│  • Impact : Service majeur arrêté                           │
│  • Temps de réponse : 15 min                                │
│  • Temps de résolution : 4h                                 │
│                                                              │
│  PRIORITÉ 2 - HAUTE                                          │
│  • Impact : Dégradation importante                          │
│  • Temps de réponse : 30 min                                │
│  • Temps de résolution : 8h                                 │
│                                                              │
│  PRIORITÉ 3 - MOYENNE                                        │
│  • Impact : Gêne modérée                                    │
│  • Temps de réponse : 2h                                    │
│  • Temps de résolution : 24h                                │
│                                                              │
│  PRIORITÉ 4 - BASSE                                          │
│  • Impact : Gêne mineure                                    │
│  • Temps de réponse : 4h                                    │
│  • Temps de résolution : 48h                                │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Outils ITSM courants

### GLPI (Open Source)

```
┌─────────────────────────────────────────────────────────────┐
│                         GLPI                                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  FONCTIONNALITÉS :                                           │
│  • Gestion des tickets                                      │
│  • Inventaire automatique (avec FusionInventory)            │
│  • Base de connaissances                                    │
│  • Gestion du parc informatique                             │
│  • Rapports et statistiques                                 │
│                                                              │
│  AVANTAGES :                                                 │
│  ✓ Gratuit et open source                                   │
│  ✓ Communauté active                                        │
│  ✓ Plugins nombreux                                         │
│                                                              │
│  URL DEMO : https://demo.glpi-project.org                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### ServiceNow

```
┌─────────────────────────────────────────────────────────────┐
│                      SERVICENOW                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  FONCTIONNALITÉS :                                           │
│  • ITSM complet (Incident, Problem, Change)                 │
│  • Workflows automatisés                                    │
│  • CMDB intégrée                                            │
│  • Portail self-service                                     │
│  • Rapports avancés                                         │
│                                                              │
│  AVANTAGES :                                                 │
│  ✓ Solution entreprise complète                             │
│  ✓ Automatisation poussée                                   │
│  ✓ Intégrations nombreuses                                  │
│                                                              │
│  URL DEV : https://developer.servicenow.com                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Jira Service Management

```
┌─────────────────────────────────────────────────────────────┐
│                JIRA SERVICE MANAGEMENT                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  FONCTIONNALITÉS :                                           │
│  • Gestion des demandes et incidents                        │
│  • Files d'attente personnalisables                        │
│  • SLA configurables                                        │
│  • Portail client                                           │
│  • Intégration Confluence (KB)                             │
│                                                              │
│  AVANTAGES :                                                 │
│  ✓ Interface moderne                                        │
│  ✓ Intégration écosystème Atlassian                        │
│  ✓ Version cloud ou serveur                                │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## ✨ Bonnes pratiques ticketing

### Création d'un ticket

```
┌─────────────────────────────────────────────────────────────┐
│           STRUCTURE D'UN BON TICKET                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  TITRE : Clair et descriptif                                │
│  ✅ "Outlook - Impossible d'envoyer des emails - Erreur 550"│
│  ❌ "Outlook bug"                                           │
│                                                              │
│  DESCRIPTION :                                               │
│  • Contexte : Que faisait l'utilisateur ?                   │
│  • Symptôme : Que se passe-t-il exactement ?                │
│  • Message d'erreur : Texte exact ou capture               │
│  • Depuis quand : Date/heure de début                       │
│  • Impact : Conséquences sur le travail                     │
│                                                              │
│  INFORMATIONS REQUISES :                                     │
│  • Demandeur : Nom, service, contact                        │
│  • Asset : Nom du PC, numéro d'inventaire                   │
│  • Catégorie : Classification du ticket                     │
│  • Priorité : Selon matrice impact/urgence                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Mise à jour du ticket

```
┌─────────────────────────────────────────────────────────────┐
│           RÈGLES DE MISE À JOUR                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. DOCUMENTER CHAQUE ACTION                                 │
│     • Qu'avez-vous fait ?                                   │
│     • Quel résultat ?                                       │
│     • Quelle suite ?                                        │
│                                                              │
│  2. CHRONOLOGIE CLAIRE                                       │
│     14:30 - Prise en charge du ticket                       │
│     14:35 - Contact utilisateur, diagnostic distant         │
│     14:45 - Identification du problème : profil corrompu    │
│     15:00 - Réparation du profil Outlook                    │
│     15:10 - Test avec utilisateur : OK                      │
│                                                              │
│  3. COMMUNICATION UTILISATEUR                                │
│     • Informer de la prise en charge                        │
│     • Informer des délais si attente                        │
│     • Confirmer la résolution                               │
│                                                              │
│  4. CHANGEMENT D'ÉTAT                                        │
│     • Mettre à jour le statut en temps réel                │
│     • Ne pas laisser un ticket "En cours" si en attente    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Exemple de ticket bien documenté

```
TICKET : INC0012345
═══════════════════════════════════════════════════════════════

TITRE : Outlook 365 - Impossible d'envoyer des emails externes

DEMANDEUR : Marie MARTIN - Comptabilité
CONTACT : marie.martin@entreprise.fr | Poste 1234
ASSET : PC-COMPTA-015

CATÉGORIE : Messagerie > Outlook > Envoi/Réception
PRIORITÉ : P3 - Moyenne
SLA : Résolution 24h

───────────────────────────────────────────────────────────────
DESCRIPTION INITIALE (09:15)
───────────────────────────────────────────────────────────────
L'utilisatrice ne peut plus envoyer d'emails vers l'extérieur
depuis ce matin 8h30.
Les emails internes fonctionnent.
Message d'erreur : "Votre message n'a pas pu être remis"
Code erreur : 550 5.7.1 Relay denied
Impact : Envoi factures clients bloqué (échéance fin de semaine)

───────────────────────────────────────────────────────────────
JOURNAL DE TRAITEMENT
───────────────────────────────────────────────────────────────

[09:30 - Jean DUPONT]
Prise en charge du ticket.
Contact utilisateur : problème confirmé.
Test envoi interne : OK
Test envoi externe : KO - même erreur
→ Escalade N2 pour vérification serveur mail

[10:15 - Pierre MARTIN - N2]
Vérification serveur Exchange : RAS
Vérification SPF/DKIM : OK
Vérification IP blacklist : IP sortante dans blacklist Spamhaus
→ Demande de delist en cours

[11:30 - Pierre MARTIN - N2]
Delist effectué par Spamhaus (délai 1-2h propagation)

[13:45 - Pierre MARTIN - N2]
Test envoi externe : OK
Utilisatrice informée, confirmation test réussi
→ Ticket résolu

[14:00 - Marie MARTIN]
Validé, je peux à nouveau envoyer mes emails. Merci !
→ Ticket clôturé

───────────────────────────────────────────────────────────────
RÉSOLUTION : IP serveur mail dans blacklist Spamhaus
ACTION : Demande de delist effectuée
DURÉE : 4h30
───────────────────────────────────────────────────────────────
```

---

## 📊 KPI et reporting

### Indicateurs clés

```
┌─────────────────────────────────────────────────────────────┐
│                    KPI SUPPORT IT                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  VOLUME                                                      │
│  • Nombre de tickets créés / période                        │
│  • Nombre de tickets résolus / période                      │
│  • Backlog (tickets en attente)                             │
│                                                              │
│  PERFORMANCE                                                 │
│  • Temps moyen de résolution (MTTR)                         │
│  • Temps moyen de première réponse                          │
│  • Taux de résolution au premier contact (FCR)              │
│  • Taux de respect SLA                                      │
│                                                              │
│  QUALITÉ                                                     │
│  • Taux de réouverture des tickets                          │
│  • Satisfaction utilisateur (CSAT)                          │
│  • Taux d'escalade N2/N3                                    │
│                                                              │
│  EXEMPLE OBJECTIFS :                                         │
│  • MTTR < 4h pour P2                                        │
│  • FCR > 70%                                                │
│  • SLA respect > 95%                                        │
│  • CSAT > 4/5                                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Dashboard exemple

```
┌─────────────────────────────────────────────────────────────┐
│              DASHBOARD SUPPORT - JANVIER 2026               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  TICKETS CE MOIS                                             │
│  ┌──────────────────────────────────────┐                   │
│  │ Créés      : 450  ████████████████   │                   │
│  │ Résolus    : 435  ███████████████    │                   │
│  │ En cours   : 45   ███                │                   │
│  │ Backlog    : 15   █                  │                   │
│  └──────────────────────────────────────┘                   │
│                                                              │
│  RÉPARTITION PAR CATÉGORIE                                   │
│  ┌──────────────────────────────────────┐                   │
│  │ Messagerie     : 30%  ██████████     │                   │
│  │ Poste travail  : 25%  ████████       │                   │
│  │ Réseau         : 20%  ███████        │                   │
│  │ Applications   : 15%  █████          │                   │
│  │ Autres         : 10%  ███            │                   │
│  └──────────────────────────────────────┘                   │
│                                                              │
│  INDICATEURS CLÉS                                            │
│  ┌──────────────────────────────────────┐                   │
│  │ MTTR moyen     : 3h45   ✅ (<4h)     │                   │
│  │ FCR            : 72%    ✅ (>70%)    │                   │
│  │ SLA P1         : 98%    ✅ (>95%)    │                   │
│  │ SLA P2         : 94%    ⚠️ (<95%)    │                   │
│  │ CSAT           : 4.3/5  ✅ (>4)      │                   │
│  └──────────────────────────────────────┘                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Rédiger un ticket

L'utilisateur vous appelle : "Mon Word plante à chaque fois que j'ouvre un gros document avec des images".

Rédigez le ticket complet.

<details>
<summary>Solution</summary>

```
TICKET : INC0012XXX

TITRE : Word 365 - Plantage ouverture documents volumineux avec images

DEMANDEUR : [Nom Utilisateur] - [Service]
CONTACT : [Email] | [Téléphone]
ASSET : [Nom PC]

CATÉGORIE : Bureautique > Microsoft Office > Word
PRIORITÉ : P3 - Moyenne

DESCRIPTION :
L'utilisateur rencontre des plantages systématiques de Microsoft Word
lors de l'ouverture de documents volumineux contenant des images.

- Symptôme : Word se ferme sans message d'erreur
- Déclencheur : Ouverture fichiers > 10 Mo avec images
- Depuis : Cette semaine
- Fréquence : Systématique sur ce type de documents
- Fichiers concernés : Rapports annuels, catalogues produits
- Documents sans images : OK
- Impact : Impossible de travailler sur les rapports trimestriels

ACTIONS À EFFECTUER :
- Vérifier version Office et mises à jour
- Tester en mode sans échec (winword /safe)
- Vérifier les add-ins installés
- Réparer installation Office si nécessaire
```
</details>

### Exercice 2 : Calculer les KPI

Données du mois :
- Tickets créés : 200
- Tickets résolus : 185
- Tickets résolus au 1er contact : 130
- Tickets réouverts : 10
- Temps total résolution : 740h

Calculez : FCR, MTTR, Taux de réouverture

<details>
<summary>Solution</summary>

```
FCR (First Contact Resolution) = Résolus 1er contact / Total résolus
FCR = 130 / 185 = 70.3%

MTTR (Mean Time To Resolution) = Temps total / Tickets résolus
MTTR = 740h / 185 = 4h

Taux de réouverture = Réouverts / Total résolus
Taux réouverture = 10 / 185 = 5.4%

Analyse :
- FCR 70% : Bon (objectif généralement >70%)
- MTTR 4h : À surveiller selon les SLA
- Réouverture 5.4% : Acceptable (<10%)
```
</details>

---

## 📚 Ressources

- [ITIL Foundation](https://www.axelos.com/certifications/itil-certifications)
- [GLPI Demo](https://demo.glpi-project.org)
- [ServiceNow Developer](https://developer.servicenow.com)

---

## ✅ Checklist de révision

- [ ] Comprendre ITIL (Incident vs Demande vs Problème)
- [ ] Maîtriser le cycle de vie d'un ticket
- [ ] Créer des tickets bien documentés
- [ ] Mettre à jour les tickets en temps réel
- [ ] Analyser les KPI de support
- [ ] Utiliser au moins un outil ITSM (GLPI, ServiceNow)

---

<div align="center">

**🎉 Félicitations ! Vous avez terminé le programme Maître Emplois !**

[⬅️ Retour au sommaire](./README.md)

</div>
