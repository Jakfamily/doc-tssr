# Installation windows server / windows CLI

**lexique**
mode cors = sans interface graphique 

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
`Le sysprep` (System Preparation Tool) est un utilitaire de Windows utilisé pour préparer une image d'un système d'exploitation pour un déploiement sur plusieurs ordinateurs.


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