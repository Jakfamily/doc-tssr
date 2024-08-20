# Historique des Commandes
```bash
!!           # Exécuter la dernière commande
touch foo.sh
chmod +x !$  # !$ est le dernier argument de la dernière commande, c'est-à-dire foo.sh
```
# Manipulation Répertoires
## Navigation dans les Répertoires
```bash
pwd                       # Afficher le chemin du répertoire actuel
ls                        # Lister les répertoires
ls -a|--all               # Lister les répertoires, y compris les fichiers cachés
ls -l                     # Lister les répertoires sous forme longue
ls -l -h|--human-readable # Lister les répertoires sous forme longue avec des tailles lisibles
ls -t                     # Lister les répertoires par date de modification, du plus récent au plus ancien
stat foo.txt              # Lister la taille, les dates de création et de modification d'un fichier
stat foo                  # Lister la taille, les dates de création et de modification d'un répertoire
tree                      # Lister l'arborescence des répertoires et fichiers
tree -a                   # Lister l'arborescence des répertoires et fichiers, y compris cachés
tree -d                   # Lister l'arborescence des répertoires
cd foo                    # Aller au sous-répertoire foo
cd                        # Aller au répertoire personnel
cd ~                      # Aller au répertoire personnel
cd -                      # Aller au dernier répertoire
pushd foo                 # Aller au sous-répertoire foo et ajouter le répertoire précédent à la pile
popd                      # Revenir au répertoire dans la pile sauvegardé par `pushd`
```
## Création de Répertoires
```bash
mkdir foo                        # Créer un répertoire
mkdir foo bar                    # Créer plusieurs répertoires
mkdir -p|--parents foo/bar       # Créer un répertoire imbriqué
mkdir -p|--parents {foo,bar}/baz # Créer plusieurs répertoires imbriqués
mktemp -d|--directory            # Créer un répertoire temporaire
```
## Déplacement de Répertoires
```bash
cp -R|--recursive foo bar                               # Copier le répertoire
mv foo bar                                              # Déplacer le répertoire
rsync -z|--compress -v|--verbose /foo /bar              # Copier le répertoire, écrase la destination
rsync -a|--archive -z|--compress -v|--verbose /foo /bar # Copier le répertoire, sans écraser la destination
rsync -avz /foo username@hostname:/bar                  # Copier un répertoire local vers un répertoire distant
rsync -avz username@hostname:/foo /bar                  # Copier un répertoire distant vers un répertoire local
```
## Suppression de Répertoires
```bash
rmdir foo                        # Supprimer un répertoire vide
rm -r|--recursive foo            # Supprimer un répertoire y compris son contenu
rm -r|--recursive -f|--force foo # Supprimer un répertoire y compris son contenu, ignorer les fichiers inexistants et ne jamais demander
```
# Manipualtion de fichiers
## Création de Fichiers
```bash
touch foo.txt         # Créer un fichier ou mettre à jour le timestamp d'un fichier existant
touch foo.txt bar.txt # Créer plusieurs fichiers
touch {foo,bar}.txt   # Créer plusieurs fichiers
touch test{1..3}      # Créer test1, test2 et test3
touch test{a..c}      # Créer testa, testb et testc
mktemp                # Créer un fichier temporaire
```
## Sortie Standard, Erreur Standard et Entrée Standard
```bash
echo "foo" > bar.txt       # Écraser le fichier avec le contenu
echo "foo" >> bar.txt      # Ajouter au fichier avec le contenu
ls exists 1> stdout.txt    # Rediriger la sortie standard vers un fichier
ls noexist 2> stderror.txt # Rediriger la sortie d'erreur standard vers un fichier
ls 2>&1 > out.txt          # Rediriger la sortie standard et d'erreur vers un fichier
ls > /dev/null             # Éliminer la sortie standard et d'erreur
read foo                   # Lire à partir de l'entrée standard et écrire dans la variable foo
```
## Déplacement de Fichiers
```bash
cp foo.txt bar.txt                                 # Copier le fichier
mv foo.txt bar.txt                                 # Déplacer le fichier
rsync -z|--compress -v|--verbose /foo.txt /bar     # Copier le fichier rapidement s'il n'a pas changé
rsync -z|--compress -v|--verbose /foo.txt /bar.txt # Copier et renommer le fichier rapidement s'il n'a pas changé
```
## Suppression de Fichiers
```bash
rm foo.txt            # Supprimer le fichier
rm -f|--force foo.txt # Supprimer le fichier, ignorer les fichiers inexistants et ne jamais demander
```
## Lecture de Fichiers
```bash
cat foo.txt  # Afficher tout le contenu
less foo.txt # Afficher une partie du contenu à la fois (g - aller au début du fichier, SHIFT+g - aller à la fin du fichier, /foo pour rechercher 'foo')
head foo.txt # Afficher les 10 premières lignes du fichier
tail foo.txt # Afficher les 10 dernières lignes du fichier
open foo.txt # Ouvrir le fichier dans l'éditeur par défaut
wc foo.txt   # Lister le nombre de lignes, de mots et de caractères dans le fichier
```
## Permissions de Fichiers
| #   | Permission               | rwx | Binaire    |
| --- | ------------------------ | --- | --- |
| 7   | lire, écrire et exécuter | rwx | 111    |
| 6   | lire et écrire           | rw- | 110    |
| 5   | lire et exécuter         | r-x | 101    |
| 4   | lecture seule            | r-  | 100    |
| 3   | écrire et exécuter       | -wx | 011    |
| 2   | écrire seulement         | -w- | 010    |
| 1   | exécuter seulement       | -x  | 001    |
| 0   | aucun                    | --  | 000    |

Pour un répertoire, exécuter signifie que vous pouvez entrer dans un répertoire.

| Utilisateurs | Groupes | Description |
| -------- | -------- | -------- |
| 6 | 4 | L'utilisateur peut lire et écrire, tout le monde peut lire (Permissions de fichier par défaut) |
| 7 | 5 | L'utilisateur peut lire, écrire et exécuter, tout le monde peut lire et exécuter (Permissions de répertoire par défaut) |
```bash
ls -l /foo.sh            # Lister les permissions de fichier
chmod +100 foo.sh        # Ajouter 1 aux permissions de l'utilisateur
chmod -100 foo.sh        # Soustraire 1 aux permissions de l'utilisateur
chmod u+x foo.sh         # Donner la permission d'exécution à l'utilisateur
chmod g+x foo.sh         # Donner la permission d'exécution au groupe
chmod u-x,g-x foo.sh     # Retirer la permission d'exécution à l'utilisateur et au groupe
chmod u+x,g+x,o+x foo.sh # Donner la permission d'exécution à tout le monde
chmod a+x foo.sh         # Donner la permission d'exécution à tout le monde
chmod +x foo.sh          # Donner la permission d'exécution à tout le monde
```
## Recherche de Fichiers
Pour trouver des fichiers binaires pour une commande.
```bash
type wget    # Trouver le binaire
which wget   # Trouver le binaire
whereis wget # Trouver le binaire, la source et les fichiers de manuel
```
```locate``` utilise un index et est rapide.
```bash
updatedb             # Mettre à jour l'index
locate foo.txt       # Trouver un fichier
locate --ignore-case # Trouver un fichier en ignorant la casse
locate f*.txt        # Trouver un fichier texte commençant par 'f'
```
```find``` n'utilise pas d'index et est lent.
```bash
find /path -name foo.txt                   # Trouver un fichier
find /path -iname foo.txt                  # Trouver un fichier avec une recherche insensible à la casse
find /path -name "*.txt"                   # Trouver tous les fichiers texte
find /path -name foo.txt -delete           # Trouver un fichier et le supprimer
find /path -name "*.png" -exec pngquant {} # Trouver tous les fichiers .png et exécuter pngquant dessus
find /path -type f -name foo.txt           # Trouver un fichier
find /path -type d -name foo               # Trouver un répertoire
find /path -type l -name foo.txt           # Trouver un lien symbolique
find /path -type f -mtime +30              # Trouver des fichiers qui n'ont pas été modifiés depuis 30 jours
find /path -type f -mtime +30 -delete      # Supprimer des fichiers qui n'ont pas été modifiés depuis 30 jours
```
## Recherche dans les Fichiers
```bash
grep 'foo' /bar.txt                         # Rechercher 'foo' dans le fichier 'bar.txt'
grep 'foo' /bar -r|--recursive              # Rechercher 'foo' dans le répertoire 'bar'
grep 'foo' /bar -R|--dereference-recursive  # Rechercher 'foo' dans le répertoire 'bar' et suivre les liens symboliques
grep 'foo' /bar -l|--files-with-matches     # Afficher uniquement les fichiers qui correspondent
grep 'foo' /bar -L|--files-without-match    # Afficher uniquement les fichiers qui ne correspondent pas
grep 'Foo' /bar -i|--ignore-case            # Recherche insensible à la casse
grep 'foo' /bar -x|--line-regexp            # Correspondre à la ligne entière
grep 'foo' /bar -C|--context 1              # Ajouter N lignes de contexte au-dessus et en dessous de chaque résultat de recherche
grep 'foo' /bar -v|--invert-match           # Afficher uniquement les lignes qui ne correspondent pas
grep 'foo' /bar -c|--count                  # Compter le nombre de lignes qui correspondent
grep 'foo' /bar -n|--line-number            # Ajouter des numéros de ligne
grep 'foo' /bar --colour                    # Ajouter de la couleur à la sortie
grep 'foo\|bar' /baz -R                     # Rechercher 'foo' ou 'bar' dans le répertoire 'baz'
grep --extended-regexp|-E 'foo|bar' /baz -R # Utiliser des expressions régulières
egrep 'foo|bar' /baz -R                     # Utiliser des expressions régulières
```
## Remplacer dans les Fichiers
```bash
sed 's/fox/bear/g' foo.txt               # Remplacer fox par bear dans foo.txt et afficher dans la console
sed 's/fox/bear/gi' foo.txt              # Remplacer fox (insensible à la casse) par bear dans foo.txt et afficher dans la console
sed 's/red fox/blue bear/g' foo.txt      # Remplacer rouge par bleu et fox par bear dans foo.txt et afficher dans la console
sed 's/fox/bear/g' foo.txt > bar.txt     # Remplacer fox par bear dans foo.txt et enregistrer dans bar.txt
sed 's/fox/bear/g' foo.txt -i|--in-place # Remplacer fox par bear et écraser foo.txt
```
## Liens Symboliques
```bash
ln -s|--symbolic foo bar            # Créer un lien 'bar' vers le dossier 'foo'
ln -s|--symbolic -f|--force foo bar # Écraser un lien symbolique existant 'bar'
ls -l                               # Montrer où pointent les liens symboliques
```
## Compression de Fichiers
### zip

Compresse un ou plusieurs fichiers en fichiers *.zip.
```bash
zip foo.zip /bar.txt                # Compresser bar.txt dans foo.zip
zip foo.zip /bar.txt /baz.txt       # Compresser bar.txt et baz.txt dans foo.zip
zip foo.zip /{bar,baz}.txt          # Compresser bar.txt et baz.txt dans foo.zip
zip -r|--recurse-paths foo.zip /bar # Compresser le répertoire bar dans foo.zip
```
### gzip

Compresse un seul fichier en fichiers *.gz.
```bash
gzip /bar.txt foo.gz           # Compresser bar.txt dans foo.gz et supprimer bar.txt
gzip -k|--keep /bar.txt foo.gz # Compresser bar.txt dans foo.gz
```
### tar -c

Compresse (éventuellement) et combine un ou plusieurs fichiers en un seul fichier *.tar, *.tar.gz, *.tpz ou *.tgz.
```bash
tar -c|--create -z|--gzip -f|--file=foo.tgz /bar.txt /baz.txt # Compresser bar.txt et baz.txt dans foo.tgz
tar -c|--create -z|--gzip -f|--file=foo.tgz /{bar,baz}.txt    # Compresser bar.txt et baz.txt dans foo.tgz
tar -c|--create -z|--gzip -f|--file=foo.tgz /bar              # Compresser le répertoire bar dans foo.tgz
```
## Décompression de Fichiers
### unzip
```bash
unzip foo.zip # Décompresser foo.zip dans le répertoire actuel
```
### gunzip
```bash
gunzip foo.gz           # Décompresser foo.gz dans le répertoire actuel et supprimer foo.gz
gunzip -k|--keep foo.gz # Décompresser foo.gz dans le répertoire actuel
```
### tar -x
```bash
tar -x|--extract -z|--gzip -f|--file=foo.tar.gz # Décompresser foo.tar.gz dans le répertoire actuel
tar -x|--extract -f|--file=foo.tar              # Décompresser foo.tar dans le répertoire actuel
```
# Système Global
## Utilisation du Disque
```bash
df                     # Lister les disques, la taille, l'espace utilisé et disponible
df -h|--human-readable # Lister les disques, la taille, l'espace utilisé et disponible dans un format lisible
du                     # Lister le répertoire actuel, les sous-répertoires et les tailles de fichiers
du /foo/bar            # Lister le répertoire spécifié, les sous-répertoires et les tailles de fichiers
du -h|--human-readable # Lister le répertoire actuel, les sous-répertoires et les tailles de fichiers dans un format lisible
du -d|--max-depth      # Lister le répertoire actuel, les sous-répertoires et les tailles de fichiers dans la profondeur maximale
du -d 0                # Lister la taille du répertoire actuel
```
## Utilisation de la Mémoire
```bash
free                 # Afficher l'utilisation de la mémoire
free -h|--human      # Afficher l'utilisation de la mémoire dans un format lisible
free -h|--human --si # Afficher l'utilisation de la mémoire dans un format lisible en puissance de 1000 au lieu de 1024
free -s|--seconds 5  # Afficher l'utilisation de la mémoire et mettre à jour toutes les cinq secondes
```
## Paquets
```bash
apt update                   # Rafraîchir l'index du dépôt
apt search wget              # Rechercher un paquet
apt show wget                # Lister les informations sur le paquet wget
apt list --all-versions wget # Lister toutes les versions du paquet
apt install wget             # Installer la dernière version du paquet wget
apt install wget=1.2.3       # Installer une version spécifique du paquet wget
apt remove wget              # Supprimer le paquet wget
apt upgrade                  # Mettre à jour tous les paquets pouvant être mis à jour
```
## Arrêt et Redémarrage
```bash
shutdown                    # Arrêter dans 1 minute
shutdown now "À bientôt"    # Arrêter immédiatement
shutdown +5 "À bientôt"     # Arrêter dans 5 minutes
shutdown --reboot           # Redémarrer dans 1 minute
shutdown -r now "À bientôt" # Redémarrer immédiatement
shutdown -r +5 "À bientôt"  # Redémarrer dans 5 minutes
shutdown -c                 # Annuler un arrêt ou un redémarrage
reboot                      # Redémarrer maintenant
reboot -f                   # Forcer un redémarrage
```
## Identification des Processus
```bash
top             # Lister tous les processus de manière interactive
htop            # Lister tous les processus de manière interactive
ps all          # Lister tous les processus
pidof foo       # Retourner le PID de tous les processus foo
CTRL+Z          # Suspendre un processus en cours d'exécution au premier plan
bg              # Reprendre un processus suspendu et l'exécuter en arrière-plan
fg              # Ramener le dernier processus en arrière-plan au premier plan
fg 1            # Ramener le processus en arrière-plan avec le PID au premier plan
sleep 30 &      # Dormir pendant 30 secondes et déplacer le processus en arrière-plan
jobs            # Lister tous les travaux en arrière-plan
jobs -p         # Lister tous les travaux en arrière-plan avec leur PID
lsof            # Lister tous les fichiers ouverts et le processus qui les utilise
lsof -itcp:4000 # Retourner le processus écoutant sur le port 4000
```
## Priorité des Processus

Les priorités des processus vont de -20 (plus élevé) à 19 (plus bas).
```bash
nice -n -20 foo # Changer la priorité du processus par nom
renice 20 PID   # Changer la priorité du processus par PID
ps -o ni PID    # Retourner la priorité du processus de PID
```
## Tuer des Processus
```bash
CTRL+C       # Tuer un processus en cours d'exécution au premier plan
kill PID     # Arrêter le processus par PID de manière gracieuse. Envoie le signal TERM.
kill -9 PID  # Forcer l'arrêt du processus par PID. Envoie le signal SIGKILL.
pkill foo    # Arrêter le processus par nom de manière gracieuse. Envoie le signal TERM.
pkill -9 foo # Forcer l'arrêt du processus par nom. Envoie le signal SIGKILL.
killall foo  # Tuer tous les processus avec le nom spécifié de manière gracieuse.
```
## Date & Heure
```bash
date               # Afficher la date et l'heure
date --iso-8601    # Afficher la date au format ISO8601
date --iso-8601=ns # Afficher la date et l'heure au format ISO8601
time tree          # Mesurer le temps d'exécution de la commande tree
```
## Tâches Planifiées
```
* * * * *
Minute, Heure, Jour du mois, Mois, Jour de la semaine
```
```bash
crontab -l                 # Lister le crontab
crontab -e                 # Éditer le crontab dans Vim
crontab /path/crontab      # Charger le crontab depuis un fichier
crontab -l > /path/crontab # Sauvegarder le crontab dans un fichier
* * * * * foo              # Exécuter foo chaque minute
*/15 * * * * foo           # Exécuter foo toutes les 15 minutes
0 * * * * foo              # Exécuter foo chaque heure
15 6 * * * foo             # Exécuter foo quotidiennement à 6h15
44 4 * * 5 foo             # Exécuter foo chaque vendredi à 4h44
0 0 1 * * foo              # Exécuter foo à minuit le premier du mois
0 0 1 1 * foo              # Exécuter foo à minuit le premier de l'année
at -l                      # Lister les tâches programmées
at -c 1                    # Afficher la tâche avec l'ID 1
at -r 1                    # Supprimer la tâche avec l'ID 1
at now + 2 minutes         # Créer une tâche dans Vim à exécuter dans 2 minutes
at 12:34 PM next month     # Créer une tâche dans Vim à exécuter à 12h34 le mois prochain
at tomorrow                # Créer une tâche dans Vim à exécuter demain
```
# Réseaux
## Requêtes HTTP
```bash
curl https://example.com                                                                                 # Retourner le corps de la réponse
curl -i|--include https://example.com                                                                    # Inclure le code d'état et les en-têtes HTTP
curl -L|--location https://example.com                                                                   # Suivre les redirections
curl -o|--remote-name foo.txt https://example.com                                                        # Sortir vers un fichier texte
curl -H|--header "User-Agent: Foo" https://example.com                                                   # Ajouter un en-tête HTTP
curl -X|--request POST -H "Content-Type: application/json" -d|--data '{"foo":"bar"}' https://example.com # POST JSON
curl -X POST -H --data-urlencode foo="bar" http://example.com                                            # POST encodé en URL
wget https://example.com/file.txt .                                                                      # Télécharger un fichier dans le répertoire courant
wget -O|--output-document foo.txt https://example.com/file.txt                                           # Sortir vers un fichier avec le nom spécifié
```
## Dépannage Réseau
```bash
ping example.com                                                    # Envoyer plusieurs requêtes ping en utilisant le protocole ICMP
ping -c 10 -i 5 example.com                                         # Faire 10 tentatives, espacées de 5 secondes
ip addr                                                             # Lister les adresses IP sur le système
ip route show                                                       # Afficher les adresses IP vers le routeur
netstat -i|--interfaces                                             # Lister toutes les interfaces réseau et leur utilisation
netstat -l|--listening                                              # Lister tous les ports ouverts
traceroute example.com                                              # Lister tous les serveurs par lesquels passe le trafic réseau
mtr -w|--report-wide example.com                                    # Lister continuellement tous les serveurs par lesquels passe le trafic réseau
mtr -r|--report -w|--report-wide -c|--report-cycles 100 example.com # Sortir un rapport qui liste le trafic réseau 100 fois
nmap 0.0.0.0                                                        # Scanner les 1000 ports ouverts les plus courants sur localhost
nmap 0.0.0.0 -p1-65535                                              # Scanner les ports ouverts sur localhost entre 1 et 65535
nmap 192.168.4.3                                                    # Scanner les 1000 ports ouverts les plus courants sur une adresse IP distante
nmap -sP 192.168.1.1/24                                             # Découvrir toutes les machines sur le réseau en les pingant
```
## DNS
```bash
host example.com     # Afficher les adresses IPv4 et IPv6
dig example.com      # Afficher les informations DNS complètes
cat /etc/resolv.conf # resolv.conf liste les serveurs de noms
```
# Matériel
```bash
lsusb # Lister les périphériques USB
lspci # Lister le matériel PCI
lshw  # Lister tout le matériel
```
# Multiplexeurs de Terminal
Démarrer plusieurs sessions de terminal. Les sessions actives persistent après les redémarrages. ```tmux``` est plus moderne que ```screen```.
```bash
tmux             # Démarrer une nouvelle session (CTRL-b + d pour détacher)
tmux ls          # Lister toutes les sessions
tmux attach -t 0 # Rattacher à une session
screen           # Démarrer une nouvelle session (CTRL-a + d pour détacher)
screen -ls       # Lister toutes les sessions
screen -R 31166  # Rattacher à une session
exit             # Quitter une session
```
# Protocole de Shell Sécurisé (SSH)
```bash
ssh hostname                 # Se connecter à hostname en utilisant votre nom d'utilisateur actuel sur le port SSH par défaut 22
ssh -i foo.pem hostname      # Se connecter à hostname en utilisant le fichier d'identité
ssh user@hostname            # Se connecter à hostname en utilisant l'utilisateur sur le port SSH par défaut 22
ssh user@hostname -p 8765    # Se connecter à hostname en utilisant l'utilisateur sur un port personnalisé
ssh ssh://user@hostname:8765 # Se connecter à hostname en utilisant l'utilisateur sur un port personnalisé
```
Définir l'utilisateur et le port par défaut dans ```~/.ssh/config```, afin que vous puissiez simplement entrer le nom la prochaine fois :
```bash
$ cat ~/.ssh/config
Host name
User foo
Hostname 127.0.0.1
Port 8765
$ ssh name
```
# Copie Sécurisée
```bash
scp foo.txt ubuntu@hostname:/home/ubuntu # Copier foo.txt dans le répertoire distant spécifié
```
# BASH
## Profil Bash
* bash - ```.bashrc```
* zsh - ```.zshrc```
```bash
# Toujours exécuter ls après cd
function cd {
builtin cd "$@" && ls
}

# Demander à l'utilisateur avant d'écraser des fichiers
alias cp='cp --interactive'
alias mv='mv --interactive'
alias rm='rm --interactive'

# Toujours afficher l'utilisation du disque dans un format lisible par l'homme
alias df='df -h'
alias du='du -h'
```
## Script Bash
### Variables
```bash
#!/bin/bash
foo=123                # Initialiser la variable foo avec 123
declare -i foo=123     # Initialiser un entier foo avec 123
declare -r foo=123     # Initialiser une variable en lecture seule foo avec 123
echo $foo              # Afficher la variable foo
echo ${foo}_'bar'      # Afficher la variable foo suivie de _bar
echo ${foo:-'default'} # Afficher la variable foo si elle existe sinon afficher default
export foo             # Rendre foo disponible pour les processus enfants
unset foo              # Rendre foo indisponible pour les processus enfants
```
### Variables d'Environnement
```bash
#!/bin/bash
env            # Lister toutes les variables d'environnement
echo $PATH     # Afficher la variable d'environnement PATH
export FOO=Bar # Définir une variable d'environnement
```
### Fonctions
```bash
#!/bin/bash
greet() {
local world = "World"
echo "$1 $world"
return "$1 $world"
}

greet "Hello"
greeting=$(greet "Hello")
```
### Codes de Sortie
```bash
#!/bin/bash
exit 0  # Quitter le script avec succès
exit 1  # Quitter le script sans succès
echo $? # Afficher le dernier code de sortie
```
### Instructions Conditionnelles
#### Opérateurs Booléens

* ```$foo``` - Est vrai
* ```!$foo``` - Est faux

#### Opérateurs Numériques

* ```-eq``` - Égal
* ```-ne``` - Pas égal
* ```-gt``` - Supérieur à
* ```-ge``` - Supérieur ou égal à
* ```-lt``` - Inférieur à
* ```-le``` - Inférieur ou égal à
* ```-e``` foo.txt - Vérifier si le fichier existe
* ```-z``` foo - Vérifier si la variable existe

#### Opérateurs de Chaîne

* ```=``` - Égal
* ```==``` - Égal
* ```-z``` - Est nul
* ```-n``` - N'est pas nul
* ```<``` - Est inférieur dans l'ordre alphabétique ASCII
* ```>``` - Est supérieur dans l'ordre alphabétique ASCII
 
#### Instructions If
```bash
#!/bin/bash
if [[$foo = 'bar']]; then
echo 'un'
elif [[$foo = 'bar']] || [[$foo = 'baz']]; then
echo 'deux'
elif [[$foo = 'ban']] && [[$USER = 'bat']]; then
echo 'trois'
else
echo 'quatre'
fi
```
#### Instructions If Inline
```bash
#!/bin/bash
[[ $USER = 'rehan' ]] && echo 'oui' || echo 'non'
```
#### Boucles While
```bash
#!/bin/bash
declare -i counter
counter=10
while [$counter -gt 2]; do
echo Le compteur est $counter
counter=counter-1
done
```
#### Boucles For
```bash
#!/bin/bash
for i in {0..10..2}
do
echo "Index: $i"
done
for filename in file1 file2 file3
do
echo "Contenu: " >> $filename
done
for filename in *;
do
echo "Contenu: " >> $filename
done
```
#### Instructions Case
```bash
#!/bin/bash
echo "Quel temps fera-t-il demain ?"
read weather
case $weather in
sunny | warm ) echo "Beau temps : " $weather
;;
cloudy | cool ) echo "Pas mal de temps : " $weather
;;
rainy | cold ) echo "Temps terrible : " $weather
;;
* ) echo "Ne comprend pas"
;;
esac
```