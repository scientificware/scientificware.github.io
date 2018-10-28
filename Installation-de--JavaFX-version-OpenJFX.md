![logo_jdk_java_net](https://user-images.githubusercontent.com/19194678/47614657-89218580-daa3-11e8-9d37-b4764e9d80db.png)

# Windows 10 :
- Télécharger la dernière version : à partir du site [jdk.java.net/](http://jdk.java.net/)
- Extraire l'archive dans `C:\Program Files\Java`
- Vérifier la signature avec : `certutil`.
  > Exemple : `certutil -hashfile openjdk-11.0.1_windows-x64_bin.zip SHA256`
- Mettre à jour les chemins :
  - ![scsh_paremetres](https://user-images.githubusercontent.com/19194678/47615031-a8231600-daa9-11e8-845a-22185dd5dcef.png)
  - A partir de l'écran d'accueil (figure ci-dessus) sélectionner successivement :  
    > Paramètres
    > Sytème
    > Informations système
    > Infomation système
    > Paramètres système avancés
    > Variables d'environnement
  - Dans la partie Variables système ou utilisateur cliquer
    > Nouvelle
  - Puis la nommer `JAVA_HOME`
  - Renseigner le chemin vers le répertoire d'installation de la valeur d'OpenJDK par défaut.
![scsh_nouvelle_variable_systeme](https://user-images.githubusercontent.com/19194678/47615269-246b2880-daad-11e8-9997-f1b445dfe676.png)


# Linux :