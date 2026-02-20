# Guide de l'Épreuve Orale/Plateau Technique (1h)

## 📋 Informations générales

**Type d'épreuve** : Épreuve orale/plateau technique  
**Durée** : 1 heure  
**⚠️ IMPORTANT** : Ce n'est PAS forcément le même jour que les deux autres épreuves

---

## ⚠️ CONSIGNES CRITIQUES

### ❌ CE QU'IL NE FAUT JAMAIS DIRE

**"Je n'ai pas vu ça en formation"**

- Passez en premier si vous le sentez
- Habillez-vous comme des professionnels
- **2 jurys seulement** pour vos beaux yeux
- Vous avez **5 minutes** pour lire et comprendre un schéma d'infrastructure
- Comprendre un maximum de concepts/faire attention aux équipements et aux vlans/bien regarder la position du pare-feu
- Après les 5 minutes, les questions vont arriver principalement sur les équipements et tes concepts
- Une fois les questions sur le schéma terminées, on passe aux questions du théoriques
- Revoir les questions réussies comme non

---

## 📝 Structure de l'épreuve

### Phase 1 : Analyse du schéma (5 minutes)

**Plateau technique : 15 minutes**

#### ⚡ Attention à l'attitude !
**Soyez professionnel !**

**Manipulation pratique :**
- Connecter sur un switch et créer un vlan à votre nom puis l'attribuer sur un port
- Réaliser une migration à chaud d'une VM sur un ESXI
- Vérifier et réparer un service Linux
- Adressage

**📌 Conseils pour l'analyse :**
- Ne demandez pas les mots de passe (MDP) aux jurys
- Ne demandez pas si vous avez réussi ou non au jury

---

## 🎯 Compétences évaluées

### 1. Lecture et compréhension d'infrastructure

**Ce qu'on attend de toi :**
- Identifier rapidement les équipements (switchs, routeurs, firewalls, serveurs)
- Comprendre les flux réseau
- Repérer les VLANs et leur segmentation
- Identifier les points critiques (pare-feu, passerelles)

**Checklist rapide (5 min) :**
```
☐ Identifier tous les équipements
☐ Repérer les zones réseau (DMZ, LAN, WAN)
☐ Noter les VLANs et leurs numéros
☐ Localiser le pare-feu et ses règles
☐ Identifier les serveurs et leurs rôles
☐ Comprendre les interconnexions
```

---

### 2. Questions sur les équipements

**Types de questions attendues :**
- Rôle de chaque équipement
- Configuration des VLANs
- Segmentation réseau
- Sécurité (pare-feu, ACL)
- Redondance et haute disponibilité

**Exemples de réponses à préparer :**

#### "À quoi sert ce switch ?"
```
"Ce switch de couche 3 assure :
- La distribution du réseau vers les postes de travail
- La segmentation en VLANs (VLAN 10 = serveurs, VLAN 20 = utilisateurs)
- Le routage inter-VLAN
- La liaison vers le pare-feu pour l'accès Internet"
```

#### "Pourquoi le pare-feu est-il positionné ici ?"
```
"Le pare-feu est positionné entre le réseau interne et Internet pour :
- Filtrer le trafic entrant et sortant
- Protéger le réseau interne des menaces externes
- Appliquer des règles de sécurité strictes
- Permettre le NAT pour les utilisateurs internes"
```

---

### 3. Questions théoriques

Après l'analyse du schéma, le jury pose des questions théoriques variées.

**Domaines couverts :**
- Réseau (TCP/IP, subnetting, routage, VLANs)
- Systèmes (Windows Server, Linux, Active Directory)
- Virtualisation (VMware, Hyper-V)
- Sécurité (pare-feu, VPN, authentification)
- Services (DNS, DHCP, AD, GPO)

**Revoir particulièrement :**
- Les questions réussies lors des autres épreuves
- Les questions ratées (comprendre pourquoi)
- Les bases essentielles (modèle OSI, adressage IP, services réseau)

---

## 🛠️ Phase pratique (Plateau technique - 15 min)

### Manipulation 1 : Configuration de VLAN sur switch

**Tâche :** Connecter sur un switch et créer un vlan à votre nom puis l'attribuer sur un port

**Étapes détaillées :**

#### Connexion au switch
```bash
# Via console (câble série)
# ou via SSH
ssh admin@192.168.1.1
```

#### Création du VLAN
```
# Mode privilégié
enable
# Mode configuration
configure terminal

# Créer le VLAN (exemple : VLAN 50 nommé "Rimk")
vlan 50
name Rimk
exit
```

#### Attribution du VLAN à un port
```
# Sélectionner l'interface (exemple : port 10)
interface fastethernet 0/10
# ou
interface gigabitethernet 0/10

# Configurer le port en mode access
switchport mode access
switchport access vlan 50

# Activer le port
no shutdown
exit
```

#### Vérification
```
# Vérifier les VLANs créés
show vlan brief

# Vérifier la configuration du port
show interfaces fastethernet 0/10 switchport
```

**Points de vigilance :**
- ✅ Bien noter le numéro de VLAN demandé
- ✅ Respecter la syntaxe exacte (selon constructeur : Cisco, HP, etc.)
- ✅ Vérifier la configuration après application
- ✅ Ne pas perturber les VLANs existants

---

### Manipulation 2 : Migration à chaud d'une VM (vMotion)

**Tâche :** Réaliser une migration à chaud d'une VM sur un ESXI

**Prérequis pour vMotion :**
- VM allumée (powered on)
- Hosts ESXi dans le même cluster
- Réseau vMotion configuré
- Stockage partagé (SAN, NFS) ou Storage vMotion
- Compatibilité CPU entre hosts

**Étapes via vSphere Client :**

1. **Sélectionner la VM**
   - Clic droit sur la VM à migrer
   - Sélectionner "Migrate..."

2. **Choisir le type de migration**
   - "Change compute resource only" (vMotion simple)
   - ou "Change both compute resource and storage" (si nécessaire)

3. **Sélectionner l'hôte de destination**
   - Choisir l'ESXi de destination
   - Vérifier la compatibilité (indicateurs verts)

4. **Sélectionner le réseau** (si demandé)
   - Garder la configuration réseau actuelle
   - ou mapper vers nouveaux réseaux

5. **Définir la priorité**
   - "Schedule vMotion with high priority" (recommandé)

6. **Valider et lancer**
   - Vérifier le résumé
   - Cliquer "Finish"

**Vérification :**
- La VM reste accessible pendant la migration
- Aucune interruption de service
- Vérifier que la VM tourne sur le nouvel hôte

**En ligne de commande (PowerCLI) :**
```powershell
# Se connecter au vCenter
Connect-VIServer -Server vcenter.local

# Migrer la VM
Get-VM -Name "MaVM" | Move-VM -Destination (Get-VMHost "esxi02.local")
```

**Points de vigilance :**
- ✅ Vérifier que le vMotion est activé
- ✅ Confirmer le stockage partagé
- ✅ S'assurer de la compatibilité CPU
- ✅ Ne pas migrer vers un hôte surchargé

---

### Manipulation 3 : Vérifier et réparer un service Linux

**Tâche :** Vérifier et réparer un service Linux

**Exemple : Service Apache ne démarre pas**

#### 1. Vérifier l'état du service
```bash
# Vérifier le statut
sudo systemctl status apache2
# ou (selon la distribution)
sudo systemctl status httpd

# Vérifier si le service est activé au démarrage
sudo systemctl is-enabled apache2
```

#### 2. Analyser les erreurs
```bash
# Consulter les logs système
sudo journalctl -u apache2 -n 50

# Consulter les logs Apache
sudo tail -f /var/log/apache2/error.log

# Vérifier la configuration
sudo apache2ctl configtest
# ou
sudo apachectl configtest
```

#### 3. Diagnostiquer les problèmes courants

**Port déjà utilisé :**
```bash
# Vérifier quel processus utilise le port 80
sudo netstat -tulpn | grep :80
# ou
sudo ss -tulpn | grep :80
# ou
sudo lsof -i :80

# Si un autre processus l'utilise, l'arrêter
sudo systemctl stop <service-conflictuel>
```

**Erreur de configuration :**
```bash
# Tester la configuration
sudo apache2ctl configtest

# Localiser l'erreur (exemple ligne 45 du fichier de config)
sudo nano /etc/apache2/apache2.conf

# Corriger l'erreur (virgule manquante, directive invalide, etc.)
```

**Permissions incorrectes :**
```bash
# Vérifier les permissions du DocumentRoot
ls -la /var/www/html

# Corriger si nécessaire
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

**Module manquant :**
```bash
# Lister les modules activés
sudo apache2ctl -M

# Activer un module (exemple : rewrite)
sudo a2enmod rewrite

# Redémarrer Apache
sudo systemctl restart apache2
```

#### 4. Réparer le service
```bash
# Corriger la configuration
sudo nano /etc/apache2/apache2.conf

# Tester la nouvelle configuration
sudo apache2ctl configtest

# Si OK, redémarrer le service
sudo systemctl restart apache2

# Vérifier que c'est bien démarré
sudo systemctl status apache2

# Activer au démarrage si nécessaire
sudo systemctl enable apache2
```

#### 5. Vérification finale
```bash
# Tester l'accès web
curl http://localhost

# ou
curl http://127.0.0.1

# Vérifier les processus Apache
ps aux | grep apache2
```

**Autres services courants à connaître :**

**SSH (sshd) :**
```bash
sudo systemctl status sshd
sudo journalctl -u sshd
sudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
```

**DNS (named/bind9) :**
```bash
sudo systemctl status named  # CentOS/RHEL
sudo systemctl status bind9  # Debian/Ubuntu
sudo named-checkconf  # Vérifier la config
sudo named-checkzone example.com /etc/bind/db.example.com
```

**DHCP (isc-dhcp-server) :**
```bash
sudo systemctl status isc-dhcp-server
sudo nano /etc/dhcp/dhcpd.conf
sudo dhcpd -t  # Tester la config
```

**Points de vigilance :**
- ✅ Toujours vérifier les logs avant de modifier quoi que ce soit
- ✅ Tester la configuration avant de redémarrer
- ✅ Faire une sauvegarde du fichier de config avant modification
- ✅ Comprendre l'erreur plutôt que d'appliquer des solutions aléatoires

---

### Manipulation 4 : Adressage

**Tâche :** Questions ou exercices sur l'adressage IP

**Exemples de tâches possibles :**

#### Configurer une adresse IP statique

**Windows Server :**
```powershell
# Via PowerShell
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.1.100 -PrefixLength 24 -DefaultGateway 192.168.1.1

Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses 192.168.1.10,192.168.1.11
```

**Ou via GUI :**
1. Panneau de configuration → Centre réseau
2. Modifier les paramètres de la carte
3. Propriétés IPv4
4. Configurer manuellement

**Linux :**
```bash
# Méthode moderne (netplan - Ubuntu 18.04+)
sudo nano /etc/netplan/01-netcfg.yaml

# Contenu :
network:
  version: 2
  ethernets:
    ens33:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [192.168.1.10, 8.8.8.8]

# Appliquer
sudo netplan apply

# Méthode traditionnelle (interfaces)
sudo nano /etc/network/interfaces

# Contenu :
auto ens33
iface ens33 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 192.168.1.10 8.8.8.8

# Redémarrer le réseau
sudo systemctl restart networking
```

#### Calculer un sous-réseau (subnetting)

**Exemple de question :**
"Vous devez créer 4 sous-réseaux à partir de 192.168.10.0/24. Donnez les plages."

**Réponse :**
```
Réseau de base : 192.168.10.0/24 (256 adresses)
Besoin de 4 sous-réseaux → /26 (64 adresses chacun)

Sous-réseau 1 : 192.168.10.0/26
  - Plage : 192.168.10.1 à 192.168.10.62
  - Broadcast : 192.168.10.63

Sous-réseau 2 : 192.168.10.64/26
  - Plage : 192.168.10.65 à 192.168.10.126
  - Broadcast : 192.168.10.127

Sous-réseau 3 : 192.168.10.128/26
  - Plage : 192.168.10.129 à 192.168.10.190
  - Broadcast : 192.168.10.191

Sous-réseau 4 : 192.168.10.192/26
  - Plage : 192.168.10.193 à 192.168.10.254
  - Broadcast : 192.168.10.255
```

#### Diagnostiquer un problème réseau

**Scénario :** "Un PC ne peut pas accéder à Internet. Diagnostiquez."

**Méthodologie de dépannage :**
```bash
# 1. Vérifier la configuration IP
ipconfig /all  # Windows
ip addr show   # Linux

# 2. Tester la connectivité locale
ping 127.0.0.1  # Loopback (carte réseau fonctionne)

# 3. Tester la passerelle
ping 192.168.1.1  # Passerelle par défaut

# 4. Tester la résolution DNS
nslookup google.com
# ou
dig google.com

# 5. Tester Internet
ping 8.8.8.8  # Google DNS (sans résolution DNS)
ping google.com  # Avec résolution DNS

# 6. Vérifier le routage
tracert google.com  # Windows
traceroute google.com  # Linux
```

**Diagnostic par élimination :**
```
Loopback OK, Passerelle KO → Problème réseau local (câble, VLAN, switch)
Passerelle OK, DNS KO → Problème de configuration DNS
DNS OK, Internet KO → Problème de routage/pare-feu
```

---

## 🎓 Conseils stratégiques

### Pendant les 5 minutes d'analyse

1. **Gardez votre calme**
   - Respirez profondément
   - Lisez le schéma méthodiquement

2. **Identifiez les éléments clés**
   - Combien de VLANs ?
   - Où est le pare-feu ?
   - Quels sont les serveurs ?
   - Quelle est la topologie ?

3. **Préparez des questions mentalement**
   - Pourquoi cette architecture ?
   - Quels sont les points de défaillance ?
   - Comment améliorer la sécurité ?

### Pendant les questions

1. **Écoutez attentivement**
   - Ne coupez pas le jury
   - Demandez des précisions si nécessaire (poliment)

2. **Structurez vos réponses**
   - Introduction brève
   - Développement structuré
   - Conclusion si pertinent

3. **Soyez honnête**
   - Si vous ne savez pas, dites-le
   - Proposez un raisonnement logique
   - Montrez votre capacité d'analyse

4. **Restez professionnel**
   - Langage technique approprié
   - Attitude confiante mais humble
   - Pas de familiarité

### Pendant la phase pratique

1. **Annoncez ce que vous faites**
   - "Je vais d'abord vérifier..."
   - "Je vais maintenant configurer..."
   - Commentez vos actions

2. **Vérifiez chaque étape**
   - Ne passez pas à l'étape suivante sans vérifier
   - Utilisez les commandes de vérification

3. **Ne paniquez pas en cas d'erreur**
   - Analysez calmement
   - Cherchez dans la bonne direction
   - Demandez un indice si vraiment bloqué

---

## 📚 Révisions prioritaires

### Réseau (priorité haute)
- ✅ Modèle OSI (7 couches)
- ✅ Adressage IP et subnetting
- ✅ VLANs (configuration, trunk, access)
- ✅ Routage (statique, dynamique)
- ✅ Services réseau (DNS, DHCP, NAT)
- ✅ Équipements (switch, routeur, firewall, différences)

### Systèmes Windows (priorité haute)
- ✅ Active Directory (structure, rôles FSMO)
- ✅ GPO (création, application, héritage)
- ✅ DNS et DHCP Windows
- ✅ Partages réseau et permissions NTFS
- ✅ Installation et configuration de rôles

### Systèmes Linux (priorité moyenne)
- ✅ Gestion des services (systemctl)
- ✅ Configuration réseau
- ✅ Permissions et propriétaires
- ✅ Services essentiels (SSH, Apache, DNS, DHCP)
- ✅ Logs et dépannage

### Virtualisation (priorité moyenne)
- ✅ VMware ESXi / vSphere
- ✅ vMotion et migrations
- ✅ Stockage partagé
- ✅ Snapshots et sauvegardes
- ✅ Réseaux virtuels

### Sécurité (priorité moyenne)
- ✅ Pare-feu (règles, zones)
- ✅ VPN (types, protocoles)
- ✅ Authentification (RADIUS, LDAP)
- ✅ Chiffrement (SSL/TLS, IPsec)
- ✅ Bonnes pratiques

---

## 🎯 Checklist finale avant l'épreuve

### La veille
```
☐ Revoir les schémas d'infrastructure types
☐ Refaire des exercices de subnetting
☐ Réviser les commandes de base (switch, Linux, Windows)
☐ Relire les questions théoriques réussies ET ratées
☐ Préparer ses vêtements professionnels
☐ Vérifier l'heure et le lieu de l'épreuve
☐ Bien dormir !
```

### Le jour J
```
☐ Arriver 15 minutes en avance
☐ Tenue professionnelle
☐ Attitude calme et confiante
☐ Éteindre son téléphone
☐ Prendre de quoi noter (stylo + papier si autorisé)
```

### Pendant l'épreuve
```
☐ Écouter attentivement les consignes
☐ Utiliser les 5 minutes d'analyse à fond
☐ Répondre de manière structurée
☐ Annoncer ce qu'on fait pendant la pratique
☐ Vérifier chaque manipulation
☐ Rester professionnel du début à la fin
```

---

## 💡 Phrases types à utiliser

### Pour gagner du temps de réflexion
- "C'est une excellente question. Laissez-moi structurer ma réponse..."
- "Si je comprends bien, vous me demandez... [reformuler]"
- "Plusieurs aspects sont à considérer ici..."

### Pour montrer sa méthodologie
- "Je procéderais en plusieurs étapes..."
- "La première chose à vérifier serait..."
- "Pour diagnostiquer ce problème, je commencerais par..."

### En cas de doute
- "Je ne suis pas totalement certain, mais selon ma compréhension..."
- "Ce n'est pas un domaine que j'ai beaucoup pratiqué, mais je pense que..."
- "Je préférerais vérifier la documentation officielle avant de répondre définitivement..."

### Pendant la manipulation
- "Je vais maintenant créer le VLAN..."
- "Je vérifie que la configuration est correcte..."
- "Comme on peut le voir ici, le service est bien démarré..."

---

## ⏱️ Gestion du temps (1h)

**Répartition suggérée :**
```
00:00 - 00:05 : Analyse du schéma (imposé)
00:05 - 00:20 : Questions sur le schéma (15 min)
00:20 - 00:35 : Plateau technique (15 min)
00:35 - 00:55 : Questions théoriques (20 min)
00:55 - 01:00 : Questions du jury / conclusion (5 min)
```

**Le jury peut ajuster selon tes performances !**

---

## 🚀 État d'esprit gagnant

### Ce que le jury évalue vraiment

1. **Compétences techniques** (40%)
   - Connaissance théorique
   - Capacité pratique
   - Rigueur méthodologique

2. **Capacité d'analyse** (30%)
   - Compréhension d'une infrastructure
   - Logique de dépannage
   - Vision d'ensemble

3. **Communication** (20%)
   - Clarté des explications
   - Vocabulaire technique
   - Structuration des réponses

4. **Attitude professionnelle** (10%)
   - Confiance sans arrogance
   - Gestion du stress
   - Honnêteté intellectuelle

### Les erreurs qui pardonnent
- ✅ Ne pas connaître une commande précise
- ✅ Hésiter sur un détail technique
- ✅ Demander une précision au jury

### Les erreurs qui ne pardonnent pas
- ❌ "Je n'ai pas vu ça en formation"
- ❌ Attitude désinvolte ou arrogante
- ❌ Ne pas savoir les bases fondamentales
- ❌ Tenue inappropriée
- ❌ Mentir ou inventer des réponses

---

## 📖 Ressources de dernière minute

### Commandes à revoir absolument

**Switch Cisco :**
```
enable
configure terminal
show running-config
show vlan brief
show interfaces status
copy running-config startup-config
```

**Linux (systemd) :**
```
systemctl status <service>
systemctl start/stop/restart <service>
systemctl enable/disable <service>
journalctl -u <service>
systemctl list-units --type=service
```

**Windows PowerShell :**
```
Get-Service
Start-Service
Stop-Service
Restart-Service
Get-NetIPConfiguration
Test-Connection
Resolve-DnsName
```

**Diagnostic réseau :**
```
ping
tracert / traceroute
nslookup / dig
ipconfig / ifconfig / ip addr
netstat / ss
route print / ip route
```

---

## 🎬 Débrief

### Après l'épreuve

**Prenez vos notes pour le jury :**
- Préparez vos documents pour l'avenir
- OUI vous avez des questions pour le jury

**Questions pertinentes à poser au jury :**
1. "Quels sont les points que je devrais améliorer ?"
2. "Y a-t-il des technologies spécifiques que vous me recommandez d'approfondir ?"
3. "Comment voyez-vous l'évolution du métier de TSSR ?"

**Quel que soit le résultat :**
- Tu as acquis beaucoup de connaissances
- Cette expérience te servira
- L'important est d'apprendre de chaque épreuve

---

## ✨ Message de motivation

Tu as travaillé dur pour arriver jusqu'ici. Cette épreuve est l'occasion de montrer tout ce que tu as appris. Rappelle-toi :

1. **Tu connais plus de choses que tu ne le penses**
2. **Le jury veut que tu réussisses**
3. **Une erreur n'est pas éliminatoire**
4. **Ton attitude compte autant que tes connaissances**

**Respire, reste toi-même, et montre-leur ce dont tu es capable !**

---

## 🎯 Derniers conseils du prof

### Le secret pour réussir

**CALM :**
- **C**ompréhension (des questions)
- **A**nalyse (méthodique)
- **L**ogique (raisonnement structuré)
- **M**éthodologie (approche professionnelle)

**STAR :**
- **S**ituation (comprendre le contexte)
- **T**âche (identifier ce qu'on attend de toi)
- **A**ction (expliquer ce que tu ferais)
- **R**ésultat (conclure sur l'objectif atteint)

### En résumé

Tu as les compétences. Tu as la formation. Tu as l'expérience de développeur qui te donne une logique solide. Maintenant, c'est le moment de montrer que tu peux être un excellent TSSR !

**Fais-toi confiance, reste professionnel, et vas-y à fond !** 💪

Bonne chance ! Tu vas assurer ! 🚀

---

*Document créé pour Rimk - TSSR Nextformation 2024-2025*
