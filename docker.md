# Docker 

## Commande de base docker 

### lexique : 
`registry` : logiciel qui permet de partager des images a d'autre personne.
exemple: docker hub 


### Affichez l'ensemble des conteneurs existants


```docker=
docker images # Lister les images disponibles sur l'ordinateur
docker ps # Lister les conteneurs en cours d'exécution (non pas les images)
```
### Recuperez une image du Docker Hub
```docker=
docker pull <nom_de_l_image> # Télécharger une image existante depuis le Docker Hub ou un autre registre Docker
```
### Lancer un conteneur docker ou l'arretez
```docker=
docker run -it <nom_de_l_image> # Lancer un conteneur interactif à partir de l'image spécifiée

docker run -it -d <nom_de_l_image> # Lancer un conteneur en mode détaché (en arrière-plan) à partir de l'image spécifiée

docker stop <container_id> # Arrêter le conteneur avec l'ID spécifié
```

### Demarrez un serveur nginnx avec docker 
```docker=
docker run -d -p 8080:80 nginx # -d on detache -p pour utilisation de port 
```
#### rentrer dans le conteneur docker 
```docker=
docker exec -ti ID_RETOURNÉ_LORS_DU_DOCKER_RUN bash
```
- `-ti` permet d'avoir un shell bash 

**exemple :**
rentrer dans le conteneur pour naviguer au `cd /usr/share/nginx/html` afin de modifier le fichier `index.html` et voir le resultat sur `localhost:8080`

### suprimer conteneur docker 
```docker=
docker rm <container-id>
```

### nettoyer son systeme 
```docker=
docker system prune
```
Celle-ci va supprimer les données suivantes :

* l'ensemble des conteneurs Docker qui ne sont pas en status running ;
* l'ensemble des réseaux créés par Docker qui ne sont pas utilisés par au moins un conteneur ;
* l'ensemble des images Docker non utilisées ;
* l'ensemble des caches utilisés pour la création d'images Docker.

## Dockerfile 

### lexique : 
* `FROM` qui vous permet de définir l'image source ;

* `RUN` qui vous permet d’exécuter des commandes dans votre conteneur ;

* `ADD` qui vous permet d'ajouter des fichiers dans votre conteneur ;

* `WORKDIR` qui vous permet de définir votre répertoire de travail ;

* `EXPOSE` qui permet de définir les ports d'écoute par défaut ;

* `VOLUME` qui permet de définir les volumes utilisables ;

* `CMD` qui permet de définir la commande par défaut lors de l’exécution de vos conteneurs Docker.

### Dans le repertoire racine du projet 
```docker=
cree un fichier dockerfile

## indique l'image a utiliser comme base utilisable q'une seul fois danzs le fichier dockerfile.
FROM debian:12 

#RUN permet d'executer des commande dans le conteneur
RUN apt-get update -yq \
&& apt-get install curl gnupg -yq \
&& curl -sL https://deb.nodesource.com/setup_10.x | bash \
&& apt-get install nodejs -yq \
&& apt-get clean -y

#permet de copier ou telecharger des fichier dans l'image 
ADD ./app/

#WORDIR commande equivalante a cd 
WORKDIR /app

RUN npm install 

#EXPOSE permet d'indiquer sur quelle port l'app ecoute 
#VOLUME  permet d'imdiquer quel repertoire je veut partager avec l'hote

EXPOSE 2368
VOLUME /app/logs

#CMD obligatop9ire a chaque fin de fichier permet au conteneur de savoir quelle commande il doit exécuter lors de son démarrage

CMD npm run start 
```

de la meme maniere que gitignore cree a la racine un .dockerignore qui contiendra 
node_modules
.git


### Profitez de l'optimisation Docker
```docker=
docker build 
```
permet a docker de cree chaque coneteneur pour chaque instruction 
permet de gagner du temps car si ya pas de modification sur l'instruction il va pas rebuild 

### Lancez votre conteneur personnalise !

cree l'image docker 
```docker=
docker build -t <nom_de_l_image> .
```
* `-t` permet de donner un nom a l'image
* `.` indique ou le dockerfile ce trouvent (dans l'exemple dans le repertoire courant )

## Creez votre image sur le Docker Hub

### lexique : 
* docker push qui vous permet d'envoyer vos images locales sur une registry ;

* docker search qui vous permet de rechercher une image sur votre registry.

### Envoyez votre image sur le Docker Hub
cree un compte sur docker hub 
<https://hub.docker.com/>

->create a repository 
->nom 
->description 
->visibilite
->create

#### Pour envoyer une image vers un repository 

```Docker
#La commande suivante crée un lien entre votre image locale et le nom que vous souhaitez donner sur le Docker Hub (ou un autre registre).
docker tag <nom_de_l_image>:<tag_local> <username>/<nom_de_l_image>:<tagname>

#Ensuite, utilisez cette commande pour envoyer l'image taguée vers le repository.
docker push <username>/<nom_de_l_image>:<tagname>
```
Quelques points à noter : 
* Le tag :latest est le tag par défaut. Si aucun tag n'est spécifié, Docker applique ce tag à l'image.

* Il est recommandé de spécifier des tags explicites pour gérer différentes versions de vos images, par exemple :v1.0 ou :dev.

* L'utilisation de tags clairs facilite la gestion des versions et le déploiement des images Docker.

### trouvez une images en ligne de commande

```docker=
docker search <nom_de_l_image>
```

## Docker compose 

### lexique
* `image` qui permet de spécifier l'image source pour le conteneur ;

* `build` qui permet de spécifier le Dockerfile source pour créer l'image du conteneur ;

* `volume` qui permet de spécifier les points de montage entre le système hôte et les conteneurs ;

* `restart` qui permet de définir le comportement du conteneur en cas d'arrêt du processus ;

* `environment` qui permet de définir les variables d’environnement ;

* `depends_on` qui permet de dire que le conteneur dépend d'un autre conteneur ;

* `ports` qui permet de définir les ports disponibles entre la machine host et le conteneur.

### commande de base 
besion d'un fichier `docker-compose.yml`

```docker=
docker-compose up -d # permettra de démarrer l'ensemble des conteneurs en arrière-plan ;

docker-compose ps # permettra de voir le statut de l'ensemble de votre stack ;

docker-compose logs -f --tail 5 # permettra d'afficher les logs de votre stack ;

docker-compose stop # permettra d'arrêter l'ensemble des services d'une stack ;

docker-compose down # permettra de détruire l'ensemble des ressources d'une stack ;

docker-compose config # permettra de valider la syntaxe de votre fichier docker-compose.yml  .
```

### ecrire des fichier docker-compose 

créer un fichier `docker-compose.yml` à la racine de votre projet

definir la version de docker compose 
```docker=
version: `3.8` 
```

L'ensemble des conteneurs qui doivent être créés doivent être définis sous l'argument `services` . Chaque conteneur commence avec un nom qui lui est propre ; dans notre cas, notre premier conteneur se nommera db  .

```docker=
version: '3.8'  # Vous pouvez ajuster la version en fonction de vos besoins

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

volumes:
  db_data:
```

Explications :

* **version** : Définit la version de la syntaxe de Docker Compose. 3.8 est une version stable et largement compatible.

* **services** : Contient la définition des services (conteneurs) à déployer.
  * **db** : Nom du service (conteneur) pour la base de données.
    * **image** : Spécifie l'image Docker à utiliser, ici `mysql:5.7`.
    * **volumes** : Permet de faire persister les données de la base de données. `db_data` est un volume Docker qui sera monté à l'emplacement `/var/lib/mysql` dans le conteneur.
    * **restart** : Définit la politique de redémarrage du conteneur. `always` signifie que le conteneur redémarrera automatiquement s'il s'arrête.
    * **environment** : Définit les variables d'environnement pour configurer MySQL.

* **volumes** : Définit le volume `db_data` utilisé pour stocker les données persistantes.


### second services : wordpress

```docker=
services:
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
``` 
* **wordpress** : Service (conteneur) pour l'application WordPress.
  * **depends_on** : Assure que le service `db` est démarré avant le service `wordpress`. Cela garantit que la base de données est prête à être utilisée lorsque WordPress démarre.
  * **image** : Spécifie l'image Docker à utiliser pour ce service, ici `wordpress:latest`, ce qui signifie que la dernière version stable de WordPress sera utilisée.
  * **ports** : Configure le mappage des ports entre l'hôte et le conteneur. `8000:80` signifie que le port 80 du conteneur sera exposé sur le port 8000 de l'hôte. Vous pouvez accéder à WordPress via `http://localhost:8000`.
  * **restart** : Définit la politique de redémarrage du conteneur. `always` signifie que le conteneur redémarrera automatiquement en cas d'arrêt.
  * **environment** : Définit les variables d'environnement pour configurer la connexion à la base de données MySQL.
    * **WORDPRESS_DB_HOST** : Spécifie l'hôte et le port de la base de données MySQL. `db:3306` indique que le service `db` (MySQL) écoute sur le port 3306.
    * **WORDPRESS_DB_USER** : Nom d'utilisateur pour la connexion à la base de données. `wordpress` est utilisé ici.
    * **WORDPRESS_DB_PASSWORD** : Mot de passe pour la connexion à la base de données. `wordpress` est utilisé ici.
    * **WORDPRESS_DB_NAME** : Nom de la base de données que WordPress utilisera. `wordpress` est utilisé ici.

