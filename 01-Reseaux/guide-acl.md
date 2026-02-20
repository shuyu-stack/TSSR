# Guide des ACL (Access Control Lists)

> Guide pour Packet Tracer - Formation TSSR

---

## Sommaire

1. [C'est quoi une ACL ?](#1-cest-quoi-une-acl)
2. [Types d'ACL](#2-types-dacl)
3. [ACL Standard](#3-acl-standard)
4. [ACL Étendue](#4-acl-étendue)
5. [ACL Nommées](#5-acl-nommées)
6. [Où appliquer une ACL ?](#6-où-appliquer-une-acl)
7. [Vérification et dépannage](#7-vérification-et-dépannage)
8. [Exemples pratiques](#8-exemples-pratiques)

---

## 1. C'est quoi une ACL ?

### Analogie simple

> Imagine un **videur de boîte de nuit**. Il a une liste de règles :
> - "Les VIP peuvent entrer" ✅
> - "Les moins de 18 ans ne peuvent pas entrer" ❌
> - "Tous les autres... ça dépend de la politique par défaut"
>
> Une **ACL**, c'est ce videur, mais pour les paquets réseau.

### Définition

Une **ACL (Access Control List)** est une liste de règles qui **autorise** ou **bloque** le trafic réseau selon certains critères :
- Adresse IP source
- Adresse IP destination
- Port (HTTP, SSH, etc.)
- Protocole (TCP, UDP, ICMP)

### À quoi ça sert ?

1. **Sécurité** : Bloquer le trafic non autorisé
2. **Filtrage** : Contrôler qui accède à quoi
3. **NAT** : Définir quelles IP peuvent être "natées"
4. **QoS** : Identifier le trafic prioritaire
5. **VPN** : Définir le trafic intéressant

### Fonctionnement

```
        Paquet arrive
             │
             ▼
    ┌─────────────────┐
    │ Règle 1 match ? │──► OUI ──► Action (permit/deny)
    └────────┬────────┘
             │ NON
             ▼
    ┌─────────────────┐
    │ Règle 2 match ? │──► OUI ──► Action (permit/deny)
    └────────┬────────┘
             │ NON
             ▼
    ┌─────────────────┐
    │ Règle 3 match ? │──► OUI ──► Action (permit/deny)
    └────────┬────────┘
             │ NON
             ▼
    ┌─────────────────┐
    │  DENY IMPLICITE │ ← Tout ce qui n'est pas autorisé est bloqué !
    │   (invisible)   │
    └─────────────────┘
```

> **IMPORTANT :** À la fin de chaque ACL, il y a un **deny all** implicite. Si aucune règle ne matche, le paquet est **bloqué**.

---

## 2. Types d'ACL

### Vue d'ensemble

| Type | Numéro | Critères de filtrage | Usage |
|------|--------|---------------------|-------|
| **Standard** | 1-99, 1300-1999 | IP source uniquement | Filtrage simple |
| **Étendue** | 100-199, 2000-2699 | Source, dest, port, protocole | Filtrage précis |
| **Nommée** | Nom texte | Standard ou étendue | Plus lisible |

### Quand utiliser quoi ?

```
ACL Standard :
"Je veux bloquer tout le trafic venant du réseau 192.168.1.0"
→ Je ne me soucie pas de la destination ni du port

ACL Étendue :
"Je veux bloquer l'accès SSH (port 22) depuis 192.168.1.0 vers le serveur 10.0.0.100"
→ J'ai besoin de préciser source, destination ET port
```

---

## 3. ACL Standard

### Caractéristiques

- Filtre uniquement sur l'**adresse IP source**
- Numéros : **1-99** ou **1300-1999**
- À placer **près de la destination** (car peu précise)

### Syntaxe

```cisco
access-list [numéro] [permit|deny] [source] [wildcard]
```

| Paramètre | Description | Exemple |
|-----------|-------------|---------|
| numéro | 1-99 ou 1300-1999 | `10` |
| permit/deny | Autoriser ou bloquer | `deny` |
| source | IP ou réseau source | `192.168.1.0` |
| wildcard | Masque inversé | `0.0.0.255` |

### Mots-clés spéciaux

| Mot-clé | Équivalent | Signification |
|---------|------------|---------------|
| `any` | `0.0.0.0 255.255.255.255` | N'importe quelle IP |
| `host X.X.X.X` | `X.X.X.X 0.0.0.0` | Une IP exacte |

### Exemples de syntaxe

```cisco
! Bloquer une IP spécifique
access-list 10 deny host 192.168.1.100

! Bloquer un réseau entier
access-list 10 deny 192.168.1.0 0.0.0.255

! Autoriser tout le reste (IMPORTANT sinon deny implicite !)
access-list 10 permit any
```

### Application sur une interface

```cisco
! Appliquer l'ACL sur une interface
R1(config)# interface g0/0
R1(config-if)# ip access-group 10 in    ! Trafic ENTRANT
! ou
R1(config-if)# ip access-group 10 out   ! Trafic SORTANT
```

### Exemple complet : Bloquer un réseau

**Scénario :** Bloquer le réseau 192.168.2.0/24 d'accéder au réseau 192.168.1.0/24

```
    192.168.1.0/24          192.168.2.0/24
         │                       │
    ┌────┴────┐             ┌────┴────┐
    │   PC1   │             │  PC2    │ ← À bloquer
    └────┬────┘             └────┬────┘
         │                       │
    ┌────┴────┐             ┌────┴────┐
    │  g0/0   │─────────────│  g0/1   │
    │   R1    │             │         │
    └─────────┘             └─────────┘
```

```cisco
en
conf t

! Créer l'ACL
access-list 10 deny 192.168.2.0 0.0.0.255
access-list 10 permit any

! Appliquer près de la destination (g0/0 sortie vers 192.168.1.0)
int g0/0
 ip access-group 10 out

end
wr
```

---

## 4. ACL Étendue

### Caractéristiques

- Filtre sur : **source, destination, protocole, port**
- Numéros : **100-199** ou **2000-2699**
- À placer **près de la source** (pour bloquer le trafic au plus tôt)

### Syntaxe

```cisco
access-list [num] [permit|deny] [protocole] [source] [wildcard] [dest] [wildcard] [eq port]
```

| Paramètre | Description | Exemple |
|-----------|-------------|---------|
| protocole | ip, tcp, udp, icmp | `tcp` |
| source | IP/réseau source | `192.168.1.0` |
| dest | IP/réseau destination | `10.0.0.100` |
| eq port | Port spécifique | `eq 22` (SSH) |

### Protocoles et ports courants

| Service | Protocole | Port |
|---------|-----------|------|
| HTTP | TCP | 80 |
| HTTPS | TCP | 443 |
| SSH | TCP | 22 |
| Telnet | TCP | 23 |
| FTP | TCP | 20, 21 |
| DNS | UDP/TCP | 53 |
| DHCP | UDP | 67, 68 |
| SMTP | TCP | 25 |
| Ping (ICMP) | ICMP | - |

### Opérateurs de port

| Opérateur | Signification | Exemple |
|-----------|---------------|---------|
| `eq` | Égal à | `eq 80` |
| `neq` | Différent de | `neq 22` |
| `lt` | Inférieur à | `lt 1024` |
| `gt` | Supérieur à | `gt 1023` |
| `range` | Entre X et Y | `range 20 21` |

### Exemples de syntaxe

```cisco
! Bloquer SSH (port 22) depuis 192.168.1.0 vers le serveur 10.0.0.100
access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 22

! Autoriser HTTP/HTTPS vers n'importe où
access-list 100 permit tcp any any eq 80
access-list 100 permit tcp any any eq 443

! Bloquer le ping (ICMP) depuis une IP spécifique
access-list 100 deny icmp host 192.168.1.50 any

! Autoriser tout le reste
access-list 100 permit ip any any
```

### Exemple complet : Sécuriser un serveur

**Scénario :**
- Autoriser HTTP (80) et HTTPS (443) vers le serveur
- Autoriser SSH (22) seulement depuis le réseau admin (192.168.10.0/24)
- Bloquer tout le reste vers le serveur

```
    [Admins]               [Serveur Web]           [Utilisateurs]
  192.168.10.0/24           10.0.0.100            192.168.1.0/24
        │                       │                       │
        └───────────────────────┼───────────────────────┘
                                │
                           ┌────┴────┐
                           │   R1    │
                           └─────────┘
```

```cisco
en
conf t

! ACL étendue pour protéger le serveur
access-list 100 permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.100 eq 22
access-list 100 permit tcp any host 10.0.0.100 eq 80
access-list 100 permit tcp any host 10.0.0.100 eq 443
access-list 100 deny ip any host 10.0.0.100
access-list 100 permit ip any any

! Appliquer près de la source (interface vers les utilisateurs)
int g0/1
 ip access-group 100 in

end
wr
```

---

## 5. ACL Nommées

### Pourquoi ?

- Plus **lisibles** qu'un numéro
- Possibilité de **modifier** des lignes (ajouter/supprimer)
- Identiques aux ACL numérotées en fonctionnement

### Syntaxe ACL Nommée Standard

```cisco
R1(config)# ip access-list standard NOM_ACL
R1(config-std-nacl)# deny host 192.168.1.100
R1(config-std-nacl)# permit any
R1(config-std-nacl)# exit
```

### Syntaxe ACL Nommée Étendue

```cisco
R1(config)# ip access-list extended NOM_ACL
R1(config-ext-nacl)# permit tcp any host 10.0.0.100 eq 80
R1(config-ext-nacl)# permit tcp any host 10.0.0.100 eq 443
R1(config-ext-nacl)# deny ip any any
R1(config-ext-nacl)# exit
```

### Application

```cisco
int g0/0
 ip access-group NOM_ACL in
```

### Avantage : Modifier une ACL

```cisco
! Voir les numéros de séquence
R1# sh access-list BLOCAGE-SSH
Extended IP access list BLOCAGE-SSH
    10 deny tcp any host 10.0.0.100 eq 22
    20 permit ip any any

! Supprimer une ligne
R1(config)# ip access-list extended BLOCAGE-SSH
R1(config-ext-nacl)# no 10

! Ajouter une ligne avec numéro de séquence
R1(config-ext-nacl)# 5 permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.100 eq 22
```

---

## 6. Où appliquer une ACL ?

### Règle générale

| Type ACL | Placement | Raison |
|----------|-----------|--------|
| **Standard** | Près de la **destination** | Filtre uniquement la source, donc on ne peut pas être précis avant |
| **Étendue** | Près de la **source** | Bloque le trafic indésirable au plus tôt |

### Direction : IN vs OUT

```
                    Interface g0/0
                         │
    ┌────────────────────┼────────────────────┐
    │        ROUTEUR     │                    │
    │                    │                    │
    │   IN ──────────────┼──────────────► OUT │
    │   (trafic entrant) │  (trafic sortant) │
    │                    │                    │
    └────────────────────┼────────────────────┘
                         │
```

| Direction | Signification |
|-----------|---------------|
| `in` | Trafic qui **entre** dans le routeur par cette interface |
| `out` | Trafic qui **sort** du routeur par cette interface |

### Exemple visuel

```
      [PC1]                                    [Serveur]
   192.168.1.10                               10.0.0.100
        │                                          │
        │         ┌─────────────────┐              │
        │         │       R1        │              │
        └────────►│ g0/0       g0/1 │◄─────────────┘
          IN      │                 │     IN
                  └─────────────────┘

Pour bloquer PC1 vers Serveur :
- ACL étendue sur g0/0 direction IN (près de la source)
```

### Limite : Une ACL par interface/direction

> On ne peut appliquer qu'**UNE SEULE ACL** par interface et par direction.
> - g0/0 IN : 1 ACL max
> - g0/0 OUT : 1 ACL max
> - g0/1 IN : 1 ACL max
> - etc.

---

## 7. Vérification et dépannage

### Commandes de vérification

```cisco
! Voir toutes les ACL
R1# show access-lists

! Voir une ACL spécifique
R1# show access-list 100
R1# show access-list NOM_ACL

! Voir les ACL appliquées sur les interfaces
R1# show ip interface g0/0 | include access list

! Voir la config complète
R1# show running-config | section access-list
```

### Exemple de sortie

```
R1# show access-lists
Standard IP access list 10
    10 deny   192.168.2.0, wildcard bits 0.0.0.255 (25 matches)
    20 permit any (143 matches)

Extended IP access list 100
    10 permit tcp any host 10.0.0.100 eq www (87 matches)
    20 deny ip any any (12 matches)
```

> Le compteur `(X matches)` indique combien de paquets ont correspondu à cette règle.

### Dépannage

**Problème : L'ACL bloque tout**

1. As-tu oublié le `permit any` à la fin ?
   ```cisco
   ! Le deny implicite bloque tout !
   access-list 10 deny 192.168.1.0 0.0.0.255
   access-list 10 permit any    ← OUBLIÉ !
   ```

2. L'ordre des règles est-il correct ?
   ```cisco
   ! MAUVAIS : Le permit any annule le deny
   access-list 10 permit any
   access-list 10 deny host 192.168.1.100   ← Jamais atteint !

   ! BON : Deny d'abord, puis permit
   access-list 10 deny host 192.168.1.100
   access-list 10 permit any
   ```

**Problème : L'ACL ne bloque rien**

1. Est-elle appliquée sur la bonne interface ?
   ```cisco
   R1# sh ip int g0/0 | include access
   ```

2. Est-elle dans la bonne direction (in/out) ?

3. Le wildcard mask est-il correct ?
   ```cisco
   ! MAUVAIS : bloque seulement 192.168.1.0 exactement
   access-list 10 deny 192.168.1.0 0.0.0.0

   ! BON : bloque tout le réseau 192.168.1.0/24
   access-list 10 deny 192.168.1.0 0.0.0.255
   ```

### Supprimer une ACL

```cisco
! Supprimer une ACL numérotée
R1(config)# no access-list 10

! Retirer une ACL d'une interface
R1(config)# int g0/0
R1(config-if)# no ip access-group 10 in
```

---

## 8. Exemples pratiques

### Exemple 1 : Bloquer un PC spécifique (ACL Standard)

**Scénario :** Bloquer le PC 192.168.1.100 d'accéder au réseau 10.0.0.0/24

```cisco
en
conf t

! ACL standard
access-list 1 deny host 192.168.1.100
access-list 1 permit any

! Appliquer près de la destination
int g0/1
 ip access-group 1 out

end
wr
```

### Exemple 2 : Autoriser seulement le web (ACL Étendue)

**Scénario :** Les utilisateurs ne peuvent accéder qu'à HTTP et HTTPS, rien d'autre.

```cisco
en
conf t

! ACL étendue
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
access-list 100 permit udp 192.168.1.0 0.0.0.255 any eq 53   ! DNS
access-list 100 deny ip any any

! Appliquer près de la source
int g0/0
 ip access-group 100 in

end
wr
```

### Exemple 3 : Protection serveur (ACL Nommée)

**Scénario :**
- Autoriser SSH depuis le réseau admin (192.168.10.0/24)
- Autoriser HTTP/HTTPS depuis tout le monde
- Bloquer tout le reste

```cisco
en
conf t

ip access-list extended PROTECT-SERVEUR
 permit tcp 192.168.10.0 0.0.0.255 host 10.0.0.100 eq 22
 permit tcp any host 10.0.0.100 eq 80
 permit tcp any host 10.0.0.100 eq 443
 deny ip any host 10.0.0.100 log
 permit ip any any
 exit

int g0/0
 ip access-group PROTECT-SERVEUR in

end
wr
```

### Exemple 4 : Bloquer le ping (ICMP)

```cisco
en
conf t

access-list 101 deny icmp any any
access-list 101 permit ip any any

int g0/0
 ip access-group 101 in

end
wr
```

### Exemple 5 : ACL pour NAT

```cisco
! ACL utilisée pour définir qui peut être "naté"
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 permit 192.168.2.0 0.0.0.255

! Utilisation avec NAT
ip nat inside source list 1 interface g0/0 overload
```

---

## Tableau récapitulatif

### Types d'ACL

| Type | Numéros | Filtre sur | Placement |
|------|---------|------------|-----------|
| Standard | 1-99, 1300-1999 | Source | Près destination |
| Étendue | 100-199, 2000-2699 | Tout | Près source |
| Nommée | Texte | Tout | Selon type |

### Commandes principales

| Action | Commande |
|--------|----------|
| Créer ACL standard | `access-list [1-99] permit/deny [source] [wildcard]` |
| Créer ACL étendue | `access-list [100-199] permit/deny [proto] [src] [dst] [port]` |
| Créer ACL nommée | `ip access-list [standard/extended] [NOM]` |
| Appliquer ACL | `ip access-group [num/nom] [in/out]` |
| Voir ACL | `show access-lists` |
| Supprimer ACL | `no access-list [num]` |

---

## Checklist configuration ACL

```
□ Identifier ce qu'on veut bloquer/autoriser
□ Choisir le type d'ACL (standard ou étendue)
□ Écrire les règles dans le BON ORDRE
□ Ne pas oublier "permit any" à la fin (si nécessaire)
□ Appliquer sur la bonne interface
□ Appliquer dans la bonne direction (in/out)
□ Tester avec ping / accès web / etc.
□ Vérifier les compteurs (show access-lists)
□ Sauvegarder (wr)
```

---

## Erreurs courantes

| Erreur | Conséquence | Solution |
|--------|-------------|----------|
| Oublier `permit any` | Tout est bloqué | Ajouter à la fin |
| Mauvais ordre des règles | Règles ignorées | Spécifique avant général |
| Mauvaise direction | ACL inefficace | Vérifier in/out |
| Mauvaise interface | ACL inefficace | Vérifier placement |
| Wildcard incorrect | Mauvais filtrage | 255 - masque = wildcard |

---

**Version 1.0 - Janvier 2025**
**Formation TSSR - Nextformation**
