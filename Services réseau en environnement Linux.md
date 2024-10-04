--- 
En cours de rédaction 

---
## Renommer les cartes réseaux sous Linux avec `systemd` (ens ou enp0s[0-n])


### Méthode dynamique (non persistante après redémarrage)
```bash=
# Afficher les informations réseau
ip a

# Ajouter une IP
ip a add 10.11.12.13/24 dev ens33

# Supprimer une IP
ip a del 172.16.0.1/16 dev ens37

# Réinitialiser la configuration de l'interface réseau
ip a flush ens33

# Désactiver une interface réseau
ip link set ens33 down

# Activer une interface réseau
ip link set ens37 up
```

### Méthode persistante (via le fichier /etc/network/interfaces)
```bash=
# Interface ens33 configurée manuellement
auto ens33
iface ens33 inet static
    address 192.168.66.6  # Utilisation possible de la notation CIDR (/24)
    netmask 255.255.255.0

# Interface ens37 configurée via DHCP
auto ens37
iface ens37 inet dhcp
```
ne pas oublier de redemarter le reseaux apres configuration : 
```bash=
systemctl restart networking
```
###  Méthode network manager 
```
# Vérifie si Network Manager est activé et en cours d'exécution
systemctl status NetworkManager

# Modifier la connexion existante "Wired connection 1" pour attribuer une adresse IP
nmcli connection modify Wired\ connection\ 1 ipv4.addresses 192.168.1.100/24

# Passer la méthode de gestion de l'IP à "manual" (désactiver le DHCP)
nmcli connection modify Wired\ connection\ 1 ipv4.addresses ip/mask ipv4.method manual

# Ajouter une seconde adresse IP à l'interface réseau (si nécessaire)
nmcli connection modify Wired\ connection\ 1 +ipv4.addresses 192.168.1.101/24

# Redémarre la connexion réseau
nmcli connection up Wired\ connection\ 1
```
## Passerelle par defaut 

```bash=
# Ajouter gateway dans `/etc/network/interfaces`
auto ens33
iface ens33 inet static
    address 192.168.66.6  # Utilisation possible de la notation CIDR (/24)
    netmask 255.255.255.0
		gateway 192.168.66.254
```

Nous pouvons également ajouter la routes par default à l'aide de la commande `ip r`, de la même manière que pour les adresses via `ip`.

```bash=
ip r 									# Affiche la table de routage 
ip r add default      # Ajouter une route par défaut
ip r change default   # Changer une route par défaut
ip r del default      # Supprimer une route par défaut
```

## Nom d'Hôte / FQDN (Full Qualified Domain Name = nom d'hote + nom de domaine)
```bash
# Pour vérifier le nom d'hôte
hostname
```
Le nom d'hôte figure dans le fichier /etc/hostname qui doit contenir uniquement le nom d'hôte. 

Il doit également être résolu localement dans le fichier /etc/hosts, comme suit : 
```bash=
127.0.0.1  localhost
127.0.1.1  test-lan.test (FQDN)  test-lan
```

## Configuration DNS
elle vas ce fairte soit par etc/hosts 
et dans le fichier etc/resolv.conf 

l'ordre de resolution et defini dans le fichier `etc/nsswitch.conf`
config standart
on trouveras hosts: files dns ce qui veut dire lors de la requete dns en premier je lis le fichier `etc/host` si il trouve pas il vas interoger le fichier etc/resolve.conf 

soit avec le net work manager 
```bash =
#avec networkmanager
nmcli connection modify Wired\ connection\ 1 ipv4.dns 192.168.1
```
```bash=
#Dans etc/resolv.conf 
search demo.eni ad.campus-eni.fr   #permet de definir les suffix dns
domain ad.campus-eni.fr            # nom de domaine principal utiliser 
nameserver 10.0.0.3                #pour definir les server dns 
```
si la configuration reseaux est en dhcp cest celui ci qui fournit la configurration dns 

# Routage 

```bash 
# Afficher la table de routage
ip route

# Ajouter une route vers une IP spécifique via une passerelle
ip route add 10.11.12.13 via 172.16.6.123

# Ajouter une route vers un réseau
ip route add 10.11.12.13/16 via 172.16.6.123

# Ajouter une route par défaut
ip route add default via 172.16.6.123

# Modifier une route par défaut existante
ip route change default via 172.16.6.123

# Supprimer une route vers un réseau
ip route del 10.12.13.14/16

# Configuration persistante dans /etc/network/interfaces
# Exemple d'ajout d'une route dans ce fichier
auto eth0
iface eth0 inet static
  address 192.168.1.10
  netmask 255.255.255.0
  gateway 192.168.1.1
  post-up ip route add 10.11.12.0/24 via 192.168.1.1

# Activer le routage IP dans /etc/sysctl.conf
# passer la valeur forward a 1 au lieu de 0
net.ipv4.ip_fordward=1

# Recharger la configuration sysctl pour prendre en compte le changement
sysctl -p

# Vérifier que le routage IP est bien activé
sysctl net.ipv4.ip_forward
```

# pfsense 

une fois installer modifier le sens des carte reseaux selon la config pour ce faire on assigne les interface en tapant 1 
 intervertir les carte 
 
puis configurer les adresse ip en tapant 2 

tres intuitifs




# Mettre en place un acces a distance 

generer une pair de cles ssh
```bash=
ssh-keygen
```
une fois generer cpier la cles sur machine distante 
```bash=
ssh-copy-id user@ip
```
ne pas oublier d'installer open-ssh

# Mise en place du service DNS avec BIND9

### Installation de BIND9

```bash
sudo apt install bind9
```
### Configuration de base
Modifier le fichier /etc/bind/named.conf.options
Ouvrez le fichier avec un éditeur de texte :

```bash
sudo nano /etc/bind/named.conf.options
```
Ajoutez ou modifiez les lignes suivantes :

```bash
options {
    // Désactiver DNSSEC
    dnssec-validation no;

    // Désactiver IPv6 si non utilisé
    listen-on-v6 { none; };

    // Les autres options seront ajoutées plus tard
};
```
### Création d'une ACL
Modifiez le fichier /etc/bind/named.conf pour ajouter une ACL :

```bash
sudo nano /etc/bind/named.conf
```
Ajoutez ces lignes au début du fichier :

```bash
acl "trusted-c" {
    192.168.1.0/24;
    10.0.0.0/8;
    localhost;
};
```

### Configuration des options de récursion
Retournez dans le fichier /etc/bind/named.conf.options et ajoutez la ligne suivante dans le bloc "options" :

```bash
options {
    // ... autres options ...

    // Autoriser les requêtes récursives depuis les réseaux de confiance
    allow-recursion { trusted; };
};
```
### Vérification et redémarrage
Vérifiez la configuration :

```bash
sudo named-checkconf
```
Si aucune erreur n'est signalée, redémarrez le service BIND9 :
```bash
sudo systemctl restart bind9
```
