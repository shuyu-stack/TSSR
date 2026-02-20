# Support à Distance - Télémaintenance et Ticketing

> 📚 **Module :** Maître Emplois - Mission 03
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 6-8 heures
> 🎯 **Niveau :** N1-N2 (Débutant à Intermédiaire)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction](#-introduction)
- [Outils de prise en main à distance](#-outils-de-prise-en-main-à-distance)
- [Méthodologie de support à distance](#-méthodologie-de-support-à-distance)
- [Gestion des appels téléphoniques](#-gestion-des-appels-téléphoniques)
- [Résolution des incidents courants](#-résolution-des-incidents-courants)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Maîtriser les principaux outils de télémaintenance
- ✅ Établir une connexion à distance sécurisée
- ✅ Guider un utilisateur par téléphone pour résoudre son problème
- ✅ Diagnostiquer et résoudre les incidents à distance
- ✅ Respecter les bonnes pratiques de sécurité
- ✅ Documenter efficacement les interventions à distance

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les cours 01 et 02 (Kiosque et Proximité)
- [ ] Connaître Windows 10/11 et ses outils d'administration
- [ ] Avoir des bases en réseau (IP, DNS, ports)
- [ ] Savoir communiquer clairement par téléphone

**Matériel nécessaire :**
- 💻 PC avec accès aux outils de télémaintenance
- 🎧 Casque téléphonique
- 📱 Accès à l'outil ITSM

---

## 📚 Introduction

### Qu'est-ce que le support à distance ?

Le **support à distance** permet de diagnostiquer et résoudre des problèmes informatiques sans se déplacer physiquement, en prenant le contrôle du poste de l'utilisateur via Internet/réseau.

```
┌─────────────────────────────────────────────────────────────┐
│                    SUPPORT À DISTANCE                        │
│                                                              │
│   TECHNICIEN                              UTILISATEUR        │
│   ┌─────────┐                             ┌─────────┐       │
│   │   🖥️    │◄═════════════════════════▶│    🖥️   │       │
│   │ Console │      Connexion             │   PC    │       │
│   │ support │      sécurisée             │ distant │       │
│   └─────────┘                             └─────────┘       │
│        │                                       │            │
│        │    ┌───────────────────────┐         │            │
│        └────┤ Outils télémaintenance├─────────┘            │
│             │ - TeamViewer          │                       │
│             │ - AnyDesk             │                       │
│             │ - Quick Assist        │                       │
│             │ - SCCM Remote         │                       │
│             └───────────────────────┘                       │
└─────────────────────────────────────────────────────────────┘
```

### Avantages du support à distance

| Avantage | Description |
|----------|-------------|
| **Rapidité** | Intervention immédiate, pas de déplacement |
| **Économie** | Pas de frais de transport |
| **Efficacité** | Plusieurs interventions en parallèle possible |
| **Traçabilité** | Enregistrement des sessions possible |
| **Disponibilité** | Support 24/7 possible |

### Limites du support à distance

| Limite | Quand escalader ? |
|--------|-------------------|
| Problème matériel | PC ne démarre pas, écran HS |
| Pas de réseau | Utilisateur déconnecté |
| Installation physique | Nouveau poste, câblage |
| Sécurité | Données sensibles, pas de consentement |

---

## 🔧 Outils de prise en main à distance

### Comparatif des outils

| Outil | Usage | Avantages | Inconvénients |
|-------|-------|-----------|---------------|
| **TeamViewer** | Universel | Multi-plateforme, simple | Payant en entreprise |
| **AnyDesk** | Universel | Léger, rapide | Moins de fonctions |
| **Quick Assist** | Windows | Gratuit, intégré | Windows uniquement |
| **SCCM Remote** | Entreprise | Intégré AD, pas besoin user | Infrastructure SCCM |
| **DameWare** | Entreprise | Complet, AD intégré | Payant |

### TeamViewer

#### Installation et configuration

```
┌─────────────────────────────────────────────────────────────┐
│  TEAMVIEWER - Configuration entreprise                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  CÔTÉ TECHNICIEN :                                           │
│  1. Installer TeamViewer Full (avec console)                │
│  2. Créer un compte TeamViewer                              │
│  3. Configurer l'accès à la liste de contacts               │
│                                                              │
│  CÔTÉ UTILISATEUR :                                          │
│  1. Installer TeamViewer Host ou QuickSupport               │
│  2. Donner l'ID et le mot de passe au technicien            │
│                                                              │
│  PARAMÈTRES RECOMMANDÉS :                                    │
│  - Qualité : "Optimiser la vitesse"                         │
│  - Enregistrement : Activé (si autorisé)                    │
│  - Transfert fichiers : Activé                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### Utilisation TeamViewer

```
CONNEXION :
1. Ouvrir TeamViewer
2. Entrer l'ID partenaire (9 chiffres)
3. Cliquer "Connexion"
4. Entrer le mot de passe communiqué
5. Accepter le contrôle (côté utilisateur)

FONCTIONNALITÉS UTILES :
- Ctrl+Alt+Suppr à distance : Barre d'outils > Actions
- Transfert de fichiers : Barre d'outils > Fichiers
- Chat : Communication > Chat
- Redémarrer en mode sans échec : Actions > Redémarrer
- Enregistrement : Extras > Enregistrer
```

### Microsoft Quick Assist (Assistance rapide)

#### Avantages Quick Assist

- ✅ Gratuit et intégré à Windows 10/11
- ✅ Pas d'installation nécessaire
- ✅ Sécurisé (compte Microsoft)
- ✅ Simple d'utilisation

#### Utilisation Quick Assist

**Côté technicien (aidant) :**
```
1. Ouvrir Quick Assist
   - Rechercher "Assistance rapide" dans le menu Démarrer
   - Ou Win + Ctrl + Q

2. Cliquer "Aider quelqu'un"

3. Se connecter avec son compte Microsoft

4. Copier le code de sécurité (6 caractères)

5. Communiquer le code à l'utilisateur

6. Attendre la connexion et l'acceptation
```

**Côté utilisateur (aidé) :**
```
1. Ouvrir Quick Assist
   - Rechercher "Assistance rapide"
   - Ou Win + Ctrl + Q

2. Entrer le code de sécurité fourni par le technicien

3. Cliquer "Partager l'écran"

4. Choisir :
   - "Contrôle total" → Le technicien peut agir
   - "Afficher l'écran" → Le technicien voit seulement

5. Cliquer "Autoriser"
```

### SCCM / MECM Remote Control

#### Configuration préalable (administrateur)

```powershell
# Vérifier que le client SCCM est installé
Get-WmiObject -Namespace root\ccm -Class SMS_Client

# Vérifier la policy Remote Control
Get-WmiObject -Namespace root\ccm\policy\machine -Class CCM_RemoteToolsConfig
```

#### Utilisation SCCM Remote

```
┌─────────────────────────────────────────────────────────────┐
│  SCCM REMOTE CONTROL                                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  DEPUIS LA CONSOLE SCCM :                                    │
│  1. Assets and Compliance > Devices                         │
│  2. Rechercher le PC par nom                                │
│  3. Clic droit > Start > Remote Control                     │
│                                                              │
│  DEPUIS CMRCVIEWER.EXE :                                     │
│  1. Ouvrir C:\Program Files\Microsoft Configuration Manager │
│     \AdminConsole\bin\i386\CmRcViewer.exe                   │
│  2. Entrer le nom du PC                                      │
│  3. Cliquer "Connect"                                        │
│                                                              │
│  AVANTAGES :                                                 │
│  - Pas besoin de code/mot de passe                          │
│  - Notification à l'utilisateur (configurable)              │
│  - Intégré à l'inventaire SCCM                              │
│  - Journalisation automatique                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### PowerShell Remoting

#### Pour les tâches d'administration sans GUI

```powershell
# Activer PowerShell Remoting (une fois, sur le PC distant)
Enable-PSRemoting -Force

# Se connecter à un PC distant
Enter-PSSession -ComputerName PC-USER-001

# Exécuter une commande à distance
Invoke-Command -ComputerName PC-USER-001 -ScriptBlock {
    Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
}

# Exécuter un script sur plusieurs PC
$computers = @("PC-001", "PC-002", "PC-003")
Invoke-Command -ComputerName $computers -FilePath "C:\Scripts\diagnostic.ps1"

# Copier un fichier vers un PC distant
Copy-Item -Path "C:\Tools\fix.exe" -Destination "\\PC-USER-001\C$\Temp\" -Force
```

---

## 📋 Méthodologie de support à distance

### Le workflow type

```
┌─────────────────────────────────────────────────────────────┐
│              WORKFLOW SUPPORT À DISTANCE                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. RÉCEPTION DE L'APPEL / TICKET                           │
│     │                                                        │
│     ▼                                                        │
│  2. IDENTIFICATION & QUALIFICATION                           │
│     - Qui ? (nom, service, contact)                         │
│     - Quoi ? (problème décrit)                              │
│     - Quand ? (depuis quand)                                │
│     │                                                        │
│     ▼                                                        │
│  3. PRISE EN MAIN À DISTANCE                                │
│     - Demander le consentement                              │
│     - Établir la connexion                                  │
│     - Vérifier l'accès                                      │
│     │                                                        │
│     ▼                                                        │
│  4. DIAGNOSTIC                                               │
│     - Reproduire le problème                                │
│     - Identifier la cause                                   │
│     - Chercher dans la base de connaissances               │
│     │                                                        │
│     ▼                                                        │
│  5. RÉSOLUTION                                               │
│     - Appliquer la solution                                 │
│     - Vérifier avec l'utilisateur                          │
│     - Expliquer ce qui a été fait                          │
│     │                                                        │
│     ▼                                                        │
│  6. CLÔTURE                                                  │
│     - Confirmer la résolution                               │
│     - Documenter le ticket                                  │
│     - Déconnecter proprement                                │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Script de prise en main

**Introduction :**
```
"Bonjour [Nom], c'est [Votre nom] du support informatique.
Je vous appelle concernant votre ticket [numéro].
Pour vous aider, j'aurais besoin de prendre le contrôle
de votre ordinateur à distance. Êtes-vous d'accord ?"

[Si oui]

"Parfait. Je vais vous guider pour établir la connexion.
Pouvez-vous ouvrir [TeamViewer / Quick Assist] ?"
```

**Pendant l'intervention :**
```
"Je suis maintenant connecté à votre PC.
Je vais commencer par vérifier [description].
Vous pouvez suivre mes actions sur votre écran.
N'hésitez pas à me poser des questions."
```

**Clôture :**
```
"Voilà, le problème est résolu.
[Explication simple de ce qui a été fait]
Je vais me déconnecter maintenant.
Si vous avez d'autres questions, n'hésitez pas à nous recontacter.
Bonne journée !"
```

### Bonnes pratiques de sécurité

```
┌─────────────────────────────────────────────────────────────┐
│  🔒 SÉCURITÉ - RÈGLES À RESPECTER                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  AVANT LA CONNEXION :                                        │
│  ✓ Vérifier l'identité de l'utilisateur                     │
│  ✓ Obtenir le consentement explicite                        │
│  ✓ S'assurer que l'utilisateur est présent                  │
│                                                              │
│  PENDANT LA CONNEXION :                                      │
│  ✓ Prévenir avant chaque action importante                  │
│  ✓ Ne pas accéder aux données personnelles                  │
│  ✓ Ne pas lire les emails/documents ouverts                 │
│  ✓ Demander à l'utilisateur de fermer ses documents         │
│                                                              │
│  APRÈS LA CONNEXION :                                        │
│  ✓ Se déconnecter proprement                                │
│  ✓ Informer l'utilisateur de la déconnexion                 │
│  ✓ Ne pas garder les identifiants de connexion             │
│                                                              │
│  ⚠️ INTERDIT :                                               │
│  ✗ Se connecter sans consentement                           │
│  ✗ Rester connecté quand l'utilisateur est absent           │
│  ✗ Enregistrer la session sans autorisation                 │
│  ✗ Accéder à des fichiers non liés au problème             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 📞 Gestion des appels téléphoniques

### Structure d'un appel de support

```
┌─────────────────────────────────────────────────────────────┐
│  STRUCTURE APPEL - 7 ÉTAPES                                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ACCUEIL (30 sec)                                         │
│     "Support IT, [Prénom] bonjour, comment puis-je          │
│      vous aider ?"                                           │
│                                                              │
│  2. IDENTIFICATION (1 min)                                   │
│     - Nom, prénom                                            │
│     - Service/département                                    │
│     - Numéro de poste/asset                                 │
│                                                              │
│  3. ÉCOUTE DU PROBLÈME (2-3 min)                            │
│     - Laisser l'utilisateur expliquer                       │
│     - Ne pas interrompre                                     │
│     - Prendre des notes                                      │
│                                                              │
│  4. REFORMULATION (30 sec)                                   │
│     "Si je comprends bien, vous ne pouvez pas..."           │
│                                                              │
│  5. DIAGNOSTIC/QUESTIONS (2-5 min)                          │
│     - Poser les questions QQOQCP                            │
│     - Demander les messages d'erreur exacts                 │
│                                                              │
│  6. RÉSOLUTION/ESCALADE (variable)                          │
│     - Résoudre si possible                                   │
│     - Ou créer ticket et escalader                          │
│                                                              │
│  7. CLÔTURE (30 sec)                                         │
│     - Confirmer la résolution ou le suivi                   │
│     - Donner le numéro de ticket                            │
│     - Remercier et saluer                                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Guider un utilisateur sans prise en main

Parfois, vous devez guider l'utilisateur sans prendre le contrôle.

**Techniques de guidage :**

```
┌─────────────────────────────────────────────────────────────┐
│  TECHNIQUES DE GUIDAGE TÉLÉPHONIQUE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. UTILISER DES REPÈRES VISUELS                            │
│     ❌ "Cliquez sur le bouton en bas à droite"              │
│     ✅ "Cliquez sur le bouton bleu qui dit 'Envoyer'"       │
│                                                              │
│  2. DONNER UNE ÉTAPE À LA FOIS                              │
│     ❌ "Allez dans Panneau de config > Réseau > Propriétés" │
│     ✅ "D'abord, cliquez sur Démarrer... c'est fait ?"      │
│        "Maintenant, tapez 'panneau de configuration'..."    │
│                                                              │
│  3. VÉRIFIER CHAQUE ÉTAPE                                    │
│     "Vous voyez bien la fenêtre avec [description] ?"       │
│     "Qu'est-ce qui s'affiche sur votre écran ?"             │
│                                                              │
│  4. ÊTRE PATIENT                                             │
│     - Ne pas soupirer                                        │
│     - Répéter si nécessaire                                  │
│     - Reformuler avec d'autres mots                         │
│                                                              │
│  5. UTILISER DES RACCOURCIS                                  │
│     "Appuyez sur la touche Windows, c'est la touche avec    │
│      le logo Windows entre Ctrl et Alt"                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Exemple de guidage - Vider le cache DNS :**

```
Technicien : "Je vais vous guider pour résoudre le problème.
             Êtes-vous devant votre ordinateur ?"

Utilisateur : "Oui"

Technicien : "Parfait. Appuyez sur la touche Windows de votre
             clavier. C'est la touche avec le logo Windows,
             entre Ctrl et Alt, en bas à gauche."

Utilisateur : "D'accord, c'est fait."

Technicien : "Un menu s'est ouvert ? Vous voyez une barre
             de recherche ?"

Utilisateur : "Oui, je la vois."

Technicien : "Tapez les lettres C-M-D, comme Charlie, Michel,
             Daniel. Vous voyez 'Invite de commandes' apparaître ?"

Utilisateur : "Oui, c'est là."

Technicien : "Faites un clic droit dessus, et cliquez sur
             'Exécuter en tant qu'administrateur'."

[...continue étape par étape...]
```

### Gérer les appels difficiles

#### L'utilisateur qui ne comprend pas

```
┌─────────────────────────────────────────────────────────────┐
│  STRATÉGIES - UTILISATEUR PERDU                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Ralentir le débit                                        │
│  2. Utiliser des analogies simples                          │
│  3. Proposer de prendre en main à distance                  │
│  4. Si impossible, planifier intervention sur site          │
│                                                              │
│  PHRASES UTILES :                                            │
│  "Pas de problème, on va y aller doucement"                 │
│  "C'est normal, c'est technique"                            │
│  "Je vais vous montrer plutôt que vous expliquer"           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### L'utilisateur impatient

```
┌─────────────────────────────────────────────────────────────┐
│  STRATÉGIES - UTILISATEUR PRESSÉ                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Reconnaître l'urgence                                    │
│     "Je comprends que c'est urgent"                         │
│                                                              │
│  2. Donner un délai estimé                                  │
│     "Je vais résoudre ça en 5 minutes maximum"              │
│                                                              │
│  3. Agir efficacement                                        │
│     - Moins d'explications, plus d'actions                  │
│     - Questions fermées (oui/non)                           │
│                                                              │
│  4. Si impossible de résoudre rapidement                    │
│     "Pour vous débloquer immédiatement, je vous propose     │
│      [solution temporaire]. Je reviens vers vous dans       │
│      l'heure pour la solution définitive."                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔧 Résolution des incidents courants

### Problème : Mot de passe oublié

```powershell
# Active Directory Users and Computers
# Ou PowerShell :

# Réinitialiser le mot de passe
Set-ADAccountPassword -Identity "jdupont" -Reset -NewPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force)

# Forcer le changement au prochain login
Set-ADUser -Identity "jdupont" -ChangePasswordAtLogon $true

# Débloquer le compte (si verrouillé)
Unlock-ADAccount -Identity "jdupont"

# Vérifier le statut du compte
Get-ADUser -Identity "jdupont" -Properties LockedOut, PasswordLastSet, Enabled
```

**Script téléphonique :**
```
"D'accord, je vais réinitialiser votre mot de passe.
Votre nouveau mot de passe temporaire est [mot de passe].
À votre prochaine connexion, Windows vous demandera
de le changer. Choisissez un mot de passe d'au moins
8 caractères avec des majuscules, minuscules et chiffres."
```

### Problème : Application ne démarre pas

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC - APPLICATION NE DÉMARRE PAS                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ÉTAPE 1 : Vérifier le message d'erreur                     │
│  - Demander le texte exact                                  │
│  - Faire une capture d'écran                                │
│                                                              │
│  ÉTAPE 2 : Tester en mode admin                             │
│  - Clic droit > Exécuter en tant qu'administrateur          │
│                                                              │
│  ÉTAPE 3 : Vérifier le Gestionnaire des tâches              │
│  - L'application est-elle déjà en cours d'exécution ?       │
│  - La terminer et relancer                                  │
│                                                              │
│  ÉTAPE 4 : Vérifier les logs                                │
│  - Observateur d'événements > Applications                  │
│  - Chercher les erreurs récentes                            │
│                                                              │
│  ÉTAPE 5 : Réparer/Réinstaller                              │
│  - Programmes et fonctionnalités > Modifier > Réparer       │
│  - Désinstaller et réinstaller si nécessaire               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

```powershell
# Vérifier si le processus tourne
Get-Process -Name "outlook" -ErrorAction SilentlyContinue

# Terminer le processus
Stop-Process -Name "outlook" -Force

# Vérifier les événements d'erreur
Get-EventLog -LogName Application -EntryType Error -Newest 10

# Réparer Office (exemple)
# Via CMD admin :
# "C:\Program Files\Common Files\microsoft shared\ClickToRun\OfficeC2RClient.exe" /repair
```

### Problème : Lenteur du PC

```powershell
# Diagnostic rapide à distance

# 1. Vérifier l'utilisation CPU/RAM
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, CPU, WorkingSet

# 2. Vérifier l'espace disque
Get-PSDrive C | Select-Object Used, Free, @{n='%Free';e={[math]::Round($_.Free/($_.Used+$_.Free)*100,2)}}

# 3. Vérifier les services qui consomment
Get-Process | Where-Object {$_.CPU -gt 10} | Select-Object Name, CPU, Description

# 4. Vérifier les programmes au démarrage
Get-CimInstance -ClassName Win32_StartupCommand | Select-Object Name, Command, Location

# 5. Nettoyer les fichiers temporaires
Remove-Item -Path "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item -Path "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue
```

### Problème : Pas d'accès aux dossiers partagés

```powershell
# 1. Vérifier la connectivité réseau
Test-Connection -ComputerName "SERVEUR-FICHIERS" -Count 2

# 2. Vérifier les partages accessibles
net view \\SERVEUR-FICHIERS

# 3. Vérifier les lecteurs réseau mappés
Get-PSDrive -PSProvider FileSystem | Where-Object {$_.DisplayRoot -like "\\*"}

# 4. Remapper un lecteur
# Supprimer l'ancien mappage
net use S: /delete

# Recréer le mappage
net use S: \\SERVEUR-FICHIERS\Partage /persistent:yes

# 5. Vérifier les permissions de l'utilisateur
whoami /groups
```

### Problème : Imprimante ne fonctionne pas

```powershell
# 1. Vérifier les imprimantes installées
Get-Printer | Select-Object Name, DriverName, PortName, PrinterStatus

# 2. Vérifier la file d'attente
Get-PrintJob -PrinterName "HP-COMPTA-001"

# 3. Vider la file d'attente
Get-PrintJob -PrinterName "HP-COMPTA-001" | Remove-PrintJob

# 4. Redémarrer le spouleur
Restart-Service -Name Spooler

# 5. Tester l'impression
# Panneau de configuration > Imprimantes > Propriétés > Imprimer page de test
```

### Problème : Outlook ne se synchronise pas

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC OUTLOOK                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. VÉRIFIER LE MODE EN LIGNE/HORS LIGNE                    │
│     - Barre de statut en bas : "Connecté" ou "Hors ligne"   │
│     - Onglet Envoi/Réception > Travailler en mode hors ligne│
│                                                              │
│  2. TESTER LA CONNEXION                                      │
│     - Ctrl + Clic droit sur icône Outlook dans systray      │
│     - "État de la connexion"                                │
│                                                              │
│  3. VIDER LE CACHE OST                                       │
│     - Fichier > Paramètres du compte > Fichiers de données  │
│     - Supprimer le fichier .ost                             │
│     - Redémarrer Outlook                                    │
│                                                              │
│  4. RÉPARER LE PROFIL                                        │
│     - Panneau de config > Mail > Profils                    │
│     - Supprimer et recréer le profil                        │
│                                                              │
│  5. VÉRIFIER AVEC OWA                                        │
│     - Tester sur https://outlook.office.com                 │
│     - Si OWA fonctionne : problème local                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Simulation d'appel

**Objectif :**
Pratiquer la gestion d'un appel de support du début à la fin.

**Scénario :**
L'utilisateur Pierre MARTIN (Comptabilité) appelle : "Je n'arrive plus à ouvrir mes fichiers Excel depuis ce matin. Un message dit que le fichier est corrompu mais c'était OK hier !"

**Consignes :**
1. Rédigez votre script d'accueil
2. Listez les questions à poser
3. Proposez les étapes de diagnostic
4. Rédigez le ticket

<details>
<summary>Cliquez pour voir la solution</summary>

**Script d'accueil :**
```
"Support IT, [Prénom] bonjour !
Comment puis-je vous aider ?

[Écoute du problème]

D'accord M. Martin, je comprends que vos fichiers Excel
ne s'ouvrent plus avec un message d'erreur.
Je vais vous aider à résoudre ça.
Pouvez-vous me donner le nom de votre PC ?"
```

**Questions à poser (QQOQCP) :**
- Quel fichier exactement ? (un seul ou tous ?)
- Où est stocké le fichier ? (local ou serveur ?)
- Le message d'erreur exact ?
- Avez-vous fait une modification hier soir ?
- D'autres collègues ont-ils le même problème ?

**Étapes de diagnostic :**
1. Demander le chemin du fichier
2. Prendre en main à distance
3. Tester l'ouverture d'autres fichiers Excel
4. Vérifier si le fichier est sur un partage réseau
5. Tester l'ouverture avec Excel Online
6. Vérifier s'il y a une version précédente (Shadow Copy)

**Résolution probable :**
- Si fichier réseau : problème de connectivité ou permissions
- Si fichier local : fichier corrompu → restauration
- Si tous les .xlsx : problème Excel → réparation Office

**Ticket :**
```
Titre : Excel - Impossible ouvrir fichier - Erreur corruption
Demandeur : Pierre MARTIN - Comptabilité
Asset : PC-COMPTA-007
Priorité : P3

Description :
L'utilisateur ne peut plus ouvrir son fichier budget_2026.xlsx
Message : "Le fichier est corrompu et ne peut pas être ouvert"
Le fichier fonctionnait hier soir.
Fichier stocké sur : S:\Compta\Budgets\

Diagnostic :
- Autres fichiers Excel OK → problème limité à ce fichier
- Fichier accessible depuis autre PC → pas de problème réseau
- Pas de version précédente disponible

Actions :
- Tentative de réparation fichier : KO
- Recherche de backup : en cours
```

</details>

### Exercice 2 : Guidage téléphonique

**Objectif :**
Guider un utilisateur pour effectuer un flush DNS sans prise en main à distance.

**Scénario :**
Marie DURAND n'arrive plus à accéder au site intranet. Vous avez diagnostiqué un problème de cache DNS et devez la guider pour le vider. Elle n'est pas très à l'aise avec l'informatique.

**Consignes :**
Rédigez le dialogue complet pour guider Marie étape par étape.

<details>
<summary>Cliquez pour voir la solution</summary>

```
Technicien : "Marie, je vais vous guider pour résoudre le
             problème. On va vider ce qu'on appelle le cache
             DNS, c'est une mémoire qui garde les adresses
             des sites. Êtes-vous prête ?"

Marie : "Oui, allez-y."

Technicien : "Regardez en bas à gauche de votre écran.
             Vous voyez la barre avec le bouton Windows ?"

Marie : "Oui, avec le logo Windows."

Technicien : "Parfait. Cliquez une fois sur ce bouton,
             ou appuyez sur la touche Windows de votre clavier."

Marie : "D'accord, un menu s'est ouvert."

Technicien : "Très bien. Maintenant, tapez directement
             les lettres C-M-D. Comme Charlie, Marie, Denis."

Marie : "J'ai tapé, je vois 'Invite de commandes'."

Technicien : "Excellent ! Maintenant, attention c'est important :
             faites un CLIC DROIT dessus, pas un clic gauche,
             un clic DROIT."

Marie : "Clic droit fait, il y a un petit menu."

Technicien : "Dans ce menu, cliquez sur 'Exécuter en tant
             qu'administrateur'."

Marie : "Une fenêtre me demande si j'autorise..."

Technicien : "Cliquez sur 'Oui' pour autoriser."

Marie : "Voilà, une fenêtre noire s'est ouverte."

Technicien : "Parfait ! C'est l'invite de commandes.
             Maintenant, tapez exactement ce que je vais
             vous dire : ipconfig espace /flushdns
             Je répète : i-p-c-o-n-f-i-g, puis un espace,
             puis slash-f-l-u-s-h-d-n-s"

Marie : "ipconfig /flushdns, c'est ça ?"

Technicien : "C'est exactement ça ! Maintenant appuyez
             sur la touche Entrée."

Marie : "Il y a écrit 'Configuration IP de Windows -
         Cache de résolution DNS vidé.'"

Technicien : "Excellent Marie, c'est parfait !
             Le cache est vidé. Vous pouvez fermer cette
             fenêtre noire. Maintenant, réessayez d'accéder
             à l'intranet."

Marie : "Oh, ça marche ! Je vois la page !"

Technicien : "Super ! Le problème est résolu. Si ça se
             reproduit, n'hésitez pas à nous rappeler.
             Bonne journée Marie !"
```

</details>

### Exercice 3 : Diagnostic à distance

**Objectif :**
Diagnostiquer et résoudre un problème via prise en main à distance.

**Scénario :**
Vous êtes connecté en TeamViewer sur le PC de Marc. Il se plaint que "l'ordinateur est très lent depuis quelques jours".

**Consignes :**
1. Listez les vérifications à effectuer (dans l'ordre)
2. Donnez les commandes/actions pour chaque vérification
3. Proposez les solutions selon les résultats

<details>
<summary>Cliquez pour voir la solution</summary>

**Checklist de diagnostic lenteur :**

| Ordre | Vérification | Action | Solution si problème |
|-------|--------------|--------|---------------------|
| 1 | CPU | Task Manager > CPU | Identifier et terminer le processus |
| 2 | RAM | Task Manager > Mémoire | Fermer applications, ajouter RAM |
| 3 | Disque | Task Manager > Disque | Vérifier espace, SSD health |
| 4 | Espace disque | Explorateur > Propriétés C: | Nettoyage disque |
| 5 | Démarrage | Task Manager > Démarrage | Désactiver programmes inutiles |
| 6 | Malware | Windows Security | Scan complet |
| 7 | Mises à jour | Windows Update | Vérifier MAJ en cours |

**Commandes PowerShell :**

```powershell
# 1. Processus qui consomment le plus
Get-Process | Sort-Object CPU -Desc | Select -First 10 Name, CPU, WS

# 2. Utilisation mémoire
Get-Process | Sort-Object WorkingSet -Desc | Select -First 10 Name, @{n='RAM(MB)';e={[math]::Round($_.WorkingSet/1MB)}}

# 3. Espace disque
Get-PSDrive C | Select Used, Free

# 4. Programmes au démarrage
Get-CimInstance Win32_StartupCommand | Select Name, Command

# 5. Température CPU (si outil installé)
# Utiliser HWiNFO ou Core Temp

# 6. Vérifier les services suspects
Get-Service | Where-Object {$_.Status -eq 'Running'} | Sort-Object DisplayName
```

**Actions correctives selon diagnostic :**

| Constat | Action |
|---------|--------|
| CPU 100% par un processus | Terminer le processus, vérifier au redémarrage |
| RAM 95%+ | Fermer applications, vérifier fuite mémoire |
| Disque 95%+ plein | Nettoyage disque, déplacer fichiers |
| Beaucoup de programmes au démarrage | Désactiver les non essentiels |
| Processus suspect | Scan antivirus, vérifier avec Process Explorer |
| Mises à jour Windows en cours | Patienter ou planifier redémarrage |

</details>

---

## 📚 Ressources

### Outils de télémaintenance
- [TeamViewer](https://www.teamviewer.com/) - Prise en main universelle
- [AnyDesk](https://anydesk.com/) - Alternative légère
- [Quick Assist](https://support.microsoft.com/quick-assist) - Intégré Windows

### Documentation Microsoft
- [Remote Desktop](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/)
- [PowerShell Remoting](https://docs.microsoft.com/powershell/scripting/learn/remoting/)

### Tutoriels
- [IT-Connect - Prise en main à distance](https://www.it-connect.fr)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] L'utilisation de TeamViewer / Quick Assist
- [ ] La méthodologie de support à distance (workflow)
- [ ] Les scripts téléphoniques (accueil, guidage, clôture)
- [ ] Le guidage d'un utilisateur sans prise en main
- [ ] Les règles de sécurité (consentement, déconnexion)
- [ ] Le diagnostic à distance (commandes PowerShell)
- [ ] La résolution des incidents courants à distance

---

<div align="center">

**Cours suivant :** [Diagnostic et résolution incidents N1-N3](./04-diagnostic-incidents.md)

[⬅️ Retour au sommaire](./README.md)

</div>
