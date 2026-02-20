# Guide des Routes Statiques

> Guide pour Packet Tracer - Formation TSSR

---

## Sommaire

1. [C'est quoi une route statique ?](#1-cest-quoi-une-route-statique)
2. [Quand utiliser les routes statiques ?](#2-quand-utiliser-les-routes-statiques)
3. [Syntaxe et configuration](#3-syntaxe-et-configuration)
4. [Route par défaut](#4-route-par-défaut)
5. [Routes statiques flottantes](#5-routes-statiques-flottantes)
6. [Vérification et dépannage](#6-vérification-et-dépannage)
7. [Exemples pratiques](#7-exemples-pratiques)

---

## 1. C'est quoi une route statique ?

### Analogie simple

> Imagine que tu dois aller de Paris à Lyon. Tu as deux options :
> - **GPS automatique (protocole dynamique)** : Il calcule le chemin tout seul et s'adapte aux bouchons
> - **Itinéraire écrit sur un papier (route statique)** : Tu suis les instructions qu'on t'a données, point par point

Une **route statique**, c'est toi (l'admin) qui dis manuellement au routeur : *"Pour aller vers ce réseau, passe par là"*.

### Comment un routeur trouve son chemin ?

```
┌─────────────────────────────────────────────────────────────┐
│                    TABLE DE ROUTAGE                         │
├─────────────────────────────────────────────────────────────┤
│  Destination        Via              Type                   │
│  192.168.1.0/24     Gig0/0          Connected (C)          │
│  192.168.2.0/24     10.0.0.2        Static (S)             │
│  10.0.0.0/24        Gig0/1          Connected (C)          │
│  0.0.0.0/0          10.0.0.1        Static (S*)            │
└─────────────────────────────────────────────────────────────┘
```

Le routeur consulte cette table pour savoir où envoyer chaque paquet.

### Routes statiques vs dynamiques

| Aspect | Route statique | Route dynamique (OSPF, EIGRP) |
|--------|----------------|-------------------------------|
| Configuration | Manuelle | Automatique |
| Adaptation | Non (fixe) | Oui (s'adapte aux pannes) |
| Ressources | Légères | Consomme CPU/RAM |
| Maintenance | Lourde si beaucoup de routes | Légère |
| Sécurité | Plus sûr (pas d'échange) | Échange d'infos entre routeurs |

---

## 2. Quand utiliser les routes statiques ?

### Cas d'usage

**Utilise les routes statiques quand :**

1. **Petit réseau** (2-3 routeurs)
   - Pas besoin de protocole complexe

2. **Connexion vers Internet (route par défaut)**
   - Une seule sortie vers le FAI

3. **Réseau stub** (cul-de-sac)
   - Un site distant avec une seule connexion

4. **Backup d'une route dynamique**
   - Route flottante en secours

5. **Sécurité**
   - Tu ne veux pas que les routeurs échangent d'infos

```
        Route statique idéale

    [Internet]
        │
        │ Route par défaut
        ▼
    ┌───────┐
    │  R1   │ ← Routeur principal
    └───┬───┘
        │
        │ Route statique (stub)
        ▼
    ┌───────┐
    │  R2   │ ← Site distant (cul-de-sac)
    └───────┘
```

**N'utilise PAS les routes statiques quand :**
- Grand réseau (trop de routes à gérer)
- Topologie qui change souvent
- Besoin de redondance automatique

---

## 3. Syntaxe et configuration

### Syntaxe de base

```cisco
ip route [réseau_destination] [masque] [next-hop OU interface_sortie]
```

| Paramètre | Description | Exemple |
|-----------|-------------|---------|
| `réseau_destination` | Le réseau à atteindre | `192.168.2.0` |
| `masque` | Masque de sous-réseau | `255.255.255.0` |
| `next-hop` | IP du routeur suivant | `10.0.0.2` |
| `interface_sortie` | Interface de sortie | `GigabitEthernet0/1` |

### Méthode 1 : Via Next-Hop (recommandée)

> Tu indiques l'IP du prochain routeur

```cisco
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

**Traduction :** *"Pour aller vers 192.168.2.0/24, envoie les paquets à 10.0.0.2"*

```
    R1                          R2
┌─────────┐    10.0.0.0/24   ┌─────────┐
│  g0/1   │──────────────────│  g0/0   │───── 192.168.2.0/24
│ 10.0.0.1│                  │ 10.0.0.2│
└─────────┘                  └─────────┘

Sur R1 : ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

### Méthode 2 : Via Interface de sortie

> Tu indiques par quelle interface sortir

```cisco
R1(config)# ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/1
```

**Quand l'utiliser ?**
- Liaisons point-à-point (Serial)
- Évite une recherche ARP supplémentaire

### Méthode 3 : Next-Hop + Interface (la plus précise)

```cisco
R1(config)# ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/1 10.0.0.2
```

### Raccourci pour le masque

```cisco
! Ces deux commandes sont identiques
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
R1(config)# ip ro 192.168.2.0 255.255.255.0 10.0.0.2
```

---

## 4. Route par défaut

### C'est quoi ?

> La **route par défaut** (default route), c'est le chemin que prend le routeur quand il ne sait pas où aller. C'est comme dire : *"Si tu ne connais pas la destination, envoie tout par là"*.

Elle est notée `0.0.0.0/0` (= tous les réseaux).

### Configuration

```cisco
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

**Traduction :** *"Pour toute destination inconnue, envoie à 10.0.0.1"*

### Cas typique : Accès Internet

```
    [Internet]
         │
         │ 203.0.113.1 (FAI)
         │
    ┌────┴────┐
    │   R1    │
    │ g0/0    │ 203.0.113.2
    │ g0/1    │ 192.168.1.254
    └────┬────┘
         │
    [LAN 192.168.1.0/24]
```

```cisco
! Sur R1 : route par défaut vers le FAI
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

### Vérification

```cisco
R1# sh ip route

Gateway of last resort is 203.0.113.1 to network 0.0.0.0

S*   0.0.0.0/0 [1/0] via 203.0.113.1
```

- `S*` = Route statique par défaut
- `Gateway of last resort` = Passerelle de dernier recours

---

## 5. Routes statiques flottantes

### C'est quoi ?

> Une **route flottante**, c'est une route de secours qui ne s'active que si la route principale tombe.

On lui donne une **Distance Administrative (AD) plus élevée** pour qu'elle soit moins prioritaire.

### Rappel Distance Administrative

| Type de route | AD par défaut |
|---------------|---------------|
| Connectée | 0 |
| **Statique** | **1** |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

### Configuration

```cisco
! Route principale (AD = 1 par défaut)
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2

! Route flottante (AD = 200, s'active si la principale tombe)
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.0.2 200
```

### Exemple : Backup de liaison

```
                    10.0.0.0/24 (Principal - Fibre)
              ┌─────────────────────────────────┐
              │                                 │
         ┌────┴────┐                       ┌────┴────┐
         │   R1    │                       │   R2    │
         │         │                       │         │
         └────┬────┘                       └─────────┘
              │                                 │
              └─────────────────────────────────┘
                    10.1.0.0/24 (Backup - 4G)
```

```cisco
! Sur R1
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2      ! Principal
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.0.2 200  ! Backup
```

**Comportement :**
1. En temps normal : trafic passe par 10.0.0.2 (fibre)
2. Si fibre tombe : trafic bascule sur 10.1.0.2 (4G)
3. Si fibre revient : retour automatique sur 10.0.0.2

---

## 6. Vérification et dépannage

### Commandes de vérification

```cisco
! Voir toutes les routes
R1# show ip route

! Voir seulement les routes statiques
R1# show ip route static

! Voir une route spécifique
R1# show ip route 192.168.2.0

! Voir la config des routes
R1# show running-config | include ip route
```

### Lecture de la table de routage

```
R1# show ip route

Codes: C - connected, S - static, R - RIP, D - EIGRP, O - OSPF

Gateway of last resort is 10.0.0.1 to network 0.0.0.0

S*   0.0.0.0/0 [1/0] via 10.0.0.1
C    10.0.0.0/24 is directly connected, GigabitEthernet0/1
L    10.0.0.254/32 is directly connected, GigabitEthernet0/1
S    192.168.2.0/24 [1/0] via 10.0.0.2
C    192.168.1.0/24 is directly connected, GigabitEthernet0/0
```

| Code | Signification |
|------|---------------|
| `S` | Route statique |
| `S*` | Route statique par défaut |
| `C` | Directement connectée |
| `L` | Adresse locale de l'interface |
| `[1/0]` | [Distance Administrative / Métrique] |

### Dépannage

**Problème : La route n'apparaît pas**

```cisco
R1# sh ip route static
! Rien n'apparaît...
```

**Causes possibles :**

1. **Interface de sortie down**
   ```cisco
   R1# sh ip int br
   ! Vérifier que l'interface est UP/UP
   ```

2. **Next-hop injoignable**
   ```cisco
   R1# ping 10.0.0.2
   ! Si timeout → problème de connectivité
   ```

3. **Erreur de syntaxe**
   ```cisco
   R1# sh run | include ip route
   ! Vérifier la commande
   ```

**Problème : Le ping ne fonctionne pas malgré la route**

→ As-tu configuré la route **retour** sur l'autre routeur ?

```
R1 ──────────────────► R2
   route vers R2 OK

R1 ◄────────────────── R2
   route retour manquante !
```

---

## 7. Exemples pratiques

### Exemple 1 : Réseau simple (2 routeurs)

```
    192.168.1.0/24          10.0.0.0/24          192.168.2.0/24
         │                      │                      │
    ┌────┴────┐            ┌────┴────┐            ┌────┴────┐
    │   PC1   │            │         │            │   PC2   │
    └────┬────┘            │         │            └────┬────┘
         │                 │         │                 │
    ┌────┴────┐       ┌────┴────┐   ┌────┴────┐   ┌────┴────┐
    │   R1    │───────│  g0/1   │   │  g0/0   │───│   R2    │
    │  g0/0   │       │ 10.0.0.1│   │ 10.0.0.2│   │  g0/1   │
    │.254     │       └─────────┘   └─────────┘   │.254     │
    └─────────┘                                   └─────────┘
```

**Configuration R1 :**
```cisco
en
conf t
hostname R1

int g0/0
 ip add 192.168.1.254 255.255.255.0
 no shut

int g0/1
 ip add 10.0.0.1 255.255.255.0
 no shut

! Route vers le réseau de R2
ip route 192.168.2.0 255.255.255.0 10.0.0.2

end
wr
```

**Configuration R2 :**
```cisco
en
conf t
hostname R2

int g0/0
 ip add 10.0.0.2 255.255.255.0
 no shut

int g0/1
 ip add 192.168.2.254 255.255.255.0
 no shut

! Route vers le réseau de R1
ip route 192.168.1.0 255.255.255.0 10.0.0.1

end
wr
```

**Test :**
```cisco
R1# ping 192.168.2.254
!!!!!
```

### Exemple 2 : Accès Internet avec route par défaut

```
      [Internet]
           │
           │ 203.0.113.1 (FAI)
      ┌────┴────┐
      │   R1    │
      │  g0/0   │ 203.0.113.2
      │  g0/1   │ 192.168.1.254
      └────┬────┘
           │
    [LAN 192.168.1.0/24]
           │
      ┌────┴────┐
      │   PC    │
      │ .1      │
      └─────────┘
```

**Configuration R1 :**
```cisco
en
conf t
hostname R1

int g0/0
 description Vers FAI
 ip add 203.0.113.2 255.255.255.252
 no shut

int g0/1
 description Vers LAN
 ip add 192.168.1.254 255.255.255.0
 no shut

! Route par défaut vers Internet
ip route 0.0.0.0 0.0.0.0 203.0.113.1

end
wr
```

**Sur le PC :**
- IP : 192.168.1.1
- Masque : 255.255.255.0
- Gateway : 192.168.1.254

### Exemple 3 : Topologie en étoile (hub and spoke)

```
                    ┌─────────┐
                    │   R1    │ (Hub central)
                    │  Siège  │
                    └────┬────┘
                         │
          ┌──────────────┼──────────────┐
          │              │              │
     ┌────┴────┐    ┌────┴────┐    ┌────┴────┐
     │   R2    │    │   R3    │    │   R4    │
     │ Site A  │    │ Site B  │    │ Site C  │
     └─────────┘    └─────────┘    └─────────┘
     192.168.2.0    192.168.3.0    192.168.4.0
```

**Sur R1 (Hub) :**
```cisco
! Routes vers chaque site
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 192.168.3.0 255.255.255.0 10.0.0.3
ip route 192.168.4.0 255.255.255.0 10.0.0.4
```

**Sur R2, R3, R4 (Spokes) :**
```cisco
! Route par défaut vers le hub
ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

---

## Récapitulatif des commandes

| Action | Commande |
|--------|----------|
| Route statique | `ip route [dest] [mask] [next-hop]` |
| Route par défaut | `ip route 0.0.0.0 0.0.0.0 [next-hop]` |
| Route flottante | `ip route [dest] [mask] [next-hop] [AD]` |
| Supprimer route | `no ip route [dest] [mask] [next-hop]` |
| Voir routes | `show ip route` |
| Voir routes statiques | `show ip route static` |

---

## Checklist configuration

```
□ Interfaces configurées et UP
□ Route aller configurée
□ Route RETOUR configurée (souvent oublié !)
□ Ping next-hop OK
□ Ping destination finale OK
□ Configuration sauvegardée (wr)
```

---

**Version 1.0 - Janvier 2025**
**Formation TSSR - Nextformation**
