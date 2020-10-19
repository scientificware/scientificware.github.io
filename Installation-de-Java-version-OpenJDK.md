![logo_jdk_java_net](https://user-images.githubusercontent.com/19194678/47614657-89218580-daa3-11e8-9d37-b4764e9d80db.png)

# Windows 10 :
- Télécharger la dernière version : à partir du site [jdk.java.net/](http://jdk.java.net/)
- Vérifier la signature avec : `certutil`.
  > Exemple : `certutil -hashfile openjdk-15_windows-x64_bin.zip SHA256`
- Utilisation de certutil :
  ```
  Utilisation :
  CertUtil [Options] -hashfile FichierEntrée [AlgorithmeHachage]
  Générer et afficher le hachage de chiffrement sur un fichier
  
  Options :
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
  
  Algorithmes de hachage : MD2 MD4 MD5 SHA1 SHA256 SHA384 SHA512
  ```
- Extraire l'archive dans `C:\Program Files\Java`
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


# Linux : (Amélioration possible : ajouter un script pour automatiser tous le nouveaux liens)
- Télécharger la dernière version : à partir du site [jdk.java.net/](http://jdk.java.net/)
- Vérifier la signature avec : `sha256sum`.
  utilisation de sha256sum :
  ```
  Usage: sha1sum [OPTION]... [FILE]...
  Print or check SHA1 (160-bit) checksums.

  Sans FICHIER ou quand FICHIER est -, lire l'entrée standard.
  
    -b, --binary         read in binary mode
    -c, --check          lire les sommes SHA1 à partir des FICHIERs et les vérifier
        --tag            créer une somme de contrôle de type BSD
        --color          print with colors
    -t, --text           lire en mode texte (par défaut)
    Note: There is no difference between binary and text mode option on GNU system.
  
  The following five options are useful only when verifying checksums:
        --ignore-missing  don't fail or report status for missing files
        --quiet          don't print OK for each successfully verified file
        --status         don't output anything, status code shows success
        --strict         exit non-zero for improperly formatted checksum lines
    -w, --warn           warn about improperly formatted checksum lines

        --help     afficher l'aide et quitter
        --version  afficher des informations de version et quitter

  The sums are computed as described in FIPS-180-1.  When checking, the input
  should be a former output of this program.  The default mode is to print a
  line with checksum, a space, a character indicating input mode ('*' for binary,
  ' ' for text or where binary is insignificant), and name for each FILE.
  
  Aide en ligne de GNU coreutils : <http://www.gnu.org/software/coreutils/>
  Signalez les problèmes de traduction de « sha1sum » à : <traduc@traduc.org>
  Full documentation at: <http://www.gnu.org/software/coreutils/sha1sum>
  or available locally via: info '(coreutils) sha1sum invocation'
  ```
  > Exemple : `sha256sum openjfx-12.0.1_linux-x64_bin-sdk.zip`
- Extraire l'archive dans `\usr\java`
  ```
  Info-ZIP UnZip 6.1c25-BETA (2018-12-20)
   Copyright (c) 1990-2018 Info-ZIP.  License: unzip --license
          THIS IS A BETA VERSION OF UNZIP -- NOT FOR GENERAL DISTRIBUTION.
  Usage: unzip [-Z] [-opts[modifiers]] file[.zip] [list] [-x xlist] [-d exdir]
   Default action: Extract files in list, except those in xlist, to exdir;
   file[.zip] may be a wildcard.  -Z => ZipInfo mode ("unzip -Z" for usage).
  Options (primary mode):
    -p  extract files to pipe, no messages     -l  list files (short format)
    -f  freshen existing files, create none    -t  test compressed archive data
    -u  update files, create if necessary      -z  display archive comment only
    -v  list verbosely/show version info       -T  timestamp archive to latest
  Options (modifiers):
    -n  never overwrite existing files         -q  quiet mode (-qq => quieter)
    -o  overwrite files WITHOUT prompting      -a  auto-convert any text files
    -j[=N] junk paths (strip all/top-N dirs)   -aa treat ALL files as text
    -C  match filenames case-insensitively     -L  make (some) names lowercase
    -X | -k  restore UID/GID | permissions     -V  retain VMS version numbers
    -K  keep setuid/setgid/tacky permissions   -D- restore dir (-D: no) times
    -M  pipe through "more" pager
  More help: unzip -hh   Examples:
    unzip data1 -x joe   # Extract all files except joe from archive data1.zip
    unzip -p foo | more  # Pipe contents of foo.zip into program "more"
    unzip -fo foo ReadMe # Replace quietly existing ReadMe if archive file newer
  ```
  > Exemple : `unzip -d /usr/java openjfx-12.0.1_linux-x64_bin-sdk.zip`
- Mettre à jour les liens avec : `update-alternatives`
  ```
  Alternatives, version 1.10 - Copyright (C) 2001 Red Hat, Inc.
  Ce produit peut être librement distribué selon les termes de la licence publique GNU (GPL).
  
  utilisation : alternatives --install <lien> <nom> <chemin> <priorité>
                    [--initscript <service>]
                    [--family <family>]
                    [--slave <lien> <nom> <chemin>]*
       alternatives --remove <nom> <chemin>
       alternatives --auto <nom>
       alternatives --config <nom>
       alternatives --display <nom>
       alternatives --set <nom> <chemin>
       alternatives --list
       alternatives --remove-all <name>

  common options: --verbose --test --help --usage --version --keep-missing
                --altdir <répertoire> --admindir <répertoire>
  ```
  > Exemples :
  >
  > Voir et modifier la configuration courante pour `java` : `update-alternatives --config java`
  >
  > Ajouter une alternative :
  >  - `update-alternatives --install /usr/bin/java java  /usr/java/jdk-12.0.1/bin/java 3000`
  >  - `update-alternatives --install /usr/bin/javac javac  /usr/java/jdk-12.0.1/bin/javac 3000` 
  > 
  > Dans une console, cela peut être sur redéfini dans les fichiers `.bashrc` puis `.bash_profile` à l'aide des variables `JAVA_HOME` et `JDK_HOME` :
  > - `export JAVA_HOME=$HOME/Documents/jdk-12.0.1`
  > - `export JDK_HOME=$HOME/Documents/jdk-12.0.1`
