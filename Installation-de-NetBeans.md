![logo_appache_netbeans](https://user-images.githubusercontent.com/19194678/50519061-5272e600-0ab9-11e9-9794-6e8b85006c2b.png)

Vérification d'intégrité du fichier téléchargé :

# Windows 10 :
- Sous windows : `certutil.exe -hashfile incubating-netbeans-10.0-bin.zip SHA512`
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