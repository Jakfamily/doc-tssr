# Documentation Cisco Packet Tracer

## 1. Configuration de Base du Routeur

### 1.1 Accéder au Mode Privilégié
```cisco
enable
```
Explication: Passez du mode utilisateur au mode privilégié pour accéder aux commandes de configuration.

### 1.2 Passer en Mode de Configuration Globale
```cisco
configure terminal ou conf t 
```
Explication: Entrez en mode de configuration globale pour effectuer des configurations sur l'ensemble du routeur.

### 1.3 Configurer le Nom du Routeur
```cisco 
hostname RouterName
```
Explication: Remplacez RouterName par le nom que vous souhaitez donner au routeur.

### 1.4 Configurer une Interface avec une Adresse IP
```cisco 
interface GigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```
Explication: Configurez l'interface GigabitEthernet0/0 avec une adresse IP et activez l'interface.

### 1.5 Configurer la Passerelle par Défaut
```cisco 
ip default-gateway 192.168.1.254
```
Explication: Définissez la passerelle par défaut pour diriger le trafic non spécifique vers cette adresse IP.

### 1.6 Configurer une Route Statique
```cisco 
ip route 192.168.2.0 255.255.255.0 192.168.1.2
```
Explication: Ajoutez une route statique pour diriger le trafic destiné au réseau 192.168.2.0/24 via le routeur 192.168.1.2.

### 1.7 Configurer la Sécurité des Connexions Telnet/SSH
```cisco
line vty 0 4
password yourpassword
login
transport input ssh
```
Explication: Sécurisez l'accès au routeur via Telnet ou SSH en définissant un mot de passe et en limitant le type de connexion.

## 2. Configuration de Base du Switch

### 2.1 Configurer le Nom du Switch
```cisco
hostname SwitchName
```
Explication: Remplacez SwitchName par le nom que vous souhaitez donner au switch.

### 2.1.2 Suprimer le nom du Switch
```cisco
no hostname
```
Explication: Cette commande supprime le nom personnalisé du switch et rétablit le nom par défaut (Switch).

### 2.2 Configurer une Interface VLAN avec une Adresse IP
```cisco
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
```
Explication: Configurez l'interface VLAN 1 avec une adresse IP pour gérer le switch à distance.

### 2.3 Configurer le Mode d'Accès d'une Interface
```
interface FastEthernet0/1
switchport mode access
switchport access vlan 10
```
Explication: Configurez l'interface FastEthernet0/1 pour qu'elle appartienne au VLAN 10

### 2.4 Configurer la Sécurité des Ports (Port Security)
```cisco
interface FastEthernet0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
```
Explication: Configurez la sécurité des ports pour limiter le nombre d'adresses MAC pouvant être apprises sur une interface et spécifiez une action en cas de violation

## 3. Configuration des VLANs

### 3.1 Créer et Nommer un VLAN
```cisco
vlan 10
name Sales
```
Explication: Créez un VLAN 10 et nommez-le Sales.

### 3.2 Assigner des Interfaces à un VLAN
```cisco
interface range FastEthernet0/2 - 4
switchport mode access
switchport access vlan 10
```
Explication: Assignez les interfaces FastEthernet0/2 à FastEthernet0/4 au VLAN 10.

### 3.3 Afficher les VLANs Configurés
```cisco 
show vlan brief
```
Explication: Affichez un résumé de tous les VLANs configurés sur le switch.

## 4. Configuration des Trunks

### 4.1 Configurer une Interface en Mode Trunk
```cisco 
interface GigabitEthernet0/1
switchport mode trunk
```
Explication: Configurez l'interface GigabitEthernet0/1 en mode trunk pour transporter plusieurs VLANs.

### 4.2 Spécifier les VLANs Autorisés sur un Trunk
```cisco 
switchport trunk allowed vlan 10,20,30
```
Explication: Autorisez uniquement les VLANs 10, 20, et 30 à traverser l'interface trunk.

### 4.3 Configurer un VLAN Natif sur un Trunk
```cisco
switchport trunk native vlan 99
```
Explication: Définissez le VLAN 99 comme VLAN natif pour cette interface trunk.

### 4.4 Afficher les Interfaces Trunk Configurées
```cisco 
show interfaces trunk
```
Explication: Affichez toutes les interfaces trunk configurées sur le switch.

### 4.5 Configurer DTP (Dynamic Trunking Protocol)
```cisco
interface GigabitEthernet0/1
switchport mode dynamic desirable
```

## 5. Sauvegarde et Restauration des Configurations

### 5.1 Sauvegarder la Configuration en Cours
```cisco 
copy running-config startup-config
```
Explication: Sauvegardez la configuration en cours dans la mémoire NVRAM pour qu'elle persiste après un redémarrage.

### 5.2 Restaurer une Configuration Sauvegardé
```cisco 
copy startup-config running-config
```
Explication: Chargez la configuration sauvegardée dans la configuration active.

### 5.3 Sauvegarder la Configuration sur un Serveur TFTP
```cisco
copy running-config tftp
```
Explication: Sauvegardez la configuration en cours sur un serveur TFTP.

### 5.4 Restaurer une Configuration depuis un Serveur TFTP
```cisco
copy tftp running-config
```
Explication: Restaurez une configuration depuis un serveur TFTP vers la configuration active.

## 6. Vérification et Dépannage

### 6.1 Vérifier la Configuration en Cours
```cisco
show running-config
```
Explication: Affichez la configuration active du routeur ou du switch.


### 6.2 Vérifier l'État des Interfaces
```cisco
show ip interface brief
```
Explication: Affichez un résumé de l'état des interfaces, y compris leur adresse IP et leur statut (up ou down).


### 6.3 Vérifier la Table de Routage
```cisco 
show ip route
```
Explication: Affichez la table de routage du routeur, indiquant comment les paquets sont routés à travers le réseau.

### 6.4 Afficher la Table MAC (MAC Address Table)
```cisco
show mac address-table
```
Explication: Affichez la table des adresses MAC, montrant quelles adresses MAC sont associées à quelles interfaces et VLANs.

## 7. Configuration VTP pour la Gestion des VLANs sur Plusieurs Switches

### 7.1 Configurer un Switch en Mode VTP Serveur
```cisco
vtp mode server
vtp domain mydomain
vtp password mypassword
```
Explication: Configurez le switch en mode VTP serveur pour gérer les VLANs dans le domaine VTP mydomain.

### 7.2 Configurer un Switch en Mode VTP Client
```cisco
vtp mode client
```
Explication: Configurez le switch en mode VTP client pour recevoir les mises à jour VLAN du serveur VTP.

### 7.3 Vérifier le Statut VTP
```cisco 
show vtp status
```
Explication: Affichez le statut VTP actuel, y compris le mode, le nom de domaine, et la version.

### 7.4 Configurer la Version VTP
```cisco
vtp version 2
```
Explication: Configurez la version du protocole VTP pour garantir la compatibilité avec tous les switches dans le domaine.

## 8. Sécurité et Administration des VLANs

### 8.1 Appliquer une ACL à un VLAN
```cisco 
interface vlan 10
ip access-group 101 in
```
Explication: Appliquez une liste de contrôle d'accès (ACL) numérotée 101 à l'interface VLAN 10 pour filtrer le trafic entrant.

### 8.2 Restreindre les VLANs Inutilisés
```cisco=
interface range GigabitEthernet0/1 - 4
switchport mode access
switchport access vlan 999
shutdown
```
Explication: Assignez les interfaces inutilisées à un VLAN inactif (comme 999) et désactivez-les pour des raisons de sécurité.

### 8.3 Activer l’Authentification 802.1X
```cisco
interface FastEthernet0/1
dot1x port-control auto
```
Explication: Activez l'authentification 802.1X sur une interface pour sécuriser l'accès au réseau via un serveur RADIUS.

### 8.4 Configurer une Bannière de Connexion
```cisco 
banner motd #Unauthorized access is prohibited.#
```
Explication: Configurez une bannière de message (MOTD) pour afficher un avertissement lors de la connexion au routeur ou au switch.

### 8.5 Configurer un Serveur DHCP sur le Routeur
```cisco
ip dhcp pool VLAN10
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
```
Explication: Configurez un pool DHCP sur le routeur pour distribuer des adresses IP aux hôtes connectés au réseau VLAN 10.

### 8.6 Configurer une ACL Étendue pour Filtrer le Trafic
```cisco
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
```
Explication: Créez une liste de contrôle d'accès (ACL) étendue pour autoriser uniquement le trafic HTTP sortant depuis le réseau 192.168.1.0/24.

### 8.7 Configurer l’Authentification des Utilisateurs Locaux
```cisco 
username admin privilege 15 secret strongpassword
```

Explication: Créez un utilisateur local avec des privilèges administratifs pour sécuriser l'accès au routeur.


## 9. Configuration d’un NAT (Network Address Translation)

### 9.1 Configurer un NAT Dynamique
```cisco
ip nat pool mypool 192.168.1.10 192.168.1.20 netmask 255.255.255.0
ip nat inside source list 1 pool mypool
```

Explication: Configurez un pool NAT pour traduire dynamiquement les adresses IP internes vers un ensemble d'adresses publiques.

### 9.2 Configurer un NAT Statique

```cisco
ip nat inside source static 192.168.1.5 203.0.113.5
```
Explication: Configurez une adresse NAT statique pour mapper une adresse IP interne à une adresse IP publique spécifique.

## 10. Configuration d’un Serveur DNS

### 10.1 Configurer un Serveur DNS sur le Routeur
```cisco
ip dns server
ip host www.example.com 192.168.1.10
```
Explication: Configurez le routeur pour qu'il fonctionne comme un serveur DNS en mappant les noms de domaine aux adresses IP.

## 11. Configuration des Services NTP (Network Time Protocol)

### 11.1 Configurer le Routeur comme un Client NTP
```cisco
ntp server 192.168.1.100
```
Explication: Synchronisez l'heure du routeur avec un serveur NTP externe pour garantir que l'horloge du réseau est correcte.

## 12. Maintenance et Surveillance

### 12.1 Configurer un Syslog pour la Surveillance des Logs

```cisco 
logging 192.168.1.200
```
Explication: Configurez le routeur pour envoyer les logs à un serveur Syslog centralisé pour une surveillance continue.

### 12.2 Configurer SNMP (Simple Network Management Protocol)

```cisco
snmp-server community public RO
snmp-server location "Data Center"
```

Explication: Configurez SNMP pour surveiller l'état du réseau et collecter des données de performance.

## 13. Dépannage Avancé
### 13.1 Vérifier les Statistiques NAT

```cisco
show ip nat statistics
```

Explication: Affichez les statistiques NAT pour vérifier le nombre de traductions actives et le statut global du NAT.

### 13.2 Vérifier les Sessions Actives

```cisco
show tcp brief
```

Explication: Affichez un résumé des sessions TCP actives pour aider à diagnostiquer les problèmes de connectivité.