Web - Pizza Paradise

Pour commencer, on pense à regarder le /robots.txt ![Images/Pasted image 20241116210409.png].

On y trouve un répertoire secret qui nous mène à une page de login ![Images/Pasted image 20241116210439.png].

Dans le code source de la page, on voit un fichier auth.js contenant le nom d'utilisateur et son mot de passe sous forme de hash ![Images/Pasted image 20241116210523.png].

On crack le hash grâce au site Crackstation pour obtenir le mot de passe ![Images/Pasted image 20241116210634.png].

On se connecte avec ce mot de passe et on arrive sur une page qui permet de télécharger des images ![Images/Pasted image 20241116211115.png].

En interceptant la requête, on remarque que le chemin est potentiellement vulnérable aux LFI ![Images/Pasted image 20241116211412.png].

On commence les tests en mettant /etc/passwd ![Images/Pasted image 20241116211453.png].

Le chemin n'est pas autorisé, alors on tente en laissant le chemin de base /assets/images ![Images/Pasted image 20241116211559.png].

Cela fonctionne ! Pour le flag, on le trouve en affichant le code source de la page actuelle ![Images/Pasted image 20241116211740.png].

Web - BioCorp

Pour ce challenge, un fichier .zip contenant le code source du site est fourni.

On repère rapidement le fichier panel.php, qui demande un header spécifique pour y accéder ![Images/Pasted image 20241116212236.png].

![Images/Pasted image 20241116222648.png].

On peut ensuite cliquer sur un Control Panel qui nous emmène ici : ![Images/Pasted image 20241116222746.png].

Grâce au code source de la page, on peut poursuivre notre investigation ! ![Images/Pasted image 20241116222820.png].

Cela fait penser à une XXE. On tente donc une attaque ![Images/Pasted image 20241116223440.png].

Rien de très compliqué ici. On peut ensuite afficher le flag ![Images/Pasted image 20241116223522.png].

Web - Cat Club

Pour ce challenge, un fichier .zip contenant le code source du site est fourni.

Le site permet de se connecter et d'afficher une galerie de chats. On remarque que notre pseudo est reflété sur cette page, ce qui nous fait penser à une possible SSTI ![Images/Pasted image 20241116214032.png].

Cependant, l'enregistrement n'accepte que des noms d'utilisateur contenant des chiffres ou des lettres. En revanche, on voit que le site utilise des JWT. Avec le code source, on peut probablement en forger un...

On trouve un endpoint intéressant ici : ![Images/Pasted image 20241116214439.png] et ![Images/Pasted image 20241116214456.png].

Avec ces informations, on forge un JWT avec l'algorithme HS256 et choisit un username. Testons avec #{9*9} ! ![Images/Pasted image 20241116214654.png].

Cela fonctionne ! ![Images/Pasted image 20241116214859.png].

Grâce au code source, on sait que le moteur de template utilisé est PugJS, alors on utilise un payload trouvé sur HackTricks, mais aucun retour sur la page web.

En testant l'exécution de la commande id : ![Images/Pasted image 20241116215217.png].

Le champ retourné est vide : ![Images/Pasted image 20241116215239.png].

On vérifie si la commande s'exécute côté serveur en utilisant curl pour envoyer une réponse externe. ![Images/Pasted image 20241116215537.png].

Et on obtient : ![Images/Pasted image 20241116215517.png].

Trouver le flag devient ensuite une formalité ![Images/Pasted image 20241116215724.png].

Web - Fruitables

Ici, aucun code source n’est fourni, donc j'ai dû tâtonner. Avec gobuster, j’ai trouvé un répertoire intéressant ![Images/Pasted image 20241116220024.png].

On remarque deux éléments : le répertoire /upload et /account.php, qui permet de se connecter. Mais sans identifiants, et avec l'inscription désactivée...

En testant une quote ' dans le champ utilisateur, on obtient une réponse intéressante ![Images/Pasted image 20241116220206.png].

Probablement une SQLi ? Après quelques tests manuels infructueux, je lance sqlmap ![Images/Pasted image 20241116220335.png].

![Images/Pasted image 20241116220349.png].

Excellent ! ![Images/Pasted image 20241116220430.png]. Une fois le mot de passe administrateur cracké, j’accède au dashboard avec une fonctionnalité d’upload.

On teste d'upload un webshell, mais c'est bloqué. ![Images/Pasted image 20241116220720.png].

En analysant la requête dans Burp Suite, je change l'extension en .jpg, ajoute une signature JPEG au début du fichier, modifie le Content-Type... et ça passe ! ![Images/Pasted image 20241116221201.png].

![Images/Pasted image 20241116221318.png]. Il ne reste qu’à afficher le flag ![Images/Pasted image 20241116221410.png].
