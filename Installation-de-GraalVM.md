[GraalVM Website](https://www.graalvm.org/)

![logo_graalwm](https://user-images.githubusercontent.com/19194678/93709640-28404200-fb40-11ea-8a2d-c1ff028735d5.png)

GraalVM is a universal virtual machine for running applications written in JavaScript, Python, Ruby, R, JVM-based languages like Java, Scala, Kotlin, Clojure, and LLVM-based languages such as C and C++. 

[SubstrateVM on AArch64](https://github.com/oracle/graal/pull/910)

# Windows 10 :
- Télécharger la dernière version : à partir du site [graalvm.org/](https://www.graalvm.org/downloads/)
- Extraire l'archive dans `C:\Program Files\GraalVM`

- Lire https://github.com/gluonhq/client-maven-plugin/
- Installer Visual Studio puis lancer Visual Studio Installer pour installer les composants nécessaires.

- lire https://www.graalvm.org/reference-manual/native-image/ pour la compilation en mode natif.

# Linux :
- Extraire l'archive zip `graalvm-svm-linux-20.1.0-ea+28.zip` dans le répertoire `/opt`
- Extraire l'archive tar.gz `tar -xf graalvm-ce-java11-linux-amd64-20.2.0-dev.tar.gz -C /opt`
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
