L'application Games arrive à accéder à internet maintenant avec la version 29 du SDK ! 😊
J'ai fait quelques manip juste pour vérifier certains paramètres mais sans vraiment rien changer :
- Android Studio > File > Settings > System Settings > HTTP Proxy 
- Cocher "Auto-detect proxy settings" > Apply > Check connection avec "http://google.fr"
- Relancer l'application Games. C'est là ce moment que j'ai vu que cela fonctionnait.
- Re-cocher "No Proxy"

Je n'ai pas essayé mais on peut aussi déclarer "No proxy dans l'émulateur" : les point de suspension en bas dans la barre verticale à droite de l'émulateur (more) > Settings > Onglet Proxy :
- Décocher "Use Android Studio HTTP proxy settings"
- Cliquer sur Apply.
- Si cela fonctionne, essayer de remettre les paramètres initiaux.

