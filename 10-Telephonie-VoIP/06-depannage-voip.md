# Dépannage VoIP - Méthodologie et outils du technicien

> 📚 **Module :** Téléphonie VoIP - Dépannage
> 📅 **Date :** Février 2026
> ⏱️ **Durée :** 3 heures
> 🎯 **Niveau :** Intermédiaire/Avancé
> 👨‍🏫 **Approche :** 100% Terrain et Pratique

---

## 📖 Table des matières

- [Message de votre formateur](#-message-de-votre-formateur)
- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Méthodologie de diagnostic](#-méthodologie-de-diagnostic)
- [Problèmes courants et solutions](#-problèmes-courants-et-solutions)
- [Analyse Wireshark](#-analyse-wireshark)
- [Commandes Cisco essentielles](#-commandes-cisco-essentielles)
- [Outils du technicien](#-outils-du-technicien)
- [Cas pratiques](#-cas-pratiques)
- [Ressources](#-ressources)

---

## 👨‍🏫 Message de votre formateur

Bonjour à tous,

**15 ans de dépannage VoIP** = Des centaines d'incidents résolus.

**La vérité :** 80% des problèmes VoIP sont dus à **5 causes** :
1. DHCP mal configuré (option 150 manquante)
2. QoS absente ou mal configurée
3. Problème réseau (câble défectueux, switch saturé)
4. Firewall qui bloque RTP
5. Mauvaise configuration ephone/ephone-dn

**Mon pire incident :** 2009, un vendredi à 17h45, 200 téléphones hors service. Cause ? Un stagiaire a fait un `erase startup-config` sur le mauvais routeur.
**Solution ?** Restauration backup + reconfiguration = 6 heures de travail.
**Leçon ?** Toujours faire des **backups**, et **vérifier** avant de valider une commande critique.

**Ce que je vais vous apprendre :**
- Une **méthodologie** qui marche (7 étapes)
- Les **outils** qui sauvent (Wireshark, commandes Cisco)
- Des **réflexes** de pro (où chercher, comment isoler)

### 🎯 Ma promesse

À la fin de ces 3 heures, vous saurez :
- ✅ Diagnostiquer 90% des problèmes VoIP
- ✅ Utiliser Wireshark pour analyser SIP/RTP
- ✅ Maîtriser les commandes de vérification Cisco
- ✅ Créer votre boîte à outils de technicien VoIP
- ✅ Résoudre des incidents **rapidement**

**Le dépannage, c'est 20% de connaissance, 80% de méthodologie !** 💪

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Appliquer** une méthodologie de diagnostic structurée
- ✅ **Identifier** rapidement la couche défaillante (L1, L2, L3, applicatif)
- ✅ **Analyser** un échange SIP/RTP avec Wireshark
- ✅ **Utiliser** les commandes Cisco de diagnostic
- ✅ **Résoudre** les problèmes courants (enregistrement, qualité, appels)
- ✅ **Documenter** un incident pour éviter sa répétition

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Avoir suivi **tous les cours précédents** (01 à 05)
- [ ] Maîtriser la **configuration CME**
- [ ] Connaître les **protocoles SIP et RTP**
- [ ] Savoir utiliser **Wireshark** (bases)
- [ ] Comprendre le **modèle OSI** (L1 à L7)

**Matériel nécessaire :**
- 💻 PC avec Packet Tracer et Wireshark
- 📝 De quoi prendre des notes
- ☕ Un café (le dépannage, c'est intense !)

---

## 🔍 Méthodologie de diagnostic

### Les 7 étapes de la méthodologie

```
┌─────────────────────────────────────────────────────────────┐
│  MÉTHODE DES 7 ÉTAPES (À SUIVRE SYSTÉMATIQUEMENT)          │
├─────────────────────────────────────────────────────────────┤
│  1. QUALIFIER LE PROBLÈME                                   │
│     → Qui ? Quoi ? Quand ? Où ? Comment ?                   │
│                                                             │
│  2. RECUEILLIR LES INFORMATIONS                             │
│     → Logs, symptômes, contexte                             │
│                                                             │
│  3. IDENTIFIER LA COUCHE OSI AFFECTÉE                       │
│     → L1 (câble) ? L2 (switch) ? L3 (routage) ? L7 (app) ? │
│                                                             │
│  4. ISOLER LA CAUSE                                         │
│     → Tests par élimination                                 │
│                                                             │
│  5. APPLIQUER LA SOLUTION                                   │
│     → Corriger la cause identifiée                          │
│                                                             │
│  6. VÉRIFIER LE RETOUR À LA NORMALE                         │
│     → Tester que ça fonctionne                              │
│                                                             │
│  7. DOCUMENTER                                              │
│     → Écrire l'incident + solution (base de connaissances)  │
└─────────────────────────────────────────────────────────────┘
```

### Étape 1 : Qualifier le problème

**Posez les bonnes questions :**

```
┌──────────────────────────────────────────────────────────┐
│  QUI ?                                                   │
├──────────────────────────────────────────────────────────┤
│  • Un utilisateur ? Plusieurs ? Tous ?                   │
│  • Un site ? Plusieurs sites ?                           │
│  • Un type de téléphone ? (modèle, firmware)             │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  QUOI ?                                                  │
├──────────────────────────────────────────────────────────┤
│  • Pas de tonalité ?                                     │
│  • Appels ne passent pas ?                               │
│  • Qualité vocale dégradée ?                             │
│  • Téléphones non enregistrés ?                          │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  QUAND ?                                                 │
├──────────────────────────────────────────────────────────┤
│  • Depuis quand ?                                        │
│  • Permanent ou intermittent ?                           │
│  • À certaines heures ? (pic d'activité)                 │
│  • Après un changement ? (config, mise à jour)           │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  OÙ ?                                                    │
├──────────────────────────────────────────────────────────┤
│  • Localisé (un bureau, un étage) ?                      │
│  • Site distant ?                                        │
│  • Généralisé (tous les sites) ?                         │
└──────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│  COMMENT ?                                               │
├──────────────────────────────────────────────────────────┤
│  • Conditions de reproduction                            │
│  • Appels internes ? Externes ? Les deux ?               │
│  • Avec tous les correspondants ?                        │
└──────────────────────────────────────────────────────────┘
```

**Exemple concret :**

```
❌ MAUVAIS :
"Les téléphones ne marchent pas."
→ Trop vague, impossible de diagnostiquer

✅ BON :
"Depuis ce matin 9h, 5 utilisateurs du 2e étage (bureau 201-205)
n'ont pas de tonalité sur leur téléphone Cisco 7841.
Ils ont des IP en DHCP. Les autres étages fonctionnent normalement.
Aucun changement n'a été fait ce week-end."
→ Précis, on peut commencer à investiguer
```

### Étape 2 : Recueillir les informations

**Sources d'information :**

```
📋 LOGS
────────────────────────────────────────────────────────
• Logs routeur CME : show logging
• Logs téléphones : Sur l'écran du phone (bouton "i")
• Logs switch : show logging
• Syslog centralisé : Vérifier le serveur Syslog

📊 MONITORING
────────────────────────────────────────────────────────
• Dashboard CME (interface web)
• SNMP (graphes de charge CPU, BP, erreurs)
• Outils de supervision (PRTG, Zabbix, Nagios)

👁️ OBSERVATION DIRECTE
────────────────────────────────────────────────────────
• État des LEDs du téléphone
• Messages d'erreur à l'écran
• Tonalité présente ou non
• Test d'un appel simple

🔧 COMMANDES DE VÉRIFICATION
────────────────────────────────────────────────────────
• show ephone registered
• show ephone summary
• show call active voice brief
• show interface status
```

### Étape 3 : Identifier la couche OSI

**Approche Bottom-Up (de bas en haut) :**

```
┌─────────────────────────────────────────────────────────────┐
│  COUCHE 1 - PHYSIQUE                                        │
├─────────────────────────────────────────────────────────────┤
│  Symptômes :                                                │
│    • Téléphone éteint (pas de LED)                          │
│    • Port switch "down"                                     │
│    • Pas d'IP (DHCP impossible)                             │
│                                                             │
│  Tests :                                                    │
│    • Vérifier câble réseau (changer le câble)               │
│    • Vérifier PoE (voltage, wattage)                        │
│    • show interface Fa0/1 (status, protocol)                │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  COUCHE 2 - LIAISON                                         │
├─────────────────────────────────────────────────────────────┤
│  Symptômes :                                                │
│    • Téléphone allumé mais pas d'IP                         │
│    • VLAN voix mal configuré                                │
│    • Boucles réseau (STP)                                   │
│                                                             │
│  Tests :                                                    │
│    • show vlan brief                                        │
│    • show interfaces switchport (VLAN voix présent ?)       │
│    • show spanning-tree (blocages ?)                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  COUCHE 3 - RÉSEAU                                          │
├─────────────────────────────────────────────────────────────┤
│  Symptômes :                                                │
│    • Téléphone a une IP mais ne s'enregistre pas            │
│    • Pas de route vers le CME                               │
│    • Problème DHCP (option 150)                             │
│                                                             │
│  Tests :                                                    │
│    • ping depuis le téléphone vers CME                      │
│    • show ip dhcp binding                                   │
│    • show ip route                                          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  COUCHE 4 - TRANSPORT                                       │
├─────────────────────────────────────────────────────────────┤
│  Symptômes :                                                │
│    • Ping OK mais enregistrement échoue                     │
│    • Firewall bloque les ports (5060, 2000)                 │
│    • ACL bloque SIP/SCCP                                    │
│                                                             │
│  Tests :                                                    │
│    • telnet 192.168.10.1 2000 (test port SCCP)              │
│    • show ip access-lists                                   │
│    • Capture Wireshark (trafic présent ?)                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  COUCHE 7 - APPLICATION                                     │
├─────────────────────────────────────────────────────────────┤
│  Symptômes :                                                │
│    • Téléphone enregistré mais pas de tonalité              │
│    • Appels ne passent pas (busy, erreur)                   │
│    • Qualité vocale médiocre                                │
│                                                             │
│  Tests :                                                    │
│    • show ephone registered (statut ?)                      │
│    • show ephone-dn (numéro configuré ?)                    │
│    • Wireshark (analyse SIP/RTP)                            │
└─────────────────────────────────────────────────────────────┘
```

### Mon exemple de diagnostic structuré

**Incident réel (2018) :**

```
PROBLÈME SIGNALÉ :
"Depuis 10h ce matin, tous les appels vers l'extérieur sont coupés
après 30 secondes exactement. Appels internes OK."

ÉTAPE 1 - QUALIFIER :
• Qui : Tous les utilisateurs
• Quoi : Coupure appels externes après 30s
• Quand : Depuis 10h (permanent)
• Où : Tous les sites
• Comment : Systématique, toujours 30s

ÉTAPE 2 - INFORMATIONS :
• Logs CME : "SIP 408 Request Timeout"
• Aucun changement de config ce matin
• Appels internes fonctionnent (SIP entre téléphones)
• Appels externes (vers PSTN via trunk SIP opérateur)

ÉTAPE 3 - COUCHE OSI :
• L1/L2/L3 : OK (appels internes fonctionnent)
• L4 : Suspect (timeout après 30s = problème TCP/UDP ?)
• L7 : Très suspect (408 = timeout applicatif SIP)

ÉTAPE 4 - ISOLER :
Capture Wireshark sur trunk SIP :
→ INVITE envoyé à l'opérateur
→ 180 Ringing reçu
→ 200 OK reçu
→ ACK NON ENVOYÉ ! ← PROBLÈME

Cause identifiée : Firewall upstream bloque les ACK SIP
(nouvelle règle ajoutée par l'équipe réseau à 10h sans nous prévenir)

ÉTAPE 5 - SOLUTION :
Contact équipe réseau → Correction de la règle firewall

ÉTAPE 6 - VÉRIFICATION :
Test appels externes → OK, plus de coupure

ÉTAPE 7 - DOCUMENTATION :
Incident documenté + Demande de validation VoIP pour tous
changements firewall à l'avenir.

TEMPS TOTAL : 45 minutes (grâce à la méthodologie)
```

---

## 🛠️ Problèmes courants et solutions

### Problème 1 : Téléphone bloqué "Configuring IP"

**Symptômes :**
```
LED du téléphone : Orange clignotant
Écran : "Configuring IP" ou "Obtaining IP Address"
Durée : Indéfinie (bloqué)
```

**Causes possibles :**

| Cause | Test | Solution |
|-------|------|----------|
| **Pas de DHCP** | `show ip dhcp binding` sur routeur | Configurer DHCP |
| **Option 150 manquante** | `show run \| include option 150` | Ajouter `option 150 ip X.X.X.X` |
| **VLAN voix mal configuré** | `show int Fa0/1 switchport` | `switchport voice vlan 10` |
| **Câble défectueux** | Changer le câble | Remplacer le câble |
| **PoE insuffisant** | `show power inline Fa0/1` | Vérifier budget PoE switch |

**Solution pas à pas :**

```cisco
! 1. Vérifier le DHCP
Router# show ip dhcp pool VOICE
Pool VOICE :
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 option 150 ip 192.168.10.1   ← Doit être présent !

! Si option 150 absente :
Router(config)# ip dhcp pool VOICE
Router(dhcp-config)# option 150 ip 192.168.10.1
Router(dhcp-config)# exit

! 2. Vérifier le VLAN voix sur le switch
Switch# show interfaces FastEthernet 0/1 switchport
...
Access Mode VLAN: 1 (DATA)
Voice VLAN: 10 (VOICE)   ← Doit être configuré !

! Si VLAN voix absent :
Switch(config)# interface FastEthernet 0/1
Switch(config-if)# switchport voice vlan 10
Switch(config-if)# exit

! 3. Débrancher/rebrancher le téléphone
! (ou restart via CME)
Router(config)# telephony-service
Router(config-telephony)# reset all
```

---

### Problème 2 : Téléphone non enregistré

**Symptômes :**
```
LED du téléphone : Rouge fixe
Écran : "Unregistered" ou numéro grisé
Pas de tonalité
```

**Diagnostic :**

```cisco
! Vérifier les téléphones enregistrés
Router# show ephone registered
! Si le téléphone n'apparaît pas :

! Vérifier les téléphones non enregistrés
Router# show ephone unregistered
ephone-1 Mac:0001.9641.D4A1 NOT REGISTERED
```

**Causes + Solutions :**

```cisco
! === CAUSE 1 : MAC address incorrecte ===
Router# show ephone 1
ephone-1 Mac:0001.9641.D4A1   ← Vérifier que c'est bien la MAC du phone

! Pour voir la vraie MAC du téléphone :
! Appuyer sur le bouton "i" ou "?" sur le phone
! Ou : show ephone unregistered

! Corriger si besoin :
Router(config)# ephone 1
Router(config-ephone)# no mac-address 0001.9641.D4A1
Router(config-ephone)# mac-address 0001.9641.XXXX
! (remplacer XXXX par la vraie MAC)
Router(config-ephone)# exit

! === CAUSE 2 : Pas de ephone-dn associé ===
Router# show ephone 1
button 1: dn 1  number 2001 CH1   IDLE   ← Doit être présent

! Si pas de button configuré :
Router(config)# ephone 1
Router(config-ephone)# button 1:1
! (Bouton 1 = ephone-dn 1)
Router(config-ephone)# exit

! === CAUSE 3 : Fichiers CNF manquants ===
Router# show telephony-service tftp-bindings
tftp-server system:/its/SEP0001.9641.D4A1.cnf alias SEP0001...
! Si aucun fichier CNF :

Router(config)# telephony-service
Router(config-telephony)# create cnf-files
Creating CNF files... OK
Router(config-telephony)# exit

! === CAUSE 4 : IP source-address incorrecte ===
Router# show telephony-service all
Ip Address: 192.168.10.1 Port 2000   ← Doit être l'IP correcte

! Si incorrecte :
Router(config)# telephony-service
Router(config-telephony)# no ip source-address
Router(config-telephony)# ip source-address 192.168.10.1 port 2000
Router(config-telephony)# exit

! === APRÈS TOUTE MODIFICATION ===
Router(config)# telephony-service
Router(config-telephony)# reset all
! Redémarre tous les téléphones
```

---

### Problème 3 : Pas de tonalité

**Symptômes :**
```
LED du téléphone : Verte
Écran : Numéro affiché (ex: 2001)
État : Téléphone enregistré
Problème : Aucune tonalité au décroché
```

**Diagnostic :**

```cisco
! 1. Vérifier que le téléphone est bien enregistré
Router# show ephone 1
ephone-1 Mac:0001.9641.D4A1 TCP socket:[1] activeLine:0 REGISTERED
button 1: dn 1  number 2001 CH1   IDLE

! 2. Vérifier le ephone-dn
Router# show ephone-dn 1
ephone-dn 1
 number 2001   ← Numéro configuré
 name Alice
 ...

! 3. Vérifier que le button est bien associé
Router# show ephone 1 | include button
button 1: dn 1  number 2001 CH1   IDLE   ← OK
```

**Causes + Solutions :**

```cisco
! === CAUSE 1 : Button mal configuré ===
Router(config)# ephone 1
Router(config-ephone)# no button 1
Router(config-ephone)# button 1:1
Router(config-ephone)# exit

! === CAUSE 2 : ephone-dn en shutdown ===
Router# show ephone-dn 1 | include shutdown
! Si "shutdown" apparaît :

Router(config)# ephone-dn 1
Router(config-ephone-dn)# no shutdown
Router(config-ephone-dn)# exit

! === CAUSE 3 : Problème firmware ===
Router# show ephone phone-load
! Vérifier que le firmware est correct

! Si besoin, forcer un reload :
Router(config)# ephone 1
Router(config-ephone)# reset
Router(config-ephone)# exit
```

---

### Problème 4 : Appels ne passent pas

**Symptômes :**
```
Tonalité : OK
Composition du numéro : OK
Problème : Tonalité d'occupation, ou rien ne se passe
```

**Diagnostic structuré :**

```cisco
! 1. Vérifier que les 2 ephone-dn existent
Router# show ephone-dn summary
DN  Tag  Num       State           CH  Port
1   1    2001      IDLE            0   -
2   2    2002      IDLE            0   -

! 2. Tester un appel et observer les états
Router# show ephone registered
! Pendant l'appel :
ephone-1 button 1: dn 1  number 2001 CH1   CONNECTED   ← OK
ephone-2 button 1: dn 2  number 2002 CH1   CONNECTED   ← OK

! 3. Vérifier les appels actifs
Router# show call active voice brief
<Hangup Time>  <Call ID> <Remote IP:port>  <Codec>  <Dial-peer>
10:25:32          1       192.168.10.102    g711ulaw    1001
```

**Causes + Solutions :**

```cisco
! === CAUSE 1 : ephone-dn en mode single-line ===
! (Ne permet qu'un appel à la fois, pas de transfert/conf)

Router(config)# ephone-dn 1
Router(config-ephone-dn)# no number
Router(config-ephone-dn)# exit

Router(config)# no ephone-dn 1
Router(config)# ephone-dn 1 dual-line
! Recréer en dual-line
Router(config-ephone-dn)# number 2001
Router(config-ephone-dn)# name Alice
Router(config-ephone-dn)# exit

! === CAUSE 2 : Codec incompatible ===
Router# show telephony-service all
Codec: g711ulaw, g729r8   ← Doit être compatible entre phones

! === CAUSE 3 : Problème de dial-peer (appels externes) ===
Router# show dial-peer voice summary
! Vérifier que les dial-peers correspondent aux numéros

! === CAUSE 4 : Pas de allow-connections ===
Router(config)# ephone-dn 1
Router(config-ephone-dn)# allow-connections all to all
Router(config-ephone-dn)# exit
```

---

### Problème 5 : Qualité vocale dégradée

**Symptômes :**
```
Appels passent : OK
Problème : Voix hachée, robotique, écho, coupures
```

**Causes par symptôme :**

| Symptôme | Cause probable | Test | Solution |
|----------|----------------|------|----------|
| **Voix hachée/robotique** | Jitter, perte paquets | Wireshark RTP analysis | QoS, BP suffisante |
| **Écho** | Latence > 300ms | `ping` avec timestamp | Réduire latence |
| **Coupures** | Saturation BP | `show interface` (drops) | QoS, upgrade BP |
| **Volume faible** | Gain mal réglé | Réglage téléphone | Ajuster gain |
| **Bruit de fond** | Interférences | Changer câble | Cable blindé |

**Diagnostic avec Wireshark :**

```
1. Capturer un appel avec Wireshark
   Filtre : rtp

2. Analyser le flux RTP
   Telephony → RTP → Stream Analysis

3. Vérifier les métriques :
   ┌─────────────────────────────────────────┐
   │ Max Delta    : < 30 ms    ✅ OK        │
   │ Max Jitter   : < 30 ms    ✅ OK        │
   │ Lost packets : < 1%       ✅ OK        │
   │ Sequence errors : 0       ✅ OK        │
   └─────────────────────────────────────────┘

Si une métrique est hors limites :
→ Problème QoS / réseau
```

**Solution QoS :**

```cisco
! Vérifier que QoS est activée
Switch# show mls qos
QoS is enabled   ← Doit être activé

! Vérifier la QoS sur routeur WAN
Router# show policy-map interface Serial 0/0/0
Class-map: VOICE-RTP (match-any)
  (priority)
  Output Queue: Conversation 256
  Bandwidth 33% (660 kbps)   ← OK
  (pkts matched/bytes matched) 12345/1234567
  (total drops/bytes drops) 0/0   ← Doit être 0 !

! Si drops > 0 :
! → Augmenter priority ou BP du lien
```

---

## 📡 Analyse Wireshark

### Configuration Wireshark pour VoIP

**Filtres essentiels :**

```
┌─────────────────────────────────────────────────────────────┐
│  FILTRES WIRESHARK VOIP                                     │
├─────────────────────────────────────────────────────────────┤
│  sip                  : Tout le trafic SIP                  │
│  sip.Method == INVITE : Uniquement les INVITE              │
│  sip.Status-Code      : Codes réponse (180, 200, 404, ...)│
│  rtp                  : Tout le trafic RTP                  │
│  rtcp                 : Contrôle RTP (stats qualité)        │
│  sccp                 : Trafic SCCP (Cisco)                 │
└─────────────────────────────────────────────────────────────┘
```

### Analyser un appel SIP complet

**Étapes :**

```
1. Démarrer la capture Wireshark
2. Lancer un appel (ex: 2001 appelle 2002)
3. Stopper la capture
4. Appliquer filtre : sip || rtp
5. Analyser la séquence
```

**Séquence normale :**

```
Paquet  Temps   Source          Dest            Protocole  Info
──────────────────────────────────────────────────────────────────
1       0.000   192.168.10.101  192.168.10.1    SIP        INVITE sip:2002@...
2       0.010   192.168.10.1    192.168.10.101  SIP        Status: 100 Trying
3       0.050   192.168.10.1    192.168.10.102  SIP        INVITE sip:2002@...
4       0.080   192.168.10.102  192.168.10.1    SIP        Status: 180 Ringing
5       0.090   192.168.10.1    192.168.10.101  SIP        Status: 180 Ringing
6       2.500   192.168.10.102  192.168.10.1    SIP        Status: 200 OK
7       2.510   192.168.10.1    192.168.10.101  SIP        Status: 200 OK
8       2.520   192.168.10.101  192.168.10.1    SIP        ACK sip:2002@...
9       2.530   192.168.10.1    192.168.10.102  SIP        ACK sip:2002@...
──────────────────────────────────────────────────────────────────
10      2.540   192.168.10.101  192.168.10.102  RTP        Payload type=PCMU
11      2.560   192.168.10.102  192.168.10.101  RTP        Payload type=PCMU
12      2.580   192.168.10.101  192.168.10.102  RTP        Payload type=PCMU
...     (RTP continu pendant la conversation)
──────────────────────────────────────────────────────────────────
500     60.000  192.168.10.101  192.168.10.1    SIP        BYE sip:2002@...
501     60.010  192.168.10.1    192.168.10.102  SIP        BYE sip:2002@...
502     60.020  192.168.10.102  192.168.10.1    SIP        Status: 200 OK
503     60.030  192.168.10.1    192.168.10.101  SIP        Status: 200 OK
```

**Points à vérifier :**
- ✅ INVITE → 100 Trying → 180 Ringing → 200 OK → ACK
- ✅ RTP commence après l'ACK
- ✅ Pas de 4xx (erreur client) ou 5xx (erreur serveur)
- ✅ BYE → 200 OK à la fin

### Analyser la qualité RTP

**Menu Wireshark :**

```
Telephony → RTP → Stream Analysis
```

**Fenêtre d'analyse RTP :**

```
┌─────────────────────────────────────────────────────────────┐
│  RTP STREAM ANALYSIS                                        │
├─────────────────────────────────────────────────────────────┤
│  Start Time         : 2.540                                 │
│  Duration           : 57.460 sec                            │
│  SSRC               : 0x12345678                            │
│  Payload Type       : PCMU (0)                              │
│  Packets            : 2873                                  │
│  Lost               : 0 (0.0%)          ← ✅ OK (< 1%)     │
│  Max Delta          : 24.53 ms          ← ✅ OK (< 30 ms)  │
│  Max Jitter         : 12.34 ms          ← ✅ OK (< 30 ms)  │
│  Mean Jitter        : 3.21 ms           ← ✅ OK            │
│  Sequence Errors    : 0                 ← ✅ OK            │
└─────────────────────────────────────────────────────────────┘
```

**Interprétation :**

```
✅ EXCELLENT : Lost < 0.5%, Jitter < 20 ms
⚠️  ACCEPTABLE : Lost 0.5-1%, Jitter 20-30 ms
❌ MAUVAIS : Lost > 1%, Jitter > 30 ms
```

### Écouter la conversation (RTP)

**Attention : Uniquement si RTP NON chiffré (pas SRTP) !**

```
Telephony → RTP → Stream Analysis → Player

→ Vous pouvez écouter la conversation capturée !
```

**Utilisation éthique :**
- ✅ Sur VOS propres systèmes de test
- ✅ Avec autorisation écrite pour audit
- ❌ JAMAIS sans autorisation (illégal !)

---

## 🖥️ Commandes Cisco essentielles

### Commandes de vérification globale

```cisco
! === ÉTAT GÉNÉRAL CME ===
show telephony-service all
! Affiche toute la config CME (max phones, IP source, etc.)

show ephone summary
! Résumé de tous les téléphones

show ephone-dn summary
! Résumé de tous les numéros

show call active voice brief
! Appels actifs en ce moment

show voice call summary
! Statistiques globales des appels
```

### Commandes de vérification téléphones

```cisco
! === TÉLÉPHONES ENREGISTRÉS ===
show ephone registered
! Liste tous les téléphones enregistrés avec détails

show ephone 1
! Détails du téléphone 1 (MAC, IP, button, état)

show ephone phone-load
! Firmware chargé sur les téléphones

! === TÉLÉPHONES NON ENREGISTRÉS ===
show ephone unregistered
! Liste les téléphones vus mais non enregistrés

show ephone attempts
! Historique des tentatives d'enregistrement
```

### Commandes de vérification numéros

```cisco
show ephone-dn 1
! Détails du ephone-dn 1 (numéro, nom, call-forward, etc.)

show ephone-dn summary
! Résumé de tous les ephone-dn

show ephone-dn statistics
! Stats d'utilisation des lignes
```

### Commandes de vérification réseau

```cisco
! === DHCP ===
show ip dhcp binding
! Baux DHCP actifs (IP attribuées)

show ip dhcp pool VOICE
! Détails du pool DHCP voix

show ip dhcp conflict
! Conflits d'IP détectés

! === VLAN / SWITCH ===
show vlan brief
! Liste des VLANs

show interfaces FastEthernet 0/1 switchport
! Config du port (VLAN access, VLAN voice)

show interfaces trunk
! État des trunks

! === QOS ===
show mls qos
! État de la QoS (enabled/disabled)

show mls qos interface Fa0/1
! QoS sur un port

show policy-map interface Se0/0/0
! QoS sur routeur (stats, drops)
```

### Commandes de diagnostic avancé

```cisco
! === DEBUG (ATTENTION : VERBEUX !) ===
debug ephone register
! Voir les tentatives d'enregistrement

debug ephone detail
! Détails complets des événements téléphones

debug voice rtp session named-event
! Stats RTP en temps réel

! IMPORTANT : Arrêter les debugs après usage !
no debug all
undebug all
```

### Mon script de diagnostic rapide

**À exécuter en cas de problème :**

```cisco
! ===========================================================
! SCRIPT DE DIAGNOSTIC VOIP - COPIER/COLLER
! ===========================================================

show clock
! Timestamp

show telephony-service all | include (max-ephones|max-dn|ip source)
! Config de base CME

show ephone registered | count
! Nombre de téléphones enregistrés

show ephone summary
! Résumé téléphones

show ephone-dn summary
! Résumé numéros

show call active voice brief
! Appels en cours

show ip dhcp binding | include Voice
! Baux DHCP voix

show interface | include (line protocol|errors|drops)
! Erreurs réseau

show logging | include (ERR|WARN)
! Logs d'erreurs

! ===========================================================
! SAUVEGARDER LA SORTIE ET ANALYSER
! ===========================================================
```

---

## 🧰 Outils du technicien

### Boîte à outils logicielle

```
┌─────────────────────────────────────────────────────────────┐
│  OUTILS ESSENTIELS                                          │
├─────────────────────────────────────────────────────────────┤
│  🔬 Wireshark         : Capture et analyse réseau           │
│  📞 Cisco IP Communicator : Softphone pour tests            │
│  🔧 Putty / SecureCRT : Terminal SSH                        │
│  📊 PRTG / Zabbix     : Monitoring                          │
│  📝 Notepad++         : Édition configs                     │
│  🌐 Chrome / Firefox  : Interface web CME                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  OUTILS AVANCÉS                                             │
├─────────────────────────────────────────────────────────────┤
│  🔍 SIPp              : Test de charge SIP                  │
│  📡 VoIPER            : Scanner vulnérabilités VoIP         │
│  🎤 Audacity          : Analyse audio (fichiers voix)       │
│  🔐 Kali Linux        : Tests sécurité (autorisation !)     │
└─────────────────────────────────────────────────────────────┘
```

### Boîte à outils matérielle

```
┌─────────────────────────────────────────────────────────────┐
│  KIT TECHNICIEN VOIP                                        │
├─────────────────────────────────────────────────────────────┤
│  ✅ Câbles RJ45 (x5, différentes longueurs)                 │
│  ✅ Testeur de câble RJ45                                   │
│  ✅ Multimètre (test PoE voltage)                           │
│  ✅ Switch 8 ports (PoE) pour tests                         │
│  ✅ Adaptateur USB-RJ45 (pour PC portable)                  │
│  ✅ Casque USB (pour softphone)                             │
│  ✅ Téléphone IP de test (Cisco 7841)                       │
│  ✅ Injecteur PoE                                           │
│  ✅ Console câble (USB vers RJ45/Serial)                    │
│  ✅ Hub USB (ports pour tous les câbles !)                  │
└─────────────────────────────────────────────────────────────┘
```

### Checklist d'intervention sur site

```
┌─────────────────────────────────────────────────────────────┐
│  AVANT DE PARTIR EN INTERVENTION                            │
├─────────────────────────────────────────────────────────────┤
│  ☐ Laptop chargé + chargeur                                │
│  ☐ Tous les logiciels installés (Putty, Wireshark)         │
│  ☐ Câbles réseau (x5)                                       │
│  ☐ Console câble                                            │
│  ☐ Accès VPN configuré (si intervention distante)          │
│  ☐ Identifiants admin notés (dans gestionnaire mdp)        │
│  ☐ Backup de la config récente                             │
│  ☐ Téléphone du client / contact sur place                 │
│  ☐ Bouteille d'eau + snack (ça peut être long !)           │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Cas pratiques

### Cas 1 : Tous les téléphones hors service (critique)

**Scénario :**

Lundi 08h30, vous arrivez au bureau. Tous les téléphones affichent "Unregistered". Panique générale.

**Mission : Rétablir le service le plus vite possible.**

**Diagnostic :**

```cisco
! 1. Vérifier l'état du routeur CME
Router# show ephone registered
! Aucun téléphone enregistré

! 2. Vérifier le service CME
Router# show telephony-service all
CME Version 9.0
IP Address: 192.168.10.1 Port 2000   ← OK
Max phones: 20
Max DNs: 50

! 3. Vérifier la connectivité réseau
Router# show ip interface brief
GigabitEthernet0/0     192.168.10.1    YES manual up                    up

! 4. Vérifier les logs
Router# show logging | include (ERR|CRIT)
*Jan 13 08:15:23.456: %SYS-3-CPUHOG: Task is running for (2000)msecs
*Jan 13 08:15:25.123: %VOICE-3-LINEDOWN: All lines down, CME service stopped

! → Problème identifié : Charge CPU excessive

Router# show processes cpu sorted
CPU utilization for five seconds: 99%/95%; one minute: 98%; five minutes: 97%

PID  Runtime(ms)   Invoked    uSecs   5Sec   1Min   5Min TTY Process
123  1234567890    9876543    12345   98%    97%    96%  0   Backup Process

! → Un processus de backup monopolise le CPU

! 5. Solution immédiate : Arrêter le backup
Router# no backup   (ou identifier et tuer le processus problématique)

! 6. Redémarrer le service CME
Router# conf t
Router(config)# telephony-service
Router(config-telephony)# reset all
Router(config-telephony)# exit

! 7. Vérifier le retour à la normale
Router# show ephone registered
! → Téléphones reviennent progressivement

TEMPS DE RÉSOLUTION : 10 minutes
```

---

### Cas 2 : Qualité vocale dégradée le matin uniquement

**Scénario :**

Tous les matins entre 9h et 10h30, les utilisateurs se plaignent de voix hachée. Après 10h30, tout redevient normal.

**Mission : Identifier et corriger la cause.**

**Diagnostic :**

```
1. QUALIFIER LE PROBLÈME
   • Qui : Tous les utilisateurs
   • Quoi : Voix hachée
   • Quand : 9h-10h30 tous les jours
   • Où : Tous les sites

2. OBSERVATION
   • Horaire 9h-10h30 = heure de pointe réseau (arrivée des employés)
   • Corrélation : Téléchargement emails, synchronisation cloud

3. CAPTURE WIRESHARK (à 9h15, pendant le problème)
   Filtre : rtp
   Telephony → RTP → Stream Analysis

   Résultat :
   • Lost packets : 3.5%   ← ❌ MAUVAIS (> 1%)
   • Max Jitter : 87 ms    ← ❌ MAUVAIS (> 30 ms)

   → Problème de réseau (congestion)

4. VÉRIFIER LA QOS

   Switch# show mls qos
   QoS is disabled   ← ❌ PROBLÈME !

   Router# show policy-map interface Gi0/0
   ! Aucune policy-map appliquée   ← ❌ PROBLÈME !

5. SOLUTION : Activer QoS

   ! Sur switch
   Switch(config)# mls qos
   Switch(config)# interface range Fa0/1 - 24
   Switch(config-if-range)# auto qos voip cisco-phone
   Switch(config-if-range)# exit

   ! Sur routeur (si WAN)
   Router(config)# class-map VOICE-RTP
   Router(config-cmap)# match ip dscp ef
   Router(config-cmap)# exit

   Router(config)# policy-map WAN-QOS
   Router(config-pmap)# class VOICE-RTP
   Router(config-pmap-c)# priority percent 33
   Router(config-pmap-c)# exit
   Router(config-pmap)# exit

   Router(config)# interface GigabitEthernet0/0
   Router(config-if)# service-policy output WAN-QOS
   Router(config-if)# exit

6. VÉRIFICATION (lendemain à 9h30)
   Capture Wireshark :
   • Lost packets : 0.1%   ← ✅ OK
   • Max Jitter : 15 ms    ← ✅ OK

   Utilisateurs : Plus de plainte !

TEMPS DE RÉSOLUTION : 1 heure (+ 24h de vérification)
```

---

### Cas 3 : Un utilisateur ne peut pas appeler l'extérieur

**Scénario :**

L'utilisateur Alice (2001) peut appeler en interne mais reçoit "busy" quand elle compose un numéro externe (0123456789).

**Mission : Résoudre pour Alice uniquement.**

**Diagnostic :**

```cisco
! 1. Vérifier la config d'Alice
Router# show ephone-dn 1
ephone-dn 1 dual-line
 number 2001
 name Alice
 ...

! 2. Tester un appel externe avec un autre utilisateur (Bob)
! Bob (2002) → 0123456789 : ✅ Fonctionne

! 3. Différence entre Alice et Bob ?

Router# show ephone-dn 1 | include (class|translation)
! Aucune restriction visible

Router# show ephone-dn 2 | include (class|translation)
translation-profile outgoing allow-external
! → Bob a un translation-profile, pas Alice !

! 4. SOLUTION : Appliquer le même profil à Alice

Router# show running-config | section voice translation
voice translation-profile allow-external
 translate called 1

voice translation-rule 1
 rule 1 permit /^0/

! 5. Appliquer à Alice
Router(config)# ephone-dn 1
Router(config-ephone-dn)# translation-profile outgoing allow-external
Router(config-ephone-dn)# exit

! 6. TEST
! Alice compose 0123456789 → ✅ Fonctionne !

TEMPS DE RÉSOLUTION : 5 minutes
```

---

## 📚 Ressources

### Documentation officielle

- [Cisco CME Troubleshooting Guide](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cucme/troubleshooting/guide/cme_trbl.html)
- [Cisco IOS Debug Commands](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-110/13730-debug.html)
- [Wireshark VoIP Analysis](https://wiki.wireshark.org/VoIP_calls)

### Formations recommandées

- **Cisco CCNA Voice** (obsolète mais excellente base)
- **Cisco CCNP Collaboration**
- **Wireshark VoIP Analysis** (cours en ligne)

### Livres

- "Troubleshooting IP Routing Protocols" (Cisco Press)
- "Packet Guide to Voice over IP" (O'Reilly)

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, incidents rencontrés et solutions trouvées)*

**Incidents que j'ai résolus :**
-
-
-

**Astuces perso :**
-
-

**Contacts utiles :**
- Support Cisco :
- Opérateur télécom :
- Collègue expert :

---

## ✅ Checklist de révision

Avant de terminer le module VoIP, assurez-vous de maîtriser :

- [ ] J'applique systématiquement la méthodologie en 7 étapes
- [ ] Je sais qualifier un problème (Qui/Quoi/Quand/Où/Comment)
- [ ] Je peux identifier la couche OSI défaillante
- [ ] Je connais les 5 problèmes VoIP les plus courants
- [ ] Je sais utiliser Wireshark pour analyser SIP/RTP
- [ ] Je maîtrise les commandes Cisco de diagnostic
- [ ] Je peux résoudre un problème d'enregistrement
- [ ] Je peux diagnostiquer un problème de qualité vocale
- [ ] J'ai ma boîte à outils technicien (logiciels + matériel)
- [ ] Je documente tous mes incidents pour capitaliser

---

<div align="center">

**Cours précédent :** [05-securite-voip.md](05-securite-voip.md)

**FIN DU MODULE VOIP**

[⬅️ Retour au sommaire](README.md)

---

## 🎓 Félicitations !

Vous avez terminé le module **Téléphonie VoIP** complet !

**Ce que vous maîtrisez maintenant :**
- ✅ Fondamentaux de la VoIP
- ✅ Protocoles SIP, RTP, SCCP
- ✅ Configuration Cisco CME
- ✅ QoS et VLANs voix
- ✅ Sécurité VoIP
- ✅ Dépannage avancé

**Prochaines étapes :**
1. Pratiquer, pratiquer, pratiquer ! (labs Packet Tracer)
2. Mettre en place un projet perso (CME à la maison)
3. Passer la certification **CCNP Collaboration** (si objectif)
4. Contribuer à la communauté (forums, blogs)

**Bon courage dans votre carrière de TSSR ! 🚀**

</div>
