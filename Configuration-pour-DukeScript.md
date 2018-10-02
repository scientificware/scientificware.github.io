Ce qui suit est un tutoriel personnel pour la configuration de mon environnement de développement utilisant DukeScript.
----
![duke_script](https://user-images.githubusercontent.com/19194678/46353509-a7d75c80-c65c-11e8-9eb0-3f989948e850.png)

Documentation DukeScript [Getting Started with DukeScript](https://dukescript.com/getting_started.html)Getting Started with DukeScript

# Installation avec Windows.
  - Installer sdkmanager :
À propos de  `sdkmanager` :
Il est possible d'utiliser le `sdk` avec une autre version que celle de la série Java 8. En cas d'utilisation du `sdk` avec une version de Java supérieure ou égale à 9, il faut modifier le fichier `sdkmanager` de la manière suivante :
   - Documentation : [`sdkmanger`](https://developer.android.com/studio/command-line/sdkmanager)
   - Non documenté : `sdkmanager` a besoin du module `java.xml.bind`
   - Télécharger `sdk-tools-windows.zip`
   - Créer un dossier `android_sdk` (en cas d'autre choix remplacer `android_sdk` par le nouveau nom dans la suite)
   - Editer `sdkmanager` et remplacer :
     -`set DEFAULT_JVM_OPTS="-Dcom.android.sdklib.toolsdir=%~dp0\.."`
     - Par `set DEFAULT_JVM_OPTS="-Dcom.android.sdklib.toolsdir=%~dp0\.." --add-modules java.xml.bind`
     - Éclaircir ce point, voir [Five Command Line Options To Hack The Java 9 Module System](https://blog.codefx.org/java/five-command-line-options-to-hack-the-java-9-module-system/)

  - Installer la dernière version de build-tools :
    - `sdkmanager --list`
    - Dans la liste, choisir le `build-tools` ayant le numéro le plus élevé (28.0.3 le jour de la rédaction de ce tutoriel)
    - `sdkmanager "build-tools;28.0.3"`

  - Installer la bonne plateforme ?
    - `sdkmanager "platforms;android-28"`
    - `sdkmanager "platforms;android-27"`
    - ...

# Installation avec Mageia.
  - Installer sdkmanager :

À propos de  `sdkmanager` :
Il est possible d'utiliser le `sdk` avec une autre version que celle de la série Java 8. En cas d'utilisation du `sdk` avec une version de Java supérieure ou égale à 9, il faut modifier le fichier `sdkmanager` de la manière suivante :
    - Documentation : [`sdkmanger`](https://developer.android.com/studio/command-line/sdkmanager)
    - Non documenté : `sdkmanager` a besoin du module `java.xml.bind`
    - Télécharger `sdk-tools-linux.zip`
    - Créer un dossier `android_sdk` (en cas d'autre choix remplacer `android_sdk` par le nouveau nom dans la suite)
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