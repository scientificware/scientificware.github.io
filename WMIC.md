WMIC (Windows Management Instrumentation Command-line)
----
Description : Interface de ligne de commande permettant l'accès à l’infrastructure de gestion Windows (WMI) et la gestion d'un système d’exploitation de la famille Windows.

Référence : [Windows Dev Center](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmic).

>Exemple :
>- Pour obtenir la clé d'activation de sa version Windows 10
>  - Ouvrir une invite de commande,
>  - Coller la commande suivante : `wmic path softwarelicensingservice get OA3xOriginalProductKey`.