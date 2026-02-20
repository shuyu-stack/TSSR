# Support Windows Client 10/11 - Niveau 1 et 2

> 📚 **Module :** Windows Server - Support utilisateur
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 6-8 heures
> 🎯 **Niveau :** Fondamental (CRITIQUE pour emploi)
> 🎓 **Formateur virtuel :** Architecte réseau avec +20 ans d'expérience
> 💼 **Focus :** Ce que vous ferez 80% du temps en premier poste

---

## 👨‍🏫 Message de votre formateur

> **Soyons honnêtes : votre premier job de TSSR, c'est 80% de support utilisateur.**
>
> En 20 ans, j'ai formé des dizaines de juniors. Tous pensent qu'ils vont faire de l'administration système avancée dès le premier jour. La réalité ?
>
> **Vos 6 premiers mois = répondre au téléphone et dépanner des postes Windows.**
>
> Ce n'est PAS dégradant. C'est ESSENTIEL. C'est comme ça qu'on apprend :
> - ✅ Comment communiquer avec des utilisateurs (qui ne parlent pas technique)
> - ✅ Les problèmes réels du terrain (pas les exercices d'école)
> - ✅ La gestion du stress (5 tickets en attente, téléphone qui sonne)
> - ✅ La méthodologie de diagnostic (étape par étape)
>
> **Les tickets les plus fréquents (statistique personnelle sur 10 000 tickets) :**
> - 30% : "Mon PC est lent"
> - 25% : "Je n'ai plus internet/réseau"
> - 15% : "Mon imprimante ne marche pas"
> - 10% : "J'ai oublié mon mot de passe"
> - 10% : "Mon logiciel plante"
> - 10% : Divers (écran noir, périphériques USB, etc.)
>
> **Si vous maîtrisez ce cours, vous résoudrez 90% des tickets en moins de 10 minutes.**
>
> C'est ce qui fait la différence entre :
> - Un technicien qui galère et met 1 heure par ticket
> - Un technicien efficace qui ferme 20 tickets par jour
>
> **À l'examen TSSR, il y a 80% de chances** d'avoir une mise en situation de support (dépanner un problème Windows).

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Partie 1 : Méthodologie de support](#-partie-1--méthodologie-de-support)
- [Partie 2 : Les 10 problèmes les plus fréquents](#-partie-2--les-10-problèmes-les-plus-fréquents)
- [Partie 3 : Outils de diagnostic essentiels](#-partie-3--outils-de-diagnostic-essentiels)
- [Partie 4 : Dépannage avancé](#-partie-4--dépannage-avancé)
- [Partie 5 : Communication avec l'utilisateur](#-partie-5--communication-avec-lutilisateur)
- [Partie 6 : Gestion des tickets](#-partie-6--gestion-des-tickets)
- [Cas pratiques réels](#-cas-pratiques-réels)
- [Checklist de diagnostic](#-checklist-de-diagnostic)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Appliquer** une méthodologie de diagnostic structurée
- ✅ **Résoudre** les 10 problèmes Windows les plus fréquents en moins de 10 minutes
- ✅ **Utiliser** les outils de diagnostic Windows (Event Viewer, Task Manager, etc.)
- ✅ **Communiquer** efficacement avec un utilisateur non technique
- ✅ **Documenter** correctement un ticket de support
- ✅ **Escalader** un ticket au niveau 2/3 quand nécessaire
- ✅ **Gérer** le stress et les priorités (plusieurs tickets simultanés)
- ✅ **Réussir** la mise en situation de support à l'examen TSSR

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [x] Connaître les bases de Windows (interface, fichiers/dossiers)
- [x] Avoir suivi les cours DNS/DHCP et Active Directory
- [x] Savoir ouvrir une invite de commandes
- [ ] *Recommandé :* Avoir fait du support utilisateur (stage, alternance)

**Matériel nécessaire :**
- 💻 Poste Windows 10 ou 11 (VM ou physique)
- 🎧 Casque pour simuler un appel téléphonique (exercices)
- 📝 Carnet de notes pour votre "knowledge base" personnelle
- ☕ Patience et empathie !

---

## 🔷 Partie 1 : Méthodologie de support

### La règle d'or du support : ÉCOUTEZ d'abord

**Erreur classique du technicien junior :**
```
Utilisateur : "Mon PC ne marche plus"
Technicien junior : "Redémarre ton PC" (sans écouter)
Résultat : Le problème n'est pas résolu, utilisateur frustré
```

**Bon réflexe du technicien confirmé :**
```
Utilisateur : "Mon PC ne marche plus"
Technicien : "OK, je vais vous aider. Qu'est-ce qui ne fonctionne pas exactement ?"
Utilisateur : "Je ne peux pas imprimer"
Technicien : (pose les bonnes questions, diagnostique, résout en 5 min)
```

### La méthodologie en 6 étapes (ÉCIRDT)

Utilisez TOUJOURS cette méthodologie. C'est celle enseignée dans toutes les formations professionnelles.

```
┌─────────────────────────────────────────────────────────┐
│  1. ÉCOUTER    - Laisser l'utilisateur expliquer        │
├─────────────────────────────────────────────────────────┤
│  2. CLARIFIER  - Poser des questions précises           │
├─────────────────────────────────────────────────────────┤
│  3. IDENTIFIER - Reproduire/comprendre le problème      │
├─────────────────────────────────────────────────────────┤
│  4. RÉSOUDRE   - Appliquer la solution                  │
├─────────────────────────────────────────────────────────┤
│  5. DOCUMENTER - Noter dans le ticket                   │
├─────────────────────────────────────────────────────────┤
│  6. TESTER     - Vérifier que ça marche vraiment        │
└─────────────────────────────────────────────────────────┘
```

#### 1. ÉCOUTER (2 minutes)

**Ce qu'il faut faire :**
- Laisser l'utilisateur expliquer le problème SANS l'interrompre
- Prendre des notes
- Montrer de l'empathie : "Je comprends, c'est frustrant"

**Questions à poser :**
- "Depuis quand avez-vous ce problème ?"
- "Est-ce que ça marchait avant ?"
- "Qu'est-ce qui a changé récemment ?" (mise à jour, nouvel équipement, etc.)
- "Est-ce que vos collègues ont le même problème ?"

#### 2. CLARIFIER (2 minutes)

**Transformer le langage utilisateur en langage technique :**

| L'utilisateur dit | Ce qu'il veut vraiment dire |
|-------------------|----------------------------|
| "Mon PC est cassé" | Écran noir ? Pas d'internet ? Logiciel planté ? |
| "Internet ne marche pas" | Navigateur ? Outlook ? Tout ? |
| "Mon PC est lent" | Lent au démarrage ? Lent en utilisation ? Applications lentes ? |
| "J'ai un virus" | Fenêtres pub ? Antivirus alerté ? PC qui rame ? |

**Posez des questions fermées (oui/non) :**
- "Est-ce que vous avez un message d'erreur ?"
- "Est-ce que vous pouvez ouvrir d'autres programmes ?"
- "Est-ce que vous avez redémarré le PC ?"

#### 3. IDENTIFIER (5 minutes)

**Reproduire le problème :**
- Si possible, reproduisez le problème AVEC l'utilisateur (partage d'écran, accès distant)
- Si vous ne pouvez pas reproduire, demandez à l'utilisateur de refaire devant vous

**Isoler la cause :**
```
Problème : "Je ne peux pas accéder au serveur de fichiers"

Test 1 : Pouvez-vous accéder à internet ? → Oui
→ Le réseau local fonctionne, ce n'est pas un problème de câble

Test 2 : Pouvez-vous pinger le serveur (ping 192.168.1.10) ? → Non
→ Problème de réseau ou serveur

Test 3 : Est-ce que vos collègues ont le même problème ? → Non
→ Problème spécifique à ce poste

Conclusion : Problème de configuration réseau sur ce poste
```

#### 4. RÉSOUDRE (10 minutes max)

**Règle des 10 minutes :**
- Si vous ne trouvez pas en 10 minutes → ESCALADEZ au niveau 2
- Ne perdez pas 2 heures sur un ticket alors que 5 autres attendent

**Appliquez la solution :**
- Expliquez ce que vous faites à l'utilisateur (pédagogie)
- Faites-le participer si possible (il apprendra pour la prochaine fois)

#### 5. DOCUMENTER (2 minutes)

**Dans le ticket, notez TOUJOURS :**
- Symptôme initial
- Diagnostic posé
- Solution appliquée
- Résultat (résolu ou escaladé)

**Exemple de bonne documentation :**
```
Ticket #12345
Utilisateur : Marie MARTIN (mmartin)
Symptôme : Impossible d'imprimer (erreur "Imprimante hors ligne")
Diagnostic : Pilote d'imprimante corrompu
Solution : Désinstallation et réinstallation du pilote HP LaserJet
Résultat : ✅ Résolu - Utilisateur peut imprimer
Temps : 8 minutes
```

#### 6. TESTER (2 minutes)

**NE JAMAIS fermer un ticket sans vérifier :**
- Demandez à l'utilisateur de tester DEVANT VOUS
- "Pouvez-vous essayer d'imprimer maintenant ?"
- Attendez la confirmation : "Oui, ça marche !"
- Seulement APRÈS, fermez le ticket

> ⚠️ **Piège classique :**
> Technicien : "Normalement ça devrait marcher, je ferme le ticket"
> 2 heures plus tard : L'utilisateur rappelle "Ça ne marche toujours pas"
> → Ticket rouvert, utilisateur en colère, temps perdu

### Quand escalader au niveau 2 ?

**Escaladez SI :**
- Vous ne trouvez pas en 10 minutes
- Le problème nécessite des droits admin que vous n'avez pas
- Le problème touche plusieurs utilisateurs (incident majeur)
- Le problème nécessite une intervention physique (matériel défectueux)
- Vous n'êtes pas sûr de votre solution (risque de casser quelque chose)

**N'escaladez PAS si :**
- Vous n'avez pas encore appliqué la méthodologie de base
- Vous n'avez pas fait redémarrer le PC (90% des problèmes se résolvent avec un redémarrage)
- Vous n'avez pas vérifié les câbles/connexions

---

## 🔷 Partie 2 : Les 10 problèmes les plus fréquents

Voici les 10 tickets que vous traiterez EN BOUCLE en support N1/N2, avec la solution rapide.

### 🥇 Problème #1 : "Mon PC est lent" (30% des tickets)

**Causes fréquentes (par ordre) :**
1. Disque dur plein (90% d'utilisation)
2. Trop de programmes au démarrage
3. Malware/virus
4. Mise à jour Windows en cours
5. Disque dur défectueux (HDD vieillissant)

**Diagnostic rapide (2 minutes) :**

1. **Ouvrir le Gestionnaire des tâches** (Ctrl + Shift + Esc)
   - Onglet **Performance**
   - Vérifiez :
     - CPU : > 80% en permanence ? → Problème
     - Mémoire : > 90% ? → Problème
     - Disque : > 90% en permanence ? → Problème (souvent la cause N°1)

2. **Vérifier l'espace disque**
   - Ouvrir **Ce PC** (Explorateur)
   - Disque C: rouge (< 10% libre) ? → **BINGO, c'est ça !**

**Solution (5 minutes) :**

**Si disque plein :**
```
1. Nettoyage de disque
   - Appuyer sur Windows, taper "disk cleanup"
   - Sélectionner C:
   - Cocher toutes les cases (surtout "Windows Update Cleanup")
   - OK → Supprimer les fichiers

2. Vider la corbeille

3. Supprimer les fichiers temporaires manuellement
   - Windows + R → taper %temp% → Entrée
   - Ctrl + A (tout sélectionner)
   - Supprimer (certains ne pourront pas, c'est normal)

Gain typique : 5-20 Go
```

**Si trop de programmes au démarrage :**
```
1. Gestionnaire des tâches → Onglet "Démarrage" (Startup)
2. Identifier les programmes inutiles (Spotify, Skype, Teams s'ils ne sont pas nécessaires)
3. Clic droit → "Désactiver"
4. Redémarrer le PC
```

**Si mise à jour Windows en cours :**
```
Symptôme : Disque à 100%, processus "Windows Update"
Solution : Attendre (ça peut prendre 30 min - 1h)
Dire à l'utilisateur : "Windows installe des mises à jour, c'est normal. Laissez le PC allumé, ça va se terminer."
```

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Vérifier l'espace disque rapidement
> Get-PSDrive C | Select-Object Used,Free
>
> # Nettoyer les fichiers temporaires
> Remove-Item -Path "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue
>
> # Lister les programmes au démarrage
> Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location
> ```

---

### 🥈 Problème #2 : "Je n'ai plus internet" (25% des tickets)

**Diagnostic en 30 secondes :**

**Étape 1 : Icône réseau dans la barre des tâches**
- ❌ Croix rouge ou globe : Pas de connexion réseau
- ⚠️ Triangle jaune : Connecté mais pas d'internet
- ✅ Icône normale : Problème ailleurs (DNS, proxy, navigateur)

**Étape 2 : Ping en cascade**

```cmd
# Test 1 : Connectivité locale
ping 127.0.0.1
→ Si ça ne marche pas : Carte réseau HS (très rare)

# Test 2 : Passerelle
ping 192.168.1.1
→ Si ça ne marche pas : Problème réseau local (câble, switch)

# Test 3 : DNS
ping 8.8.8.8
→ Si ça ne marche pas : Problème de routage/internet

# Test 4 : Résolution DNS
ping google.com
→ Si ça ne marche pas mais Test 3 OK : Problème DNS
```

**Solutions selon le diagnostic :**

**Cas 1 : Croix rouge (pas de connexion)**
```
1. Vérifier le câble réseau (débrancher/rebrancher)
2. Vérifier que le WiFi est activé (touches Fn + F2 selon le PC)
3. Redémarrer la carte réseau :
   - Panneau de configuration → Centre Réseau et partage
   - Modifier les paramètres de la carte
   - Clic droit sur la carte → Désactiver
   - Clic droit → Activer
```

**Cas 2 : Triangle jaune (connecté mais pas d'internet)**
```
Cause fréquente : Problème DHCP ou DNS

Solution :
1. Libérer/renouveler l'IP
   ipconfig /release
   ipconfig /renew

2. Vider le cache DNS
   ipconfig /flushdns

3. Redémarrer le PC

4. Si ça ne marche toujours pas → Vérifier le serveur DHCP (niveau 2)
```

**Cas 3 : Seulement certains sites ne marchent pas**
```
Cause : Problème DNS

Solution :
1. Changer les DNS pour utiliser Google DNS
   - Panneau de configuration → Centre Réseau et partage
   - Modifier les paramètres de la carte
   - Clic droit sur la carte → Propriétés
   - Double-clic sur "Protocole Internet version 4 (TCP/IPv4)"
   - Cocher "Utiliser l'adresse de serveur DNS suivante"
   - DNS préféré : 8.8.8.8
   - DNS auxiliaire : 8.8.4.4
   - OK

2. Vider le cache DNS
   ipconfig /flushdns

3. Tester
```

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Diagnostic réseau automatique
> Test-NetConnection google.com -InformationLevel Detailed
>
> # Redémarrer la carte réseau
> Restart-NetAdapter -Name "Ethernet"
>
> # Changer les DNS en PowerShell
> Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("8.8.8.8","8.8.4.4")
> ```

---

### 🥉 Problème #3 : "Mon imprimante ne marche pas" (15% des tickets)

**Causes fréquentes :**
1. Imprimante "hors ligne" (offline)
2. Bourrage papier
3. Cartouche vide
4. Pilote corrompu
5. Câble/réseau déconnecté

**Diagnostic (1 minute) :**

```
1. Vérifier dans Paramètres → Imprimantes et scanners
   - L'imprimante apparaît ?
   - Statut : "Prête" ou "Hors ligne" ?

2. Vérifier sur l'imprimante physique
   - Écran d'erreur ?
   - Voyant rouge allumé ?
   - Papier coincé ?
```

**Solutions :**

**Cas 1 : Imprimante "hors ligne"**
```
1. Panneau de configuration → Périphériques et imprimantes
2. Clic droit sur l'imprimante → "Voir les travaux d'impression en cours"
3. Menu "Imprimante" → Décocher "Utiliser l'imprimante hors connexion"
4. Si ça ne marche pas :
   - Clic droit sur l'imprimante → "Supprimer les travaux d'impression"
   - Redémarrer le spouleur d'impression :
     services.msc → "Spouleur d'impression" → Redémarrer
```

**Cas 2 : Pilote corrompu**
```
1. Désinstaller l'imprimante
   - Paramètres → Imprimantes et scanners
   - Clic sur l'imprimante → Supprimer

2. Réinstaller
   - "Ajouter une imprimante"
   - Si réseau : Chercher automatiquement
   - Si USB : Brancher le câble, Windows installe automatiquement

3. Tester une page de test
```

**Cas 3 : Imprimante réseau introuvable**
```
1. Vérifier que le serveur d'impression est accessible
   ping [IP du serveur d'impression]

2. Vérifier que le partage est accessible
   \\NOM-SERVEUR\NOM-IMPRIMANTE

3. Si ça ne marche pas :
   - Problème DNS (voir Problème #2)
   - Problème de permissions (escalade niveau 2)
```

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Lister les imprimantes installées
> Get-Printer
>
> # Redémarrer le spouleur d'impression
> Restart-Service Spooler
>
> # Supprimer tous les travaux d'impression
> Stop-Service Spooler
> Remove-Item -Path "C:\Windows\System32\spool\PRINTERS\*" -Force
> Start-Service Spooler
>
> # Installer une imprimante réseau
> Add-Printer -ConnectionName "\\SERVEUR\Imprimante-RDC"
> ```

---

### Problème #4 : "J'ai oublié mon mot de passe" (10% des tickets)

**Solution (2 minutes) :**

**Si environnement Active Directory :**
```
1. Ouvrir "Active Directory Users and Computers" (dsa.msc)
2. Chercher l'utilisateur (Ctrl + F)
3. Clic droit → "Réinitialiser le mot de passe" (Reset Password)
4. Entrer un nouveau mot de passe temporaire (ex: "Temp2026!")
5. Cocher "L'utilisateur doit changer le mot de passe à la prochaine ouverture de session"
6. OK
7. Communiquer le mot de passe temporaire à l'utilisateur (téléphone, en personne)
8. L'utilisateur se connecte et choisit un nouveau mot de passe
```

> ⚠️ **Sécurité :**
> - Ne JAMAIS envoyer un mot de passe par email
> - Toujours vérifier l'identité de l'utilisateur avant de réinitialiser
> - Noter dans le ticket : "Identité vérifiée par téléphone" ou "En personne"

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Réinitialiser le mot de passe d'un utilisateur
> Set-ADAccountPassword -Identity "jdupont" -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "Temp2026!" -Force)
>
> # Forcer le changement au prochain login
> Set-ADUser -Identity "jdupont" -ChangePasswordAtLogon $true
>
> # Débloquer un compte verrouillé
> Unlock-ADAccount -Identity "jdupont"
> ```

---

### Problème #5 : "Mon logiciel plante" (10% des tickets)

**Diagnostic (2 minutes) :**

```
1. Quel logiciel ? (Word, Excel, navigateur, etc.)
2. Quand plante-t-il ? (au démarrage, à l'utilisation, aléatoirement)
3. Message d'erreur ?
4. Est-ce que ça marche sur un autre PC ?
```

**Solutions :**

**Cas 1 : Application ne démarre pas**
```
1. Redémarrer le PC (vraiment, ça résout 50% des cas)

2. Vérifier si le processus est déjà en cours
   - Gestionnaire des tâches → Onglet "Détails"
   - Chercher le processus (ex: WINWORD.EXE pour Word)
   - Si présent : Clic droit → "Fin de tâche"
   - Relancer l'application

3. Vérifier les logs Windows (Event Viewer)
   - eventvwr.msc
   - Journaux Windows → Application
   - Chercher des erreurs récentes liées au logiciel
```

**Cas 2 : Application plante aléatoirement**
```
1. Désinstaller et réinstaller l'application
   - Paramètres → Applications
   - Chercher l'application → Désinstaller
   - Réinstaller depuis la source (CD, réseau, internet)

2. Mettre à jour Windows
   - Paramètres → Mise à jour et sécurité
   - Rechercher des mises à jour
   - Installer tout

3. Si ça persiste : Créer un nouveau profil utilisateur (niveau 2)
```

**Cas 3 : Application Office plante (Word, Excel, Outlook)**
```
Solution spécifique Office : Réparer Office

1. Panneau de configuration → Programmes et fonctionnalités
2. Chercher "Microsoft Office"
3. Clic droit → "Modifier"
4. Sélectionner "Réparation rapide" (Quick Repair)
5. Cliquer "Réparer"
6. Si ça ne marche toujours pas : "Réparation en ligne" (Online Repair)
```

---

### Problème #6 : "Écran noir au démarrage" (5% des tickets)

**Causes :**
- Problème de carte graphique
- Problème de mise à jour Windows
- Corruption du profil utilisateur
- Disque dur défectueux

**Diagnostic :**

```
Étape 1 : L'écran est-il vraiment noir ?
- Voyants du PC allumés ? → PC démarre
- Écran complètement noir ou logo Windows puis noir ?

Étape 2 : Tester si Windows tourne en arrière-plan
- Appuyer sur Ctrl + Alt + Suppr
- Si vous voyez l'écran de connexion → Windows tourne, c'est un problème d'affichage

Étape 3 : Démarrer en mode sans échec
- Redémarrer le PC
- Appuyer sur F8 (ou Shift + F8) pendant le démarrage
- Sélectionner "Mode sans échec"
```

**Solutions :**

**Si mode sans échec fonctionne :**
```
1. Désinstaller les derniers pilotes de carte graphique
   - Gestionnaire de périphériques (devmgmt.msc)
   - Cartes graphiques → Clic droit → Désinstaller

2. Désinstaller les dernières mises à jour Windows
   - Panneau de configuration → Programmes → Afficher les mises à jour installées
   - Désinstaller la dernière mise à jour (KB...)

3. Redémarrer en mode normal
```

**Si mode sans échec ne fonctionne pas :**
```
→ Problème matériel ou corruption Windows
→ ESCALADE niveau 2 (réinstallation Windows, changement disque dur)
```

---

### Problème #7 : "Impossible de se connecter au domaine"

**Symptôme :** Message "Le service d'ouverture de session ne peut pas ouvrir la session utilisateur"

**Causes :**
- PC sorti du domaine
- Compte utilisateur verrouillé
- Problème DNS
- Contrôleur de domaine injoignable

**Diagnostic (2 minutes) :**

```cmd
# Vérifier si le PC est toujours dans le domaine
systeminfo | findstr /B "Domaine"

# Résultat attendu : "Domaine: SOLARIS.local"
# Si "Groupe de travail: WORKGROUP" → PC sorti du domaine
```

**Solutions :**

**Cas 1 : Compte verrouillé**
```
Sur le serveur AD :
1. Active Directory Users and Computers
2. Chercher l'utilisateur
3. Onglet "Compte" → Décocher "Le compte est verrouillé"
4. OK
```

**Cas 2 : PC sorti du domaine**
```
1. Reconnecter le PC au domaine (nécessite droits admin local)
   - Paramètres → Système → Informations système
   - "Paramètres système avancés"
   - Onglet "Nom de l'ordinateur" → Modifier
   - Cocher "Domaine" → Taper "SOLARIS.local"
   - OK → Entrer identifiants admin domaine
   - Redémarrer

→ Si vous n'avez pas les droits : ESCALADE niveau 2
```

**Cas 3 : Problème DNS**
```
Vérifier que le PC utilise le bon serveur DNS :

ipconfig /all

DNS Servers doit pointer vers le contrôleur de domaine (ex: 192.168.1.10)

Si ce n'est pas le cas :
- Corriger manuellement dans les paramètres réseau
- Ou vérifier le serveur DHCP (options DHCP)
```

---

### Problème #8 : "Mon lecteur réseau a disparu"

**Causes :**
- Serveur de fichiers inaccessible
- GPO de mappage pas appliquée
- Identifiants expirés

**Diagnostic rapide :**

```cmd
# Tester l'accès au partage réseau
net use

# Lister les lecteurs mappés actuels
# Si le lecteur n'apparaît pas → Pas mappé

# Tester l'accès au serveur
ping srv-fichiers.solaris.local

# Tester l'accès au partage
dir \\srv-fichiers\commun
```

**Solutions :**

**Cas 1 : Mapper manuellement (temporaire)**
```
1. Explorateur → Clic droit sur "Ce PC" → "Connecter un lecteur réseau"
2. Lecteur : P:
3. Dossier : \\SRV-FICHIERS\Commun
4. Cocher "Se reconnecter à l'ouverture de session"
5. Terminer
```

**Cas 2 : Forcer l'application de la GPO (permanent)**
```cmd
gpupdate /force
```

Déconnectez-vous et reconnectez-vous.

Le lecteur devrait réapparaître.

---

### Problème #9 : "Mon PC redémarre tout seul"

**Causes :**
- Mise à jour Windows automatique
- Surchauffe
- Alimentation défectueuse
- Malware

**Diagnostic :**

```
1. Vérifier les logs système
   - eventvwr.msc
   - Journaux Windows → Système
   - Chercher les événements "Kernel-Power" ou "Arrêt inattendu"

2. Vérifier les mises à jour Windows
   - Paramètres → Mise à jour et sécurité → Historique des mises à jour
   - Si des mises à jour se sont installées récemment → C'est la cause
```

**Solutions :**

**Cas 1 : Mises à jour Windows forcées**
```
Configurer les heures d'activité :
1. Paramètres → Mise à jour et sécurité
2. Modifier les heures d'activité
3. Définir 8h-18h (heures de travail)
→ Windows ne redémarrera plus pendant ces heures
```

**Cas 2 : Surchauffe**
```
1. Vérifier la température du CPU
   - Gestionnaire des tâches → Performance → CPU → Température (si affiché)
   - Ou utiliser un logiciel : HWMonitor, Core Temp

2. Si > 80°C en utilisation normale → Surchauffe
   - Nettoyer les ventilateurs (poussière)
   - Vérifier que les ventilateurs tournent
   - Si PC portable : Utiliser un support ventilé

→ Si problème persiste : ESCALADE (changement pâte thermique, ventilateur HS)
```

---

### Problème #10 : "J'ai supprimé un fichier par erreur"

**Solution selon l'endroit de suppression :**

**Cas 1 : Fichier sur le PC local**
```
1. Vérifier la corbeille
   - Ouvrir la Corbeille (icône bureau)
   - Chercher le fichier
   - Clic droit → Restaurer

2. Si la corbeille est vide : Utiliser un logiciel de récupération
   - Recuva (gratuit) : https://www.ccleaner.com/recuva
   - Installer → Scanner le disque → Récupérer

⚠️ Taux de réussite : 50-70% (si le fichier n'a pas été écrasé)
```

**Cas 2 : Fichier sur un partage réseau (serveur)**
```
1. Vérifier les versions précédentes (Shadow Copy)
   - Clic droit sur le DOSSIER contenant le fichier
   - Propriétés → Onglet "Versions précédentes"
   - Sélectionner une version d'hier ou avant-hier
   - Ouvrir → Récupérer le fichier

2. Si pas de versions précédentes : Contacter l'admin pour restauration de sauvegarde
```

**Cas 3 : Email supprimé (Outlook)**
```
1. Vérifier les "Éléments supprimés"
2. Si vide : Vérifier "Éléments récupérables"
   - Outlook → Onglet "Dossier" → "Récupérer des éléments supprimés"
   - Sélectionner l'email → Récupérer
```

---

## 🔷 Partie 3 : Outils de diagnostic essentiels

### 1. Gestionnaire des tâches (Task Manager) - L'outil N°1

**Ouvrir :** Ctrl + Shift + Esc

**Onglets importants :**

#### Onglet "Processus"
- Voir les applications en cours
- Identifier les processus qui consomment beaucoup (tri par CPU, Mémoire, Disque)
- Tuer un processus bloqué (Fin de tâche)

#### Onglet "Performance"
- CPU : Utilisation en %
- Mémoire : Utilisée / Totale
- **Disque : SI > 90% en permanence = PC lent**
- Réseau : Transfert de données

#### Onglet "Démarrage"
- Programmes qui se lancent au démarrage de Windows
- Désactiver les programmes inutiles pour accélérer le démarrage

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Lister les processus qui consomment le plus de CPU
> Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
>
> # Lister les processus qui consomment le plus de mémoire
> Get-Process | Sort-Object WS -Descending | Select-Object -First 10
>
> # Tuer un processus
> Stop-Process -Name "chrome" -Force
> ```

---

### 2. Observateur d'événements (Event Viewer)

**Ouvrir :** eventvwr.msc

**Utilité :** Voir les logs système (erreurs, avertissements, informations)

**Journaux importants :**

#### Journaux Windows → Système
- Erreurs matérielles
- Problèmes de drivers
- Arrêts inattendus

#### Journaux Windows → Application
- Plantages d'applications
- Erreurs logicielles

**Comment l'utiliser :**
1. Ouvrir Event Viewer
2. Journaux Windows → Système (ou Application)
3. Trier par "Niveau" (Level) → Erreurs en premier
4. Double-clic sur une erreur pour voir les détails
5. Googler le code d'erreur (Event ID)

**Exemple :**
```
Event ID: 41
Source: Kernel-Power
Description: "Le système a redémarré sans s'arrêter correctement au préalable"
→ Arrêt brutal (problème électrique, surchauffe, ou plantage)
```

---

### 3. Moniteur de ressources (Resource Monitor)

**Ouvrir :** resmon.exe

**Utilité :** Diagnostic avancé (plus détaillé que Task Manager)

- Voir EXACTEMENT quel processus utilise le disque/réseau
- Identifier les fichiers verrouillés (impossible à supprimer/déplacer)
- Voir les connexions réseau actives

---

### 4. Informations système

**Ouvrir :** msinfo32.exe

**Utilité :** Voir toutes les infos matérielles et logicielles du PC

- Modèle du PC
- Version de Windows (Build)
- RAM installée
- Type de BIOS (UEFI ou Legacy)
- Pilotes installés

**Pratique pour :**
- Vérifier la compatibilité d'un logiciel
- Identifier le modèle exact du PC (pour télécharger des pilotes)

---

### 5. Gestionnaire de périphériques (Device Manager)

**Ouvrir :** devmgmt.msc

**Utilité :** Gérer les drivers (pilotes)

**Point d'exclamation jaune (⚠️) sur un périphérique :**
→ Driver manquant ou défectueux

**Solution :**
1. Clic droit sur le périphérique → "Mettre à jour le pilote"
2. Si ça ne marche pas : Désinstaller → Redémarrer (Windows réinstalle automatiquement)

---

### 6. Vérificateur de fichiers système (SFC)

**Utilité :** Réparer les fichiers Windows corrompus

**Commande :**
```cmd
sfc /scannow
```

**Durée :** 15-30 minutes

**Quand l'utiliser :**
- Après un plantage
- Erreurs système bizarres
- Applications Windows ne se lancent pas

**Résultat :**
- "Windows n'a détecté aucune violation d'intégrité" → Tout est OK
- "La protection des ressources Windows a détecté des fichiers corrompus et les a réparés" → Problème résolu
- "La protection des ressources Windows a détecté des fichiers corrompus mais n'a pas pu les réparer" → Problème grave → Réinstallation Windows

---

### 7. Check Disk (CHKDSK)

**Utilité :** Vérifier et réparer les erreurs du disque dur

**Commande :**
```cmd
chkdsk C: /f /r
```

- `/f` : Corrige les erreurs
- `/r` : Localise les secteurs défectueux et récupère les données

**⚠️ Important :** Cette commande nécessite un redémarrage (le scan se fait au démarrage)

**Quand l'utiliser :**
- Fichiers corrompus fréquemment
- Messages d'erreur sur le disque
- PC très lent (disque peut-être défectueux)

---

### 8. Outils réseau essentiels (déjà vus)

```cmd
ipconfig /all        # Config réseau complète
ipconfig /release    # Libérer l'IP DHCP
ipconfig /renew      # Renouveler l'IP DHCP
ipconfig /flushdns   # Vider le cache DNS

ping google.com      # Test connectivité
tracert google.com   # Tracer le chemin réseau
nslookup google.com  # Test résolution DNS

netstat -an          # Voir les connexions actives
```

---

## 🔷 Partie 5 : Communication avec l'utilisateur

### Les 3 règles d'or de la communication

#### Règle #1 : Parler SIMPLE

**❌ Mauvais :**
"Votre stack TCP/IP est corrompue, je vais flush le cache ARP et reinitialiser le Winsock."

**✅ Bon :**
"Votre connexion réseau a un problème, je vais la réinitialiser. Ça prendra 2 minutes."

#### Règle #2 : Montrer de l'EMPATHIE

**❌ Mauvais :**
Utilisateur : "Mon PC ne marche plus, je dois envoyer un rapport urgent !"
Technicien : "Redémarre ton PC."

**✅ Bon :**
"Je comprends, c'est urgent. Je vais vous aider tout de suite. On va commencer par redémarrer le PC, ça résout souvent le problème rapidement."

#### Règle #3 : INFORMER et RASSURER

Pendant que vous travaillez, expliquez ce que vous faites :
- "Je vérifie votre connexion réseau..."
- "Je regarde les logs pour identifier la cause..."
- "Parfait, j'ai trouvé le problème, je vais le corriger maintenant."

**Ça évite le silence gênant et rassure l'utilisateur.**

---

### Gérer les utilisateurs difficiles

#### Type 1 : L'utilisateur agressif

**Symptôme :** "C'est inadmissible ! Ça fait 3 fois que j'appelle ! Vous êtes incompétents !"

**Réaction :**
1. **Rester calme** (ne PAS le prendre personnellement)
2. **Laisser évacuer** (laissez-le s'exprimer 30 secondes)
3. **Montrer de l'empathie** : "Je comprends votre frustration, c'est normal d'être en colère."
4. **Proposer une solution** : "Je vais tout faire pour résoudre votre problème maintenant."

#### Type 2 : L'utilisateur qui pense tout savoir

**Symptôme :** "J'ai déjà tout essayé, j'ai regardé sur internet, c'est sûrement un virus."

**Réaction :**
1. **Valoriser** : "Très bien, vous avez déjà fait les vérifications de base, c'est parfait."
2. **Proposer d'aller plus loin** : "Je vais faire quelques tests supplémentaires pour identifier la cause exacte."
3. **Impliquer** : "Pouvez-vous me décrire exactement ce que vous avez déjà essayé ?"

#### Type 3 : L'utilisateur perdu

**Symptôme :** "Je ne comprends rien à l'informatique, je ne sais pas où cliquer..."

**Réaction :**
1. **Rassurer** : "Pas de souci, je vais vous guider étape par étape."
2. **Donner des instructions ULTRA simples** : "En bas à gauche de votre écran, vous voyez le logo Windows ? Cliquez dessus."
3. **Être patient** (ça peut prendre plus de temps)

---

## 🔷 Partie 6 : Gestion des tickets

### Anatomie d'un bon ticket

**Informations obligatoires :**
- Numéro de ticket
- Utilisateur (nom, email, téléphone)
- Date et heure d'ouverture
- **Symptôme** (ce que l'utilisateur a décrit)
- **Diagnostic** (ce que vous avez identifié)
- **Solution** (ce que vous avez fait)
- Statut (Ouvert, En cours, Résolu, Fermé)
- Temps passé

**Exemple de ticket EXCELLENT :**
```
Ticket #12567
Utilisateur : Sophie DURAND (sdurand@solaris.local - 01 23 45 67 89)
Ouvert le : 12/01/2026 14:35
Catégorie : Réseau / Connectivité

Symptôme :
Utilisatrice ne peut plus accéder aux partages réseau (lecteurs P: et K: absents).
Le problème a commencé ce matin après une mise à jour Windows.

Diagnostic :
GPO de mappage de lecteurs non appliquée suite à la mise à jour Windows.
Commande gpresult confirme que la GPO "GPO - Mappage Lecteurs" n'apparaît pas.

Solution appliquée :
1. gpupdate /force (échec - erreur accès DC)
2. Vérification DNS : OK (ping dc1.solaris.local OK)
3. Reconnexion session : Déconnexion / Reconnexion
4. gpupdate /force (succès)
5. Lecteurs P: et K: réapparus

Résultat : ✅ Résolu
Temps passé : 12 minutes
Statut : Fermé

Remarques : Problème lié à la mise à jour Windows KB5034441 (plusieurs cas similaires aujourd'hui).
Peut nécessiter un correctif au niveau GPO si ça persiste.
```

### Priorisation des tickets (méthode ITIL)

| Priorité | Impact | Urgence | Exemple | Délai |
|----------|--------|---------|---------|-------|
| **P1 - Critique** | Haute | Haute | Serveur de fichiers HS (50 personnes bloquées) | < 1h |
| **P2 - Haute** | Haute | Moyenne | Imprimante principale HS (10 personnes) | < 4h |
| **P3 - Moyenne** | Moyenne | Moyenne | PC lent (1 personne) | < 1 jour |
| **P4 - Basse** | Basse | Basse | Demande de logiciel non urgent | < 1 semaine |

**Règle :** Traiter TOUJOURS les P1 en premier, même si vous avez 10 tickets P3 en attente.

---

## 💼 Cas pratiques réels

### Cas #1 : L'incident du lundi matin (scénario examen)

**Contexte :**
Lundi matin 9h. Vous arrivez au bureau. 15 tickets ouverts ce week-end. Votre téléphone sonne.

**Appel 1 (9h05) :**
"Bonjour, c'est Marie de la compta. Mon PC ne démarre plus, j'ai un écran noir. J'ai une réunion dans 30 minutes et j'ai besoin de mes fichiers !"

**Appel 2 (9h10) :**
"Salut, c'est Jacques. Internet ne marche plus dans tout le bâtiment B, c'est normal ?"

**Appel 3 (9h15) :**
"C'est Sophie, mon mot de passe a expiré et je ne peux plus me connecter."

**Question : Dans quel ordre traitez-vous ces tickets ?**

<details>
<summary>Cliquez pour voir la réponse</summary>

**Priorisation correcte :**

1. **Appel 2 - Internet bâtiment B (P1 - Critique)**
   - Impact : Élevé (tout un bâtiment, ~30 personnes)
   - Urgence : Haute (bloque le travail de tout le monde)
   - Action immédiate :
     - Vérifier le switch réseau du bâtiment B
     - Si problème matériel → Escalade niveau 2
     - Temps estimé : 5-15 min

2. **Appel 3 - Mot de passe expiré Sophie (P3 - Moyenne - MAIS RAPIDE)**
   - Impact : Faible (1 personne)
   - Urgence : Haute (bloquée)
   - **Mais résolution ultra-rapide (2 minutes)**
   - Action : Réinitialiser le mot de passe AD
   - Traiter AVANT le ticket de Marie car c'est 2 min vs 30 min

3. **Appel 1 - PC de Marie (P2 - Haute)**
   - Impact : Moyen (1 personne, mais réunion urgente)
   - Urgence : Haute (réunion dans 30 min)
   - Action :
     - Si écran noir : Mode sans échec
     - Si ça ne marche pas : Prêter un PC de remplacement
     - Récupérer les fichiers plus tard
   - Temps estimé : 15-30 min

**Principe :** Impact élevé d'abord, puis rapides, puis complexes.

</details>

---

## ✅ Checklist de diagnostic (à imprimer et garder sur vous)

### Problème : "Mon PC est lent"
- [ ] Gestionnaire des tâches → Onglet Performance → Vérifier CPU/Mémoire/Disque
- [ ] Vérifier l'espace disque (Ce PC → C: doit avoir > 10% libre)
- [ ] Vérifier programmes au démarrage (Task Manager → Startup)
- [ ] Nettoyage de disque si nécessaire
- [ ] Redémarrer le PC

### Problème : "Pas d'internet"
- [ ] Icône réseau : Croix rouge ou triangle jaune ?
- [ ] ping 127.0.0.1 (test carte réseau)
- [ ] ping 192.168.1.1 (test passerelle)
- [ ] ping 8.8.8.8 (test internet)
- [ ] ping google.com (test DNS)
- [ ] ipconfig /release → ipconfig /renew
- [ ] ipconfig /flushdns
- [ ] Redémarrer le PC

### Problème : "Imprimante ne marche pas"
- [ ] Imprimante allumée ? Papier ? Voyant d'erreur ?
- [ ] Imprimante "Prête" ou "Hors ligne" ? (Paramètres → Imprimantes)
- [ ] Supprimer les travaux d'impression
- [ ] Redémarrer le spouleur (services.msc → Spouleur d'impression)
- [ ] Désinstaller/Réinstaller l'imprimante
- [ ] Tester une page de test

### Problème : "Logiciel plante"
- [ ] Quel logiciel ? Quand plante-t-il ? Message d'erreur ?
- [ ] Fermer le processus (Task Manager → Fin de tâche)
- [ ] Redémarrer le PC
- [ ] Event Viewer → Journaux Windows → Application (vérifier les erreurs)
- [ ] Désinstaller/Réinstaller le logiciel
- [ ] Si Office : Réparer Office

---

## 📝 Message final de votre formateur

> **Félicitations ! Vous avez maintenant les bases du support N1/N2 !**
>
> **La vérité sur le support :**
> - Les 6 premiers mois seront répétitifs (mêmes problèmes en boucle)
> - C'est NORMAL. C'est comme ça qu'on apprend.
> - Avec de l'expérience, vous résoudrez 90% des tickets en moins de 5 minutes
>
> **Mes conseils pour réussir :**
> 1. **Créez votre "knowledge base"** : Un fichier Word avec VOS solutions aux problèmes courants
> 2. **Soyez patient avec les utilisateurs** : Ce n'est pas de leur faute s'ils ne comprennent pas la technique
> 3. **Documentez vos tickets** : Dans 6 mois, vous aurez oublié comment vous avez résolu ce problème bizarre
> 4. **N'ayez pas peur d'escalader** : Mieux vaut escalader que casser quelque chose
> 5. **Apprenez de vos collègues** : Regardez comment ils travaillent, posez des questions
>
> **À l'examen :**
> - Il y aura très probablement une mise en situation de support
> - Appliquez la méthodologie ÉCIRDT
> - Montrez que vous savez COMMUNIQUER (pas seulement la technique)
> - Documentez votre démarche (le jury note ça !)
>
> **En entreprise :**
> - Votre valeur = Nombre de tickets résolus × Satisfaction utilisateur
> - Un utilisateur content = Moins de tickets futurs (il apprendra à se débrouiller)
> - Soyez l'expert qui explique, pas celui qui fait à la place
>
> **Vous êtes prêt pour le support ! Maintenant, pratiquez ! 💪**

---

<div align="center">

**Cours suivant :** [Linux Serveur](../03-Linux/linux-serveur-fondamentaux.md)

[⬅️ Retour au sommaire](../README.md) | [📊 Progression](../progression.md)

---

**💡 Créez votre propre "cheat sheet" avec les 10 problèmes + solutions !**

</div>
