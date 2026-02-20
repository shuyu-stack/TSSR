# 📋 CORRECTION — Questionnaire Professionnel TSSR
### Par un Architecte Réseau & Admin Systèmes Senior (20 ans d'expérience)
> *"Dans ce métier, comprendre POURQUOI est aussi important que savoir QUOI faire. Je vais vous donner les réponses, mais surtout les mécanismes derrière."*

---

## 🖥️ PARTIE 1 — Mettre en service un équipement numérique

---

### 1.1 — Que permet un outil centralisé de gestion d'équipements mobiles (MDM) ? Donnez des exemples.

#### ✅ Réponse attendue

Un outil **MDM (Mobile Device Management)** permet à l'administrateur de gérer à distance l'ensemble des équipements mobiles d'une entreprise (smartphones, tablettes, laptops) depuis une console centralisée.

Il permet notamment de :
- **Déployer des applications** à distance sur les appareils
- **Appliquer des politiques de sécurité** (chiffrement, verrouillage par code PIN, longueur minimale de mot de passe)
- **Effacer à distance** les données d'un appareil perdu ou volé (**wipe**)
- **Inventorier** les équipements (modèle, OS, version, applications installées)
- **Restreindre les fonctionnalités** (bloquer la caméra, le Bluetooth, les stores non autorisés)
- **Gérer les certificats** pour l'accès Wi-Fi ou VPN d'entreprise

**Exemples d'outils MDM :**
- **Microsoft Intune** (intégré à Azure / Microsoft 365)
- **VMware Workspace ONE** (anciennement AirWatch)
- **Jamf Pro** (spécialisé Apple/macOS/iOS)
- **MobileIron**

#### 💡 Astuce du prof
> Pensez au MDM comme à un "Active Directory pour mobiles". Vous avez une GPO pour les PC du domaine ? Le MDM c'est l'équivalent pour les smartphones. Si un commercial perd son téléphone dans le RER, vous pouvez effacer toutes les données en 2 clics depuis votre console. C'est ça la valeur ajoutée.

---

### 1.2 — Que recommandez-vous pour sécuriser les accès par mot de passe en entreprise ? Expliquez la politique à mettre en place.

#### ✅ Réponse attendue

La politique de sécurité des mots de passe doit couvrir plusieurs axes :

**Complexité :**
- Longueur minimale de **12 caractères** (14 recommandé)
- Combinaison obligatoire : majuscules, minuscules, chiffres, caractères spéciaux
- Interdiction des mots du dictionnaire et des informations personnelles (prénom, date de naissance)

**Renouvellement :**
- Expiration du mot de passe tous les **90 jours** maximum
- Historique conservé : interdire la réutilisation des **12 derniers mots de passe**

**Verrouillage de compte :**
- Verrouillage après **3 à 5 tentatives** échouées
- Durée de verrouillage : **30 minutes** ou déverrouillage manuel par l'admin

**Authentification renforcée :**
- Mise en place du **MFA (Multi-Factor Authentication)** : mot de passe + code OTP (Google Authenticator, Microsoft Authenticator)
- Utilisation d'un **gestionnaire de mots de passe** d'entreprise (Bitwarden, KeePass, CyberArk)

**En entreprise, on applique ces règles via :**
- **GPO (Group Policy Object)** dans Active Directory pour les postes Windows
- **Fine-Grained Password Policy** pour des règles différenciées par groupe

#### 💡 Astuce du prof
> La règle des 90 jours est en train d'évoluer. Le **NIST** américain recommande désormais de ne plus forcer le changement régulier SI le mot de passe est long et unique. Mais en France, l'**ANSSI** maintient des recommandations classiques. En exam, restez sur la politique classique. En entreprise, coupler un bon gestionnaire de mots de passe avec du MFA est LA vraie solution.

---

### Quels avantages voyez-vous à l'utilisation d'un outil de gestion de parc ?

#### ✅ Réponse attendue

Un outil de **gestion de parc informatique** (ex: **GLPI**, OCS Inventory, Lansweeper) apporte :

- **Inventaire automatisé** : connaissance en temps réel de tout le matériel et logiciels présents
- **Suivi du cycle de vie** : date d'achat, garantie, date de renouvellement prévu
- **Gestion des licences logicielles** : évite la sous-licence (risque légal) et la sur-licence (gaspillage d'argent)
- **Traçabilité** : savoir quel utilisateur a quel équipement
- **Optimisation des coûts** : identifier les machines obsolètes à remplacer
- **Lien avec la gestion d'incidents** : un ticket GLPI peut être associé directement à un équipement

#### 💡 Astuce du prof
> Vous avez travaillé sur GLPI en formation. Retenez que sans gestion de parc, vous naviguez à l'aveugle. Imaginez devoir patcher une faille de sécurité critique sur tous les Windows 10 de l'entreprise. Sans inventaire, vous ne savez même pas combien vous en avez. Le parc, c'est la base de tout.

---

## 🚨 PARTIE 2 — Gérer les incidents et les problèmes

---

### 2.1 — Quels avantages apporte l'utilisation d'un outil de gestion d'incidents ?

#### ✅ Réponse attendue

Un outil de ticketing (ex: **GLPI**, ServiceNow, Jira Service Management, Freshdesk) apporte :

- **Traçabilité complète** : chaque incident est enregistré, daté, suivi jusqu'à résolution
- **Priorisation** : les incidents critiques (serveur down) sont traités avant les incidents mineurs
- **Mesure de la qualité de service** : calcul du **temps de résolution moyen**, respect des **SLA** (Service Level Agreement)
- **Base de connaissances** : les solutions documentées évitent de réinventer la roue
- **Reporting** : statistiques pour identifier les équipements ou services qui génèrent le plus d'incidents
- **Communication** : l'utilisateur est informé de l'avancement de son ticket

#### 💡 Astuce du prof
> Sans outil de ticketing, c'est le chaos : les demandes arrivent par mail, téléphone, en passant dans le couloir... Certaines se perdent, les utilisateurs se plaignent, et vous n'avez aucune visibilité sur votre charge de travail. Le ticket, c'est votre protection légale et votre preuve de travail.

---

### 2.2 — Différenciez un incident d'un problème d'un point de vue ITIL

#### ✅ Réponse attendue

Selon le référentiel **ITIL (Information Technology Infrastructure Library)** :

| | **Incident** | **Problème** |
|---|---|---|
| **Définition** | Interruption non planifiée ou dégradation d'un service | Cause racine d'un ou plusieurs incidents |
| **Objectif** | Rétablir le service le plus vite possible | Identifier et éliminer la cause pour éviter la récurrence |
| **Urgence** | Immédiate | Peut être traité sur le moyen terme |
| **Exemple** | "L'imprimante du 3ème étage ne répond plus" | "Pourquoi cette imprimante tombe en panne chaque mois ?" |

**En résumé :**
- **Incident** = symptôme → on traite l'urgence (on redémarre le service)
- **Problème** = cause racine → on cherche pourquoi ça arrive (on analyse les logs, le matériel, la configuration)

#### 💡 Astuce du prof
> Analogie médicale : si un patient a de la fièvre, vous lui donnez du Doliprane → c'est gérer l'**incident**. Si vous cherchez POURQUOI il a de la fièvre (infection, virus, etc.) → c'est gérer le **problème**. ITIL fait exactement la même distinction. Un même problème peut générer des dizaines d'incidents.

---

## 🤝 PARTIE 3 — Assister à l'utilisation des ressources collaboratives

---

### 3.1 — Donnez un exemple d'outil collaboratif synchrone et un exemple d'outil collaboratif asynchrone.

#### ✅ Réponse attendue

**Outil synchrone** (communication en temps réel, les participants doivent être connectés simultanément) :
- **Microsoft Teams** (visioconférence, chat en direct)
- **Zoom**, Google Meet, Cisco Webex

**Outil asynchrone** (la communication ne nécessite pas que les participants soient connectés au même moment) :
- **Email / Messagerie** (Outlook, Gmail)
- **Confluence** (wiki collaboratif), SharePoint
- **Slack** (peut être utilisé en asynchrone)

#### 💡 Astuce du prof
> **Syn**chrone = **en même temps** (pensez "synchroniser vos montres"). **A**synchrone = **pas en même temps**. En entreprise, les deux sont complémentaires. Une réunion Teams pour décider, un email pour confirmer par écrit.

---

### 3.2 — Pourquoi écrit-on $H$1 et pas H1 dans une formule Excel ?

#### ✅ Réponse attendue

Le symbole **$** dans Excel permet de **figer une référence de cellule** lors de la recopie d'une formule.

- **H1** : référence **relative** → si on copie la formule vers le bas ou la droite, la référence change automatiquement (H2, H3, I1...)
- **$H$1** : référence **absolue** → quelle que soit la direction de recopie, la formule pointe toujours vers la cellule H1

Dans l'exemple du questionnaire, H1 contient le **taux de TVA (20%)**. Si on utilise H1 sans $, en copiant la formule vers le bas pour calculer la TVA des lignes suivantes, Excel chercherait H2, H3... (qui sont vides). Avec **$H$1**, toutes les formules font référence au même taux de TVA.

**Variantes possibles :**
- `$H1` : colonne H figée, ligne relative
- `H$1` : ligne 1 figée, colonne relative
- `$H$1` : colonne ET ligne figées (référence totalement absolue)

#### 💡 Astuce du prof
> Le raccourci clavier pour faire apparaître/retirer le $ est **F4** en éditant une cellule. Retenez l'image : $ = "clou" qui fixe la référence. Sans clou, la référence glisse quand vous tirez la formule.

---

### 3.3 — Listez les différentes étapes à respecter dans une communication téléphonique de résolution d'incident.

#### ✅ Réponse attendue

1. **Décrocher rapidement** (objectif : avant la 3ème sonnerie)
2. **Se présenter** : nom, prénom, service ("Support informatique, bonjour, je suis [Prénom], que puis-je faire pour vous ?")
3. **Identifier l'appelant** : nom, service, numéro de poste ou matricule
4. **Écouter et reformuler** le problème pour valider la compréhension
5. **Qualifier l'incident** : urgence, impact (un utilisateur ou toute une équipe ?), depuis quand ?
6. **Créer un ticket** dans l'outil de gestion d'incidents
7. **Diagnostiquer et résoudre** à distance si possible, sinon planifier une intervention
8. **Confirmer la résolution** avec l'utilisateur ("Est-ce que ça fonctionne maintenant ?")
9. **Clôturer le ticket** avec la solution documentée
10. **Conclure l'appel** poliment

#### 💡 Astuce du prof
> Ne jamais dire "je ne sais pas" et raccrocher. Si vous ne pouvez pas résoudre, dites : "Je n'ai pas la réponse immédiatement, je crée un ticket et je vous rappelle avant [heure]." L'utilisateur a besoin de savoir qu'il ne sera pas oublié.

---

## 🌐 PARTIE 4 — Maintenir et exploiter le réseau local et la téléphonie

---

### 4.1 — Découpage du réseau 192.168.20.128/25 en 4 sous-réseaux

#### ✅ Réponse attendue

**Analyse du réseau de départ :**
- Réseau : `192.168.20.128/25`
- /25 = 128 adresses (126 hôtes utilisables + réseau + broadcast)
- Pour découper en **4 sous-réseaux**, on a besoin de **2 bits supplémentaires** (2² = 4)
- Nouveau masque : /25 + 2 = **/27** → masque `255.255.255.224`
- Chaque sous-réseau contient **32 adresses** (30 hôtes utilisables)

**Tableau des 2 premiers sous-réseaux :**

| | **Sous-réseau 1** | **Sous-réseau 2** |
|---|---|---|
| **Adresse réseau** | 192.168.20.128 | 192.168.20.160 |
| **Masque** | 255.255.255.224 (/27) | 255.255.255.224 (/27) |
| **1ère adresse hôte** | 192.168.20.129 | 192.168.20.161 |
| **Dernière adresse hôte** | 192.168.20.158 | 192.168.20.190 |
| **Broadcast** | 192.168.20.159 | 192.168.20.191 |

#### 💡 Astuce du prof
> **Méthode rapide pour le subnetting :**
> 1. Vous avez un /25 à découper en 4 → 4 = 2² → vous volez 2 bits → nouveau préfixe = /27
> 2. Taille d'un bloc /27 = 2^(32-27) = 2^5 = **32 adresses**
> 3. Les sous-réseaux s'enchaînent par blocs de 32 : .128, .160, .192, .224
> 4. Broadcast = adresse suivante - 1. Donc pour .128 : prochain bloc = .160, broadcast = .159
>
> **Truc mnémotechnique :** "**N**ombre de sous-réseaux = 2^bits volés, **T**aille du bloc = 256 - valeur du masque dans l'octet intéressant"

---

### 4.2 — Quelles sont les actions possibles pour sécuriser un réseau sans fil ?

#### ✅ Réponse attendue

- **Chiffrement WPA3** (ou WPA2-Enterprise minimum) — bannir WEP et WPA1 totalement obsolètes
- **Authentification 802.1X** avec un serveur RADIUS pour les réseaux d'entreprise (chaque utilisateur a ses propres identifiants)
- **Masquage du SSID** (l'identifiant du réseau n'est pas diffusé publiquement)
- **Filtrage par adresse MAC** (liste blanche des équipements autorisés)
- **Segmentation réseau** : Wi-Fi invités isolé sur un VLAN séparé, sans accès au LAN interne
- **Désactivation du WPS** (Wi-Fi Protected Setup — vulnérable aux attaques par force brute)
- **Positionnement des bornes** pour limiter la portée du signal hors des locaux
- **Mise à jour du firmware** des points d'accès
- **Détection des points d'accès pirates** (Rogue AP detection)

#### 💡 Astuce du prof
> Le filtrage MAC semble séduisant mais c'est de la sécurité par obscurité : une adresse MAC se spoofie en 30 secondes. C'est un complément, jamais une solution principale. En entreprise, le couple **WPA2/WPA3-Enterprise + RADIUS + VLAN invité** est le standard minimum.

---

### 4.3 — Sur quels ports du switch peut-on brancher ce téléphone IP ?

#### ✅ Réponse attendue

Le switch présenté est un **D-Link DGS-1008P**. Le téléphone IP affiché s'alimente via le réseau grâce à la technologie **PoE (Power over Ethernet)**.

Il faut brancher le téléphone sur les **ports PoE** du switch, identifiés par le logo PoE au-dessus des ports. Sur ce modèle, les ports PoE sont généralement les ports **1 à 4** (à vérifier sur la documentation constructeur).

Les ports **sans PoE** ne pourront pas alimenter le téléphone — il faudrait alors utiliser un injecteur PoE externe.

#### 💡 Astuce du prof
> **PoE = Power over Ethernet** = courant électrique + données dans le même câble RJ45. Standard **IEEE 802.3af** (15,4W max) ou **IEEE 802.3at / PoE+** (30W). Un téléphone IP standard consomme ~6-8W. Un point d'accès Wi-Fi peut nécessiter du PoE+. Toujours vérifier la puissance totale disponible sur le switch (budget PoE).

---

### 4.4 — Quelles sont les routes statiques à ajouter sur Routeur1 pour permettre la communication entre PC0 et PC3 ?

#### ✅ Réponse attendue

En analysant le schéma :
- **PC0** : 192.168.1.1 / réseau 192.168.1.0/24 → connecté à Routeur1 via Switch0
- **PC3** : 192.168.2.2 / réseau 192.168.2.0/24 → connecté à Router3 via Switch1
- **Routeur1** est connecté à Router2 (172.14.1.x) et peut voir Router3 via 41.11.21.x

Routes statiques à ajouter sur **Routeur1** :

```
! Pour atteindre le réseau de PC3 (192.168.2.0/24)
! via Router3, en passant par la liaison série vers Router2 puis Router3
ip route 192.168.2.0 255.255.255.0 172.14.1.1

! Et sur Router2, route vers 192.168.2.0 via Router3
ip route 192.168.2.0 255.255.255.0 41.11.21.1
```

Il faut également des routes retour sur Router3 vers le réseau de PC0.

#### 💡 Astuce du prof
> **Méthode pour les routes statiques :** "Je suis sur Routeur1, je veux joindre le réseau de PC3. Par quelle interface dois-je sortir ? Vers quelle adresse de mon voisin immédiat dois-je envoyer le paquet ?" → C'est la **gateway** de la route statique. La commande Cisco : `ip route [réseau_destination] [masque] [next-hop ou interface]`

---

## 🔒 PARTIE 5 — Sécuriser les accès à Internet

---

### 5.1 — Quel est le rôle de l'ACL 101 ?

#### ✅ Réponse attendue

```
access-list 101 deny tcp host 180.0.0.30 host 220.0.0.60 eq www
access-list 101 deny tcp host 180.0.0.30 host 220.0.0.60 eq 443
access-list 101 permit ip any any
```

L'**ACL 101** bloque tout le trafic HTTP (port 80) et HTTPS (port 443) provenant de l'hôte **180.0.0.30** à destination de l'hôte **220.0.0.60**.

En clair : **l'hôte 180.0.0.30 ne peut pas accéder au site web de 220.0.0.60** (ni en HTTP ni en HTTPS).

La dernière ligne `permit ip any any` autorise tout le reste du trafic → l'ACL est permissive par défaut sauf pour cette restriction précise.

**Comparaison ACL 100 :**
```
access-list 100 deny icmp host 110.0.0.10 180.0.0.0 0.255.255.255
access-list 100 permit ip any any
```
L'ACL 100 bloque les **pings (ICMP)** venant de 110.0.0.10 vers tout le réseau 180.x.x.x.

#### 💡 Astuce du prof
> Sur Cisco, les ACL se lisent ligne par ligne, **de haut en bas**, et s'arrêtent à la première règle qui correspond. Il y a toujours un **"deny all" implicite** à la fin — mais ici il est contrebalancé par le `permit ip any any` explicite. Pensez à lire une ACL comme un videur de boîte de nuit : il regarde sa liste dans l'ordre et prend la première décision qui correspond.

---

### 5.2 — Que faut-il faire pour accéder de façon sécurisée au serveur HTTPS depuis Internet ?

#### ✅ Réponse attendue

En analysant le schéma (Internet → Pare-feu → DMZ avec serveur WWW) :

1. **Configurer une règle NAT** sur le pare-feu : translater l'adresse publique (65.43.18.1) vers l'adresse privée du serveur en DMZ (192.168.100.x) — c'est du **DNAT / Port Forwarding**
2. **Ouvrir le port 443 (HTTPS)** dans le pare-feu, uniquement en entrée depuis Internet vers le serveur DMZ
3. **Installer un certificat SSL/TLS valide** sur le serveur (Let's Encrypt, ou certificat signé par une CA de confiance)
4. **Laisser le serveur en DMZ** (zone démilitarisée) : il ne doit jamais avoir accès direct au LAN interne
5. Fermer tous les autres ports inutiles

#### 💡 Astuce du prof
> La **DMZ** est une zone tampon entre Internet et votre LAN. Si le serveur web est compromis, l'attaquant est bloqué en DMZ et ne peut pas rebondir sur votre réseau interne. C'est le principe de **défense en profondeur**. Ne jamais mettre un serveur accessible depuis Internet directement dans le LAN !

---

### 5.3 — Pourquoi mettre à jour le firmware d'un équipement réseau ?

#### ✅ Réponse attendue

- **Corriger des failles de sécurité** : les vulnérabilités découvertes dans le firmware sont exploitables par des attaquants pour prendre le contrôle de l'équipement
- **Corriger des bugs** : stabilité améliorée, résolution de dysfonctionnements
- **Ajouter de nouvelles fonctionnalités** ou améliorer les performances
- **Maintenir la compatibilité** avec les nouveaux protocoles et standards
- **Rester conforme** aux exigences de sécurité de l'entreprise et aux réglementations (ISO 27001, etc.)

#### 💡 Astuce du prof
> En 2017, la faille **EternalBlue** dans des équipements non patchés a permis la propagation de **WannaCry** dans le monde entier. Des milliers d'équipements réseau non mis à jour ont servi de point d'entrée. Le patch existait depuis 2 mois. Moralité : le firmware obsolète est une porte ouverte. En entreprise, mettez en place une politique de patch management avec une fenêtre de maintenance mensuelle.

---

## 💻 PARTIE 6 — Maintenir et exploiter un environnement virtualisé

---

### 6.1 — Quel est l'intérêt de la mise en cluster des hyperviseurs ?

#### ✅ Réponse attendue

La **mise en cluster** consiste à relier plusieurs hyperviseurs (ex: ESXi de VMware) pour qu'ils fonctionnent ensemble comme un pool de ressources unifié.

Avantages :
- **Haute disponibilité (HA)** : si un hyperviseur tombe en panne, les VMs sont automatiquement redémarrées sur les autres nœuds du cluster
- **Répartition de charge (DRS - Distributed Resource Scheduler)** : les VMs sont migrées automatiquement vers les hôtes les moins chargés
- **Migration à chaud (vMotion)** : déplacer une VM d'un hyperviseur à un autre sans interruption de service
- **Maintenance sans interruption** : mettre un hôte en maintenance mode et déplacer ses VMs sur les autres
- **Mutualisation des ressources** : la RAM et le CPU de tous les hôtes sont vus comme un pool global

#### 💡 Astuce du prof
> Imaginez un cluster comme une équipe de serveurs solidaires. Si l'un tombe malade (panne matérielle), ses collègues reprennent son travail automatiquement. C'est ça **VMware HA**. Et **vMotion** c'est comme déménager dans un nouvel appartement sans jamais couper le téléphone.

---

### 6.2 — Citez un outil pour sauvegarder les machines virtuelles hébergées sur un hyperviseur.

#### ✅ Réponse attendue

- **Veeam Backup & Replication** ← la référence du marché, compatible VMware et Hyper-V
- **VMware vSphere Data Protection (VDP)**
- **Nakivo Backup & Replication**
- **Acronis Cyber Backup**
- **Commvault**

Ces outils permettent de sauvegarder les VMs en **mode image** (snapshot complet) sans interruption de service, avec des fonctionnalités de **déduplication**, **compression**, et **réplication**.

#### 💡 Astuce du prof
> **Veeam** est la réponse qui impressionne en entretien. Retenez aussi la règle **3-2-1** pour les sauvegardes : **3** copies des données, sur **2** supports différents, dont **1** hors site (ou dans le cloud). Et surtout : **une sauvegarde non testée n'est pas une sauvegarde**.

---

### 6.3 — Dans un cluster, un hyperviseur affiche des performances dégradées. Que feriez-vous ?

#### ✅ Réponse attendue

**Démarche méthodique :**

1. **Identifier l'hyperviseur concerné** dans la console de gestion (vCenter, Proxmox, etc.)
2. **Analyser les métriques** : CPU, RAM, I/O disque, réseau — identifier le goulot d'étranglement
3. **Vérifier les alertes et logs** de l'hyperviseur (journaux système, événements vCenter)
4. **Migrer les VMs** les plus consommatrices vers les autres nœuds du cluster (**vMotion**) pour soulager l'hôte
5. **Passer l'hôte en maintenance mode** si nécessaire pour intervention physique
6. **Diagnostiquer la cause** : disque défaillant ? Surchauffe ? Carte réseau en erreur ? RAM défectueuse ?
7. **Intervenir** : remplacement de composant défaillant, nettoyage physique, mise à jour firmware
8. **Réintégrer l'hôte** dans le cluster et vérifier le retour à la normale
9. **Documenter l'incident** et créer un ticket problème si récurrence

#### 💡 Astuce du prof
> La clé, c'est de ne pas paniquer et d'être **méthodique**. Les examinateurs veulent voir que vous savez **mitiger l'impact** d'abord (migrer les VMs) avant de chercher la cause. Un bon technicien protège la production pendant qu'il diagnostique.

---

## 🐧 PARTIE 7 — Maintenir et exploiter un serveur Linux

---

### 7.1 — Si vous ajoutez un disque dur supplémentaire avec une seule partition, comment sera-t-elle nommée ?

#### ✅ Réponse attendue

En analysant la capture, le disque existant est `/dev/sda` avec des partitions `/dev/sda1`, `/dev/sda2`, etc.

Le nouveau disque dur sera nommé **`/dev/sdb`** et sa partition unique sera **`/dev/sdb1`**.

**Logique de nommage Linux :**
- Les disques SATA/SCSI/SSD sont nommés `sda`, `sdb`, `sdc`... dans l'ordre de détection
- Les partitions ajoutent un chiffre : `sda1`, `sda2`, `sdb1`...
- Les disques NVMe suivent une autre convention : `nvme0n1`, `nvme0n1p1`...

#### 💡 Astuce du prof
> **sd** = SCSI Disk (même pour les SATA modernes). La lettre (a, b, c...) = ordre de détection par le noyau. Le chiffre = numéro de partition. Simple comme bonjour une fois qu'on a compris la logique. Pour vérifier : `lsblk` ou `fdisk -l` après branchement.

---

### 7.2 — L'utilisateur planchet ne peut plus accéder au répertoire production. Cause et résolution.

#### ✅ Réponse attendue

**Cause probable :**
Le message `Permission non accordée` indique un problème de **droits Unix** sur le répertoire `/home/planchet/production/`. L'utilisateur `planchet` n'a pas les permissions nécessaires (lecture + exécution) sur ce répertoire.

**Causes possibles :**
- Les droits du répertoire ont été modifiés accidentellement
- L'appartenance du répertoire (propriétaire ou groupe) a changé
- L'utilisateur a été retiré d'un groupe qui avait accès

**Outils de diagnostic :**
```bash
# Voir les permissions du répertoire
ls -la /home/planchet/

# Voir à quels groupes appartient l'utilisateur
id planchet
groups planchet

# Vérifier les ACL étendues si présentes
getfacl /home/planchet/production/
```

**Résolution selon le cas :**
```bash
# Si les droits sont trop restrictifs (ex: 700 au lieu de 750)
chmod 750 /home/planchet/production/

# Si le propriétaire est incorrect
chown planchet:planchet /home/planchet/production/

# Si l'utilisateur doit être ajouté à un groupe
usermod -aG nom_groupe planchet
# Puis l'utilisateur doit se reconnecter pour que le changement soit pris en compte
```

#### 💡 Astuce du prof
> Les permissions Linux se lisent avec la commande `ls -la`. Vous verrez quelque chose comme `drwxr-x---`. Découpez-le en 4 parties : `d` (répertoire), `rwx` (propriétaire), `r-x` (groupe), `---` (autres). Pour accéder à un répertoire, il faut le droit **x (exécution)**. C'est contre-intuitif mais "entrer" dans un répertoire = l'exécuter.

---

### 7.3 — Bonnes pratiques pour sécuriser la connexion SSH (d'après la config visible)

#### ✅ Réponse attendue

En analysant le fichier `/etc/ssh/sshd_config` visible dans la capture, plusieurs points sont à corriger :

**Problèmes identifiés :**
- `Port 22` → port par défaut, très ciblé par les scanneurs et brute-force
- `PermitRootLogin yes` → **CRITIQUE** : le compte root ne doit jamais se connecter directement en SSH
- `LoginGraceTime 120` → trop long (120 secondes pour s'authentifier)
- `ServerKeyBits 1024` → taille de clé trop faible (obsolète)

**Bonnes pratiques à appliquer :**
```
# Changer le port SSH (ex: 2222 ou autre port non standard)
Port 2222

# Interdire la connexion root directe
PermitRootLogin no

# Utiliser uniquement l'authentification par clé (désactiver les mots de passe)
PasswordAuthentication no
PubkeyAuthentication yes

# Réduire le temps de grâce
LoginGraceTime 30

# Limiter les utilisateurs autorisés
AllowUsers alice bob

# Augmenter la taille des clés
ServerKeyBits 4096

# Réduire le nombre de tentatives d'authentification
MaxAuthTries 3

# Désactiver les protocoles anciens (déjà Protocol 2 dans la config, bien)
Protocol 2
```

**Mesures complémentaires :**
- Mettre en place **fail2ban** pour bloquer automatiquement les IPs après X tentatives échouées
- Utiliser un **VPN** ou un **bastion SSH** pour n'exposer SSH qu'en interne

#### 💡 Astuce du prof
> `PermitRootLogin yes` dans une config SSH, c'est comme laisser la porte d'entrée de votre datacenter ouverte avec un panneau "Direction ici". Le compte root est connu de tous les scripts d'attaque. Interdisez-le et utilisez `sudo`. Changer le port 22 ne sécurise pas, mais réduit le bruit dans les logs.

---

## 🖥️ PARTIE 8 — Configurer les services de déploiement et terminaux clients légers

---

### 8.1 — Décrivez la procédure de création et déploiement d'un master Windows.

#### ✅ Réponse attendue

**Étapes de création d'un master (image de référence) :**

1. **Installer Windows** sur une machine de référence (propre, sans données utilisateur)
2. **Installer et configurer** tous les logiciels standards de l'entreprise (Office, antivirus, drivers...)
3. **Appliquer les configurations** et paramètres de sécurité de l'entreprise
4. **Exécuter Sysprep** (`C:\Windows\System32\Sysprep\sysprep.exe`) avec l'option "OOBE + Generalize" pour :
   - Supprimer le SID unique de la machine (indispensable pour éviter les conflits réseau)
   - Réinitialiser la configuration matérielle pour s'adapter à d'autres machines
5. **Capturer l'image** avec un outil de déploiement (Windows Deployment Services - WDS, MDT, SCCM/Endpoint Manager, ou Clonezilla)
6. **Stocker l'image** sur le serveur de déploiement

**Déploiement :**
1. Démarrage PXE (boot réseau) sur les postes cibles
2. Sélection de l'image dans le menu de déploiement
3. Application automatique de l'image + intégration au domaine Active Directory
4. Application des GPO au premier démarrage

#### 💡 Astuce du prof
> **Sysprep** est l'étape critique que beaucoup oublient. Sans Sysprep, toutes vos machines auraient le même SID Windows → conflits d'authentification sur le domaine, problèmes de licences. C'est comme cloner un passeport sans changer le numéro.

---

### 8.2 — Quels avantages apporte un service centralisé de mises à jour logicielles ?

#### ✅ Réponse attendue

Solution de référence en environnement Windows : **WSUS (Windows Server Update Services)** ou **SCCM/Endpoint Manager**.

Avantages :
- **Réduction de la bande passante** : les mises à jour sont téléchargées une seule fois depuis Microsoft puis distribuées en interne
- **Contrôle et validation** : l'admin teste les mises à jour avant de les déployer (éviter les mises à jour qui cassent des applications métier)
- **Planification** : déploiement pendant les heures creuses, par groupes d'ordinateurs
- **Traçabilité et conformité** : tableau de bord montrant les machines à jour / en retard
- **Sécurité** : garantit que tous les postes sont patchés contre les dernières vulnérabilités
- **Uniformité du parc** : tous les postes sont au même niveau de version

#### 💡 Astuce du prof
> Sans WSUS, chaque PC télécharge ses mises à jour directement depuis Microsoft. Avec 500 postes qui téléchargent 1 Go de mises à jour le même jour, vous saturez votre lien internet. Avec WSUS, c'est 1 téléchargement pour 500 déploiements internes. Et vous évitez qu'une mise à jour buggée casse toute la prod un lundi matin.

---

### 8.3 — Inconvénients des terminaux clients légers par rapport à des postes fixes.

#### ✅ Réponse attendue

Un **client léger (thin client)** dépend entièrement d'un serveur central (Citrix, RDS...).

**Inconvénients :**
- **Dépendance au réseau** : si le réseau ou le serveur central tombe, tous les utilisateurs sont bloqués (SPOF - Single Point of Failure)
- **Charge serveur concentrée** : le serveur doit gérer les sessions de tous les utilisateurs simultanément → dimensionnement coûteux
- **Limitations sur les applications lourdes** : graphisme 3D, montage vidéo, CAO sont difficiles à virtualiser
- **Latence** : perceptible notamment pour les applications temps réel ou les connexions distantes
- **Coût de l'infrastructure serveur** : serveurs puissants, licences Citrix ou RDS, stockage centralisé
- **Problèmes de périphériques** : redirection d'imprimantes, clés USB, lecteurs de cartes parfois complexe
- **Migration difficile** : forte dépendance à l'éditeur de la solution (vendor lock-in)

#### 💡 Astuce du prof
> La vraie force du client léger c'est la **sécurité** (aucune donnée sur le poste) et la **facilité d'administration** (tout est centralisé). Mais si votre serveur Citrix tombe, c'est toute l'entreprise qui s'arrête. Toujours prévoir une **haute disponibilité** sur l'infrastructure serveur si vous choisissez cette solution.

---

## 🔐 PARTIE 9 — Maintenir et sécuriser les accès réseaux distants

---

### 9.1 — Quels types de VPN sont représentés ?

#### ✅ Réponse attendue

- **Schéma 1** (Client isolé → Internet → Pare-feu VPN → Réseau entreprise) : **VPN d'accès à distance (Remote Access VPN)** — aussi appelé **SSL VPN** ou **VPN client-to-site**. Utilisé par les salariés en télétravail.

- **Schéma 2** (Site A ↔ Internet ↔ Site B, avec pare-feu VPN de chaque côté) : **VPN site à site (Site-to-Site VPN)** — aussi appelé **VPN LAN-to-LAN**. Connecte deux agences de l'entreprise en permanence, de manière transparente pour les utilisateurs.

#### 💡 Astuce du prof
> Retenez l'image : **Remote Access = un nomade qui se connecte** / **Site-to-site = deux bureaux reliés en permanence**. Dans les deux cas, le tunnel VPN chiffre les données qui transitent par Internet. Sans VPN, vos données circuleraient en clair.

---

### 9.2 — Complétez la description sur le chiffrement asymétrique

#### ✅ Réponse attendue

> Pour envoyer un message privé à Bob, Alice utilise la **clé publique** de Bob pour rendre « illisible » le « texte en clair » et Bob utilise sa **clé privée** pour transformer le texte « illisible » en « texte en clair ». Ce processus représente un chiffrement **asymétrique**.

**Principe du chiffrement asymétrique (ou à clé publique) :**
- Chaque personne possède une **paire de clés** : une **clé publique** (partagée avec tout le monde) et une **clé privée** (gardée secrète)
- Ce qui est chiffré avec la clé publique ne peut être déchiffré qu'avec la clé privée correspondante
- Exemples d'utilisation : **HTTPS (TLS)**, **SSH**, **GPG**, **S/MIME** pour les emails

#### 💡 Astuce du prof
> Analogie du cadenas : la **clé publique** = cadenas ouvert que vous distribuez à tout le monde. N'importe qui peut mettre un message dans une boîte et fermer votre cadenas dessus. Mais **seul vous** avez la **clé privée** pour l'ouvrir. Malin, non ? Le chiffrement **symétrique** c'est l'inverse : une seule clé partagée pour chiffrer et déchiffrer (ex: AES).

---

### 9.3 — Traduire l'extrait du guide IPSec/ISAKMP

#### ✅ Réponse attendue

**Traduction :**

> Lorsque les négociations ISAKMP commencent, le pair qui initie la négociation envoie l'ensemble de ses politiques (policies) au pair distant, et ce dernier tente de trouver une correspondance. Le pair distant compare toutes les politiques reçues avec chacune de ses propres politiques configurées, dans l'ordre de priorité (la priorité la plus haute en premier), jusqu'à trouver une correspondance.

**Explication :**
ISAKMP est la phase de négociation qui précède l'établissement d'un tunnel IPSec. Les deux équipements doivent se mettre d'accord sur les algorithmes de chiffrement, d'authentification, et d'échange de clés à utiliser. Ils comparent leurs "policies" (propositions) et sélectionnent la première qui est compatible entre les deux.

---

## 📡 PARTIE 10 — Superviser l'infrastructure

---

### 10.1 — Quel est le nom de la communauté permettant de modifier la configuration des paramètres ?

#### ✅ Réponse attendue

En analysant la configuration SNMP visible :
```
snmp-server community TSSR RO
snmp-server community ADM-SNMP RW
```

La communauté permettant de **modifier** la configuration est **`ADM-SNMP`** avec le droit **RW (Read-Write)**.

La communauté `TSSR` en **RO (Read-Only)** permet uniquement de lire les informations, pas de les modifier.

#### 💡 Astuce du prof
> **SNMP v1/v2c** utilise des "community strings" comme mot de passe en clair sur le réseau — extrêmement peu sécurisé. En production moderne, on utilise **SNMPv3** qui apporte authentification et chiffrement. Changer les noms de communauté par défaut (`public`, `private`) est le minimum vital.

---

### 10.2 — Informations intéressantes à surveiller sur un commutateur de couche accès

#### ✅ Réponse attendue

- **Charge CPU et RAM** du switch
- **État des ports** (up/down) — détecter une panne de port ou un équipement débranché
- **Utilisation de la bande passante** par port — détecter un port saturé
- **Erreurs de trame** : CRC errors, collisions, runts, giants — indicateur de câbles défectueux ou de problème de duplex
- **Spanning Tree** : état des ports STP, détecter des changements de topologie anormaux
- **Table MAC** : nombre d'adresses MAC apprises (détecter un flooding anormal)
- **Température** de l'équipement
- **Alimentation redondante** : état des alimentations PSU
- **PoE** : consommation vs budget disponible

---

### 10.3 — Analyser le tableau de bord Nagios et définir la démarche

#### ✅ Réponse attendue

En analysant la capture Nagios :

**Problème identifié :** Le service **Swap Usage** est en état **CRITICAL** sur localhost : "SWAP CRITICAL - 0% free (0 MB out of 1GB)"

**Démarche :**

1. **Confirmer l'alerte** : se connecter en SSH sur le serveur concerné
2. **Analyser l'utilisation mémoire** :
   ```bash
   free -h          # Vue globale RAM et swap
   top              # Processus consommateurs en temps réel
   htop             # Version plus lisible de top
   vmstat 1 5       # Statistiques mémoire/swap en temps réel
   ```
3. **Identifier le(s) processus** qui consomment trop de RAM (forçant l'utilisation du swap)
4. **Actions correctives selon le diagnostic :**
   - Si un processus a une fuite mémoire → le redémarrer (avec précaution)
   - Si la charge est légitime → envisager d'augmenter la RAM ou le swap
   - Si c'est un pic temporaire → surveiller l'évolution
5. **Vérifier les autres services** (HTTP, SSH, PING sont OK → le serveur est accessible)
6. **Documenter et escalader** si nécessaire

#### 💡 Astuce du prof
> Le swap à 0% libre = la RAM est pleine et le système utilise le disque dur comme mémoire d'appoint. C'est beaucoup plus lent et peut provoquer un crash si le swap se remplit aussi. C'est une urgence de niveau 2. Nagios vous alerte AVANT que ça dégénère en panne complète — c'est tout l'intérêt de la supervision.

---

## ☁️ PARTIE 11 — Intervenir dans un environnement de Cloud Computing

---

### 11.1 — Remplir les champs PaaS, IaaS, SaaS

#### ✅ Réponse attendue (de haut en bas dans le schéma)

Le schéma montre les couches gérées par le fournisseur cloud vs le client :

- **SaaS** (Software as a Service) — couche haute : Applications + Middleware gérés par le fournisseur (ex: Gmail, Office 365, Salesforce). L'utilisateur utilise juste l'application, il ne gère rien en dessous.

- **PaaS** (Platform as a Service) — couche milieu : Middleware + OS gérés par le fournisseur (ex: Heroku, Azure App Service, Google App Engine). Le développeur déploie son code sans gérer l'infrastructure.

- **IaaS** (Infrastructure as a Service) — couche basse : Virtualisation + Serveurs + Réseau + Datacenter gérés par le fournisseur (ex: AWS EC2, Azure VM, Google Compute Engine). Le client gère l'OS et tout ce qui est au-dessus.

---

### 11.2 — Différences entre IaaS et PaaS

#### ✅ Réponse attendue

| | **IaaS** | **PaaS** |
|---|---|---|
| **Ce que le fournisseur gère** | Datacenter, réseau, serveurs, virtualisation | + OS, middleware, runtime |
| **Ce que le client gère** | OS, middleware, applications, données | Applications et données uniquement |
| **Flexibilité** | Maximum — contrôle total de l'OS | Moindre — OS imposé par le fournisseur |
| **Complexité d'administration** | Élevée | Faible |
| **Pour qui** | Admins système, architectes | Développeurs |
| **Exemples** | AWS EC2, Azure VM | Heroku, Google App Engine |

**En résumé :** IaaS = vous louez des serveurs virtuels bruts. PaaS = vous déposez votre code et la plateforme s'occupe du reste.

#### 💡 Astuce du prof
> Mnémotechnique : **IaaS** = "j'ai mon **Infrastructure** dans le cloud mais je gère encore beaucoup". **PaaS** = "j'ai ma **Plateforme** de dev, je pose juste mon code". **SaaS** = "j'utilise juste le **Software**, je ne gère rien". Plus on monte dans la pile, moins on administre, moins on contrôle.

---

## 📝 Récapitulatif — Points clés à retenir pour l'examen

| Thème | Concept essentiel |
|---|---|
| MDM | Gestion centralisée des mobiles = AD pour smartphones |
| Mots de passe | Complexité + MFA + gestionnaire de mots de passe |
| Incident vs Problème | Symptôme vs Cause racine (ITIL) |
| Subnetting | Bits volés → 2^n sous-réseaux, blocs de taille 2^(32-masque) |
| ACL Cisco | Lecture séquentielle, deny implicite final |
| SSH sécurisé | No root, no password auth, changer le port, fail2ban |
| VPN | Remote access (nomade) vs Site-to-site (agences) |
| Chiffrement | Asymétrique = clé publique chiffre, clé privée déchiffre |
| Cloud | IaaS/PaaS/SaaS = niveau de responsabilité décroissant |
| SNMP | RO = lecture, RW = lecture+écriture, v3 = sécurisé |

---

*Document rédigé dans un objectif pédagogique — Formation TSSR*
*"La théorie, c'est quand on sait tout et que rien ne fonctionne. La pratique, c'est quand tout fonctionne et que personne ne sait pourquoi. En salle des serveurs, on combine les deux." — Sagesse de datacenter*
