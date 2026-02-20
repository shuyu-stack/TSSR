# Adressage IP et Subnetting

> 📚 **Module :** Réseaux - Fondamentaux  
> 📅 **Date :** Janvier 2026  
> ⏱️ **Durée :** 6 heures  
> 🎯 **Niveau :** Débutant → Intermédiaire

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Prérequis](#-prérequis)
- [Introduction aux adresses IP](#-introduction-aux-adresses-ip)
- [Comprendre les masques de sous-réseau](#-comprendre-les-masques-de-sous-réseau)
- [Classes d'adresses IP](#-classes-dadresses-ip)
- [Calcul d'adresses IP](#-calcul-dadresses-ip)
- [Le Subnetting (découpage en sous-réseaux)](#-le-subnetting-découpage-en-sous-réseaux)
- [Méthodes de calcul rapide](#-méthodes-de-calcul-rapide)
- [Exercices pratiques](#-exercices-pratiques)
- [Ressources](#-ressources)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Expliquer ce qu'est une adresse IP et son rôle dans un réseau
- ✅ Distinguer les différentes classes d'adresses IP
- ✅ Comprendre et utiliser les masques de sous-réseau
- ✅ Calculer l'adresse réseau, l'adresse de broadcast et la plage d'hôtes
- ✅ Découper un réseau en sous-réseaux (subnetting)
- ✅ Résoudre des problèmes de subnetting complexes
- ✅ Configurer correctement les adresses IP sur un serveur

---

## 📋 Prérequis

Avant de commencer ce cours, vous devez :

- [ ] Comprendre les bases des systèmes binaire et décimal
- [ ] Savoir convertir du binaire vers le décimal et inversement
- [ ] Connaître les notions de réseau informatique (LAN, routeur, switch)
- [ ] Avoir une calculatrice ou un outil de conversion binaire

**Matériel nécessaire :**
- 💻 Calculatrice scientifique (ou en ligne)
- 📝 Papier et stylo pour les calculs
- 🖥️ Accès à un terminal Windows/Linux pour tester

**Rappel système binaire :**
| Décimal | Binaire | Puissance de 2 |
|---------|---------|----------------|
| 1 | 00000001 | 2⁰ |
| 2 | 00000010 | 2¹ |
| 4 | 00000100 | 2² |
| 8 | 00001000 | 2³ |
| 16 | 00010000 | 2⁴ |
| 32 | 00100000 | 2⁵ |
| 64 | 01000000 | 2⁶ |
| 128 | 10000000 | 2⁷ |

---

## 📚 Introduction aux adresses IP

### Qu'est-ce qu'une adresse IP ?

Une **adresse IP (Internet Protocol)** est un identifiant unique attribué à chaque appareil connecté à un réseau informatique. C'est comme une **adresse postale** pour les ordinateurs.

### Pourquoi c'est important ?

✅ **Communication** : Les appareils utilisent les IP pour s'envoyer des données  
✅ **Identification** : Chaque appareil a une adresse unique sur le réseau  
✅ **Routage** : Les routeurs utilisent les IP pour acheminer les paquets  
✅ **Administration** : Permet de gérer et organiser le réseau  

### Structure d'une adresse IPv4

Une adresse IPv4 est composée de **4 octets** (32 bits au total) séparés par des points.

**Exemple :** `192.168.1.10`

```
Notation décimale :  192    .    168    .    1      .    10
Notation binaire  :  11000000.10101000.00000001.00001010
Nombre de bits    :  [8 bits].[8 bits].[8 bits].[8 bits] = 32 bits
```

**Valeurs possibles :**
- Chaque octet peut aller de **0 à 255** (en décimal)
- Soit **00000000 à 11111111** (en binaire)

### Anatomie d'une adresse IP

Une adresse IP se divise en **deux parties** :

| Partie | Description | Exemple (192.168.1.10/24) |
|--------|-------------|---------------------------|
| **Partie réseau** | Identifie le réseau | 192.168.1 |
| **Partie hôte** | Identifie l'appareil dans le réseau | 10 |

Le **masque de sous-réseau** définit où se trouve la séparation entre ces deux parties.

---

## 🎭 Comprendre les masques de sous-réseau

### Qu'est-ce qu'un masque de sous-réseau ?

Le **masque de sous-réseau** (subnet mask) est un nombre de 32 bits qui définit quelle partie de l'adresse IP représente le réseau et quelle partie représente l'hôte.

### Notations du masque

Il existe **deux notations** pour écrire un masque :

**1. Notation décimale :** `255.255.255.0`  
**2. Notation CIDR (slash) :** `/24`

### Tableau de correspondance

| Notation CIDR | Masque décimal | Masque binaire | Bits réseau | Bits hôte | Nombre d'hôtes |
|---------------|----------------|----------------|-------------|-----------|----------------|
| /8 | 255.0.0.0 | 11111111.00000000.00000000.00000000 | 8 | 24 | 16 777 214 |
| /16 | 255.255.0.0 | 11111111.11111111.00000000.00000000 | 16 | 16 | 65 534 |
| /24 | 255.255.255.0 | 11111111.11111111.11111111.00000000 | 24 | 8 | 254 |
| /25 | 255.255.255.128 | 11111111.11111111.11111111.10000000 | 25 | 7 | 126 |
| /26 | 255.255.255.192 | 11111111.11111111.11111111.11000000 | 26 | 6 | 62 |
| /27 | 255.255.255.224 | 11111111.11111111.11111111.11100000 | 27 | 5 | 30 |
| /28 | 255.255.255.240 | 11111111.11111111.11111111.11110000 | 28 | 4 | 14 |
| /29 | 255.255.255.248 | 11111111.11111111.11111111.11111000 | 29 | 3 | 6 |
| /30 | 255.255.255.252 | 11111111.11111111.11111111.11111100 | 30 | 2 | 2 |

### Comment ça fonctionne ?

Le masque utilise des **1** pour la partie réseau et des **0** pour la partie hôte.

**Exemple avec 192.168.1.10/24 :**

```
Adresse IP :   192.168.1.10   = 11000000.10101000.00000001.00001010
Masque /24 :   255.255.255.0  = 11111111.11111111.11111111.00000000
                                 [--- Partie réseau ---][- Hôte -]
```

Les **24 premiers bits** (3 octets) = **réseau**  
Les **8 derniers bits** (1 octet) = **hôte**

> 💡 **Astuce :** Plus le nombre après le `/` est grand, plus il y a de bits pour le réseau, donc **moins d'hôtes disponibles**.

---

## 🏷️ Classes d'adresses IP

Les adresses IPv4 sont divisées en **5 classes** (A, B, C, D, E), mais seules les **3 premières** sont utilisées pour les réseaux classiques.

### Tableau des classes

| Classe | Premier octet | Plage | Masque par défaut | Utilisation | Nombre d'hôtes/réseau |
|--------|---------------|-------|-------------------|-------------|----------------------|
| **A** | 1-126 | 1.0.0.0 à 126.255.255.255 | 255.0.0.0 (/8) | Très grands réseaux | ~16 millions |
| **B** | 128-191 | 128.0.0.0 à 191.255.255.255 | 255.255.0.0 (/16) | Grands réseaux | ~65 000 |
| **C** | 192-223 | 192.0.0.0 à 223.255.255.255 | 255.255.255.0 (/24) | Petits réseaux | 254 |
| **D** | 224-239 | 224.0.0.0 à 239.255.255.255 | - | Multicast | - |
| **E** | 240-255 | 240.0.0.0 à 255.255.255.255 | - | Expérimental | - |

### Adresses IP spéciales

| Adresse | Type | Description |
|---------|------|-------------|
| 127.0.0.1 | Loopback | Adresse de bouclage (localhost) |
| 0.0.0.0 | Réseau par défaut | Représente "n'importe quelle adresse" |
| 255.255.255.255 | Broadcast général | Diffusion vers tous les hôtes |
| 10.0.0.0 - 10.255.255.255 | Privée (Classe A) | Réseaux privés (RFC 1918) |
| 172.16.0.0 - 172.31.255.255 | Privée (Classe B) | Réseaux privés (RFC 1918) |
| 192.168.0.0 - 192.168.255.255 | Privée (Classe C) | Réseaux privés (RFC 1918) |
| 169.254.0.0 - 169.254.255.255 | APIPA | Auto-configuration (pas de DHCP) |

### Adresses IP publiques vs privées

**IP Publiques :**
- ✅ Routables sur Internet
- ✅ Uniques au monde
- 💰 Doivent être achetées/louées

**IP Privées :**
- ✅ Utilisables librement en interne
- ❌ Non routables sur Internet
- 🔄 Nécessitent du NAT pour accéder à Internet

> 💡 **Astuce TSSR :** Dans un lab ou environnement de test, utilisez **TOUJOURS** des adresses privées (192.168.x.x, 10.x.x.x, 172.16-31.x.x).

---

## 🧮 Calcul d'adresses IP

### Les 4 informations essentielles

Pour chaque réseau, on doit connaître :

1. **Adresse réseau** (Network ID) : Premier élément du réseau
2. **Première adresse utilisable** : Premier hôte assignable
3. **Dernière adresse utilisable** : Dernier hôte assignable
4. **Adresse de broadcast** : Diffusion vers tous les hôtes du réseau

### Méthode de calcul

#### Étape 1 : Trouver l'adresse réseau

L'adresse réseau s'obtient en faisant un **ET logique** (AND) entre l'IP et le masque.

**Exemple : 192.168.1.130/25**

```
IP en binaire :     11000000.10101000.00000001.10000010  (192.168.1.130)
Masque /25 :        11111111.11111111.11111111.10000000  (255.255.255.128)
                    ────────────────────────────────────
ET logique (AND) :  11000000.10101000.00000001.10000000  = 192.168.1.128
```

**Adresse réseau : 192.168.1.128**

#### Étape 2 : Trouver l'adresse de broadcast

L'adresse de broadcast a **tous les bits hôte à 1**.

```
Adresse réseau :    11000000.10101000.00000001.10000000  (192.168.1.128)
Bits hôte à 1 :     00000000.00000000.00000000.01111111  (derniers 7 bits)
                    ────────────────────────────────────
Broadcast :         11000000.10101000.00000001.11111111  = 192.168.1.255
```

**Adresse de broadcast : 192.168.1.255**

#### Étape 3 : Plage d'hôtes utilisables

- **Première IP utilisable** = Adresse réseau + 1
- **Dernière IP utilisable** = Adresse de broadcast - 1

```
Réseau :         192.168.1.128  ❌ Non utilisable
Première IP :    192.168.1.129  ✅ Assignable
...
Dernière IP :    192.168.1.254  ✅ Assignable
Broadcast :      192.168.1.255  ❌ Non utilisable
```

### Formule magique

**Nombre d'hôtes utilisables = 2^(bits hôte) - 2**

Exemple avec /25 (7 bits hôte) :
- 2^7 = 128 adresses totales
- 128 - 2 = **126 hôtes utilisables**
- (-2 car on enlève l'adresse réseau et le broadcast)

### Tableau récapitulatif de l'exemple

| Information | Valeur |
|-------------|--------|
| Adresse IP | 192.168.1.130/25 |
| Masque décimal | 255.255.255.128 |
| Adresse réseau | 192.168.1.128 |
| Première IP utilisable | 192.168.1.129 |
| Dernière IP utilisable | 192.168.1.254 |
| Adresse de broadcast | 192.168.1.255 |
| Nombre d'hôtes | 126 |

---

## 🔪 Le Subnetting (découpage en sous-réseaux)

### Qu'est-ce que le subnetting ?

Le **subnetting** consiste à **diviser un grand réseau en plusieurs petits sous-réseaux**. C'est essentiel pour :

✅ **Optimiser l'utilisation des adresses**  
✅ **Séparer les départements** (Direction, IT, Compta)  
✅ **Améliorer la sécurité** (isolation des réseaux)  
✅ **Réduire le broadcast** (moins de trafic inutile)  

### Exemple pratique : Découper 192.168.1.0/24

**Besoin :** Créer **4 sous-réseaux** de même taille à partir de 192.168.1.0/24

#### Étape 1 : Calculer le nombre de bits nécessaires

Pour **4 sous-réseaux**, on a besoin de **2 bits** (car 2² = 4).

```
Masque d'origine :  /24  = 11111111.11111111.11111111.00000000
                                                       ^^^^^^^^
                                                       8 bits hôte

Emprunter 2 bits :  /26  = 11111111.11111111.11111111.11000000
                                                       ^^------
                                                       2 bits   6 bits
                                                       réseau   hôte
```

**Nouveau masque : /26 (255.255.255.192)**

#### Étape 2 : Calculer la taille de chaque sous-réseau

- **Bits hôte restants :** 6 bits
- **Nombre d'adresses par sous-réseau :** 2^6 = 64
- **Nombre d'hôtes utilisables :** 64 - 2 = 62

#### Étape 3 : Lister les sous-réseaux

| Sous-réseau | Adresse réseau | Première IP | Dernière IP | Broadcast | Usage |
|-------------|----------------|-------------|-------------|-----------|-------|
| **Sous-réseau 1** | 192.168.1.0 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 | Direction |
| **Sous-réseau 2** | 192.168.1.64 | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 | IT |
| **Sous-réseau 3** | 192.168.1.128 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 | Compta |
| **Sous-réseau 4** | 192.168.1.192 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 | Invités |

### Méthode du "pas d'incrémentation"

**Formule rapide :** Pas = 256 - dernier octet du masque

Pour un masque /26 (255.255.255.192) :
- Pas = 256 - 192 = **64**
- Les réseaux commencent à : 0, 64, 128, 192, 256 (fin)

> 💡 **Astuce :** L'adresse de broadcast est toujours **l'adresse du prochain réseau - 1**.

---

## ⚡ Méthodes de calcul rapide

### Tableau de référence rapide

Mémorisez ce tableau pour calculer rapidement :

| /Masque | Dernier octet | Pas | Nombre de sous-réseaux | Hôtes/réseau |
|---------|---------------|-----|------------------------|--------------|
| /24 | 0 | 256 | 1 | 254 |
| /25 | 128 | 128 | 2 | 126 |
| /26 | 192 | 64 | 4 | 62 |
| /27 | 224 | 32 | 8 | 30 |
| /28 | 240 | 16 | 16 | 14 |
| /29 | 248 | 8 | 32 | 6 |
| /30 | 252 | 4 | 64 | 2 |

### Technique de la "puissance de 2"

Pour trouver rapidement le nombre d'hôtes :

| Bits hôte | Calcul | Hôtes utilisables |
|-----------|--------|-------------------|
| 8 bits | 2⁸ - 2 | 254 |
| 7 bits | 2⁷ - 2 | 126 |
| 6 bits | 2⁶ - 2 | 62 |
| 5 bits | 2⁵ - 2 | 30 |
| 4 bits | 2⁴ - 2 | 14 |
| 3 bits | 2³ - 2 | 6 |
| 2 bits | 2² - 2 | 2 |

### Méthode "Subnet à l'envers"

Si on vous donne le **nombre d'hôtes nécessaires**, voici comment trouver le masque :

**Exemple :** J'ai besoin de **50 hôtes**.

1. Trouve la puissance de 2 supérieure : 2⁶ = 64 ✅ (assez)
2. Bits hôte = 6
3. Masque = 32 - 6 = **/26**

**Exemple 2 :** J'ai besoin de **100 hôtes**.

1. 2⁶ = 64 ❌ (pas assez)
2. 2⁷ = 128 ✅ (assez)
3. Bits hôte = 7
4. Masque = 32 - 7 = **/25**

---

## 🎯 Exercices pratiques

### Exercice 1 : Calcul simple

**Objectif :** Calculer les informations réseau de base

**Donnée :** `172.16.50.70/22`

**Questions :**
1. Quelle est l'adresse réseau ?
2. Quelle est la première IP utilisable ?
3. Quelle est la dernière IP utilisable ?
4. Quelle est l'adresse de broadcast ?
5. Combien d'hôtes sont disponibles ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Méthode de calcul :**

1. **Masque /22** = 255.255.252.0
   - Binaire : 11111111.11111111.11111100.00000000
   - Bits hôte : 10 bits (32 - 22)

2. **Adresse réseau :**
   ```
   172.16.50.70    = 172.16.00110010.01000110
   255.255.252.0   = 255.255.11111100.00000000
   AND             = 172.16.00110000.00000000
                   = 172.16.48.0
   ```

3. **Nombre d'hôtes :** 2¹⁰ - 2 = 1024 - 2 = 1022 hôtes

4. **Adresse de broadcast :**
   ```
   Réseau :     172.16.48.0
   +1022 hôtes
   +1 (broadcast)
   Broadcast :  172.16.51.255
   ```

**Réponses :**
1. Adresse réseau : **172.16.48.0**
2. Première IP : **172.16.48.1**
3. Dernière IP : **172.16.51.254**
4. Broadcast : **172.16.51.255**
5. Nombre d'hôtes : **1022**

</details>

---

### Exercice 2 : Subnetting simple

**Objectif :** Découper un réseau en sous-réseaux

**Donnée :** `10.10.0.0/16`

**Besoin :** Créer **8 sous-réseaux** de taille égale

**Questions :**
1. Quel sera le nouveau masque ?
2. Combien d'hôtes par sous-réseau ?
3. Listez les 3 premiers sous-réseaux avec leurs plages

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

1. **Nouveau masque :**
   - Pour 8 sous-réseaux : 2³ = 8
   - On emprunte 3 bits : /16 + 3 = **/19**
   - Masque : **255.255.224.0**

2. **Hôtes par sous-réseau :**
   - Bits hôte : 32 - 19 = 13 bits
   - 2¹³ - 2 = **8190 hôtes**

3. **Pas d'incrémentation :**
   - 256 - 224 = 32 (dans le 3ème octet)

4. **Les 3 premiers sous-réseaux :**

| Sous-réseau | Adresse réseau | Première IP | Dernière IP | Broadcast |
|-------------|----------------|-------------|-------------|-----------|
| 1 | 10.10.0.0 | 10.10.0.1 | 10.10.31.254 | 10.10.31.255 |
| 2 | 10.10.32.0 | 10.10.32.1 | 10.10.63.254 | 10.10.63.255 |
| 3 | 10.10.64.0 | 10.10.64.1 | 10.10.95.254 | 10.10.95.255 |

</details>

---

### Exercice 3 : VLSM (Variable Length Subnet Mask)

**Objectif :** Créer des sous-réseaux de tailles différentes

**Donnée :** `192.168.100.0/24`

**Besoins :**
- Département A : 100 hôtes
- Département B : 50 hôtes
- Département C : 25 hôtes
- Département D : 10 hôtes

**Questions :**
1. Quel masque pour chaque département ?
2. Attribuez les sous-réseaux (en commençant par le plus grand)

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Méthode VLSM :** Toujours commencer par le **plus grand** besoin.

1. **Département A (100 hôtes) :**
   - Besoin : 2⁷ = 128 adresses (assez)
   - Masque : /25 (7 bits hôte)
   - Sous-réseau : **192.168.100.0/25**
   - Plage : 192.168.100.1 - 192.168.100.126

2. **Département B (50 hôtes) :**
   - Besoin : 2⁶ = 64 adresses (assez)
   - Masque : /26 (6 bits hôte)
   - Sous-réseau : **192.168.100.128/26**
   - Plage : 192.168.100.129 - 192.168.100.190

3. **Département C (25 hôtes) :**
   - Besoin : 2⁵ = 32 adresses (assez)
   - Masque : /27 (5 bits hôte)
   - Sous-réseau : **192.168.100.192/27**
   - Plage : 192.168.100.193 - 192.168.100.222

4. **Département D (10 hôtes) :**
   - Besoin : 2⁴ = 16 adresses (assez)
   - Masque : /28 (4 bits hôte)
   - Sous-réseau : **192.168.100.224/28**
   - Plage : 192.168.100.225 - 192.168.100.238

**Tableau récapitulatif :**

| Département | Hôtes requis | Masque | Adresse réseau | Plage utilisable |
|-------------|--------------|--------|----------------|------------------|
| A | 100 | /25 | 192.168.100.0 | .1 - .126 |
| B | 50 | /26 | 192.168.100.128 | .129 - .190 |
| C | 25 | /27 | 192.168.100.192 | .193 - .222 |
| D | 10 | /28 | 192.168.100.224 | .225 - .238 |

**Adresses restantes :** 192.168.100.240 - 192.168.100.255 (réservées pour extension)

</details>

---

### Exercice 4 : Diagnostic réseau

**Objectif :** Identifier les problèmes de configuration IP

**Scénario :**
```
Serveur : 192.168.10.50/24
Client  : 192.168.10.200/25
```

**Questions :**
1. Le client et le serveur sont-ils sur le même réseau ?
2. Pourquoi le client ne peut pas communiquer avec le serveur ?
3. Quelle est la solution ?

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Analyse :**

**Serveur (192.168.10.50/24) :**
- Masque : 255.255.255.0
- Réseau : 192.168.10.0
- Plage : 192.168.10.1 - 192.168.10.254

**Client (192.168.10.200/25) :**
- Masque : 255.255.255.128
- Réseau : 192.168.10.128 (car 200 > 128)
- Plage : 192.168.10.129 - 192.168.10.254

**Réponses :**

1. **NON**, ils ne sont pas sur le même réseau :
   - Serveur : réseau **192.168.10.0**
   - Client : réseau **192.168.10.128**

2. Le client pense être sur un sous-réseau différent à cause de son masque /25.

3. **Solutions possibles :**
   - **Solution 1 :** Mettre le même masque /24 sur le client
   - **Solution 2 :** Utiliser un routeur entre les deux réseaux
   - **Solution 3 (recommandée) :** Uniformiser avec /24 sur tout le réseau

</details>

---

### Exercice 5 : Plan d'adressage entreprise

**Objectif :** Concevoir un plan d'adressage complet

**Contexte :**
Vous êtes technicien TSSR dans une entreprise. On vous donne le réseau **10.20.0.0/16** à découper pour :

- **Direction :** 500 postes
- **IT :** 100 postes  
- **Comptabilité :** 50 postes
- **Commercial :** 200 postes
- **Serveurs :** 30 machines
- **Imprimantes :** 20 machines
- **Point d'accès WiFi :** 50 AP

**Questions :**
1. Proposez un découpage avec VLSM
2. Documentez chaque sous-réseau
3. Réservez de l'espace pour future extension

**Solution :**

<details>
<summary>Cliquez pour voir la solution</summary>

**Plan d'adressage (ordre décroissant) :**

| Département | Besoin | Puissance de 2 | Masque | Réseau | Plage |
|-------------|--------|----------------|--------|--------|-------|
| Direction | 500 | 2⁹ = 512 | /23 | 10.20.0.0/23 | 10.20.0.1 - 10.20.1.254 |
| Commercial | 200 | 2⁸ = 256 | /24 | 10.20.2.0/24 | 10.20.2.1 - 10.20.2.254 |
| IT | 100 | 2⁷ = 128 | /25 | 10.20.3.0/25 | 10.20.3.1 - 10.20.3.126 |
| Comptabilité | 50 | 2⁶ = 64 | /26 | 10.20.3.128/26 | 10.20.3.129 - 10.20.3.190 |
| WiFi AP | 50 | 2⁶ = 64 | /26 | 10.20.3.192/26 | 10.20.3.193 - 10.20.3.254 |
| Serveurs | 30 | 2⁵ = 32 | /27 | 10.20.4.0/27 | 10.20.4.1 - 10.20.4.30 |
| Imprimantes | 20 | 2⁵ = 32 | /27 | 10.20.4.32/27 | 10.20.4.33 - 10.20.4.62 |

**Espace restant pour extension :** 10.20.4.64 - 10.20.255.254

**Documentation type :**

```markdown
# Plan d'adressage - Entreprise XYZ

## Direction (500 postes)
- Réseau : 10.20.0.0/23
- Passerelle : 10.20.0.1
- DHCP : 10.20.0.10 - 10.20.1.254
- VLAN : 10

## IT (100 postes)
- Réseau : 10.20.3.0/25
- Passerelle : 10.20.3.1
- DHCP : 10.20.3.10 - 10.20.3.126
- VLAN : 20

[etc...]
```

</details>

---

## 📚 Ressources

### Outils en ligne

- [IP Calculator de SubnetOnline](http://www.subnetonline.com/pages/subnet-calculators/ip-subnet-calculator.php)
- [VLSM Subnet Calculator](https://www.vlsm-calc.net/)
- [Visual Subnet Calculator](https://www.davidc.net/sites/default/subnets/subnets.html)
- [Convertisseur Binaire-Décimal](https://www.rapidtables.com/convert/number/binary-to-decimal.html)

### Documentation officielle

- [RFC 1918 - Private Address Space](https://tools.ietf.org/html/rfc1918)
- [RFC 4632 - CIDR](https://tools.ietf.org/html/rfc4632)
- [IANA IPv4 Address Registry](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xhtml)

### Tutoriels vidéo

- [Subnetting Made Easy (YouTube)](https://www.youtube.com/watch?v=example)
- [VLSM Explained (NetworkChuck)](https://www.youtube.com/watch?v=example)

### Livres recommandés

- "TCP/IP Illustrated" - W. Richard Stevens
- "Network+ Guide to Networks" - Jill West
- "Cisco CCNA - Routing et Switching" - Todd Lammle

### Pratique

- [Subnet Game (jeu d'entraînement)](http://www.subnettingquestions.com/)
- [Packet Tracer](https://www.netacad.com/courses/packet-tracer) - Simulation Cisco

---

## 📝 Notes personnelles

*(Ajoutez ici vos notes, observations et questions durant le cours)*

**Astuces que j'ai retenues :**
- 
- 

**Erreurs à éviter :**
- 
- 

**Questions à poser au formateur :**
- 
- 

---

## ✅ Checklist de révision

Avant de passer au module suivant (Active Directory), assurez-vous de maîtriser :

- [ ] Convertir du binaire en décimal et inversement
- [ ] Identifier la classe d'une adresse IP
- [ ] Calculer l'adresse réseau à partir d'une IP et d'un masque
- [ ] Calculer la plage d'hôtes utilisables
- [ ] Déterminer le nombre d'hôtes disponibles selon le masque
- [ ] Découper un réseau en sous-réseaux de taille égale
- [ ] Appliquer la méthode VLSM pour des sous-réseaux de tailles différentes
- [ ] Diagnostiquer des problèmes de configuration IP
- [ ] Créer un plan d'adressage pour une entreprise

**Auto-évaluation :**
- ⭐⭐⭐⭐⭐ : Je maîtrise complètement
- ⭐⭐⭐⭐⚪ : Je suis à l'aise, besoin de réviser
- ⭐⭐⭐⚪⚪ : Je comprends les bases
- ⭐⭐⚪⚪⚪ : Besoin de revoir le cours
- ⭐⚪⚪⚪⚪ : Besoin d'aide du formateur

---

## 🎓 Pour aller plus loin

### IPv6 (aperçu)

IPv6 est la nouvelle version du protocole IP avec des adresses de **128 bits**.

**Exemple d'adresse IPv6 :**
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

**Notation simplifiée :**
```
2001:db8:85a3::8a2e:370:7334
```

**Avantages d'IPv6 :**
- ✅ Espace d'adressage quasi-infini (340 undécillions d'adresses)
- ✅ Plus besoin de NAT
- ✅ Sécurité intégrée (IPsec)
- ✅ Auto-configuration

> 💡 **Note :** IPv6 sera abordé dans un cours dédié.

---

<div align="center">

**Cours suivant :** [Active Directory Domain Services](./active-directory.md)

**Cours précédent :** [Modèle OSI et TCP/IP](../01-Reseaux/modele-osi-tcpip.md)

[⬅️ Retour au sommaire](../../README.md)

---

### 💪 "Le subnetting, c'est comme les échecs : ça semble compliqué au début, mais avec de la pratique, ça devient naturel !"🚀

</div>