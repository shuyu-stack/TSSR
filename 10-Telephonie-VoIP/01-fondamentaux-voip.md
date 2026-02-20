# Fondamentaux de la VoIP - De la théorie au terrain

> 📚 **Module :** Téléphonie VoIP - Fondamentaux
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Intermédiaire
> 👨‍🏫 **Approche :** Architecte réseau → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Qu'est-ce que la VoIP ?](#-quest-ce-que-la-voip-)
- [Histoire et évolution](#-histoire-et-évolution)
- [VoIP vs Téléphonie classique](#-voip-vs-téléphonie-classique)
- [Concepts de base](#-concepts-de-base)
- [Architecture réseau VoIP](#-architecture-réseau-voip)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

J'ai déployé ma **première infrastructure VoIP en 2006** - un projet de 350 téléphones IP Cisco pour remplacer un vieux PBX Alcatel. Ça a été un **cauchemar** pendant 3 mois (qualité vocale médiocre, coupures aléatoires), puis ça a tourné **parfaitement pendant 10 ans**.

**Pourquoi je vous raconte ça ?**

Parce que la VoIP, c'est **simple en théorie** mais **complexe en pratique**. Il faut comprendre :
- Le réseau IP (votre base)
- La voix (temps réel, qualité)
- Les protocoles spécifiques (SIP, RTP)
- La QoS (absolument critique)

Dans ce cours, je ne vais pas vous balancer de la théorie sèche. Je vais vous montrer **comment ça marche vraiment**, avec des exemples de mes 15 ans de terrain.

### 🎯 Ma promesse

À la fin de ces 4 heures, vous saurez :
- ✅ Expliquer la VoIP à votre grand-mère
- ✅ Comprendre **pourquoi** la VoIP a remplacé le RTC
- ✅ Identifier les composants d'une architecture VoIP
- ✅ Calculer la bande passante nécessaire

Allez, on démarre ! 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** ce qu'est la VoIP et son fonctionnement
- ✅ **Comparer** VoIP et téléphonie traditionnelle (avantages/inconvénients)
- ✅ **Identifier** les composants d'une architecture VoIP
- ✅ **Comprendre** les concepts de codec, bande passante, latence, jitter
- ✅ **Calculer** la bande passante nécessaire pour X appels
- ✅ **Justifier** techniquement un projet VoIP à votre direction

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Comprendre le **modèle OSI** (couches 1 à 4 minimum)
- [ ] Maîtriser **l'adressage IP** et le subnetting
- [ ] Connaître les protocoles **TCP** et **UDP**
- [ ] Avoir des notions de **QoS** (souhaitable mais pas obligatoire)

**Matériel nécessaire :**
- 💻 PC avec accès Internet
- 📝 De quoi prendre des notes
- 🎧 Casque audio (pour tester un softphone)

---

## 📞 Qu'est-ce que la VoIP ?

### Définition simple

**VoIP** = **Voice over IP** = Voix sur IP

Au lieu de passer par le réseau téléphonique classique (RTC), on fait transiter la voix sur le **réseau informatique** (IP).

### L'analogie du courrier

Imaginez deux façons d'envoyer un message :

```
┌─────────────────────────────────────────────────────────────┐
│  TÉLÉPHONIE CLASSIQUE (RTC)                                 │
│  = Courrier avec facteur dédié                              │
├─────────────────────────────────────────────────────────────┤
│  Vous avez UN facteur personnel qui fait UNIQUEMENT         │
│  vos courses. Rapide, fiable, mais CHER.                    │
│                                                             │
│  • Circuit dédié entre vous et votre correspondant          │
│  • Excellente qualité                                       │
│  • Mais coût élevé (une ligne = une paire de cuivres)       │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  VOIP (Voice over IP)                                       │
│  = Courrier avec La Poste                                   │
├─────────────────────────────────────────────────────────────┤
│  Votre lettre voyage avec plein d'autres dans un camion     │
│  partagé. Moins cher, mais peut arriver en retard si         │
│  bouchon.                                                    │
│                                                             │
│  • Votre voix = découpée en paquets IP                      │
│  • Paquets = partagent le réseau avec les données           │
│  • Qualité = dépend du réseau (d'où l'importance de QoS)    │
└─────────────────────────────────────────────────────────────┘
```

### Le processus de conversion

Quand vous parlez dans un téléphone IP, voici ce qui se passe :

```
1. CAPTATION
   Votre voix → Microphone du téléphone
   (Onde sonore analogique)

2. NUMÉRISATION
   Onde sonore → Échantillonnage → Valeurs numériques
   (Conversion analogique → numérique)

3. COMPRESSION
   Données brutes → Codec (G.711, G.729) → Données compressées
   (Réduction de 50% à 90% de la taille)

4. ENCAPSULATION
   Données compressées → Paquets RTP → Paquets UDP → Paquets IP
   (Ajout des en-têtes réseau)

5. TRANSMISSION
   Paquets IP → Réseau Ethernet → Internet/LAN → Destination
   (Transport sur l'infrastructure IP)

6. RÉCEPTION
   Le processus inverse chez votre correspondant
```

### Schéma complet

```
 Personne A                                           Personne B
     │                                                      │
     │ Voix                                           Voix  │
     ▼                                                      ▼
┌─────────┐                                          ┌─────────┐
│Téléphone│                                          │Téléphone│
│   IP    │                                          │   IP    │
│  (Cisco │                                          │ (Cisco  │
│  7841)  │                                          │  8841)  │
└────┬────┘                                          └────┬────┘
     │                                                    │
     │ Paquets RTP (voix numérisée)                      │
     │                                                    │
     ▼                                                    ▼
┌─────────────────────────────────────────────────────────────┐
│                     RÉSEAU IP                               │
│  • Switchs (avec PoE, QoS)                                  │
│  • Routeurs (avec QoS)                                      │
│  • Call Manager (gestion des appels)                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 📜 Histoire et évolution

### Mon vécu personnel

Voici l'évolution de la téléphonie que j'ai vécue :

#### 1995-2005 : L'ère des PBX propriétaires

**Ce que j'ai connu :**
- PBX Alcatel, Nortel, Ericsson
- Téléphones analogiques avec câbles RJ11
- Un technicien télécom = un métier à part (pas nous, les informaticiens !)

**Exemple concret (2003)** :
```
Entreprise : 200 employés
PBX : Alcatel OmniPCX 4400
Coût : 120 000€ + 15 000€/an de maintenance
Déménagement d'un service (20 postes) : 8 000€ (recâblage)

Problème : Le service informatique ne pouvait RIEN faire.
Tout passait par le prestataire télécom.
```

#### 2006-2010 : Les débuts de la VoIP

**Mon premier projet VoIP (2006)** :

```
Migration PBX Alcatel → Cisco CallManager 4.0
Entreprise : 350 utilisateurs
Budget : 180 000€

❌ PROBLÈMES RENCONTRÉS :
• Qualité vocale médiocre (pas de QoS)
• Switchs sans PoE → injecteurs partout
• Incompatibilité avec certains fax
• Formation utilisateurs difficile

✅ MAIS APRÈS 3 MOIS :
• Tout fonctionnait parfaitement
• Économie : 25 000€/an sur la maintenance
• Mobilité : softphones pour commerciaux
• Intégration CRM (révolutionnaire à l'époque !)
```

**La leçon :** La VoIP, c'est génial... quand c'est bien fait.

#### 2011-2020 : La maturité

**Généralisations :**
- ✅ Switchs avec PoE en standard
- ✅ QoS automatique (LLDP-MED)
- ✅ Interfaces d'administration web (fini le CLI pur)
- ✅ Softphones performants (Jabber, Teams)

**Projet marquant (2015)** :
```
Groupe industriel : 1200 utilisateurs, 12 sites
Migration complète PBX → Cisco UC

Résultat :
• ROI en 18 mois
• Satisfaction utilisateurs : 8.9/10
• 0 plainte depuis 3 ans
• Visioconférence intégrée
```

#### 2021-Aujourd'hui : Le tout-IP obligatoire

**Contexte :** Les opérateurs **coupent le RTC** (réseau cuivre historique).

**Conséquence :** La VoIP n'est **plus un choix**, c'est une **obligation**.

**Solutions actuelles :**
- Cloud (Teams, Webex, 8x8)
- On-premise (Cisco CUCM, Asterisk, 3CX)
- Hybride

---

## ⚖️ VoIP vs Téléphonie classique

### Comparaison technique

| Critère | RTC/PBX classique | VoIP/ToIP |
|---------|-------------------|-----------|
| **Infrastructure** | Câblage cuivre dédié (RJ11) | Réseau IP (RJ45 Ethernet) |
| **Qualité** | Excellente (circuit dédié) | Variable (dépend du réseau) |
| **Coût initial** | Élevé (PBX propriétaire) | Modéré (utilise l'infrastructure existante) |
| **Maintenance** | Coûteuse (prestataire spécialisé) | Réduite (équipes IT internes) |
| **Évolutivité** | Limitée (cartes physiques) | Quasi illimitée (licences logicielles) |
| **Mobilité** | Inexistante (poste fixe) | Totale (softphones, mobile) |
| **Intégration SI** | Impossible | Complète (CRM, annuaire, etc.) |
| **Bande passante** | 64 Kbps fixe (G.711) | 8 à 90 Kbps (codec variable) |

### Retour d'expérience : Migration réelle

**Contexte (2014)** : PME 120 utilisateurs, PBX Alcatel de 1998

```
┌─────────────────────────────────────────────────────────────┐
│  AVANT (PBX Alcatel)                                        │
├─────────────────────────────────────────────────────────────┤
│  • Maintenance : 12 000€/an                                 │
│  • Ajout d'un utilisateur : 2 jours + 300€                  │
│  • Téléphones : propriétaires Alcatel uniquement            │
│  • Fonctionnalités : basiques (transfert, conférence 3)     │
│  • Mobilité : aucune                                        │
│  • Reporting : inexistant                                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  APRÈS (Cisco CME)                                          │
├─────────────────────────────────────────────────────────────┤
│  • Maintenance : 3 000€/an (on gère en interne)             │
│  • Ajout d'un utilisateur : 10 minutes, 0€                  │
│  • Téléphones : Cisco + softphones PC/mobile                │
│  • Fonctionnalités : conférence 8, messagerie vocale, CTI   │
│  • Mobilité : commerciaux joignables partout                │
│  • Reporting : stats détaillées (durée, coûts, etc.)        │
│                                                             │
│  ROI : 14 mois                                              │
└─────────────────────────────────────────────────────────────┘
```

### Quand rester en téléphonie classique ?

**Cas rares où je déconseille la VoIP :**

1. **Réseau informatique défaillant**
   - Switchs obsolètes (pas de QoS)
   - Bande passante insuffisante
   - Pas de budget pour upgrade réseau

2. **Entreprises très spécifiques**
   - Centrales d'appels avec matériel propriétaire
   - Systèmes d'urgence (hôpitaux) avec contraintes légales
   - Sites isolés sans connexion Internet fiable

3. **Transition difficile**
   - Résistance au changement forte
   - Aucune compétence IT interne
   - Aucun budget formation

**Mais dans 95% des cas : La VoIP est le bon choix.**

---

## 🧮 Concepts de base

### 1. Le Codec (Codeur/Décodeur)

**Définition :** Algorithme qui compresse/décompresse la voix.

**Les codecs courants :**

| Codec | Bande passante | Qualité (MOS) | Usage recommandé |
|-------|----------------|---------------|------------------|
| **G.711** | 87 Kbps | 4.4 / 5 | LAN (excellente qualité) |
| **G.729** | 31 Kbps | 3.9 / 5 | WAN (liens faibles) |
| **G.722** | 87 Kbps | 4.5 / 5 | HD Voice (visio) |
| **Opus** | 24-90 Kbps | 4.5 / 5 | WebRTC (Teams, Webex) |

**MOS** = Mean Opinion Score (note de qualité vocale de 1 à 5)

**Mon conseil :**
```
• LAN (réseau local) : G.711 (qualité maximale)
• WAN (sites distants) : G.729 (économie de bande passante)
• Visioconférence : G.722 (HD)
```

### 2. La Bande passante

**Formule simplifiée :**

```
Bande passante = Codec + En-têtes IP/UDP/RTP

Exemple G.711 :
  Codec : 64 Kbps
  + En-têtes : 23 Kbps (Ethernet, IP, UDP, RTP)
  ───────────────────
  = 87 Kbps par appel

Exemple G.729 :
  Codec : 8 Kbps
  + En-têtes : 23 Kbps
  ───────────────────
  = 31 Kbps par appel
```

**Calcul pour un site :**

```
Site distant : 30 utilisateurs
Taux d'occupation : 20% (6 appels simultanés max)
Codec : G.729

Bande passante nécessaire :
6 appels × 31 Kbps = 186 Kbps
+ 20% de marge = 223 Kbps

Lien recommandé : 512 Kbps minimum
```

### 3. La Latence (délai)

**Définition :** Temps pour qu'un paquet aille de A vers B.

**Valeurs acceptables :**

```
┌────────────────────────────────────────┐
│  < 150 ms   : Excellent               │
│  150-300 ms : Acceptable               │
│  > 300 ms   : Inacceptable (écho)      │
└────────────────────────────────────────┘
```

**Sources de latence :**
- Temps de propagation (distance physique)
- Temps de traitement (codec)
- Temps de mise en file d'attente (congestion)

**Mon expérience :**
Un client avait un lien satellite (700 ms de latence). **Impossible** de faire de la VoIP de qualité.

### 4. La Gigue (Jitter)

**Définition :** Variation de la latence entre les paquets.

**Exemple :**
```
Paquet 1 arrive en 50 ms
Paquet 2 arrive en 80 ms  ← +30 ms de jitter
Paquet 3 arrive en 55 ms  ← -25 ms de jitter
```

**Valeurs acceptables :**

```
┌────────────────────────────────────────┐
│  < 30 ms    : Excellent                │
│  30-50 ms   : Acceptable               │
│  > 50 ms    : Voix hachée, robotique   │
└────────────────────────────────────────┘
```

**Solution :** Buffer de gigue (tampon qui réordonne les paquets)

### 5. La Perte de paquets

**Définition :** Pourcentage de paquets RTP perdus en route.

**Valeurs acceptables :**

```
┌────────────────────────────────────────┐
│  < 1%       : Imperceptible            │
│  1-3%       : Perceptible mais OK      │
│  > 3%       : Dégradation importante   │
└────────────────────────────────────────┘
```

**Causes principales :**
- Congestion réseau (pas de QoS)
- Câbles défectueux
- Switchs surchargés

---

## 🏗️ Architecture réseau VoIP

### Les 3 composants essentiels

```
┌─────────────────────────────────────────────────────────────┐
│  1. CALL MANAGER / IP-PBX (Le cerveau)                      │
├─────────────────────────────────────────────────────────────┤
│  Rôle : Gestion des appels, numérotation, fonctionnalités  │
│                                                             │
│  Solutions :                                                │
│    • Cisco CUCM (Unified Communications Manager)           │
│    • Cisco CME (Call Manager Express) ← Nos TPs            │
│    • Asterisk / FreePBX (open source)                      │
│    • 3CX (Windows/Linux)                                    │
│    • Cloud (Teams, Webex, RingCentral)                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  2. TÉLÉPHONES IP / ENDPOINTS (Les terminaux)               │
├─────────────────────────────────────────────────────────────┤
│  Types :                                                    │
│    • Téléphones IP physiques (Cisco 7841, 8841)            │
│    • Softphones (Jabber, Teams, Zoiper)                    │
│    • Passerelles analogiques (ATA) pour fax                │
│    • Applications mobiles                                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  3. INFRASTRUCTURE RÉSEAU (Le transport)                    │
├─────────────────────────────────────────────────────────────┤
│  Exigences :                                                │
│    • Switchs avec PoE (802.3af/at)                         │
│    • QoS (Quality of Service) OBLIGATOIRE                  │
│    • VLANs séparés (voix / données)                        │
│    • Bande passante suffisante                             │
└─────────────────────────────────────────────────────────────┘
```

### Schéma d'architecture type PME

```
                      INTERNET
                         │
                ┌────────▼────────┐
                │   Firewall      │
                │   (avec ACL)    │
                └────────┬────────┘
                         │
                ┌────────▼────────┐
                │  Switch Core    │
                │  (L3, QoS)      │
                └───┬─────────┬───┘
                    │         │
         ┌──────────┘         └──────────┐
         │                               │
  ┌──────▼──────┐                 ┌──────▼──────┐
  │   Serveur   │                 │   Switch    │
  │  VoIP/CME   │                 │   Accès     │
  │192.168.10.1 │                 │ (PoE, QoS)  │
  └─────────────┘                 └──┬────────┬─┘
                                     │        │
                              ┌──────┘        └──────┐
                              │                      │
                         ┌────▼────┐            ┌────▼────┐
                         │Téléphone│            │Téléphone│
                         │IP 7841  │            │IP 8841  │
                         │PoE/VLAN │            │PoE/VLAN │
                         └─────────┘            └─────────┘
```

### Flux d'un appel VoIP

```
1. ENREGISTREMENT (au démarrage)
   ────────────────────────────────
   Téléphone → DHCP → Reçoit IP + Option 150 (TFTP)
   Téléphone → TFTP → Télécharge config (SEPxxxx.cnf.xml)
   Téléphone → Call Manager → S'enregistre (SCCP ou SIP)

2. APPEL SORTANT (Alice appelle Bob)
   ────────────────────────────────
   Alice décroche → Compose 2002
   Téléphone Alice → Call Manager : "INVITE 2002"
   Call Manager → Téléphone Bob : "INVITE from Alice"
   Téléphone Bob sonne → Bob décroche
   Call Manager : "OK, vous pouvez parler"

3. COMMUNICATION (flux RTP direct)
   ────────────────────────────────
   Téléphone Alice ←──── RTP ────→ Téléphone Bob
   (Les paquets voix passent DIRECTEMENT entre les 2 phones)
   (Le Call Manager ne touche PAS au flux vocal !)

4. FIN D'APPEL
   ────────────────────────────────
   Alice raccroche
   Téléphone Alice → Call Manager : "BYE"
   Call Manager → Téléphone Bob : "BYE"
   Fin de la communication
```

**Point clé :** Le Call Manager gère la **signalisation** (qui appelle qui), mais le flux **vocal** (RTP) passe **directement** entre les téléphones.

---

## 🎯 Exercices pratiques

### Exercice 1 : Calcul de bande passante

**Objectif :** Dimensionner un lien WAN pour un site distant.

**Énoncé :**
Vous devez relier un site distant de 40 utilisateurs. Le taux d'occupation téléphonique est estimé à 20%.

**Questions :**

1. Combien d'appels simultanés maximum ?
2. Quelle bande passante en codec G.711 ?
3. Quelle bande passante en codec G.729 ?
4. Quel lien WAN recommandez-vous avec 30% de marge ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```
1. Appels simultanés :
   40 utilisateurs × 20% = 8 appels max

2. Bande passante G.711 :
   8 appels × 87 Kbps = 696 Kbps

3. Bande passante G.729 :
   8 appels × 31 Kbps = 248 Kbps

4. Lien WAN recommandé :
   Avec G.729 + 30% marge :
   248 Kbps × 1.30 = 322 Kbps

   Lien minimal : 512 Kbps (standard opérateur)
   Lien recommandé : 1 Mbps (confort + données)
```

**Justification :** G.729 pour économiser la bande passante WAN (coût), qualité suffisante (MOS 3.9).

</details>

---

### Exercice 2 : Diagnostic de problème

**Objectif :** Identifier la cause d'un problème de qualité vocale.

**Scénario :**
Un utilisateur se plaint de "voix robotique" lors des appels vers le site distant.

**Symptômes :**
- Appels en interne (même site) : ✅ OK
- Appels vers site distant : ❌ Voix hachée
- Internet fonctionne normalement

**Question :** Quelle est la cause probable ?

**Choix :**
- A) Problème de codec
- B) Problème de latence
- C) Problème de jitter ou perte de paquets
- D) Problème de bande passante

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Réponse : C) Problème de jitter ou perte de paquets**

**Explication :**

"Voix robotique" = symptôme typique de **perte de paquets** ou **jitter excessif**.

**Pourquoi pas les autres ?**

- A) Codec : Impacterait tous les appels, pas seulement WAN
- B) Latence : Causerait de l'écho ou des blancs, pas de voix robotique
- D) Bande passante : Empêcherait l'établissement de l'appel ou causerait des coupures

**Diagnostic à faire :**

```bash
# 1. Vérifier la QoS sur le lien WAN
Router# show policy-map interface <interface-WAN>

# 2. Vérifier les pertes de paquets
Router# show interface <interface> | include drops

# 3. Faire un test ping avec variation
$ ping -c 100 <IP-site-distant>

# 4. Analyser le RTP avec Wireshark
Telephony → RTP → Stream Analysis
→ Vérifier Packet Loss et Jitter
```

**Solution probable :** Activer/corriger la QoS sur le lien WAN.

</details>

---

## 📚 Ressources

### Documentation officielle
- [RFC 3261 - SIP](https://www.rfc-editor.org/rfc/rfc3261)
- [Cisco VoIP Technologies](https://www.cisco.com/c/en/us/tech/unified-communications/index.html)
- [G.711 Codec Specification](https://www.itu.int/rec/T-REC-G.711)

### Calculateurs en ligne
- [VoIP Bandwidth Calculator](http://www.erlang.com/calculator/lipb/)
- [Codec Comparison Tool](https://www.voip-info.org/codecs/)

### Vidéos recommandées
- [How VoIP Works (Computerphile)](https://www.youtube.com/watch?v=3QhU9jd03a0)
- [SIP Protocol Explained](https://www.youtube.com/results?search_query=sip+protocol+explained)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Mes questions :**
-
-
-

**Points à approfondir :**
-
-

---

## ✅ Checklist de révision

Avant de passer au cours suivant, assurez-vous de maîtriser :

- [ ] Je sais expliquer ce qu'est la VoIP
- [ ] Je connais les différences entre VoIP et téléphonie classique
- [ ] Je sais calculer la bande passante nécessaire
- [ ] Je connais les codecs principaux (G.711, G.729)
- [ ] Je comprends les concepts de latence, jitter, perte de paquets
- [ ] Je sais identifier les 3 composants d'une architecture VoIP
- [ ] Je peux justifier un projet VoIP à un client

---

<div align="center">

**Cours suivant :** [02-protocoles-voip.md](02-protocoles-voip.md)

[⬅️ Retour au sommaire](README.md)

</div>
