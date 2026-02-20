# 🎯 Guide Complet : Calcul Réseau Simplifié

## 📋 Table des matières
1. [Les bases essentielles](#1-les-bases-essentielles)
2. [La méthode universelle en 3 étapes](#2-la-méthode-universelle-en-3-étapes)
3. [Tableau de référence](#3-tableau-de-référence)
4. [Exemples détaillés](#4-exemples-détaillés)
5. [Cas particuliers](#5-cas-particuliers)
6. [Astuces et pièges à éviter](#6-astuces-et-pièges-à-éviter)

---

## 1. Les bases essentielles

### 🎯 Ce qu'il faut ABSOLUMENT retenir

| Concept | Définition | Exemple |
|---------|------------|---------|
| **Adresse réseau** | Identifiant du réseau (premier IP, réservée) | `192.168.1.0` |
| **Broadcast** | Adresse pour contacter tous les hôtes (dernier IP, réservée) | `192.168.1.255` |
| **Masque de sous-réseau** | Définit la taille du réseau | `255.255.255.0` ou `/24` |
| **CIDR** | Notation avec `/XX` | `/24`, `/26`, `/27` |
| **PAS** | Intervalle entre deux sous-réseaux | 64, 32, 16... |

### 🧮 Formule magique

```
Nombre d'IPs utilisables = 2^(32-CIDR) - 2

Exemples :
/24 → 2^8 - 2 = 254 IPs
/26 → 2^6 - 2 = 62 IPs
/27 → 2^5 - 2 = 30 IPs
```

---

## 2. La méthode universelle en 3 étapes

### ✅ ÉTAPE 1 : Trouver le masque

**Tableau de conversion rapide :**

| CIDR | Masque complet | Dernier octet | Mémo |
|------|----------------|---------------|------|
| `/24` | `255.255.255.0` | **0** | Le classique |
| `/25` | `255.255.255.128` | **128** | Moitié de 256 |
| `/26` | `255.255.255.192` | **192** | 128 + 64 |
| `/27` | `255.255.255.224` | **224** | 192 + 32 |
| `/28` | `255.255.255.240` | **240** | 224 + 16 |
| `/29` | `255.255.255.248` | **248** | 240 + 8 |
| `/30` | `255.255.255.252` | **252** | Liaison point à point |

### ✅ ÉTAPE 2 : Calculer le PAS

```
PAS = 256 - (dernier octet du masque)
```

**Exemples :**
- `/24` : 256 - 0 = **256** (tout l'octet)
- `/25` : 256 - 128 = **128**
- `/26` : 256 - 192 = **64**
- `/27` : 256 - 224 = **32**
- `/28` : 256 - 240 = **16**

### ✅ ÉTAPE 3 : Trouver le réseau

**Méthode 1 : Les multiples (recommandée)**
```
Liste les multiples du PAS : 0, PAS, 2×PAS, 3×PAS...
Encadre ton IP
Le plus petit = adresse réseau
```

**Méthode 2 : La division**
```
Dernier octet de l'IP ÷ PAS = X,YZ...
Garde X (partie entière)
X × PAS = adresse réseau (dernier octet)
```

---

## 3. Tableau de référence

### 📊 Tableau complet des masques courants

| CIDR | Masque | PAS | Multiples (dernier octet) | Nb d'IPs | Nb utilisables | Usage typique |
|------|--------|-----|---------------------------|----------|----------------|---------------|
| `/24` | `255.255.255.0` | 256 | 0 | 256 | 254 | Réseau d'entreprise standard |
| `/25` | `255.255.255.128` | 128 | 0, 128 | 128 | 126 | Division en 2 |
| `/26` | `255.255.255.192` | 64 | 0, 64, 128, 192 | 64 | 62 | Département/service |
| `/27` | `255.255.255.224` | 32 | 0, 32, 64, 96, 128, 160, 192, 224 | 32 | 30 | Petit bureau |
| `/28` | `255.255.255.240` | 16 | 0, 16, 32, 48, 64... | 16 | 14 | Très petit réseau |
| `/29` | `255.255.255.248` | 8 | 0, 8, 16, 24, 32... | 8 | 6 | Mini-réseau |
| `/30` | `255.255.255.252` | 4 | 0, 4, 8, 12, 16... | 4 | 2 | Liaison point à point |

---

## 4. Exemples détaillés

### 🔵 Exemple 1 : `/26` (le plus courant en subnetting)

**Exercice : `192.168.1.130/26`**

#### Étape par étape

```
ÉTAPE 1 : Masque
/26 → 255.255.255.192

ÉTAPE 2 : PAS
256 - 192 = 64

ÉTAPE 3 : Multiples de 64
0, 64, 128, 192, 256

130 est entre 128 et 192
→ Adresse réseau = 192.168.1.128

Prochain réseau = 192
Broadcast = 192 - 1 = 191
```

#### Résultat final

| Élément | Valeur |
|---------|--------|
| **Adresse réseau** | `192.168.1.128` |
| **Première IP utilisable** | `192.168.1.129` |
| **Dernière IP utilisable** | `192.168.1.190` |
| **Broadcast** | `192.168.1.191` |
| **Nombre d'IPs utilisables** | 62 |

#### Visualisation

```
Bloc 1 : 192.168.1.0   à 192.168.1.63   (réseau .0)
Bloc 2 : 192.168.1.64  à 192.168.1.127  (réseau .64)
Bloc 3 : 192.168.1.128 à 192.168.1.191  (réseau .128) ← Ton IP 130 est ICI
Bloc 4 : 192.168.1.192 à 192.168.1.255  (réseau .192)
```

---

### 🟢 Exemple 2 : `/27` (petit département)

**Exercice : `10.20.30.200/27`**

#### Étape par étape

```
ÉTAPE 1 : Masque
/27 → 255.255.255.224

ÉTAPE 2 : PAS
256 - 224 = 32

ÉTAPE 3 : Multiples de 32
0, 32, 64, 96, 128, 160, 192, 224, 256

200 est entre 192 et 224
→ Adresse réseau = 10.20.30.192

Prochain réseau = 224
Broadcast = 224 - 1 = 223
```

#### Résultat final

| Élément | Valeur |
|---------|--------|
| **Adresse réseau** | `10.20.30.192` |
| **Première IP utilisable** | `10.20.30.193` |
| **Dernière IP utilisable** | `10.20.30.222` |
| **Broadcast** | `10.20.30.223` |
| **Nombre d'IPs utilisables** | 30 |

#### Tous les sous-réseaux /27 dans 10.20.30.0/24

```
SR 1 : 10.20.30.0   - 10.20.30.31   (30 IPs)
SR 2 : 10.20.30.32  - 10.20.30.63   (30 IPs)
SR 3 : 10.20.30.64  - 10.20.30.95   (30 IPs)
SR 4 : 10.20.30.96  - 10.20.30.127  (30 IPs)
SR 5 : 10.20.30.128 - 10.20.30.159  (30 IPs)
SR 6 : 10.20.30.160 - 10.20.30.191  (30 IPs)
SR 7 : 10.20.30.192 - 10.20.30.223  (30 IPs) ← Ton IP 200 est ICI
SR 8 : 10.20.30.224 - 10.20.30.255  (30 IPs)
```

---

### 🟣 Exemple 3 : `/28` (très petit réseau)

**Exercice : `172.16.5.75/28`**

#### Étape par étape

```
ÉTAPE 1 : Masque
/28 → 255.255.255.240

ÉTAPE 2 : PAS
256 - 240 = 16

ÉTAPE 3 : Multiples de 16
0, 16, 32, 48, 64, 80, 96...

75 est entre 64 et 80
→ Adresse réseau = 172.16.5.64

Prochain réseau = 80
Broadcast = 80 - 1 = 79
```

#### Résultat final

| Élément | Valeur |
|---------|--------|
| **Adresse réseau** | `172.16.5.64` |
| **Première IP utilisable** | `172.16.5.65` |
| **Dernière IP utilisable** | `172.16.5.78` |
| **Broadcast** | `172.16.5.79` |
| **Nombre d'IPs utilisables** | 14 |

---

## 5. Cas particuliers

### ⚠️ CAS 1 : Quand le prochain réseau dépasse 255

**Problème : `192.168.1.200/26`**

```
Réseau actuel = 192
PAS = 64
Prochain réseau = 192 + 64 = 256

❌ 256 n'existe pas dans un octet !
✅ 256 = passage à l'octet suivant = 192.168.2.0

Broadcast = 192.168.2.0 - 1 = 192.168.1.255
```

#### Règle générale

```
Si (dernier octet + PAS) ≥ 256
→ Prochain réseau change d'octet

Exemples :
192.168.1.240 + 32 = 192.168.1.272 → 192.168.2.16
10.0.0.224 + 64 = 10.0.0.288 → 10.0.1.32
```

### ⚠️ CAS 2 : IP au début du réseau

**Problème : `10.0.0.5/26`**

```
PAS = 64
Multiples : 0, 64, 128, 192

5 est entre 0 et 64
→ Adresse réseau = 10.0.0.0

C'est normal ! L'IP donnée peut être proche du début.
```

### ⚠️ CAS 3 : IP à la fin du réseau

**Problème : `172.16.5.190/26`**

```
PAS = 64
Multiples : 0, 64, 128, 192

190 est entre 128 et 192
→ Adresse réseau = 172.16.5.128
→ Broadcast = 172.16.5.191

190 est avant-dernière IP utilisable (191 - 1 = 190)
```

### ⚠️ CAS 4 : Le piège du /30 (liaison point à point)

**Caractéristiques du /30 :**
```
PAS = 4
Seulement 2 IPs utilisables !

Exemple : 10.1.1.0/30
- Réseau : 10.1.1.0
- IP 1 : 10.1.1.1 (routeur A)
- IP 2 : 10.1.1.2 (routeur B)
- Broadcast : 10.1.1.3
```

**Usage :** Uniquement pour relier 2 routeurs.

---

## 6. Astuces et pièges à éviter

### ✅ ASTUCES

#### 1. Mémorisation des masques (mnémotechnique)

```
/24 → 0       (Zéro problème, c'est facile !)
/25 → 128     (La Moitié de 256)
/26 → 192     (128 + 64 = 192)
/27 → 224     (192 + 32 = 224)
/28 → 240     (224 + 16 = 240)
```

#### 2. Vérification rapide

**Pour vérifier si ton calcul est bon :**
```
Première IP + Nombre d'IPs totales - 1 = Broadcast

Exemple /26 :
192.168.1.128 + 64 - 1 = 192.168.1.191 ✅
```

#### 3. Le "truc" du /24

```
Avec un /24, c'est ultra simple :
- Réseau = tout sauf dernier octet → .0
- Broadcast = tout sauf dernier octet → .255

Exemple : 10.50.30.100/24
- Réseau = 10.50.30.0
- Broadcast = 10.50.30.255
```

#### 4. Calculette mentale pour les multiples

**Pour /26 (PAS=64) :**
```
Compte de 64 en 64 sur tes doigts :
0 (poing fermé)
64 (1 doigt)
128 (2 doigts)
192 (3 doigts)
256 (4 doigts)
```

---

### ❌ PIÈGES À ÉVITER

#### 1. Confondre IP donnée et adresse réseau

```
❌ FAUX : 
"L'IP est 192.168.1.50/24 donc le réseau est 192.168.1.50"

✅ CORRECT :
"L'IP est 192.168.1.50/24 donc le réseau est 192.168.1.0"
```

#### 2. Oublier le -2 pour les IPs utilisables

```
❌ FAUX : /26 = 64 IPs utilisables

✅ CORRECT : /26 = 64 - 2 = 62 IPs utilisables
(on enlève réseau et broadcast)
```

#### 3. Mauvais calcul du prochain réseau

```
❌ FAUX : Réseau actuel + réseau suivant
Exemple : 64 + 128 = 192

✅ CORRECT : Réseau actuel + PAS
Exemple : 64 + 64 = 128
```

#### 4. Additionner bêtement quand on dépasse 255

```
❌ FAUX : 192.168.1.240 + 32 = 192.168.1.272

✅ CORRECT : 192.168.1.240 + 32 = 192.168.2.16
(272 - 256 = 16, et on change d'octet)
```

---

## 📝 Fiche de révision rapide

### La checklist des 3 étapes

```
□ ÉTAPE 1 : Je connais le masque pour ce CIDR
□ ÉTAPE 2 : J'ai calculé le PAS (256 - dernier octet)
□ ÉTAPE 3 : J'ai trouvé le bon multiple (liste ou division)
```

### Les 3 valeurs à retenir par cœur

```
/24 → masque 0,   PAS 256
/26 → masque 192, PAS 64
/27 → masque 224, PAS 32
```

### Formule finale

```
Réseau ✓
+ PAS = Prochain réseau
- 1 = Broadcast ✓
```

---

## 🎯 Exercices d'entraînement

### Niveau 1 : Facile (/24)
1. `10.0.0.50/24`
2. `192.168.100.200/24`
3. `172.16.5.75/24`

### Niveau 2 : Moyen (/26)
1. `192.168.1.130/26`
2. `10.50.30.80/26`
3. `172.16.10.200/26`

### Niveau 3 : Avancé (/27)
1. `10.20.30.100/27`
2. `192.168.5.200/27`
3. `172.31.50.75/27`

### Niveau 4 : Expert (cas limites)
1. `192.168.1.250/26` (proche de 255)
2. `10.0.0.5/27` (proche de 0)
3. `172.16.5.240/28` (dépassement d'octet)

---

## 💾 Tableau de référence à imprimer

| CIDR | Masque | PAS | IPs totales | IPs utilisables |
|------|--------|-----|-------------|-----------------|
| /24 | 255.255.255.0 | 256 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 4 | 2 |

---

## 🎓 Conclusion

**La méthode en 3 mots :**
1. **MASQUE** (convertir le CIDR)
2. **PAS** (256 - masque)
3. **MULTIPLES** (encadrer l'IP)

**Avec de la pratique régulière, tu feras ça en 30 secondes !** ⏱️

---

## 📚 Ressources complémentaires

### Sites pour s'entraîner
- [SubnettingPractice.com](http://www.subnettingpractice.com/)
- [Subnet Calculator](https://www.subnet-calculator.com/)

### Outils de vérification
- Calculatrice de sous-réseaux en ligne
- Applications mobiles de subnetting

### Conseils pour le diplôme TSSR
- Pratique quotidienne : 5-10 exercices par jour
- Révision espacée : jour 1, jour 3, jour 7, jour 14
- Focus sur /24, /26, /27 (les plus courants)

---

**Bon courage pour ton TSSR !** 💪🚀

*Document créé pour la formation TSSR - Janvier 2026*
