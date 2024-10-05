----
en cours de redaction 

----

# Installation de Python et Configuration d'Ansible

Ce document fournit des instructions pour installer Python, créer un environnement virtuel et installer Ansible sur votre machine.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les éléments suivants :

- Un système d'exploitation Linux.
- Accès à la ligne de commande ou à un terminal.

## Étape 1 : Installer Python

### Sur Ubuntu/Debian

1. Mettre à jour les paquets :
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. Installer Python et les outils de développement :
   ```bash
   sudo apt install python3 python3-pip python3-venv -y
   ```

## Créer un Environnement Virtuel

Une fois Python installé, vous pouvez créer un environnement virtuel pour isoler vos dépendances :

1. Créez un environnement virtuel :
   ```bash
   venv ansible
   ```

3. Activez l'environnement virtuel :

   - **Sur Linux :**
  	```bash
  	source venv/bin/activate
		 
	tips cree un alias activate-ansible="source venv/bin/activate"
  	```

## Installer Ansible

Avec l'environnement virtuel activé, installez Ansible à l'aide de pip :

```bash
pip install ansible
```

## Vérifier l'Installation

Pour vérifier que l'installation d'Ansible a réussi, exécutez la commande suivante :

```bash
ansible --version
```

