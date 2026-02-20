# 🎯 ECF BLANC - PRATIQUE WINDOWS WORKSTATION

**Auteur** : Guide de préparation ECF TSSR
**Durée** : ~1h45
**Méthode** : Interface Graphique (GUI)
**Prérequis** : VMware Workstation, ISO Windows 10/11 Pro

---

## 📚 LEXIQUE - Comprends où tu vas

```plaintext
🖥️ VM (Virtual Machine)      → Ordinateur virtuel dans VMware
💿 ISO                        → Image disque (comme un DVD virtuel)
🔌 VMnet2                     → Réseau virtuel isolé (ton labo privé)
📊 DHCP                       → Distribution automatique d'IP
🌐 IP fixe                    → Adresse IP permanente (pour serveurs)
💾 Disque C:                  → Disque système (Windows installé dessus)
💾 Disque D:                  → Disque de données/backup
📦 Image système              → Photo complète du disque C: (sauvegarde totale)
🔄 Restauration               → Remettre le disque comme avant avec l'image
🎯 Snapshot VMware            → Point de sauvegarde rapide (checkpoint)
```

---

## 🎯 OBJECTIFS DE CET EXERCICE

Tu vas apprendre à :
1. ✅ Installer une machine Windows 10/11 Pro
2. ✅ Configurer le réseau (DHCP)
3. ✅ Ajouter un disque de sauvegarde (80 Go)
4. ✅ Créer une image système complète (backup)
5. ✅ Simuler une panne de disque
6. ✅ Restaurer le système depuis l'image (Disaster Recovery)

**💡 Pourquoi c'est important :** En entreprise, un disque peut tomber en panne. Savoir restaurer rapidement = sauver l'entreprise !

---

## ⚡ RACCOURCIS RAPIDES - Accès interfaces

```plaintext
Win + X                     → Menu rapide (accès Gestion des disques, etc.)
Win + R → diskmgmt.msc     → Gestion des disques
Win + R → ncpa.cpl         → Connexions réseau
Win + R → sysdm.cpl        → Propriétés système (renommer PC)
Win + R → appwiz.cpl       → Programmes et fonctionnalités
Win + R → control          → Panneau de configuration
Win + I                     → Paramètres Windows
Win + E                     → Explorateur de fichiers
Win + Shift + S             → Capture d'écran
Ctrl + Shift + Esc          → Gestionnaire des tâches

💡 Tape "cmd" dans Démarrer → Ligne de commande
💡 Tape "panneau" dans Démarrer → Panneau de configuration
```

---

## PARTIE 1 : INSTALLATION WINDOWS 10/11

### 1.1 Créer la VM dans VMware

**Étapes cliquables :**

```plaintext
1. Ouvrir VMware Workstation
2. File → New Virtual Machine
3. Choisir "Typical (recommended)"
4. Next
5. "I will install the operating system later"
6. Next
7. Guest operating system :
   - Microsoft Windows
   - Version : Windows 10 x64 (ou 11)
8. Next
9. Virtual machine name : WIN10-CLIENT
   Location : (laisser par défaut)
10. Next
11. Maximum disk size : 60 GB
    ✅ Cocher "Store virtual disk as a single file"
12. Next
13. Cliquer sur "Customize Hardware"
14. Configuration :
    - Memory : 4096 MB (4 Go)
    - Processors : 2
    - Network Adapter : Custom (VMnet2)
    - CD/DVD : Use ISO image → Browse → Sélectionner ISO Windows 10/11
15. Close
16. Finish
```

**💡 ASTUCE** : VMnet2 = Réseau isolé. Comme créer un réseau privé pour tes tests, sans toucher ton vrai réseau.

**📸 Screenshot 1** : Fenêtre de résumé de la VM avant "Finish"

---

### 1.2 Installation Windows 10/11 Pro

**Démarrer la VM et installer :**

```plaintext
1. Cliquer sur "Power on this virtual machine"
2. Windows démarre depuis l'ISO
3. Choisir :
   - Langue : Français
   - Format heure : Français (France)
   - Clavier : Français
4. Suivant
5. "Installer maintenant"
6. Clé de produit : "Je n'ai pas de clé de produit"
7. Sélectionner "Windows 10 Pro" (ou 11 Pro)
8. Accepter les termes → Suivant
9. Type d'installation : "Personnalisée"
10. Où installer : Sélectionner "Espace non alloué" → Suivant
11. ⏳ Attendre (15-20 min)
12. Redémarrage automatique (plusieurs fois, c'est normal)
13. Configuration initiale :
    - Région : France
    - Clavier : Français
    - Réseau : Ignorer
    - Compte local : Admin / Admin123!
14. Refuser Cortana, localisation, etc.
```

**💡 ASTUCE** :
- **Personnalisée** = Installation propre (efface le disque)
- **Mise à niveau** = Garde les fichiers (pas pour une VM neuve)

**📸 Screenshot 2** : Bureau Windows ouvert après installation

---

### 1.3 Vérifier la configuration réseau DHCP

**Méthode GUI :**

```plaintext
1. Clic sur l'icône réseau (en bas à droite)
2. "Paramètres réseau et Internet"
3. "Propriétés" (sous Ethernet)
4. Vérifier :
   - Adresse IPv4 : 192.168.xxx.xxx (DHCP VMware)
   - Passerelle : 192.168.xxx.2
   - DNS : 192.168.xxx.2
```

**Méthode CMD (plus rapide) :**

```cmd
1. Win + R → cmd → Entrée
2. Taper : ipconfig
3. Lire l'adresse IP affichée
```

**📸 Screenshot 3** : Résultat `ipconfig` montrant l'IP DHCP

**💡 ASTUCE** : DHCP = Comme ton WiFi à la maison, l'IP est donnée automatiquement.

---

### 1.4 Renommer la machine

**Méthode rapide (GUI) :**

```plaintext
1. Win + R → sysdm.cpl → Entrée
2. Onglet "Nom de l'ordinateur"
3. "Modifier"
4. Nom : WIN10-CLIENT
5. OK → OK → Redémarrer
```

**Méthode alternative :**

```plaintext
1. Clic droit sur "Ce PC" → Propriétés
2. "Renommer ce PC"
3. WIN10-CLIENT → Suivant → Redémarrer
```

**📸 Screenshot 4** : Propriétés système montrant "WIN10-CLIENT"

**💡 ASTUCE** : Le nom est important pour identifier la machine sur le réseau et dans Active Directory.

---

## PARTIE 2 : DISQUE DE BACKUP ET SAUVEGARDE

### 2.1 Ajouter un disque dur virtuel (80 Go)

**Dans VMware (VM éteinte) :**

```plaintext
1. Éteindre la VM : VM → Power → Shut Down Guest
2. Edit virtual machine settings
3. Add (en bas)
4. Hard Disk → Next
5. SCSI (Recommended) → Next
6. Create a new virtual disk → Next
7. Disk size : 80 GB
   ✅ "Store virtual disk as a single file"
8. Next
9. Laisser : WIN10-CLIENT-Backup.vmdk
10. Finish → OK
11. Démarrer la VM
```

**💡 ASTUCE** : C'est comme ajouter un 2ème disque dur physique. Windows ne le voit pas encore car il n'est pas formaté.

---

### 2.2 Initialiser et formater le disque

**Ouvrir Gestion des disques (méthode rapide) :**

```plaintext
Win + X → Gestion des disques
OU
Win + R → diskmgmt.msc → Entrée
```

**Popup "Initialiser le disque" :**

```plaintext
1. ✅ Cocher "Disque 1"
2. Style : GPT (GUID Partition Table)
3. OK
```

**💡 ASTUCE** : GPT = Format moderne (supporte disques > 2 To). MBR = ancien.

**Créer une partition :**

```plaintext
Tu vois maintenant :
- Disque 0 (C:) = 60 Go (système)
- Disque 1 = 80 Go (Non alloué, en noir)

1. Clic droit sur "Non alloué" du Disque 1
2. "Nouveau volume simple"
3. Suivant
4. Taille : (laisser maximum) → Suivant
5. Lettre : D: → Suivant
6. Formater :
   - Système : NTFS
   - Nom du volume : Backup
   - ✅ "Formatage rapide"
7. Suivant → Terminer
```

**💡 ASTUCE** :
- **NTFS** = Format Windows moderne (comme ext4 pour Linux)
- **Nom "Backup"** = Ce que tu verras dans "Ce PC"

**📸 Screenshot 5** : Gestion des disques montrant C: (60 Go) et D: Backup (80 Go)

---

### 2.3 Créer une image système (backup complet)

**Ouvrir l'outil de sauvegarde (méthode rapide) :**

```plaintext
Win + R → control → Entrée
Ou taper "panneau" dans Démarrer

1. Panneau de configuration
2. "Système et sécurité"
3. "Sauvegarder et restaurer (Windows 7)"
   (Oui, écrit Windows 7 même sur Win10/11 !)
4. Menu gauche : "Créer une image système"
```

**Assistant de création :**

```plaintext
1. Où enregistrer ?
   → "Sur un disque dur"
   → Sélectionner : D: (Backup)
2. Suivant
3. Lecteurs à inclure :
   → ✅ C: (coché automatiquement)
   → ❌ D: (ne PAS cocher, c'est le disque de destination)
4. Suivant
5. Confirmer → "Démarrer la sauvegarde"
6. ⏳ Attendre (10-20 min)
7. "Créer un disque de réparation ?" → Non
8. Fermer
```

**💡 ASTUCE** : Image système = Photo complète de C:. Si le disque plante, tu restaures TOUT exactement comme avant.

**📸 Screenshot 6** : Message "La sauvegarde a réussi" avec taille

---

### 2.4 Installer quelques logiciels (avant la panne simulée)

Pour rendre la restauration plus visible, installe des logiciels :

```plaintext
1. Ouvrir Edge
2. Télécharger Firefox : https://www.mozilla.org/fr/firefox/
3. Installer Firefox
4. (Optionnel) Télécharger Chrome
```

**📸 Screenshot 7** : Menu Démarrer montrant Firefox installé

---

### 2.5 Simuler une panne de disque et restaurer

#### Étape A : Simuler la panne

**VM éteinte, supprimer le disque C: :**

```plaintext
1. Éteindre la VM (Shut Down Guest)
2. Edit virtual machine settings
3. Sélectionner "Hard Disk (SCSI 0:0)" → Disque 60 Go (C:)
4. Remove → OK
```

**💡 ASTUCE** : Simule un disque dur mort (HS, kaput).

---

#### Étape B : Ajouter un disque vierge de remplacement

```plaintext
1. Edit virtual machine settings
2. Add
3. Hard Disk → Next
4. SCSI (Recommended) → Next
5. Create a new virtual disk → Next
6. Disk size : 60 GB
   ✅ "Store virtual disk as a single file"
7. Next → Finish → OK
```

---

#### Étape C : Booter sur l'ISO et restaurer

**Configurer le boot sur ISO :**

```plaintext
1. Edit virtual machine settings
2. CD/DVD (SATA)
3. ✅ "Use ISO image file"
4. Browse → Ton ISO Windows 10/11
5. OK
6. Démarrer la VM
```

**Processus de restauration :**

```plaintext
1. "Appuyez sur une touche..." → Appuyer rapidement
2. Fenêtre installation Windows
3. Suivant
4. En bas à gauche : "Réparer l'ordinateur"
5. Dépannage (Troubleshoot)
6. Options avancées
7. "Récupération de l'image système"
8. Sélectionner Windows 10/11
9. Assistant détecte l'image sur D: automatiquement
   "Utiliser la dernière image système"
10. Suivant
11. (Si demandé) Exclure D: pour ne pas l'écraser
12. Suivant → Terminer
13. "Êtes-vous sûr ?" → Oui
14. ⏳ Attendre (10-20 min)
15. Redémarrer
```

**💡 ASTUCE** : Tu viens de faire un **Disaster Recovery** ! C'est exactement comme ça qu'on récupère un serveur après panne matérielle.

**📸 Screenshot 8** : Bureau Windows restauré + Firefox toujours là !

**Vérification finale :**

```cmd
Win + R → winver → Entrée
```

**📸 Screenshot 9** : Version Windows confirmée

---

## ✅ CHECKLIST COMPLÈTE

### Partie Installation

```markdown
- [ ] VM WIN10-CLIENT créée (4 Go RAM, 2 CPU, VMnet2)
- [ ] Windows 10/11 Pro installé
- [ ] IP DHCP vérifiée avec ipconfig
- [ ] Machine renommée en WIN10-CLIENT
- [ ] 4 screenshots capturés
```

### Partie Backup & Restauration

```markdown
- [ ] Disque D: (Backup, 80 Go) ajouté
- [ ] Disque initialisé en GPT et formaté NTFS
- [ ] Image système créée sur D:
- [ ] Logiciels installés (Firefox)
- [ ] Disque C: supprimé (panne simulée)
- [ ] Nouveau disque ajouté
- [ ] Restauration depuis image réussie
- [ ] Système fonctionne + logiciels présents
- [ ] 5 screenshots capturés
```

---

## 🚀 ASTUCES PRO

### Snapshot VMware (point de sauvegarde rapide)

Avant chaque étape critique :

```plaintext
1. VM → Snapshot → Take Snapshot
2. Nom : "Avant suppression disque C:"
3. Si problème : VM → Snapshot → Revert to Snapshot
```

### Commandes utiles

```cmd
ipconfig                → Voir IP
ipconfig /all           → Tout voir (DNS, passerelle, etc.)
systeminfo              → Infos système complètes
winver                  → Version Windows
hostname                → Nom de la machine
```

### Lexique technique

```plaintext
Disaster Recovery (DR)  → Récupération après sinistre
Backup                  → Sauvegarde
Image système           → Clone complet du disque
Snapshot                → Point de restauration rapide (VMware)
NTFS                    → Système de fichiers Windows
GPT                     → Table de partition moderne (> 2 To)
MBR                     → Table de partition ancienne (< 2 To)
```

---

**Suite** : [PRATIQUE-ACTIVE-DIRECTORY.md](PRATIQUE-ACTIVE-DIRECTORY.md)

---

*Guide créé pour la préparation ECF TSSR - Nextformation 2025-2026*
