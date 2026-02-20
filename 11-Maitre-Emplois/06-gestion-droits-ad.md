# Gestion des Droits et Accès Utilisateurs (Active Directory & GPO)

> 📚 **Module :** Maître Emplois - Mission 06
> 📅 **Date :** Janvier 2026
> ⏱️ **Durée :** 8-10 heures
> 🎯 **Niveau :** N2-N3 (Intermédiaire à Avancé)

---

## 📖 Table des matières

- [Objectifs](#-objectifs)
- [Active Directory - Gestion des comptes](#-active-directory---gestion-des-comptes)
- [Groupes et permissions](#-groupes-et-permissions)
- [GPO - Stratégies de groupe](#-gpo---stratégies-de-groupe)
- [Cas pratiques courants](#-cas-pratiques-courants)
- [Exercices pratiques](#-exercices-pratiques)

---

## 🎯 Objectifs

À la fin de ce cours, vous serez capable de :

- ✅ Créer et gérer des comptes utilisateurs dans AD
- ✅ Gérer les groupes de sécurité et de distribution
- ✅ Créer et appliquer des GPO
- ✅ Résoudre les problèmes d'accès et de permissions
- ✅ Utiliser PowerShell pour l'administration AD

---

## 👤 Active Directory - Gestion des comptes

### Opérations courantes N1/N2

#### Création d'un utilisateur

```powershell
# Créer un utilisateur avec PowerShell
New-ADUser -Name "Jean DUPONT" `
    -GivenName "Jean" `
    -Surname "DUPONT" `
    -SamAccountName "jdupont" `
    -UserPrincipalName "jdupont@entreprise.local" `
    -Path "OU=Utilisateurs,OU=Paris,DC=entreprise,DC=local" `
    -AccountPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force) `
    -ChangePasswordAtLogon $true `
    -Enabled $true

# Copier un utilisateur existant (modèle)
Get-ADUser -Identity "modele.compta" -Properties MemberOf |
    ForEach-Object {
        $_.MemberOf | ForEach-Object { Add-ADGroupMember -Identity $_ -Members "jdupont" }
    }
```

#### Réinitialisation de mot de passe

```powershell
# Réinitialiser le mot de passe
Set-ADAccountPassword -Identity "jdupont" -Reset -NewPassword (ConvertTo-SecureString "NouveauMdp123!" -AsPlainText -Force)

# Forcer le changement au prochain login
Set-ADUser -Identity "jdupont" -ChangePasswordAtLogon $true

# Débloquer un compte verrouillé
Unlock-ADAccount -Identity "jdupont"

# Vérifier le statut du compte
Get-ADUser -Identity "jdupont" -Properties LockedOut, PasswordLastSet, Enabled, PasswordExpired |
    Select-Object Name, LockedOut, PasswordLastSet, Enabled, PasswordExpired
```

#### Désactivation et suppression

```powershell
# Désactiver un compte (départ employé)
Disable-ADAccount -Identity "jdupont"

# Déplacer vers OU des comptes désactivés
Move-ADObject -Identity "CN=Jean DUPONT,OU=Utilisateurs,DC=entreprise,DC=local" `
    -TargetPath "OU=Comptes_Desactives,DC=entreprise,DC=local"

# Supprimer après la période de rétention
Remove-ADUser -Identity "jdupont" -Confirm:$false
```

### Structure OU recommandée

```
entreprise.local
├── OU=Utilisateurs
│   ├── OU=Paris
│   │   ├── OU=Direction
│   │   ├── OU=Comptabilité
│   │   └── OU=Commercial
│   └── OU=Lyon
├── OU=Groupes
│   ├── OU=Sécurité
│   └── OU=Distribution
├── OU=Ordinateurs
│   ├── OU=Postes
│   └── OU=Serveurs
└── OU=Comptes_Desactives
```

---

## 👥 Groupes et permissions

### Types de groupes

| Type | Portée | Usage |
|------|--------|-------|
| **Sécurité Global** | Domaine | Regrouper utilisateurs par fonction |
| **Sécurité Domain Local** | Domaine | Attribuer permissions sur ressources |
| **Distribution** | Email | Listes de diffusion |

### Stratégie AGDLP

```
┌─────────────────────────────────────────────────────────────┐
│                    STRATÉGIE A-G-DL-P                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  A  (Account)        → Comptes utilisateurs                 │
│  ↓                                                           │
│  G  (Global Group)   → Groupes globaux (par fonction)       │
│  ↓                      Ex: GG_Comptabilite                 │
│  DL (Domain Local)   → Groupes locaux (par ressource)       │
│  ↓                      Ex: DL_Partage_Compta_RW            │
│  P  (Permission)     → Permissions sur la ressource         │
│                         Ex: Lecture/Écriture sur dossier    │
│                                                              │
│  EXEMPLE :                                                   │
│  User jdupont → GG_Comptabilite → DL_Partage_Compta_RW →   │
│  Permission Modify sur \\SRV\Partage\Comptabilite          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Gestion des groupes PowerShell

```powershell
# Créer un groupe de sécurité global
New-ADGroup -Name "GG_Comptabilite" `
    -GroupScope Global `
    -GroupCategory Security `
    -Path "OU=Groupes,DC=entreprise,DC=local" `
    -Description "Utilisateurs du service Comptabilité"

# Ajouter un membre au groupe
Add-ADGroupMember -Identity "GG_Comptabilite" -Members "jdupont"

# Voir les membres d'un groupe
Get-ADGroupMember -Identity "GG_Comptabilite" | Select-Object Name

# Voir les groupes d'un utilisateur
Get-ADUser -Identity "jdupont" -Properties MemberOf |
    Select-Object -ExpandProperty MemberOf
```

---

## 📋 GPO - Stratégies de groupe

### Création et liaison de GPO

```powershell
# Créer une GPO
New-GPO -Name "GPO_Mappage_Lecteurs_Compta"

# Lier la GPO à une OU
New-GPLink -Name "GPO_Mappage_Lecteurs_Compta" -Target "OU=Comptabilité,OU=Paris,DC=entreprise,DC=local"

# Forcer la mise à jour des GPO sur un PC
gpupdate /force

# Résultat des GPO appliquées
gpresult /r

# Rapport détaillé HTML
gpresult /h C:\Temp\gpresult.html
```

### GPO courantes à maîtriser

#### Mappage de lecteurs réseau

```
Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteurs

Nouveau mappage :
- Action : Mettre à jour
- Emplacement : \\SRV-FILES\Compta$
- Lettre : S:
- Étiquette : Comptabilité
- Ciblage : Groupe de sécurité GG_Comptabilite
```

#### Déploiement de logiciels

```
Configuration ordinateur > Stratégies > Paramètres du logiciel > Installation de logiciel

Clic droit > Nouveau > Package
- Sélectionner le fichier .msi sur un partage réseau
- Choisir "Attribué" ou "Publié"
```

#### Stratégies de mot de passe

```
Configuration ordinateur > Stratégies > Paramètres Windows >
Paramètres de sécurité > Stratégies de compte > Stratégie de mot de passe

- Longueur minimale : 8 caractères
- Complexité : Activée
- Durée de vie maximale : 90 jours
- Historique : 12 mots de passe
```

### Dépannage GPO

```powershell
# Vérifier les GPO appliquées
gpresult /r /scope:computer
gpresult /r /scope:user

# Rapport RSoP détaillé
gpresult /h rapport.html

# Simuler l'application des GPO
Get-GPResultantSetOfPolicy -Computer "PC-USER01" -User "jdupont" -ReportType Html -Path "C:\Temp\rsop.html"

# Vérifier la réplication des GPO
repadmin /showrepl
dcdiag /test:sysvolcheck /test:advertising
```

---

## 💼 Cas pratiques courants

### Cas 1 : Nouvel employé

```powershell
# Script création nouvel employé
param(
    [string]$Prenom,
    [string]$Nom,
    [string]$Service,
    [string]$Manager
)

$login = ($Prenom.Substring(0,1) + $Nom).ToLower()
$email = "$login@entreprise.fr"
$password = "Bienvenue2026!"
$ou = "OU=$Service,OU=Utilisateurs,DC=entreprise,DC=local"

# Créer le compte
New-ADUser -Name "$Prenom $Nom" `
    -GivenName $Prenom `
    -Surname $Nom `
    -SamAccountName $login `
    -UserPrincipalName $email `
    -EmailAddress $email `
    -Path $ou `
    -Manager $Manager `
    -AccountPassword (ConvertTo-SecureString $password -AsPlainText -Force) `
    -ChangePasswordAtLogon $true `
    -Enabled $true

# Ajouter aux groupes du service
Add-ADGroupMember -Identity "GG_$Service" -Members $login

Write-Host "Compte créé : $login / $password"
```

### Cas 2 : Accès refusé à un dossier partagé

```
┌─────────────────────────────────────────────────────────────┐
│  DIAGNOSTIC ACCÈS REFUSÉ                                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Vérifier que l'utilisateur est dans le bon groupe      │
│     Get-ADUser jdupont -Properties MemberOf                 │
│                                                              │
│  2. Vérifier les permissions du partage                     │
│     Get-SmbShareAccess -Name "Partage"                      │
│                                                              │
│  3. Vérifier les permissions NTFS                           │
│     Get-Acl "D:\Partage" | Format-List                      │
│                                                              │
│  4. Forcer la mise à jour des tickets Kerberos             │
│     klist purge (sur le PC client)                          │
│     Se déconnecter/reconnecter                              │
│                                                              │
│  5. Vérifier la réplication AD                              │
│     repadmin /syncall                                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎯 Exercices pratiques

### Exercice : Script de gestion utilisateur

Créez un script qui :
1. Crée un utilisateur
2. L'ajoute aux groupes de son service
3. Génère un rapport

<details>
<summary>Solution</summary>

```powershell
function New-Employee {
    param(
        [Parameter(Mandatory)][string]$FirstName,
        [Parameter(Mandatory)][string]$LastName,
        [Parameter(Mandatory)][string]$Department
    )

    $sam = ($FirstName.Substring(0,1) + $LastName).ToLower() -replace '[éèê]','e'
    $upn = "$sam@entreprise.local"
    $pw = "Welcome" + (Get-Random -Minimum 1000 -Maximum 9999) + "!"

    try {
        New-ADUser -Name "$FirstName $LastName" `
            -GivenName $FirstName -Surname $LastName `
            -SamAccountName $sam -UserPrincipalName $upn `
            -Path "OU=$Department,OU=Users,DC=entreprise,DC=local" `
            -AccountPassword (ConvertTo-SecureString $pw -AsPlainText -Force) `
            -ChangePasswordAtLogon $true -Enabled $true

        Add-ADGroupMember -Identity "GG_$Department" -Members $sam

        return [PSCustomObject]@{
            Login = $sam
            Password = $pw
            Email = $upn
            Status = "Success"
        }
    } catch {
        return [PSCustomObject]@{ Status = "Error"; Message = $_.Exception.Message }
    }
}

# Utilisation
New-Employee -FirstName "Marie" -LastName "Martin" -Department "Comptabilite"
```
</details>

---

## 📚 Ressources

- [Microsoft AD Documentation](https://docs.microsoft.com/windows-server/identity/ad-ds/)
- [Group Policy Documentation](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/group-policy/)

---

## ✅ Checklist de révision

- [ ] Créer/modifier/supprimer des comptes AD
- [ ] Gérer les groupes de sécurité (AGDLP)
- [ ] Créer et lier des GPO
- [ ] Diagnostiquer les problèmes d'accès
- [ ] Utiliser PowerShell pour l'administration AD

---

<div align="center">

**Cours suivant :** [Installation et support des applications métiers](./07-applications-metiers.md)

[⬅️ Retour au sommaire](./README.md)

</div>
