# 📞 Téléphonie VoIP - Voice over IP

> 📚 **Module :** Réseaux - Téléphonie sur IP
> 🎯 **Niveau :** Intermédiaire
> ⏱️ **Durée totale :** 20 heures (5 jours)
> 👨‍🏫 **Formateur :** Architecte réseau - 15 ans d'expérience VoIP

---

## 📖 Présentation du module

La **VoIP** (Voice over IP) est devenue incontournable dans toutes les entreprises. Ce module vous apprend à **configurer**, **sécuriser** et **dépanner** une infrastructure de téléphonie IP professionnelle.

### 🎯 Ce que vous allez apprendre

- ✅ Comprendre les protocoles VoIP (SIP, RTP, SCCP)
- ✅ Configurer un Call Manager Express sur routeur Cisco
- ✅ Mettre en place la QoS pour garantir la qualité vocale
- ✅ Sécuriser votre infrastructure VoIP
- ✅ Diagnostiquer et résoudre les problèmes courants

### 💼 Retour d'expérience terrain

Tous les cours sont basés sur **des projets réels** :
- Migration PBX Alcatel → Cisco UC (1200 utilisateurs)
- Déploiements multi-sites avec QoS
- Dépannage d'incidents en production
- Optimisation de la bande passante

---

## 📚 Plan de formation

### Module 1 : Fondamentaux (4h)
**[01-fondamentaux-voip.md](01-fondamentaux-voip.md)**
- Qu'est-ce que la VoIP ?
- VoIP vs téléphonie traditionnelle
- Histoire et évolution
- Concepts de base (codec, bande passante, latence)
- Architecture réseau VoIP

### Module 2 : Protocoles VoIP (4h)
**[02-protocoles-voip.md](02-protocoles-voip.md)**
- SIP (Session Initiation Protocol) ⭐ Le plus important
- RTP (Real-time Transport Protocol)
- SCCP (Skinny Client Control Protocol - Cisco)
- H.323 (ancien standard)
- Codecs audio (G.711, G.729, Opus)

### Module 3 : Configuration Cisco CME (6h)
**[03-configuration-cme-packet-tracer.md](03-configuration-cme-packet-tracer.md)**
- Configuration Call Manager Express
- DHCP avec option 150 (TFTP)
- Enregistrement des téléphones IP
- Plan de numérotation
- Fonctionnalités avancées (renvoi, conférence, messagerie)
- **TP Packet Tracer** : Déploiement complet

### Module 4 : QoS et VLANs (3h)
**[04-qos-vlans-voip.md](04-qos-vlans-voip.md)**
- Pourquoi séparer voix et données ?
- Configuration VLAN voix
- QoS : marquage CoS et DSCP
- Priorisation du trafic vocal
- Calculs de bande passante

### Module 5 : Sécurité VoIP (2h)
**[05-securite-voip.md](05-securite-voip.md)**
- Menaces VoIP (Toll Fraud, écoute clandestine, DoS)
- Authentification SIP
- Chiffrement (TLS, SRTP)
- Protection firewall et ACL
- Bonnes pratiques

### Module 6 : Dépannage et optimisation (3h)
**[06-depannage-voip.md](06-depannage-voip.md)**
- Méthodologie de diagnostic (7 étapes)
- Problèmes courants et solutions
- Analyse Wireshark (SIP, RTP)
- Commandes de vérification
- Outils du technicien

---

## 🛠️ Outils nécessaires

### Logiciels
- **Cisco Packet Tracer** 8.x (simulation VoIP)
- **Wireshark** (capture et analyse)
- **Softphone** (X-Lite, Zoiper, 3CX Phone)

### Matériel (optionnel en production)
- Téléphones IP Cisco (7841, 8841)
- Routeur Cisco ISR (2911, 4331) avec licence CME
- Switch Cisco avec PoE

---

## 📋 Prérequis

Avant de commencer ce module, vous devez maîtriser :

- [ ] **Réseau de base** : modèle OSI, TCP/IP, adressage IP
- [ ] **Configuration Cisco** : modes CLI, VLANs, routage basique
- [ ] **Packet Tracer** : utilisation de base
- [ ] **Commandes réseau** : ping, traceroute, show commands

**Si vous avez des lacunes**, commencez par :
- [Modèle OSI](../modele-osi.md)
- [Adressage IP et Subnetting](../adressage-ip-subnetting.md)
- [Guide CLI Cisco](../guide-cli-cisco-eigrp.md)

---

## 🎓 Certifications liées

Ce module vous prépare aux certifications :

- **Cisco CCNA** (partie Collaboration)
- **Cisco CCNA Voice** (certification spécialisée)
- **CompTIA Network+** (section VoIP)

---

## 📝 Méthode d'apprentissage recommandée

### Étape 1 : Théorie (40%)
1. Lire **attentivement** chaque cours
2. Prendre des **notes manuscrites** (mémorisation++)
3. Comprendre le **pourquoi**, pas juste le **comment**

### Étape 2 : Pratique (50%)
1. Reproduire **tous les TP** dans Packet Tracer
2. **Casser** votre configuration pour comprendre les erreurs
3. Dépanner **sans regarder les solutions** d'abord

### Étape 3 : Révision (10%)
1. Refaire les **exercices** 1 semaine après
2. Expliquer les concepts **à quelqu'un** (ou à voix haute)
3. Créer vos **propres scénarios** de TP

---

## 💡 Conseils de votre formateur

### ✅ À FAIRE
- Pratiquez **TOUS LES JOURS** (même 30 minutes)
- Faites des **erreurs** (c'est comme ça qu'on apprend)
- Posez des **questions** (aucune question n'est stupide)
- Documentez vos **configurations** (comme en prod)

### ❌ À ÉVITER
- Apprendre **par cœur** sans comprendre
- Copier-coller les configs **sans réfléchir**
- Passer à la suite **sans maîtriser** le chapitre en cours
- Ignorer les **messages d'erreur** (ils sont vos amis !)

---

## 📚 Ressources complémentaires

### Documentation officielle
- [Cisco CallManager Express](https://www.cisco.com/c/en/us/support/unified-communications/unified-communications-manager-express/tsd-products-support-series-home.html)
- [RFC 3261 - SIP](https://www.rfc-editor.org/rfc/rfc3261)
- [Cisco VoIP Design Guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucm/srnd/collab11/collab11.html)

### Fichiers fournis
- **[voip_packet_tracer.md](voip_packet_tracer.md)** - Lab complet Packet Tracer
- **[VoIP_ToIP_Presentation.pptx](VoIP_ToIP_Presentation.pptx)** - Présentation PowerPoint

### Outils en ligne
- [Calculateur codec VoIP](http://www.erlang.com/calculator/lipb/)
- [Simulateur QoS](https://www.cisco.com/c/en/us/support/docs/quality-of-service-qos/qos-policing/22833-qos-faq.html)

---

## 📊 Progression recommandée

```
Semaine 1 : Fondamentaux + Protocoles          (Cours 1 & 2)
Semaine 2 : Configuration CME                  (Cours 3)
Semaine 3 : QoS + Sécurité                     (Cours 4 & 5)
Semaine 4 : Dépannage + Projet final           (Cours 6 + TP)
```

---

## 🎯 Projet final

À la fin de ce module, vous devrez réaliser un **projet complet** :

### Énoncé
Déployer une infrastructure VoIP pour une PME de 50 utilisateurs sur 2 sites distants.

**Contraintes :**
- Budget limité (solutions Cisco CME)
- Lien WAN 2 Mbps entre les sites
- Qualité vocale garantie (QoS)
- Sécurité (authentification, chiffrement)

**Livrables :**
- Schéma réseau
- Configurations complètes (routeurs, switchs)
- Plan de numérotation
- Documentation de dépannage
- Présentation orale (10 min)

---

## ✅ Checklist avant de commencer

Assurez-vous d'avoir :

- [ ] Installé **Cisco Packet Tracer** 8.x
- [ ] Installé **Wireshark**
- [ ] Accès à un **PC/VM Windows ou Linux**
- [ ] **30 heures** disponibles pour le module complet
- [ ] **Envie d'apprendre** (le plus important !)

---

<div align="center">

**🚀 Prêt à démarrer ?**

**Commencez par :** [01-fondamentaux-voip.md](01-fondamentaux-voip.md)

[⬅️ Retour au module Réseaux](../README.md)

</div>
