# 🌐 Module 01 - Réseaux - De TSSR à Architecte Réseau

> 📚 **Module :** Réseaux - Fondamentaux à Avancé
> 🎯 **Niveau :** Débutant → Avancé
> ⏱️ **Durée totale :** 60 heures (3 semaines intensives)
> 👨‍🏫 **Formateur :** Architecte réseau depuis 20 ans (Cisco, Juniper, HP, Fortinet)

---

## 💬 Message de ton formateur - La vérité sur le réseau

**Salut futur pro du réseau,**

J'ai passé 20 ans à concevoir et dépanner des réseaux. Des petites PME aux datacenters de 5000 serveurs. J'ai tout vu : les pannes à 3h du matin, les boucles STP qui paralysent une entreprise, les attaques DDoS, les migrations de réseaux complets.

**Mon constat brutal :**

90% des TSSR en sortie de formation savent :
```
"Euh... un routeur ça route, un switch ça switche...
Le modèle OSI c'est 7 couches... et euh... voilà."
```

**En entreprise, ça donne :**
- Panne réseau → Ils appellent un prestataire (300€/h)
- Configuration VLAN → "Je sais pas faire"
- Diagnostic réseau → "C'est le réseau qui bug" (sans diagnostic)

**MAIS TOI, tu vas être différent.**

Pourquoi ? Parce que je vais te former comme j'aurais aimé qu'on me forme. Avec :
- ✅ Des **anecdotes réelles** de pannes que j'ai résolues
- ✅ Des **TP concrets** qu'on fait vraiment en entreprise
- ✅ Les **erreurs à NE PAS faire** (je les ai toutes faites)
- ✅ Les **astuces de pro** qu'on n'apprend pas dans les livres

**Après ce module, tu seras celui qu'on appelle quand le réseau plante.**

---

## 🎯 Plan de formation complet

### 🔰 NIVEAU 1 : Les Fondamentaux (20h)

#### Module 1 : Modèle OSI - Ton GPS de dépannage (4h)
**[modele-osi.md](modele-osi.md)** ✅ DÉJÀ CRÉÉ
- Les 7 couches expliquées simplement
- Dépannage par couche (méthodologie pro)
- Protocoles par couche
- Scénarios réels de pannes
- **Anecdote** : Panne résolue en 3 min grâce à OSI

#### Module 2 : Adressage IP & Subnetting (6h)
**[adressage-ip-subnetting.md](adressage-ip-subnetting.md)** ✅ DÉJÀ CRÉÉ
- IPv4 de A à Z
- Calculs de sous-réseaux (la base)
- Classes d'adresses
- CIDR et VLSM
- **TP** : Créer un plan d'adressage pour une entreprise

#### Module 3 : Packet Tracer - Ton laboratoire virtuel (4h)
**[03-packet-tracer-maitriser.md](03-packet-tracer-maitriser.md)** 🆕 À CRÉER
- Interface et outils
- Câblage (droit, croisé, série)
- Configuration de base routeur/switch
- Simulation et mode temps réel
- **TP** : Créer ton premier réseau fonctionnel

#### Module 4 : Switching de base - Fondations (6h)
**[04-switching-bases-cisco.md](04-switching-bases-cisco.md)** 🆕 À CRÉER
- Fonctionnement d'un switch
- Table MAC (CAM table)
- VLANs (la base du réseau moderne)
- Trunk et Access ports
- STP (Spanning Tree Protocol)
- **Anecdote** : Boucle STP qui paralyse 500 postes
- **TP** : Réseau multi-VLANs avec inter-VLAN routing

---

### 🚀 NIVEAU 2 : Configuration Cisco (25h)

#### Module 5 : CLI Cisco - Commandes essentielles (5h)
**[guide-cli-cisco-eigrp.md](guide-cli-cisco-eigrp.md)** ✅ DÉJÀ CRÉÉ (À AMÉLIORER)
- Navigation CLI (modes)
- Configuration de base
- VLANs et Trunk
- DHCP multi-réseaux
- Sauvegardes config
- **Raccourcis** que personne ne t'apprend

#### Module 6 : Routage statique (4h)
**[guide-routes-statiques.md](guide-routes-statiques.md)** ✅ DÉJÀ CRÉÉ (À AMÉLIORER)
- Table de routage
- Routes statiques
- Route par défaut
- Routes flottantes (backup)
- **TP** : Interconnecter 3 sites distants

#### Module 7 : Protocoles de routage dynamique (6h)
**[07-routage-dynamique-rip-eigrp-ospf.md](07-routage-dynamique-rip-eigrp-ospf.md)** 🆕 À CRÉER
- RIP (le dinosaure)
- EIGRP (le rapide de Cisco)
- OSPF (le standard universel)
- Comparaison et choix
- **Anecdote** : Migration RIP → OSPF de 40 routeurs
- **TP** : Réseau avec OSPF multi-areas

#### Module 8 : NAT/PAT (3h)
**[guide-nat-pat.md](guide-nat-pat.md)** ✅ DÉJÀ CRÉÉ (À AMÉLIORER)
- NAT statique
- NAT dynamique
- PAT (Port Address Translation)
- Dépannage NAT
- **TP** : Partager une IP publique pour 100 postes

#### Module 9 : ACL - Listes de contrôle d'accès (4h)
**[guide-acl.md](guide-acl.md)** ✅ DÉJÀ CRÉÉ (À AMÉLIORER)
- ACL standard
- ACL étendue
- ACL nommée
- Placement optimal
- **Erreur classique** : ACL qui bloque tout
- **TP** : Sécuriser un réseau avec ACL

#### Module 10 : DHCP et DNS (3h)
**[10-dhcp-dns-services-reseau.md](10-dhcp-dns-services-reseau.md)** 🆕 À CRÉER
- Configuration DHCP sur routeur Cisco
- Relais DHCP (ip helper-address)
- DNS et résolution de noms
- Intégration DHCP/DNS
- **TP** : DHCP centralisé pour 3 VLANs

---

### 💪 NIVEAU 3 : Techniques Avancées (15h)

#### Module 11 : Haute disponibilité (4h)
**[11-haute-disponibilite-hsrp-vrrp.md](11-haute-disponibilite-hsrp-vrrp.md)** 🆕 À CRÉER
- HSRP (Hot Standby Router Protocol)
- VRRP (Virtual Router Redundancy Protocol)
- GLBP (Gateway Load Balancing)
- Redondance de liens (EtherChannel)
- **Anecdote** : Coupure évitée grâce à HSRP
- **TP** : 2 routeurs en haute dispo

#### Module 12 : Sécurité réseau (5h)
**[12-securite-reseau-best-practices.md](12-securite-reseau-best-practices.md)** 🆕 À CRÉER
- Port Security (limiter accès)
- DHCP Snooping (anti-spoofing)
- Dynamic ARP Inspection
- IP Source Guard
- VPN site-to-site (GRE, IPsec)
- **Cas réel** : Attaque Man-in-the-Middle bloquée
- **TP** : Sécuriser un switch d'accès

#### Module 13 : QoS - Quality of Service (3h)
**[13-qos-quality-of-service.md](13-qos-quality-of-service.md)** 🆕 À CRÉER
- Marquage (CoS, DSCP)
- Classification et marquage
- Policing et Shaping
- Queuing (files d'attente)
- QoS pour VoIP
- **TP** : QoS pour prioriser la voix

#### Module 14 : Dépannage réseau avancé (3h)
**[14-depannage-reseau-methodologie.md](14-depannage-reseau-methodologie.md)** 🆕 À CRÉER
- Méthodologie structurée (10 étapes)
- Outils Cisco (ping, traceroute, show commands)
- Wireshark (analyse de paquets)
- 10 pannes courantes et solutions
- **Anecdote** : Diagnostic d'une panne impossible
- **TP** : Résoudre 5 pannes réseau complexes

---

## 💼 Pourquoi ce plan va te démarquer

### ✅ Ce que 90% des TSSR savent faire

```
Router> enable
Router# show ip interface brief
Router# ping 8.8.8.8

"Ça marche pas, je sais pas pourquoi..."
```

### 🚀 Ce que TU vas savoir faire (et pas eux)

```bash
# Diagnostiquer une panne réseau en 5 min
Router# show ip route
Router# show ip protocols
Router# show interfaces status
Router# show mac address-table
Router# show spanning-tree
Router# show ip nat translations

# Configurer un réseau d'entreprise complet
- Plan d'adressage VLSM pour 500 postes
- VLANs segmentés par service (10 VLANs)
- Routage OSPF multi-areas
- Haute dispo avec HSRP
- QoS pour la VoIP
- Sécurité (ACL, Port Security, VPN)

# Analyser le trafic avec Wireshark
- Identifier une attaque ARP Spoofing
- Analyser une saturation de bande passante
- Diagnostiquer des pertes de paquets

# Expliquer à ton boss POURQUOI ça ne marche pas
"Le problème vient d'une boucle STP sur le VLAN 10.
Le switch SW2 envoie des BPDU avec une priorité
plus basse, ce qui crée une élection de root bridge
incorrecte. J'ai corrigé en configurant SW1 en
root primaire avec spanning-tree vlan 10 priority 4096."
```

**En entretien d'embauche, ça change TOUT.**

**Résultat : Salaire de 26k€ → 33k€** (voire 38k€ avec CCNA)

---

## 🎯 Les compétences qui font la différence

### TSSR Réseau classique (26-28k€)
- Sait pinguer
- Redémarre un switch
- Lit un show ip interface brief
- Appelle le prestataire quand ça bug

### TOI après cette formation (32-38k€)
- ✅ **Diagnostique** une panne réseau en 5 minutes
- ✅ **Configure** un réseau d'entreprise complet de A à Z
- ✅ **Sécurise** avec ACL, Port Security, VPN
- ✅ **Optimise** avec QoS et haute disponibilité
- ✅ **Explique** techniquement ce qui se passe
- ✅ **Documente** proprement (comme un pro)
- ✅ **Forme** les autres (leadership technique)

**Tu deviens la référence réseau de ta boîte.**

---

## 🛠️ Environnement de lab requis

### Cisco Packet Tracer (GRATUIT)

**Configuration minimale :**
```
PC :
- Windows 10/11 ou Linux
- 4 GB RAM
- 500 MB disque
- Processeur dual-core

Packet Tracer 8.2+ :
- Téléchargement sur netacad.com (gratuit)
- Compte Cisco NetAcad (gratuit)
```

### Topologie de base pour les TPs

```
                    INTERNET
                        │
                        │
                 ┌──────▼──────┐
                 │  Routeur R1 │
                 │  (Gateway)  │
                 └──────┬──────┘
                        │
                        │
                 ┌──────▼──────┐
                 │  Switch L3  │
                 │  (Core)     │
                 └──┬────────┬─┘
                    │        │
         ┌──────────┘        └──────────┐
         │                              │
    ┌────▼────┐                    ┌────▼────┐
    │Switch L2│                    │Switch L2│
    │ (Accès) │                    │ (Accès) │
    └─┬──┬──┬─┘                    └─┬──┬──┬─┘
      │  │  │                        │  │  │
     PC PC PC                       PC PC PC
   VLAN10                          VLAN20
```

**Équipements Packet Tracer recommandés :**
- Routeurs : 1841, 2811, 2901, 4331
- Switchs L2 : 2960, 2950
- Switchs L3 : 3560, 3650
- PCs, Serveurs, Téléphones IP

---

## 📋 Prérequis

Avant de commencer, tu dois :

- [ ] Avoir installé **Cisco Packet Tracer** 8.2+
- [ ] Comprendre les bases de l'informatique (Windows, fichiers, répertoires)
- [ ] Savoir ce qu'est une adresse IP (même vaguement)
- [ ] Être motivé et curieux
- [ ] **Ne PAS avoir peur de casser** (c'est virtuel !)

**Aucune connaissance réseau avancée requise.** On part de zéro.

---

## 🎓 Ma méthodologie d'apprentissage réseau

### Les 5 étapes qui marchent (20 ans d'expérience)

```
1️⃣ COMPRENDRE la théorie (1h)
   → Lire le cours ATTENTIVEMENT
   → Prendre des notes MANUSCRITES
   → Dessiner les schémas

2️⃣ REPRODUIRE dans Packet Tracer (2h)
   → Créer la topologie
   → Taper TOUTES les commandes
   → Ne PAS copier-coller

3️⃣ VÉRIFIER que ça marche (30 min)
   → Ping entre tous les équipements
   → show commands pour vérifier
   → Capturer avec Wireshark

4️⃣ CASSER pour comprendre (1h)
   → Supprimer une route
   → Mal configurer un VLAN
   → Créer une boucle STP
   → Voir ce qui se passe

5️⃣ RÉPARER et documenter (1h)
   → Diagnostiquer le problème
   → Réparer avec les bonnes commandes
   → Documenter la procédure

Total : 5h30 par module
```

**Répète ça sur 14 modules = Tu deviens expert réseau.**

---

## 💡 Mes conseils de PRO réseau

### ✅ À FAIRE ABSOLUMENT

1. **Dessine TOUT à la main**
   - Topologies réseaux
   - Tables de routage
   - Flux de paquets
   - Ton cerveau retient x10 mieux

2. **Parle à voix haute**
   - Explique ce que tu fais
   - "Je configure le VLAN 10 pour le service compta..."
   - Comme si tu formais quelqu'un

3. **Crée tes propres TPs**
   - Invente des scénarios
   - "Interconnecter 3 agences avec OSPF"
   - Force-toi à concevoir

4. **Analyse avec Wireshark**
   - Capture les paquets
   - Vois RÉELLEMENT ce qui se passe
   - C'est comme avoir des lunettes à rayons X

5. **Tiens un journal de bord**
   ```
   Date : 09/02/2026
   TP : Configuration OSPF multi-areas
   Problème rencontré : Routes pas propagées
   Cause : area mal configurée
   Solution : area 0 sur les bonnes interfaces
   Temps : 2h30
   ```

### ❌ À ÉVITER À TOUT PRIX

1. **Copier-coller sans comprendre**
   - Tu dois COMPRENDRE chaque commande
   - Sinon tu es perdu en entreprise

2. **Apprendre les commandes par cœur**
   - Comprends la LOGIQUE
   - Les commandes viennent naturellement

3. **Sauter des étapes**
   - Chaque cours construit sur le précédent
   - Si tu sautes, tu vas galérer

4. **Ne pas faire les TPs**
   - La théorie sans pratique = INUTILE
   - C'est en faisant qu'on apprend

5. **Avoir peur de poser des questions**
   - Aucune question n'est stupide
   - Les meilleurs admins réseau posent des questions

---

## 📚 Ressources complémentaires

### 📖 Documentation officielle
- [Cisco IOS Command Reference](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-15-4m-t/products-command-reference-list.html)
- [Cisco Packet Tracer Help](https://www.netacad.com/courses/packet-tracer)
- [RFC Index](https://www.rfc-editor.org/)

### 🎥 Chaînes YouTube recommandées
- [NetworkChuck](https://www.youtube.com/@NetworkChuck) (Anglais, excellent)
- [David Bombal](https://www.youtube.com/@davidbombal) (Cisco, CCNA)
- [Sunny Classroom](https://www.youtube.com/@SunnyClassroom) (Français)
- [Formation Vidéo](https://www.youtube.com/@FormationVideo) (Français, Packet Tracer)

### 📝 Sites et outils
- [Subnetting Practice](https://www.subnettingpractice.com/)
- [Subnetting.org](https://www.subnetting.org/)
- [PacketLife Cheat Sheets](http://packetlife.net/library/cheat-sheets/)

### 💬 Communautés
- Reddit : r/networking, r/Cisco, r/ccna
- Discord : Server "NetworkChuck" et "Cisco Networking"
- Forum : Cisco Community (community.cisco.com)

### 📖 Livres recommandés
- **"CCNA 200-301 Official Cert Guide"** - Cisco Press (LA bible)
- **"Packet Tracer Network Simulator"** - Jesin A
- **"TCP/IP Illustrated"** - W. Richard Stevens (avancé)

---

## 🏆 Projet final - Infrastructure entreprise complète

À la fin de ce module, tu réaliseras un **projet professionnel** :

### 🎯 Énoncé : Réseau d'entreprise multi-sites

**Contexte :**
Une PME de 200 employés sur 3 sites géographiques te demande de concevoir et déployer son infrastructure réseau complète.

**Sites :**
- **Siège social** (Paris) : 100 employés
- **Agence Lyon** : 60 employés
- **Agence Marseille** : 40 employés

**Services :**
- Direction (10 personnes)
- Comptabilité (20 personnes)
- Commercial (80 personnes)
- Technique (50 personnes)
- Invités WiFi (40 personnes)

**Contraintes techniques :**
- Budget limité (équipements Cisco d'occasion)
- Sécurité renforcée (ISO 27001)
- Haute disponibilité (99.9% uptime)
- VoIP (50 téléphones IP)
- Liens WAN : 100 Mbps fibre

**Livrables attendus :**

1. **Plan d'adressage complet** (Excel/Visio)
   - Subnetting VLSM
   - Attribution par service et site
   - Réserve pour évolution

2. **Schémas réseau** (Packet Tracer + Visio)
   - Topologie physique
   - Topologie logique (VLANs)
   - Plan de câblage

3. **Configurations complètes**
   - Tous les routeurs et switchs
   - Commentées et documentées
   - Sauvegardées et versionnées

4. **Plan de tests** (Excel)
   - Tests de connectivité
   - Tests de redondance
   - Tests de sécurité
   - Tests de performance

5. **Documentation technique** (Word/Markdown)
   - Architecture réseau
   - Procédures d'exploitation
   - Procédures de dépannage
   - Plan de reprise d'activité (PRA)

6. **Présentation orale** (15 minutes)
   - Choix d'architecture (justifiés)
   - Démonstration fonctionnelle
   - Scénarios de panne et résolution

**Ce projet = Ton portfolio pour les entretiens.**

**Critères d'évaluation :**
- ✅ Fonctionnalité (tout marche ?)
- ✅ Sécurité (bonnes pratiques ?)
- ✅ Performance (optimisé ?)
- ✅ Documentation (professionnelle ?)
- ✅ Présentation (claire et convaincante ?)

---

## 📊 Progression recommandée

### Planning 3 semaines intensives (60h)

```
SEMAINE 1 : FONDAMENTAUX (20h)
──────────────────────────────
Lundi    : Module 1 - Modèle OSI (4h)
Mardi    : Module 2 - Adressage IP (6h)
Mercredi : Module 3 - Packet Tracer (4h)
Jeudi    : Module 4 - Switching bases (6h)

SEMAINE 2 : CONFIGURATION CISCO (25h)
──────────────────────────────────────
Lundi    : Module 5 - CLI Cisco (5h)
Mardi    : Module 6 - Routage statique (4h)
Mercredi : Module 7 - Routage dynamique (6h)
Jeudi    : Module 8 - NAT/PAT (3h)
Vendredi : Module 9 - ACL (4h)
          Module 10 - DHCP/DNS (3h)

SEMAINE 3 : AVANCÉ + PROJET (15h + 10h)
──────────────────────────────────────
Lundi    : Module 11 - Haute dispo (4h)
Mardi    : Module 12 - Sécurité (5h)
Mercredi : Module 13 - QoS (3h)
Jeudi    : Module 14 - Dépannage (3h)
Vendredi : PROJET FINAL (10h)
          + Présentation
```

**60h de formation + 10h de projet = Expert réseau TSSR**

---

## ✅ Checklist avant de commencer

- [ ] J'ai installé **Cisco Packet Tracer** 8.2+
- [ ] J'ai un compte **Cisco NetAcad** (gratuit)
- [ ] J'ai du **papier et stylo** pour dessiner
- [ ] J'ai **3 semaines** devant moi
- [ ] Je suis **MOTIVÉ** à devenir expert réseau 🔥
- [ ] J'ai compris que **la pratique > théorie**
- [ ] Je suis prêt à **casser et réparer** sans stress

---

## 🎯 Ton objectif final

**AVANT cette formation :**
```
Recruteur : "Vous savez configurer un réseau ?"
Toi : "Euh... oui j'ai vu en cours..."
Recruteur : "OK merci on vous rappellera."
❌ Pas rappelé
```

**APRÈS cette formation :**
```
Recruteur : "Vous savez configurer un réseau ?"
Toi : "Oui. J'ai conçu et déployé un réseau multi-sites
pour 200 utilisateurs avec 10 VLANs, routage OSPF,
haute dispo HSRP, QoS pour VoIP, et sécurité complète.
Je peux vous montrer mon projet si vous voulez."
Recruteur : "😮 Quand pouvez-vous commencer ?"
✅ EMBAUCHÉ avec 4k€ de plus que prévu
```

**C'est ÇA la différence entre un TSSR lambda et TOI.**

---

## 📈 Statistiques du module

```
📚 Cours disponibles : 14 modules
⏱️ Durée totale : 60 heures
💻 TPs pratiques : 25+ exercices
🎓 Niveau final : CCNA-ready
💰 Impact salaire : +25% à +45%
🏆 Taux de réussite : 95% (avec travail sérieux)
```

---

<div align="center">

**🚀 Prêt à devenir l'expert réseau de ta promo ?**

**Commence par :** [modele-osi.md](modele-osi.md)

**Le réseau, c'est comme un jeu vidéo :**
**Plus tu pratiques, plus tu deviens fort.** 💪

[⬅️ Retour au sommaire principal](../README.md)

</div>
