![logo_android_studio](https://user-images.githubusercontent.com/19194678/95673693-577c2900-0bab-11eb-942c-f52112aa6b56.png)

# Windows 10 :
Télécharger Android Studio à partir du site [android.com](https://developer.android.com/studio/)

# Linux :
Télécharger Android Studio à partir du site [android.com](https://developer.android.com/studio/)

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