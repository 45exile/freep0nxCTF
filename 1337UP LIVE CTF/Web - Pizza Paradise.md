Web - Pizza Paradise

Pour commencer, on pense à regarder le /robots.txt 

![test](Images/20241116210409.png)

On y trouve un répertoire secret qui nous mène à une page de login 

![test](Images/20241116210439.png)

Dans le code source de la page, on voit un fichier auth.js contenant le nom d'utilisateur et son mot de passe sous forme de hash 

![test](Images/20241116210523.png)

On crack le hash grâce au site Crackstation pour obtenir le mot de passe 

![test](Images/20241116210634.png)

On se connecte avec ce mot de passe et on arrive sur une page qui permet de télécharger des images 

![test](Images/20241116211115.png)

En interceptant la requête, on remarque que le chemin est potentiellement vulnérable aux LFI 

![test](Images/20241116211412.png)

On commence les tests en mettant /etc/passwd 

![test](Images/20241116211453.png)

Le chemin n'est pas autorisé, alors on tente en laissant le chemin de base /assets/images 

![test](Images/20241116211559.png)

Cela fonctionne ! Pour le flag, on le trouve en affichant le code source de la page actuelle 

![test](Images/20241116211740.png)
