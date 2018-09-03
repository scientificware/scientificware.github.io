Documentation [Gluon Mobile Documentation](http://docs.gluonhq.com/charm)

![](http://docs.gluonhq.com/charm/5.0.1/images/arch/gluon-arch1.png)

![](http://docs.gluonhq.com/charm/5.0.1/images/arch/gluonmobile-docs-tooling.png)

Installation avec Mageia.

À propos de  `sdkmanager` :

- Documentation : [`sdkmanger`](https://developer.android.com/studio/command-line/sdkmanager)
- Non documenté : `sdkmanager` a besoin du module `java.xml.bind`
- Télécharger `sdk-tools-linux.zip`
- Editer `sdkmanager` et remplacer :
  -`DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME"'`
  - Par `DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME" --add-modules java.xml.bind'`
  - Éclaircir ce point, voir [Five Command Line Options To Hack The Java 9 Module System](https://blog.codefx.org/java/five-command-line-options-to-hack-the-java-9-module-system/)

Définir la variable ANDROID_HOME :
- Activer l'affichage des fichiers cachés.
- Ouvrir avec write le fichier `.bash_profile`.
- Ajouter les lignes suivantes :
```
export ANDROID_HOME=$HOME/Téléchargements
PATH=$PATH:$ANDROID_HOME/tools
```