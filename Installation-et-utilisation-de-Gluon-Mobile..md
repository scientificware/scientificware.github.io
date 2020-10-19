Ce qui suit est un tutoriel personnel pour la configuration de mon environnement de développement utilisant Gluon Mobile.
----
Documentation [Gluon Mobile Documentation](http://docs.gluonhq.com/charm)
![](http://docs.gluonhq.com/charm/5.0.1/images/arch/gluon-arch1.png)

![](http://docs.gluonhq.com/charm/5.0.1/images/arch/gluonmobile-docs-tooling.png)

# Windows 10 :

Installer le SDK d'Android : [Documentation officielle](https://developer.android.com/studio/projects/install-ndk).
![android_sdk_tools](https://user-images.githubusercontent.com/19194678/96441960-a6106f80-120a-11eb-9595-f7e7bd59b3af.png)

# Linux Mageia :
Utiliser de préférence la version installée avec l'installateur intégré d'Android Studio.
- Cocher la case **Android SDK Command-line Tools (latest)**
![android_sdk_tools](https://user-images.githubusercontent.com/19194678/96441960-a6106f80-120a-11eb-9595-f7e7bd59b3af.png)
- La commande `sdkmanager` à utiliser est alors dans le dossier `/home/scientificware2016/android_sdk/cmdline-tools/latest/bin`
- Les commandes décrites après le paragraphe suivant (mis en commentaire) fonctionnent.

> Ce qui suit, ne fonctionne pas ! Le lancement de `sdkmanager` conduit à une erreur. Préférer la méthode précédente.
> À propos de  `sdkmanager` :
> Il est possible d'utiliser le `sdk` avec une autre version que celle de la série Java 8. En cas d'utilisation du `sdk` avec une version de Java supérieure ou égale à 9, il faut modifier le fichier `sdkmanager` de la manière suivante :
> - Documentation : [`sdkmanger`](https://developer.android.com/studio/command-line/sdkmanager)
> - Non documenté : `sdkmanager` a besoin du module `java.xml.bind`
> - Télécharger `sdk-tools-linux.zip`
> - Créer un dossier `android_sdk` (en cas d'autre choix remplacer `android_sdk` par le nouveau nom dans la suite)
> - Editer `sdkmanager` et remplacer :
>   -`DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME"'`
>   - Par `DEFAULT_JVM_OPTS='"-Dcom.android.sdklib.toolsdir=$APP_HOME" --add-modules java.xml.bind'`
>   - Éclaircir ce point, voir [Five Command Line Options To Hack The Java 9 Module System](https://blog.codefx.org/java/five-command-line-options-to-hack-the-java-9-module-system/)

Installer la dernière version de build-tools :
- `./sdkmanager --list`
- Dans la liste, choisir le `build-tools` ayant le numéro le plus élevé (28.0.2 le jour de la rédaction de ce tutoriel)
- `./sdkmanager "build-tools;28.0.2"`

Installer Android Support Repository
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
export ANDROID_HOME=$HOME/android_sdk
PATH=$PATH:$ANDROID_HOME/tools
```

Tester l'application dans le simulateur de mobile :
- Documentation : [Run apps on the Android Emulator](https://developer.android.com/studio/run/emulator).

Configurer la signature d'un paquet apk :+1: 
- Documentation Gluon [signingconfig](http://docs.gluonhq.com/charm/5.0.1/#_signingconfig)
- Documentation Android [signature avec Gradle](https://developer.android.com/studio/publish/app-signing#gradle-sign)
- Documentation Oracle [`keytool`](https://docs.oracle.com/javase/10/tools/keytool.htm#JSWOR-GUID-5990A2E4-78E3-47B7-AE75-6D1826259549)
- Étapes :
  - Créer une clé privée avec `keytool` :
    - Ouvrir un terminal dans le dossier où doit être créé la clé privée.
    - Saisir `keytool -genkey -v -keystore my-release-key.pkcs12 -alias scientificware -keyalg RSA -keysize 2048 -validity 10000`
    - Saisir les mots de passe.
- Cette clé privée sera placée dans le répertoire `certificats` ou `certificat` (à uniformiser) créé dans le répertoire NetBeansProjects.
- Dans le fenêtre du projet, ouvrir le fichier `build.gradle` ![screenshot_20180922_115323](https://user-images.githubusercontent.com/19194678/45916110-dd13cb80-be60-11e8-9835-7eb8a7da5d59.png)
- Inserer les lignes suivantes : `my.keystore` est à remplacer par :
  - Gluon version 1 (sans VM) : `../../certificats/[nom du certificat]` si le certificat est dans le dossier `NetBeansProjects/certificats/`.
  - Gluon version 2 (avec VM) : `../../../../certificats/[nom du certificat]` si le certificat est dans le dossier `NetBeansProjects/certificats/`.
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

Modifinier à nouveau le fichier `build.gradle` pour sélectionner les version du SDK mais aussi les SDK ciblés :

```
jfxmobile {
    android {
        manifest = 'src/android/AndroidManifest.xml'
        compileSdkVersion = 26
        minSdkVersion = 26
        signingConfig {
            storeFile file('my.keystore')
            storePassword 'storePass'
            keyAlias 'alias'
            keyPassword 'keyPass'
        }
    }
}
```

Mais apparemment cela ne suffit pas, il faut aussi modifier le fichier `/src/android/AndroidManifest.xml`
`<uses-sdk android:minSdkVersion="9" android:targetSdkVersion="26"/>`

Ne pas oublier de modifier également dans `/src/android/AndroidManifest.xml` les numéro de version de l'application :
- `versionCode` s'incrémente de 1.
- `versionName` est laissé au libre choix du développeur
- Ces variable ce trouve dans la ligne suivante :
`<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.fxmessages" android:versionCode="3" android:versionName="0.00003">`

### Sous Android : Le chargement d'un fichier à partir d'un paquet `.jar` n'est pas pris en charge par le SDK Android.
Il faut dans ce cas le charger dans une chaine de caractère puis charger cette chaine.
Cette procédure étant souvent employée sous FXMessages, impose la création d'une méthode s'occupant de cette tâche.
Dans cette méthode, il est important de définir le codage utilisé pour éviter tout aléa dû à la plateforme d'exécution. C'est ce qui explique la présence de `StandardCharsets.UTF_8`.
```
    private void webViewLoad(WebView webView, String resource) {
        StringWriter writer = new StringWriter();

        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new InputStreamReader(getClass().getResourceAsStream(resource), StandardCharsets.UTF_8));
            String line = null;
            while ((line = reader.readLine()) != null) {
                writer.append(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (reader != null) {
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        webView.getEngine().loadContent(writer.toString());
    }
```
