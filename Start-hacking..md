file:///home/scientificware2016/Téléchargements/Screenshot_20180806_163713.png

Suppose we posted an issue for example : > HTMLEditor and MathML. #118 

Update your master reprository :
- go to your master reprository `git checkout master`.
- Update master `git pull upstream master`.

Update your develop reprository :
- go to your develop reprository `git checkout develop`.
- Update develop `git pull upstream develop.

Create a branch base on the develop branch with a name relative to the issue (issue#118_HTMLEditor_and_MathML for example) `git branch issue#118_HTMLEditor_and_MathML`.

Go your new working branch `git checkout issue#118_HTMLEditor_and_MathML`

Start hacking.