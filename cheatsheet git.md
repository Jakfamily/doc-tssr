# Cheat Sheet Git

### 1. **Modifier le dernier commit**
   ```bash
   git commit --amend
   ```
   Utilisé pour modifier le dernier commit.

### 2. **Réécrire l'historique d'une branche**
   ```bash
   git rebase
   ```
   Permet de réécrire l’historique d’une branche.

### 3. **Mettre de côté le développement en cours**
   ```bash
   git stash
   ```
   Utilisé pour mettre de côté le développement en cours.

### 4. **Récupérer le développement mis de côté**
   ```bash
   git stash pop
   ```
   Permet de récupérer le développement mis de côté.

### 5. **Identifier un commit problématique**
   ```bash
   git bisect
   ```
   Utilisé pour identifier un commit problématique en parcourant l’historique.

### 6. **Annuler un commit**
   ```bash
   git revert
   ```
   Permet d’annuler un commit.


# Cheat Sheet Git Flow

### Installation de Git Flow
```bash
brew install git-flow # Pour macOS avec Homebrew
apt-get install git-flow # Pour Debian/Ubuntu
```

### Initialiser Git Flow
```bash
git flow init
```
Initialise un nouveau dépôt Git Flow. Suivez les instructions pour définir les noms des branches.

### Démarrer une nouvelle fonctionnalité
```bash
git flow feature start <feature-name>
```
Crée et bascule sur une nouvelle branche de fonctionnalité.

### Finir une fonctionnalité
```bash
git flow feature finish <feature-name>
```
Fusionne la branche de fonctionnalité dans `develop` et supprime la branche de fonctionnalité.

### Démarrer un correctif
```bash
git flow hotfix start <hotfix-name>
```
Crée et bascule sur une nouvelle branche de correctif à partir de `master`.

### Finir un correctif
```bash
git flow hotfix finish <hotfix-name>
```
Fusionne le correctif dans `master` et `develop`, et supprime la branche de correctif.

### Démarrer une version
```bash
git flow release start <release-version>
```
Crée une nouvelle branche de version à partir de `develop`.

### Finir une version
```bash
git flow release finish <release-version>
```
Fusionne la branche de version dans `master` et `develop`, crée un tag pour la version, et supprime la branche de version.

### Visualiser l'état des branches
```bash
git flow feature list
git flow hotfix list
git flow release list
```
Affiche la liste des fonctionnalités, correctifs ou versions en cours.

### Annuler une fonctionnalité en cours
```bash
git flow feature abort
```
Abandonne la création d'une fonctionnalité et supprime la branche de fonctionnalité.

### Annuler un correctif en cours
```bash
git flow hotfix abort
```
Abandonne la création d'un correctif et supprime la branche de correctif.

# Automatiser des traitement grace au hook

### 1. **Introduction aux Hooks Git**
Les hooks Git sont des scripts qui s'exécutent automatiquement à des moments spécifiques du cycle de vie de Git, permettant d'automatiser certaines actions.

### 2. **Localisation des Hooks**
Les hooks se trouvent dans le répertoire `.git/hooks` de votre dépôt. Les fichiers `.sample` sont des exemples de hooks désactivés.

### 3. **Types de Hooks Communs**

#### a. **`pre-commit`**
- **Déclenchement :** Avant la création d'un commit.
- **Utilisation :** Exécute des vérifications avant d'accepter le commit.
  
  ```bash
  #!/bin/sh
  # Exécuter des tests
  npm test
  ```

#### b. **`prepare-commit-msg`**
- **Déclenchement :** Avant l'édition du message de commit.
- **Utilisation :** Pré-remplit le message de commit.

#### c. **`commit-msg`**
- **Déclenchement :** Après la saisie du message de commit.
- **Utilisation :** Valide le format du message de commit.

  ```bash
  #!/bin/sh
  # Vérifier le format du message de commit
  if ! grep -qE '^(feat|fix|docs|style|refactor|test|chore): ' "$1"; then
      echo "Le message de commit doit commencer par feat|fix|docs|style|refactor|test|chore: "
      exit 1
  fi
  ```

#### d. **`pre-push`**
- **Déclenchement :** Avant un push vers un dépôt distant.
- **Utilisation :** Effectue des vérifications avant de pousser.

  ```bash
  #!/bin/sh
  # Exécuter des tests avant le push
  npm test
  ```

#### e. **`post-commit`**
- **Déclenchement :** Après la création d'un commit.
- **Utilisation :** Exécute des actions après un commit.

### 4. **Configurer les Hooks**
1. **Activer un hook :** Renommez un fichier dans `.git/hooks` en supprimant `.sample`.
2. **Rendre le script exécutable :**
   ```bash
   chmod +x .git/hooks/<hook-name>
   ```

### 5. **Bonnes Pratiques**
- **Performance :** Évitez les opérations longues.
- **Consistance :** Utilisez des outils de formatage et de linting.
- **Documentation :** Documentez le fonctionnement des hooks.

### 6. **Limitations**
- **Hooks locaux :** Spécifiques à chaque dépôt local, non transférés avec le dépôt distant. Utilisez des outils comme Husky pour gérer les hooks dans un projet partagé.

