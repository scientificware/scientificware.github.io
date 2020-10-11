![logo_apache-maven-project](https://user-images.githubusercontent.com/19194678/95678268-d4b79600-0bcb-11eb-9617-6c04bf7d8519.png)

# Windows 10 :
- Télécharger Maven sur le site d'[Apache Maven](https://maven.apache.org/)
- Vérifier l'intégrité du fichier : `certutil -hashfile apache-maven-3.6.3-bin.zip SHA512`
- Extraire l'archive dans un dossier, par exemple : `C:\Users\ScientificWare\maven`
- Mettre à jour la variable PATH : `setx /M PATH "C:\Users\ScientificWare\maven\apache-maven-3.6.3\bin;%PATH%"`