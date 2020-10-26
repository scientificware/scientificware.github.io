![logo_graalwm](https://user-images.githubusercontent.com/19194678/93709640-28404200-fb40-11ea-8a2d-c1ff028735d5.png)

L'objet de ce document est la mise en place d'une chaîne de production d'applications natives utilisant GraalVM.
----
Dans notre plan de marche, nous nous concentrons uniquement sur :
- Deux plateformes de production :
  - Windows 10,
  - Linux (en particulier la distribution Mageia). 
- Pour trois plateformes d'exécution :
  - Windows 10, 
  - Linux (en particulier la distribution Mageia)
  - et Linux/Android.

Pour macOS et iOS, nous attendons la mise en place de la nouvelle architecture ARM d'Apple pour investir.

[GraalVM Website](https://www.graalvm.org/)

GraalVM is a universal virtual machine for running applications written in JavaScript, Python, Ruby, R, JVM-based languages like Java, Scala, Kotlin, Clojure, and [LLVM](https://llvm.org/)-based languages such as C and C++. 

[SubstrateVM on AArch64](https://github.com/oracle/graal/pull/910)

Prérequis pour suivre le tutoriel : [GraalVM GET STARTED](https://www.graalvm.org/docs/getting-started/).

Compilation dans le cas d'un package : (Complément par rapport au tutoriel)

Par exemple, pour le programme suivant :
```
package hello;

public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello World!!");
        System.exit(0);
    }
}
```
  - Pour compiler : `javac -d . HelloWorld.java`
  - Pour l'exécuter avec la machine virtuelle : `java hello.HelloWorld`
  - Pour construire un executable sur l'hôte : `native-image hello.HelloWorld`

Limitation(s) du moment :   

 Platforme |OpenJFX|API AWT|API Swing|Autres
---|---|---|---|---
Windows 10|:x: | :heavy_check_mark:20.3.0 | :heavy_check_mark:20.3.0|:heavy_check_mark:
Linux pur|Gluon :heavy_check_mark:| :heavy_check_mark:20.3.0| :heavy_check_mark:20.3.0|:heavy_check_mark:
Linux/Android|Gluon :heavy_check_mark:| :x:|:x:|:heavy_check_mark:
macOs iOS|:question: |:question: |:question: | :question:

- Pas de compilation native sous Windows ou Linux avec awt et donc Swing :
  - Pour plus de détail suivre :
    - [[native-image] Windows with a swing application](https://github.com/oracle/graal/issues/1327).
    - Mais aussi [Linker failure: undefined reference to `Java_java_util_prefs_FileSystemPreferences_chmod' #2856](https://github.com/oracle/graal/issues/2856).
  - Pour plus de renseignements sur la réflexion avec GraalVM : [Reflection Use in Native Images](https://github.com/oracle/graal/blob/master/substratevm/Reflection.md)
  - Exemple de procédure pour le `HelloAWT.java` programme utilisant l'API `awt` :
    - AWT ([Abstract Window ToolKit](https://openjdk.java.net/groups/awt/)).

    ```
    package hello;

    import java.awt.Frame;
    import java.awt.Label;
    import java.awt.event.WindowAdapter;
    import java.awt.event.WindowEvent;

    public class HelloAWT {

        public static void main(String[] args) {
            Frame f = new Frame( "Hello world!" );
            Label label = new Label("Hello World", Label.CENTER);
            f.add(label);
            f.addWindowListener(
                new WindowAdapter(){
                    public void windowClosing( WindowEvent e ){
                        System.exit( 0 );
                    }
                });
            f.setSize( 300, 100 );
            f.setVisible(true);
            System.out.println("Hello World!!");
            //System.exit( 0 );
        }
    }
    ```
  - Créer un fichier JSON `rconfig.json` avec le contenu suivant :
    ```
    [
      {
        "name": "sun.awt.windows.WToolkit",
        "methods": [{"name":"<init>","parameterTypes":[] }]
      },
      {
        "name": "sun.awt.Win32GraphicsEnvironment",
        "methods": [{"name":"<init>","parameterTypes":[] }]
      }
    ]
    ```
  - Pour compiler : `javac -d . HelloAWT.java`
  - Pour compiler en détectant les éléments dépréciés : `javac -d . -deprecation  HelloAWT.java`
  - Pour l'exécuter avec la machine virtuelle : `java hello.HelloAWT`
  - Il ne reste plus qu'à compiler nativement le bytecode en passant le paramètre `-H:ReflectionConfigurationFiles` à la commande `native-image` : `native-image --no-fallback -H:ReflectionConfigurationFiles=./rconfig.json hello.HelloAWT`
  - Pour Windows : `native-image --no-fallback -H:NativeLinkerOption=/opt/graalvm-ce-java11-20.2.0/lib/static/windows-amd64/prefs.lib  hello.HelloAWT`

  - Exemple de procédure pour le `HelloSwing.java` programme utilisant l'API `Swing` :
    - Swing ([openjdk.java.net](https://openjdk.java.net)).

    ```
    package hello;

    import java.awt.*;
    import javax.swing.*;

    public class HelloSwing {

        public static void main(String[] args) {
            EventQueue.invokeLater(() -> {
                JPanel panel = new JPanel();
                panel.add(new JLabel("Hello Swing Users !"));
                panel.setPreferredSize(new Dimension(600,200));
                JFrame frame = new JFrame("Hello Swing App");
                frame.setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);
                frame.getContentPane().add(panel);
                frame.pack();
                frame.setVisible(true);
            });
        }

    }
    ```
  - Créer un fichier JSON `rconfig.json` avec le contenu suivant :
    ```
    [
      {
        "name": "sun.awt.windows.WToolkit",
        "methods": [{"name":"<init>","parameterTypes":[] }]
      },
      {
        "name": "sun.awt.Win32GraphicsEnvironment",
        "methods": [{"name":"<init>","parameterTypes":[] }]
      }
    ]
    ```
  - Pour compiler : `javac -d . HelloSwing.java`
  - Pour compiler en détectant les éléments dépréciés : `javac -d . -deprecation  HelloSwing.java`
  - Pour l'exécuter avec la machine virtuelle : `java hello.HelloSwing`
  - Il ne reste plus qu'à compiler nativement le bytecode en passant le paramètre `-H:ReflectionConfigurationFiles` à la commande `native-image` : `native-image --no-fallback -H:ReflectionConfigurationFiles=./rconfig.json hello.HelloSwing`
  - Pour Windows : `native-image --no-fallback -H:NativeLinkerOption=/opt/graalvm-ce-java11-20.2.0/lib/static/windows-amd64/prefs.lib  hello.HelloSwing`

- L'utilisation d'OpenJFX est possible mais au prix d'une forte augmentation de l'exécutable. Cela tient à la nature même d'OpenJFX dont une large partie du code est du C++ enveloppée d'une fine couche de Java.


# Windows 10 :
- Télécharger la dernière version : à partir du site [graalvm.org/](https://www.graalvm.org/downloads/)
- Extraire l'archive dans `C:\Program Files\GraalVM`

- Pour compiler en mode natif :
  - Lire https://github.com/gluonhq/client-maven-plugin/
  - Installer [Visual Studio (Communauté)](https://visualstudio.microsoft.com/fr/downloads/) puis lancer Visual Studio Installer à partir du menu démarrer pour installer les composants nécessaires. Aller dans l'onglet, "Composants individuels". Utiliser la zone de recherche pour trouver plus facilement les composants.
    ![visual_studio_installer_ajouts_composants](https://user-images.githubusercontent.com/19194678/95646212-7d7bcd80-0ac6-11eb-9d1b-35d0fefc1cf8.png)
    - MSVC v142 - VS 2019 C++ x64/x86 Build Tools (v14.27)
    - Prise en charge de C++/CLI pour Build Tools v142 (14.27) 
    - SDK CRT (runtime C) universel pour Windows
    - Kit SDK Windows 10 (10.0.19041.0)

  - Pour compléter la variable PATH avec le chemin vers GraalVM : `setx /M PATH "C:\Progra~1\GrallVM\graalvm-ce-java11-20.2.0\bin;%PATH%"`
  - Pour afficher le contenu de la variable est vérifier l'ajout du chemin : `echo %PATH%`
  - Pour définir la variable d'environnement JAVA_HOME `setx /M JAVA_HOME "C:\Progra~1\GraalVM\graalvm-ce-java11-20.2.0"`
  - Pour vérifier si la variable est bien définie : `echo %JAVA_HOME%`

- lire https://www.graalvm.org/reference-manual/native-image/ pour la compilation en mode natif.

- Exemple d'utilisation : `native-image -jar "Loto_2020.jar"`, `native-image HelloWorld`, ...

# Linux pur :
- Télécharger la dernière version : à partir du site [graalvm.org/](https://www.graalvm.org/downloads/).
- Extraire l'archive tar.gz `tar -xf graalvm-ce-java11-linux-amd64-20.2.0.tar.gz -C /opt`
```
Utilisation : tar [OPTION...] [FICHIER]...
GNU 'tar' saves many files together into a single tape or disk archive, and can
restore individual files from the archive.

Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
  tar -xf archive.tar          # Extract all files from archive.tar.

 Sélection des noms de fichiers locaux :

      --add-file=FICHIER     Ajouter le FICHIER donné à l'archive (utile si
                             son nom commence par un tiret)
  -C, --directory=RÉP       Utiliser RÉP comme répertoire de travail
      --exclude=MOTIF        Exclure les fichiers correspondant au MOTIF
      --exclude-backups      exclure les fichiers de sauvegarde et de verrou
      --exclude-caches       Exclure le contenu des répertoires contenant
                             CACHEDIR.TAG, sauf le fichier de tag lui-même
      --exclude-caches-all   Exclure les répertoires contenant CACHEDIR.TAG
      --exclude-caches-under Tout exclure dans les répertoires contenant
                             CACHEDIR.TAG
      --exclude-ignore=FICHIER   lire le motif d'exclusion de chaque
                             répertoire depuis FICHIER, s'il existe
      --exclude-ignore-recursive=FICHIER
                             lire le motif d'exclusion de chaque répertoire et
                             de ses sous-répertoire depuis FICHIER, s'il
                             existe
      --exclude-tag=FICHIER  Exclure le contenu des répertoires contenant le
                             FICHIER, sauf le FICHIER lui-même
      --exclude-tag-all=FICHIER   Exclure les répertoires contenant le FICHIER
                            
      --exclude-tag-under=FICHIER
                             Tout exclure dans les répertoires contenant le
                             FICHIER
      --exclude-vcs          Exclure les répertoires des gestionnaires de
                             version
      --exclude-vcs-ignores  lire les motifs d'exclusion depuis les fichiers
                             d'exclusion des gestionnaires de version
      --no-null              désactive l'effet de l'option --null précédente
                            
      --no-recursion         Empêcher la descente automatique dans les
                             sous-répertoires
      --no-unquote           Ne pas enlever la protection de caractères du
                             fichier d'entrée ou des noms de membres
      --no-verbatim-files-from   -T traite les noms de fichiers commençants
                             par un tiret comme des options (par défaut)
      --null                 « -T » permet de lire les noms terminés par un
                             NULL ; implique --verbatim-files-from
      --recursion            Parcourir les sous-répertoires de manière
                             récursive (par défaut)
  -T, --files-from=FICHIER   Lire depuis le FICHIER la liste des noms à
                             extraire ou à créer
      --unquote              Enlever la protection de caractères du fichier
                             d'entrée ou des noms de membres (par défaut)
      --verbatim-files-from  -T reads file names verbatim (no escape or option
                             handling)
  -X, --exclude-from=FICHIER Exclure les motifs listés dans le FICHIER

 Options de correspondance de noms de fichiers (pour les motifs d'exclusion et
 d'inclusion)

      --anchored             Les motifs doivent correspondre au début des noms
                             de fichiers
      --ignore-case          Ignorer la casse (majuscules/minuscules)
      --no-anchored          Les motifs peuvent correspondre après n'importe
                             quel « / » (par défaut pour l'exclusion)
      --no-ignore-case       Correspondance sensible à la casse (comportement
                             par défaut)
      --no-wildcards         Correspondance exacte de chaîne
      --no-wildcards-match-slash   « / » ne correspond à aucun caractère de
                             correspondance
      --wildcards            Utiliser des caractères de correspondance (par
                             défaut pour l'exclusion)
      --wildcards-match-slash   « / » peut correspondre à un caractère de
                             correspondance (par défaut pour l'exclusion)

 Mode d'opération principal :

  -A, --catenate, --concatenate   Ajouter des fichiers tar à une archive
  -c, --create               Créer une nouvelle archive
  -d, --diff, --compare      Trouver les différences entre l'archive et le
                             système de fichiers
      --delete               Effacer de l'archive (pas sur les bandes
                             magnétiques !)
  -r, --append               Ajouter des fichiers à la fin de l'archive
  -t, --list                 Afficher le contenu de l'archive
      --test-label           Tester l'étiquette du volume d'archive et
                             terminer
  -u, --update               Ajouter seulement les fichiers plus récents que
                             les copies présentes dans l'archive
  -x, --extract, --get       Extraire les fichiers de l'archive

 Modificateurs d'opération :

      --check-device         vérifier les numéros de périphériques lors de
                             la création d'archives incrémentales (par
                             défaut)
  -g, --listed-incremental=FICHIER
                             Prendre en charge les sauvegardes incrémentales
                             au nouveau format GNU
  -G, --incremental          Prendre en charge les sauvegardes incrémentales
                             à l'ancien format GNU
      --hole-detection=TYPE  technique de détection de trous
      --ignore-failed-read   Ne pas s'arrêter à cause des non-zéros sur les
                             fichiers illisibles
      --level=NOMBRE         niveau de vidage d'archive incrémentale au
                             nouveau format GNU
  -n, --seek                 L'archive peut être parcourue
      --no-check-device      Ne pas vérifier les numéros de périphériques
                             lors de la création d'archives incrémentales
      --no-seek              L'archive ne peut pas être parcourue
      --occurrence[=NOMBRE]  Traiter seulement l'occurrence n°NOMBRE de chaque
                             fichier dans l'archive ; cette option n'est
                             valable qu'accompagnée de l'une des
                             sous-commandes « --delete », « --diff », «
                             --extract » ou « --list » et lorsqu'une liste
                             de fichiers est fournie soit sur la ligne de
                             commande, soit avec l'option « -T ». NOMBRE vaut
                             1 par défaut.
      --sparse-version=MAJEUR[.MINEUR]
                             Définir la version du format de dispersion à
                             utiliser (implique « --sparse »)
  -S, --sparse               Économiser efficacement l'espace dans les
                             fichiers dispersés (fichiers à trous)

 Contrôle de l'écrasement :

  -k, --keep-old-files       Ne pas écraser les fichiers préexistants lors de
                             l'extraction et les traiter comme des erreurs
      --keep-directory-symlink   Écraser les liens symboliques préexistants
                             vers des répertoires lors de l'extraction
      --keep-newer-files     Ne pas écraser les fichier préexistants qui sont
                             plus récents que leur copie dans l'archive
      --no-overwrite-dir     Préserver les métadonnées des répertoires
                             préexistants
      --one-top-level[=RÉP] créer un sous-répertoire pour éviter de perdre
                             les fichiers extraits
      --overwrite            Écraser les fichiers préexistants lors de
                             l'extraction
      --overwrite-dir        Écraser les métadonnées des répertoires
                             préexistants lors de l'extraction (comportement
                             par défaut)
      --recursive-unlink     Vider les hiérarchies avant d'extraire les
                             répertoires
      --remove-files         Supprimer les fichiers après les avoir ajoutés
                             à l'archive
      --skip-old-files       Ne pas écraser les fichiers préexistants lors de
                             l'extraction et les ignorer silencieusement
  -U, --unlink-first         Effacer chaque fichier préexistant avant
                             l'extraction
  -W, --verify               Tenter de vérifier l'archive après écriture

 Choix du flux de sortie :

      --ignore-command-error Ignorer les codes de retour des processus enfants
      --no-ignore-command-error   Traiter les codes de retours non nuls des
                             processus enfants comme des erreurs
  -O, --to-stdout            Extraire les fichiers vers la sortie standard
      --to-command=COMMANDE  Renvoyer par tube les fichiers extraits vers un
                             autre programme

 Traitement des attributs de fichiers :

      --atime-preserve[=MÉTHODE]
                             Préserve la date d'accès des fichiers archivés,
                             soit en la restaurant après lecture (MÉTHODE =
                             « replace » par défaut) ou en ne définissant
                             pas les dates initialement (MÉTHODE = « system
                             »)
      --clamp-mtime          Définir la date seulement lorsque le fichier est
                             plus récent que la valeur de l’argument
                             --mtime
      --delay-directory-restore   Reporter à la fin de l'extraction le
                             changement des dates de modification et des
                             permissions des répertoires extraits
      --group=NOM            Utiliser NOM comme groupe des fichiers ajoutés
      --group-map=FICHIER    utilise FICHIER pour la correspondance des noms et
                             GIDs du propriétaire
      --mode=CHANGEMENTS     Utiliser les CHANGEMENTS de mode (symboliques)
                             pour les fichiers ajoutés
      --mtime=DATE-OU-FICHIER   Définir la date de modification des fichiers
                             ajoutés avec DATE-OU-FICHIER
  -m, --touch                Ne pas extraire la date de modification du
                             fichier
      --no-delay-directory-restore
                             Annule l'effet de l'option «
                             --delay-directory-restore »
      --no-same-owner        S'approprier les fichiers lors de l'extraction
                             (par défaut pour les utilisateurs ordinaires)
      --no-same-permissions  Appliquer l'umask de l'utilisateur lors de
                             l'extraction des permissions (par défaut pour les
                             utilisateurs normaux)
      --numeric-owner        Toujours utiliser les valeurs numériques des
                             utilisateurs/groupes
      --owner=NOM            Utiliser NOM comme propriétaire des fichiers
                             ajoutés
      --owner-map=FICHIER    utilise FICHIER pour la correspondance des noms et
                             UIDs du propriétaire
  -p, --preserve-permissions, --same-permissions
                             Extraire les informations de permissions sur les
                             fichiers (par défaut pour le superutilisateur)
      --same-owner           essayer d'extraire les fichiers avec le même
                             propriétaire que dans l'archive (par défaut pour
                             le superutilisateur)
  -s, --preserve-order, --same-order
                             les arguments des membres sont listés dans le
                             même ordre que les fichiers de l'archive
      --sort=ORDRE           ordre de tri du répertoire : aucun (par défaut),
                             nom ou inode

 Traitement des attributs de fichiers étendus :

      --acls                 Activer la prise en charge des ACL POSIX
      --no-acls              Désactiver la prise en charge des ACL POSIX
      --no-selinux           Désactiver la prise en charge du contexte SELinux
                            
      --no-xattrs            Désactiver la prise en charge des attributs
                             étendus
      --selinux              Activer la prise en charge du contexte SELinux
      --xattrs               Activer la prise en charge des attributs étendus
      --xattrs-exclude=MASQUE   spécifier le motif d'exlusion pour les clefs
                             xattr
      --xattrs-include=MASQUE   spécifier le motif d'inclusion pour les clefs
                             xattr

 Sélection et option de périphérique :

  -f, --file=ARCHIVE         Utiliser le fichier ou le périphérique ARCHIVE
      --force-local          Le fichier d'archive est local même si « : » a
                             été spécifié
  -F, --info-script=NOM, --new-volume-script=NOM
                             Exécuter le script à la fin de chaque cartouche
                             (implique « -M »)
  -L, --tape-length=NOMBRE   Changer de cartouche après avoir écrit NOMBRE x
                             1024 octets
  -M, --multi-volume         Créer/lister/extraire une archive multi-volumes
      --rmt-command=COMMANDE Utiliser la COMMANDE rmt fournie au lieu de rmt
      --rsh-command=COMMANDE Utiliser la COMMANDE distante à la place de rsh
      --volno-file=FICHIER   Utiliser/mettre à jour le numéro de volume dans
                             le FICHIER

 Blocs du périphérique :

  -b, --blocking-factor=BLOCS   BLOCS x 512 octets par enregistrement
  -B, --read-full-records    Refaire les blocs pendant la lecture (pour les
                             tubes BSD 4.2)
  -i, --ignore-zeros         Ignorer les blocs de zéros dans l'archive (càd
                             EOF)
      --record-size=NOMBRE   NOMBRE d'octets par enregistrement (multiple de
                             512)

 Sélection du format d'archive :

  -H, --format=FORMAT        Créer l'archive au format désiré.
  -Y, --lzma                 filter the archive through lzma (deprecated flag)

 FORMAT peut prendre une des valeurs suivantes :

    gnu                      Format GNU tar 1.13.x
    oldgnu                   Format GNU issu de tar <= 1.12
    pax                      Format POSIX 1003.1-2001 (pax)
    posix                    Identique à pax
    ustar                    Format POSIX 1003.1-1988 (ustar)
    v7                       Vieux format tar V7

      --old-archive, --portability
                             Identique à « --format=v7 »
      --pax-option=mot_clé[[:]=valeur][,mot_clé[[:]=valeur]...
                             Mots-clés de contrôle pax
      --posix                Identique à « --format=posix »
  -V, --label=TEXTE          Créer l'archive en attribuant le TEXTE au nom de
                             volume. À la lecture ou à l'extraction, utiliser
                             le TEXTE comme motif de correspondance (glob) au
                             nom de volume.

 Options de compression :

  -a, --auto-compress        Utiliser le suffixe de l'archive pour déterminer
                             le programme de compression
  -I, --use-compress-program=PROG
                             Filtrer à travers le PROG (doit accepter l'option
                             « -d »)
  -j, --bzip2                Filtrer l'archive à travers bzip2
  -J, --xz                   Filtrer l'archive à travers xz
      --lzip                 Filtrer l'archive à travers lzip
      --lzma                 Filtrer l'archive à travers xz --format=lzma
      --lzop                 Filtrer l'archive à travers lzop
      --no-auto-compress     Ne pas utiliser l'extension du fichier d'archive
                             pour déterminer le programme de compression
  -z, --gzip, --gunzip, --ungzip   Filtrer l'archive à travers gzip
      --zstd                 Filtrer l'archive à travers zstd
  -Z, --compress, --uncompress   Filtrer l'archive à travers compress

 Sélection des fichiers locaux :

      --backup[=CONTRÔLE]   Faire une copie de sauvegarde avant suppression,
                             choisir le CONTRÔLE de version
  -h, --dereference          Suivre les liens symboliques ; archiver les
                             fichiers vers lesquels ils pointent
      --hard-dereference     Suivre les liens physiques : archiver les fichiers
                             vers lesquels ils pointent
  -K, --starting-file=NOM-DE-MEMBRE
                             Débuter au NOM-DE-MEMBRE durant la lecture de
                             l'archive
      --newer-mtime=DATE     Ne comparer que la date et l'heure de modification
                             des données
  -N, --newer=DATE-OU-FICHIER, --after-date=DATE-OU-FICHIER
                             Stocker seulement les fichiers plus récents que
                             DATE-OU-FICHIER
      --one-file-system      Rester dans le système de fichiers local lors de
                             la création de l'archive
  -P, --absolute-names       Ne pas enlever le « / » au début des noms de
                             fichiers
      --suffix=CHAÎNE       Faire une copie de sauvegarde avant suppression,
                             en remplaçant le suffixe habituel (« ~ » sauf
                             s'il est définit par la variable d'environnement
                             SIMPLE_BACKUP_SUFFIX)

 Transformation des noms de fichiers :

      --strip-components=NOMBRE   Supprimer NOMBRE composants au début des
                             noms de fichiers à l'extraction
      --transform=EXPRESSION, --xform=EXPRESSION
                             Utiliser l'EXPRESSION de remplacement « sed »
                             pour transformer les noms de fichiers

 Options d'affichage :

      --checkpoint[=NOMBRE]  Afficher un message de progression tous les NOMBRE
                             enregistrements (10 par défaut)
      --checkpoint-action=ACTION   exécuter l'ACTION à chaque point de
                             contrôle
      --full-time            afficher l'heure du fichier en pleine résolution
      --index-file=FICHIER   Envoyer la sortie détaillée vers le FICHIER
  -l, --check-links          Afficher un message si tous les liens n'ont pas pu
                             être suivis et archivés
      --no-quote-chars=CHAÎNE   Enlever la protection des caractères faisant
                             partie de la CHAÎNE
      --quote-chars=CHAÎNE  Protéger aussi les caractères faisant partie de
                             la CHAÎNE
      --quoting-style=STYLE  Définir le style de protection de caractères
                             appliqués aux noms. Voir ci-dessous pour les
                             valeurs du STYLE
  -R, --block-number         Afficher le numéro du bloc de l'archive avec
                             chaque message
      --show-defaults        Afficher les paramètres par défaut de tar
      --show-omitted-dirs    Lors du listage ou de l'extraction, lister chaque
                             répertoire qui ne concorde pas avec le critère
                             de recherche
      --show-snapshot-field-ranges
                             afficher les intervalles valides des champs des
                             fichiers d'instantané
      --show-transformed-names, --show-stored-names
                             Afficher les noms des fichiers ou des archives
                             après transformation
      --totals[=SIGNAL]      Afficher le nombre total d'octets après
                             traitement de l'archive. Avec un argument,
                             afficher ce nombre si le SIGNAL est émis. Les
                             signaux permis sont : SIGHUP, SIGQUIT, SIGINT,
                             SIGUSR1 et SIGUSR2. Les noms sans préfixe « SIG
                             » sont aussi acceptés
      --utc                  Afficher les dates de modification de fichier en
                             UTC
  -v, --verbose              Afficher de manière détaillée les fichiers
                             traités
      --warning=MOTCLÉ      Contrôle d'avertissement
  -w, --interactive, --confirmation
                             Demander confirmation pour chaque action

 Options de compatibilité :

  -o                         Lors de la création, identique à «
                             --old-archive ». Lors de l'extraction, identique
                             à « --no-same-owner »

 Autres options :

  -?, --help                 Afficher cette aide-mémoire
      --restrict             Désactiver certaines options potentiellement
                             néfastes
      --usage                Afficher un court mode d'emploi
      --version              Afficher la version du programme

Les arguments obligatoires ou facultatifs pour les formes longues des options
le sont également pour les formes courtes qui leur correspondent.

The backup suffix is '~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.
The version control may be set with --backup or VERSION_CONTROL, values are:

  none, off       never make backups
  t, numbered     make numbered backups
  nil, existing   numbered if numbered backups exist, simple otherwise
  never, simple   always make simple backups

Les arguments valables pour l'option « --quoting-style » sont :

  literal
  shell
  shell-always
  shell-escape
  shell-escape-always
  c
  c-maybe
  escape
  locale
  clocale

Les valeurs par défaut de *ce* tar sont :
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/usr/sbin/rmt
--rsh-command=/usr/bin/ssh
```
- Pour compiler en natif. A partir du centre de contrôle Mageia installer ou vérifier si les paquets suivants sont installés :
  - glibc-devel,
  - zlib-devel (header files for the C library and zlib),
  - gcc,
  - libstdc++-static.
- Détail des utilisations et des options pour la commande `GraalVM Updater` : `./gu`
```
GraalVM Component Updater v2.0.0

Usage:
        gu info [-cClLnprstuvV] <param>         prints info about specific component (from file, URL or catalog)
        gu available [-aClvV] <expr>            lists components available in catalog
        gu install [-0CcDfiLMnosruvyxY] <param> installs a component package
        gu list [-clv] <expression>             lists installed components, or components from catalog
        gu remove [-0DfMxv] <id>                uninstalls a component
        gu update [-cCnLsux] [<ver>] [<param>]  upgrades to recent GraalVM
        gu rebuild-images                       rebuilds native images. Use -h for detailed usage

Common options:
  -A, --auto-yes              say YES or ACCEPT to all questions.
  -c, --catalog               treat parameters as component IDs from catalog of GraalVM components. This is the default.
  -C, --custom-catalog <url>  use user-supplied catalog at URL.
  -e, --debug                 debugging. Prints stacktraces, ...
  -E, --no-catalog-errors     do not stop, if at least one catalog is working.
  -h, --help                  print help.
  -L, --local-file, --file    treat parameters as local filenames of packaged components.
  -N, --non-interactive       noninteractive mode. Fail when input is required.
  --show-version              print version information and continue.
  -u, --url                   interpret parameters as URLs of packaged components.
  -v, --verbose               be verbose. Prints versions and dependency info.
  --version                   print version.

Use
        gu <command> -h
to get specific help.

Runtime options:
  --native                                     Run using the native launcher with limited Java access (default).
  --jvm                                        Run on the Java Virtual Machine with Java access.
  --vm.[option]                                Pass options to the host VM. To see available options, use '--help:vm'.
  --log.file=<String>                          Redirect guest languages logging into a given file.
  --log.[logger].level=<String>                Set language log level to OFF, SEVERE, WARNING, INFO, CONFIG, FINE, FINER, FINEST or ALL.
  --help                                       Print this help message.
  --help:vm                                    Print options for the host VM.
```
  - Pour avoir la liste des modules déjà installés : `./gu list`
```
ComponentId              Version             Component name      Origin 
--------------------------------------------------------------------------------
graalvm                  20.2.0              GraalVM Core 
```
  - Pour obtenir les composants supplémentaires de GraalVM installables : `./gu available`
```
Downloading: Component catalog from www.graalvm.org
ComponentId              Version             Component name      Origin 
--------------------------------------------------------------------------------
llvm-toolchain           20.2.0              LLVM.org toolchain  github.com
native-image             20.2.0              Native Image        github.com
python                   20.2.0              Graal.Python        github.com
R                        20.2.0              FastR               github.com
ruby                     20.2.0              TruffleRuby         github.com
wasm                     20.2.0              GraalWasm           github.com
```
  - Pour installer le composant `Native Image` `./gu install native-image`
  - Pour installer le composant `Graal.Python` `./gu install python`

- Extraire l'archive zip `graalvm-svm-linux-20.1.0-ea+28.zip` dans le répertoire `/opt`
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
  -U  use escapes for all non-ASCII Unicode  -UU ignore any Unicode fields
  -C  match filenames case-insensitively     -L  make (some) names lowercase
  -X | -k  restore UID/GID | permissions     -V  retain VMS version numbers
  -K  keep setuid/setgid/tacky permissions   -D- restore dir (-D: no) times
  -M  pipe through "more" pager
More help: unzip -hh   Examples:
  unzip data1 -x joe   # Extract all files except joe from archive data1.zip
  unzip -p foo | more  # Pipe contents of foo.zip into program "more"
  unzip -fo foo ReadMe # Replace quietly existing ReadMe if archive file newer
```

  > Exemples (à exécuter en mode administrateur) :
  > Pour modifier la variable d'environnement `PAT`H de manière provisoire dans une console : `export PATH=$PATH:/opt/graalvm-ce-java11-20.2.0/bin`.
  >
  > Voir et modifier la configuration courante pour `java` : `update-alternatives --config java`
  >
  > Ajouter une alternative :
  > - `update-alternatives --install /usr/bin/java java /opt/graalvm-ce-java11-20.2.0/bin/java 3000`
  > - `update-alternatives --install /usr/bin/javac javac /opt/graalvm-ce-java11-20.2.0/bin/javac 3000` 
  >
  > Enlever une alternative :
  > - `update-alternatives --remove java /opt/graalvm-ce-java11-20.2.0/bin/java`
  > - `update-alternatives --remove javac /opt/graalvm-ce-java11-20.2.0/bin/javac`
  >

# Linux/Android :
- Mettre en place un environnement sur Linux OS ou WSL2, comme indiqué précédemment.

  Actuellement, Android ne peut être construit que sur Linux OS (ou à partir de Windows WSL2).

  Le SDK Android est requis pour créer des applications pour la plate-forme Android. Le SDK Android sera téléchargé automatiquement par le plugin client et configuré avec les packages requis.

  Si vous disposez déjà d'une installation locale du SDK Android, vous pouvez remplacer ce comportement en définissant des variables d'environnement nommées ANDROID_SDK, qui pointe vers le dossier Android SDK sur votre système, et ANDROID_NDK pointant vers le dossier ndk-bundle. Elles peuvent être définies directement dans la console ou dans le fichier `.bash_profile` du répertoire utilisateur, en utilisant les commandes suivantes :
  - `export ANDROID_SDK=/home/scientificware2016/android_sdk`
  - `export ANDROID_NDK=/home/scientificware2016/android_sdk/ndk-bundle`

- S'assurer que les packages requis suivants sont bien installés, en utilisant '/android_sdk/cmdline-tools/latest/bin/sdkmanager' comme indiqué sur la fiche installation de Gluon Plugin : 
  - platform-tools
  - platforms;android-29
  - build-tools;29.0.3
  - ndk-bundle
  - extras;android;m2repository
  - extras;google;m2repository

  > Exemple :
  > - `./sdkmanager --version` pour connaître la version de sdkmanager.
  > - `./sdkmanager --list` pour afficher la liste des composants disponibles.
  > - ./sdkmanager "extras;google;m2repository" pour install le package `extras;google;m2repository`
  > - ...

# macOS et iOS :
- ...
