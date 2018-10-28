![logo_jdk_java_net](https://user-images.githubusercontent.com/19194678/47614657-89218580-daa3-11e8-9d37-b4764e9d80db.png)

# Windows 10 :
- Télécharger la dernière version : à partir du site [jdk.java.net/](http://jdk.java.net/)
- Extraire l'archive dans `C:\Program Files\Java`
- Vérifier la signature avec : `certutil`.
  > Exemple : `certutil -hashfile openjdk-11.0.1_windows-x64_bin.zip SHA256`
- Mettre à jour les chemins :
  - Open Search and type advanced system settings
  - In the shown options, select the View advanced system settings link
  - Under the Advanced tab, click Environment Variables
  - In the System variables section, click New (or User variables for single user setting)
  - Set JAVA_HOME as the Variable name and the path to the JDK installation as the Variable value and click OK
    Click OK and click Apply to apply the changes


# Linux :