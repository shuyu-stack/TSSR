# Diagnostic, Traitement et Résolution des Incidents (N1 à N3)

> 📚 **Module :** Maître Emplois - Mission 04
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 10-12 heures
> 🎯 **Niveau :** N1 à N3 (Débutant à Expert)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction](#-introduction)
- [Méthodologie de diagnostic](#-méthodologie-de-diagnostic)
- [Incidents bureautiques](#-incidents-bureautiques)
- [Incidents matériels](#-incidents-matériels)
- [Incidents logiciels](#-incidents-logiciels)
- [Incidents réseau](#-incidents-réseau)
- [Escalade et gestion des priorités](#-escalade-et-gestion-des-priorités)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Appliquer une méthodologie de diagnostic structurée
- ✅ Diagnostiquer et résoudre les incidents bureautiques (Office, navigateurs)
- ✅ Identifier et traiter les pannes matérielles
- ✅ Résoudre les problèmes logiciels Windows
- ✅ Diagnostiquer les incidents réseau
- ✅ Escalader correctement vers les niveaux supérieurs
- ✅ Utiliser les outils de diagnostic professionnels

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Maîtriser les cours 01 à 03
- [ ] Connaître l'architecture PC (composants)
- [ ] Comprendre le fonctionnement de Windows
- [ ] Avoir des bases solides en réseau TCP/IP

**Matériel nécessaire :**
- 💻 PC de test avec Windows 10/11
- 🔧 Outils de diagnostic (Sysinternals, etc.)
- 🌐 Accès à un environnement Active Directory

---

## 📚 Introduction

### Les niveaux de support (ITIL)

```
┌─────────────────────────────────────────────────────────────┐
│                 PYRAMIDE DU SUPPORT IT                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│                        ┌─────┐                               │
│                        │ N3  │  Experts / Ingénieurs        │
│                        │     │  (Architecture, Dev, Sécu)    │
│                     ┌──┴─────┴──┐                            │
│                     │    N2     │  Techniciens avancés       │
│                     │           │  (Admin sys, réseau)       │
│                  ┌──┴───────────┴──┐                         │
│                  │       N1        │  Support premier niveau │
│                  │                 │  (Help Desk, kiosque)   │
│               ┌──┴─────────────────┴──┐                      │
│               │      UTILISATEURS      │                     │
│               └────────────────────────┘                     │
│                                                              │
│  N1 : 70% des incidents résolus                             │
│  N2 : 20% des incidents résolus                             │
│  N3 : 10% des incidents résolus                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Compétences par niveau

| Niveau | Responsabilités | Exemples d'incidents |
|--------|-----------------|---------------------|
| **N1** | Premier contact, diagnostic initial, résolution simple | Reset mdp, déblocage compte, redémarrage, config imprimante |
| **N2** | Diagnostic approfondi, résolution complexe, admin système | Réinstallation OS, GPO, config réseau, scripts |
| **N3** | Expertise technique, architecture, développement | Infrastructure serveur, sécurité, bugs applicatifs |

---

## 🔍 Méthodologie de diagnostic

### La méthode STAR

```
┌─────────────────────────────────────────────────────────────┐
│                    MÉTHODE S.T.A.R.                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  S - SYMPTÔME                                                │
│      → Que se passe-t-il exactement ?                       │
│      → Quel message d'erreur ?                              │
│      → Depuis quand ?                                        │
│                                                              │
│  T - TEST                                                    │
│      → Reproduire le problème                               │
│      → Tester les hypothèses                                │
│      → Isoler la cause                                      │
│                                                              │
│  A - ACTION                                                  │
│      → Appliquer la solution                                │
│      → Documenter les changements                           │
│      → Vérifier le résultat                                 │
│                                                              │
│  R - RÉSULTAT                                                │
│      → Le problème est-il résolu ?                          │
│      → L'utilisateur est-il satisfait ?                     │
│      → Documentation de la solution                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Les questions de diagnostic

```
┌─────────────────────────────────────────────────────────────┐
│  🔍 QUESTIONS CLÉS - TOUT INCIDENT                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  TEMPORALITÉ :                                               │
│  • Depuis quand le problème existe-t-il ?                   │
│  • Le problème est-il permanent ou intermittent ?           │
│  • Qu'est-ce qui a changé récemment ?                       │
│                                                              │
│  CONTEXTE :                                                  │
│  • Quelle action déclenchait le problème ?                  │
│  • D'autres utilisateurs ont-ils le même problème ?         │
│  • Le problème se produit-il sur un autre PC ?              │
│                                                              │
│  DÉTAILS TECHNIQUES :                                        │
│  • Quel est le message d'erreur exact ?                     │
│  • Avez-vous une capture d'écran ?                          │
│  • Avez-vous essayé de redémarrer ?                         │
│                                                              │
│  IMPACT :                                                    │
│  • Pouvez-vous travailler autrement ?                       │
│  • Quelle est l'urgence ?                                   │
│  • Combien de personnes sont impactées ?                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Le processus de résolution

```
         ┌──────────────────┐
         │   INCIDENT       │
         │   signalé        │
         └────────┬─────────┘
                  │
         ┌────────▼─────────┐
         │  QUALIFICATION   │
         │  (priorité,      │
         │   catégorie)     │
         └────────┬─────────┘
                  │
         ┌────────▼─────────┐     ┌─────────────────┐
         │   DIAGNOSTIC     │────▶│ Base de         │
         │   (cause racine) │◀────│ connaissances   │
         └────────┬─────────┘     └─────────────────┘
                  │
        ┌─────────┴─────────┐
        │                   │
 ┌──────▼──────┐    ┌───────▼───────┐
 │  SOLUTION   │    │   ESCALADE    │
 │  connue     │    │   N2/N3       │
 └──────┬──────┘    └───────┬───────┘
        │                   │
        └─────────┬─────────┘
                  │
         ┌────────▼─────────┐
         │  APPLICATION     │
         │  solution        │
         └────────┬─────────┘
                  │
         ┌────────▼─────────┐
         │  VÉRIFICATION    │
         │  (avec user)     │
         └────────┬─────────┘
                  │
         ┌────────▼─────────┐
         │  DOCUMENTATION   │
         │  & CLÔTURE       │
         └──────────────────┘
```

---

## 📄 Incidents bureautiques

### Microsoft Office - Problèmes courants

#### Word/Excel ne répond plus

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC - OFFICE NE RÉPOND PLUS                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  NIVEAU N1 :                                                 │
│  1. Forcer la fermeture (Task Manager)                      │
│  2. Relancer l'application                                  │
│  3. Désactiver les compléments                              │
│     Fichier > Options > Compléments > Gérer COM Add-ins     │
│                                                              │
│  NIVEAU N2 :                                                 │
│  1. Réparer Office                                           │
│     Panneau config > Programmes > Office > Modifier > Réparer│
│  2. Supprimer le profil Office                              │
│     %appdata%\Microsoft\[Word|Excel]                        │
│  3. Réinstaller Office                                       │
│                                                              │
│  NIVEAU N3 :                                                 │
│  1. Analyser les logs Office                                │
│  2. Vérifier les conflits DLL                               │
│  3. Escalade Microsoft si bug confirmé                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

```powershell
# Réparer Office en ligne de commande
# Office Click-to-Run
& "C:\Program Files\Common Files\microsoft shared\ClickToRun\OfficeC2RClient.exe" /repair

# Réparer Office MSI
# Via Panneau de configuration

# Désactiver les compléments COM (registre)
# HKEY_CURRENT_USER\Software\Microsoft\Office\[Version]\[App]\Resiliency\DisabledItems
```

#### Outlook - Problèmes de profil

```
┌─────────────────────────────────────────────────────────────┐
│  OUTLOOK - ARBRE DE DÉCISION                                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│                 Outlook ne démarre pas                       │
│                         │                                    │
│          ┌──────────────┼──────────────┐                    │
│          │              │              │                    │
│     Mode sans      Message        Crash                     │
│      échec        d'erreur       immédiat                   │
│          │              │              │                    │
│          ▼              ▼              ▼                    │
│     Démarrer      Identifier      Réparer                   │
│     outlook.exe   l'erreur        profil                    │
│     /safe              │              │                     │
│          │              │              │                    │
│     Désactiver    Chercher        Supprimer                 │
│     add-ins       dans KB         fichier .ost              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Commandes Outlook utiles :**

```powershell
# Démarrer Outlook en mode sans échec
outlook.exe /safe

# Créer un nouveau profil Outlook
outlook.exe /profiles

# Réparer le fichier PST/OST
# Outil : scanpst.exe
# Emplacement : C:\Program Files\Microsoft Office\root\Office16\

# Supprimer le cache Outlook
Remove-Item "$env:LOCALAPPDATA\Microsoft\Outlook\*.ost" -Force

# Réinitialiser le volet de navigation
outlook.exe /resetnavpane

# Nettoyer les formulaires personnalisés
outlook.exe /cleanfreebusy
```

### Navigateurs web

#### Chrome/Edge ne fonctionne plus

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC NAVIGATEUR                                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ÉTAPE 1 : Test en navigation privée                        │
│  - Chrome : Ctrl + Shift + N                                │
│  - Edge : Ctrl + Shift + N                                  │
│  → Si OK en privé : problème d'extension ou cache           │
│                                                              │
│  ÉTAPE 2 : Désactiver les extensions                        │
│  - chrome://extensions ou edge://extensions                 │
│  - Désactiver toutes les extensions                         │
│  - Réactiver une par une                                    │
│                                                              │
│  ÉTAPE 3 : Vider le cache                                   │
│  - Ctrl + Shift + Suppr                                     │
│  - Sélectionner "Toutes les données"                        │
│                                                              │
│  ÉTAPE 4 : Réinitialiser le navigateur                      │
│  - Paramètres > Réinitialiser les paramètres               │
│                                                              │
│  ÉTAPE 5 : Réinstaller                                       │
│  - Désinstaller complètement                                │
│  - Supprimer le profil utilisateur                          │
│  - Réinstaller                                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

```powershell
# Chemin profil Chrome
# %LOCALAPPDATA%\Google\Chrome\User Data\Default

# Chemin profil Edge
# %LOCALAPPDATA%\Microsoft\Edge\User Data\Default

# Réinstaller Chrome silencieusement
# Télécharger et exécuter ChromeSetup.exe

# Effacer le cache DNS du navigateur
# Dans la barre d'adresse : chrome://net-internals/#dns → Clear host cache
```

### Adobe Reader / PDF

```
┌─────────────────────────────────────────────────────────────┐
│  PDF NE S'OUVRE PAS                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  VÉRIFICATIONS :                                             │
│  1. Le fichier PDF est-il corrompu ?                        │
│     → Tester avec un autre PDF                              │
│                                                              │
│  2. Adobe Reader est-il à jour ?                            │
│     → Aide > Rechercher les mises à jour                    │
│                                                              │
│  3. Mode protégé activé ?                                   │
│     → Edition > Préférences > Sécurité (Avancé)            │
│     → Désactiver "Mode protégé au démarrage"               │
│                                                              │
│  4. Réinitialiser les préférences                           │
│     → Supprimer le dossier %appdata%\Adobe\Acrobat\         │
│                                                              │
│  5. Réparer l'installation                                   │
│     → Aide > Réparer l'installation                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔧 Incidents matériels

### Diagnostic hardware systématique

```
┌─────────────────────────────────────────────────────────────┐
│  CHECKLIST DIAGNOSTIC MATÉRIEL                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ⚡ ALIMENTATION                                             │
│  □ PC branché à une prise fonctionnelle ?                   │
│  □ Câble d'alimentation en bon état ?                       │
│  □ Interrupteur I/O sur I ?                                 │
│  □ LED alimentation allumée ?                               │
│  □ Ventilateur alimentation tourne ?                        │
│                                                              │
│  🖥️ AFFICHAGE                                                │
│  □ Écran allumé (LED) ?                                     │
│  □ Câble vidéo bien connecté des deux côtés ?               │
│  □ Bon port vidéo sélectionné sur l'écran ?                 │
│  □ Test avec autre câble/écran ?                            │
│                                                              │
│  💾 STOCKAGE                                                 │
│  □ Disque dur détecté dans le BIOS ?                        │
│  □ Bruits anormaux (cliquetis) ?                            │
│  □ LED d'activité disque ?                                  │
│  □ Test SMART positif ?                                     │
│                                                              │
│  🧠 MÉMOIRE                                                  │
│  □ Barrettes bien insérées (clips) ?                        │
│  □ Barrettes dans les bons slots ?                          │
│  □ Test avec une seule barrette ?                           │
│  □ Test MemTest86 ?                                         │
│                                                              │
│  🌡️ TEMPÉRATURE                                              │
│  □ Ventilateurs fonctionnels ?                              │
│  □ Poussière excessive ?                                    │
│  □ Pâte thermique OK (si vieux PC) ?                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Pannes courantes et solutions

#### PC ne démarre pas du tout

| Symptôme | Cause probable | Solution |
|----------|----------------|----------|
| Aucune LED, aucun son | Alimentation HS ou non branchée | Vérifier câble, tester autre prise, changer alim |
| LED allumée mais pas de boot | RAM mal insérée ou HS | Réinsérer/tester RAM |
| Beeps au démarrage | Voir codes beep BIOS | Consulter doc constructeur |
| Ventilateurs tournent puis arrêt | Court-circuit ou surchauffe | Vérifier connexions, nettoyer |

#### Écran noir

```
┌─────────────────────────────────────────────────────────────┐
│  ÉCRAN NOIR - DIAGNOSTIC                                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ÉCRAN ÉTEINT (pas de LED)                               │
│     → Vérifier alimentation écran                           │
│     → Tester autre prise électrique                         │
│                                                              │
│  2. ÉCRAN ALLUMÉ MAIS NOIR                                  │
│     → Vérifier câble vidéo                                  │
│     → Tester autre entrée (HDMI, VGA, DP)                   │
│     → Tester avec autre écran                               │
│                                                              │
│  3. PC DÉMARRE MAIS ÉCRAN NOIR                              │
│     → Écouter les beeps POST                                │
│     → Vérifier carte graphique insérée                      │
│     → Tester sortie vidéo intégrée (si applicable)          │
│                                                              │
│  4. WINDOWS DÉMARRE PUIS ÉCRAN NOIR                         │
│     → Mode sans échec                                        │
│     → Désinstaller driver graphique                         │
│     → Restauration système                                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### Surchauffe

```powershell
# Vérifier la température (PowerShell + WMI)
Get-WmiObject MSAcpi_ThermalZoneTemperature -Namespace "root/wmi" |
    Select-Object @{n='Temp °C';e={($_.CurrentTemperature/10)-273.15}}

# Outils recommandés :
# - HWiNFO (gratuit)
# - Core Temp
# - SpeedFan
# - Open Hardware Monitor
```

**Seuils de température CPU :**
| État | Température | Action |
|------|-------------|--------|
| Normal | < 60°C | OK |
| Élevé | 60-80°C | Surveiller |
| Critique | > 80°C | Nettoyer, vérifier ventilation |
| Danger | > 95°C | Arrêt immédiat, intervention |

#### Disque dur défaillant

```powershell
# Vérifier l'état SMART
wmic diskdrive get status

# Détails SMART avec PowerShell
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, OperationalStatus, HealthStatus

# Vérifier les erreurs disque
Get-WinEvent -FilterHashtable @{LogName='System'; ProviderName='disk'} -MaxEvents 10

# Lancer une vérification
chkdsk C: /f /r

# Outil : CrystalDiskInfo pour analyse SMART détaillée
```

**Signes d'un disque mourant :**
- Bruits de cliquetis ou grincements
- Lenteurs anormales
- Fichiers corrompus fréquents
- Erreurs SMART (Reallocated Sectors, etc.)
- Écrans bleus KERNEL_DATA_INPAGE_ERROR

---

## 💻 Incidents logiciels

### Windows - Problèmes courants

#### Écrans bleus (BSOD)

```
┌─────────────────────────────────────────────────────────────┐
│  ANALYSE BSOD - MÉTHODOLOGIE                                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ÉTAPE 1 : Collecter les informations                       │
│  - Code erreur (ex: IRQL_NOT_LESS_OR_EQUAL)                │
│  - Fichier incriminé (si indiqué)                          │
│  - Circonstances (action avant le crash)                   │
│                                                              │
│  ÉTAPE 2 : Analyser le minidump                            │
│  - Emplacement : C:\Windows\Minidump\                      │
│  - Outil : BlueScreenView ou WinDbg                        │
│                                                              │
│  ÉTAPE 3 : Identifier la cause                             │
│  - Driver défectueux (80% des cas)                         │
│  - RAM défectueuse                                          │
│  - Disque dur HS                                            │
│  - Malware                                                   │
│                                                              │
│  ÉTAPE 4 : Appliquer la solution                           │
│  - Mettre à jour/rollback driver                           │
│  - Test mémoire (mdsched)                                  │
│  - Scan antivirus                                          │
│  - Restauration système                                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Codes BSOD courants :**

| Code | Cause probable | Solution |
|------|----------------|----------|
| IRQL_NOT_LESS_OR_EQUAL | Driver défectueux | MAJ ou rollback driver |
| PAGE_FAULT_IN_NONPAGED_AREA | RAM ou driver | Test RAM, MAJ drivers |
| KERNEL_DATA_INPAGE_ERROR | Disque dur | chkdsk, vérifier SMART |
| SYSTEM_SERVICE_EXCEPTION | Driver ou Windows corrompu | sfc /scannow, DISM |
| DRIVER_IRQL_NOT_LESS_OR_EQUAL | Driver spécifique | Identifier et MAJ |
| CRITICAL_PROCESS_DIED | Process système crashé | Restauration, réinstall |

```powershell
# Réparer les fichiers système Windows
sfc /scannow

# Si sfc échoue, réparer l'image Windows
DISM /Online /Cleanup-Image /RestoreHealth

# Puis relancer sfc
sfc /scannow

# Vérifier l'intégrité de Windows
DISM /Online /Cleanup-Image /CheckHealth
```

#### Windows Update bloqué

```powershell
# Arrêter les services Windows Update
Stop-Service -Name wuauserv, cryptSvc, bits, msiserver -Force

# Renommer les dossiers de cache
Rename-Item "C:\Windows\SoftwareDistribution" "SoftwareDistribution.old"
Rename-Item "C:\Windows\System32\catroot2" "catroot2.old"

# Redémarrer les services
Start-Service -Name wuauserv, cryptSvc, bits, msiserver

# Forcer la détection des mises à jour
wuauclt /detectnow

# Ou via PowerShell (Windows 10+)
Install-Module PSWindowsUpdate -Force
Get-WindowsUpdate
Install-WindowsUpdate -AcceptAll
```

#### Problèmes de profil utilisateur

```
┌─────────────────────────────────────────────────────────────┐
│  PROFIL UTILISATEUR CORROMPU                                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  SYMPTÔMES :                                                 │
│  - Connexion avec profil temporaire                         │
│  - "Nous ne pouvons pas vous connecter avec ce compte"      │
│  - Paramètres perdus à chaque connexion                     │
│                                                              │
│  SOLUTION N1 - Redémarrage                                  │
│  1. Redémarrer le PC                                         │
│  2. Essayer de se reconnecter                               │
│                                                              │
│  SOLUTION N2 - Réparation registre                          │
│  1. Se connecter avec un autre admin                        │
│  2. regedit → HKLM\SOFTWARE\Microsoft\Windows NT\           │
│     CurrentVersion\ProfileList                               │
│  3. Trouver le SID de l'utilisateur                         │
│  4. Supprimer le .bak si présent                            │
│  5. Vérifier ProfileImagePath                               │
│                                                              │
│  SOLUTION N3 - Nouveau profil                               │
│  1. Renommer C:\Users\[user] → C:\Users\[user].old          │
│  2. Se reconnecter (nouveau profil créé)                    │
│  3. Migrer les données de l'ancien profil                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Malwares et virus

```
┌─────────────────────────────────────────────────────────────┐
│  DÉTECTION ET SUPPRESSION MALWARE                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  SIGNES D'INFECTION :                                        │
│  • PC très lent sans raison                                 │
│  • Pop-ups publicitaires                                    │
│  • Programme inconnu au démarrage                           │
│  • Page d'accueil navigateur modifiée                       │
│  • Antivirus désactivé                                      │
│  • Fichiers chiffrés (ransomware)                          │
│                                                              │
│  PROCÉDURE DE NETTOYAGE :                                   │
│  1. Déconnecter du réseau                                   │
│  2. Mode sans échec avec prise en charge réseau            │
│  3. Scan Windows Defender hors ligne                        │
│  4. Scan Malwarebytes                                       │
│  5. Scan ADWCleaner (adwares)                               │
│  6. Vérifier les programmes installés                       │
│  7. Vérifier les extensions navigateur                      │
│  8. Vérifier le démarrage (msconfig)                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

```powershell
# Scan Windows Defender rapide
Start-MpScan -ScanType QuickScan

# Scan Windows Defender complet
Start-MpScan -ScanType FullScan

# Mettre à jour les définitions
Update-MpSignature

# Scan hors ligne (redémarre le PC)
Start-MpWDOScan
```

---

## 🌐 Incidents réseau

### Diagnostic réseau systématique

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC RÉSEAU - COUCHE PAR COUCHE                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  COUCHE 1 - PHYSIQUE                                         │
│  □ Câble branché ?                                          │
│  □ LED carte réseau active ?                                │
│  □ Câble en bon état ?                                      │
│                                                              │
│  COUCHE 2 - LIAISON                                          │
│  □ Driver carte réseau OK ?                                 │
│  □ Carte réseau activée ?                                   │
│                                                              │
│  COUCHE 3 - RÉSEAU                                           │
│  □ Adresse IP valide (pas 169.254.x.x) ?                   │
│  □ Masque et passerelle corrects ?                         │
│  □ Ping passerelle OK ?                                     │
│  □ Ping externe OK (8.8.8.8) ?                             │
│                                                              │
│  COUCHE 4-7 - TRANSPORT/APPLICATION                          │
│  □ DNS résout les noms ?                                    │
│  □ Ports non bloqués (firewall) ?                          │
│  □ Services accessibles ?                                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Commandes de diagnostic réseau

```powershell
# === DIAGNOSTIC COMPLET RÉSEAU ===

# 1. Configuration IP
ipconfig /all

# 2. Test connectivité locale
ping 127.0.0.1

# 3. Test passerelle
ping 192.168.1.1  # Adapter selon le réseau

# 4. Test Internet
ping 8.8.8.8

# 5. Test résolution DNS
nslookup google.fr
Resolve-DnsName google.fr

# 6. Trace du chemin réseau
tracert 8.8.8.8
Test-NetConnection -ComputerName google.fr -TraceRoute

# 7. Vérifier les connexions actives
netstat -ano

# 8. Test de port spécifique
Test-NetConnection -ComputerName serveur -Port 443

# 9. Informations carte réseau
Get-NetAdapter | Select-Object Name, Status, LinkSpeed

# 10. Réinitialisation réseau
ipconfig /release
ipconfig /renew
ipconfig /flushdns
netsh winsock reset
netsh int ip reset
# Redémarrer après
```

### Problèmes réseau courants

#### Pas d'adresse IP (APIPA)

```
┌─────────────────────────────────────────────────────────────┐
│  ADRESSE 169.254.x.x (APIPA)                                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  SIGNIFICATION :                                             │
│  Le PC n'a pas reçu d'IP du serveur DHCP                    │
│                                                              │
│  CAUSES POSSIBLES :                                          │
│  • Câble réseau débranché/défectueux                        │
│  • Serveur DHCP hors service                                │
│  • Port switch désactivé                                    │
│  • VLAN incorrect                                           │
│                                                              │
│  DIAGNOSTIC :                                                │
│  1. Vérifier le câble réseau                                │
│  2. Tester sur un autre port switch                         │
│  3. Vérifier si autres PC ont le même problème              │
│  4. Contacter équipe réseau si DHCP down                    │
│                                                              │
│  SOLUTION TEMPORAIRE :                                       │
│  Configurer une IP statique si autorisé                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### DNS ne résout pas

```powershell
# Vider le cache DNS
ipconfig /flushdns

# Vérifier les serveurs DNS configurés
Get-DnsClientServerAddress

# Tester la résolution DNS
nslookup intranet.entreprise.local
nslookup google.fr 8.8.8.8  # Test avec DNS Google

# Changer les serveurs DNS temporairement
Set-DnsClientServerAddress -InterfaceIndex 12 -ServerAddresses ("8.8.8.8","8.8.4.4")

# Voir le cache DNS local
Get-DnsClientCache
```

#### VPN ne se connecte pas

```
┌─────────────────────────────────────────────────────────────┐
│  VPN - DIAGNOSTIC                                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  VÉRIFICATIONS :                                             │
│  □ Connexion Internet fonctionnelle ?                       │
│  □ Identifiants VPN corrects ?                              │
│  □ Certificat VPN valide (pas expiré) ?                     │
│  □ Logiciel VPN à jour ?                                    │
│  □ Firewall/antivirus ne bloque pas ?                       │
│  □ Ports VPN ouverts (ex: 443, 1194, 500) ?                │
│                                                              │
│  TESTS :                                                     │
│  1. Ping du serveur VPN                                      │
│  2. Telnet vers le port VPN                                 │
│  3. Test depuis une autre connexion (4G mobile)             │
│                                                              │
│  SOLUTIONS :                                                 │
│  • Réinstaller le client VPN                                │
│  • Supprimer et recréer la connexion VPN                    │
│  • Vérifier avec l'équipe sécurité/réseau                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## ⬆️ Escalade et gestion des priorités

### Matrice d'escalade

```
┌─────────────────────────────────────────────────────────────┐
│  QUAND ESCALADER ?                                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ESCALADE N1 → N2 :                                          │
│  • Diagnostic > 15 min sans avancer                         │
│  • Nécessite droits admin domaine                           │
│  • Installation/réinstallation OS                           │
│  • Configuration serveur nécessaire                         │
│  • Problème récurrent non résolu                            │
│                                                              │
│  ESCALADE N2 → N3 :                                          │
│  • Problème infrastructure (serveur, réseau core)           │
│  • Bug applicatif nécessitant développement                 │
│  • Incident de sécurité                                     │
│  • Problème architectural                                   │
│  • Nécessite intervention fournisseur                       │
│                                                              │
│  ESCALADE → MANAGEMENT :                                     │
│  • Incident critique impactant l'entreprise                 │
│  • Utilisateur VIP mécontent                                │
│  • Besoin de décision budgétaire                           │
│  • SLA en danger de violation                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Rédiger une bonne escalade

```
┌─────────────────────────────────────────────────────────────┐
│  TEMPLATE D'ESCALADE                                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  TICKET : INC0012345                                         │
│  PRIORITÉ : P2                                               │
│  UTILISATEUR : Jean DUPONT - Direction                      │
│                                                              │
│  PROBLÈME :                                                  │
│  [Description claire et concise du problème]                │
│                                                              │
│  DIAGNOSTIC EFFECTUÉ :                                       │
│  1. [Action 1] → Résultat                                   │
│  2. [Action 2] → Résultat                                   │
│  3. [Action 3] → Résultat                                   │
│                                                              │
│  HYPOTHÈSE :                                                 │
│  [Votre analyse de la cause probable]                       │
│                                                              │
│  RAISON DE L'ESCALADE :                                      │
│  [Pourquoi N2/N3 est nécessaire]                            │
│                                                              │
│  IMPACT :                                                    │
│  [Conséquences si non résolu]                               │
│                                                              │
│  SUGGESTION :                                                │
│  [Votre proposition de solution si vous en avez une]        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Diagnostic BSOD

**Scénario :**
Un utilisateur a des écrans bleus réguliers depuis 3 jours. Le code erreur est "DRIVER_IRQL_NOT_LESS_OR_EQUAL".

**Consignes :**
1. Quelles questions posez-vous à l'utilisateur ?
2. Quels outils utilisez-vous pour analyser ?
3. Quelle est la cause probable ?
4. Quelle solution proposez-vous ?

<details>
<summary>Cliquez pour voir la solution</summary>

**Questions à poser :**
- Qu'avez-vous installé ou mis à jour récemment ?
- Le problème se produit-il lors d'une action particulière ?
- Avez-vous ajouté du matériel ?
- Le PC a-t-il eu une mise à jour Windows ?

**Outils d'analyse :**
- BlueScreenView pour analyser les minidumps
- Event Viewer pour voir les erreurs
- Driver Verifier pour identifier le driver fautif
- Windows Update History

**Cause probable :**
DRIVER_IRQL_NOT_LESS_OR_EQUAL → Un driver tente d'accéder à une mémoire paginée à un niveau IRQL trop élevé. Généralement un driver défectueux ou incompatible.

**Solution :**
```powershell
# 1. Analyser avec BlueScreenView - identifier le driver
# 2. Mettre à jour le driver incriminé
# Via Gestionnaire de périphériques > Driver > Mettre à jour

# 3. Ou rollback si mise à jour récente
# Gestionnaire de périphériques > Propriétés > Pilote > Version précédente

# 4. Si problème persiste
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth

# 5. Vérifier les mises à jour Windows
Get-WindowsUpdate
```

</details>

### Exercice 2 : Lenteur système

**Scénario :**
"Mon PC est devenu très lent depuis la semaine dernière. Tout met plusieurs minutes à s'ouvrir."

**Consignes :**
1. Listez les vérifications à effectuer (dans l'ordre)
2. Donnez les commandes pour chaque vérification
3. Proposez les solutions selon les résultats

<details>
<summary>Cliquez pour voir la solution</summary>

**Checklist de diagnostic :**

| Ordre | Vérification | Commande/Outil | Solution si problème |
|-------|--------------|----------------|---------------------|
| 1 | Utilisation CPU | Task Manager / Get-Process | Identifier et terminer le processus |
| 2 | Utilisation RAM | Task Manager | Fermer apps, augmenter RAM |
| 3 | Utilisation disque | Task Manager | Attendre ou terminer le processus |
| 4 | Espace disque | Get-PSDrive C | Nettoyage disque |
| 5 | Programmes démarrage | Task Manager > Démarrage | Désactiver les inutiles |
| 6 | Malware | Windows Defender | Scan complet |
| 7 | Température | HWiNFO | Nettoyer la poussière |
| 8 | Santé disque | CrystalDiskInfo | Remplacer si warning |

**Commandes de diagnostic :**

```powershell
# 1. Top processus CPU
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, @{n='RAM(MB)';e={[math]::Round($_.WS/1MB)}}

# 2. Espace disque
Get-PSDrive C | Select-Object @{n='Used(GB)';e={[math]::Round($_.Used/1GB)}}, @{n='Free(GB)';e={[math]::Round($_.Free/1GB)}}

# 3. Programmes au démarrage
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command | Format-Table -AutoSize

# 4. Services en cours
Get-Service | Where-Object {$_.Status -eq 'Running'} | Measure-Object

# 5. Vérifier Windows Update en cours
Get-WindowsUpdateLog

# 6. Scan antivirus
Start-MpScan -ScanType QuickScan
```

</details>

### Exercice 3 : Pas de réseau

**Scénario :**
"Je n'ai plus accès au réseau. Impossible d'accéder aux dossiers partagés ni à Internet."

**Consignes :**
Rédigez la procédure complète de diagnostic et résolution.

<details>
<summary>Cliquez pour voir la solution</summary>

```
PROCÉDURE DIAGNOSTIC RÉSEAU

1. VÉRIFICATION PHYSIQUE
   □ Câble réseau branché des deux côtés
   □ LED verte/orange sur la carte réseau
   □ Tester avec un autre câble si possible

2. CONFIGURATION IP
   > ipconfig /all

   Vérifier :
   □ Adresse IP valide (pas 169.254.x.x)
   □ Masque de sous-réseau correct
   □ Passerelle par défaut renseignée
   □ Serveurs DNS renseignés

3. TESTS DE CONNECTIVITÉ
   > ping 127.0.0.1        ← Test carte réseau
   > ping [IP passerelle]  ← Test réseau local
   > ping 8.8.8.8          ← Test Internet
   > ping google.fr        ← Test DNS

4. RÉSULTATS ET ACTIONS

   Si ping 127.0.0.1 échoue :
   → Problème carte réseau
   → Vérifier driver, réactiver la carte

   Si ping passerelle échoue :
   → Problème réseau local
   → Vérifier câble, brassage, switch

   Si ping 8.8.8.8 échoue mais passerelle OK :
   → Problème routeur/Internet
   → Contacter équipe réseau

   Si ping Google échoue mais 8.8.8.8 OK :
   → Problème DNS
   → ipconfig /flushdns
   → Vérifier config DNS

5. RÉINITIALISATION COMPLÈTE
   > ipconfig /release
   > ipconfig /renew
   > ipconfig /flushdns
   > netsh winsock reset
   > netsh int ip reset
   > Redémarrer le PC

6. SI TOUJOURS KO
   → Escalade N2 réseau
   → Vérifier le port switch
   → Vérifier le DHCP
   → Vérifier les VLANs
```

</details>

---

## 📚 Ressources

### Outils de diagnostic essentiels

| Outil | Usage | Lien |
|-------|-------|------|
| **Sysinternals Suite** | Diagnostic avancé Windows | [Microsoft](https://docs.microsoft.com/sysinternals/) |
| **BlueScreenView** | Analyse BSOD | [Nirsoft](https://www.nirsoft.net/utils/blue_screen_view.html) |
| **CrystalDiskInfo** | Santé disque SMART | [Crystal](https://crystalmark.info/) |
| **HWiNFO** | Monitoring hardware | [HWiNFO](https://www.hwinfo.com/) |
| **Wireshark** | Analyse réseau | [Wireshark](https://www.wireshark.org/) |
| **Process Monitor** | Surveillance processus | [Microsoft](https://docs.microsoft.com/sysinternals/downloads/procmon) |

### Documentation
- [Microsoft Troubleshooting](https://docs.microsoft.com/troubleshoot/)
- [IT-Connect](https://www.it-connect.fr)

---

## ✅ Checklist de révision

Avant de passer au module suivant, assurez-vous de maîtriser :

- [ ] La méthodologie STAR de diagnostic
- [ ] Les outils Sysinternals (Process Monitor, etc.)
- [ ] L'analyse des écrans bleus (BSOD)
- [ ] Le diagnostic matériel (alimentation, RAM, disque)
- [ ] La résolution des problèmes Office courants
- [ ] Le diagnostic réseau couche par couche
- [ ] Les critères et méthode d'escalade

---

<div align="center">

**Cours suivant :** [Déploiement et configuration des postes de travail](./05-deploiement-postes.md)

[⬅️ Retour au sommaire](./README.md)

</div>
