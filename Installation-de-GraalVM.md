[GraalVM Website](https://www.graalvm.org/)

![logo_graalwm](https://user-images.githubusercontent.com/19194678/93709640-28404200-fb40-11ea-8a2d-c1ff028735d5.png)

GraalVM is a universal virtual machine for running applications written in JavaScript, Python, Ruby, R, JVM-based languages like Java, Scala, Kotlin, Clojure, and [LLVM](https://llvm.org/)-based languages such as C and C++. 

[SubstrateVM on AArch64](https://github.com/oracle/graal/pull/910)

Prérequis pour suivre le tutoriel : [GraalVM GET STARTED](https://www.graalvm.org/docs/getting-started/).

Limitation(s) du moment :
- Pas de compilation native sous Windows ou Linux avec Swing : [[native-image] Windows with a swing application](https://github.com/oracle/graal/issues/1327)


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
  - Pour définir la variable d'environnement JAVA_HOME `setx /M JAVA_HOME "C:\Progra~1\GraalVM\graalvm-ce-java11-20.2.0"`

- lire https://www.graalvm.org/reference-manual/native-image/ pour la compilation en mode natif.

- Exemple d'utilisation : `native-image -jar "Loto_2020.jar"`, `native-image HelloWorld`, ...

# Linux :
- Extraire l'archive tar.gz `tar -xf graalvm-ce-java11-linux-amd64-20.2.0.tar.gz -C /opt`
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
graalvm                  20.2.0-dev          GraalVM Core 
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
> Exemple : `unzip -d /opt graalvm-svm-linux-20.1.0-ea+28.zip`

