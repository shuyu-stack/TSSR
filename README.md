# 📚 Formation TSSR - Cours et Tutoriels

[![Formation](https://img.shields.io/badge/Formation-TSSR-blue.svg)](https://www.nextformation.com)
[![Durée](https://img.shields.io/badge/Durée-Décembre%202024%20→%20Juin%202025-green.svg)](https://www.nextformation.com)
[![Niveau](https://img.shields.io/badge/Niveau-Bac+2-orange.svg)](https://www.francecompetences.fr)
[![Statut](https://img.shields.io/badge/Statut-En%20cours-yellow.svg)](https://github.com/rimk-tssr)

> 🎓 Documentation complète de ma formation **Technicien Supérieur Systèmes et Réseaux (TSSR)** chez Nextformation.

---

## 📖 À propos

Ce dépôt contient l'ensemble de mes **cours, tutoriels, exercices et projets** réalisés durant ma formation TSSR de décembre 2024 à juin 2025.

**Objectif de la formation :** Devenir technicien support niveau 2 / administrateur réseau, capable d'installer, configurer et maintenir des infrastructures IT (serveurs Windows/Linux, réseaux, sécurité).

### 🎯 Compétences visées

- ✅ Administration Windows Server (Active Directory, GPO, DNS, DHCP)
- ✅ Administration Linux (Debian, Ubuntu, CentOS)
- ✅ Gestion des réseaux (TCP/IP, subnetting, routage, switching)
- ✅ Virtualisation (VMware, VirtualBox, Hyper-V)
- ✅ Sécurité informatique (pare-feu, VPN, antivirus)
- ✅ Support utilisateur (niveau 1 et 2)
- ✅ Scripting (PowerShell, Bash)
- ✅ Sauvegarde et restauration

---

## 📂 Structure du dépôt

```
📦 formation-tssr/
├── 📁 01-Reseaux/
│   ├── cours-tcp-ip.md
│   ├── subnetting-exercices.md
│   └── configuration-routeur.md
│
├── 📁 02-Windows-Server/
│   ├── installation-windows-server.md
│   ├── active-directory.md
│   ├── gpo-mappage-lecteurs.md
│   ├── serveur-ftp.md
│   └── fsrm-quotas.md
│
├── 📁 03-Linux/
│   ├── commandes-de-base.md
│   ├── gestion-utilisateurs.md
│   ├── apache-nginx.md
│   └── scripts-bash.md
│
├── 📁 04-Virtualisation/
│   ├── vmware-workstation.md
│   ├── virtualbox-configuration.md
│   └── hyper-v.md
│
├── 📁 05-Securite/
│   ├── pare-feu-windows.md
│   ├── vpn-configuration.md
│   └── bonnes-pratiques.md
│
├── 📁 06-Scripting/
│   ├── powershell-bases.md
│   ├── scripts-administration.md
│   └── bash-automatisation.md
│
├── 📁 07-Projets/
│   ├── projet-infrastructure-complete.md
│   ├── mise-en-place-domaine.md
│   └── migration-serveur.md
│
├── 📁 08-Certifications/
│   ├── preparation-ccna.md
│   └── preparation-mcsa.md
│
├── 📁 09-Ressources/
│   ├── liens-utiles.md
│   ├── outils-recommandes.md
│   └── lexique-technique.md
│
├── 📁 10-Maitre-Emplois/
│   └── emplois-technicien-exploitation.md
│
├── 📁 11-Telephonie-VoIP/
│   ├── README.md (plan de formation)
│   ├── 01-fondamentaux-voip.md
│   ├── 02-protocoles-voip.md
│   ├── 03-configuration-cme-packet-tracer.md
│   ├── 04-qos-vlans-voip.md
│   ├── 05-securite-voip.md
│   ├── 06-depannage-voip.md
│   ├── voip_packet_tracer.md (TP)
│   └── VoIP_ToIP_Presentation.pptx
│
└── README.md (ce fichier)
```

---

## 🚀 Modules de formation

### Module 1 : Fondamentaux des Réseaux
- [x] Modèle OSI et TCP/IP
- [x] Adressage IPv4 et subnetting
- [ ] IPv6
- [ ] Protocoles réseau (DNS, DHCP, HTTP, FTP)
- [ ] Configuration routeur et switch

### Module 2 : Windows Server
- [x] Installation et configuration Windows Server 2025
- [x] Active Directory Domain Services (AD DS)
- [x] Gestion des utilisateurs, groupes et OUs
- [x] Stratégies de groupe (GPO)
- [x] Serveur FTP
- [x] Partages réseau (SMB)
- [ ] DHCP et DNS
- [ ] WDS (Windows Deployment Services)
- [ ] Hyper-V

### Module 3 : Linux
- [ ] Installation et configuration (Debian, Ubuntu, CentOS)
- [ ] Commandes de base et gestion de fichiers
- [ ] Gestion des utilisateurs et permissions
- [ ] Services réseau (Apache, Nginx, SSH)
- [ ] Scripts Bash
- [ ] Sécurité Linux (iptables, SELinux)

### Module 4 : Virtualisation
- [x] VMware Workstation
- [x] VirtualBox
- [ ] Hyper-V
- [ ] Docker (introduction)

### Module 5 : Sécurité
- [ ] Concepts de sécurité informatique
- [ ] Pare-feu Windows et Linux
- [ ] VPN (OpenVPN, IPsec)
- [ ] Antivirus et protection endpoint
- [ ] Sauvegardes et plans de reprise

### Module 6 : Support et Dépannage
- [ ] Méthodologie de diagnostic
- [ ] Outils de diagnostic réseau
- [ ] Résolution de problèmes Windows
- [ ] Résolution de problèmes Linux
- [ ] Documentation des incidents

### Module 11 : Téléphonie VoIP
- [x] Fondamentaux de la VoIP (concepts, architecture)
- [ ] Protocoles VoIP (SIP, RTP, SCCP, codecs)
- [ ] Configuration Call Manager Express (Cisco CME)
- [ ] QoS et VLANs voix
- [ ] Sécurité VoIP (authentification, chiffrement)
- [ ] Dépannage et optimisation

---

## 📝 Projets réalisés

### ✅ Projet 1 : Infrastructure Windows Server complète
**Date :** Janvier 2026  
**Description :** Mise en place d'un domaine Active Directory complet avec :
- Contrôleur de domaine (SOLARIS.local)
- Structure OUs (Direction, Comptabilité)
- Utilisateurs et groupes de sécurité
- GPO de mappage de lecteurs réseau
- Serveur FTP avec authentification AD
- Partages réseau sécurisés (NTFS + SMB)

**Documentation :** [Guide complet Windows Server](./02-Windows-Server/)

### 🔄 Projet 2 : À venir...

---

## 🛠️ Outils et environnement

### Matériel
- 💻 PC de développement : [Ta config]
- 🖥️ Lab virtualisé : VMware Workstation / VirtualBox

### Logiciels
- **Virtualisation :** VMware Workstation, VirtualBox
- **Systèmes :** Windows Server 2025, Windows 10/11, Ubuntu 24.04, Debian 12
- **IDE/Éditeurs :** VS Code, PowerShell ISE, Vim
- **Réseau :** Cisco Packet Tracer, Wireshark, PuTTY
- **Documentation :** Markdown, Draw.io, Obsidian

### Plateformes d'apprentissage
- 🎓 Formip (plateforme Nextformation)
- 🐧 Linux Journey
- 📚 Microsoft Learn
- 🌐 Cisco NetAcad
- 📖 Documentation officielle

---

## 📚 Ressources utiles

### Documentation officielle
- [Microsoft Docs](https://docs.microsoft.com)
- [Linux Documentation Project](https://tldp.org)
- [Cisco Documentation](https://www.cisco.com/c/en/us/support/index.html)

### Tutoriels et cours
- [IT-Connect](https://www.it-connect.fr)
- [LearnLinux.tv](https://www.learnlinux.tv)
- [PowerShell.org](https://powershell.org)

### Communautés
- [Reddit r/sysadmin](https://reddit.com/r/sysadmin)
- [Stack Overflow](https://stackoverflow.com)
- [Discord Nextformation](https://discord.gg/nextformation) *(lien exemple)*

### Certification
- 🏆 Objectif : **CCNA** (Cisco Certified Network Associate)
- 🏆 Objectif : **MCSA** (Microsoft Certified Solutions Associate) - si disponible

---

## 📈 Progression

| Module | Avancement | Statut |
|--------|-----------|--------|
| Réseaux | 🟢🟢🟢⚪⚪ | 60% |
| Windows Server | 🟢🟢🟢🟢⚪ | 80% |
| Linux | 🟢⚪⚪⚪⚪ | 20% |
| Virtualisation | 🟢🟢🟢⚪⚪ | 60% |
| Sécurité | ⚪⚪⚪⚪⚪ | 0% |
| Scripting | 🟢🟢⚪⚪⚪ | 40% |

**Dernière mise à jour :** 10 janvier 2026

---

## 🤝 Contribution

Ce dépôt est personnel mais **les suggestions et corrections sont les bienvenues** !

Si vous trouvez une erreur ou avez une amélioration à proposer :
1. Ouvrez une **Issue**
2. Proposez une **Pull Request**
3. Contactez-moi directement

---

## 📞 Contact

- 👤 **Nom :** Sonny
- 🎓 **Formation :** TSSR @ Nextformation
- 📅 **Période :** Décembre 2024 - Juin 2025
- 🌍 **Localisation :** Paris, France
- 💼 **LinkedIn :** [Votre profil LinkedIn]
- 📧 **Email :** sonny_levanneur@hotmail.com
- 🐙 **GitHub :** [@votre-username](https://github.com/votre-username)

---

## 📜 Licence

Ce projet est sous licence **MIT**. Vous êtes libre d'utiliser, modifier et distribuer ce contenu à des fins éducatives.

Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## ⭐ Remerciements

Un grand merci à :
- 🏫 **Nextformation** pour la formation
- 👨‍🏫 Les formateurs pour leur accompagnement
- 👥 Ma promotion pour l'entraide
- 🌐 La communauté IT pour les ressources partagées

---

## 🎯 Objectif professionnel

**Poste visé :** Technicien Support Niveau 2 / Administrateur Réseau  
**Région :** Île-de-France (Paris)  
**Disponibilité :** À partir d'avril 2025 (stage) puis juin 2025 (CDI)

**Compétences recherchées par les entreprises :**
- Support technique N2
- Administration Windows Server / Active Directory
- Gestion des infrastructures réseau
- Virtualisation (VMware, Hyper-V)
- Scripting PowerShell / Bash
- Anglais technique

---

<div align="center">

### 💪 "La pratique mène à la maîtrise"

**Fait avec ❤️ durant ma formation TSSR**

[⬆ Retour en haut](#-formation-tssr---cours-et-tutoriels)

</div>