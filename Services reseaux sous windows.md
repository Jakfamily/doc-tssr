# Installation windows server / windows CLI

**Lexique**
- **Mode CORS** : Fonctionnement sans interface graphique.

## Prerequis 
#### windows server
```powershell=
2 ram
2 processors
32 gb disk 
```
#### windows 10
```powershell=
2 ram
2 processors
32 gb disk  
```
## Guide d'installation

### windows server / windows 10
```powershell=
boot sur le cd 
choisir langue 
horaire

activation ou non 
selectioner avec experience bureau pour linterface graphique 

selectioner personnaliser : installer uniquement windows 

choix mdp admin
maj mini chiffre ```

ctrl + alt + del pour deverouiller la machine ou bouton ![image](https://hackmd.io/_uploads/ryG2xAgTC.png)

pas oublier d'installer vmware tools 
```

## Preparation du  sysprep

**lexique**
- `Le sysprep` (System Preparation Tool) est un utilitaire de Windows utilisé pour préparer une image d'un système d'exploitation pour un déploiement sur plusieurs ordinateurs.


#### mode graphique
Sysprep se trouve déjà dans les installations de Windows. Pour y accéder :
```powershell=
ouvrir une session administrateur 
dans le répertoire C:\Windows\System32\Sysprep\
on trouveras l'exécutable sysprep.exe.
double click 
selectionner OOBE 
cochez la case 
Generaliser 
dans la section Option D'arret choissir arreter 
```
#### mode terminal 
```powershell=
ouvrir terminal en mode administrateur 

lancer -> C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown
```

## Configuration reseaux

### configuration ip et nom

```powershell=
w + r 
control 

reseaux et internet 
centre reseaux partage 
modifier les params carte 
choisir carte reseaux 
clic droigt propriteter 
doucle clic sur protocl interne version 4 
utiliser static 

config nom d'hote 
system 
changer nom 
```
### ajout de role et de fonctionaliter sur windows server
![image](https://hackmd.io/_uploads/Hk8nnK-p0.png)

```powershell=
gerer 
ajouter des role 
installer un role ou une fonctionaliter 
selectioner notre serveur 
liste des role 
cocher un serveur web iis par exemple 
suivant 

liste de fonctionaliter 
sauvegarde w server 
on touche pas au service du role 
redemarer le serveur si besion 
installation 

outils daministration windows 
un ensemble de console mmc 
pour verifier si tous et bien installer 

```
# Gestion du stockage et tables de partitionement 

**Lexique**

- **MBR** (Master Boot Record) : Format historique de table de partition, le BIOS recherche un secteur d'amorçage sur les médias bootables.

- **GPT** (GUID Partition Table) : Format utilisé par UEFI, offrant plus de flexibilité et de fiabilité que MBR.


## Types de configurations de disque

### Configuration de base
- Simplifie la gestion du disque.
- Toutes les données sont dans des **partitions**.
- Utilisation d'un seul disque.

### Configuration dynamique
- La gestion se fait par **ensemble de disques**.
- Les données sont inscrites dans des **volumes**.
- Nécessaire pour le **RAID logiciel**.

### Conversion 

![image](https://hackmd.io/_uploads/H1CqvZN6C.png)
 

## Types de partitions et volumes

- Un disque peut contenir **4 partitions principales** ou **3 partitions principales** et **1 partition étendue** dans laquelle il est possible de créer des lecteurs logiques.
  - Les **lecteurs logiques** peuvent être formatés.

### Partition dynamique

- **Volume simple** : Le seul type de volume qui utilise un seul disque physique.
  - Il peut être étendu sur le même disque.

- **Volume fractionné** : Un volume fractionné utilise plusieurs disques physiques.


## Types de RAID

### RAID 0 (agrégation par bandes)
- Répartit les données en bandes sur **2 disques** (ou plus). La taille des disques doit être égale.
- **Aucune tolérance aux pannes** : Si un disque échoue, toutes les données sont perdues.

![image](https://hackmd.io/_uploads/Sym4jWNaC.png)


### RAID 1 (miroir)
- Duplique les données sur **deux disques**.
- Offre une **tolérance aux pannes** : les données sont toujours accessibles si un des disques échoue.
- La capacité utile est équivalente à celle d'un seul disque, car les données sont entièrement dupliquées.

![image](https://hackmd.io/_uploads/HyErjZE60.png)


### RAID 5 (répartition avec parité)
**Meilleur compromis entre tolérance de panne et coûts**
- Nécessite au moins **3 disques**.
- Répartit les données et la **parité** (utilisée pour la récupération) sur plusieurs disques.
- Offre une **tolérance aux pannes** : un seul disque peut tomber en panne sans perte de données.
- La capacité totale équivaut à la somme des disques moins un.

![image](https://hackmd.io/_uploads/r19IiW4aR.png)


### RAID 10 (RAID 1+0)
- Combine les avantages du **RAID 1** (miroir) et du **RAID 0** (bande).
- Nécessite au moins **4 disques**.
- Les données sont d'abord dupliquées (comme dans RAID 1), puis réparties en bandes (comme dans RAID 0).
- Offre une **tolérance aux pannes** tout en améliorant les performances.
- La capacité utile est équivalente à la moitié de l'espace total disponible.

## Formatage et outils sous Windows

### Types de formatage

- **FAT32**
  - **Historique** : Utilisé principalement dans les versions antérieures de Windows, comme Windows 9x.
  - **Limites** : Taille maximale d'une partition de **2 To**.
  
- **NTFS**
  - **Historique** : Introduit avec Windows NT.
  - **Limites** : Taille maximale d'une partition de **256 To**.
  - **Caractéristiques** : Offre des fonctionnalités avancées telles que les permissions de fichiers, le chiffrement, et les journaux de transactions.

- **ReFS (Resilient File System)**
  - **Historique** : Introduit avec Windows Server 2012.
  - **Limites** : Supporte des tailles de volume bien plus grandes, avec une capacité de stockage largement supérieure à NTFS.
  - **Caractéristiques** : Conçu pour les environnements de stockage à haute disponibilité, avec des fonctionnalités telles que la correction d'erreurs et la protection contre la corruption des données.

### Les Outils 

- la console de gestion de disque (`diskmgmt.msc`)
- diskpart (`CMD`)
- powershell 
 
#### TP : Gestion des disques et volumes en RAID sous Windows

Objectif

Ajouter des disques SCSI, configurer des volumes RAID 5 et miroir, et gérer les lettres de lecteur.

1. Ajouter 3 disques SCSI de 10 Go

    - Accéder à la gestion des disques avec diskmgmt.msc.
    - Mettre les disques en ligne (online).
    - Initialiser les disques en utilisant une partition GPT.

2. Changer la lettre du lecteur CD

    - Ouvrir diskpart via Win + R.
    - Lister les volumes avec list volume.
    - Sélectionner le volume 0 (CD) : select volume 0.
    - Attribuer la lettre L : assign letter=L.

3. Créer un volume RAID 5 (hébergeant 6 Go de données)

  	- Ouvrir la gestion des disques (diskmgmt.msc).
	- Clic droit sur un espace non alloué, choisir Nouveau volume RAID 5.
	- Ajouter les 3 disques au volume RAID 5.
	- Attribuer la lettre D au nouveau volume.
	- Sélectionner le type de formatage (NTFS recommandé).
    - Terminer l'assistant pour créer le volume RAID 5.

4. Créer un volume miroir entre les disques 1 et 3 (4 Go)

    - Clic droit sur un espace non alloué des disques 1 et 3, choisir Nouveau volume en miroir.
    - Allouer 4 Go pour chaque disque.
    - Formater le volume après l'initialisation.

5. Créer un volume agrégé pour la base de données

    - Suivre le même principe que le volume miroir pour agrandir la bande passante dédiée à la base de données.

# Active Directory

## Définition
Active Directory sert à **centraliser** :
- La gestion de l'**authentification**.
- La gestion des **paramètres utilisateurs**.
- Fournir une **base de fonctionnement** aux services réseau.

### Protocoles utilisés
- **DNS**
- **LDAP**
- **Kerberos**

**lexique**
- **DNS** : Résolution des noms de machine et localisation des services réseau.
- **LDAP** : Norme pour les systèmes d'annuaire avec une structure de base de données.
- **Kerberos** : Protocole d'authentification reposant sur un mécanisme de clés secrètes et l'utilisation de tickets.

