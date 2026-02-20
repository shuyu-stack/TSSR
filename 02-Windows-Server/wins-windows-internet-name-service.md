# WINS - Windows Internet Name Service

> 📚 **Module :** Windows Server - Services réseau (obsolète)
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 2-3 heures
> 🎯 **Niveau :** Complémentaire (au programme mais obsolète)
> 🎓 **Formateur virtuel :** Architecte réseau avec +20 ans d'expérience
> ⚠️ **Note :** Technologie obsolète, remplacée par DNS

---

## 👨‍🏫 Message de votre formateur

> **Soyons francs : WINS est une technologie OBSOLÈTE depuis Windows 2000.**
>
> Pourquoi ce cours alors ? Parce que c'est **encore au programme de l'examen TSSR** et que vous pourriez tomber sur de vieux réseaux qui l'utilisent encore.
>
> **La réalité du terrain (20 ans d'expérience) :**
> - 95% des entreprises modernes n'utilisent PLUS WINS
> - DNS a complètement remplacé WINS
> - Vous verrez peut-être WINS dans de **très vieilles infrastructures** (PME qui n'ont jamais migré, administrations publiques avec du matériel ancien)
>
> **À l'examen :**
> - Il y a environ 20% de chances qu'une question porte sur WINS
> - Généralement : "Quelle est la différence entre DNS et WINS ?"
> - Ou : "À quoi servait WINS ?"
>
> **Ce que vous devez retenir :**
> - WINS = résolution de noms NetBIOS → Adresse IP
> - DNS = résolution de noms FQDN → Adresse IP
> - WINS a été remplacé par DNS
> - Si vous voyez WINS en entreprise → Recommandez de migrer vers DNS
>
> **Temps conseillé sur ce cours : 30 minutes de lecture, pas besoin de pratiquer en profondeur.**

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Qu'est-ce que WINS ?](#-quest-ce-que-wins)
- [Différence entre WINS et DNS](#-différence-entre-wins-et-dns)
- [Installation de WINS](#-installation-de-wins-pour-info)
- [Configuration de base](#-configuration-de-base)
- [Quand désactiver WINS](#-quand-désactiver-wins)
- [Questions d'examen types](#-questions-dexamen-types)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ **Expliquer** ce qu'est WINS et à quoi il servait
- ✅ **Différencier** WINS et DNS
- ✅ **Comprendre** pourquoi WINS est obsolète
- ✅ **Répondre** aux questions d'examen sur WINS
- ✅ **Recommander** la migration de WINS vers DNS en entreprise

---

## 📚 Qu'est-ce que WINS ?

### Définition

**WINS** (Windows Internet Name Service) est un **service de résolution de noms NetBIOS** développé par Microsoft.

**En langage simple :**
WINS traduit les **noms NetBIOS** (noms courts d'ordinateurs, 15 caractères max) en **adresses IP**.

### Exemple concret

**Avant (années 1990-2000) avec WINS :**
```
Vous tapez :    \\SERVEUR1
WINS traduit :  \\SERVEUR1 → 192.168.1.10
Résultat :      Vous accédez au serveur
```

**Aujourd'hui avec DNS :**
```
Vous tapez :    \\serveur1.solaris.local
DNS traduit :   serveur1.solaris.local → 192.168.1.10
Résultat :      Vous accédez au serveur (mais avec un nom complet)
```

### Noms NetBIOS vs Noms DNS

| Caractéristique | NetBIOS (WINS) | DNS |
|----------------|----------------|-----|
| **Longueur** | 15 caractères max | 255 caractères max |
| **Format** | PC-COMPTA (nom simple) | pc-compta.solaris.local (FQDN) |
| **Hiérarchie** | ❌ Non (plat) | ✅ Oui (domaines, sous-domaines) |
| **Internet** | ❌ Non (réseau local uniquement) | ✅ Oui (fonctionne sur internet) |
| **Technologie** | ⚰️ Obsolète (1980s) | ✅ Moderne (toujours utilisé) |

### Pourquoi WINS a été créé ?

**Contexte historique (années 1980-90) :**
- Windows 3.1, Windows 95, Windows NT utilisaient **NetBIOS** pour communiquer sur le réseau
- Il n'y avait pas de DNS intégré à Windows
- Les ordinateurs devaient "se trouver" sur le réseau avec des noms courts

**Problème sans WINS :**
```
PC1 cherche PC2 sur le réseau
→ PC1 envoie un "broadcast" (message à tout le monde) : "Qui est PC2 ?"
→ Sur un grand réseau (500 PCs) : Trafic broadcast ÉNORME (réseau saturé)
```

**Solution avec WINS :**
```
PC1 cherche PC2
→ PC1 demande au serveur WINS : "Quelle est l'IP de PC2 ?"
→ WINS répond : "PC2 = 192.168.1.50"
→ Pas de broadcast, réseau fluide
```

### Pourquoi WINS est obsolète ?

**3 raisons :**

1. **DNS est bien meilleur**
   - Fonctionne sur internet
   - Hiérarchique (domaines, sous-domaines)
   - Standard universel (pas seulement Microsoft)

2. **Windows supporte DNS nativement depuis Windows 2000**
   - Plus besoin de NetBIOS
   - Active Directory utilise DNS (pas WINS)

3. **NetBIOS est une faille de sécurité**
   - Protocole non chiffré
   - Vulnérable aux attaques (man-in-the-middle, spoofing)
   - Recommandé de désactiver NetBIOS sur les réseaux modernes

---

## 🔷 Différence entre WINS et DNS

### Tableau comparatif complet

| Critère | WINS | DNS |
|---------|------|-----|
| **Nom résolu** | NetBIOS (SERVEUR1) | FQDN (serveur1.solaris.local) |
| **Port** | 137 (UDP), 138 (UDP), 139 (TCP) | 53 (UDP/TCP) |
| **Base de données** | Dynamique (auto-enregistrement) | Statique + dynamique (avec DDNS) |
| **Hiérarchie** | ❌ Plat | ✅ Hiérarchique |
| **Standard** | 🪟 Propriétaire Microsoft | 🌐 Standard internet (RFC) |
| **Sécurité** | ⚠️ Faible (non chiffré) | ✅ Meilleure (DNSSEC possible) |
| **Utilisation** | ⚰️ Obsolète | ✅ Universel |
| **Intégration AD** | ❌ Non | ✅ Oui (obligatoire) |

### Exemples de résolution

**WINS :**
```
Nom NetBIOS : PC-COMPTA-01
WINS répond :  192.168.1.100
```

**DNS :**
```
Nom FQDN :    pc-compta-01.solaris.local
DNS répond :  192.168.1.100
```

### Quand utiliser WINS (rare aujourd'hui)

**Vous DEVEZ utiliser WINS si :**
- Vous avez de **très vieux ordinateurs** (Windows NT 4.0, Windows 98) qui ne supportent pas DNS
- Vous avez des **applications legacy** (anciennes) qui utilisent NetBIOS
- Vous avez des **segments réseau isolés** sans DNS

**Dans 99% des cas modernes : Utilisez DNS, pas WINS.**

---

## 🔷 Installation de WINS (pour info)

> ⚠️ **Note :** Vous n'avez probablement PAS besoin d'installer WINS. Cette section est uniquement pour l'examen.

### Installer le rôle WINS

**Méthode GUI :**
1. Server Manager → Manage → Add Roles and Features
2. Cochez **WINS Server**
3. Install

**Méthode PowerShell :**
```powershell
Install-WindowsFeature WINS -IncludeManagementTools
```

### Ouvrir la console WINS

```
1. Server Manager → Tools → WINS
2. Ou : Taper "wins" dans la recherche Windows
```

---

## 🔷 Configuration de base

### Inscrire un client WINS (obsolète)

**Sur un poste Windows client :**

1. Panneau de configuration → Centre Réseau et partage
2. Modifier les paramètres de la carte
3. Clic droit sur la carte réseau → Propriétés
4. Double-clic sur "Protocole Internet version 4 (TCP/IPv4)"
5. Cliquer sur **Avancé**
6. Onglet **WINS**
7. Ajouter l'adresse IP du serveur WINS
8. OK

**Note :** Aujourd'hui, cette étape est inutile si vous avez un DNS fonctionnel.

### Vérifier les enregistrements WINS

**Console WINS :**
1. Ouvrir WINS
2. Clic droit sur le serveur → **Show Database**
3. Vous voyez tous les noms NetBIOS enregistrés avec leurs adresses IP

---

## 🔷 Quand désactiver WINS ?

### Vous DEVEZ désactiver WINS si :

✅ **Tous vos postes sont Windows 10/11**
✅ **Tous vos serveurs sont Windows Server 2016+**
✅ **Vous utilisez Active Directory avec DNS**
✅ **Vous n'avez pas d'applications legacy nécessitant NetBIOS**

### Comment désactiver NetBIOS (recommandé pour la sécurité)

**Sur chaque poste client :**

1. Panneau de configuration → Centre Réseau et partage
2. Modifier les paramètres de la carte
3. Clic droit sur la carte → Propriétés
4. Double-clic sur "Protocole Internet version 4 (TCP/IPv4)"
5. Avancé → Onglet **WINS**
6. Sélectionner **Désactiver NetBIOS sur TCP/IP** (Disable NetBIOS over TCP/IP)
7. OK

**Via GPO (pour tous les postes d'un coup) :**
- Computer Configuration → Administrative Templates → Network → DNS Client
- "Turn off Multicast Name Resolution" : **Enabled**

> 💡 **Astuce de pro (PowerShell) :**
> ```powershell
> # Désactiver NetBIOS sur toutes les cartes réseau
> $adapters = Get-WmiObject Win32_NetworkAdapterConfiguration | Where-Object {$_.IPEnabled -eq $true}
> foreach ($adapter in $adapters) {
>     $adapter.SetTcpipNetbios(2)  # 2 = Disable
> }
>
> # Vérifier
> Get-WmiObject Win32_NetworkAdapterConfiguration | Where-Object {$_.IPEnabled} | Select-Object Description, TcpipNetbiosOptions
> # 0 = Default (DHCP decide), 1 = Enabled, 2 = Disabled
> ```

---

## 🔷 Questions d'examen types

### Question 1 : Quelle est la différence entre WINS et DNS ?

**Réponse attendue :**
- WINS résout les noms **NetBIOS** (courts, 15 caractères) en adresses IP
- DNS résout les noms **FQDN** (longs, hiérarchiques) en adresses IP
- WINS est **obsolète** et a été remplacé par DNS
- DNS est **standard internet**, WINS est propriétaire Microsoft

---

### Question 2 : À quoi servait WINS ?

**Réponse attendue :**
WINS servait à résoudre les noms NetBIOS en adresses IP pour éviter le trafic broadcast sur les réseaux Windows (années 1990-2000). Aujourd'hui, DNS le remplace complètement.

---

### Question 3 : Dois-je installer WINS sur un réseau Windows moderne ?

**Réponse attendue :**
**Non.** Sur un réseau moderne avec Active Directory et DNS, WINS est inutile et même déconseillé pour des raisons de sécurité. Il faut **désactiver NetBIOS** sur les cartes réseau.

---

### Question 4 : Scénario - Vous arrivez dans une entreprise qui utilise encore WINS. Que recommandez-vous ?

**Réponse attendue :**
1. **Audit :** Vérifier s'il reste des applications legacy nécessitant NetBIOS
2. **Migration :** Configurer DNS pour remplacer WINS
3. **Test :** Tester que toutes les applications fonctionnent avec DNS seul
4. **Désactivation :** Désactiver NetBIOS sur les cartes réseau via GPO
5. **Arrêt WINS :** Éteindre le serveur WINS
6. **Documentation :** Noter la migration dans la documentation IT

---

## 📝 Résumé pour l'examen (à retenir en 2 minutes)

### Ce qu'il faut absolument savoir sur WINS :

✅ **WINS = Windows Internet Name Service**
✅ **Fonction :** Résolution de noms NetBIOS → IP
✅ **NetBIOS :** Noms courts (15 caractères max), pas de hiérarchie
✅ **Obsolète :** Remplacé par DNS depuis Windows 2000
✅ **Différence DNS/WINS :**
   - DNS : FQDN (serveur1.solaris.local)
   - WINS : NetBIOS (SERVEUR1)
✅ **Recommandation :** Désactiver NetBIOS et utiliser DNS
✅ **Sécurité :** NetBIOS/WINS = Faille de sécurité (non chiffré)

### Phrase à retenir pour l'examen :

> "WINS était utilisé pour résoudre les noms NetBIOS en adresses IP sur les anciens réseaux Windows, mais il est obsolète depuis Windows 2000 et a été remplacé par DNS qui offre plus de fonctionnalités et une meilleure sécurité."

---

## 📚 Ressources complémentaires

### Documentation Microsoft (archives)
- [What is WINS?](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731125(v=ws.10)) (archive)
- [Disabling NetBIOS over TCP/IP](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/disable-netbios-tcp-ip)

### Articles recommandés
- "Why WINS is obsolete and should be disabled" (recherche Google)
- "Migrating from WINS to DNS" (IT-Connect, TechNet)

---

## 📝 Message final de votre formateur

> **WINS en une phrase : Vous n'en aurez probablement jamais besoin, mais sachez que ça existe pour l'examen.**
>
> **En 20 ans de carrière :**
> - J'ai installé WINS : 5 fois (toutes avant 2005)
> - J'ai désactivé WINS : 50 fois (migrations de vieux réseaux)
> - J'ai recommandé d'installer WINS : 0 fois
>
> **À l'examen :**
> - Si on vous demande "Qu'est-ce que WINS ?" → Vous savez répondre maintenant
> - Si on vous demande "Devez-vous installer WINS ?" → La réponse est NON (sauf application legacy)
>
> **En entreprise :**
> - Si vous voyez WINS en production → Proposez une migration vers DNS
> - Désactivez NetBIOS pour améliorer la sécurité
>
> **Temps passé sur ce cours : 15 minutes de lecture suffisent.**
>
> **Passez maintenant au prochain cours (plus important) ! 🚀**

---

<div align="center">

**Cours suivant :** [Visio - Schémas réseau](./visio-schemas-reseau.md)

[⬅️ Retour au sommaire](../README.md) | [📊 Progression](../progression.md)

---

**💡 Retenez : WINS = Obsolète, utilisez DNS !**

</div>
