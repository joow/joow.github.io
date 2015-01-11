---
layout: post
title: Utiliser curl pour uploader un fichier
---
[cURL] est un outil très puissant, souvent utilisé pour automatiser certaines tâches nécessitant une interaction avec un site web.  
En effet il permet de télécharger des ressources ou à l'inverse soumettre des formulaires. Il suffit de se dire que tout ce que permet votre navigateur peut être fait à l'aide de [cURL]. Si il n'est pas concevable de l'utiliser comme outil principal pour la navigation son utilité est indéniable dès lors que vous souhaitez automatiser certaines tâches.  
Dans ce court billet nous allons voir comment utiliser [cURL] pour soumettre un fichier à un site web.

## Contexte
Imaginons que votre processus de livraison nécessite l'upload d'un artefact délivré sur une page dédiée. Vous souhaitez automatiser cette partie de votre longue livraison afin de ne pas avoir à faire manuellement cette tâche à la fin de chaque cycle de développement.  
Vous pourriez me rétorquer que la simplification du processus de livraison serait la solution la plus simple, malheureusement vous (et votre équipe de développement) est rarement maître de ce processus.  

## Implémentation
Lorsque vous avez besoin d'uploader un fichier sur un site, vous devez en général être au préalable authentifié. La connexion est dans la plus part des cas persistée à l'aide d'un [cookie], la première étape est donc de vous connecter afin d'obtenir le précieux biscuit :smile:

    curl --silent --show-error --insecure --cookie-jar cookie.txt --data "user=<user>&pass=<password>" <adresse> >/dev/null

La ligne de commande précédente permet de soumettre les informations d'authentification à une adresse donnée et de stocker le [cookie] de connexion dans le fichier `cookie.txt`. Il est important de noter que le nom des champs `user` et `pass` dépend du site sur lequel vous souhaitez vous connecter, il pourrait très bien s'appeler _username_, _password_ ou n'importe quelle autre valeur.  
Afin de trouver ces informations (ainsi que l'adresse de connexion à utiliser) les outils de développement de votre navigateur sont précieux. Ceux de [Firefox] ou [Chrome] peuvent même vous donner directement la requête [cURL] à utiliser (pour [Firefox] aller dans l'onglet "Réseau" des outils de développement et effectuer un clic droit sur l'adresse voulue puis sélectionnez "Copier comme cURL").

Une fois le [cookie] d'authentification obtenu il est possible d'uploader le fichier en soumettant le formulaire nécessaire :

	curl --silent --show-error --insecure --cookie cookie.txt --request POST --form file=@<file> <adresse> >/dev/null

Cette commande permet de soumettre le contenu (symbole `@`) du fichier `file` comme paramètre `file` du formulaire de l'adresse avec une requête [POST] en réutilisant le fichier stockant le [cookie] de connexion. Ici encore le paramètre du formulaire s'appelle `file` mais pourrait très bien s'appeler `content`, `fichier` ou `a` !

Comme vous pouvez le voir [cURL] permet assez facilement d'uploader un fichier par le biais d'un formulaire. De la même manière il pourrait être utilisé pour soumettre un formulaire requérant différentes données (nom, prénom, adresse, ...). Le principal est de savoir que cela est possible, à partir du moment où l'on comprend que [cURL] permet d'automatiser tout ce que vous pouvez faire avec votre navigateur votre imagination est la seule limite (et accessoirement votre patience à lire la très complète page de manuel de l'outil :wink:).  

Pour ceux qui rechercheraient une syntaxe plus simple [HTTPie] semble une très bonne alternative, se décrivant comme un outil similaire à [cURL] pour les humains :relieved:.

[cURL]: http://curl.haxx.se/
[cookie]: http://www.wikiwand.com/en/HTTP_cookie
[Firefox]: https://mozilla.org/firefox
[Chrome]: https://www.google.com/chrome/browser/desktop/index.html
[POST]: http://www.wikiwand.com/en/POST_%28HTTP%29
[HTTPie]: https://github.com/jakubroztocil/httpie