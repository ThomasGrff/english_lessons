# HELP – Workflow Git pour le projet `anglais-site`

Ce fichier résume **quoi faire** et **quelles commandes utiliser** quand tu modifies ton site :

- sur **ton ordinateur** (local)
- sur **GitHub** (éditeur en ligne)
- avec ou sans **branches**

---

## 1. Vocabulaire rapide

- **repo** : ton projet Git (dossier `anglais-site`)
- **commit** : une “photo” de ton projet à un instant T avec un message
- **branch** (`main`, etc.) : une ligne de développement
- **remote** : la version sur GitHub (`origin`)
- **pull** : récupérer les changements de GitHub vers ton ordinateur
- **push** : envoyer tes changements de ton ordinateur vers GitHub
- **merge** : fusionner des changements de différentes branches/versions

---

## 2. Cas le plus courant : je modifie les fichiers sur MON ORDINATEUR

### 2.1. Étapes à suivre à chaque fois

1. Aller dans le dossier du projet :

   ```bash
   cd /chemin/vers/anglais-site
   ```

2. Voir ce qui a changé :

   ```bash
   git status
   ```

3. Ajouter les fichiers modifiés au commit :

   - Pour tout ajouter :

     ```bash
     git add .
     ```

   - ou fichier par fichier :

     ```bash
     git add index.html
     git add grammar/2025-11-20-present-perfect-continuous.html
     git add css/style.css
     ```

4. Créer un commit avec un message clair :

   ```bash
   git commit -m "Add new grammar sheet for Present Perfect Continuous"
   ```

5. Envoyer sur GitHub :

   ```bash
   git push
   ```

> ⚠️ Astuce : fais un petit commit assez souvent plutôt qu’un énorme commit très rarement.

---

## 3. Quand j’ai MODIFIÉ DES FICHIERS sur GITHUB (en ligne) **ET** sur mon ordinateur

Dans ce cas, il faut **toujours récupérer les changements de GitHub avant de push**.

1. Aller dans le dossier :

   ```bash
   cd /chemin/vers/anglais-site
   ```

2. Récupérer les changements de GitHub :

   ```bash
   git pull
   ```

   - Si tout se passe bien → Git merge automatiquement ✅  
   - S’il y a des conflits → voir section **5. Gérer un conflit de merge**

3. Ensuite, workflow normal :

   ```bash
   git add .
   git commit -m "Update files after merging with GitHub"
   git push
   ```

---

## 4. Utiliser des BRANCHES (optionnel mais propre)

Si tu veux tester une nouvelle fonctionnalité (nouveau design, refonte CSS, etc.) sans casser `main`, tu peux utiliser des branches.

### 4.1. Créer une branche

Depuis `main` :

```bash
cd /chemin/vers/anglais-site
git checkout main
git pull                          # mettre main à jour
git checkout -b nouvelle-fonction
```

Tu es maintenant sur la branche `nouvelle-fonction`.

### 4.2. Travailler sur la branche

Tu modifies tes fichiers normalement, puis :

```bash
git add .
git commit -m "Describe the changes on this branch"
git push -u origin nouvelle-fonction
```

Tu peux maintenant, si tu veux, créer une **pull request** sur GitHub, ou juste merger localement.

### 4.3. Merger la branche dans `main`

Quand tu es content du résultat :

```bash
cd /chemin/vers/anglais-site

# 1. Revenir sur main
git checkout main

# 2. Mettre main à jour avec GitHub (important)
git pull

# 3. Merger ta branche dans main
git merge nouvelle-fonction
```

- Si pas de conflit → merge OK ✅  
- S’il y a conflit → voir section 5.

Puis envoyer sur GitHub :

```bash
git push
```

Optionnel : supprimer la branche locale une fois qu’elle est mergée :

```bash
git branch -d nouvelle-fonction
```

---

## 5. Gérer un CONFLIT de merge (cas spécial)

Un conflit arrive quand Git ne sait pas quelle version garder (par exemple, même ligne modifiée sur GitHub et en local).

### 5.1. Ce que tu verras dans le fichier

Exemple dans `index.html` :

```text
<<<<<<< HEAD
  <li>Total grammar sheets: <strong>3</strong></li>
=======
  <li>Total grammar sheets: <strong>4</strong></li>
>>>>>>> nouvelle-fonction
```

- `<<<<<<< HEAD` : ta version actuelle (branche sur laquelle tu es)
- `=======` : séparation
- `>>>>>>> nouvelle-fonction` : l’autre version

### 5.2. Ce que tu dois faire

1. Ouvrir le fichier dans ton éditeur.
2. Choisir le contenu correct (par exemple, `4` si c’est le bon nombre).
3. **Supprimer tous** les marqueurs :

```html
<li>Total grammar sheets: <strong>4</strong></li>
```

4. Enregistrer le fichier.

5. Dire à Git que le conflit est résolu :

```bash
git add index.html
git commit
```

(s’il te demande un message de merge, tu peux laisser celui par défaut)

6. Envoyer sur GitHub :

```bash
git push
```

---

## 6. Résumé des commandes les plus utiles (avec commentaires)

```bash
# Aller dans le projet
cd /chemin/vers/anglais-site

# Voir l’état des fichiers (modifiés, nouveaux, etc.)
git status

# Ajouter tous les fichiers modifiés au prochain commit
git add .

# Ajouter un seul fichier
git add chemin/vers/fichier.html

# Créer un commit avec un message
git commit -m "Message clair décrivant les modifications"

# Envoyer la branche courante sur GitHub
git push

# Récupérer les dernières modifications de GitHub et les merger dans ta branche locale
git pull

# Créer une nouvelle branche et l’activer
git checkout -b nom-de-branche

# Changer de branche
git checkout main

# Merger une branche dans la branche courante
git merge nom-de-branche

# Supprimer une branche locale après merge
git branch -d nom-de-branche
```

---

## 7. Routines à retenir

### Routine 1 – Je travaille SEULEMENT en local

```bash
cd /chemin/vers/anglais-site
git status
git add .
git commit -m "Describe changes"
git push
```

### Routine 2 – J’ai aussi modifié sur GitHub

```bash
cd /chemin/vers/anglais-site
git pull              # merge d’abord les changements GitHub
git status
# (résoudre les conflits si besoin)
git add .
git commit -m "Describe changes after merge"
git push
```

### Routine 3 – Je teste quelque chose sur une branche

```bash
cd /chemin/vers/anglais-site
git checkout main
git pull
git checkout -b test-nouvelle-fonction

# ... modifications ...
git add .
git commit -m "Test new function"
git checkout main
git pull
git merge test-nouvelle-fonction
git push
git branch -d test-nouvelle-fonction
```

---

Fin du fichier 😊  
Tu peux maintenant l’ouvrir quand tu bloques et copier-coller les commandes au besoin.
