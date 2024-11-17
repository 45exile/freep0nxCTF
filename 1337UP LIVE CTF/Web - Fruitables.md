**Web - Fruitables**

Ici, aucun code source n’est fourni, donc j'ai dû tâtonner. Avec gobuster, j’ai trouvé un répertoire intéressant 

![test](Images/20241116220024.png)

On remarque deux éléments : le répertoire /upload et /account.php, qui permet de se connecter. Mais sans identifiants, et avec l'inscription désactivée...

En testant une quote ' dans le champ utilisateur, on obtient une réponse intéressante 

![test](Images/20241116220206.png)

Probablement une SQLi ? Après quelques tests manuels infructueux, je lance sqlmap 

![test](Images/20241116220335.png)



![test](Images/20241116220349.png)

Excellent ! 

![test](Images/20241116220430.png) 

Une fois le mot de passe administrateur cracké, j’accède au dashboard avec une fonctionnalité d’upload.

On teste d'upload un webshell, mais c'est bloqué. 

![test](Images/20241116220720.png)

En analysant la requête dans Burp Suite, je change l'extension en .jpg, ajoute une signature JPEG au début du fichier, modifie le Content-Type... et ça passe ! 

![test](Images/20241116221201.png)



![test](Images/20241116221318.png) Il ne reste qu’à afficher le flag 

![test](Images/20241116221410.png)
