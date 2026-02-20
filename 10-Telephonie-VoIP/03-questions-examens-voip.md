# Questions d'examen VoIP - Préparation

> 📚 **Module :** Téléphonie VoIP - Questions/Réponses
> 📅 **Date :** Février 2026
> 🎯 **Objectif :** Réviser les concepts clés pour les examens
> ⏱️ **Durée :** Révision rapide

---

## 📖 Table des matières

- [Introduction](#introduction)
- [Questions ouvertes](#questions-ouvertes)
- [Conseils pour l'examen](#conseils-pour-lexamen)

---

## Introduction

Ce document regroupe les questions types que vous pouvez rencontrer lors d'un examen sur la VoIP, avec des réponses simples et compréhensibles. Les questions sont issues de vrais examens.

**Comment utiliser ce document :**
1. Lisez d'abord la question
2. Essayez d'y répondre mentalement
3. Vérifiez votre réponse
4. Notez les points que vous ne maîtrisez pas encore

---

## Questions ouvertes

### Question 1 : Conversion de la voix analogique

**Question :** Dans une communication VoIP, la voix analogique est :

**Réponse :**
La voix analogique est **convertie en données numériques**.

**Explication :**
Quand vous parlez dans un téléphone IP, votre voix (onde sonore analogique) est :
1. **Captée** par le microphone
2. **Numérisée** par un convertisseur analogique-numérique (ADC)
3. **Compressée** par un codec (G.711, G.729, etc.)
4. **Encapsulée** dans des paquets IP
5. **Transmise** sur le réseau

C'est le contraire de la téléphonie classique (RTC) qui transporte la voix sous forme analogique sur des fils de cuivre.

---

### Question 2 : Protocole de signalisation

**Question :** Quel protocole gère la signalisation des appels ?

**Réponse :**
**SIP** (Session Initiation Protocol)

**Explication :**
SIP est le protocole standard qui gère :
- L'établissement des appels (INVITE)
- La terminaison des appels (BYE)
- L'enregistrement des téléphones (REGISTER)
- La modification des sessions en cours

**Autres protocoles de signalisation existants :**
- **SCCP** (Skinny) : protocole propriétaire Cisco
- **H.323** : ancien standard, aujourd'hui obsolète

**Analogie :** SIP, c'est comme le serveur dans un restaurant qui prend votre commande et coordonne, mais ne cuisine pas le plat.

---

### Question 3 : Ce que transporte le protocole SIP

**Question :** Le protocole SIP transporte :

**Réponse :**
SIP transporte **la signalisation des appels** (les messages de contrôle), PAS la voix elle-même.

**Explication :**
SIP ne transporte QUE les informations de contrôle :
- Qui appelle qui ?
- Quel numéro est composé ?
- Est-ce que le destinataire est disponible ?
- Quelle est l'adresse IP pour envoyer la voix ?
- Quel codec utiliser ?

**Important :** La voix elle-même est transportée par **RTP** (Real-time Transport Protocol), pas par SIP !

**Schéma simplifié :**
```
SIP   → Contrôle (qui, quand, comment)
RTP   → Voix (le son lui-même)
```

---

### Question 4 : Sur quoi fonctionne le protocole RTP

**Question :** Le protocole RTP fonctionne principalement sur :

**Réponse :**
RTP fonctionne sur **UDP** (User Datagram Protocol)

**Explication :**
RTP utilise UDP plutôt que TCP car :
- ✅ **Rapidité** : Pas de contrôle de réception (pas d'ACK)
- ✅ **Temps réel** : Pas de retransmission des paquets perdus
- ✅ **Faible latence** : Essentiel pour la voix en direct

**Pourquoi pas TCP ?**
- ❌ TCP retransmet les paquets perdus → délai inacceptable pour la voix
- ❌ TCP garantit l'ordre → attente qui crée des coupures
- ❌ TCP a plus d'overhead (en-têtes plus gros)

**En résumé :**
- **SIP** → Peut utiliser TCP ou UDP (généralement UDP)
- **RTP** → Toujours UDP (temps réel exige la rapidité)

---

### Question 5 : Port par défaut du SIP

**Question :** Le port par défaut du SIP est :

**Réponse :**
**Port 5060** (UDP ou TCP)

**Explication :**
- **5060** : SIP non sécurisé (clair)
- **5061** : SIP sécurisé avec TLS (chiffré)

**À retenir :**
```
SIP standard  : 5060
SIP sécurisé  : 5061
```

**Exemple de configuration firewall :**
```
permit udp any any eq 5060  (SIP)
permit tcp any any eq 5061  (SIP-TLS)
```

---

### Question 6 : Port sécurisé du SIP (TLS)

**Question :** Le port sécurisé du SIP (TLS) est généralement :

**Réponse :**
**Port 5061** (TCP avec TLS)

**Explication :**
TLS (Transport Layer Security) chiffre la signalisation SIP pour :
- ✅ Protéger les numéros composés
- ✅ Protéger les identités (qui appelle qui)
- ✅ Empêcher l'interception des messages SIP

**Important :**
- SIP-TLS (port 5061) protège **la signalisation**
- SRTP protège **la voix** (le flux RTP)

**Configuration sécurisée complète :**
```
Signalisation : SIP-TLS (port 5061)
Média         : SRTP (RTP chiffré)
```

---

### Question 7 : Ports utilisés par RTP

**Question :** RTP utilise généralement des ports :

**Réponse :**
RTP utilise des ports **dynamiques entre 16384 et 32767** (UDP)

**Explication :**
Contrairement à SIP qui a un port fixe (5060), RTP utilise une plage de ports dynamiques :
- **Cisco** : 16384-32767 (par défaut)
- **Autres constructeurs** : peuvent utiliser 8000-48000

**Pourquoi une plage ?**
Chaque appel utilise 2 flux RTP :
- Un flux A → B
- Un flux B → A

Pour 10 appels simultanés, il faut donc 20 ports RTP différents !

**Important pour les firewalls :**
```
# Il faut ouvrir TOUTE la plage :
permit udp any any range 16384 32767

# Sinon certains appels ne passeront pas !
```

**Piège classique :** N'ouvrir que quelques ports (ex: 16384-20000) → certains appels fonctionnent, d'autres non !

---

### Question 8 : Protocole de supervision de qualité RTP

**Question :** Le protocole qui supervise la qualité des flux RTP est :

**Réponse :**
**RTCP** (RTP Control Protocol)

**Explication :**
RTCP est le "compagnon" de RTP qui surveille la qualité de la communication.

**Ce que fait RTCP :**
- 📊 Compte les paquets envoyés/reçus
- 📊 Mesure les paquets perdus (%)
- 📊 Calcule le jitter (variation de délai)
- 📊 Mesure le RTT (Round Trip Time)

**Fonctionnement :**
- RTCP envoie des rapports toutes les 5 secondes
- Port utilisé : **RTP + 1**
  - Exemple : Si RTP = 16384, alors RTCP = 16385

**⚠️ Attention :** Ne pas confondre avec **QoS** !
- **RTCP** = Protocole qui mesure la qualité
- **QoS** = Mécanisme qui améliore la qualité (priorisation des paquets)

**Schéma :**
```
RTP  (port 16384) → Transporte la voix
RTCP (port 16385) → Surveille la qualité
```

---

### Question 9 : Qu'est-ce qu'un codec en VoIP ?

**Question :** Qu'est-ce qu'un codec en VoIP ?

**Réponse :**
Un codec est un algorithme qui **compresse et décompresse** la voix numérique.

**Définition :**
**Codec** = **CO**deur + **DEC**odeur

**Rôle du codec :**
1. **Encoder** : Compresser la voix pour réduire la bande passante
2. **Décoder** : Décompresser la voix pour la restituer

**Analogie :** Un codec, c'est comme un fichier ZIP :
- **ZIP faible** → Gros fichier, qualité parfaite (G.711)
- **ZIP fort** → Petit fichier, qualité réduite (G.729)

**Les codecs principaux :**

| Codec | Bande passante | Qualité (MOS) | Usage |
|-------|----------------|---------------|-------|
| **G.711** | 64 Kbps | 4.4/5 (excellente) | LAN (réseau local) |
| **G.729** | 8 Kbps | 3.9/5 (bonne) | WAN (liens lents) |
| **G.722** | 64 Kbps | 4.5/5 (HD) | Visioconférence |
| **Opus** | 6-510 Kbps | 4.5/5 (excellente) | WebRTC (Teams, Zoom) |

**Exemple concret :**
```
Voix brute numérisée : 1,4 Mbps
Après codec G.711    : 64 Kbps (divisé par 22)
Après codec G.729    : 8 Kbps (divisé par 175)
```

---

### Question 10 : Qu'est-ce qu'un IPBX ?

**Question :** Qu'est-ce qu'un IPBX ? Citez un exemple.

**Réponse :**
Un **IPBX** (Internet Protocol Private Branch eXchange) est un **serveur de téléphonie qui gère les appels** via le protocole IP.

**Explication simple :**
L'IPBX, c'est le "cerveau" de votre téléphonie :
- Il enregistre les téléphones
- Il route les appels entre utilisateurs
- Il gère la numérotation
- Il fournit les fonctionnalités (transfert, conférence, messagerie vocale, etc.)

**Analogie :**
Un IPBX, c'est comme un central téléphonique d'entreprise, mais qui fonctionne sur le réseau informatique (IP) plutôt que sur le réseau téléphonique classique.

**Exemples d'IPBX :**

**Solutions commerciales :**
- ✅ **Cisco CUCM** (Call Manager) - Leader du marché
- ✅ **Cisco CME** (Call Manager Express) - Version simplifiée
- ✅ **3CX** - IPBX logiciel (Windows/Linux)
- ✅ **Mitel** - Constructeur historique

**Solutions open source :**
- ✅ **Asterisk** - Le plus connu (gratuit)
- ✅ **FreePBX** - Interface web pour Asterisk

**Solutions cloud :**
- ✅ **Microsoft Teams** - Téléphonie intégrée
- ✅ **Cisco Webex Calling**
- ✅ **RingCentral**

**Schéma simplifié :**
```
┌─────────────────┐
│     IPBX        │
│ (Cisco CME)     │  ← Le serveur (cerveau)
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
┌───▼───┐ ┌───▼───┐
│ Tel IP│ │ Tel IP│  ← Les téléphones (clients)
│ 2001  │ │ 2002  │
└───────┘ └───────┘
```

---

### Question 11 : À quoi sert la QoS dans la VoIP/ToIP ?

**Question :** À quoi sert le QoS dans le cadre de la VoIP / ToIP ?

**Réponse :**
**QoS** (Quality of Service) = Mécanisme qui **priorise le trafic vocal** sur le réseau pour garantir une qualité d'appel optimale.

**Explication :**
Sur un réseau, il y a plein de types de trafic :
- 📞 Voix (VoIP)
- 📧 Emails
- 🌐 Navigation web
- 📥 Téléchargements

Sans QoS, tous ces flux sont traités de la même manière. Si quelqu'un télécharge un gros fichier, la voix peut être **hachée, robotique ou coupée**.

**Rôle de la QoS :**
La QoS dit au réseau :
> "La voix, c'est PRIORITAIRE ! Si le réseau est saturé, passe d'abord les paquets de voix, les autres attendront."

**Concrètement, la QoS :**
- ✅ Marque les paquets VoIP (DSCP EF = priorité maximale)
- ✅ Crée des files d'attente prioritaires
- ✅ Réserve de la bande passante pour la voix
- ✅ Limite les autres trafics si besoin

**Exemple sans QoS :**
```
Utilisateur télécharge un fichier de 500 Mo
→ Réseau saturé
→ Paquets VoIP retardés ou perdus
→ Voix hachée, coupures
❌ Qualité catastrophique
```

**Exemple avec QoS :**
```
Utilisateur télécharge un fichier de 500 Mo
→ QoS priorise les paquets VoIP
→ Le téléchargement ralentit un peu
→ La voix passe sans problème
✅ Qualité excellente
```

**Les 3 mécanismes de QoS :**
1. **Classification** : Identifier les paquets VoIP (DSCP, CoS)
2. **Marquage** : Marquer les paquets comme prioritaires
3. **Priorisation** : Traiter en priorité dans les files d'attente

**Configuration typique (switchs/routeurs) :**
```
Classification → DSCP EF (46) pour la voix
               → DSCP AF31 (26) pour la signalisation

Priorisation → File LLQ (Low Latency Queueing)
             → Bande passante réservée : 30%
```

**Sans QoS :**
```
Latence    : Variable (peut dépasser 300 ms)
Jitter     : Élevé (> 50 ms)
Perte      : Importante (> 3%)
Qualité    : ❌ Médiocre à catastrophique
```

**Avec QoS :**
```
Latence    : < 150 ms
Jitter     : < 30 ms
Perte      : < 1%
Qualité    : ✅ Excellente
```

**Conclusion :** La QoS est **OBLIGATOIRE** en VoIP. Sans elle, vous aurez des problèmes de qualité dès que le réseau est chargé.

---

## Conseils pour l'examen

### Stratégies de réponse

**1. Questions sur les protocoles**
- Pensez à la séparation **signalisation** (SIP) vs **média** (RTP)
- SIP = contrôle, RTP = voix

**2. Questions sur les ports**
```
SIP      : 5060 (clair), 5061 (TLS)
RTP      : 16384-32767 (dynamique)
RTCP     : RTP + 1
SCCP     : 2000
```

**3. Questions sur les codecs**
- **LAN** → G.711 (qualité)
- **WAN** → G.729 (économie bande passante)
- **HD** → G.722 (visio, executives)

**4. Questions sur la qualité**
- **RTCP** = mesure la qualité (protocole)
- **QoS** = améliore la qualité (mécanisme)

### Pièges classiques à éviter

❌ **Piège 1 :** Confondre SIP et RTP
- SIP ne transporte PAS la voix, seulement la signalisation

❌ **Piège 2 :** Dire que IPBX = serveur web
- C'est un serveur de téléphonie IP, pas un serveur web

❌ **Piège 3 :** Confondre RTCP et QoS
- RTCP mesure, QoS améliore

❌ **Piège 4 :** Oublier la conversion analogique → numérique
- La voix DOIT être numérisée pour passer sur IP

❌ **Piège 5 :** Penser que SIP utilise TCP
- SIP peut utiliser UDP ou TCP, mais généralement UDP (port 5060)

### Mots-clés à connaître

**Conversion de la voix :**
- Analogique → Numérique
- Échantillonnage, numérisation, compression

**Protocoles :**
- SIP (signalisation)
- RTP (média/voix)
- RTCP (supervision qualité)

**Qualité :**
- Latence (< 150 ms)
- Jitter (< 30 ms)
- Perte de paquets (< 1%)
- QoS (priorisation)

**Équipements :**
- IPBX (serveur téléphonie)
- Téléphone IP (endpoint)
- Codec (compression)

---

## Récapitulatif rapide

| Question | Réponse courte |
|----------|----------------|
| Voix analogique en VoIP ? | Convertie en données numériques |
| Protocole de signalisation ? | SIP (Session Initiation Protocol) |
| SIP transporte quoi ? | La signalisation (contrôle), PAS la voix |
| RTP fonctionne sur ? | UDP (User Datagram Protocol) |
| Port SIP ? | 5060 (UDP/TCP) |
| Port SIP sécurisé ? | 5061 (TLS) |
| Ports RTP ? | 16384-32767 (UDP dynamique) |
| Supervision qualité RTP ? | RTCP (RTP Control Protocol) |
| Codec ? | Algorithme de compression/décompression voix |
| IPBX ? | Serveur de téléphonie IP (ex: Cisco CME, Asterisk) |
| QoS ? | Priorisation du trafic vocal pour garantir la qualité |

---

## Exercice final d'auto-évaluation

Essayez de répondre sans regarder les réponses :

1. Quel protocole transporte la voix ? (Pas la signalisation !)
2. Pourquoi RTP utilise UDP et pas TCP ?
3. Quelle est la différence entre RTCP et QoS ?
4. Donnez 2 exemples d'IPBX.
5. Quel codec utiliser pour un lien WAN lent ?

**Réponses :**
<details>
<summary>Cliquez pour voir</summary>

1. **RTP** (Real-time Transport Protocol)
2. UDP est plus rapide (pas de retransmission), essentiel pour le temps réel
3. RTCP **mesure** la qualité, QoS **améliore** la qualité
4. Cisco CUCM, Asterisk, 3CX, FreePBX (2 au choix)
5. **G.729** (8 Kbps, compression forte)

</details>

---

## Ressources complémentaires

Pour approfondir, consultez :
- [01-fondamentaux-voip.md](01-fondamentaux-voip.md) - Les bases de la VoIP
- [02-protocoles-voip.md](02-protocoles-voip.md) - Détails sur SIP, RTP, SCCP
- [04-qos-vlans-voip.md](04-qos-vlans-voip.md) - Configuration de la QoS

---

<div align="center">

**Bon courage pour vos révisions !** 💪

[⬅️ Retour au sommaire](README.md)

</div>
