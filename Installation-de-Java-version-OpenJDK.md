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
  - Depuis une console de commande : 
    - `setx /M PATH "C:\Program Files\Java\..\bin;%PATH%"`
    - `setx /M JAVA_HOME "C:\Program Files\Java\.."`
  - Depuis l'interface graphique de Windows :
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


# Linux : (Amélioration possible : ajouter un script pour automatiser tous les nouveaux liens)
- Télécharger la dernière version : à partir du site [jdk.java.net/](http://jdk.java.net/)
- Vérifier la signature avec : `sha256sum`.
  > Exemple : `sha256sum openjdk-15.0.1_linux-x64_bin.tar.gz`
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
- Extraire l'archive avec `tar` dans `/opt/java`:
  > Exemple : `tar -xf openjdk-15.0.1_linux-x64_bin.tar.gz -C /opt/java`
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
- Utiliation d'UnZip :

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
  > Exemple : `unzip -d /usr/java openjfx-15.0.1_linux-x64_bin-sdk.zip`
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
  > Exemples (à exécuter en mode administrateur) :
  >
  > Voir et modifier la configuration courante pour `java` : `update-alternatives --config java`
  >
  > Ajouter une alternative :
  > - `update-alternatives --install /usr/bin/java java  /usr/java/jdk-12.0.1/bin/java 3000`
  > - `update-alternatives --install /usr/bin/javac javac  /usr/java/jdk-12.0.1/bin/javac 3000` 
  >
  > Enlever une alternative :
  > - `update-alternatives --remove java /usr/java/jdk-12.0.1/bin/java`
  > - `update-alternatives --remove javac /usr/java/jdk-12.0.1/bin/javac`
  >
  > Dans une console, cela peut être sur redéfini dans les fichiers `.bashrc` puis `.bash_profile` à l'aide des variables `JAVA_HOME` et `JDK_HOME` :
  > - `export JAVA_HOME=$HOME/Documents/jdk-12.0.1`
  > - `export JDK_HOME=$HOME/Documents/jdk-12.0.1`
