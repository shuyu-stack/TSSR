# 📋 Plan de Réorganisation - Structure des Cours TSSR

## 🎯 Objectif
Réorganiser tous les fichiers selon la structure définie dans le README pour une hiérarchie claire et logique.

---

## 📂 Structure ACTUELLE (avant réorganisation)

```
Git-TSSR/
├── README.md
├── TEMPLATE-cours.md
├── planning-formation-tssr.md
├── progression.md
├── Guide_Configuration_GPO_Etape_Par_Etape.md     ← À déplacer
├── Guide_GPO_Avancees.md                          ← À déplacer
├── Guide_VMware_Lab_Windows_Server.md             ← À déplacer
├── Modules/
│   ├── 01-Reseaux/
│   │   ├── adressage-ip-subnetting.md
│   │   └── modele-osi.md
│   └── 02-Windows-Server/
│       ├── Active-directory.md
│       ├── dns-dhcp-windows-server.md
│       ├── fsrm-quotas.md
│       ├── gpo-strategies-groupe.md
│       ├── serveur-ftp.md
│       ├── windows-client-10-11.md
│       └── wins-windows-internet-name-service.md
└── Ressources/
    ├── guide-calcul-reseau.md
    ├── Guide_Choix_Plages_IP_DHCP.md
    └── visio-schemas-reseau.md
```

---

## 📂 Structure CIBLE (après réorganisation)

```
Git-TSSR/
├── 📄 README.md                              (racine - inchangé)
├── 📄 TEMPLATE-cours.md                      (racine - inchangé)
├── 📄 planning-formation-tssr.md             (racine - inchangé)
├── 📄 progression.md                         (racine - inchangé)
│
├── 📁 01-Reseaux/
│   ├── modele-osi.md                         (déplacé de Modules/01-Reseaux/)
│   ├── adressage-ip-subnetting.md            (déplacé de Modules/01-Reseaux/)
│   └── 📄 README.md                          (à créer)
│
├── 📁 02-Windows-Server/
│   ├── Active-directory.md                   (déplacé de Modules/02-Windows-Server/)
│   ├── dns-dhcp-windows-server.md            (déplacé de Modules/02-Windows-Server/)
│   ├── fsrm-quotas.md                        (déplacé de Modules/02-Windows-Server/)
│   ├── gpo-strategies-groupe.md              (déplacé de Modules/02-Windows-Server/)
│   ├── serveur-ftp.md                        (déplacé de Modules/02-Windows-Server/)
│   ├── windows-client-10-11.md               (déplacé de Modules/02-Windows-Server/)
│   ├── wins-windows-internet-name-service.md (déplacé de Modules/02-Windows-Server/)
│   ├── Guide_Configuration_GPO_Etape_Par_Etape.md  (déplacé de racine)
│   ├── Guide_GPO_Avancees.md                 (déplacé de racine)
│   └── 📄 README.md                          (à créer)
│
├── 📁 03-Linux/
│   ├── 📄 README.md                          (à créer)
│   └── (à venir)
│
├── 📁 04-Virtualisation/
│   ├── Guide_VMware_Lab_Windows_Server.md    (déplacé de racine) ⭐
│   └── 📄 README.md                          (à créer)
│
├── 📁 05-Securite/
│   ├── 📄 README.md                          (à créer)
│   └── (à venir)
│
├── 📁 06-Scripting/
│   ├── 📄 README.md                          (à créer)
│   └── (à venir)
│
├── 📁 07-Projets/
│   ├── 📄 README.md                          (à créer)
│   └── (à venir)
│
├── 📁 08-Certifications/
│   ├── 📄 README.md                          (à créer)
│   └── (à venir)
│
└── 📁 09-Ressources/
    ├── guide-calcul-reseau.md                (déplacé de Ressources/)
    ├── Guide_Choix_Plages_IP_DHCP.md         (déplacé de Ressources/)
    ├── visio-schemas-reseau.md               (déplacé de Ressources/)
    └── 📄 README.md                          (à créer)
```

---

## 🔄 Actions à effectuer

### Phase 1 : Créer les dossiers manquants
```bash
mkdir 01-Reseaux
mkdir 02-Windows-Server
mkdir 03-Linux
mkdir 04-Virtualisation
mkdir 05-Securite
mkdir 06-Scripting
mkdir 07-Projets
mkdir 08-Certifications
mkdir 09-Ressources
```

### Phase 2 : Déplacer les fichiers depuis Modules/
```bash
# Depuis Modules/01-Reseaux/
mv Modules/01-Reseaux/modele-osi.md 01-Reseaux/
mv Modules/01-Reseaux/adressage-ip-subnetting.md 01-Reseaux/

# Depuis Modules/02-Windows-Server/
mv Modules/02-Windows-Server/Active-directory.md 02-Windows-Server/
mv Modules/02-Windows-Server/dns-dhcp-windows-server.md 02-Windows-Server/
mv Modules/02-Windows-Server/fsrm-quotas.md 02-Windows-Server/
mv Modules/02-Windows-Server/gpo-strategies-groupe.md 02-Windows-Server/
mv Modules/02-Windows-Server/serveur-ftp.md 02-Windows-Server/
mv Modules/02-Windows-Server/windows-client-10-11.md 02-Windows-Server/
mv Modules/02-Windows-Server/wins-windows-internet-name-service.md 02-Windows-Server/
```

### Phase 3 : Déplacer les nouveaux guides créés aujourd'hui
```bash
# Guides GPO → Windows Server
mv Guide_Configuration_GPO_Etape_Par_Etape.md 02-Windows-Server/
mv Guide_GPO_Avancees.md 02-Windows-Server/

# Guide VMware → Virtualisation
mv Guide_VMware_Lab_Windows_Server.md 04-Virtualisation/
```

### Phase 4 : Déplacer depuis Ressources/
```bash
mv Ressources/guide-calcul-reseau.md 09-Ressources/
mv Ressources/Guide_Choix_Plages_IP_DHCP.md 09-Ressources/
mv Ressources/visio-schemas-reseau.md 09-Ressources/
```

### Phase 5 : Supprimer les anciens dossiers vides
```bash
rmdir Modules/01-Reseaux
rmdir Modules/02-Windows-Server
rmdir Modules
rmdir Ressources
```

### Phase 6 : Créer les README.md pour chaque dossier
- 01-Reseaux/README.md
- 02-Windows-Server/README.md
- 03-Linux/README.md
- 04-Virtualisation/README.md
- 05-Securite/README.md
- 06-Scripting/README.md
- 07-Projets/README.md
- 08-Certifications/README.md
- 09-Ressources/README.md

---

## ✅ Validation finale

Après réorganisation, vérifier :
- [ ] Tous les fichiers sont dans les bons dossiers
- [ ] Aucun fichier perdu
- [ ] Anciens dossiers (Modules/, Ressources/) supprimés
- [ ] README.md créés pour chaque module
- [ ] Liens dans le README principal mis à jour si nécessaire
- [ ] Structure Git propre (git status)

---

## 📊 Résumé des déplacements

| Fichier | Depuis | Vers |
|---------|--------|------|
| modele-osi.md | Modules/01-Reseaux/ | 01-Reseaux/ |
| adressage-ip-subnetting.md | Modules/01-Reseaux/ | 01-Reseaux/ |
| Active-directory.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| dns-dhcp-windows-server.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| fsrm-quotas.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| gpo-strategies-groupe.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| serveur-ftp.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| windows-client-10-11.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| wins-windows-internet-name-service.md | Modules/02-Windows-Server/ | 02-Windows-Server/ |
| **Guide_Configuration_GPO_Etape_Par_Etape.md** | **Racine** | **02-Windows-Server/** |
| **Guide_GPO_Avancees.md** | **Racine** | **02-Windows-Server/** |
| **Guide_VMware_Lab_Windows_Server.md** | **Racine** | **04-Virtualisation/** |
| guide-calcul-reseau.md | Ressources/ | 09-Ressources/ |
| Guide_Choix_Plages_IP_DHCP.md | Ressources/ | 09-Ressources/ |
| visio-schemas-reseau.md | Ressources/ | 09-Ressources/ |

**Total : 15 fichiers déplacés**

---

## 🚀 Exécution

**Prêt à exécuter la réorganisation ?**

Taper "OUI" pour lancer la réorganisation automatique.
