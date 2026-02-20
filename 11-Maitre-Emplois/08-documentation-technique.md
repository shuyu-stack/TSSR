# Rédaction de Procédures et Documentation Technique

> 📚 **Module :** Maître Emplois - Mission 08
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 4-6 heures
> 🎯 **Niveau :** N1-N3 (Tous niveaux)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Pourquoi documenter](#-pourquoi-documenter)
- [Types de documentation](#-types-de-documentation)
- [Structure d'une procédure](#-structure-dune-procédure)
- [Outils de documentation](#-outils-de-documentation)
- [Bonnes pratiques](#-bonnes-pratiques)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Rédiger des procédures claires et utilisables
- ✅ Créer une documentation technique structurée
- ✅ Utiliser les outils de documentation (Wiki, Confluence)
- ✅ Maintenir une base de connaissances à jour
- ✅ Documenter les incidents et leurs solutions

---

## 📝 Pourquoi documenter

```
┌─────────────────────────────────────────────────────────────┐
│               IMPORTANCE DE LA DOCUMENTATION                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  POUR L'ÉQUIPE :                                             │
│  • Partage des connaissances                                │
│  • Continuité de service (absences, turnover)               │
│  • Gain de temps (pas réinventer la roue)                  │
│  • Formation des nouveaux                                   │
│                                                              │
│  POUR L'UTILISATEUR :                                        │
│  • Résolution autonome (self-service)                       │
│  • Réponses cohérentes                                      │
│  • Réduction des tickets                                    │
│                                                              │
│  POUR L'ENTREPRISE :                                         │
│  • Qualité de service constante                             │
│  • Conformité et audit                                      │
│  • Capitalisation du savoir                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Types de documentation

### Classification

| Type | Public | Exemple |
|------|--------|---------|
| **Procédure** | Techniciens | Comment réinitialiser un mot de passe AD |
| **Mode opératoire** | Techniciens | Déploiement d'un poste via MDT |
| **FAQ** | Utilisateurs | Questions fréquentes Office 365 |
| **Article KB** | Tous | Résolution erreur Outlook 0x800CCC0E |
| **Architecture** | N2/N3 | Schéma infrastructure réseau |
| **Post-mortem** | Équipe IT | Analyse incident majeur |

---

## 📋 Structure d'une procédure

### Template standard

```markdown
# [Titre de la procédure]

## Informations
| Élément | Valeur |
|---------|--------|
| Version | 1.0 |
| Date | 14/01/2026 |
| Auteur | Prénom NOM |
| Validé par | Responsable |

## Objectif
[Description en 1-2 phrases de l'objectif]

## Prérequis
- [ ] Accès à [outil/système]
- [ ] Droits [niveau requis]
- [ ] Matériel : [liste]

## Procédure

### Étape 1 : [Titre]
1. Action 1
2. Action 2
   > ⚠️ **Attention** : Point important

### Étape 2 : [Titre]
1. Action 1
   ```
   Commande à exécuter
   ```
2. Action 2

## Vérification
- [ ] Point de contrôle 1
- [ ] Point de contrôle 2

## En cas de problème
| Problème | Solution |
|----------|----------|
| Erreur X | Action Y |

## Historique des modifications
| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0 | 14/01/2026 | P. NOM | Création |
```

### Exemple concret : Reset mot de passe AD

```markdown
# Réinitialisation mot de passe Active Directory

## Informations
| Élément | Valeur |
|---------|--------|
| Version | 2.1 |
| Date | 14/01/2026 |
| Auteur | Support IT |
| Niveau | N1 |

## Objectif
Réinitialiser le mot de passe d'un utilisateur Active Directory
et débloquer son compte si verrouillé.

## Prérequis
- [ ] Accès à ADUC ou PowerShell
- [ ] Droits "Account Operators" minimum
- [ ] Identité de l'utilisateur vérifiée

## Procédure

### Méthode 1 : Via ADUC (GUI)

1. Ouvrir **Active Directory Users and Computers**
2. Rechercher l'utilisateur (Ctrl+F)
3. Clic droit sur l'utilisateur > **Reset Password**
4. Entrer le nouveau mot de passe temporaire
5. Cocher **User must change password at next logon**
6. Si compte verrouillé : Clic droit > **Unlock Account**
7. Cliquer **OK**

### Méthode 2 : Via PowerShell

```powershell
# Réinitialiser le mot de passe
Set-ADAccountPassword -Identity "jdupont" -Reset `
    -NewPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force)

# Forcer changement au prochain login
Set-ADUser -Identity "jdupont" -ChangePasswordAtLogon $true

# Débloquer si verrouillé
Unlock-ADAccount -Identity "jdupont"
```

## Vérification
- [ ] L'utilisateur peut se connecter
- [ ] Le changement de mot de passe est demandé

## Communication utilisateur
> "Votre nouveau mot de passe temporaire est [MOT_DE_PASSE].
> À votre prochaine connexion, Windows vous demandera de le changer.
> Choisissez un mot de passe avec au moins 8 caractères,
> des majuscules, minuscules et chiffres."

## En cas de problème
| Problème | Solution |
|----------|----------|
| Compte toujours verrouillé | Vérifier verrouillage sur plusieurs DC |
| Erreur "Access Denied" | Vérifier vos droits AD |
| Utilisateur non trouvé | Vérifier l'orthographe du login |
```

---

## 🛠️ Outils de documentation

### Outils populaires

```
┌─────────────────────────────────────────────────────────────┐
│                  OUTILS DE DOCUMENTATION                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  WIKI / BASE DE CONNAISSANCES :                              │
│  • Confluence (Atlassian)                                   │
│  • SharePoint Wiki                                          │
│  • MediaWiki                                                │
│  • Notion                                                   │
│  • GitBook                                                  │
│                                                              │
│  TICKETING AVEC KB :                                         │
│  • GLPI                                                     │
│  • ServiceNow Knowledge Base                                │
│  • Jira Service Management                                  │
│  • Freshdesk                                                │
│                                                              │
│  MARKDOWN / CODE :                                           │
│  • GitHub/GitLab Wiki                                       │
│  • MkDocs                                                   │
│  • Docusaurus                                               │
│  • VS Code + Extensions                                     │
│                                                              │
│  DIAGRAMMES :                                                │
│  • Draw.io (diagrams.net)                                   │
│  • Lucidchart                                               │
│  • Microsoft Visio                                          │
│  • PlantUML                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Structure d'une base de connaissances

```
📁 Base de connaissances IT
├── 📁 Procédures internes (IT)
│   ├── 📁 Active Directory
│   │   ├── Reset mot de passe
│   │   ├── Création compte
│   │   └── Gestion des groupes
│   ├── 📁 Réseau
│   ├── 📁 Déploiement
│   └── 📁 Sécurité
├── 📁 FAQ Utilisateurs
│   ├── 📁 Office 365
│   ├── 📁 VPN
│   ├── 📁 Impression
│   └── 📁 Téléphonie
├── 📁 Architecture
│   ├── Schéma réseau
│   ├── Infrastructure serveurs
│   └── Annuaire applications
└── 📁 Post-mortems
    └── Incident du 15/12/2025
```

---

## ✨ Bonnes pratiques

### Les 10 règles d'or

```
┌─────────────────────────────────────────────────────────────┐
│              10 RÈGLES DE BONNE DOCUMENTATION                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. CLARTÉ                                                   │
│     → Une action par étape                                  │
│     → Vocabulaire simple et précis                          │
│                                                              │
│  2. STRUCTURE                                                │
│     → Titres et sous-titres                                 │
│     → Numérotation des étapes                               │
│                                                              │
│  3. CAPTURES D'ÉCRAN                                         │
│     → Annoter les zones importantes                         │
│     → Mettre à jour quand l'interface change               │
│                                                              │
│  4. PRÉREQUIS EXPLICITES                                     │
│     → Droits nécessaires                                    │
│     → Outils requis                                         │
│                                                              │
│  5. VÉRIFICATION                                             │
│     → Comment savoir si c'est réussi ?                     │
│                                                              │
│  6. GESTION DES ERREURS                                      │
│     → Problèmes courants et solutions                       │
│                                                              │
│  7. VERSIONING                                               │
│     → Numéro de version                                     │
│     → Historique des modifications                          │
│                                                              │
│  8. VALIDATION                                               │
│     → Test par un collègue                                  │
│     → Approbation responsable                               │
│                                                              │
│  9. MISE À JOUR                                              │
│     → Révision régulière (annuelle minimum)                │
│     → Mise à jour après changement système                 │
│                                                              │
│  10. ACCESSIBILITÉ                                           │
│      → Facile à trouver (recherche, tags)                  │
│      → Permissions appropriées                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Rédiger une procédure

Rédigez une procédure pour "Ajouter une imprimante réseau sur un poste Windows".

<details>
<summary>Solution</summary>

```markdown
# Ajout d'une imprimante réseau sur Windows 10/11

## Informations
| Élément | Valeur |
|---------|--------|
| Version | 1.0 |
| Date | 14/01/2026 |
| Niveau | N1 |

## Objectif
Installer une imprimante réseau partagée sur un poste Windows.

## Prérequis
- [ ] Nom ou IP de l'imprimante
- [ ] Poste connecté au réseau
- [ ] Droits utilisateur standard (ou admin si driver)

## Procédure

### Étape 1 : Accéder aux paramètres d'impression
1. Appuyer sur **Win + I** pour ouvrir les Paramètres
2. Cliquer sur **Bluetooth et appareils** (Win11) ou **Périphériques** (Win10)
3. Cliquer sur **Imprimantes et scanners**

### Étape 2 : Ajouter l'imprimante
1. Cliquer sur **Ajouter un appareil** (ou "Ajouter une imprimante")
2. Attendre la détection automatique
3. Si l'imprimante apparaît : la sélectionner et cliquer **Ajouter**
4. Si non détectée :
   - Cliquer **L'imprimante souhaitée n'est pas répertoriée**
   - Sélectionner **Ajouter une imprimante à l'aide d'une adresse IP**
   - Entrer l'IP : `192.168.1.100` (exemple)
   - Cliquer **Suivant**

### Étape 3 : Configuration
1. Sélectionner le driver approprié
2. Nommer l'imprimante
3. Choisir de partager ou non
4. Imprimer une page de test

## Vérification
- [ ] L'imprimante apparaît dans la liste
- [ ] La page de test s'imprime correctement

## En cas de problème
| Problème | Solution |
|----------|----------|
| Imprimante non détectée | Vérifier IP, ping l'imprimante |
| Driver non trouvé | Télécharger depuis site fabricant |
| Erreur d'impression | Vérifier file d'attente, redémarrer spooler |
```
</details>

### Exercice 2 : Créer un article KB

Créez un article de base de connaissances pour l'erreur "Outlook ne se connecte pas au serveur".

<details>
<summary>Solution</summary>

```markdown
# KB-2024-0156 : Outlook ne se connecte pas au serveur Exchange

## Symptômes
- Outlook affiche "Déconnecté" dans la barre de statut
- Les emails ne s'envoient/reçoivent pas
- Message "Impossible de se connecter au serveur"

## Cause
Plusieurs causes possibles :
- Problème de connexion réseau
- Profil Outlook corrompu
- Cache Outlook saturé
- Certificat expiré

## Résolution

### Solution 1 : Vérifier la connexion
1. Tester Internet (ouvrir un site web)
2. Si OK, tester Outlook Web (outlook.office.com)
3. Si OWA fonctionne → problème local Outlook

### Solution 2 : Redémarrer Outlook en mode sans échec
1. Fermer Outlook
2. Appuyer Win + R
3. Taper `outlook.exe /safe`
4. Si fonctionne → désactiver les compléments

### Solution 3 : Réparer le profil
1. Panneau de configuration > Mail
2. Profils > Supprimer le profil
3. Recréer le profil
4. Relancer Outlook

### Solution 4 : Vider le cache OST
1. Fermer Outlook
2. Naviguer vers `%localappdata%\Microsoft\Outlook`
3. Supprimer le fichier .ost
4. Relancer Outlook (resynchronisation)

## Mots-clés
Outlook, Exchange, connexion, déconnecté, OST, profil
```
</details>

---

## 📚 Ressources

- [Microsoft Style Guide](https://docs.microsoft.com/style-guide/)
- [IT Documentation Best Practices](https://www.itglue.com/)

---

## ✅ Checklist de révision

- [ ] Rédiger une procédure structurée
- [ ] Créer des articles de base de connaissances
- [ ] Utiliser les captures d'écran annotées
- [ ] Versionner et maintenir la documentation
- [ ] Organiser une base de connaissances

---

<div align="center">

**Cours suivant :** [Participation aux projets IT](./09-projets-it.md)

[⬅️ Retour au sommaire](./README.md)

</div>
