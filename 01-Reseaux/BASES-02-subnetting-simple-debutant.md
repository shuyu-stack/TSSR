# Le Subnetting expliqué simplement - Pour les vrais débutants

## Message du formateur

Le subnetting, c'est **le truc qui fait peur à tout le monde**. Mais je vais te montrer une méthode **ULTRA SIMPLE** que j'utilise depuis 15 ans. Pas de maths compliquées, promis.

Tu vas voir, avec des analogies concrètes, c'est beaucoup plus facile qu'on le pense.

**Important :** Si tu n'as pas encore lu le cours sur les IPs, va le lire d'abord. Le subnetting, c'est la suite logique.

---

## C'est quoi le subnetting ? (Analogie immeuble)

### Imagine un immeuble de 256 appartements

```
Option 1 : SANS subnetting
──────────────────────────
Un SEUL grand immeuble avec 256 appartements

┌─────────────────────────────────────┐
│  IMMEUBLE UNIQUE - 256 appartements │
│                                     │
│  Compta │ RH │ Compta │ Direction  │
│  RH │ Compta │ Direction │ RH...    │
│                                     │
│  Tout le monde mélangé              │
└─────────────────────────────────────┘

Problèmes :
❌ Tout le monde voit tout le monde
❌ Bruit partout (broadcast = crier dans tout l'immeuble)
❌ Pas de séparation par service
❌ Difficile à gérer (256 personnes dans un seul registre)


Option 2 : AVEC subnetting
──────────────────────────
Tu DÉCOUPES l'immeuble en 4 IMMEUBLES de 64 appartements

┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ IMMEUBLE 1   │ │ IMMEUBLE 2   │ │ IMMEUBLE 3   │ │ IMMEUBLE 4   │
│ 64 apparts   │ │ 64 apparts   │ │ 64 apparts   │ │ 64 apparts   │
│              │ │              │ │              │ │              │
│   COMPTA     │ │      RH      │ │  DIRECTION   │ │  TECHNIQUE   │
│              │ │              │ │              │ │              │
└──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘

Avantages :
✅ Chaque service a son immeuble
✅ Moins de bruit dans chaque immeuble
✅ Plus facile à gérer (4 registres de 64 personnes)
✅ Plus sécurisé (séparation)

C'EST ÇA LE SUBNETTING !
= Découper un grand réseau en petits réseaux
```

### En vrai

```
SANS subnetting :
────────────────
192.168.10.0/24 = 254 machines possibles
→ Tous les PCs dans le MÊME réseau
→ Tous les broadcasts vus par tout le monde

AVEC subnetting :
─────────────────
192.168.10.0/26 = 62 machines (sous-réseau 1 - Compta)
192.168.10.64/26 = 62 machines (sous-réseau 2 - RH)
192.168.10.128/26 = 62 machines (sous-réseau 3 - Direction)
192.168.10.192/26 = 62 machines (sous-réseau 4 - Technique)

→ 4 réseaux séparés
→ Moins de broadcast dans chaque réseau
→ Meilleure organisation
```

---

## Le masque de sous-réseau (expliqué simplement)

### L'analogie du code postal

Le masque, c'est comme un **code postal** ou une **frontière**.

```
Adresse postale : 15 rue Victor Hugo, 92100 Boulogne
                  ↑              ↑        ↑
               Numéro          Rue    Code postal

Le code postal (92100) = ton "masque"
→ Il dit : "Tu es dans le quartier 92100"
→ Tous ceux en 92100 sont dans ton quartier
→ Ceux en 75001 (Paris) sont dans un AUTRE quartier

─────────────────────────────────────────────────

En réseau :

IP : 192.168.10.50
Masque : 255.255.255.0
         ↑───────────↑  ↑
           Réseau    Machine

255.255.255.0 dit :
"Les 3 premiers nombres (192.168.10) = TON RÉSEAU"
"Le dernier nombre (50) = TON PC"

Donc tous les PCs en 192.168.10.X sont dans le MÊME réseau.
```

### Comment ça marche ?

Le masque dit : **"Jusqu'où va ton réseau ?"**

```
Masque : 255.255.255.0

255 = "Partie RÉSEAU" (fixe)
0 = "Partie MACHINE" (variable)

Exemple :
IP : 192.168.10.50
Masque : 255.255.255.0

192.168.10 → Partie RÉSEAU (ne change pas)
        50 → Partie MACHINE (change pour chaque PC)

Résultat : Tous les PCs de 192.168.10.1 à 192.168.10.254
           sont dans le MÊME réseau.
```

---

## Les 3 masques à retenir (pour débutant)

Tu n'as besoin de retenir que **3 masques** pour commencer. Les autres, tu les verras plus tard.

```
┌─────────────────────────────────────────────────────┐
│  MASQUE 1 : 255.255.255.0  (/24)  ← LE PLUS COURANT│
│  ────────────────────────────────────────────────   │
│  Permet : 254 machines                              │
│  Usage : Petits réseaux d'entreprise               │
│          TPs Packet Tracer                         │
│                                                     │
│  Exemple :                                          │
│  Réseau : 192.168.10.0                              │
│  Masque : 255.255.255.0                             │
│  PCs : de 192.168.10.1 à 192.168.10.254             │
│                                                     │
│  💡 Pour tes TPs, utilise TOUJOURS /24             │
├─────────────────────────────────────────────────────┤
│  MASQUE 2 : 255.255.0.0  (/16)                      │
│  ────────────────────────────────────────────────   │
│  Permet : 65 534 machines                           │
│  Usage : Moyennes/grandes entreprises               │
│                                                     │
│  Exemple :                                          │
│  Réseau : 172.16.0.0                                │
│  Masque : 255.255.0.0                               │
│  PCs : de 172.16.0.1 à 172.16.255.254               │
├─────────────────────────────────────────────────────┤
│  MASQUE 3 : 255.0.0.0  (/8)                         │
│  ────────────────────────────────────────────────   │
│  Permet : 16 millions de machines                   │
│  Usage : Très grandes entreprises, datacenters     │
│                                                     │
│  Exemple :                                          │
│  Réseau : 10.0.0.0                                  │
│  Masque : 255.0.0.0                                 │
│  PCs : de 10.0.0.1 à 10.255.255.254                 │
└─────────────────────────────────────────────────────┘
```

### C'est quoi le "/24", "/16", "/8" ?

C'est la **notation CIDR** (notation courte). Les pros utilisent ça pour aller plus vite.

```
Au lieu d'écrire : 192.168.10.0  255.255.255.0
On écrit : 192.168.10.0/24

Équivalences à retenir :
/24 = 255.255.255.0  ← Le plus courant
/16 = 255.255.0.0
/8  = 255.0.0.0
```

**Pour l'instant, utilise toujours /24 dans tes TPs. C'est le plus simple.**

---

## Les adresses spéciales dans un réseau

Dans chaque réseau, il y a **2 adresses spéciales** à ne JAMAIS donner à un PC.

```
Exemple : Réseau 192.168.10.0/24

┌────────────────────────────────────────────────┐
│ 192.168.10.0  → ADRESSE RÉSEAU (réservée)      │
│                 Identifie le réseau lui-même   │
│                 ❌ NE PAS UTILISER             │
├────────────────────────────────────────────────┤
│ 192.168.10.1 à 192.168.10.254                  │
│ → ADRESSES UTILISABLES pour les machines      │
│ ✅ TU PEUX LES UTILISER                        │
├────────────────────────────────────────────────┤
│ 192.168.10.255  → ADRESSE BROADCAST (réservée) │
│                   Pour envoyer à TOUT LE MONDE │
│                   ❌ NE PAS UTILISER           │
└────────────────────────────────────────────────┘

Résultat : Sur 256 adresses, tu peux en utiliser 254
```

### Récap rapide

```
Réseau 192.168.10.0/24 :

192.168.10.0 → Adresse réseau (on ne touche pas)
192.168.10.1 → Souvent le routeur
192.168.10.2-254 → Les machines
192.168.10.255 → Broadcast (on ne touche pas)

Total utilisable : 254 machines
```

---

## Méthode SIMPLE pour découper un réseau (pas de maths!)

Tu as un grand réseau et tu veux le **découper en plusieurs petits réseaux**. Voici la méthode visuelle.

### Exemple concret

**Problème :**
Découper le réseau **192.168.10.0/24** en **4 sous-réseaux** (pour 4 services).

---

### Méthode visuelle (étape par étape)

**Étape 1 : Combien d'adresses au départ ?**

```
Réseau de départ : 192.168.10.0/24
→ De 192.168.10.0 à 192.168.10.255
→ 256 adresses
```

**Étape 2 : Combien je veux de sous-réseaux ?**

```
Je veux : 4 sous-réseaux
(Compta, RH, Direction, Technique)
```

**Étape 3 : Diviser**

```
256 adresses ÷ 4 sous-réseaux = 64 adresses par sous-réseau
```

**Étape 4 : Découper**

```
┌────────────────────────────────────────┐
│  SOUS-RÉSEAU 1 : Compta                │
│  192.168.10.0 à 192.168.10.63          │
│  (64 adresses)                         │
│  Masque : 255.255.255.192 (/26)        │
│                                        │
│  Utilisable : .1 à .62 (62 machines)   │
├────────────────────────────────────────┤
│  SOUS-RÉSEAU 2 : RH                    │
│  192.168.10.64 à 192.168.10.127        │
│  (64 adresses)                         │
│  Masque : 255.255.255.192 (/26)        │
│                                        │
│  Utilisable : .65 à .126 (62 machines) │
├────────────────────────────────────────┤
│  SOUS-RÉSEAU 3 : Direction             │
│  192.168.10.128 à 192.168.10.191       │
│  (64 adresses)                         │
│  Masque : 255.255.255.192 (/26)        │
│                                        │
│  Utilisable : .129 à .190 (62 machines)│
├────────────────────────────────────────┤
│  SOUS-RÉSEAU 4 : Technique             │
│  192.168.10.192 à 192.168.10.255       │
│  (64 adresses)                         │
│  Masque : 255.255.255.192 (/26)        │
│                                        │
│  Utilisable : .193 à .254 (62 machines)│
└────────────────────────────────────────┘
```

**Astuce :** Tu n'as PAS besoin de calculer le masque 255.255.255.192. Sur un routeur Cisco, tu tapes juste `/26` et il comprend.

---

## Tableau de découpage rapide (mémo)

Voici un **tableau magique** pour découper un réseau /24. Imprime-le et garde-le !

```
┌───────────────────────────────────────────────────────┐
│  DÉCOUPER UN RÉSEAU /24                               │
├─────────────────┬──────────────┬───────────────────────┤
│  Je veux        │  Nouveau     │  Adresses par         │
│  X sous-réseaux │  masque      │  sous-réseau          │
├─────────────────┼──────────────┼───────────────────────┤
│  2              │  /25         │  128 adresses         │
│                 │              │  (126 machines)       │
├─────────────────┼──────────────┼───────────────────────┤
│  4              │  /26         │  64 adresses          │
│                 │              │  (62 machines)        │
├─────────────────┼──────────────┼───────────────────────┤
│  8              │  /27         │  32 adresses          │
│                 │              │  (30 machines)        │
├─────────────────┼──────────────┼───────────────────────┤
│  16             │  /28         │  16 adresses          │
│                 │              │  (14 machines)        │
└─────────────────┴──────────────┴───────────────────────┘

Exemple d'utilisation :
Je veux 4 sous-réseaux à partir de 192.168.10.0/24
→ Je regarde la ligne "4"
→ Nouveau masque = /26
→ Chaque sous-réseau = 62 machines

Mes 4 sous-réseaux :
1. 192.168.10.0/26
2. 192.168.10.64/26
3. 192.168.10.128/26
4. 192.168.10.192/26
```

---

## TP Packet Tracer - Créer 2 sous-réseaux

### Objectif

Créer **2 réseaux séparés** avec un routeur au milieu.
- Réseau 1 (gauche) : 192.168.10.0/24
- Réseau 2 (droite) : 192.168.20.0/24

À la fin, les PCs du réseau 1 pourront communiquer avec les PCs du réseau 2 (grâce au routeur).

---

### Topologie cible

```
PC1 ─┐              ┌─ PC3
PC2 ─┤─ SW1 ─ R1 ─ SW2 ─┤─ PC4
                         └─ PC5

Réseau 1 (gauche) : 192.168.10.0/24
Réseau 2 (droite) : 192.168.20.0/24
```

---

### Étape 1 : Créer la topologie

**Ajouter les équipements :**

1. Ajoute **2 PCs** (PC0, PC1) → Ce sera le réseau de gauche
2. Ajoute **1 switch 2960** (Switch0)
3. Ajoute **1 routeur 1841** (Router0)
   - En bas, clique sur "Routers" → choisis "1841"
4. Ajoute **1 switch 2960** (Switch1)
5. Ajoute **3 PCs** (PC2, PC3, PC4) → Ce sera le réseau de droite

---

### Étape 2 : Câbler

**Côté gauche (Réseau 1) :**
1. PC0 → Switch0 (FastEthernet0/1)
2. PC1 → Switch0 (FastEthernet0/2)
3. Switch0 (FastEthernet0/24) → Router0 (FastEthernet0/0)

**Côté droite (Réseau 2) :**
1. Router0 (FastEthernet0/1) → Switch1 (FastEthernet0/24)
2. PC2 → Switch1 (FastEthernet0/1)
3. PC3 → Switch1 (FastEthernet0/2)
4. PC4 → Switch1 (FastEthernet0/3)

**Attends que tous les câbles deviennent VERTS.**

---

### Étape 3 : Configurer les PCs du réseau 1

**PC0 :**
- IP : 192.168.10.10
- Masque : 255.255.255.0
- Passerelle : 192.168.10.1

**PC1 :**
- IP : 192.168.10.11
- Masque : 255.255.255.0
- Passerelle : 192.168.10.1

**C'est quoi la passerelle ?** C'est l'adresse IP du routeur. Quand un PC veut sortir de son réseau, il envoie tout au routeur.

---

### Étape 4 : Configurer les PCs du réseau 2

**PC2 :**
- IP : 192.168.20.10
- Masque : 255.255.255.0
- Passerelle : 192.168.20.1

**PC3 :**
- IP : 192.168.20.11
- Masque : 255.255.255.0
- Passerelle : 192.168.20.1

**PC4 :**
- IP : 192.168.20.12
- Masque : 255.255.255.0
- Passerelle : 192.168.20.1

---

### Étape 5 : Configurer le routeur (CLI)

Maintenant, on va configurer le routeur. C'est la première fois que tu vas utiliser la **ligne de commande Cisco** (CLI). Ne stresse pas, on y va pas à pas.

**Ouvrir le CLI du routeur :**

1. Clique sur **Router0**
2. Va dans l'onglet **"CLI"** (en haut)
3. Une fenêtre noire s'ouvre (invite de commandes)

**Appuie sur ENTRÉE** pour commencer.

---

**Configuration étape par étape :**

```cisco
! Tu vois "Router>" (c'est normal, c'est le mode utilisateur)
! On va passer en mode privilégié puis en mode configuration

Router> enable
Router#

! Le "#" signifie que tu es en mode privilégié (bien !)

Router# configure terminal
Router(config)#

! Le "(config)" signifie que tu es en mode configuration
```

**Configurer l'interface côté réseau 1 (FastEthernet 0/0) :**

```cisco
Router(config)# interface fastEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Explications :**
- `interface fastEthernet 0/0` → Je configure l'interface Fa0/0
- `ip address 192.168.10.1 255.255.255.0` → Je lui donne l'IP 192.168.10.1 (la passerelle du réseau 1)
- `no shutdown` → J'allume l'interface (sinon elle reste éteinte)
- `exit` → Je sors de la config de cette interface

---

**Configurer l'interface côté réseau 2 (FastEthernet 0/1) :**

```cisco
Router(config)# interface fastEthernet 0/1
Router(config-if)# ip address 192.168.20.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Explications :**
- Même chose, mais pour l'interface Fa0/1
- IP 192.168.20.1 (la passerelle du réseau 2)

---

**Sauvegarder la configuration :**

```cisco
Router(config)# exit
Router# write memory

! Ou (équivalent) :
Router# copy running-config startup-config
```

**Pourquoi sauvegarder ?**
Si tu ne sauvegardes pas et que tu redémarres le routeur, **tu perds tout**. Alors prends l'habitude de sauvegarder.

---

### Étape 6 : Tester

**Test 1 : PC0 ping PC1 (même réseau)**

1. Clique sur PC0
2. Desktop → Command Prompt
3. Tape : `ping 192.168.10.11`
4. Résultat attendu : **Reply from...** ✅

**Test 2 : PC0 ping PC2 (réseau différent via routeur)**

1. Depuis PC0
2. Tape : `ping 192.168.20.10`
3. Résultat attendu : **Reply from...** ✅

**Si ça marche, BRAVO ! Tu as créé 2 sous-réseaux reliés par un routeur !**

---

### Dépannage

**Si le ping ne passe pas entre les 2 réseaux :**

1. **Vérifie les passerelles** : Chaque PC doit avoir la bonne passerelle
   - PCs réseau 1 : passerelle 192.168.10.1
   - PCs réseau 2 : passerelle 192.168.20.1

2. **Vérifie les IPs du routeur** :
   - Clique sur Router0 → CLI
   - Tape : `show ip interface brief`
   - Tu dois voir :
     - Fa0/0 : 192.168.10.1 (up/up)
     - Fa0/1 : 192.168.20.1 (up/up)

3. **Vérifie les câbles** : Tous verts ?

---

## Exercices progressifs

### Exercice 1 (FACILE) - Comprendre le découpage

Tu as le réseau **192.168.50.0/24** et tu veux le découper en **2 sous-réseaux**.

**Questions :**
1. Combien d'adresses par sous-réseau ?
2. Quel est le nouveau masque ?
3. Donne les 2 plages d'IPs.

<details>
<summary>📖 Voir la solution</summary>

**Réponses :**

1. **256 ÷ 2 = 128 adresses par sous-réseau**

2. **Nouveau masque : /25** (ou 255.255.255.128)

3. **Les 2 sous-réseaux :**
   ```
   Sous-réseau 1 :
   192.168.50.0/25
   IPs utilisables : 192.168.50.1 à 192.168.50.126

   Sous-réseau 2 :
   192.168.50.128/25
   IPs utilisables : 192.168.50.129 à 192.168.50.254
   ```

</details>

---

### Exercice 2 (MOYEN) - Plan d'adressage

Une entreprise a 3 services :
- Compta : 20 personnes
- RH : 10 personnes
- Direction : 5 personnes

Tu as le réseau **192.168.100.0/24** à découper.

**Questions :**
1. Combien de sous-réseaux tu dois créer ?
2. Découpe le réseau en 4 sous-réseaux (pour avoir de la marge)
3. Attribue un sous-réseau à chaque service
4. Donne les plages IP de chaque service

<details>
<summary>📖 Voir la solution</summary>

**Réponses :**

1. **3 services = 3 sous-réseaux minimum** (mais on va créer 4 pour avoir de la marge)

2. **Découpage en 4 sous-réseaux :**
   - 256 ÷ 4 = 64 adresses par sous-réseau
   - Nouveau masque : /26

3. **Attribution :**
   ```
   Sous-réseau 1 : Compta
   192.168.100.0/26
   IPs : 192.168.100.1 à 192.168.100.62
   (62 machines possibles → suffisant pour 20 personnes)

   Sous-réseau 2 : RH
   192.168.100.64/26
   IPs : 192.168.100.65 à 192.168.100.126
   (62 machines possibles → suffisant pour 10 personnes)

   Sous-réseau 3 : Direction
   192.168.100.128/26
   IPs : 192.168.100.129 à 192.168.100.190
   (62 machines possibles → suffisant pour 5 personnes)

   Sous-réseau 4 : Réservé (pour futur service)
   192.168.100.192/26
   IPs : 192.168.100.193 à 192.168.100.254
   ```

4. **Plan d'adressage détaillé :**
   ```
   Service Compta (192.168.100.0/26) :
   - Passerelle : 192.168.100.1
   - Serveur : 192.168.100.2
   - PCs : 192.168.100.10 à 192.168.100.30

   Service RH (192.168.100.64/26) :
   - Passerelle : 192.168.100.65
   - Serveur : 192.168.100.66
   - PCs : 192.168.100.70 à 192.168.100.80

   Service Direction (192.168.100.128/26) :
   - Passerelle : 192.168.100.129
   - PCs : 192.168.100.130 à 192.168.100.135
   ```

</details>

---

### Exercice 3 (AVANCÉ) - TP Packet Tracer complet

Crée cette topologie dans Packet Tracer :

```
3 réseaux :
- Réseau Ventes : 192.168.10.0/26 (3 PCs)
- Réseau Prod : 192.168.10.64/26 (2 PCs)
- Réseau Admin : 192.168.10.128/26 (2 PCs)

Topologie :
PC-Ventes1 ─┐
PC-Ventes2 ─┤─ SW1 ─┐
PC-Ventes3 ─┘       │
                    ├─ Router ─┐
PC-Prod1 ─┬─ SW2 ───┘          │
PC-Prod2 ─┘                    │
                               │
PC-Admin1 ─┬─ SW3 ─────────────┘
PC-Admin2 ─┘
```

**Tâches :**
1. Crée la topologie
2. Configure les IPs de tous les PCs
3. Configure le routeur (3 interfaces)
4. Teste les pings entre tous les réseaux

<details>
<summary>📖 Voir la solution complète</summary>

**Configuration des PCs :**

```
Réseau Ventes (192.168.10.0/26) :
PC-Ventes1 : 192.168.10.10 / 255.255.255.192 / GW 192.168.10.1
PC-Ventes2 : 192.168.10.11 / 255.255.255.192 / GW 192.168.10.1
PC-Ventes3 : 192.168.10.12 / 255.255.255.192 / GW 192.168.10.1

Réseau Prod (192.168.10.64/26) :
PC-Prod1 : 192.168.10.70 / 255.255.255.192 / GW 192.168.10.65
PC-Prod2 : 192.168.10.71 / 255.255.255.192 / GW 192.168.10.65

Réseau Admin (192.168.10.128/26) :
PC-Admin1 : 192.168.10.130 / 255.255.255.192 / GW 192.168.10.129
PC-Admin2 : 192.168.10.131 / 255.255.255.192 / GW 192.168.10.129
```

**Configuration du routeur :**

```cisco
Router> enable
Router# configure terminal

! Interface pour réseau Ventes
Router(config)# interface fastEthernet 0/0
Router(config-if)# ip address 192.168.10.1 255.255.255.192
Router(config-if)# no shutdown
Router(config-if)# exit

! Interface pour réseau Prod
Router(config)# interface fastEthernet 0/1
Router(config-if)# ip address 192.168.10.65 255.255.255.192
Router(config-if)# no shutdown
Router(config-if)# exit

! Interface pour réseau Admin
Router(config)# interface fastEthernet 1/0
Router(config-if)# ip address 192.168.10.129 255.255.255.192
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Tests de validation :**

```
Depuis PC-Ventes1 :
ping 192.168.10.11 (PC-Ventes2 - même réseau) → OK
ping 192.168.10.70 (PC-Prod1 - autre réseau) → OK
ping 192.168.10.130 (PC-Admin1 - autre réseau) → OK

Si tout fonctionne, BRAVO ! Tu maîtrises le subnetting !
```

</details>

---

## Récapitulatif - Ce que tu as appris

✅ **Le subnetting** = découper un grand réseau en petits réseaux

✅ **Le masque** = frontière qui dit "jusqu'où va ton réseau"

✅ **Les 3 masques principaux** :
  - /24 (255.255.255.0) → 254 machines
  - /16 (255.255.0.0) → 65 534 machines
  - /8 (255.0.0.0) → 16 millions de machines

✅ **Adresses spéciales** :
  - Première adresse = adresse réseau (ne pas utiliser)
  - Dernière adresse = broadcast (ne pas utiliser)

✅ **Méthode de découpage** :
  - Nombre d'adresses ÷ nombre de sous-réseaux souhaités
  - Utiliser le tableau de découpage

✅ **Configuration routeur Cisco** :
  - `interface fastEthernet X/X`
  - `ip address X.X.X.X Y.Y.Y.Y`
  - `no shutdown`
  - `write memory`

---

## Prochaine étape

Maintenant que tu comprends le subnetting, on va voir les **VLANs** (réseaux virtuels sur un même switch).

**Conseils avant de continuer :**
- Refais les exercices jusqu'à ce que ce soit fluide
- Crée d'autres découpages (2, 8, 16 sous-réseaux)
- Entraîne-toi à configurer des routeurs dans Packet Tracer

**Tu progresses super bien, continue comme ça ! 💪**
