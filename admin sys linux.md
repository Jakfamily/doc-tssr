
# Installation des systèmes
- Une fois arrivé à l'étape des partitionnements des disques, on pick l'option manuel
![image](https://hackmd.io/_uploads/ryhzHpxjC.png)
- Choisir son hard drive, puis configurer LVM
![image](https://hackmd.io/_uploads/H1jGIpgiR.png)
- Ensuite, on créer son groupe de volumes, puis ses volumes logiques
![image](https://hackmd.io/_uploads/ry9FU6gjR.png)
![image](https://hackmd.io/_uploads/ByscUaloR.png)
- Ne pas oublier de bien choisir ses points de montage, exemple sur lvboot
![image](https://hackmd.io/_uploads/B1J8DTlsR.png)
- Exemple de volumes faits
![image](https://hackmd.io/_uploads/rJD6Pals0.png)
- **Attention a cette étape** : bien cocher oui puis france et ftp.fr-debian
![image](https://hackmd.io/_uploads/r1VmOTlo0.png)

# Utilisateurs et environnement
## Utilisateur principal et privilèges
Ajout de l'utilisateur principal en sudoers : **commandes à faire en root**
```bash
visudo

%username ALL=(ALL:ALL)
```
## Utilisateurs et groupes
**Voici un exemple complet de la procédure**

Créer l'utilisateur alice avec un répertoire personnel, bash comme shell par défaut, et l'ajouter au groupe administrateur
```bash
groupadd administrateur

sudo useradd -m -s /bin/bash -g administrateur alice
```
```bash
sudo groupmod -n nouveau_nom ancien_nom 
```
Définir un mot de passe par défaut (exemple : Ch@ngeMe) pour alice
```bash
echo "alice:Ch@ngeMe" | sudo chpasswd
```
Ou
```bash
sudo passwd alice
```
Forcer alice à changer son mot de passe lors de sa première connexion
```bash
sudo chage -d 0 alice
```
**Résultat final** 
```
Répertoire personnel : /home/alice
Shell par défaut : /bin/bash
Groupe principal : administrateur
Mot de passe initial : Ch@ngeMe
Mot de passe à changer : Alice devra changer son mot de passe lors de sa première connexion pour des raisons de sécurité.
```
## Environnement de travail
installation de vim 
```bash
sudo apt install vim
```
creation du .vimrc 
```bash
vim .vimrc
```
parametres de base de vim

* Affiche les numéros de ligne dans la marge gauche de l'éditeur.

```set number```

* Active la coloration syntaxique pour améliorer la lisibilité du code.

```syntax enable```

* Met en surbrillance toutes les occurrences de la recherche actuelle.

```set hlsearch```

* Active la recherche incrémentale, c'est-à-dire que les résultats de la recherche sont mis à jour à mesure que vous tapez.

```set incsearch```

* Active l'indentation intelligente, ce qui ajuste l'indentation en fonction de la syntaxe du code.

```set smartindent```

* Active l'indentation automatique lors de la frappe de nouvelles lignes.

```set autoindent```

* Définit la largeur d'un onglet à 4 espaces.

```set tabstop=4```

* Définit le nombre d'espaces utilisés pour chaque niveau d'indentation lors du décalage automatique (shift).

```set shiftwidth=4```

* Remplace les tabulations par des espaces, ce qui permet de conserver une mise en page cohérente.

```set expandtab```

* Configure les options de complétion automatique :
* `menu` : Affiche les options de complétion dans un menu déroulant.
* `menuone` : Affiche le menu de complétion même s'il n'y a qu'une seule option.
* `noselect` : Ne sélectionne pas automatiquement la première option du menu de complétion.

```set completeopt=menu,menuone,noselect```

Ne pas oublier de commenter les lignes des sources deb-src cdroom
```bash
vim /etc/apt/sources.list
```
Ajouter la commande dans le fichier vim 
```bash
:g/^deb-src/s/^/#/
```
![image](https://hackmd.io/_uploads/SJ6MJk-jC.png)


# Stockage et ressources

## Partitionnement de disques

### Ajout d'un nouveau disque / Installation d'outils
Après avoir ajouté le disque dans la VM, redémarrez la machine

**Installer le paquet util-linux :**
```bash
sudo apt install util-linux
```
**lexique :**
- ```lsblk``` Affiche les informations sur les périphériques de bloc (disques et partitions) 
- ```fdisk``` Permet de manipuler les partitions des disques

```bash
sudo fdisk /dev/sdb
```
### Creer une nouvelle partition

- taper `n`
- selectionner `p` partition principal
- choisir le numero : entrer par default
- définir le premier secteur: entrer par default
- définir la taille de la partition : +taille(G,M,..)
- taper `t` : definissez le type 83 pour linux
- taper `w` : pour ecrire la partition

### Formater les partitions

Si ext4:
```bash
sudo mkfs.ext4 -L label /dev/sdb*
```
Si xfs:
- installer xfs
```bash
sudo apt install xfsprogs

sudo mkfs.xfs -L label /dev/sdb*
```
### Pour ajouter ou modifier le label 
- ext4
```bash
sudo e2label /dev/sdb* nouveau label
```
- xfs
```bash
sudo xfs_admin -L new-label /dev/disque-xfs-a-modifier
```
## Occupation des espaces disques

### Monter les partitions
Créer les points de montage temporaires 
```bash 
sudo mkdir /mnt/PROFILS

sudo mount /dev/sdb1 /mnt/PROFILS
```
Attention de ne pas oublier 
``` bash 
rm -rf /mnt/dossier-de-montage
```
Pour vérifier si elles sont montées corectement 
```bash 
df -h 
```
![image](https://hackmd.io/_uploads/Hk3LWJboR.png)

Démonter les partitions temporaires
```bash 
sudo umount /mnt/PROFILS
```
### Modifications des partitions en changeant la partition de volume  (exemple)
**outil :**
- `rsync` utilisé principalement pour la synchronisation de fichiers et de répertoires entre différents emplacements.
- ```sudo apt install rsync```
**option :**
```bash
    -r # Recursif, copie les répertoires et leur contenu de manière récursive.
    -a # signifie "archive mode". C'est une option combinée pour que la synchronisation conserve l'intégrité des fichiers et répertoires
    -l # Conserve les liens symboliques.
    -p # Conserve les permissions.
    -t # Conserve les timestamps (date et heure de modification des fichiers).
    -g # Conserve le groupe.
    -o # Conserve le propriétaire.
    -D # Conserve les périphériques et les fichiers spéciaux.
    -v # Active le mode verbeux 
```
Après avoir vérifié que le répertoire n'avait pas de souci lors du montage, on prépare le répertoire que l'on souhaite déplacer de volume.

### Création d'un Répertoire de Sauvegarde
```bash
sudo mkdir /home_backup # creation du dossier backup a la racine 
sudo rsync -av /home/ /home_backup/ 
```
On peut aussi utiliser la commande `cp` pour copier les données sur notre backup :
```bash
sudo cp -rpv /home/* /home_backup/
```
#### Tips
On peut vérifier rapidement que les données ont été copiées
```bash
ls -la /home
ls -la /home_backup
```
### Démonter l'ancien `/home` si nécessaire

Si `/home` est sur une partition différente, vérifier avec `df -h`
```bash
sudo umount /home
```
Si on rencontre des difficultées pour démonter `/home`, utliser
```bash
sudo umount -l /home
```
### Monter PROFILS sur `/home`
```bash 
sudo mount /dev/sdb1 /home
```
**Attention** : si on a utilisé la commande `cp`, il faut recopier les données du backup sur notre home
```bash
sudo cp -rpv /home_backup/ /home/*
```
Vérifier que le volume est correctement monté
```bash
df -h | grep /home
```
A nouveau, on peut vérifier que les données soient bien copiées sur notre `/home`
```bash
ls -la /home
ls -la /home_backup
```
### Mettre à jour `/etc/fstab` pour le montage automatique
```bash
sudo nano /etc/fstab
```
Remplacer le point de montage de `/home`
![image](https://hackmd.io/_uploads/BJnS9JGoA.png)
![image](https://hackmd.io/_uploads/SkXA8C-iC.png)

Verifier les droit de chaque user 
```bash
ls -la /home
```
Ne pas oublier de supprimer le dossier backup une fois fini
```
sudo rm -rf /home_backup
```
### Monter les partitions automatique
Editer le ficher `/etc/fstab`

### Monter un volume avec dossier partagé 
Commencer par créer un repertoire commun exemple `/services`
```bash
sudo mkdir /services
```
Monter le volume exemple `DATA` sur /services
```bash
sudo mount /dev/sdb2 /services
```
Ajouter la ligne suivante pour que le volume soit monté automatiquement `/etc/fstab`
```bash
/dev/sdb2 /services ext4 defaults 0 2
```
On oublie pas la vérification habituelle
```bash
df -h /services
```
Créer les sous-repertoires
```bash
sudo mkdir -p /services/commercial
sudo mkdir -p /services/informatique
sudo mkdir -p /services/comptabilite
sudo mkdir -p /services/logistique
sudo mkdir -p /services/direction
```
Ajouter les permissions aux groupes concernés
```bash
chown :Commercial commercial
chown :Informatique informatique
chown :Comptabilite comptabilite
chown :Logistique logistique
chown :Direction direction
```
Puis accorder les droits requis, ici lecture+écriture aux seuls membres de chaque groupe
```bash
chmod g+x *
ou
chmod 770 *
```
# Configuration avancée système
Ici, on cherche à modifer la durée du chargeur d'amorçage grub
```bash
sudo nano /etc/default/grub
```
On modifie la ligne `GRUB_TIMEOUT=5` par `GRUB_TIMEOUT=2`

![image](https://hackmd.io/_uploads/rJFQjgfjR.png)

Ensuite, il faut faire une mise à jour du fichier
```bash
sudo update-grub
```
### Redimensioner le volume logique swap
 
```bash 
cat /ect/fstab #pour recuperer le chemin du swap
``` 
si swap activé 
```bash 
sudo swapoff /dev/chemin/du/swap 
```
Cette commande ajuste le volume logique swap pour utiliser tout l'espace libre restant dans le groupe de volume associé
```bash
sudo lvresize -l +100%FREE /dev/chemin/du/swap
```
Reformater le volume swap 
```bash
sudo mkswap /dev/chemin/du/swap
```

Vérification de la taille 
```bash
sudo lvdisplay /dev/chemin/du/swap
```
# Installation d'applications
```bash
sudo apt install remmina remmina -plugin-rdp
```
# Sauvegarde et restauration
Ici, on veut planifier une sauvegarde quotidienne sous forme de fichiers `tar`
On peut créer un script
```bash
nano /root/script.sh
```
On ajoute ces 3 lignes
```bash
#!/bin/bash
tar cf /root/HomeBack.tar /home
tar cf /root/ServicesBack.tar /services
```
N'oublions pas de modifier les permissions
```bash
chmod u+x /root/script.sh
```
Ensuite, on planifie le script avec une contrab
```bash
crontab -e
```
Et on ajoute dans la crontab
```bash
30 12 * * 1-5 /root/script.sh
```
La sauvegarde aura donc lieu tous les jours à 12h30.
### Sauvegarde partagée (à revoir)
On va aussi vouloir configurer une duplication des fichiers à l'aide de la commande `scp`, pour que les fichiers soient dispo sur un autre poste (ici celui du binôme de TP)
On vérifie si on à les paquets requis
```bash
apt-cache search scp
```
Puis on ajoute une ligne dans notre crontab
```bash
scp /root/*.tar userdubinome@ipdubinome:/nomdudossier
```
**Attention** : la ligne de commande est ajoutée à la suite de la première à l'aide d'un `&&`

![image](https://hackmd.io/_uploads/ryttGXziA.png)

# Partitionnement LVM
 On souhaite monter une partition xts dans le dossier `/var/log`
 
 Pour des raisons de sécurité, on passe en mode maintenance
 ```bash
 systemctl isolate rescue.target
 ou
 init 1
 ```
 On copie les données dans un dossier temporaire
 ```bash
 mkdir /mnt/logs
 cp -rvp /var/log /mnt/logs
 ```
 Puis on modifie le fichier `/etc/fstab`
 
* Méthode 1
```bash
sudo nano /etc/fstab
```
Et on ajoute ici la ligne `/dev/sdb3`

![image](https://hackmd.io/_uploads/SksoyEGiA.png)

* Méthode 2
```bash
echo "LABEL=LOGS /var/log xfs defaults 0 0" >> /etc/fstab
mount -a
```
Ensuite on peut recopier les données du dossier temporaire
```bash
cp -rvp /mnt/logs/* /var/log/
```
On peut déjà vérifier le contenu des 2 dossiers
```bash
ls -la /var/log
ls -la /mnt/logs
```
Maintenant on peut redémarrer la machine
```bash 
reboot 
```
Et on fini par vérifier le bon fonctionnement
```bash 
sudo tail /var/log/syslog 
```
## Configuration avancée des systèmes
On ajoute deux disques de 20G à notre machine dans le but de créer un volume logique LVM

Vérification des nouveaux disques 
```bash
lsblk -l
```
#### Créer des partitions sur les nouveaux disques

* Pour le disque /dev/sdc :

```bash
fdisk /dev/sdc
```
Dans fdisk :

Taper `n` pour créer une nouvelle partition.

Accepter les valeurs par défaut pour utiliser tout l'espace.

Taper `t` et entrez 8e pour définir le type comme "Linux LVM".

Taper `w` pour écrire les changements sur le disque.

Répéter ces étapes pour le disque /dev/sdd.

#### Créer des volumes physiques LVM
`pvcreate`
```bash
pvcreate /dev/sdc1 /dev/sdd1
```

#### Créer un groupe de volumes LVM

Créer un groupe de volumes nommé, par exemple `vg_opt` à partir des volumes physiques :

```bash
vgcreate vg_opt /dev/sdc1 /dev/sdd1
```

Créer un volume logique LVM

```bash
lvcreate -L 32G -n lv_opt vg_opt
```
Formater le volume logique en ext4

```bash
mkfs.ext4 /dev/vg_opt/lv_opt
```
Monter le volume logique sur /opt

```bash
mount /dev/vg_opt/lv_opt /opt
```
#### Rendre le montage permanent

Ouvrir le fichier /etc/fstab avec un éditeur de texte (ici nano)

```bash
sudo nano /etc/fstab
```
```bash
/dev/vg_opt/lv_opt /opt ext4 defaults 0 2
```

Vérification
```bash
reebot

df -h /opt
```