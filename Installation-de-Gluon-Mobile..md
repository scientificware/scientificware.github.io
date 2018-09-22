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

Installer la dernière version de build-tools :
- `./sdkmanager --list`
- Dans la liste, choisir le `build-tools` ayant le numéro le plus élevé (28.0.2 le jour de la rédaction de ce tutoriel)
- `./sdkmanager "build-tools;28.0.2"`

Installer ?
- `./sdkmanager "extras;android;m2repository"`

Installer la bonne plateforme ?
- `./sdkmanager "platforms;android-27"`

Installer le simulateur de mobile :
- `./sdkmanager "emulator"`

Définir la variable ANDROID_HOME :
- Activer l'affichage des fichiers cachés.
- Ouvrir avec write le fichier `.bash_profile`.
- Ajouter les lignes suivantes :
```
export ANDROID_HOME=$HOME/Téléchargements
PATH=$PATH:$ANDROID_HOME/tools
```

Tester l'application dans le simulateur de mobile :
- Documentation : [Run apps on the Android Emulator](https://developer.android.com/studio/run/emulator).

Configurer la signature d'un paquet apk :
- Documentation Gluon [signingconfig](http://docs.gluonhq.com/charm/5.0.1/#_signingconfig)
- [Documentation Android signature avec Grale](https://developer.android.com/studio/publish/app-signing#gradle-sign)
- Dans le fenêtre du projet, ouvrir le fichier `build.gradle` ![screenshot_20180922_115323](https://user-images.githubusercontent.com/19194678/45916110-dd13cb80-be60-11e8-9835-7eb8a7da5d59.png)
- Inserer les lignes suivantes :
```
jfxmobile {
    android {
        manifest = 'src/android/AndroidManifest.xml'
        signingConfig {
            storeFile file('my.keystore')
            storePassword 'storePass'
            keyAlias 'alias'
            keyPassword 'keyPass'
        }
    }
}
```