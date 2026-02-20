# Sécurité VoIP - Protéger vos communications

> 📚 **Module :** Téléphonie VoIP - Sécurité
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 2 heures
> 🎯 **Niveau :** Intermédiaire/Avancé
> 👨‍🏫 **Approche :** Terrain + Cas réels

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Les menaces VoIP](#-les-menaces-voip)
- [Authentification SIP](#-authentification-sip)
- [Chiffrement TLS et SRTP](#-chiffrement-tls-et-srtp)
- [Firewall et ACL](#-firewall-et-acl)
- [Bonnes pratiques](#-bonnes-pratiques)
- [Cas pratiques](#-cas-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**2011 - Le réveil brutal.**

Un lundi matin, un client m'appelle, paniqué :
> "On a une facture télécom de **47 000€** pour le mois dernier. Normalement on est à 2 000€. C'est quoi ce bordel ?"

**Diagnostic :** Leur système VoIP s'est fait **hacker**. Les pirates ont utilisé leur infrastructure pour passer **3 200 heures d'appels internationaux** vers des numéros surtaxés.

**Comment ?** SIP non sécurisé, ouvert sur Internet, sans authentification.

**Conséquence :**
- 47 000€ de perte (l'opérateur a refusé le geste commercial)
- 1 semaine de stress et d'investigation
- Réputation du DSI entachée

**Solution (post-incident) :**
- Authentification SIP obligatoire
- Firewall strict (ACL)
- Chiffrement TLS/SRTP
- Monitoring des appels anormaux

**La leçon :** **La sécurité VoIP, ce n'est pas optionnel.**

### 🎯 Ma promesse

À la fin de ces 2 heures, vous saurez :
- ✅ Identifier les menaces VoIP (Toll Fraud, écoute, DoS)
- ✅ Sécuriser SIP avec authentification
- ✅ Chiffrer les communications (TLS/SRTP)
- ✅ Configurer un firewall pour VoIP
- ✅ Appliquer les bonnes pratiques de sécurité

**Ne reproduisez JAMAIS l'erreur de ce client !** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Identifier** les menaces VoIP principales
- ✅ **Configurer** l'authentification SIP
- ✅ **Mettre en place** le chiffrement TLS et SRTP
- ✅ **Sécuriser** un firewall pour VoIP
- ✅ **Appliquer** les bonnes pratiques de sécurité
- ✅ **Détecter** une tentative d'attaque

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir suivi les cours **01 à 04** (VoIP, protocoles, CME, QoS)
- [ ] Connaître les **protocoles SIP et RTP**
- [ ] Maîtriser les **ACL** (Access Control Lists) Cisco
- [ ] Comprendre les bases du **chiffrement** (clés, certificats)

**Matériel nécessaire :**
- 💻 PC avec Packet Tracer
- 📝 De quoi prendre des notes

---

## 🛡️ Les menaces VoIP

### Vue d'ensemble

```
┌─────────────────────────────────────────────────────────────┐
│  TOP 5 DES MENACES VOIP                                     │
├─────────────────────────────────────────────────────────────┤
│  1. TOLL FRAUD (Fraude téléphonique)      → 💰 Financier   │
│  2. ÉCOUTE CLANDESTINE (Eavesdropping)    → 🔒 Confidentialité│
│  3. DÉNI DE SERVICE (DoS)                 → 🚫 Disponibilité│
│  4. VISHING (Phishing vocal)              → 🎭 Social Engineering│
│  5. SPAM VOCAL (SPIT)                     → 📞 Nuisance    │
└─────────────────────────────────────────────────────────────┘
```

### 1. Toll Fraud - Fraude téléphonique

**Définition :** Les pirates utilisent votre infrastructure VoIP pour passer des appels **à vos frais**.

**Scénario typique :**

```
1. Scanner Internet trouve votre serveur SIP ouvert (port 5060)
2. Test d'authentification faible/inexistante
3. Enregistrement d'extensions SIP
4. Passage d'appels vers numéros internationaux surtaxés
5. Facture astronomique pour vous
```

**Exemple réel (2011) :**

```
ENTREPRISE : PME 50 personnes, Cisco CME
ATTAQUE : Week-end du 14 juillet (personne au bureau)

Vendredi 18h00 : Tout va bien
Samedi 02h00 : Début de l'attaque (détectée en analyse post-mortem)
Dimanche 23h59 : Fin de l'attaque

BILAN :
• 3 200 heures d'appels vers numéros surtaxés (Algérie, Tunisie, Somalie)
• 47 000€ de facture
• 12 extensions SIP compromises

CAUSE :
• SIP ouvert sur Internet sans firewall
• Pas d'authentification forte
• Pas de restriction géographique (ACL)
• Pas de monitoring/alerting

TEMPS DE DÉTECTION : 2 jours (lundi matin, appel de l'opérateur)
```

**Comment s'en protéger ?**

```cisco
! 1. Authentification obligatoire
telephony-service
  security authenticate credential

! 2. Restriction d'appels internationaux
voice translation-rule 1
 rule 1 reject /^00/
!
dial-peer voice 100 voip
 translation-profile outgoing block-international

! 3. Monitoring des appels anormaux
! (Alertes si > 10 appels/heure vers international)

! 4. Firewall strict (voir section dédiée)
```

### 2. Écoute clandestine (Eavesdropping)

**Définition :** Interception et écoute des communications VoIP.

**Méthodes d'attaque :**

```
┌─────────────────────────────────────────────────────────────┐
│  MÉTHODE 1 : SNIFFING (CAPTURE RÉSEAU)                     │
├─────────────────────────────────────────────────────────────┤
│  Attaquant sur le même réseau (ex: WiFi public)             │
│  → Capture des paquets RTP avec Wireshark                   │
│  → Reconstitution de la conversation                        │
│                                                             │
│  Outils : Wireshark, UCSniff, Vomit, Cain & Abel           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  MÉTHODE 2 : MAN-IN-THE-MIDDLE (MITM)                      │
├─────────────────────────────────────────────────────────────┤
│  Attaquant se positionne entre les 2 téléphones            │
│  → ARP Poisoning / ARP Spoofing                             │
│  → Interception et relais du trafic                         │
│                                                             │
│  Outils : Ettercap, Bettercap, MITMf                        │
└─────────────────────────────────────────────────────────────┘
```

**Démonstration Wireshark :**

```
# Sans chiffrement (RTP standard)
1. Capturer un appel VoIP avec Wireshark
2. Filtre : rtp
3. Telephony → RTP → Stream Analysis
4. Play Streams
   → On ENTEND la conversation !

# Avec chiffrement (SRTP)
1. Même capture
2. Filtre : rtp
   → Paquets visibles mais CHIFFRÉS
3. Impossible de décoder sans la clé
```

**Mon anecdote :**

```
2014 - Audit sécurité pour un client

Test (avec autorisation) :
• WiFi invité non isolé du réseau entreprise
• J'ouvre Wireshark sur le WiFi invité
• Je capture pendant 30 minutes
• Résultat : 4 conversations VoIP capturées et décodées

Contenu intercepté :
• Négociations commerciales sensibles
• Numéros de téléphone clients
• Informations personnelles

Réaction du client : 😱
→ Mise en place SRTP immédiate
→ Isolation WiFi invité
→ Budget sécurité débloqué
```

**Protection :**

```
✅ Chiffrement SRTP (voir section dédiée)
✅ Isolation réseau (VLANs)
✅ Détection ARP Spoofing (Dynamic ARP Inspection)
✅ Sécurité WiFi (WPA3, isolation clients)
```

### 3. Déni de Service (DoS/DDoS)

**Définition :** Saturer le système VoIP pour le rendre indisponible.

**Types d'attaques DoS VoIP :**

```
┌─────────────────────────────────────────────────────────────┐
│  SIP FLOOD                                                  │
├─────────────────────────────────────────────────────────────┤
│  Envoi massif de requêtes SIP INVITE                        │
│  → Saturation du serveur SIP                                │
│  → Téléphones ne peuvent plus s'enregistrer                │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  RTP FLOOD                                                  │
├─────────────────────────────────────────────────────────────┤
│  Envoi massif de paquets RTP                                │
│  → Saturation bande passante                                │
│  → Qualité vocale dégradée/impossible                       │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  REGISTRATION HIJACKING                                     │
├─────────────────────────────────────────────────────────────┤
│  Usurpation d'identité lors de l'enregistrement SIP        │
│  → L'attaquant prend la place d'un téléphone légitime      │
│  → Interception/détournement des appels                     │
└─────────────────────────────────────────────────────────────┘
```

**Protection :**

```cisco
! Limitation du taux de requêtes SIP
voice class sip-options-keepalive 1
 transport udp
 sip-profiles 100

voice class sip-profiles 100
 request ANY sip-header Max-Forwards modify "70"
 response ANY sip-header Server remove

! ACL anti-flood
ip access-list extended ANTI-FLOOD-SIP
 permit udp any any eq 5060 log
 deny ip any any log
!
interface GigabitEthernet0/0
 ip access-group ANTI-FLOOD-SIP in
 rate-limit input 512000 8000 8000 conform-action transmit exceed-action drop
```

### 4. VISHING (Voice Phishing)

**Définition :** Phishing par téléphone (usurpation d'identité).

**Scénario typique :**

```
1. Attaquant usurpe le Caller-ID (affichage numéro)
   → Affiche "BANQUE DE FRANCE" ou "SERVICE IT"

2. Appel de la victime
   → "Bonjour, nous avons détecté une activité suspecte"

3. Demande d'informations sensibles
   → Mots de passe, codes, numéros de carte

4. Vol d'identité / accès frauduleux
```

**Protection :**

```
✅ Sensibilisation utilisateurs (ne JAMAIS donner de mdp par téléphone)
✅ Vérification Caller-ID (méfiance si incohérent)
✅ Callback (rappeler le numéro officiel)
✅ MFA (Multi-Factor Authentication)
```

### 5. SPAM Vocal (SPIT)

**SPIT** = **SP**am over **I**nternet **T**elephony

**Définition :** Appels automatisés non sollicités (publicité, arnaques).

**Exemple :**

```
"Bonjour, vous avez gagné un iPhone 15..."
"Dernière chance pour réduire vos impôts..."
"Votre CPF expire bientôt..."
```

**Protection :**

```cisco
! Liste noire (blacklist)
voice class black-list 1
 deny 0123456789
 deny 0987654321

! Application
dial-peer voice 200 voip
 voice-class black-list 1
```

---

## 🔐 Authentification SIP

### Principe

Sans authentification, **n'importe qui peut s'enregistrer** sur votre serveur SIP et passer des appels.

**Authentification SIP** = Vérifier l'identité avant d'autoriser l'enregistrement.

### Mécanisme HTTP Digest

SIP utilise l'authentification **HTTP Digest** (comme un site web).

**Flux d'authentification :**

```
Téléphone                      Serveur SIP
     │                              │
     │ 1) REGISTER (sans auth)      │
     ├─────────────────────────────>│
     │                              │
     │ 2) 401 Unauthorized          │
     │    + Challenge (nonce)       │
     │<─────────────────────────────┤
     │                              │
     │ 3) REGISTER + Credentials    │
     │    (username + hash password)│
     ├─────────────────────────────>│
     │                              │
     │ 4) 200 OK                    │
     │<─────────────────────────────┤
     │                              │
```

**Point clé :** Le mot de passe n'est **jamais envoyé en clair**, seulement un **hash**.

### Configuration Cisco CME

#### Activer l'authentification

```cisco
! Mode configuration CME
CME-Router(config)# telephony-service
CME-Router(config-telephony)# security authenticate credential
! Authentification obligatoire pour l'enregistrement
CME-Router(config-telephony)# exit

! Créer des utilisateurs
CME-Router(config)# username alice secret Cisco123!
CME-Router(config)# username bob secret Cisco456!

! Associer les credentials aux ephone-dn
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# number 2001
CME-Router(config-ephone-dn)# name Alice
CME-Router(config-ephone-dn)# credential username alice password Cisco123!
CME-Router(config-ephone-dn)# exit

CME-Router(config)# ephone-dn 2
CME-Router(config-ephone-dn)# number 2002
CME-Router(config-ephone-dn)# name Bob
CME-Router(config-ephone-dn)# credential username bob password Cisco456!
CME-Router(config-ephone-dn)# exit

! Recréer les fichiers de config
CME-Router(config)# telephony-service
CME-Router(config-telephony)# create cnf-files
CME-Router(config-telephony)# exit
```

#### Vérification

```cisco
CME-Router# show telephony-service security

Credential authentication: Enabled   ← OK !
Credential expiry time: 1 hour
...

CME-Router# debug ephone register
! Pendant l'enregistrement, vous verrez :
! "401 Unauthorized sent"
! "REGISTER with credentials received"
! "200 OK sent (authenticated)"
```

### Bonnes pratiques mots de passe

```
✅ Longueur minimale : 12 caractères
✅ Complexité : majuscules, minuscules, chiffres, symboles
✅ Unique par utilisateur
✅ Changement régulier (tous les 90 jours)
✅ Pas de mots de passe par défaut (admin/admin, cisco/cisco)

❌ NE JAMAIS FAIRE :
   • Mot de passe = prénom + 123
   • Même mot de passe pour tous
   • Mot de passe écrit sur un post-it
   • Mot de passe partagé
```

---

## 🔒 Chiffrement TLS et SRTP

### TLS (Transport Layer Security)

**TLS** = Chiffrement de la **signalisation** SIP (qui appelle qui).

**Sans TLS :**
```
SIP en clair → Visible dans Wireshark
INVITE sip:2002@192.168.10.1 SIP/2.0
From: "Alice" <sip:2001@192.168.10.1>
→ Tout le monde peut voir qui appelle qui
```

**Avec TLS (SIP over TLS = SIPS) :**
```
SIP chiffré → Illisible dans Wireshark
0x45a3f2b9c8d7e1f4...
→ Impossible de savoir qui appelle qui
```

**Port :** TLS = TCP **5061** (au lieu de UDP 5060)

### SRTP (Secure RTP)

**SRTP** = Chiffrement du **flux média** (la voix elle-même).

**Sans SRTP (RTP standard) :**
```
RTP en clair → Décodable dans Wireshark
Telephony → RTP → Play Streams
→ On peut écouter la conversation !
```

**Avec SRTP :**
```
RTP chiffré → Impossible à décoder
0x7f9e3c2a1b5d...
→ Impossible d'écouter la conversation
```

### Configuration TLS sur Cisco CME

**Note :** Configuration avancée, pas disponible dans Packet Tracer de base.

```cisco
! Générer un certificat auto-signé (ou importer un vrai certificat)
CME-Router(config)# crypto key generate rsa general-keys modulus 2048
CME-Router(config)# crypto pki trustpoint CME-CA
CME-Router(ca-trustpoint)# enrollment selfsigned
CME-Router(ca-trustpoint)# subject-name CN=CME-Router
CME-Router(ca-trustpoint)# revocation-check none
CME-Router(ca-trustpoint)# exit
CME-Router(config)# crypto pki enroll CME-CA

! Activer TLS pour SIP
CME-Router(config)# sip-ua
CME-Router(config-sip-ua)# transport tcp tls v1.2
CME-Router(config-sip-ua)# exit

! Activer SRTP
CME-Router(config)# telephony-service
CME-Router(config-telephony)# srtp
CME-Router(config-telephony)# exit
```

**Vérification :**

```cisco
CME-Router# show sip-ua status
Transport: TLS v1.2   ← OK !

CME-Router# show telephony-service all
...
SRTP: Enabled   ← OK !
```

### Mon retour d'expérience

**2016 - Migration TLS/SRTP pour un cabinet d'avocats :**

```
CONTEXTE :
• Cabinet d'avocats pénalistes
• Conversations hautement confidentielles
• Obligation légale de protection des échanges

AVANT (RTP sans chiffrement) :
• Audit révèle possibilité d'écoute WiFi
• Risque pénal pour le cabinet
• Assurance refuse de couvrir

APRÈS (TLS + SRTP) :
• Conformité RGPD
• Certification sécurité par auditeur externe
• Assurance accepte de couvrir
• Tranquillité d'esprit

DIFFICULTÉ :
• Certificats X.509 à déployer sur tous les téléphones
• Augmentation charge CPU routeur (+15%)
• Formation utilisateurs (vérifier cadenas vert)

COÛT :
• Certificats : 800€/an
• Temps de déploiement : 3 jours
• ROI : Immédiat (obligation légale)
```

---

## 🚧 Firewall et ACL

### Principe

**Firewall VoIP** = Contrôler ce qui peut entrer/sortir du réseau VoIP.

### Ports à autoriser

```
┌─────────────────────────────────────────────────────────────┐
│  PORTS VOIP À OUVRIR                                        │
├─────────────────────────────────────────────────────────────┤
│  SIGNALISATION :                                            │
│    • UDP 5060 : SIP (non sécurisé)                          │
│    • TCP 5061 : SIP over TLS (sécurisé)                     │
│    • TCP 2000 : SCCP (Cisco)                                │
│                                                             │
│  MÉDIA (RTP) :                                              │
│    • UDP 16384-32767 : Flux RTP/RTCP                        │
│    (Plage complète pour éviter les problèmes)               │
│                                                             │
│  TFTP (Téléphones Cisco) :                                  │
│    • UDP 69 : TFTP (download des configs)                   │
│                                                             │
│  MANAGEMENT :                                               │
│    • TCP 80/443 : Interface web CME (à restreindre)        │
│    • TCP 22 : SSH (administration)                          │
└─────────────────────────────────────────────────────────────┘
```

### Configuration ACL Cisco

#### ACL de base (entrante sur interface WAN)

```cisco
! Autoriser SIP depuis des IPs connues uniquement
ip access-list extended VOIP-IN
 ! Autoriser SIP depuis notre site distant
 permit udp host 203.0.113.10 any eq 5060
 permit tcp host 203.0.113.10 any eq 5061

 ! Autoriser RTP depuis notre site distant
 permit udp host 203.0.113.10 any range 16384 32767

 ! Bloquer tout le reste
 deny ip any any log
 exit

! Application sur interface WAN
interface GigabitEthernet0/1
 description WAN Internet
 ip access-group VOIP-IN in
```

#### ACL avancée (anti-spoofing + rate-limiting)

```cisco
! Anti-spoofing (bloquer IPs privées venant de l'extérieur)
ip access-list extended ANTI-SPOOF-IN
 ! Bloquer RFC1918 (IPs privées)
 deny ip 10.0.0.0 0.255.255.255 any log
 deny ip 172.16.0.0 0.15.255.255 any log
 deny ip 192.168.0.0 0.0.255.255 any log

 ! Bloquer loopback
 deny ip 127.0.0.0 0.255.255.255 any log

 ! Autoriser le reste (sera filtré par VOIP-IN après)
 permit ip any any
 exit

! Application en premier sur WAN
interface GigabitEthernet0/1
 ip access-group ANTI-SPOOF-IN in
 ! Puis VOIP-IN (voir section précédente)
```

#### Rate-limiting (anti-DoS)

```cisco
! Limiter le débit SIP à 100 paquets/seconde
class-map match-all SIP-TRAFFIC
 match protocol sip
 exit

policy-map SIP-RATE-LIMIT
 class SIP-TRAFFIC
  police 100000 8000 8000 conform-action transmit exceed-action drop
  exit
 exit

! Application
interface GigabitEthernet0/1
 service-policy input SIP-RATE-LIMIT
```

### Architecture Firewall recommandée

```
INTERNET
    │
    │ Firewall externe (avec IPS/IDS)
    │
    ▼
┌─────────────────┐
│  DMZ VoIP       │  ← SBC (Session Border Controller)
│  (Zone neutre)  │     • Masquage NAT
└────────┬────────┘     • Normalisation SIP
         │              • Détection d'attaques
         │ Firewall interne
         │
         ▼
┌─────────────────┐
│  LAN VoIP       │  ← Call Manager + Téléphones
│  (Réseau interne)│
└─────────────────┘
```

**SBC** = **Session Border Controller** = Pare-feu spécialisé VoIP

**Rôle du SBC :**
- Masquage des IPs internes (NAT/PAT)
- Normalisation des messages SIP (sécurité)
- Détection d'attaques (flood, scan)
- Transcoding de codecs
- QoS

**Produits SBC :**
- Cisco Unified Border Element (CUBE)
- Audiocodes Mediant
- Ribbon SBC
- Kamailio (open source)

---

## ✅ Bonnes pratiques

### Checklist sécurité VoIP

```
┌─────────────────────────────────────────────────────────────┐
│  AUTHENTIFICATION                                           │
├─────────────────────────────────────────────────────────────┤
│  ✅ Authentification SIP obligatoire                        │
│  ✅ Mots de passe forts (12+ caractères)                    │
│  ✅ Pas de mots de passe par défaut                         │
│  ✅ Changement régulier (90 jours)                          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  CHIFFREMENT                                                │
├─────────────────────────────────────────────────────────────┤
│  ✅ TLS pour signalisation (SIP)                            │
│  ✅ SRTP pour média (RTP)                                   │
│  ✅ Certificats valides (pas auto-signés en prod)           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  RÉSEAU                                                     │
├─────────────────────────────────────────────────────────────┤
│  ✅ VLAN voix séparé                                        │
│  ✅ ACL strictes (whitelist IPs)                            │
│  ✅ Firewall avec inspection SIP                            │
│  ✅ SBC si exposition Internet                              │
│  ✅ Pas de SIP ouvert sur Internet sans protection          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  SURVEILLANCE                                               │
├─────────────────────────────────────────────────────────────┤
│  ✅ Logs centralisés (syslog)                               │
│  ✅ Alertes sur appels anormaux (international, nuit)       │
│  ✅ Monitoring factures télécom                             │
│  ✅ IDS/IPS VoIP (détection d'attaques)                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  ADMINISTRATION                                             │
├─────────────────────────────────────────────────────────────┤
│  ✅ Interface web CME en HTTPS uniquement                   │
│  ✅ SSH pour CLI (pas Telnet)                               │
│  ✅ Comptes admin individuels (pas de compte partagé)       │
│  ✅ MFA (authentification multi-facteurs)                   │
│  ✅ Mises à jour régulières (firmware, IOS)                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  UTILISATEURS                                               │
├─────────────────────────────────────────────────────────────┤
│  ✅ Sensibilisation au vishing                              │
│  ✅ Politique de mots de passe                              │
│  ✅ Signalement incidents (numéro dédié)                    │
└─────────────────────────────────────────────────────────────┘
```

### Hardening Cisco CME

```cisco
! === DÉSACTIVER SERVICES INUTILES ===
no ip http server
! Désactive serveur HTTP (utiliser HTTPS)

ip http secure-server
! Active HTTPS uniquement

no cdp run
! Désactive CDP si pas nécessaire (fuite d'infos)

no service pad
no ip bootp server
no ip domain-lookup
! Désactive services inutilisés

! === BANNIÈRES LÉGALES ===
banner login ^C
*************************************************************
ATTENTION : Système privé. Accès non autorisé interdit.
Toute tentative d'accès sera enregistrée et poursuivie.
*************************************************************
^C

! === TIMEOUTS ===
line vty 0 4
 exec-timeout 10 0
 ! Déconnexion auto après 10 minutes d'inactivité

! === LOGS ===
logging buffered 51200
logging console critical
logging trap warnings
logging source-interface Loopback0
logging host 192.168.100.10
! Envoi logs vers serveur Syslog

! === SNMP SÉCURISÉ ===
no snmp-server community public RO
no snmp-server community private RW
! Supprime communities par défaut

snmp-server community Tr0ubl@2026 RO
! Community sécurisée

! === SSH UNIQUEMENT ===
line vty 0 4
 transport input ssh
 ! Désactive Telnet, SSH uniquement

ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 2
```

---

## 🎯 Cas pratiques

### Cas 1 : Détection d'une attaque Toll Fraud

**Scénario :**

Lundi matin, vous recevez une alerte de votre outil de monitoring :
> "200 appels vers l'international dans la nuit de samedi à dimanche"

**Mission :** Investiguer et sécuriser.

**Étapes :**

```cisco
! 1. Vérifier les logs d'appels
CME-Router# show call history voice brief
Telephone  Called   Time    Duration Codec    RemoteIPAddr
2001       0021312  02:15   01:45:32 g729     203.0.113.10
2001       0021313  04:00   02:12:18 g729     203.0.113.10
2001       0021314  06:30   01:55:42 g729     203.0.113.10
...
→ Extension 2001 compromise (appels anormaux)

! 2. Désactiver l'extension compromise
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# shutdown
CME-Router(config-ephone-dn)# exit

! 3. Vérifier l'authentification
CME-Router# show telephony-service security
Credential authentication: Disabled   ← PROBLÈME !

! 4. Activer l'authentification
CME-Router(config)# telephony-service
CME-Router(config-telephony)# security authenticate credential
CME-Router(config-telephony)# exit

! 5. Changer tous les mots de passe
CME-Router(config)# username alice secret NewP@ssw0rd!2026
CME-Router(config)# ephone-dn 1
CME-Router(config-ephone-dn)# credential username alice password NewP@ssw0rd!2026
CME-Router(config-ephone-dn)# no shutdown
CME-Router(config-ephone-dn)# exit

! 6. Bloquer les appels internationaux
CME-Router(config)# voice translation-rule 1
CME-Router(cfg-translation-rule)# rule 1 reject /^00/
CME-Router(cfg-translation-rule)# exit

CME-Router(config)# voice translation-profile block-international
CME-Router(cfg-translation-profile)# translate called 1
CME-Router(cfg-translation-profile)# exit

CME-Router(config)# dial-peer voice 100 voip
CME-Router(config-dial-peer)# translation-profile outgoing block-international
CME-Router(config-dial-peer)# exit

! 7. Mettre en place alerting
! (Script externe qui surveille les logs)
```

### Cas 2 : Écoute clandestine détectée

**Scénario :**

Un utilisateur vous signale qu'un concurrent connaît des informations confidentielles discutées au téléphone uniquement.

**Mission :** Vérifier si écoute et sécuriser.

**Étapes :**

```
1. AUDIT RÉSEAU
   • Scan Wireshark sur VLAN voix
   → Capture de 30 minutes

   Résultat : Paquets RTP en clair visibles
   → Pas de chiffrement SRTP

2. VÉRIFICATION PHYSIQUE
   • Inspection switchs (pas de device inconnu branché)
   • Vérification WiFi (pas de rogue AP)

3. ANALYSE ARP
   show ip arp
   → IP en double détectée (ARP Spoofing !)

4. SÉCURISATION
   • Activation Dynamic ARP Inspection (DAI)
   • Déploiement SRTP
   • Isolation VLAN voix/data
   • Formation utilisateurs (ne pas parler d'infos sensibles sur WiFi public)
```

---

## 📚 Ressources

### Documentation officielle

- [Cisco VoIP Security](https://www.cisco.com/c/en/us/products/security/voice-security/index.html)
- [NIST VoIP Security Guide](https://www.nist.gov/publications/security-considerations-voice-over-ip-systems)
- [OWASP VoIP Security](https://owasp.org/www-community/vulnerabilities/VoIP_Security)

### Outils de test

- **VoIPER** : Scanner de vulnérabilités VoIP
- **SIPVicious** : Suite d'audit SIP
- **Vomit** : Conversion RTP vers WAV (écoute)
- **UCSniff** : Sniffing VoIP complet

**ATTENTION :** N'utilisez ces outils QUE sur vos propres systèmes ou avec autorisation écrite !

### Articles recommandés

- [Toll Fraud Case Studies](https://www.google.com/search?q=voip+toll+fraud+case+study)
- [SRTP Implementation Best Practices](https://www.google.com/search?q=srtp+best+practices)

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

**Incidents rencontrés :**
-
-

---

## ✅ Checklist de révision

Avant de passer au cours suivant, assurez-vous de maîtriser :

- [ ] Je connais les 5 menaces VoIP principales
- [ ] Je comprends le Toll Fraud et comment l'éviter
- [ ] Je sais configurer l'authentification SIP
- [ ] Je connais la différence entre TLS et SRTP
- [ ] Je peux configurer un firewall pour VoIP (ports)
- [ ] Je sais créer des ACL restrictives
- [ ] Je connais les bonnes pratiques de sécurité VoIP
- [ ] Je peux investiguer un incident de sécurité VoIP
- [ ] Je sais mettre en place du monitoring/alerting

---

<div align="center">

**Cours précédent :** [04-qos-vlans-voip.md](04-qos-vlans-voip.md)

**Cours suivant :** [06-depannage-voip.md](06-depannage-voip.md)

[⬅️ Retour au sommaire](README.md)

</div>
