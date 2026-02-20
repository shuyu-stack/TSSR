# Protocoles VoIP - Le langage de la téléphonie IP

> 📚 **Module :** Téléphonie VoIP - Protocoles
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 4 heures
> 🎯 **Niveau :** Intermédiaire/Avancé
> 👨‍🏫 **Approche :** Architecte réseau → TSSR

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [SIP - Session Initiation Protocol](#-sip---session-initiation-protocol)
- [RTP/RTCP - Transport de la voix](#-rtprtcp---transport-de-la-voix)
- [SCCP - Skinny Client Control Protocol](#-sccp---skinny-client-control-protocol)
- [H.323 - L'ancien standard](#-h323---lancien-standard)
- [Les Codecs audio](#-les-codecs-audio)
- [Comparaisons et choix](#-comparaisons-et-choix)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

En 2008, j'ai passé **3 jours** à debugger un problème de VoIP. Les téléphones sonnaient, mais **aucun son** ne passait. Frustration totale.

**Le problème ?** Un firewall bloquait les ports RTP (le flux vocal). SIP (la signalisation) passait, mais pas RTP (la voix).

**La leçon ?** En VoIP, il y a **deux protocoles distincts** :
- **Signalisation** (SIP/SCCP) : "Qui appelle qui ?"
- **Média** (RTP) : "La voix elle-même"

Si vous ne comprenez pas cette séparation, vous galèrerez sur **tous** vos projets VoIP.

### 🎯 Ma promesse

À la fin de ces 4 heures, vous saurez :
- ✅ Expliquer la différence entre SIP et RTP
- ✅ Lire une trace Wireshark d'un appel SIP
- ✅ Comprendre pourquoi Cisco utilise SCCP
- ✅ Choisir le bon codec pour chaque situation
- ✅ Ouvrir les bons ports sur un firewall

**Accrochez-vous, c'est dense mais passionnant !** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** le rôle de chaque protocole (SIP, RTP, SCCP, H.323)
- ✅ **Analyser** un échange SIP avec Wireshark
- ✅ **Différencier** signalisation et transport média
- ✅ **Comparer** SIP vs SCCP (avantages/inconvénients)
- ✅ **Choisir** le bon codec selon la situation
- ✅ **Configurer** les ports firewall pour VoIP
- ✅ **Diagnostiquer** un problème de protocole

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir suivi le cours **01-fondamentaux-voip.md**
- [ ] Comprendre les protocoles **TCP** et **UDP**
- [ ] Connaître les **ports réseau** (1-65535)
- [ ] Savoir utiliser **Wireshark** (bases)

**Matériel nécessaire :**
- 💻 PC avec Wireshark installé
- 🔬 Packet Tracer (pour tests)
- 📝 De quoi prendre des notes

---

## 📞 SIP - Session Initiation Protocol

### Définition

**SIP** (RFC 3261) est le protocole **standard** pour établir, modifier et terminer des sessions multimédia (voix, vidéo).

**Analogie :** SIP, c'est comme le **serveur dans un restaurant**.
- Il prend votre commande
- Il transmet à la cuisine
- Il vous apporte le plat
- **Mais il ne cuisine pas !** (ça, c'est RTP)

### Caractéristiques techniques

```
┌─────────────────────────────────────────────────────────────┐
│  SIP - Session Initiation Protocol                          │
├─────────────────────────────────────────────────────────────┤
│  • Transport : UDP port 5060 (ou TCP/TLS 5061)              │
│  • Format : Texte (lisible humainement)                     │
│  • Inspiré de HTTP (méthodes, codes réponse)                │
│  • Standard IETF (ouvert, multi-constructeur)               │
│  • Utilisation : 90% des systèmes VoIP modernes             │
└─────────────────────────────────────────────────────────────┘
```

### Les méthodes SIP principales

| Méthode | Rôle | Exemple |
|---------|------|---------|
| **INVITE** | Initier un appel | "Je veux appeler 2002" |
| **ACK** | Confirmer la réception | "OK, j'ai reçu ta réponse" |
| **BYE** | Terminer un appel | "Je raccroche" |
| **CANCEL** | Annuler un appel | "Laisse tomber" |
| **REGISTER** | S'enregistrer | "Je suis le téléphone 2001" |
| **OPTIONS** | Vérifier disponibilité | "Tu es là ?" |

### Les codes de réponse SIP

Comme HTTP, SIP utilise des codes numériques :

```
┌──────────────────────────────────────────────────────────┐
│  1xx - Information                                       │
│    100 Trying       : En cours de traitement            │
│    180 Ringing      : Ça sonne chez le destinataire     │
│    183 Progress     : Appel en progression              │
├──────────────────────────────────────────────────────────┤
│  2xx - Succès                                            │
│    200 OK           : Tout est bon                       │
├──────────────────────────────────────────────────────────┤
│  3xx - Redirection                                       │
│    302 Moved        : Redirigé vers autre numéro        │
├──────────────────────────────────────────────────────────┤
│  4xx - Erreur client                                     │
│    404 Not Found    : Numéro inconnu                     │
│    407 Auth Required: Authentification nécessaire        │
│    486 Busy         : Occupé                             │
├──────────────────────────────────────────────────────────┤
│  5xx - Erreur serveur                                    │
│    500 Server Error : Problème sur le serveur           │
│    503 Unavailable  : Service indisponible              │
└──────────────────────────────────────────────────────────┘
```

### Exemple d'échange SIP complet

Voici un appel d'Alice (2001) vers Bob (2002) :

```
Alice (2001)          Serveur SIP           Bob (2002)
     │                     │                     │
     │ 1) INVITE 2002      │                     │
     ├────────────────────>│                     │
     │                     │ 2) INVITE           │
     │                     ├────────────────────>│
     │                     │                     │
     │ 3) 100 Trying       │                     │
     │<────────────────────┤                     │
     │                     │ 4) 180 Ringing      │
     │ 5) 180 Ringing      │<────────────────────┤
     │<────────────────────┤                     │
     │                     │                     │
     │                     │ Bob décroche        │
     │                     │                     │
     │                     │ 6) 200 OK           │
     │ 7) 200 OK           │<────────────────────┤
     │<────────────────────┤                     │
     │                     │                     │
     │ 8) ACK              │                     │
     ├────────────────────>│ 9) ACK              │
     │                     ├────────────────────>│
     │                     │                     │
     │ ╔═══════════════════════════════════════╗ │
     │ ║   FLUX RTP (voix directe Alice-Bob)   ║ │
     │ ║   Le serveur SIP n'est PLUS impliqué  ║ │
     │ ╚═══════════════════════════════════════╝ │
     │                     │                     │
     │                     │                     │
     │                     │ Alice raccroche     │
     │ 10) BYE             │                     │
     ├────────────────────>│ 11) BYE             │
     │                     ├────────────────────>│
     │                     │ 12) 200 OK          │
     │ 13) 200 OK          │<────────────────────┤
     │<────────────────────┤                     │
     │                     │                     │
```

**Point clé :** Après l'ACK (étape 9), le serveur SIP **ne touche plus au flux vocal**. C'est RTP qui prend le relais directement entre Alice et Bob.

### Exemple de message SIP réel

Voici un INVITE SIP capturé dans Wireshark :

```
INVITE sip:2002@192.168.10.1 SIP/2.0
Via: SIP/2.0/UDP 192.168.10.101:5060;branch=z9hG4bK776asdhds
From: "Alice" <sip:2001@192.168.10.1>;tag=1928301774
To: <sip:2002@192.168.10.1>
Call-ID: a84b4c76e66710@192.168.10.101
CSeq: 314159 INVITE
Contact: <sip:2001@192.168.10.101>
Max-Forwards: 70
Content-Type: application/sdp
Content-Length: 142

v=0
o=alice 2890844526 2890844526 IN IP4 192.168.10.101
s=Session SDP
c=IN IP4 192.168.10.101
t=0 0
m=audio 49170 RTP/AVP 0
a=rtpmap:0 PCMU/8000
```

**Décryptage :**
- `INVITE sip:2002@192.168.10.1` : J'appelle 2002
- `From: "Alice" <sip:2001@192.168.10.1>` : Je suis Alice (2001)
- `Content-Type: application/sdp` : Voici mes capacités média
- `m=audio 49170 RTP/AVP 0` : J'écoute le RTP sur port 49170
- `a=rtpmap:0 PCMU/8000` : J'utilise le codec G.711 µ-law

### Mon retour d'expérience SIP

**Projet 2016 - Migration vers SIP :**

```
CONTEXTE :
Entreprise : 800 utilisateurs
Ancien système : Cisco SCCP (propriétaire)
Nouveau : SIP trunks + téléphones SIP

AVANTAGES CONSTATÉS :
✅ Interopérabilité : mix téléphones (Cisco + Yealink + Poly)
✅ Coût : -40% sur les téléphones (concurrence)
✅ Mobilité : softphones mobiles natifs
✅ Intégration : API SIP pour CRM

PROBLÈMES RENCONTRÉS :
❌ NAT traversal : galère avec les sites distants
❌ Sécurité : tentatives de hack SIP (scanner Internet)
❌ Qualité : sans QoS, qualité médiocre
❌ Compatibilité : certains fax ne fonctionnaient pas

SOLUTIONS APPLIQUÉES :
→ SBC (Session Border Controller) pour NAT et sécurité
→ QoS stricte sur tous les équipements
→ Passerelles analogiques (ATA) pour fax critiques
→ Authentification SIP obligatoire
```

---

## 🎵 RTP/RTCP - Transport de la voix

### RTP - Real-time Transport Protocol

**Rôle :** Transporter les flux média (voix, vidéo) en temps réel.

```
┌─────────────────────────────────────────────────────────────┐
│  RTP - Real-time Transport Protocol                         │
├─────────────────────────────────────────────────────────────┤
│  • Transport : UDP (ports 16384-32767)                      │
│  • Contenu : Voix numérisée et compressée                   │
│  • Unidirectionnel : 2 flux par appel (A→B et B→A)          │
│  • Timestamp : pour synchronisation                         │
│  • Sequence number : pour réordonnancement                  │
└─────────────────────────────────────────────────────────────┘
```

### Structure d'un paquet RTP

```
┌────────────────────────────────────────────────────────────┐
│  En-tête RTP (12 octets minimum)                           │
├────────────────────────────────────────────────────────────┤
│  V   P X  CC   M   PT      Sequence Number                 │
│  2b  1b 1b 4b   1b  7b      16 bits                        │
├────────────────────────────────────────────────────────────┤
│  Timestamp (32 bits)                                        │
│  → Horodatage pour synchronisation                         │
├────────────────────────────────────────────────────────────┤
│  SSRC (32 bits)                                             │
│  → Identifiant de la source                                │
├────────────────────────────────────────────────────────────┤
│  PAYLOAD (Voix compressée)                                  │
│  → Les données vocales (codec G.711, G.729, etc.)          │
└────────────────────────────────────────────────────────────┘
```

### RTCP - RTP Control Protocol

**Rôle :** Surveiller la qualité du flux RTP.

```
RTCP envoie des statistiques toutes les 5 secondes :
• Paquets envoyés / reçus
• Paquets perdus (%)
• Jitter (variation de délai)
• Round Trip Time (RTT)

Port : RTP + 1 (exemple : RTP 16384 → RTCP 16385)
```

### Exemple concret : Analyse Wireshark

Voici ce que je vois dans Wireshark lors d'un appel :

```
FILTRE : rtp

N°    Temps    Source          Dest            Protocole  Info
──────────────────────────────────────────────────────────────
1     0.000    192.168.10.101  192.168.10.102  RTP        PT=PCMU, SSRC=0x12345678, Seq=1
2     0.020    192.168.10.101  192.168.10.102  RTP        PT=PCMU, SSRC=0x12345678, Seq=2
3     0.040    192.168.10.101  192.168.10.102  RTP        PT=PCMU, SSRC=0x12345678, Seq=3
...
100   2.000    192.168.10.101  192.168.10.102  RTP        PT=PCMU, SSRC=0x12345678, Seq=100

ANALYSE : Telephony → RTP → Stream Analysis
────────────────────────────────────────────
Max Delta   : 24 ms    ✅ OK (< 30 ms)
Max Jitter  : 12 ms    ✅ OK (< 30 ms)
Lost packets: 0 (0%)   ✅ OK (< 1%)
→ Qualité vocale : EXCELLENTE
```

### Mon anecdote RTP

**2012 - Le mystère du port RTP :**

Un client avait des appels qui **fonctionnaient 1 fois sur 2**. Totalement aléatoire.

**Diagnostic :**
```
Appels OK : RTP passe
Appels KO : Pas de RTP

J'ai capturé avec Wireshark :
→ Appels OK : RTP sur ports 16384-20000
→ Appels KO : RTP sur ports 20001-32767

CAUSE :
Firewall configuré avec "permit udp any any range 16384 20000"
→ Seulement 3616 ports ouverts sur 16383 possibles !

SOLUTION :
Permit udp any any range 16384 32767
```

**Leçon :** Les ports RTP sont **dynamiques**. Il faut ouvrir **toute la plage**.

---

## 📱 SCCP - Skinny Client Control Protocol

### Définition

**SCCP** (Skinny) est le protocole **propriétaire Cisco** pour contrôler les téléphones IP.

**Pourquoi "Skinny" ?** Parce que le téléphone est "maigre" (peu intelligent), tout le cerveau est dans le Call Manager.

### Caractéristiques

```
┌─────────────────────────────────────────────────────────────┐
│  SCCP - Skinny Client Control Protocol                      │
├─────────────────────────────────────────────────────────────┤
│  • Transport : TCP port 2000                                │
│  • Format : Binaire (non lisible)                           │
│  • Propriétaire : Cisco uniquement                          │
│  • Avantage : Très simple à configurer                      │
│  • Inconvénient : Lock-in constructeur                      │
└─────────────────────────────────────────────────────────────┘
```

### SCCP vs SIP

| Critère | SCCP | SIP |
|---------|------|-----|
| **Standard** | Propriétaire Cisco | Ouvert (IETF) |
| **Configuration** | Très simple | Plus complexe |
| **Téléphones** | Cisco uniquement | Tous constructeurs |
| **Fonctionnalités** | Complètes (Cisco) | Standard (peut varier) |
| **Interopérabilité** | ❌ Faible | ✅ Excellente |
| **Coût téléphones** | $$$ (monopole) | $ (concurrence) |

### Flux SCCP typique

```
Téléphone Cisco          Call Manager
     │                         │
     │ 1) TCP SYN (port 2000)  │
     ├────────────────────────>│
     │ 2) TCP SYN-ACK          │
     │<────────────────────────┤
     │ 3) TCP ACK              │
     ├────────────────────────>│
     │                         │
     │ 4) SCCP Register        │
     ├────────────────────────>│
     │ 5) SCCP RegisterAck     │
     │<────────────────────────┤
     │                         │
     │ 6) SCCP KeepAlive       │
     ├────────────────────────>│ (toutes les 30s)
     │                         │
     │ Utilisateur appelle 2002│
     │                         │
     │ 7) SCCP OffHook         │
     ├────────────────────────>│
     │ 8) SCCP SetLamp (line 1)│
     │<────────────────────────┤
     │ 9) SCCP CallInfo (dial) │
     │<────────────────────────┤
     │                         │
     │ [Utilisateur compose 2002] │
     │                         │
     │ 10) SCCP KeyPad (2,0,0,2)│
     ├────────────────────────>│
     │ 11) SCCP StartTone (ring)│
     │<────────────────────────┤
     │ 12) SCCP OpenReceive    │
     │<────────────────────────┤ (prépare flux RTP)
     │                         │
```

**Point clé :** Le Call Manager **contrôle tout** (affichage écran, LED, tonalités). Le téléphone est un terminal "bête".

### Mon avis personnel

**Quand utiliser SCCP ?**
- ✅ Infrastructure 100% Cisco
- ✅ Besoin de fonctionnalités Cisco avancées (extension mobility, etc.)
- ✅ Équipe IT peu expérimentée (config simple)

**Quand utiliser SIP ?**
- ✅ Mix de constructeurs
- ✅ Interopérabilité avec trunks SIP opérateurs
- ✅ Évolutivité future
- ✅ Coûts maîtrisés

**Mon choix en 2026 : SIP dans 90% des cas.**

---

## 📠 H.323 - L'ancien standard

### Définition

**H.323** est l'**ancien standard** de VoIP (années 1990-2000), aujourd'hui **obsolète**.

### Pourquoi en parler ?

Parce que vous allez le croiser sur des **vieux systèmes** encore en production :
- Anciens PBX Avaya, Nortel
- Visioconférence ancienne génération
- Gateways analogiques legacy

### Caractéristiques H.323

```
┌─────────────────────────────────────────────────────────────┐
│  H.323 - Ancien standard VoIP                               │
├─────────────────────────────────────────────────────────────┤
│  • Standard : ITU-T (télécoms traditionnels)                │
│  • Complexité : Très élevée (7 sous-protocoles !)           │
│  • Transport : TCP + UDP                                    │
│  • Usage : < 5% des systèmes en 2026                        │
│  • Statut : OBSOLÈTE (remplacé par SIP)                     │
└─────────────────────────────────────────────────────────────┘
```

### Les 7 protocoles H.323

```
H.225 (RAS)     : Enregistrement
H.225 (Q.931)   : Signalisation d'appel
H.245           : Négociation de capacités
RTP             : Transport média
RTCP            : Contrôle qualité
T.120           : Partage de données
G.7xx / H.26x   : Codecs audio/vidéo
```

**Pourquoi c'est mort ?** Trop complexe, pas adapté au NAT, propriétaire.

### Mon anecdote H.323

**2010 - Migration H.323 vers SIP :**

```
Client : Banque avec 15 agences
Système : H.323 Avaya vieux de 12 ans

PROBLÈMES :
• Impossible d'ajouter un softphone (pas compatible)
• Chaque modification = intervention prestataire
• Coût maintenance : 30 000€/an
• Aucune évolution possible

MIGRATION VERS SIP :
Budget : 80 000€
ROI : 2.6 ans
Résultat : Système moderne, flexible, maintenable

Ma recommandation : Si vous avez du H.323, MIGREZ.
```

---

## 🎼 Les Codecs audio

### Qu'est-ce qu'un codec ?

**Codec** = **Co**der + **Dec**oder

**Rôle :** Compresser la voix pour réduire la bande passante.

**Analogie :** Un codec, c'est comme un ZIP pour la voix.
- ZIP faible : gros fichier, qualité parfaite
- ZIP fort : petit fichier, qualité dégradée

### Les codecs principaux

#### G.711 - La référence qualité

```
┌─────────────────────────────────────────────────────────────┐
│  G.711 (PCMU/PCMA)                                          │
├─────────────────────────────────────────────────────────────┤
│  Bande passante  : 64 Kbps (87 Kbps avec en-têtes)          │
│  Compression     : AUCUNE (voix brute numérisée)            │
│  Qualité (MOS)   : 4.4 / 5 (excellente)                     │
│  Latence codec   : 0.125 ms (négligeable)                   │
│  Usage           : LAN, qualité maximale                    │
│  Variantes       : µ-law (USA), A-law (Europe)              │
└─────────────────────────────────────────────────────────────┘
```

**Mon avis :** **Toujours utiliser G.711 en LAN** (sauf contrainte spécifique).

#### G.729 - L'économe

```
┌─────────────────────────────────────────────────────────────┐
│  G.729 (G.729a)                                             │
├─────────────────────────────────────────────────────────────┤
│  Bande passante  : 8 Kbps (31 Kbps avec en-têtes)           │
│  Compression     : 87% (très forte)                         │
│  Qualité (MOS)   : 3.9 / 5 (bonne)                          │
│  Latence codec   : 10 ms (perceptible)                      │
│  Usage           : WAN, liens faibles                       │
│  Licence         : Payante (environ 10$/canal)              │
└─────────────────────────────────────────────────────────────┘
```

**Mon avis :** Parfait pour WAN, mais **jamais transcoder 2 fois** (G.711→G.729→G.711 = qualité catastrophique).

#### G.722 - Le HD

```
┌─────────────────────────────────────────────────────────────┐
│  G.722 (HD Voice)                                           │
├─────────────────────────────────────────────────────────────┤
│  Bande passante  : 64 Kbps (87 Kbps avec en-têtes)          │
│  Fréquences      : 50-7000 Hz (vs 300-3400 Hz en G.711)     │
│  Qualité (MOS)   : 4.5 / 5 (excellente HD)                  │
│  Usage           : Visioconférence, direction               │
│  Ressenti        : Voix naturelle, claire                   │
└─────────────────────────────────────────────────────────────┘
```

**Mon avis :** La différence qualitative est **flagrante**. Pour executives et visio.

#### Opus - Le moderne

```
┌─────────────────────────────────────────────────────────────┐
│  Opus (Standard Internet)                                   │
├─────────────────────────────────────────────────────────────┤
│  Bande passante  : 6-510 Kbps (adaptatif !)                 │
│  Qualité (MOS)   : 4.5 / 5 (excellente)                     │
│  Latence codec   : 5-66.5 ms (configurable)                 │
│  Usage           : WebRTC, Teams, Webex, Zoom               │
│  Avantage        : S'adapte à la bande passante disponible  │
│  Licence         : Gratuit, open source                     │
└─────────────────────────────────────────────────────────────┘
```

**Mon avis :** Le **futur** de la VoIP. Utilisé par tous les services cloud.

### Tableau comparatif complet

| Codec | Bande passante | MOS | Latence | Usage | Licence |
|-------|----------------|-----|---------|-------|---------|
| **G.711** | 87 Kbps | 4.4 | 0.125 ms | LAN | Gratuit |
| **G.729** | 31 Kbps | 3.9 | 10 ms | WAN | Payant |
| **G.722** | 87 Kbps | 4.5 | 4 ms | HD/Visio | Gratuit |
| **Opus** | 6-510 Kbps | 4.5 | 5-66 ms | WebRTC | Gratuit |
| **G.726** | 40 Kbps | 3.8 | 1 ms | Obsolète | Gratuit |
| **iLBC** | 15 Kbps | 4.0 | 30 ms | Mobile | Gratuit |

### Calcul bande passante par codec

```
Calcul complet (Ethernet + IP + UDP + RTP + Codec) :

G.711 :
  Ethernet : 18 octets
  IP       : 20 octets
  UDP      : 8 octets
  RTP      : 12 octets
  Payload  : 160 octets (20 ms de voix)
  ─────────────────────
  Total    : 218 octets / 20 ms = 87.2 Kbps

G.729 :
  Ethernet : 18 octets
  IP       : 20 octets
  UDP      : 8 octets
  RTP      : 12 octets
  Payload  : 20 octets (20 ms de voix)
  ─────────────────────
  Total    : 78 octets / 20 ms = 31.2 Kbps
```

**Point clé :** Les en-têtes représentent **26% du débit en G.711** et **74% en G.729** !

---

## ⚖️ Comparaisons et choix

### SIP vs SCCP vs H.323

```
┌────────────────────────────────────────────────────────────┐
│                  COMPARAISON FINALE                        │
├───────────────┬──────────────┬──────────────┬─────────────┤
│   Critère     │     SIP      │     SCCP     │    H.323    │
├───────────────┼──────────────┼──────────────┼─────────────┤
│ Standard      │ Ouvert (IETF)│ Cisco        │ ITU (mort)  │
│ Interop       │ ✅ Excellent │ ❌ Faible    │ ⚠️ Moyen    │
│ Complexité    │ ⚠️ Moyenne   │ ✅ Simple    │ ❌ Élevée   │
│ NAT traversal │ ⚠️ Complexe  │ ✅ OK        │ ❌ Difficile│
│ Fonctions     │ ✅ Complètes │ ✅ Complètes │ ⚠️ Basiques │
│ Coût phones   │ ✅ Bas       │ ❌ Élevé     │ ⚠️ Variable │
│ Évolutivité   │ ✅ Excellente│ ⚠️ Bonne     │ ❌ Limitée  │
│ En 2026       │ ✅ RECOMMANDÉ│ ⚠️ OK Cisco  │ ❌ OBSOLÈTE │
└───────────────┴──────────────┴──────────────┴─────────────┘
```

### Arbre de décision protocole

```
QUEL PROTOCOLE CHOISIR ?
│
├─ Infrastructure 100% Cisco ?
│  ├─ Oui → SCCP (simplicité) ou SIP (évolutivité)
│  └─ Non → SIP (interopérabilité)
│
├─ Mix de constructeurs ?
│  └─ Oui → SIP obligatoire
│
├─ Trunks SIP opérateur ?
│  └─ Oui → SIP obligatoire
│
├─ Ancien système H.323 ?
│  └─ Oui → MIGRER vers SIP rapidement
│
└─ Nouveau projet en 2026 ?
   └─ SIP dans 99% des cas
```

### Arbre de décision codec

```
QUEL CODEC CHOISIR ?
│
├─ Type de lien ?
│  │
│  ├─ LAN (réseau local)
│  │  └─ G.711 (qualité maximale)
│  │
│  ├─ WAN < 512 Kbps
│  │  └─ G.729 (économie bande passante)
│  │
│  └─ WAN > 1 Mbps
│     └─ G.711 (qualité)
│
├─ Usage spécifique ?
│  │
│  ├─ Visioconférence
│  │  └─ G.722 (HD Voice)
│  │
│  ├─ WebRTC (Teams, Webex)
│  │  └─ Opus (standard web)
│  │
│  └─ Direction / C-Level
│     └─ G.722 (qualité HD)
│
└─ Budget licences ?
   │
   ├─ Contraint
   │  └─ G.711 ou Opus (gratuits)
   │
   └─ Pas de contrainte
      └─ G.729 si besoin WAN
```

### Ma recommandation 2026

```
┌─────────────────────────────────────────────────────────────┐
│  CONFIGURATION RECOMMANDÉE POUR NOUVEAU PROJET              │
├─────────────────────────────────────────────────────────────┤
│  Signalisation : SIP                                        │
│  Codec LAN     : G.711 (qualité)                            │
│  Codec WAN     : G.729 (économie) ou Opus (moderne)         │
│  Codec HD      : G.722 (executives, visio)                  │
│  Téléphones    : Mix Cisco + alternatives (Yealink, Poly)   │
│  Sécurité      : TLS + SRTP obligatoires                    │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice 1 : Analyse de trame SIP

**Objectif :** Lire et comprendre un échange SIP.

**Énoncé :**

Voici une capture Wireshark d'un appel qui échoue :

```
1. INVITE sip:2003@192.168.10.1 SIP/2.0
   From: <sip:2001@192.168.10.1>
   To: <sip:2003@192.168.10.1>

2. 100 Trying

3. 404 Not Found
```

**Questions :**

1. Quel est le numéro appelant ?
2. Quel est le numéro appelé ?
3. Pourquoi l'appel échoue ?
4. Quel message SIP manque pour un appel réussi ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```
1. Numéro appelant : 2001 (From)

2. Numéro appelé : 2003 (To)

3. Pourquoi l'appel échoue :
   Code 404 = Not Found
   → Le numéro 2003 n'existe pas dans le système
   → Ou le téléphone 2003 n'est pas enregistré

4. Messages manquants pour appel réussi :
   ✅ 180 Ringing (ça sonne)
   ✅ 200 OK (décroché)
   ✅ ACK (confirmation)
   ✅ BYE (fin d'appel)

DIAGNOSTIC :
→ Vérifier que le téléphone 2003 est enregistré
→ Commande Cisco : show ephone registered
→ Ou vérifier la config du dial-peer
```

</details>

---

### Exercice 2 : Calcul bande passante multi-codecs

**Objectif :** Dimensionner un lien WAN avec plusieurs codecs.

**Énoncé :**

Site distant de 50 utilisateurs avec :
- 80% des appels en G.729 (standard)
- 20% des appels en G.711 (direction)
- Taux d'occupation : 25%

**Questions :**

1. Combien d'appels simultanés maximum ?
2. Répartition G.729 / G.711 ?
3. Bande passante totale nécessaire ?
4. Lien WAN recommandé (avec 30% marge) ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```
1. Appels simultanés :
   50 utilisateurs × 25% = 12.5 → 13 appels max

2. Répartition :
   G.729 : 13 × 80% = 10.4 → 10 appels
   G.711 : 13 × 20% = 2.6  → 3 appels

3. Bande passante totale :
   G.729 : 10 appels × 31 Kbps = 310 Kbps
   G.711 : 3 appels × 87 Kbps = 261 Kbps
   ──────────────────────────────────────
   Total : 571 Kbps

4. Lien WAN avec 30% marge :
   571 Kbps × 1.30 = 742 Kbps

   Lien standard opérateur : 1 Mbps (sécurisé)
   Ou 2 Mbps (confortable avec données)

RECOMMANDATION :
→ Lien 2 Mbps symétrique
→ QoS stricte (priorité voix)
→ Monitoring bande passante
```

</details>

---

### Exercice 3 : Diagnostic firewall

**Objectif :** Identifier un problème de ports bloqués.

**Scénario :**

Après migration VoIP, les utilisateurs signalent :
- ✅ Les téléphones s'enregistrent (LED vertes)
- ✅ Les appels sonnent chez le destinataire
- ❌ Aucun son ne passe (silence total)

**Questions :**

1. Quel protocole fonctionne ?
2. Quel protocole est bloqué ?
3. Quels ports ouvrir sur le firewall ?
4. Commande de diagnostic ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

```
1. Protocole qui fonctionne : SIP (signalisation)
   → Les téléphones s'enregistrent : REGISTER OK
   → Les appels sonnent : INVITE + 180 Ringing OK

2. Protocole bloqué : RTP (média/voix)
   → Pas de son = flux RTP bloqué

3. Ports à ouvrir sur firewall :

   SIP (déjà ouvert visiblement) :
   • UDP/TCP 5060 (SIP non sécurisé)
   • TCP 5061 (SIP TLS)

   RTP (À OUVRIR) :
   • UDP 16384-32767 (flux média)
   OU
   • UDP 8000-48000 (selon config constructeur)

4. Commandes diagnostic :

   # Wireshark :
   Filtre : rtp
   → Si aucun paquet RTP, c'est le firewall

   # Cisco :
   Router# show ip access-lists
   Router# show policy-map interface

   # Test tcpdump/tshark :
   # tcpdump -i eth0 udp portrange 16384-32767
   → Doit voir des paquets RTP pendant l'appel

SOLUTION FIREWALL :
permit udp any any range 16384 32767
(ou restreindre source/dest selon sécurité)
```

</details>

---

## 📚 Ressources

### Documentation officielle

- [RFC 3261 - SIP Protocol](https://www.rfc-editor.org/rfc/rfc3261)
- [RFC 3550 - RTP Protocol](https://www.rfc-editor.org/rfc/rfc3550)
- [ITU-T G.711 Codec](https://www.itu.int/rec/T-REC-G.711)
- [ITU-T G.729 Codec](https://www.itu.int/rec/T-REC-G.729)
- [Cisco SCCP Documentation](https://www.cisco.com/c/en/us/support/unified-communications/unified-communications-manager-callmanager/products-maintenance-guides-list.html)

### Outils pratiques

- **Wireshark** : Analyse de trames SIP/RTP
- **SIPp** : Test de charge SIP
- **Asterisk** : Lab SIP gratuit
- **Cisco Packet Tracer** : Simulation VoIP

### Calculateurs

- [VoIP Bandwidth Calculator](http://www.bandcalc.com/)
- [Codec Comparison Tool](https://www.voip-info.org/codecs/)
- [Erlang Calculator](http://www.erlang.com/calculator/)

### Vidéos recommandées

- [SIP Protocol Deep Dive](https://www.youtube.com/results?search_query=sip+protocol+deep+dive)
- [RTP Analysis with Wireshark](https://www.youtube.com/results?search_query=rtp+analysis+wireshark)

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

**Projets personnels :**
-
-

---

## ✅ Checklist de révision

Avant de passer au cours suivant, assurez-vous de maîtriser :

- [ ] Je sais expliquer la différence entre SIP et RTP
- [ ] Je peux lire un échange SIP dans Wireshark
- [ ] Je connais les codes de réponse SIP principaux (100, 180, 200, 404)
- [ ] Je comprends pourquoi SIP et RTP utilisent des ports différents
- [ ] Je sais comparer SIP et SCCP (avantages/inconvénients)
- [ ] Je connais les 4 codecs principaux (G.711, G.729, G.722, Opus)
- [ ] Je peux calculer la bande passante pour un codec donné
- [ ] Je sais choisir le bon codec selon la situation
- [ ] Je peux diagnostiquer un problème de firewall (ports RTP)
- [ ] Je sais pourquoi H.323 est obsolète

---

<div align="center">

**Cours précédent :** [01-fondamentaux-voip.md](01-fondamentaux-voip.md)

**Cours suivant :** [03-configuration-cme-packet-tracer.md](03-configuration-cme-packet-tracer.md)

[⬅️ Retour au sommaire](README.md)

</div>
