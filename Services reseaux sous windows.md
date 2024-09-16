--- 
En cours de rédaction 

---
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
note : lors du clonage de vm le sid et dupliques il seras necessaire de refaire un sysprep sur la vm cloner 

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

### Penser a auhtoriser les ping 

#### Méthode 1 : Via l'interface graphique (Pare-feu Windows)

1. **Ouvrir le Pare-feu Windows avec fonctions avancées :**
   - Appuie sur `Win + R`, tape `wf.msc`, puis appuie sur `Entrée`.
   - Cela ouvrira la console de gestion du pare-feu.

2. **Créer une règle entrante pour les requêtes ICMP :**
   - Dans le panneau de gauche, clique sur **Règles de trafic entrant**.
   - Dans le panneau de droite, clique sur **Nouvelle règle**.

3. **Configurer la nouvelle règle :**
   - Sélectionne **Personnalisée** et clique sur **Suivant**.
   - Dans la section **Type de programme**, choisis **Tous les programmes**.
   - Pour le **Type de protocole**, sélectionne **ICMPv4** (ceci est pour les requêtes IPv4, tu peux aussi ajouter ICMPv6 si nécessaire).
   - Laisse le champ **Adresse IP source et destination** par défaut, sauf si tu veux restreindre les pings à des IP spécifiques.

4. **Action de la règle :**
   - Sélectionne **Autoriser la connexion**.

5. **Appliquer à des profils réseau :**
   - Choisis les profils sur lesquels la règle s'applique (**Domaine**, **Privé**, **Public**), selon les besoins.

6. **Nommer la règle :**
   - Donne un nom à la règle, par exemple **"Autoriser Ping ICMPv4"**, puis clique sur **Terminer**.

#### Méthode 2 : Via la ligne de commande (PowerShell)

1. **Ouvrir une fenêtre PowerShell en tant qu'administrateur :**
   - Appuie sur `Win + X`, puis sélectionne **Windows PowerShell (Admin)** ou **Invite de commandes (Admin)**.

2. **Ajouter la règle pour autoriser les pings :**
   - Utilise la commande suivante pour autoriser les pings ICMP :

     ```powershell
     New-NetFirewallRule -DisplayName "Autoriser Ping ICMPv4" -Protocol ICMPv4 -Direction Inbound -Action Allow -Profile Any
     ```

   - Cette commande crée une règle pour autoriser les pings ICMPv4 dans tous les profils réseau (**domaine**, **privé**, **public**).

#### Méthode 3 : Via l'Invite de Commandes (cmd)

1. **Ouvrir l'Invite de commandes en tant qu'administrateur :**
   - Appuie sur `Win + X` et sélectionne **Invite de commandes (Admin)**.

2. **Ajouter la règle pour autoriser les pings :**
   - Utilise cette commande pour activer les pings ICMP :

     ```cmd
     netsh advfirewall firewall add rule name="Autoriser Ping" protocol=icmpv4:any,any dir=in action=allow
     ```

   - Cette commande ajoute une règle qui autorise les pings ICMPv4.

 
 
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

5. Créer un volume agrégé par bande pour la base de données

    - Suivre le même principe que le volume miroir pour agrandir la bande passante dédiée à la base de données.

# Active Directory

## Définition
Active Directory sert à **centraliser** :
- La gestion de l'**authentification**.
- La gestion des **paramètres utilisateurs**.
- Fournir une **base de fonctionnement** aux services réseau.
- Un **Active Directory (AD)** est créé et installé sur un serveur Windows Server, qui devient alors un **contrôleur de domaine (DC)**

### Protocoles utilisés
- **DNS**
- **Kerberos**

**lexique**
- **DNS** : Résolution des noms de machine et localisation des services réseau. port `53` UDP, TCP au dela de 53ko
- **LDAP** : Norme pour les systèmes d'annuaire avec une structure de base de données.
- **Kerberos** : Protocole d'authentification reposant sur un mécanisme de clés secrètes et l'utilisation de tickets.

![image](https://hackmd.io/_uploads/HJhjUhNp0.png)


### installation d'un AD 
```powershell=
gerer 
ajout de role et fonctionaliter 
installation basee sur un role ou une fonctionalite
Service AD DS 
ajoute les fonctionaliter
continuer 
redemarer si besion auomatiquement 
et installer 
```

### configuration post deploiment 
```powershell=
ajouter une nouvelle foret 
nom de domaine racine exemple.local
selectioner le niveau fonctionelle de la foret 
cree un DSRM mot de passe fort sert en cas de restauratyion de l'AD
suivant on passe le message dns dans un premier temps 
nom de domaine netbios et le nom de domaine sans le .local 
specifier les chemin par defaut on touche pas 
voir recap 
attente de validation pour savoir si cest ok 
installer 
``` 
ne pas oublier d'ajouter le nom de domaine au autres machine et preferer l'ip du controller de domaine en dns 

add l'ip du controleur de domaine
puis ajouter le nom de domaine pour ce faire 
syteme -> apropos  -> parametre avancer du systeme -> nom de lordinateur ->
![image](https://hackmd.io/_uploads/HJVKmFS6R.png)

afin de visualiser le bonne ajouts on peut le verifier garce a sa console 

```powershell=
win + r 
tapez dsa.msc
puis naviguer dans le nom de domaine 
computer et user 
```
## La base de la gestion dun domaine 
**La création de l'arborescence doit être bien comprise et réalisée avant de monter un Active Directory (AD)**

La bonne pratique veut qu'on crée une unité d'organisation et qu'on coche l'option 'Protéger l'objet contre la suppression accidentelle'

```powershell=
Ouvrez la console Ad (dsa.msc)
Créez l'uniter dorganisation principale "Mon Entreprise" : 
	Clic droit sur le domaine
	Sélectionnez "Nouveau" > "Unité d'organisation"
	Nommez-la "Mon Entreprise"
Créez des sous-UOs pour représenter la structure de votre entreprise. 
```
Par exemple :

![image](https://hackmd.io/_uploads/H1_o1nr6A.png)

Ensuite, afin d'éviter le plus d'erreurs possibles, il est possible de créer des modèles. Nous pouvons commencer par créer les groupes dont nous aurons besoin. Pour minimiser les risques d'erreurs, nous allons concevoir des modèles avec différentes restrictions afin d'explorer chaque possibilité.




 