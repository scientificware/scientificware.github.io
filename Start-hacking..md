Assume that we posted an issue, for example : > HTMLEditor and MathML. #118 

![screenshot_20180806_163713](https://user-images.githubusercontent.com/19194678/43723288-9898168e-9997-11e8-8d9a-edf4cfb9ec3d.png)

Update your master reprository :
- go to your master reprository `git checkout master`.
- Update master `git pull upstream master`.

Update your develop reprository :
- go to your develop reprository `git checkout develop`.
- Update develop `git pull upstream develop.

Create a branch base on the develop branch with a name relative to the issue (issue#118_HTMLEditor_and_MathML for example) `git branch issue#118_HTMLEditor_and_MathML`.

Go to your new working branch `git checkout issue#118_HTMLEditor_and_MathML`

Start hacking.