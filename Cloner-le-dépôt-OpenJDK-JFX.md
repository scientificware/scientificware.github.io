- Open Terminal.
- Cloner le dépôt maître.
List the current configured remote repository for your fork.
````
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
````

Specify a new remote upstream repository that will be synced with the fork.
```
git remote add upstream https://github.com/javafxports/openjdk-jfx.git
```
Verify the new upstream repository you've specified for your fork.
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
`git checkout --track origin/develop`