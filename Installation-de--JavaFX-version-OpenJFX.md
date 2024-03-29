![logo_javafx](https://user-images.githubusercontent.com/19194678/47615543-82e5d600-dab0-11e8-8fdc-debe74393928.png)
 
# Alternatives :
Bien que OpenJFX ait été dissocié de OpenJDK, deux fournisseurs continuent à distribuer des paquets contenant les deux SDK.

![azul](https://user-images.githubusercontent.com/19194678/90957767-4d358c80-e490-11ea-97ee-55c862422431.png)
- [Azul](https://www.azul.com/)

![bellsoft](https://user-images.githubusercontent.com/19194678/90957883-c6cd7a80-e490-11ea-9ac2-2fc66edd2c22.png)
- [BellSoft](https://bell-sw.com)

# Windows 10 :
- Télécharger la dernière version : à partir du site [openjfx.io](https://openjfx.io/)
- Extraire l'archive dans `C:\Program Files\Java`
- Vérifier la signature avec : `certutil`.
  > Exemple : `certutil -hashfile openjdk-11.0.1_windows-x64_bin.zip SHA256`
- Utilisation de certutil :
  ```
  Utilisation :
  CertUtil [Options] -hashfile FichierEntrée [AlgorithmeHachage]
  Générer et afficher le hachage de chiffrement sur un fichier
  
  Options :
    -Unicode          -- Écrire la sortie redirigée au format Unicode
    -gmt              -- Afficher les heures GMT
    -seconds          -- Afficher le temps en secondes et millisecondes
    -v                -- Opération en mode détaillé
    -privatekey       -- Afficher les données de mot de passe et de clé privée
    -pin Code PIN             -- Code PIN de la carte à puce
    -sid WELL_KNOWN_SID_TYPE  -- SID numérique
              22 -- Système local
              23 -- Service local
              24 -- Service réseau
  
  Algorithmes de hachage : MD2 MD4 MD5 SHA1 SHA256 SHA384 SHA512
  ```
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
![scsh_nouvelle_variable_systeme_jfx](https://user-images.githubusercontent.com/19194678/47615635-79a93900-dab1-11e8-9847-a5d35420e001.png)

Lancer un programme utilisant JavaFX 15 sous Windows 10

```
set PATH_TO_FX=C:\Users\ScientificWare\javafx\javafx-sdk-15\lib
javac --module-path %PATH_TO_FX% --add-modules=javafx.base,javafx.graphics,javafx.web FXStatique.java
java --module-path %PATH_TO_FX% --add-modules=javafx.base,javafx.graphics,javafx.web fxstatique.FXStatique
```

# Linux :

Lancer un programme utilisant JavaFX 11 sous Linux, créer un fichier de commandes :
```
#!/bin/bash
rm *.class
export PATH_TO_FX=/home/scientificware2016/Documents/open_jdk_jfx/javafx-sdk-11.0.2/lib
javac --module-path $PATH_TO_FX --add-modules=javafx.base,javafx.graphics,javafx.web FXMath.java
java --module-path $PATH_TO_FX --add-modules=javafx.base,javafx.graphics,javafx.web FXMath
```

puis lancer la commande `.\ce` à l'invite.