- Open a terminal in the folder where you want to clone the repository.
![screenshot_20180705_071147](https://user-images.githubusercontent.com/19194678/42303924-dfda33a2-8023-11e8-8ae3-af811c438225.png)
- Cloner le dépôt maître.
  - Clone the official repository
```git clone https://github.com/javafxports/openjdk-jfx.git```
 
  - Or clone your repository with your adress
```git clone https://github.com/scientificware/openjdk-jfx.git```
- List the current configured remote repository for your fork.
````
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
````
- Modifier le lien vers le dépôt dérivé.
```
git remote set-url origin https://github.com/scientificware/openjdk-jfx.git
```

- Specify a new remote upstream repository that will be synced with the fork.
```
git remote add upstream https://github.com/javafxports/openjdk-jfx.git
```

- Verify the new upstream repository you've specified for your fork.
```
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
```

- Clone a branche from the origin
```
git checkout --track origin/develop
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

- Cloner tous les dépôts du projet.

- Cloner les dépôts au cas par cas.
```git checkout --track origin/develop```

- Delete the branch named : `WebCorePatch2018062102_work`
```
git branch -D WebCorePatch2018062102_work
```