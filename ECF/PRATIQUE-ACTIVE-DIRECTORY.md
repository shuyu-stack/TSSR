# 🎯 ECF BLANC - ACTIVE DIRECTORY

**Auteur** : Guide de préparation ECF TSSR
**Durée** : ~1h15
**Méthode** : Interface Graphique (GUI)
**Prérequis** : ISO Windows Server 2019/2022, VMware Workstation

---

## 📚 LEXIQUE - Comprends où tu vas

```plaintext
🏢 Active Directory (AD)      → Base de données centralisée users/PC/permissions
👤 DC (Domain Controller)     → Serveur qui héberge Active Directory
🌐 Domaine                    → Réseau centralisé (ex: ENTREPRISE.LOCAL)
🔐 Authentification           → Vérifier identité (login/password)
📁 OU (Organizational Unit)   → Dossier pour organiser users/PC
👥 Groupe de sécurité         → Ensemble users (permissions groupées)
📜 GPO (Group Policy Object)  → Règles auto (fond écran, soft, etc.)
🌍 DNS                        → Annuaire : noms → IP
📡 DHCP                       → Distribution auto des IP
🔧 AD DS                      → Active Directory Domain Services
🎯 Promouvoir                 → Transformer serveur en DC
⚡ Forest                     → Ensemble de domaines AD
🔑 DSRM                       → Password récupération AD (mode sans échec)
```

---

## ⚡ RACCOURCIS RAPIDES

### Sur le serveur Windows Server

```plaintext
Win + R → servermanager       → Server Manager
Win + R → dnsmgmt.msc          → DNS Manager
Win + R → dhcpmgmt.msc         → DHCP Manager
Win + R → dsa.msc              → Active Directory Users & Computers
Win + R → gpmc.msc             → Group Policy Management
Win + R → ncpa.cpl             → Connexions réseau
Win + R → sysdm.cpl            → Propriétés système (renommer)
Win + R → cmd                  → Ligne de commande

💡 Server Manager → Tools → Accès à TOUS les outils AD/DNS/DHCP
```

### Sur le client Windows 10

```plaintext
Win + R → ncpa.cpl             → Config DNS
Win + R → sysdm.cpl            → Joindre domaine
Win + R → cmd                  → nslookup, ipconfig, gpupdate
```

---

## 🎯 OBJECTIFS

Créer une infrastructure d'entreprise complète :

1. ✅ Installer Windows Server 2019/2022
2. ✅ Configurer IP fixe (192.168.10.10)
3. ✅ Installer Active Directory (AD DS)
4. ✅ Créer domaine ENTREPRISE.LOCAL
5. ✅ Configurer DNS (automatique)
6. ✅ Installer DHCP (pool 192.168.10.100-200)
7. ✅ Créer structure OU (Direction, Compta, RH)
8. ✅ Créer 3 utilisateurs + groupes
9. ✅ Appliquer GPO (fond d'écran Direction)
10. ✅ Joindre client au domaine

**💡 Pourquoi :** Active Directory = Cœur de toute entreprise Windows. Gère users, PC, permissions, politiques.

---

## 📐 Architecture du réseau

### Notre plan :

```plaintext
Nom du domaine : ENTREPRISE.LOCAL
Réseau : 192.168.10.0/24

Machines :
┌─────────────────┬──────────────────┬─────────────┐
│ Nom             │ IP               │ Rôle        │
├─────────────────┼──────────────────┼─────────────┤
│ SRV-DC01        │ 192.168.10.10    │ Serveur AD  │
│ WIN10-CLIENT    │ 192.168.10.100+  │ Client DHCP │
└─────────────────┴──────────────────┴─────────────┘

Adresses IP :
- .1 - .9       : Routeur, équipements
- .10 - .50     : Serveurs (IP fixes)
- .100 - .200   : DHCP (clients automatiques)
- .201 - .254   : IP fixes (imprimantes, etc.)
```

**💡 ASTUCE** : C'est comme organiser tes dossiers de projet :
```
/serveurs     → IP .10-.50
/clients      → IP .100-.200 (auto)
/equipements  → IP .201-.254 (fixes)
```

---

## 3.1 Installation Windows Server 2019/2022

### Créer la VM Serveur

Dans VMware :

```plaintext
1. File → New Virtual Machine → Typical
2. I will install the operating system later → Next
3. Guest OS : Microsoft Windows Server 2019 (ou 2022)
4. Next
5. VM name : SRV-DC01
6. Next
7. Disk size : 80 GB → Next
8. Customize Hardware :
   - Memory : 4096 MB (6144 si tu as assez de RAM)
   - Processors : 2
   - Network Adapter : Custom (VMnet2) - MÊME réseau que le client !
   - CD/DVD : Use ISO → Browse → Ton ISO Windows Server
9. Close → Finish
10. Power on this virtual machine
```

**💡 ASTUCE** : VMnet2 = réseau isolé. Les deux machines (client et serveur) doivent être sur le MÊME VMnet pour communiquer !

### Installer Windows Server

Démarrage de l'installation :

```plaintext
1. La VM boot sur l'ISO
2. Langue : Français → Suivant
3. Installer maintenant
4. Sélectionner : Windows Server 2019 Standard (Desktop Experience)
   OU : Windows Server 2022 Standard (Desktop Experience)
   ⚠️ PAS "Standard" sans Desktop = version sans interface !
5. Accepter les termes → Suivant
6. Type : Personnalisée
7. Sélectionner le disque → Suivant
8. ⏳ Attendre (15-20 min)
9. Redémarrage automatique
10. Personnaliser les paramètres :
    Mot de passe administrateur : P@ssw0rd123!
    (En vrai, utilise un mot de passe fort !)
11. Terminer
```

**💡 ASTUCE** : "Desktop Experience" = interface graphique. Sans ça, c'est que de la ligne de commande (comme un serveur Linux sans GUI).

**📸 Screenshot 1** : Bureau Windows Server ouvert

### Configurer une IP fixe sur le serveur

Le serveur DOIT avoir une IP fixe :

```plaintext
1. Clic droit sur l'icône réseau (en bas à droite)
2. Ouvrir les paramètres réseau et Internet
3. Modifier les options d'adaptateur
4. Clic droit sur "Ethernet0" → Propriétés
5. Double-cliquer sur "Protocole Internet version 4 (TCP/IPv4)"
6. Sélectionner "Utiliser l'adresse IP suivante"
7. Remplir :
   - Adresse IP : 192.168.10.10
   - Masque de sous-réseau : 255.255.255.0
   - Passerelle par défaut : 192.168.10.1
8. Serveur DNS préféré : 127.0.0.1
   (Il pointe sur lui-même car il VA devenir le serveur DNS)
9. OK → OK → Fermer
```

**💡 ASTUCE CRITIQUE** : Pourquoi DNS = 127.0.0.1 ?
- Le serveur AD EST le serveur DNS
- Il doit pointer sur lui-même
- C'est comme un service qui s'auto-référence

Vérifier la config :

```plaintext
1. Win + R → cmd → Entrée
2. Taper : ipconfig /all
3. Vérifier que tout est bon
```

**📸 Screenshot 2** : Résultat de ipconfig /all montrant IP 192.168.10.10

### Renommer le serveur

```plaintext
1. Clic droit sur "Ce PC" → Propriétés
2. En bas, "Renommer ce PC"
3. Nouveau nom : SRV-DC01
4. Suivant
5. Redémarrer maintenant
```

**💡 ASTUCE** : Toujours renommer AVANT d'installer Active Directory ! Changer après, c'est compliqué.

---

## 3.2 Installation Active Directory Domain Services (AD DS)

Ouvrir le Server Manager (il s'ouvre automatiquement au démarrage) :

```plaintext
1. Cliquer sur "Manage" (en haut à droite)
2. Cliquer sur "Add Roles and Features"
3. Avant de commencer : Suivant
4. Type d'installation : "Role-based..." → Suivant
5. Sélection du serveur : SRV-DC01 (déjà sélectionné) → Suivant
6. Rôles de serveurs :
   ✅ Cocher "Active Directory Domain Services"
7. Une popup apparaît "Add features..." → Cliquer sur "Add Features"
8. Suivant
9. Fonctionnalités : (rien à cocher) → Suivant
10. AD DS : (juste lire) → Suivant
11. Confirmation :
    ⚠️ Cocher "Restart the destination server automatically if required"
    → Cliquer sur "Install"
12. ⏳ Attendre l'installation (5-10 min)
13. NE PAS fermer la fenêtre quand c'est terminé !
```

**📸 Screenshot 3** : Fenêtre montrant "Installation succeeded on SRV-DC01"

**💡 ASTUCE** : AD DS = la base de données des utilisateurs, groupes, machines. C'est le cœur de l'infrastructure Windows.

---

## 3.3 Promouvoir le serveur en contrôleur de domaine

Juste après l'installation, dans la même fenêtre :

```plaintext
1. Cliquer sur le lien "Promote this server to a domain controller"
   (en bleu, dans la fenêtre de résultat)
2. Configuration du déploiement :
   → Sélectionner "Add a new forest"
   → Root domain name : ENTREPRISE.LOCAL
3. Suivant
4. Options du contrôleur de domaine :
   - Forest functional level : Windows Server 2016
   - Domain functional level : Windows Server 2016
   - ✅ DNS server (doit être coché)
   - ✅ Global Catalog (GC) (déjà coché)
   - Password (DSRM) : P@ssw0rd123!
     (C'est le mot de passe de récupération, garde-le précieusement)
5. Suivant
6. Options DNS : (un warning jaune apparaît, c'est normal) → Suivant
7. Options supplémentaires :
   - NetBIOS domain name : ENTREPRISE (automatique)
8. Suivant
9. Chemins : (laisser par défaut) → Suivant
10. Examiner les options : (vérifier) → Suivant
11. Vérification de la configuration :
    ⏳ Attendre les vérifications (2-3 min)
    → Si tout est OK : "All prerequisite checks passed successfully"
12. Cliquer sur "Install"
13. ⏳ Attendre (10-15 min)
14. Le serveur redémarre automatiquement
```

**💡 ASTUCE** :
- **Forest** = La forêt (contient tous les domaines)
- **Domain** = Le domaine (ENTREPRISE.LOCAL)
- **.LOCAL** = Jamais utiliser .COM ou un vrai domaine Internet !

Après redémarrage :

```plaintext
1. L'écran de connexion affiche maintenant :
   ENTREPRISE\Administrateur
2. Se connecter avec le mot de passe : P@ssw0rd123!
```

**📸 Screenshot 4** : Écran de connexion montrant "ENTREPRISE\Administrateur"

**💡 ASTUCE** : Si tu vois "ENTREPRISE", c'est gagné ! Ton domaine Active Directory fonctionne !

---

## 3.4 Vérifier que DNS fonctionne

DNS est automatiquement installé avec AD :

```plaintext
1. Server Manager → Tools → DNS
2. Dans DNS Manager, développer "SRV-DC01"
3. Développer "Forward Lookup Zones"
4. Tu dois voir : ENTREPRISE.LOCAL
5. Cliquer dessus
6. Tu vois des enregistrements créés automatiquement
```

**📸 Screenshot 5** : DNS Manager montrant la zone ENTREPRISE.LOCAL

**💡 ASTUCE** : Sans DNS, Active Directory ne fonctionne PAS. Les clients cherchent le serveur via DNS (enregistrements SRV).

---

## 3.5 Installation et configuration DHCP

### Installer le rôle DHCP

Dans Server Manager :

```plaintext
1. Manage → Add Roles and Features
2. Suivant → Suivant → Suivant
3. Rôles de serveurs :
   ✅ Cocher "DHCP Server"
4. Add Features → Suivant
5. Fonctionnalités : Suivant
6. DHCP Server : Suivant
7. Confirmation : Install
8. ⏳ Attendre (2-3 min)
9. Quand c'est terminé, cliquer sur "Complete DHCP configuration"
```

**💡 ASTUCE** : DHCP distribue automatiquement les adresses IP aux clients (comme ta box Internet chez toi).

### Post-installation DHCP

Une nouvelle fenêtre s'ouvre :

```plaintext
Titre : "DHCP Post-Install configuration wizard"
1. Description : Suivant
2. Autorisation :
   → Utiliser les informations d'identification suivantes :
   → ENTREPRISE\Administrateur (déjà rempli)
3. Suivant
4. Summary → Commit
5. ⏳ Attendre (5 secondes)
6. Close → Close
```

**💡 ASTUCE** : L'autorisation DHCP empêche des serveurs DHCP "pirates" de distribuer des mauvaises IP (sécurité).

### Créer une étendue DHCP (pool d'adresses)

Ouvrir le gestionnaire DHCP :

```plaintext
1. Server Manager → Tools → DHCP
2. Développer "SRV-DC01.ENTREPRISE.LOCAL"
3. Développer "IPv4"
4. Clic droit sur "IPv4" → Nouvelle étendue (New Scope)
```

Assistant Nouvelle étendue :

```plaintext
1. Bienvenue : Suivant
2. Nom de l'étendue :
   - Nom : Réseau Entreprise
   - Description : Pool DHCP pour clients
3. Suivant
4. Plage d'adresses IP :
   - Adresse IP de début : 192.168.10.100
   - Adresse IP de fin : 192.168.10.200
   - Longueur : 24
   - Masque : 255.255.255.0 (automatique)
5. Suivant
6. Exclusions : (laisser vide) → Suivant
7. Durée du bail :
   - Laisser par défaut : 8 jours
8. Suivant
9. Configurer les options DHCP maintenant :
   → Oui → Suivant
10. Routeur (passerelle) :
    - Adresse IP : 192.168.10.1
    - Cliquer sur "Ajouter"
11. Suivant
12. Nom de domaine et serveurs DNS :
    - Domaine parent : ENTREPRISE.LOCAL (déjà rempli)
    - Adresse IP : 192.168.10.10
    - Cliquer sur "Ajouter"
13. Suivant
14. Serveurs WINS : (laisser vide) → Suivant
15. Activer l'étendue :
    → Oui → Suivant
16. Terminer
```

**💡 ASTUCE** :
- **Pool 100-200** = 100 adresses dispo
- **Bail 8 jours** = Un client garde son IP 8 jours (renouvelable)
- **Passerelle** = La sortie du réseau (routeur)

Vérifier que l'étendue est active :

```plaintext
Dans DHCP Manager :
1. IPv4 → Scope [192.168.10.0] Réseau Entreprise
2. Tu dois voir une flèche verte (= actif)
```

**📸 Screenshot 6** : DHCP Manager montrant l'étendue active avec flèche verte

---

## 3.6 Créer la structure Organizational Units (OU)

Ouvrir Active Directory Users and Computers :

```plaintext
1. Server Manager → Tools → Active Directory Users and Computers
```

### Créer l'OU principale :

```plaintext
1. Dans le panneau de gauche, clic droit sur "ENTREPRISE.LOCAL"
2. New → Organizational Unit
3. Name : ENTREPRISE_USERS
4. ✅ Protect container from accidental deletion (coché par défaut)
5. OK
```

### Créer les sous-OU (départements) :

```plaintext
1. Développer "ENTREPRISE.LOCAL"
2. Clic droit sur "ENTREPRISE_USERS"
3. New → Organizational Unit
4. Name : DIRECTION
5. OK

Répéter pour :
6. COMPTA
7. RH
```

### Structure finale :

```plaintext
ENTREPRISE.LOCAL
├── Builtin (système)
├── Computers (par défaut)
├── Domain Controllers (système)
├── ForeignSecurityPrincipals (système)
├── Managed Service Accounts (système)
├── Users (par défaut)
└── ENTREPRISE_USERS ← Créé
    ├── DIRECTION ← Créé
    ├── COMPTA ← Créé
    └── RH ← Créé
```

**📸 Screenshot 7** : Active Directory Users and Computers montrant la structure OU

**💡 ASTUCE** : Les OU sont comme des dossiers. Ça permet d'organiser les utilisateurs par service et d'appliquer des règles (GPO) par département.

---

## 3.7 Créer les utilisateurs

### Créer un utilisateur - Direction

Dans Active Directory Users and Computers :

```plaintext
1. Développer ENTREPRISE_USERS
2. Clic droit sur "DIRECTION" → New → User
3. Remplir :
   - First name : Marie
   - Last name : Dupont
   - User logon name : m.dupont
4. Next
5. Password : P@ssw0rd123!
6. Confirm password : P@ssw0rd123!
7. ✅ User must change password at next logon (coché)
8. ❌ User cannot change password (décoché)
9. ❌ Password never expires (décoché)
10. ❌ Account is disabled (décoché)
11. Next
12. Finish
```

**💡 ASTUCE** : "User must change password at next logon" = Sécurité. L'utilisateur DOIT changer le mot de passe temporaire à sa première connexion.

### Créer un utilisateur - Compta

```plaintext
Même procédure dans l'OU "COMPTA" :
- First name : Jean
- Last name : Martin
- User logon name : j.martin
- Password : P@ssw0rd123!
- User must change password at next logon : ✅
```

### Créer un utilisateur - RH

```plaintext
Même procédure dans l'OU "RH" :
- First name : Sophie
- Last name : Bernard
- User logon name : s.bernard
- Password : P@ssw0rd123!
- User must change password at next logon : ✅
```

**📸 Screenshot 8** : AD Users and Computers montrant les 3 utilisateurs dans leurs OU

**💡 ASTUCE** : C'est comme créer des objets utilisateur dans une base de données :

```javascript
{
  firstName: "Marie",
  lastName: "Dupont",
  username: "m.dupont",
  department: "DIRECTION",
  email: "m.dupont@ENTREPRISE.LOCAL"
}
```

---

## 3.8 Créer des groupes de sécurité

Pourquoi des groupes ? Pour donner des permissions à plusieurs personnes en même temps.

### Créer un groupe pour la Direction

```plaintext
1. Clic droit sur l'OU "DIRECTION" → New → Group
2. Remplir :
   - Group name : GRP_DIRECTION
   - Group scope : Global (par défaut)
   - Group type : Security (par défaut)
3. OK
```

### Ajouter Marie au groupe

```plaintext
1. Dans l'OU DIRECTION, double-cliquer sur "Marie Dupont"
2. Onglet "Member Of"
3. Cliquer sur "Add"
4. Taper : GRP_DIRECTION
5. Cliquer sur "Check Names" (le nom devient souligné)
6. OK
7. OK
```

Méthode alternative (plus rapide) :

```plaintext
1. Double-cliquer sur le groupe "GRP_DIRECTION"
2. Onglet "Members"
3. Add
4. Taper : m.dupont
5. Check Names → OK → OK
```

### Créer les groupes Compta et RH

```plaintext
Dans OU COMPTA :
- Créer groupe : GRP_COMPTA
- Ajouter Jean Martin (j.martin)

Dans OU RH :
- Créer groupe : GRP_RH
- Ajouter Sophie Bernard (s.bernard)
```

**📸 Screenshot 9** : Un des groupes ouvert montrant le membre

**💡 ASTUCE** : Les groupes sont comme des rôles. Au lieu de donner des droits à Marie, Jean, Sophie séparément, tu donnes des droits aux groupes GRP_DIRECTION, GRP_COMPTA, GRP_RH.

---

## 3.9 Créer une stratégie de groupe (GPO) - Exemple

### Objectif : Mettre un fond d'écran personnalisé pour la Direction

### Créer la GPO

```plaintext
1. Server Manager → Tools → Group Policy Management
2. Développer : Forest → Domains → ENTREPRISE.LOCAL
3. Développer ENTREPRISE_USERS
4. Clic droit sur "DIRECTION" → Create a GPO in this domain, and Link it here
5. Name : GPO_DIRECTION_Wallpaper
6. OK
```

### Modifier la GPO

```plaintext
1. Clic droit sur "GPO_DIRECTION_Wallpaper" → Edit
2. Une nouvelle fenêtre s'ouvre : Group Policy Management Editor
3. Naviguer vers :
   User Configuration
   └── Policies
       └── Administrative Templates
           └── Desktop
               └── Desktop
4. Dans le panneau de droite, double-cliquer sur "Desktop Wallpaper"
5. Sélectionner "Enabled"
6. Wallpaper Name : C:\Windows\Web\Wallpaper\Windows\img0.jpg
   (On utilise un fond d'écran Windows par défaut pour le test)
7. Wallpaper Style : Fill
8. OK
9. Fermer la fenêtre de l'éditeur
```

**📸 Screenshot 10** : Group Policy Management montrant la GPO liée à l'OU DIRECTION

**💡 ASTUCE** : Les GPO sont puissantes ! Tu peux :
- Installer des logiciels automatiquement
- Configurer le pare-feu
- Bloquer l'accès au panneau de configuration
- Mapper des lecteurs réseau
- Et bien plus !

---

## 3.10 Joindre WIN10-CLIENT au domaine

### D'abord, configurer le DNS sur le client :

Sur WIN10-CLIENT :

```plaintext
1. Clic droit sur l'icône réseau → Ouvrir les paramètres réseau et Internet
2. Modifier les options d'adaptateur
3. Clic droit sur "Ethernet0" → Propriétés
4. Double-cliquer sur "Protocole Internet version 4 (TCP/IPv4)"
5. Sélectionner "Utiliser l'adresse IP suivante" :
   - Adresse IP : 192.168.10.50 (ou laisser en DHCP, c'est OK aussi)
   - Masque : 255.255.255.0
   - Passerelle : 192.168.10.1
6. Serveur DNS préféré : 192.168.10.10 ⚠️ CRITIQUE
7. OK → OK → Fermer
```

**💡 ASTUCE ULTRA IMPORTANTE** : Le client DOIT pointer vers le DC pour le DNS (192.168.10.10). Sinon, il ne trouvera JAMAIS le domaine !

### Tester la résolution DNS :

```plaintext
1. Win + R → cmd → Entrée
2. Taper : nslookup ENTREPRISE.LOCAL
3. Tu dois voir :
   Server: SRV-DC01.ENTREPRISE.LOCAL
   Address: 192.168.10.10

   Name: ENTREPRISE.LOCAL
   Address: 192.168.10.10

Si ça ne fonctionne pas → Vérifie que le DNS = 192.168.10.10 !
```

### Joindre le domaine

```plaintext
1. Clic droit sur "Ce PC" → Propriétés
2. En bas, cliquer sur "Renommer ce PC (avancé)"
3. Cliquer sur "Modifier"
4. Sous "Membre de", sélectionner "Domaine"
5. Taper : ENTREPRISE.LOCAL
6. OK
7. Une fenêtre demande des identifiants :
   - Nom d'utilisateur : Administrateur
   - Mot de passe : P@ssw0rd123!
8. OK
9. Message : "Bienvenue dans le domaine ENTREPRISE.LOCAL"
10. OK → OK
11. Redémarrer maintenant
```

**📸 Screenshot 11** : Message de bienvenue dans le domaine

Après redémarrage :

```plaintext
1. Écran de connexion :
   → "Autre utilisateur" apparaît
2. Se connecter :
   - Utilisateur : m.dupont
     (ou ENTREPRISE\m.dupont pour être sûr)
   - Mot de passe : P@ssw0rd123!
3. Windows demande de changer le mot de passe :
   - Ancien : P@ssw0rd123!
   - Nouveau : Nouveau123!
   - Confirmer : Nouveau123!
4. Entrée
5. Le bureau s'ouvre !
```

**📸 Screenshot 12** : Bureau ouvert avec le compte m.dupont (vérifier dans Démarrer → en haut)

**💡 ASTUCE** : Tu es maintenant connecté avec un compte Active Directory ! Le profil se synchronise sur tous les PC du domaine.

---

## 3.11 Vérifier que tout fonctionne

### Sur le client (WIN10-CLIENT)

Vérifier le domaine :

```plaintext
1. Win + R → cmd
2. Taper : systeminfo | findstr /B /C:"Domaine"
3. Tu dois voir : Domaine: ENTREPRISE.LOCAL
```

Vérifier les GPO appliquées :

```plaintext
1. Dans cmd, taper : gpresult /R
2. Chercher la section "COMPUTER SETTINGS" et "USER SETTINGS"
3. Tu devrais voir "GPO_DIRECTION_Wallpaper" appliquée
```

Forcer l'application des GPO :

```plaintext
1. cmd : gpupdate /force
2. Attendre (ça prend 1-2 minutes)
3. Redémarrer ou se déconnecter/reconnecter
4. Le fond d'écran devrait changer !
```

**📸 Screenshot 13** : Résultat de gpresult /R montrant les GPO

### Sur le serveur (SRV-DC01)

Voir les ordinateurs du domaine :

```plaintext
1. Active Directory Users and Computers
2. Développer ENTREPRISE.LOCAL
3. Cliquer sur "Computers"
4. Tu dois voir : WIN10-CLIENT
```

Voir les baux DHCP :

```plaintext
1. DHCP Manager
2. IPv4 → Scope → Address Leases
3. Tu dois voir WIN10-CLIENT avec une IP 192.168.10.xxx
```

**📸 Screenshot 14** : DHCP montrant le bail actif de WIN10-CLIENT

**💡 ASTUCE** : Si WIN10-CLIENT apparaît ici, c'est que TOUT fonctionne : DNS, DHCP, Active Directory !

---

## ✅ Checklist Partie 3

```markdown
- [ ] VM SRV-DC01 créée
- [ ] Windows Server installé (Desktop Experience)
- [ ] IP fixe configurée (192.168.10.10)
- [ ] Serveur renommé (SRV-DC01)
- [ ] AD DS installé
- [ ] Serveur promu en DC (domaine ENTREPRISE.LOCAL)
- [ ] DNS fonctionne
- [ ] DHCP installé et autorisé
- [ ] Étendue DHCP créée (100-200)
- [ ] OU créées (DIRECTION, COMPTA, RH)
- [ ] 3 utilisateurs créés
- [ ] 3 groupes créés
- [ ] GPO créée et liée
- [ ] Client joint au domaine
- [ ] Connexion avec utilisateur du domaine OK
- [ ] 14 screenshots capturés
```

---

**Précédent** : [PRATIQUE-DISQUES-SAUVEGARDE.md](PRATIQUE-DISQUES-SAUVEGARDE.md)
**Compléments** : [PRATIQUE-ASTUCES.md](PRATIQUE-ASTUCES.md)

---

*Guide créé pour la préparation ECF TSSR - Nextformation 2025-2026*
