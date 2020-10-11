![logo_android_studio](https://user-images.githubusercontent.com/19194678/95673693-577c2900-0bab-11eb-942c-f52112aa6b56.png)

# Windows 10 :
Télécharger Android Studio à partir du site [android.com](https://developer.android.com/studio/)

# Linux :

- Vérifier si GNU C Library (glibc) 2.19 ou une version supérieure est installée (Dernière version en date sur Mageia 2.29).
- Télécharger Android Studio à partir du site [android.com](https://developer.android.com/studio/)
Android Studio

- Instructions d'installation
  ------------------------------------------------------------------------------
  1. Unpack the Android Studio distribution archive that you downloaded to
     where you wish to install the program. We will refer to this destination
     location as your {installation home} below.
  2. Open a console and cd into "{installation home}/bin" and type:
       `./studio.sh`   to start the application.
     As a side effect, this will initialize various
     configuration files in the `~/.AndroidStudio4.0` directory.
  3. [OPTIONAL] Add "`{installation home}/bin`" to your PATH environment variable so that you may start Android Studio from any directory.
  4. [OPTIONAL] To adjust the value of the JVM heap size, create ~/.AndroidStudio4.0/config/studio.vmoptions (or studio64.vmoptions if using a 64-bit JDK), and set the -Xms and -Xmx parameters. To see how to do this, you can reference the vmoptions file under "{installation home}/bin" as a model.

- [OPTIONAL] Changing the location of "config" and "system" directories
  ------------------------------------------------------------------------------
  By default, Android Studio stores all your settings under the ~/.AndroidStudio4.0/config directory and uses ~/.AndroidStudio4.0/system as a data cache.
  If you want to change these settings,

    1. Open a console and cd into ~/.AndroidStudio4.0/config
    2. Create the file "idea.properties" and open it in an editor. Set the idea.system.path and/or idea.config.path variables as desired, for
     example:
     idea.system.path=~/custom/system
     idea.config.path=~/custom/config
    3. Note that we recommend to store data cache ("system" directory) on a disk
     with at least 1GB of free space.

### Mise à jour d'`adb` (Android Debug Bridge).
- Il est arrivé qu'un message indiquant que `adb` était obsolète s'affiche.
- Apparemment, la mise à jour d'`adb` n'est pas automatique lorsqu'on télécharge un nouveau SDK.
- File > Settings > Appearance & Behavior > Androit SDK > (onglet) SDK Tools > (coher) Show Package Details.
- Dans Android SDK Build-Tools cocher le deux dernières versions stables pour le SDK qui a généré l'avertissement d'obsolescence.

### Adapter la barre d'outils.
- File > Settings > Appearance & Behavior > Menus and Toolbars > Main Tool Bar
- Ajouter à partir du Menu Principal (Main Menu), les actions Undo, Redo, Find et Replace.

### Emulateur, problème d'accès à internet.
Après l'installation du paquet `SDK platform Android 10.0(Q) API Level 29 revision 4` l'émulateur n'avait plus d'accès à internet.
Le constat a été fait en testant l'application Games.
- L'application n'accédait pas a internet et renvoyait l'erreur : `net::ERR_INTERNET_DISCONNECTED`.
- Les permissions d'accès internet dans l'`AndroidManifest.xml` étaient bien faites.

Comment le problème a-t-il été résolu ? 
- L'application Games arrive à accéder à internet maintenant avec la version 29 du SDK ! 😊 J'ai fait quelques manip juste pour vérifier certains paramètres mais sans vraiment rien changer :
  - Android Studio > File > Settings > System Settings > HTTP Proxy 
  - Cocher "Auto-detect proxy settings" > Apply > Check connection avec `http://google.fr`.
  - Relancer l'application Games. C'est là ce moment que j'ai vu que cela fonctionnait.
 - Re-cocher "No Proxy"

- Je n'ai pas essayé mais on peut aussi déclarer "No proxy dans l'émulateur" : les point de suspension en bas dans la barre verticale à droite de l'émulateur (more) > Settings > Onglet Proxy :
  - Décocher "Use Android Studio HTTP proxy settings"
  - Cliquer sur Apply.
  - Si cela fonctionne, essayer de remettre les paramètres initiaux.