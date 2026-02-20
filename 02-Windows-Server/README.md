# 🖥️ Module 02 - Windows Server

## 📚 Contenu du module

Ce module couvre l'**administration complète de Windows Server**, du déploiement à la gestion avancée d'Active Directory et des GPO.

### 📖 Cours disponibles

#### Infrastructure de base
- **[Active Directory](Active-directory.md)** - Installation et configuration AD DS
- **[DNS et DHCP](dns-dhcp-windows-server.md)** - Services réseau essentiels
- **[WINS](wins-windows-internet-name-service.md)** - Windows Internet Name Service

#### Services réseau
- **[Serveur FTP](serveur-ftp.md)** - Configuration FTP avec authentification AD
- **[FSRM - Quotas](fsrm-quotas.md)** - File Server Resource Manager

#### GPO et gestion
- **[Stratégies de groupe (GPO)](gpo-strategies-groupe.md)** - Introduction aux GPO
- **[Guide Configuration GPO - Étape par étape](Guide_Configuration_GPO_Etape_Par_Etape.md)** ⭐ - Guide complet infrastructure AD
- **[Guide GPO Avancées](Guide_GPO_Avancees.md)** ⭐ - Configurations professionnelles

#### Clients Windows
- **[Windows Client 10/11](windows-client-10-11.md)** - Configuration et gestion des postes clients

### 🎯 Compétences visées

- ✅ Installation et configuration Windows Server 2022/2025
- ✅ Déploiement Active Directory Domain Services (AD DS)
- ✅ Gestion des utilisateurs, groupes et OUs
- ✅ Configuration DNS et DHCP
- ✅ Création et déploiement de GPO (base et avancées)
- ✅ Configuration de services (FTP, partages réseau)
- ✅ Gestion des quotas avec FSRM
- ⏳ WDS (Windows Deployment Services)
- ⏳ Hyper-V

### 🛠️ Environnement de lab

- **Hyperviseur** : VMware Workstation (voir [Module 04 - Virtualisation](../04-Virtualisation/))
- **Système** : Windows Server 2022 Standard (GUI)
- **Domaine** : entreprise.local
- **Réseau** : VMnet8 (NAT) - 192.168.10.0/24

### 📋 Guides spécialisés

Ces 3 guides complets ont été créés spécifiquement pour ce module :

1. **[Guide Configuration GPO - Étape par étape](Guide_Configuration_GPO_Etape_Par_Etape.md)** (500+ lignes)
   - Configuration infrastructure de base (IP, DNS, DHCP)
   - Installation Active Directory
   - Création structure OU
   - Configuration GPO de base

2. **[Guide GPO Avancées](Guide_GPO_Avancees.md)** (800+ lignes)
   - Installation de logiciels via GPO
   - Scripts (démarrage, arrêt, ouverture, fermeture)
   - Redirection de dossiers
   - Configuration pare-feu, AppLocker, navigateurs
   - Et bien plus...

3. Voir aussi : **[Guide VMware Lab](../04-Virtualisation/Guide_VMware_Lab_Windows_Server.md)** pour l'environnement de test

### 📈 Progression

Module : 🟢🟢🟢🟢⚪ (80%)

### 🚀 Projet réalisé

**Infrastructure Windows Server complète** (Janvier 2026)
- Contrôleur de domaine SOLARIS.local
- Structure OUs (Direction, Comptabilité)
- GPO de mappage de lecteurs
- Serveur FTP avec authentification AD
- Partages réseau sécurisés

---

[⬅ Retour au sommaire principal](../README.md)
