# Déploiement GLPI Agent via GPO — Récapitulatif

## Contexte

| Élément | Valeur |
|---|---|
| Serveur GLPI | Debian 12 + Docker, IP `192.168.230.165` |
| Active Directory | Windows Server, domaine `glpi.local` |
| Client | Windows 10, IP `192.168.230.30` |
| Agent GLPI | v1.16 x64 |
| Plugin | GLPI Inventory v1.6.5 |

---

## 1. Préparer le .msi dans le SYSVOL

Déposer le fichier `GLPI-Agent-1.16-x64.msi` dans :

```
C:\Windows\SYSVOL\sysvol\glpi.local\scripts\
```

> Le chemin réseau UNC correspondant est automatiquement :
> `\\glpi.local\SYSVOL\glpi.local\scripts\`

---

## 2. Créer la GPO dans Active Directory

1. Ouvrir `gpmc.msc`
2. Clic droit sur le domaine `glpi.local` → **Créer un objet GPO** → nommer `Deploy-GLPI-Agent`
3. Clic droit sur la GPO → **Modifier**
4. Naviguer vers :
   ```
   Configuration ordinateur
     → Paramètres
       → Paramètres du logiciel
         → Installation de logiciels
   ```
5. Clic droit → **Nouveau** → **Package**
6. Saisir le chemin UNC :
   ```
   \\glpi.local\SYSVOL\glpi.local\scripts\GLPI-Agent-1.16-x64.msi
   ```
7. Choisir **Attribué** → OK

---

## 3. Appliquer la GPO sur le client Windows 10

```powershell
gpupdate /force
# Répondre O pour redémarrer
```

> L'installation se déclenche au démarrage de la machine.

---

## 4. Installer le plugin GLPI Inventory sur Debian

```bash
# Télécharger sur Debian
wget "https://github.com/glpi-project/glpi-inventory-plugin/releases/download/1.6.5/glpi-glpiinventory-1.6.5.tar.bz2" -O /tmp/glpiinventory.tar.bz2

# Copier dans le conteneur Docker
docker cp /tmp/glpiinventory.tar.bz2 glpi-app:/var/www/html/glpi/plugins/

# Entrer dans le conteneur
docker exec -it glpi-app bash

# Installer bzip2 si absent
apt-get update && apt-get install -y bzip2

# Décompresser
cd /var/www/html/glpi/plugins/
tar jxvf glpiinventory.tar.bz2
rm glpiinventory.tar.bz2

# Donner les droits
chown -R www-data:www-data glpiinventory/
exit
```

Puis dans GLPI :
```
Configuration → Plugins → Installé → Activer "GLPI Inventory"
```

---

## 5. Configurer le agent.cfg sur le client Windows 10

Fichier à modifier :
```
C:\Program Files\GLPI-Agent\etc\agent.cfg
```

Ligne à ajouter/décommenter :
```ini
server = http://192.168.230.165/
```

> ⚠️ L'URL correcte est la **racine de GLPI** (`/`), pas `/plugins/glpiinventory/`  
> GLPI 10 gère nativement la réception des inventaires.

En PowerShell :
```powershell
Add-Content "C:\Program Files\GLPI-Agent\etc\agent.cfg" "server = http://192.168.230.165/"
```

---

## 6. Activer l'inventaire dans GLPI

```
Administration → Inventaire → Configuration
→ Activer l'inventaire : ✅ Oui
→ Authentification des agents : Aucun
→ Sauvegarder
```

---

## 7. Autoriser les IPs dans GLPI Inventory

```
Administration → GLPI Inventory → Réseau → Plages IP → Ajouter
```

| Champ | Valeur |
|---|---|
| Entité | Entité racine |
| Début de plage IP | 192.168.230.1 |
| Fin de plage IP | 192.168.230.254 |

---

## 8. Démarrer et tester l'agent

```powershell
# Redémarrer le service
Restart-Service -Name "GLPI-Agent"
Get-Service -Name "GLPI-Agent"

# Forcer un inventaire immédiat
& "C:\Program Files\GLPI-Agent\glpi-agent.bat" --server "http://192.168.230.165/" --force

# Vérifier les logs
Get-Content "C:\Program Files\GLPI-Agent\logs\glpi-agent.log" | Select-Object -Last 10
```

---

## 9. Vérifier dans GLPI

```
Administration → Inventaire → Agents
```

Le client doit apparaître avec :
- **Nom** : `WIN10-CLIENT-...`
- **Dernier contact** : date récente
- **Version** : `GLPI-Agent_v1.16`

```
Parc → Ordinateurs
```

L'inventaire complet du poste (CPU, RAM, disques, logiciels) doit être visible.

---

## Erreurs rencontrées et solutions

| Erreur | Cause | Solution |
|---|---|---|
| `No target defined` | Ligne `server =` commentée dans agent.cfg | Décommenter et renseigner l'URL |
| `403 Forbidden` | Inventaire natif GLPI désactivé | Activer dans Administration → Inventaire |
| `403 Forbidden` | Plages IP non configurées | Ajouter `192.168.230.0/24` dans GLPI Inventory |
| `No such file or directory` | bzip2 absent du conteneur | `apt-get install -y bzip2` |
| Plugin 404 | Version inexistante sur GitHub | Vérifier avec `curl -s https://api.github.com/repos/glpi-project/glpi-inventory-plugin/releases/latest` |

