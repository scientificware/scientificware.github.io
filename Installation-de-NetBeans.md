![logo_appache_netbeans](https://user-images.githubusercontent.com/19194678/50519061-5272e600-0ab9-11e9-9794-6e8b85006c2b.png)
- Télécharger la dernière version : à partir du site [Apache NetBeans](https://netbeans.apache.org/)
- Vérification d'intégrité du fichier téléchargé avec : `certutil`

# Windows 10 :
- Sous windows : `certutil.exe -hashfile Apache-NetBeans-12.0-bin-windows-x64.exe SHA512`
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
# Linux :
- Télécharger la dernière version : à partir du site [Apache NetBeans](https://netbeans.apache.org/)
- Vérification d'intégrité du fichier téléchargé avec :
  - En ligne de commande : md5sum, sha1sum, sha256sum, sha512sum
    > Exemple : `sha512sum incubating-netbeans-11.0-bin.zip`
    - utilisation de sha512sum :
      ```
      Usage: sha512sum [OPTION]... [FILE]...
      Print or check SHA512 (512-bit) checksums.
      
      Sans FICHIER ou quand FICHIER est -, lire l'entrée standard.
      
        -b, --binary         read in binary mode
        -c, --check          lire les sommes SHA512 à partir des FICHIERs et les vérifier
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
      
      The sums are computed as described in FIPS-180-2.  When checking, the input
      should be a former output of this program.  The default mode is to print a
      line with checksum, a space, a character indicating input mode ('*' for binary,
      ' ' for text or where binary is insignificant), and name for each FILE.
      
      Aide en ligne de GNU coreutils : <http://www.gnu.org/software/coreutils/>
      Signalez les problèmes de traduction de « sha512sum » à : <traduc@traduc.org>
      Full documentation at: <http://www.gnu.org/software/coreutils/sha512sum>
      or available locally via: info '(coreutils) sha2 utilities'
      ```
  - Clic droit sur le fichier, Propriétés, Sommes de contrôle.
