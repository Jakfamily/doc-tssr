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
domain ad.campus-eni.fr            # nom de domaine prioncipal utiliser 
nameserver 10.0.0.3                #pour definir les server dns 
```
si la configuration reseaux est en dhcp cest celui ci qui fournit la configurration dns 