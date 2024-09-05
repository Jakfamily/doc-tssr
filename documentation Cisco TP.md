## Configuration de base des switch

###  Configurez un nom d'hôte
```cisco=
entrer en mode privilege = en (enable)
config global = conf t (configure terminal)
attribuez un nom = hostname name 
```
### configurez le mot de passe de console ainsi que celui du mode d'exécution privilégié 

#### config mdp de console 
```cisco=
line console 0 
password -> password
login
exit
```
#### Config mdp pour le mode previligier 

```cisco=
enable secret password 
```

### Verifier les configurations des mots de passe
verifier les config 
```cisco=
 end
 exit 
 entrer
```
si demande mdp c'est ok 

###  Configurer une bannière MOTD (message of the day, ou message du jour)

```cisco 
banner motd "Authorized acces only. Violators will be prosecuted to the full extent of the law."
```
### Enregistrez le fichier de configuration dans la mémoire NVRAM

s'assurer de bien quitter le mode de configuration (config)
```cisco=
exit 
copy running-config startup-config
entrer 
builing configuration .....
[ok]
exit
```
### Configurer les ordinateurs

* cliquez sur l'appareil 
* onglet Desktop
* ip configuration 
* mode static 
* entrez addresse ip et masque sous reseau

### Configurer l'interface de gestion des switch

#### Configuration ip switch 

```cisco=
en
conf t
interface vlan1
ip address 192.168.1.253 255.255.255.0
no shutdown
exit
```
ne pas oublier de copy la configuration dans nvram 

#### Verifiez la configuration de l'adresse IP

```cisco=
show ip interface brief
```

## Configuration des paramètres initiaux du routeur

### Verifier la configuration par défaut du routeur

#### Etablir une connexion console a un routeur 
* choissir cable bleu console 
* sur pc , selectionnez RS 232 au port consol
* cliquez pc -> onglet desktop -> terminal
* ok, puis entree

#### Examinez la configuration actuelle d'un routeur 

```cisco= 
en
show running-config
```
#### Examinez le contenu actuel de la mémoire vive non volatile (NVRAM)

```cisco=
show startup-config (permet d'afficher le fichier de configuration)
``` 

### Configurer et vérifier la configuration initiale du routeur

#### Configurez les paramètres initiaux du routeur
```cisco=
en
conf t 
hostname name 
line console 0 
password -> password 
login 
exit
```
chiffrez tous les mots de passe en clair
```cisco=
service password-encryption
```
configuration de la banner motd (pour comformiter legale)
```cisco=
banner motd 
```


#### Verifiez les paramètres initiaux du routeur

```cisco=
show running-config
```

### Enregistrez le fichier de configuration dans la mémoire NVRAM

s'assurer de bien quitter le mode de configuration (config)
```cisco=
exit 
copy running-config startup-config
entrer 
builing configuration .....
[ok]
exit
```

### Enregistrez le fichier de configuration initiale dans la mémoire Flash.

```cisco=
copy startup-config flash

enregistre la config sous le non de fichier entre parentheses si le nom vous convient, appuyez sur Entrée

show flash (pour verifier si le fichier et bien stocke)
```

## Configuration vlan 

![image](https://hackmd.io/_uploads/rJJrCELnR.png)
```cisco=
vlan (id)
name Faculty/Staff 
end
```

pour configurer un VLAN dédié à la voix (Voice VLAN). Par convention, ce VLAN est numéroté 150

pour verifier la configuration des vlan 
```cisco=
show vlan 
```

### Assigner des VLANs aux ports
![image](https://hackmd.io/_uploads/rkJRv1wnC.png)
![image](https://hackmd.io/_uploads/rkbUA48hA.png)

en mode previligier toujours (ex: VLAN 10 F0/11)

```cisco= 
interface f0/11
switchport mode access
switchport acces vlan 10
end
```

assigner le VOICE vlan 
```cisco=
interface f0/11
mls qos trust cos
switchport voice vlan 150
```

## Configuration trunks

#### Configuration trunk sur S1 en utilisant vlan 99 NATIVE

L'intérêt d'avoir un VLAN natif est de permettre le transit des trames sans étiquette VLAN dans le header. La bonne pratique recommande que le VLAN natif soit configuré avec le numéro 99

```cisco=
interface range g0/1 - 2
switchport mode trunk
switchport trunk native vlan 99 
end 
```

## Configuration de la sécurité des ports

![image](https://hackmd.io/_uploads/Sk2zAxwnC.png)

```cisco=
interface range fa0/1 - 2
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation restrict
exit
interface range fa0/3 - 24
shutdown
exit
end
write memory
```
#### Explication des Commandes :

* `switchport port-security` : Active la sécurité des ports sur les interfaces sélectionnées
* `switchport port-security maximum 1` : Limite le nombre de périphériques à un seul par port
* `switchport port-security mac-address sticky` : Permet au switch d'apprendre dynamiquement les adresses MAC et de les ajouter à la configuration
* `switchport port-security violation restrict` : Configure le switch pour restreindre l'accès en cas de violation sans désactiver le port
* `shutdown` : Désactive les autres ports inutilisés


#### Verifier les adresses MAC sécurisees
```cisco=
show port-security address
```

#### Pour voir les paramètres de sécurité des ports et vérifier que la sécurité des ports est activée 

```cisco=
show port-security
```

#### Pour reactiver le port selectionner l'interface puis 
```cisco=
no shutdown
```
## Configurer les routes statiques par default

#### Configurer une route statique IPv4 par défaut

```cisco=
• Création d'une route statique :
ip route [réseau] [masque_de_sous-réseau] [adresse_passerelle]
• Exemple :
ip route 192.168.10.0 255.255.255.0 10.0.0.254
• Création de la route par défaut (route utilisée pour tout le trafic inconnu) :
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```
#### Configurer une route statique IPv6 par défaut

```cisco=
• Création d'une route statique IPv6 :
ipv6 route [réseau-destination] [préfixe] [adresse-passerelle]
• Exemple :
ipv6 route 2001:db8:1::/64 fe80::1
• Création de la route par défaut (pour tout le trafic inconnu) :
ipv6 route ::/0 fe80::1
```
#### Routes flottantes (floating routes)

Les routes flottantes permettent d'augmenter la distance administrative (AD). Elles ne sont utilisées que si la route principale échoue. Vous pouvez ajouter la distance administrative dans la commande.

```cisco=
• ip route [réseau-destination] [masque_de_sous-réseau] [adresse-passerelle] [distance-administrative]
• Exemple (route flottante avec AD de 100) :
ip route 192.168.10.0 255.255.255.0 10.0.0.254 100
```

#### Attacher directement l'interface

Cela permet d'indiquer l'interface directement plutôt qu'une adresse passerelle.

```cisco=
• ip route [réseau-destination] [masque_de_sous-réseau] [interface]
• Exemple (via une interface) :
ip route 192.168.10.0 255.255.255.0 GigabitEthernet0/1
```

#### Next-hop récursif (seul le prochain saut est spécifié)

Dans une route récursive, seule une adresse intermédiaire (next-hop) est spécifiée, et le routeur résout cette adresse pour atteindre la destination finale.

```cisco=
• ip route [réseau-destination] [masque_de_sous-réseau] [adresse_next-hop]
• Exemple :
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

