# 💻 Module 04 - Virtualisation

## 📚 Contenu du module

Ce module couvre la **virtualisation** et la création d'environnements de lab pour la pratique.

### 📖 Cours disponibles

- **[Guide VMware Lab Windows Server](Guide_VMware_Lab_Windows_Server.md)** ⭐ - Guide complet (900+ lignes)

### 🎯 Compétences visées

- ✅ VMware Workstation - Configuration et gestion
- ✅ Création de machines virtuelles optimisées
- ✅ Configuration réseau virtuel (NAT, Host-Only, Bridge)
- ✅ Gestion des snapshots et clonage
- ✅ Optimisation des performances
- ⏳ VirtualBox
- ⏳ Hyper-V
- ⏳ Docker (introduction)

### 📋 Guide VMware complet

Le **[Guide VMware Lab Windows Server](Guide_VMware_Lab_Windows_Server.md)** couvre :

1. Architecture du lab recommandée
2. Configuration VMware Workstation/Player
3. Création de VMs optimisées
4. Configuration réseau (VMnet8, VMnet1)
5. Installation Windows Server
6. Configuration IP statique
7. **Snapshots** - Points de restauration critiques
8. **Clonage de VMs** - Création rapide de clients
9. Optimisation des performances
10. Troubleshooting VMware

### 🛠️ Logiciels utilisés

- **VMware Workstation Pro** (version 17.x)
- VMware Player (gratuit)
- VirtualBox (open source)
- Hyper-V (Windows)

### 💡 Architecture type du lab

```
VMware Workstation
├── SRV-DC01 (Windows Server 2022)
│   ├── RAM: 4-8 Go
│   ├── CPU: 2 cœurs
│   ├── Disque: 60 Go
│   └── IP: 192.168.10.10
├── PC-CLIENT01 (Windows 10/11)
│   ├── RAM: 4 Go
│   ├── CPU: 2 cœurs
│   └── IP: DHCP
└── PC-CLIENT02 (Windows 10/11)
    └── (clone de CLIENT01)
```

### 📈 Progression

Module : 🟢🟢🟢⚪⚪ (60%)

---

[⬅ Retour au sommaire principal](../README.md)
