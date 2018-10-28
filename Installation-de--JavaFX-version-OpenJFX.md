![logo_javafx](https://user-images.githubusercontent.com/19194678/47615543-82e5d600-dab0-11e8-8fdc-debe74393928.png)

# Windows 10 :
- Télécharger la dernière version : à partir du site [openjfx.io](https://openjfx.io/)
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
  - Puis la nommer `PATH_TO_FX`
  - Renseigner le chemin vers le répertoire d'installation de la valeur d'OpenJDK par défaut.
![scsh_nouvelle_variable_systeme](https://user-images.githubusercontent.com/19194678/47615269-246b2880-daad-11e8-9997-f1b445dfe676.png)


# Linux :