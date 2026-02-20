Configuration VoIP de base avec Packet Tracer 8.1.1
📋 Description du tutoriel
Ce guide vous montre comment configurer la téléphonie sur IP (VoIP - Voice over IP) dans Packet Tracer 8.1.1.
Vous allez apprendre à :

Configurer Call Manager Express™ sur un routeur Cisco 2811
Utiliser différents équipements de téléphonie
Configurer les "dial peers" (associations de numéros)
Connecter des téléphones IP Cisco ainsi que des téléphones analogiques au réseau


🗺️ Schéma réseau
⚠️ Important : Ne connectez que le IP Phone 1 au début du lab. Le IP Phone 2 doit rester déconnecté.
[RouterA 2811] ---- [SwitchA] ---- [IP Phone 1]
                                   [IP Phone 2] (déconnecté)

🔧 Tâche 1 : Configuration de l'interface FastEthernet 0/0 et du serveur DHCP sur RouterA
Étape 1.1 : Configuration de l'interface réseau
Configurez l'interface FastEthernet 0/0 avec l'adresse IP 192.168.10.1/24.
💡 N'oubliez pas la commande no shutdown pour activer l'interface !
ciscoRouterA>enable
RouterA#configure terminal
RouterA(config)#interface FastEthernet0/0
RouterA(config-if)#ip address 192.168.10.1 255.255.255.0
RouterA(config-if)#no shutdown
📝 Explication :

enable : passe en mode privilégié (comme sudo sous Linux)
configure terminal : entre en mode configuration
interface FastEthernet0/0 : sélectionne l'interface à configurer
ip address : attribue l'IP et le masque de sous-réseau
no shutdown : active l'interface (par défaut elles sont éteintes)

Étape 1.2 : Configuration du serveur DHCP
Le serveur DHCP fournit automatiquement à chaque téléphone IP :

Une adresse IP
L'emplacement du serveur TFTP (pour télécharger la configuration)

ciscoRouterA(config)#ip dhcp pool VOICE
RouterA(dhcp-config)#network 192.168.10.0 255.255.255.0
RouterA(dhcp-config)#default-router 192.168.10.1
RouterA(dhcp-config)#option 150 ip 192.168.10.1
📝 Explication :

ip dhcp pool VOICE : crée un pool DHCP nommé "VOICE"
network 192.168.10.0 255.255.255.0 : définit le réseau et le masque
default-router 192.168.10.1 : indique la passerelle par défaut
option 150 ip 192.168.10.1 : OBLIGATOIRE pour la VoIP - indique l'adresse du serveur TFTP où les téléphones téléchargeront leur configuration

✅ Vérification
Après la configuration, attendez quelques instants puis vérifiez que le IP Phone 1 a bien reçu une adresse IP :

Placez votre curseur sur le téléphone
Un résumé de configuration devrait apparaître


📞 Tâche 2 : Configuration du service de téléphonie Call Manager Express
Configurez le service Call Manager Express (CME) sur RouterA pour activer la VoIP sur votre réseau.
ciscoRouterA(config)#telephony-service
RouterA(config-telephony)#max-dn 5
RouterA(config-telephony)#max-ephones 5
RouterA(config-telephony)#ip source-address 192.168.10.1 port 2000
RouterA(config-telephony)#auto assign 1 to 5
📝 Explication :

telephony-service : active le mode de configuration téléphonie
max-dn 5 : définit le nombre maximum de numéros de téléphone (Directory Numbers) à 5
max-ephones 5 : définit le nombre maximum de téléphones physiques à 5
ip source-address 192.168.10.1 port 2000 : définit l'adresse IP source pour la signalisation VoIP
auto assign 1 to 5 : assigne automatiquement les numéros de téléphone (DN 1 à 5) aux téléphones qui se connectent

💡 Analogie développeur :

max-dn = nombre de routes API disponibles
max-ephones = nombre de clients pouvant se connecter
auto assign = attribution automatique (comme le DHCP pour les IPs)


🔀 Tâche 3 : Configuration du VLAN voix sur SwitchA
Appliquez la configuration suivante sur les interfaces du SwitchA. Cette configuration sépare le trafic voix et données dans des VLANs différents.
ciscoSwitchA(config)#interface range fa0/1 - 5
SwitchA(config-if-range)#switchport mode access
SwitchA(config-if-range)#switchport voice vlan 1
📝 Explication :

interface range fa0/1 - 5 : sélectionne plusieurs interfaces en même temps (de fa0/1 à fa0/5)
switchport mode access : configure les ports en mode accès (non trunk)
switchport voice vlan 1 : définit le VLAN 1 pour transporter les paquets voix

💡 Pourquoi séparer voix et données ?

Qualité de service (QoS) : prioriser le trafic voix (temps réel)
Sécurité : isoler les flux
Performance : éviter les interférences


📱 Tâche 4 : Configuration du répertoire téléphonique pour IP Phone 1
Bien que le IP Phone 1 soit déjà connecté au SwitchA, il a besoin d'une configuration supplémentaire pour pouvoir communiquer. Vous devez assigner un numéro de téléphone à ce téléphone IP.
ciscoRouterA(config)#ephone-dn 1
RouterA(config-ephone-dn)#number 54001
📝 Explication :

ephone-dn 1 : définit la première entrée du répertoire (Directory Number 1)
number 54001 : assigne le numéro de téléphone 54001 à cette entrée

💡 Analogie développeur :
javascriptconst phoneDirectory = {
  dn1: { number: "54001" }
};

✅ Tâche 5 : Vérification de la configuration
Assurez-vous que le IP Phone 1 reçoit :

Une adresse IP
Le numéro de téléphone 54001 depuis RouterA

⏱️ Note : Cela peut prendre quelques instants.
Comment vérifier :

Placez votre curseur sur l'IP Phone 1
Vérifiez l'adresse IP reçue
Vérifiez que le numéro 54001 s'affiche sur l'écran du téléphone


📱 Tâche 6 : Configuration du répertoire téléphonique pour IP Phone 2
Connectez maintenant le IP Phone 2 au SwitchA et allumez-le en utilisant l'adaptateur secteur (onglet Physical).
ciscoRouterA(config)#ephone-dn 2
RouterA(config-ephone-dn)#number 54002
📝 Explication :

ephone-dn 2 : définit la deuxième entrée du répertoire
number 54002 : assigne le numéro de téléphone 54002 à cette entrée


✅ Tâche 7 : Vérification finale et test d'appel
Vérification du IP Phone 2
Assurez-vous que le IP Phone 2 reçoit :

Une adresse IP
Le numéro de téléphone 54002 depuis RouterA

⏱️ Note : Même procédure que la tâche n°5.
Test d'appel entre les téléphones
🎯 Test final :

Sur le IP Phone 2, composez le 54001
Vérifiez que le IP Phone 1 sonne et reçoit correctement l'appel
Décrochez et testez la communication bidirectionnelle

✅ Félicitations ! Si l'appel fonctionne, votre configuration VoIP de base est opérationnelle !

📚 Récapitulatif des concepts clés
ConceptAnalogie développeurRôleDirectory Number (DN)Route APINuméro de téléphone (ex: 54001)EphoneClient/ContrôleurTéléphone physiqueAuto assignMapping automatiqueAssociation DN ↔ téléphoneOption 150Variable d'env CONFIG_SERVERAdresse du serveur TFTPVoice VLANSous-réseau DockerSéparation du trafic voix

🐛 Dépannage
Le téléphone ne reçoit pas d'IP :

Vérifiez que l'interface fa0/0 est active (show ip interface brief)
Vérifiez la configuration DHCP (show ip dhcp pool)

Le numéro ne s'affiche pas :

Attendez 30-60 secondes (temps de téléchargement de la config)
Vérifiez la configuration CME (show telephony-service)

L'appel ne passe pas :

Vérifiez les DN configurés (show ephone-dn)
Vérifiez que les téléphones sont enregistrés (show ephone)


📅 Dernière mise à jour : 04 Mai 2023
🎓 Niveau : Débutant - Intermédiaire