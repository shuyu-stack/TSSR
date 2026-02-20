# Guide NAT et PAT

> Guide pour Packet Tracer - Formation TSSR

---

## Sommaire

1. [C'est quoi le NAT ?](#1-cest-quoi-le-nat)
2. [Les types de NAT](#2-les-types-de-nat)
3. [NAT Statique](#3-nat-statique)
4. [NAT Dynamique](#4-nat-dynamique)
5. [PAT (NAT Overload)](#5-pat-nat-overload)
6. [Vérification et dépannage](#6-vérification-et-dépannage)
7. [Exemples pratiques](#7-exemples-pratiques)

---

## 1. C'est quoi le NAT ?

### Le problème

> Il y a environ **4,3 milliards** d'adresses IPv4 dans le monde, mais il y a bien plus d'appareils connectés. Comment faire ?

**Solution :** On utilise des adresses **privées** (non routables sur Internet) à l'intérieur des entreprises, et le **NAT** les traduit en adresses **publiques** pour sortir sur Internet.

### Analogie simple

> Imagine un immeuble avec 100 appartements. Tous les habitants ont une adresse interne (appartement 12, 47, etc.), mais pour le facteur, il n'y a qu'une seule adresse postale : "10 rue de la Paix". Le concierge (NAT) reçoit le courrier et le redistribue au bon appartement.

```
┌─────────────────────────────────────────────────────────────┐
│                        ENTREPRISE                           │
│  ┌─────┐  ┌─────┐  ┌─────┐                                 │
│  │PC1  │  │PC2  │  │PC3  │  Adresses PRIVÉES               │
│  │.10  │  │.11  │  │.12  │  192.168.1.0/24                 │
│  └──┬──┘  └──┬──┘  └──┬──┘                                 │
│     └────────┼───────┬┘                                    │
│              │       │                                      │
│         ┌────┴───────┴────┐                                │
│         │     ROUTEUR     │ ← NAT ici                      │
│         │   (Concierge)   │                                │
│         └────────┬────────┘                                │
│                  │ 203.0.113.5 (IP PUBLIQUE)               │
└──────────────────┼──────────────────────────────────────────┘
                   │
              [INTERNET]
```

### Vocabulaire NAT

| Terme | Description | Exemple |
|-------|-------------|---------|
| **Inside Local** | IP privée du PC (avant NAT) | 192.168.1.10 |
| **Inside Global** | IP publique vue sur Internet | 203.0.113.5 |
| **Outside Local** | IP de destination vue de l'intérieur | 8.8.8.8 |
| **Outside Global** | IP de destination réelle | 8.8.8.8 |

```
     Inside                    Outside
┌─────────────────┐      ┌─────────────────┐
│  Inside Local   │      │  Outside Local  │
│  192.168.1.10   │─────►│     8.8.8.8     │
│                 │      │                 │
│  Inside Global  │      │  Outside Global │
│  203.0.113.5    │◄─────│     8.8.8.8     │
└─────────────────┘      └─────────────────┘
```

### Adresses privées (RFC 1918)

| Classe | Plage | Masque par défaut |
|--------|-------|-------------------|
| A | 10.0.0.0 - 10.255.255.255 | /8 |
| B | 172.16.0.0 - 172.31.255.255 | /12 |
| C | 192.168.0.0 - 192.168.255.255 | /16 |

> Ces adresses ne sont **jamais routées sur Internet**. Tu peux les utiliser librement dans ton réseau.

---

## 2. Les types de NAT

### Vue d'ensemble

| Type | Traduction | Cas d'usage |
|------|------------|-------------|
| **NAT Statique** | 1 privée → 1 publique (fixe) | Serveurs (web, mail) |
| **NAT Dynamique** | N privées → Pool de publiques | Plusieurs IP publiques |
| **PAT (Overload)** | N privées → 1 publique + ports | Le plus courant (entreprises) |

```
NAT Statique :
192.168.1.10 ──────► 203.0.113.10  (toujours la même)

NAT Dynamique :
192.168.1.10 ──────► 203.0.113.10 ┐
192.168.1.11 ──────► 203.0.113.11 ├── Pool d'IP publiques
192.168.1.12 ──────► 203.0.113.12 ┘

PAT (Overload) :
192.168.1.10 ──────► 203.0.113.5:10001 ┐
192.168.1.11 ──────► 203.0.113.5:10002 ├── 1 seule IP publique
192.168.1.12 ──────► 203.0.113.5:10003 ┘   + ports différents
```

---

## 3. NAT Statique

### C'est quoi ?

> **NAT Statique** = une adresse privée est TOUJOURS traduite vers la même adresse publique. C'est une correspondance permanente, comme un numéro de téléphone direct.

### Quand l'utiliser ?

- Serveur Web accessible depuis Internet
- Serveur Mail
- Tout service qui doit être joignable de l'extérieur

### Configuration

**Syntaxe :**
```cisco
ip nat inside source static [IP_privée] [IP_publique]
```

**Exemple :** Serveur web interne (192.168.1.100) accessible via IP publique (203.0.113.100)

```
         [Internet]
              │
              │ 203.0.113.100 (IP publique du serveur)
         ┌────┴────┐
         │   R1    │
         │  g0/0   │ 203.0.113.1 (inside global)
         │  g0/1   │ 192.168.1.254 (inside local)
         └────┬────┘
              │
    ┌─────────┴─────────┐
    │                   │
┌───┴───┐          ┌────┴────┐
│ SRV   │          │   PC    │
│ .100  │          │  .10    │
└───────┘          └─────────┘
```

**Configuration R1 :**
```cisco
en
conf t

! 1. Définir les interfaces Inside et Outside
int g0/1
 ip nat inside
 exit

int g0/0
 ip nat outside
 exit

! 2. Créer la traduction statique
ip nat inside source static 192.168.1.100 203.0.113.100

end
wr
```

### Commandes expliquées

| Commande | Explication |
|----------|-------------|
| `ip nat inside` | Interface côté réseau privé |
| `ip nat outside` | Interface côté Internet |
| `ip nat inside source static A B` | Traduit A (privée) vers B (publique) |

---

## 4. NAT Dynamique

### C'est quoi ?

> **NAT Dynamique** = les adresses privées sont traduites vers un **pool** d'adresses publiques. Premier arrivé, premier servi.

### Quand l'utiliser ?

- Tu as plusieurs IP publiques disponibles
- Plus d'utilisateurs que d'IP publiques, mais pas tous connectés en même temps

### Configuration

**Étapes :**
1. Créer une ACL pour définir qui peut être "naté"
2. Créer un pool d'adresses publiques
3. Associer l'ACL au pool

```cisco
en
conf t

! 1. Interfaces Inside/Outside
int g0/1
 ip nat inside
int g0/0
 ip nat outside
exit

! 2. ACL : qui peut utiliser le NAT ?
access-list 1 permit 192.168.1.0 0.0.0.255

! 3. Pool d'adresses publiques
ip nat pool POOL-PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.0

! 4. Associer ACL et Pool
ip nat inside source list 1 pool POOL-PUBLIC

end
wr
```

### Limite du NAT Dynamique

> Si le pool est épuisé (tous les utilisateurs connectés), les nouveaux ne peuvent pas sortir sur Internet → c'est pourquoi le **PAT** est préféré.

---

## 5. PAT (NAT Overload)

### C'est quoi ?

> **PAT** (Port Address Translation) ou **NAT Overload** = TOUTES les adresses privées partagent UNE SEULE adresse publique. La distinction se fait par les **numéros de port**.

C'est le type de NAT le plus utilisé (box Internet, entreprises).

### Comment ça marche ?

```
PC1 (192.168.1.10) ──► Requête vers google.com
                       Source: 192.168.1.10:50001
                             │
                             ▼ NAT
                       Source: 203.0.113.5:10001
                             │
                             ▼
                        [Internet]

PC2 (192.168.1.11) ──► Requête vers google.com
                       Source: 192.168.1.11:50002
                             │
                             ▼ NAT
                       Source: 203.0.113.5:10002  ← Même IP, port différent
                             │
                             ▼
                        [Internet]
```

Le routeur garde une **table de traduction** pour savoir à qui renvoyer les réponses.

### Configuration PAT (Méthode 1 : avec l'IP de l'interface)

> C'est la méthode la plus simple et la plus courante.

```cisco
en
conf t

! 1. Interfaces Inside/Outside
int g0/1
 ip nat inside
int g0/0
 ip nat outside
exit

! 2. ACL : qui peut sortir ?
access-list 1 permit 192.168.1.0 0.0.0.255

! 3. PAT avec l'IP de l'interface outside (overload)
ip nat inside source list 1 interface GigabitEthernet0/0 overload

end
wr
```

**Le mot clé `overload` active le PAT !**

### Configuration PAT (Méthode 2 : avec un pool)

> Si tu as un pool d'IP publiques mais que tu veux quand même utiliser le PAT.

```cisco
en
conf t

int g0/1
 ip nat inside
int g0/0
 ip nat outside
exit

access-list 1 permit 192.168.1.0 0.0.0.255

ip nat pool POOL-PAT 203.0.113.5 203.0.113.5 netmask 255.255.255.252

ip nat inside source list 1 pool POOL-PAT overload

end
wr
```

### Récapitulatif des commandes PAT

| Élément | Commande |
|---------|----------|
| Interface inside | `ip nat inside` |
| Interface outside | `ip nat outside` |
| ACL sources autorisées | `access-list 1 permit [réseau] [wildcard]` |
| PAT avec IP interface | `ip nat inside source list 1 interface g0/0 overload` |
| PAT avec pool | `ip nat inside source list 1 pool [NOM] overload` |

---

## 6. Vérification et dépannage

### Commandes de vérification

```cisco
! Voir les traductions NAT actives
R1# show ip nat translations

! Voir les statistiques NAT
R1# show ip nat statistics

! Voir la configuration NAT
R1# show running-config | section nat
R1# show running-config | include nat
```

### Exemple de sortie "show ip nat translations"

```
R1# show ip nat translations

Pro Inside global      Inside local       Outside local      Outside global
tcp 203.0.113.5:10001  192.168.1.10:50001 8.8.8.8:443       8.8.8.8:443
tcp 203.0.113.5:10002  192.168.1.11:50234 8.8.8.8:443       8.8.8.8:443
--- 203.0.113.100      192.168.1.100      ---               ---
```

| Colonne | Signification |
|---------|---------------|
| Pro | Protocole (tcp, udp, icmp, ---) |
| Inside global | IP:port publique |
| Inside local | IP:port privée |
| Outside local/global | Destination |

### Dépannage

**Problème : Le NAT ne fonctionne pas**

1. **Vérifier les interfaces inside/outside**
   ```cisco
   R1# sh ip int g0/0 | include NAT
   R1# sh ip int g0/1 | include NAT
   ```

2. **Vérifier l'ACL**
   ```cisco
   R1# sh access-list 1
   ! Vérifier que le réseau source est bien autorisé
   ```

3. **Vérifier les traductions**
   ```cisco
   R1# sh ip nat translations
   ! Si vide → le NAT ne se déclenche pas
   ```

4. **Vérifier la route par défaut**
   ```cisco
   R1# sh ip route
   ! Il faut une route vers Internet !
   ```

**Problème : Effacer les traductions NAT**

```cisco
! Effacer toutes les traductions dynamiques
R1# clear ip nat translation *
```

### Debug NAT (pour troubleshooting avancé)

```cisco
R1# debug ip nat
! Attention : très verbeux, utiliser avec précaution

R1# undebug all    ! Pour arrêter
```

---

## 7. Exemples pratiques

### Exemple 1 : PME avec accès Internet (PAT)

**Scénario :** 50 PC doivent accéder à Internet via une seule IP publique.

```
         [Internet]
              │
              │ 203.0.113.2/30
         ┌────┴────┐
         │   R1    │
         │  g0/0   │ outside
         │  g0/1   │ inside - 192.168.1.254/24
         └────┬────┘
              │
         [Switch]
         /    |    \
       PC1   PC2   PC3
       .10   .11   .12
```

**Configuration complète R1 :**
```cisco
en
conf t
hostname R1
no ip domain-lookup

! Interface vers Internet
int g0/0
 description Vers FAI
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
 no shut

! Interface vers LAN
int g0/1
 description Vers LAN
 ip address 192.168.1.254 255.255.255.0
 ip nat inside
 no shut

! Route par défaut vers Internet
ip route 0.0.0.0 0.0.0.0 203.0.113.1

! ACL pour le NAT
access-list 1 permit 192.168.1.0 0.0.0.255

! PAT (NAT Overload)
ip nat inside source list 1 interface g0/0 overload

end
wr
```

### Exemple 2 : Serveur web accessible depuis Internet (NAT Statique + PAT)

**Scénario :**
- Les PC doivent accéder à Internet (PAT)
- Le serveur web doit être accessible depuis Internet (NAT Statique)

```
         [Internet]
              │
         ┌────┴────┐
         │   R1    │ 203.0.113.2/30
         └────┬────┘
              │
         [Switch]
         /         \
   [Serveur Web]  [PC1]
   192.168.1.100  192.168.1.10

   Accessible via : 203.0.113.100
```

**Configuration R1 :**
```cisco
en
conf t

int g0/0
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
 no shut

int g0/1
 ip address 192.168.1.254 255.255.255.0
 ip nat inside
 no shut

! Route par défaut
ip route 0.0.0.0 0.0.0.0 203.0.113.1

! NAT Statique pour le serveur web
ip nat inside source static 192.168.1.100 203.0.113.100

! ACL pour PAT (exclure le serveur car déjà en NAT statique)
access-list 1 deny 192.168.1.100
access-list 1 permit 192.168.1.0 0.0.0.255

! PAT pour les autres
ip nat inside source list 1 interface g0/0 overload

end
wr
```

### Exemple 3 : Multi-VLAN avec PAT

**Scénario :** Plusieurs VLANs doivent accéder à Internet.

```
         [Internet]
              │
         ┌────┴────┐
         │   R1    │ 203.0.113.2/30
         │  g0/0   │ outside
         │  g0/1   │ trunk (inside)
         └────┬────┘
              │
         [Switch]
         /         \
   VLAN 10        VLAN 20
   192.168.10.0   192.168.20.0
```

**Configuration R1 :**
```cisco
en
conf t

int g0/0
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
 no shut

int g0/1
 no ip address
 no shut

int g0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
 ip nat inside

int g0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.254 255.255.255.0
 ip nat inside

! Route par défaut
ip route 0.0.0.0 0.0.0.0 203.0.113.1

! ACL pour tous les VLANs
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255

! PAT
ip nat inside source list 1 interface g0/0 overload

end
wr
```

---

## Tableau récapitulatif

| Type | Commande clé | Cas d'usage |
|------|--------------|-------------|
| NAT Statique | `ip nat inside source static [privée] [publique]` | Serveurs |
| NAT Dynamique | `ip nat inside source list [ACL] pool [POOL]` | Pool d'IP publiques |
| PAT | `ip nat inside source list [ACL] interface [INT] overload` | Accès Internet standard |

---

## Checklist configuration NAT/PAT

```
□ Interface inside configurée (ip nat inside)
□ Interface outside configurée (ip nat outside)
□ ACL créée pour les sources autorisées
□ Commande NAT/PAT configurée
□ Route par défaut vers Internet
□ Test : ping 8.8.8.8 depuis un PC interne
□ Vérification : sh ip nat translations
□ Configuration sauvegardée (wr)
```

---

**Version 1.0 - Janvier 2025**
**Formation TSSR - Nextformation**
