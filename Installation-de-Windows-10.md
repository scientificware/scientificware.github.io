Attention sous Windows 10 l'encodage par défaut n'est pas utf-8.
----
Pour le modifier : 
- Démarrer > Paramètres > Heure et langue > Langue > Paramètres de la langue d'administration > Modifier les paramètres régionnaux.
- Cocher la case : Utiliser le format Unicode UTF-8.

  ![windows10_utf_8](https://user-images.githubusercontent.com/19194678/97777129-5e74d680-1b6e-11eb-8b6f-9a90b872b069.png)

- Pour vérifier si la modification a bien eu lieu. 
  - Lancer un PowerShell (Dans l'explorateur de fichier, Menu > Fichier).
  - Puis saisir la commande `[System.Text.Encoding]::Default`
  ![windows10_check_utf_8](https://user-images.githubusercontent.com/19194678/97777402-42723480-1b70-11eb-9795-89b6940734a1.png)
