# Aide a moi meme pour mes commandes

## norm
Execute la norminette sur tous les fichiers .c et .h avec le flag -R CheckForbiddenSourceHeader.

Usage :
- `norm` : Vérifie tous les fichiers .c et .h du répertoire courant.
- `norm file1.c file2.c` : Vérifie les fichiers spécifiés.
- `norm -exclude test.c libft.h` : Vérifie tous les fichiers sauf ceux exclus.

## norm2
Execute la norminette sur tous les fichiers .h avec le flag -R CheckDefine

Usage :
- Meme fonctionnement que norm

## newfile
Crée un nouveau fichier et l'ouvre dans une nouvelle fenetre Vim avec l'en-tête 42, les numeros de ligne et une grande taille de fenetre.

Usage : `newfile <nom_du_fichier>`

## vimopen
Ouvre un fichier existant dans Vim avec les memes parametres que newfile.

Usage : `vimopen <nom_du_fichier>`

## ginit
Initialise un nouveau dépôt Git pret a utiliser avec une URL donee.

Usage : `ginit <url_du_depot_distant>`

## gpush
Effectue un add, commit et push vers les dépôts origin et github.
#### Toujours etre dans le dossier racine du projet souhaite avant de gpush

Usage : 
- `gpush <fichier1> [fichier2] ...` : push les fichiers spécifiés.
- `gpush <fichier1> [fichier2] ... -m "message de commit"` : Ajoute un message de commit personnalisé.

## grestore
Recupere la backup de tous les projets realises.

Usage : `grestore`

## greinit
Réinitialise un dépôt Git de projet avec une URL donee (en cas de reinstallation de backup pour le rendre pret a utiliser).

Usage : `greinit <url_du_depot>`

## start
Compile et exécute des fichiers C avec les flags -Wall -Wextra -Werror et vérifie la norme.

Usage :
- `start fichier1.c [fichier2.c ...]` : Compile et exécute les fichiers.
- `start -c fichier1.c [fichier2.c ...]` : Compile uniquement sans exécuter.
### Note: A revoir, elle ne marche pas avec les fichiers necessitant une librairie locale.

## examstart
Exécute 'make' deux fois avec une pause de 0.5 seconde entre les deux, a executer dans le dossier de lexamshell.

Usage : `examstart`
