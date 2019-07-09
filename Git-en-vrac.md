Affichage des différentes branches du projet : `git branch`
```
[scientificware2016@localhost openjdk-jfx-compile (develop)]$ git branch
  WebCorePatch2018062102_work
  WebCorePatch2018062102_work_sauvegarde
  WebCorePatch2018063010_work
  WebCorePatch2018063010_work_sauvegarde
* develop
  develop_sauvegarde
  issue#118_HTMLEditor_and_MathML
  master
  master_test
[scientificware2016@localhost openjdk-jfx-compile (develop)]$
```
`git checkout master`

`git config -l` ou `git config --list`

Exemple :

```
[... openjdk-jfx]$ git config --list
user.name=Guy Abossolo Foh
user.email=info@scientificware.com
merge.tool=vimdiff
core.editor=kwrite
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/javafxports/openjdk-jfx.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.develop.remote=origin
branch.develop.merge=refs/heads/develop
branch.master.remote=origin
branch.master.merge=refs/heads/master
gui.wmstate=normal
gui.geometry=1045x475+417+99 221 219

```

`git pull upstream master`

`git push origin master -f`

`git pull upstream develop`

`git push origin develop -f`





