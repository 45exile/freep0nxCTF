Write UP 1337UP LIVE 2024 CTF

Web - Pizza Paradise

Pour commencer on pense à regarder le /robots.txt
![[Pasted image 20241116210409.png]]

On y trouve un répertoire secret qui nous mène à une page de login
![[Pasted image 20241116210439.png]]

Dans le code source de la page on voit un fichier auth.js avec le nom d'utilisateur et son mdp sous forme de hash
![[Pasted image 20241116210523.png]]

On crack le hash grâce au site crackstation pour obtenir le mdp
![[Pasted image 20241116210634.png]]

On se connecte grâce à ce mdp et on arrive sur une page qui nous permet de télécharger des images.
![[Pasted image 20241116211115.png]]

On intercepte la requête et on remarque que le chemin est potentiellement vulnérable aux LFI.
![[Pasted image 20241116211412.png]]

On commence les tests en mettant /etc/passwd
![[Pasted image 20241116211453.png]]

Le chemin n'est pas autorisé, alors on tente en laissant le chemin de base (/assets/images)
![[Pasted image 20241116211559.png]]

Cela fonctionne ! Pour le flag on le trouve en affichant le code de la page où on se trouve 
![[Pasted image 20241116211740.png]]


Web - BioCorp

Pour ce challenge on nous fournit un .zip contenant le code source du site.

On repère rapidement le fichier panel.php qui demande un header spécifique pour y accéder
![[Pasted image 20241116212236.png]]

![[Pasted image 20241116222648.png]]

On peut ensuite cliquer sur un Control Panel qui nous emmène ici : ![[Pasted image 20241116222746.png]]

On nous a donné le code source de la page donc autant en profiter !
![[Pasted image 20241116222820.png]]

Cela fait grandement penser à une XXE tout ça, alors c'est parti.

![[Pasted image 20241116223440.png]]

Rien de très compliqué ici, on peut ensuite afficher le flag.

![[Pasted image 20241116223522.png]]

Web - Cat Club

Pour ce challenge on nous fournit un .zip contenant le code source du site.

Nous avons un site nous permettant de nous connecter et ensuite d'afficher une gallerie de chat. On remarque que notre pseudo est reflété sur cette page, alors on pense directement à plusieurs faille, dont une SSTI.
![[Pasted image 20241116214032.png]]

Mais le problème c'est que nous ne pouvons pas s'enregistrer avec un nom d'utilisateur contenant autre chose que des chiffres et des lettres.
Par contre, on voit que le site utilise des JWT, et avec le code source du site on peut sûrement arriver à en forger...

On trouve un endpoint intéressant ici 
![[Pasted image 20241116214439.png]]
![[Pasted image 20241116214456.png]]

Avec ces infos on peut forger un JWT avec l'alg HS256 et choisir l'username qu'on veut. Testons avec #{9*9} !
![[Pasted image 20241116214654.png]]

Cela fonctionne !
![[Pasted image 20241116214859.png]]

Grâce au code source du site on sait que le moteur de template utilisé est PugJS, alors on utilise un payload trouvé sur hacktricks mais on n'obtient aucun retour sur la page web.

En tentant ici d'exécuter la commande "id" :
![[Pasted image 20241116215217.png]]

Le champ retourné est vide :
![[Pasted image 20241116215239.png]]

Mais qui nous dit que la commande n'est pas exécuté quand même côté serveur ? On test alors de renvoyer le résultat grâce à curl.
![[Pasted image 20241116215537.png]]

Et on obtient :
![[Pasted image 20241116215517.png]]

Trouver le flag devient ensuite une formalité.
![[Pasted image 20241116215724.png]]


Web - Fruitables

Ici nous n'avons pas de code source, donc au début j'ai mis un peu de temps à trouver par où commencer.
Mais grâce à gobuster j'ai réussi à trouver la voie !
![[Pasted image 20241116220024.png]]

On remarque deux choses intéressantes, le répertoire /upload, et /account.php qui nous permet de se connecter ! Sauf qu'on a aucun identifiant, et l'option pour s'enregistrer est désactivé...

En  testant de mettre une quote ' dans le champ de l'utilisateur on obtient une réponse intéressante
![[Pasted image 20241116220206.png]]

Probalement une SQLi ici ? Après quelques tests manuels non concluant on lance sqlmap.
![[Pasted image 20241116220335.png]]

![[Pasted image 20241116220349.png]]

Excellent !
![[Pasted image 20241116220430.png]]
C'est parti pour cracker le mot de passe de l'utilisateur admin !

![[Pasted image 20241116220527.png]]

On accède ensuite à un dashboard pour administrateur avec une fonctionnalité d'upload... Est-ce qu'on pourrait pas upload un webshell ici ?
![[Pasted image 20241116220720.png]]
Raté ! On va tenter d'analyser la requête dans Burp pour y arriver.
On ajoute une extension .jpg dans le fichier, la signature jpeg en début de fichier, on change le Content-Type et.... ça marche !
![[Pasted image 20241116221201.png]]

![[Pasted image 20241116221318.png]]
Il ne nous reste qu'à afficher le flag.
![[Pasted image 20241116221410.png]]

