Voici une aide claire et structurée en **Markdown (.md)** pour les calculs de sous-réseaux, adaptée à un apprenti **TSSR** 👇

---

# 📘 Aide – Calculs de Sous-Réseaux (TSSR)

## 🔹 1. Rappels de base

### Adresse IPv4

* Format : `192.168.1.1`
* Composée de **4 octets (8 bits chacun)** → total **32 bits**

### Masque de sous-réseau

* Exemple : `255.255.255.0`
* Notation CIDR : `/24`

| CIDR | Masque          | Nb d’hôtes |
| ---- | --------------- | ---------- |
| /24  | 255.255.255.0   | 254        |
| /25  | 255.255.255.128 | 126        |
| /26  | 255.255.255.192 | 62         |
| /27  | 255.255.255.224 | 30         |
| /28  | 255.255.255.240 | 14         |
| /29  | 255.255.255.248 | 6          |
| /30  | 255.255.255.252 | 2          |

👉 Formule :

```
Nombre d’hôtes = 2^(bits hôte) - 2
```

---

## 🔹 2. Identifier un réseau

### Exemple :

IP : `192.168.1.130/26`

### Étapes :

1. Masque `/26` → 255.255.255.192
2. Taille de bloc :

```
256 - 192 = 64
```

👉 Sous-réseaux possibles :

* 192.168.1.0
* 192.168.1.64
* 192.168.1.128
* 192.168.1.192

👉 130 ∈ [128 - 191]

### Résultat :

* 🌐 Réseau : `192.168.1.128`
* 📡 Broadcast : `192.168.1.191`
* 👥 Hôtes : `192.168.1.129 → 192.168.1.190`

---

## 🔹 3. Méthode rapide (terrain TSSR)

### Étapes simples :

1. Trouver le masque
2. Calculer :

```
Bloc = 256 - valeur du masque
```

3. Lister les réseaux
4. Trouver où tombe l’IP
5. Déduire :

   * Adresse réseau
   * Broadcast
   * Plage d’hôtes

---

## 🔹 4. Tableau des blocs utiles

| Masque | Bloc |
| ------ | ---- |
| 128    | 128  |
| 192    | 64   |
| 224    | 32   |
| 240    | 16   |
| 248    | 8    |
| 252    | 4    |

---

## 🔹 5. Exemple complet

### IP : `10.0.5.70/27`

1. Masque → 255.255.255.224
2. Bloc :

```
256 - 224 = 32
```

👉 Sous-réseaux :

* 10.0.5.0
* 10.0.5.32
* 10.0.5.64
* 10.0.5.96

👉 70 ∈ [64 - 95]

### Résultat :

* 🌐 Réseau : `10.0.5.64`
* 📡 Broadcast : `10.0.5.95`
* 👥 Hôtes : `10.0.5.65 → 10.0.5.94`

---

## 🔹 6. Astuce examen / terrain

✔ Toujours penser **"bloc"**
✔ Travailler uniquement sur **l’octet concerné**
✔ Apprendre les valeurs :

```
128 / 64 / 32 / 16 / 8 / 4
```

---

## 🔹 7. Mémo rapide

```
Réseau = début du bloc
Broadcast = fin du bloc
Hôtes = entre les deux
```

---

## 🔹 8. Exercice rapide

Trouver :

* Réseau
* Broadcast
* Plage d’hôtes

### IP : `192.168.10.45/28`

<details>
<summary>Correction</summary>

* Bloc = 16
* Réseaux : 0, 16, 32, 48…
* 45 ∈ [32 - 47]

👉 Réseau : 192.168.10.32
👉 Broadcast : 192.168.10.47
👉 Hôtes : 33 → 46

</details>

---

## 🚀 Conclusion

Avec la méthode du **bloc**, tu peux résoudre **90% des exercices rapidement**, ce qui est essentiel en TSSR (examen + terrain).

---

Si tu veux, je peux aussi te faire :

* une **fiche ultra condensée (1 page PDF)**
* des **exercices corrigés type examen**
* ou un **outil automatique pour t’entraîner** 👍
