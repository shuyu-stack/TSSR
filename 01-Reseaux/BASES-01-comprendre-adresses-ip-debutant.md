# Comprendre les adresses IP - Pour les vrais débutants

## Message du formateur

Hey, je sais que les IPs ça peut sembler compliqué. C'est normal, **tout le monde a galéré au début** (moi le premier!). On va y aller **DOUCEMENT**, avec des exemples simples.

Pas de stress, on va prendre le temps. Si tu ne comprends pas quelque chose, **c'est normal**, relis tranquillement. L'objectif c'est que TU comprennes, pas que tu ailles vite.

---

## C'est quoi une adresse IP ?

### L'analogie de l'adresse postale

Une adresse IP, c'est exactement comme une adresse postale. Vraiment.

```
Une IP = Une adresse postale

Ton appartement :
15 rue Victor Hugo
92100 Boulogne-Billancourt

Ton PC :
192.168.1.10

─────────────────────────────────────────

L'adresse postale sert à :
✅ Te localiser
✅ Recevoir du courrier
✅ Être unique (pas 2 personnes avec la même adresse)

L'adresse IP sert à :
✅ Localiser ton PC sur le réseau
✅ Recevoir des données
✅ Être unique (pas 2 PCs avec la même IP)
```

**En gros :** Sans adresse IP, ton PC est invisible sur le réseau. C'est comme si tu habitais quelque part mais que personne ne connaît ton adresse : impossible de t'envoyer du courrier !

---

## Les 4 nombres d'une IP

Une adresse IP, c'est **4 nombres séparés par des points**.

```
192  .  168  .  1  .  10
 ↑      ↑     ↑    ↑
Pays   Ville  Rue  Numéro
```

### Les règles simples

- Chaque nombre va de **0 à 255**
- Ils sont **toujours** séparés par des **points**
- Il y a **toujours** 4 nombres

**Exemples valides :**
- 192.168.1.1
- 10.0.0.5
- 172.16.50.100
- 8.8.8.8 (le DNS de Google)

**Exemples INVALIDES :**
- 192.168.1.256 (256 est trop grand, max = 255)
- 192.168.1 (il manque un nombre)
- 192.168.1.1.5 (il y a 5 nombres au lieu de 4)

---

## IP privée vs IP publique (analogie simple)

C'est un concept **super important** mais facile à comprendre avec une analogie.

### L'analogie de l'immeuble

```
┌─────────────────────────────────────────┐
│  IP PRIVÉE (chez toi, dans l'entreprise)│
├─────────────────────────────────────────┤
│  = Numéro d'appartement DANS l'immeuble │
│                                         │
│  Appartement 101, 102, 103...           │
│  → Chaque appart a son numéro           │
│  → Mais de l'EXTÉRIEUR, c'est le même   │
│     immeuble (même adresse de rue)      │
│                                         │
│  192.168.1.10 (ton PC)                  │
│  192.168.1.11 (PC de ton collègue)      │
│  → Visibles seulement DANS l'entreprise │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│  IP PUBLIQUE (sur Internet)                 │
├─────────────────────────────────────────────┤
│  = Adresse de l'IMMEUBLE vue de l'extérieur │
│                                             │
│  15 rue Victor Hugo                         │
│  → UNE SEULE adresse pour TOUT l'immeuble  │
│                                             │
│  90.45.123.78 (ton entreprise sur Internet) │
│  → Visible par TOUT LE MONDE                │
└─────────────────────────────────────────────┘
```

### En résumé

**IP PRIVÉE :**
- Utilisée DANS ton entreprise ou chez toi
- Invisible depuis Internet
- Peut être la même dans plusieurs entreprises (pas de problème)
- Gratuite

**IP PUBLIQUE :**
- Utilisée sur Internet
- Visible par tout le monde
- UNIQUE au monde (pas de doublon possible)
- Payante (fournie par ton fournisseur Internet)

**Exemple concret :**

Chez toi, ton PC a l'IP privée **192.168.1.10**. Ton voisin a aussi un PC avec l'IP **192.168.1.10**. **Pas de problème**, car vos deux réseaux sont séparés !

Mais sur Internet, ton immeuble a l'IP publique **90.45.123.78**. Cette IP est **unique au monde**.

---

## Les 3 plages PRIVÉES à retenir (RFC 1918)

Il existe **3 plages d'IPs privées** autorisées. Tous les autres numéros sont publics.

```
Pour tes TPs et l'entreprise, tu utilises :

┌────────────────────────────────────┐
│  PLAGE 1 : 10.x.x.x               │
│  ────────────────────────────────  │
│  De 10.0.0.0 à 10.255.255.255     │
│  Usage : Très grandes entreprises │
│          Datacenters              │
│                                   │
│  Exemple : 10.50.20.15            │
├────────────────────────────────────┤
│  PLAGE 2 : 172.16.x.x à 172.31.x.x│
│  ────────────────────────────────  │
│  De 172.16.0.0 à 172.31.255.255   │
│  Usage : Moyennes entreprises     │
│                                   │
│  Exemple : 172.16.10.25           │
├────────────────────────────────────┤
│  PLAGE 3 : 192.168.x.x  ← TOI !   │
│  ────────────────────────────────  │
│  De 192.168.0.0 à 192.168.255.255 │
│  Usage : Petites entreprises      │
│          Maison                   │
│          TPs Packet Tracer        │
│                                   │
│  Exemple : 192.168.1.10           │
└────────────────────────────────────┘

💡 POUR TES TPS : Utilise TOUJOURS 192.168.X.X
   C'est le plus simple et le plus courant.
```

### Pourquoi ces plages sont réservées ?

Parce qu'une organisation mondiale (l'IANA) a décidé que ces plages ne seraient **jamais utilisées sur Internet**.

Donc tu peux les utiliser **tranquillement** dans ton entreprise sans risquer de conflit avec Internet.

---

## Comment choisir une IP pour ton réseau ?

### Méthode SIMPLE (pour débutant)

Voici une méthode en **3 étapes** pour choisir des IPs pour ton réseau.

```
Étape 1 : Choisis ta plage
─────────────────────────
Pour tes TPs : 192.168.X.0

Étape 2 : Choisis ton numéro de réseau (X)
───────────────────────────────────────────
Pour tes TPs, utilise un nombre entre 1 et 254

Exemples :
- Réseau 1 : 192.168.10.0  (X = 10)
- Réseau 2 : 192.168.20.0  (X = 20)
- Réseau 3 : 192.168.30.0  (X = 30)

Étape 3 : Attribue des IPs aux machines
────────────────────────────────────────
Dans chaque réseau, les machines ont des numéros de 1 à 254

Exemple réseau 192.168.10.0 :
- Routeur : 192.168.10.1 (souvent le .1)
- PC1 : 192.168.10.10
- PC2 : 192.168.10.11
- PC3 : 192.168.10.12
- Serveur : 192.168.10.50

RÈGLE : Évite 0 et 255 (réservés)
```

### Convention habituelle (bonne pratique)

Voici comment les pros organisent leurs IPs :

```
192.168.10.1       → Routeur (passerelle)
192.168.10.2-9     → Serveurs
192.168.10.10-99   → PCs des utilisateurs
192.168.10.100-199 → Imprimantes, téléphones IP
192.168.10.200-254 → Équipements réseau (switchs, etc.)
```

Tu n'es **pas obligé** de suivre cette convention, mais c'est bien pour s'organiser !

---

## TP Packet Tracer guidé - Ton premier réseau avec IPs

### Objectif

Créer un réseau avec **3 PCs** et leur donner des adresses IP manuellement.

À la fin, les 3 PCs pourront communiquer entre eux (tu pourras faire des `ping`).

---

### Étape 1 : Ouvrir Packet Tracer

1. Lance **Cisco Packet Tracer**
2. Tu vois une **grande zone vide** au centre (c'est normal, c'est là que tu vas créer ton réseau)
3. En bas, tu vois des **icônes** (ordinateurs, switchs, routeurs, câbles)

---

### Étape 2 : Ajouter 3 PCs

1. En bas à gauche, clique sur l'icône **"End Devices"** (icône d'ordinateur de bureau)
2. Clique sur **"PC"** (l'ordinateur classique)
3. Clique **3 fois** dans la zone blanche pour créer **PC0**, **PC1**, **PC2**

Tu dois maintenant avoir 3 ordinateurs sur ton écran.

---

### Étape 3 : Ajouter 1 Switch

1. En bas à gauche, clique sur **"Switches"** (icône de boîte avec des ports)
2. Clique sur **"2960"** (un switch classique)
3. Clique dans la zone pour le placer au centre, sous tes 3 PCs

**C'est quoi un switch ?** C'est une boîte qui permet de relier plusieurs ordinateurs entre eux. Comme une multiprise pour le réseau.

---

### Étape 4 : Câbler les PCs au switch

Maintenant on va relier chaque PC au switch avec des câbles réseau.

1. En bas à gauche, clique sur l'icône **câble** (éclair orange)
2. Choisis **"Copper Straight-Through"** (câble droit cuivre - le premier)
3. Clique sur **PC0**
   - Une petite fenêtre s'ouvre
   - Choisis **"FastEthernet0"** (le port réseau du PC)
4. Clique sur le **Switch**
   - Choisis **"FastEthernet0/1"** (le premier port du switch)
5. Un câble apparaît entre PC0 et le switch

**Répète l'opération pour PC1 et PC2 :**
- PC1 → Switch port FastEthernet0/2
- PC2 → Switch port FastEthernet0/3

**Important :** Tu dois voir des **petits triangles verts** aux extrémités des câbles.
- Vert = connexion OK ✅
- Orange = en cours de démarrage ⏳
- Rouge = problème ❌

Attends quelques secondes que tout devienne **vert**.

---

### Étape 5 : Donner des IPs aux PCs

Maintenant, on va donner une adresse IP à chaque PC.

**Pour PC0 :**

1. Clique sur **PC0**
2. Une fenêtre s'ouvre
3. Va dans l'onglet **"Desktop"**
4. Clique sur **"IP Configuration"**
5. Coche **"Static"** (IP manuelle)
6. Entre ces valeurs **exactement** :
   ```
   IP Address: 192.168.10.10
   Subnet Mask: 255.255.255.0
   Default Gateway: (laisse vide pour l'instant)
   ```
7. Ferme la fenêtre (croix en haut à droite)

**Pour PC1 :**

1. Clique sur **PC1**
2. Desktop → IP Configuration
3. Static
4. Entre :
   ```
   IP Address: 192.168.10.11
   Subnet Mask: 255.255.255.0
   Default Gateway: (vide)
   ```

**Pour PC2 :**

1. Clique sur **PC2**
2. Desktop → IP Configuration
3. Static
4. Entre :
   ```
   IP Address: 192.168.10.12
   Subnet Mask: 255.255.255.0
   Default Gateway: (vide)
   ```

---

### Étape 6 : Tester la communication (PING)

Le moment de vérité ! On va tester si les PCs peuvent se parler.

1. Clique sur **PC0**
2. Va dans **"Desktop"**
3. Clique sur **"Command Prompt"** (invite de commandes)
4. Une fenêtre noire s'ouvre (comme le terminal Windows)
5. Tape la commande : `ping 192.168.10.11`
6. Appuie sur **Entrée**

**Résultat attendu :**

```
Pinging 192.168.10.11 with 32 bytes of data:

Reply from 192.168.10.11: bytes=32 time<1ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.10.11:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

**Si tu vois "Reply from..." → BRAVO ! ÇA MARCHE ! 🎉**

**Test supplémentaire :** Depuis PC0, essaie aussi :
- `ping 192.168.10.12` (pour tester PC2)

---

### Dépannage (si ça ne marche pas)

**Si tu vois "Request timeout" ou "Destination host unreachable" :**

1. **Vérifie les IPs** : Clique sur chaque PC → Desktop → IP Configuration
   - Pas de faute de frappe ?
   - Les 3 IPs sont différentes ?
   - Le masque est bien 255.255.255.0 ?

2. **Vérifie les câbles** :
   - Les triangles sont verts ?
   - Si orange, attends 10-20 secondes (le switch démarre)

3. **Refais un ping** : Parfois il faut attendre un peu

4. **En dernier recours** : Supprime tout et recommence (c'est normal, ça arrive !)

---

## Exercices progressifs

### Exercice 1 (FACILE)

Tu as **5 PCs** à mettre dans le réseau **192.168.50.0**.

**Question :** Donne-leur des adresses IP (choisis les numéros toi-même).

**Consignes :**
- Utilise le réseau 192.168.50.0
- Donne une IP différente à chaque PC
- Utilise le masque 255.255.255.0

<details>
<summary>📖 Voir la solution</summary>

```
PC1 : 192.168.50.10
PC2 : 192.168.50.11
PC3 : 192.168.50.12
PC4 : 192.168.50.13
PC5 : 192.168.50.14

Notes :
- Tu pouvais choisir n'importe quel numéro entre 1 et 254
- L'important c'est que chaque PC ait une IP DIFFÉRENTE
- Exemples valides aussi : .20, .21, .22, .23, .24
- Ou : .100, .101, .102, .103, .104
```

</details>

---

### Exercice 2 (MOYEN)

Crée **2 réseaux différents** :
- **Réseau Compta** : 192.168.10.0 (3 PCs)
- **Réseau Direction** : 192.168.20.0 (2 PCs)

**Question :** Donne des IPs à tous les PCs.

<details>
<summary>📖 Voir la solution</summary>

```
Réseau Compta (192.168.10.0):
─────────────────────────────
PC1: 192.168.10.10 / 255.255.255.0
PC2: 192.168.10.11 / 255.255.255.0
PC3: 192.168.10.12 / 255.255.255.0

Réseau Direction (192.168.20.0):
─────────────────────────────────
PC4: 192.168.20.10 / 255.255.255.0
PC5: 192.168.20.11 / 255.255.255.0

Notes importantes :
- Les 2 réseaux sont DIFFÉRENTS (10 vs 20)
- Les PCs du réseau Compta NE PEUVENT PAS communiquer avec les PCs Direction
  (il faudrait un routeur pour ça, on verra plus tard)
- Chaque réseau a son propre masque 255.255.255.0
```

</details>

---

### Exercice 3 (AVANCÉ)

Crée ce réseau dans Packet Tracer :

```
Réseau entreprise 192.168.100.0

- 1 switch
- 4 PCs
- 1 imprimante réseau (dans "End Devices")

Plan d'adressage :
- Routeur : 192.168.100.1
- Imprimante : 192.168.100.50
- PC-Compta1 : 192.168.100.10
- PC-Compta2 : 192.168.100.11
- PC-Direction1 : 192.168.100.20
- PC-Direction2 : 192.168.100.21
```

**Tâches :**
1. Crée la topologie
2. Câble tout le monde
3. Configure les IPs
4. Teste avec des pings

<details>
<summary>📖 Voir la solution détaillée</summary>

**Étape 1 : Créer la topologie**
- Ajoute 1 switch 2960
- Ajoute 4 PCs
- Ajoute 1 imprimante (End Devices → Printer)

**Étape 2 : Câbler**
- PC-Compta1 → Switch Fa0/1
- PC-Compta2 → Switch Fa0/2
- PC-Direction1 → Switch Fa0/3
- PC-Direction2 → Switch Fa0/4
- Printer0 → Switch Fa0/5

**Étape 3 : Configurer les IPs**

Pour chaque équipement :
- Clique dessus → Desktop → IP Configuration → Static

```
PC-Compta1:
  IP: 192.168.100.10
  Mask: 255.255.255.0

PC-Compta2:
  IP: 192.168.100.11
  Mask: 255.255.255.0

PC-Direction1:
  IP: 192.168.100.20
  Mask: 255.255.255.0

PC-Direction2:
  IP: 192.168.100.21
  Mask: 255.255.255.0

Printer0:
  IP: 192.168.100.50
  Mask: 255.255.255.0
```

**Étape 4 : Tests**

Depuis PC-Compta1, teste :
```
ping 192.168.100.11  (PC-Compta2) → OK
ping 192.168.100.20  (PC-Direction1) → OK
ping 192.168.100.50  (Imprimante) → OK
```

Si tout répond, **BRAVO !** Tu as créé un réseau complet !

</details>

---

## Récapitulatif - Ce que tu as appris

✅ **Une IP = une adresse postale** (pour localiser un PC sur le réseau)

✅ **Format d'une IP** : 4 nombres de 0 à 255, séparés par des points

✅ **IP privée vs publique** :
  - Privée = dans l'entreprise (192.168.x.x, 10.x.x.x, 172.16-31.x.x)
  - Publique = sur Internet (unique au monde)

✅ **Les 3 plages privées** :
  - 10.x.x.x (grandes entreprises)
  - 172.16-31.x.x (moyennes entreprises)
  - 192.168.x.x (petites entreprises, TPs) ← **TU UTILISES CELLE-LÀ**

✅ **Comment choisir des IPs** :
  - Réseau : 192.168.10.0
  - Machines : 192.168.10.1 à 192.168.10.254

✅ **Packet Tracer** :
  - Créer une topologie (PCs, switch)
  - Câbler (Copper Straight-Through)
  - Configurer les IPs (Desktop → IP Configuration)
  - Tester (ping)

---

## Prochaine étape

Maintenant que tu comprends les IPs, on va voir **le subnetting** (découper un réseau en sous-réseaux).

Mais ne te presse pas ! Assure-toi de bien maîtriser les IPs avant de passer à la suite.

**Conseils :**
- Refais les exercices jusqu'à ce que ce soit facile
- Crée tes propres réseaux dans Packet Tracer
- Essaie de donner des IPs à 10 PCs, 20 PCs...
- Amuse-toi avec les pings !

**Tu peux être fier de toi, tu as fait un grand pas ! 💪**
