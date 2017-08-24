---
layout: post
title: Épingler un paquet Homebrew
---

Il peut arriver que l'on souhaite ignorer les mises à jour d'un paquet [Homebrew](https://brew.sh/).

Malheureusement il arrive aussi assez souvent dans ce cas que l'on ait oublié de bloquer la mise à jour du paquet en question.  
Il faut donc tout d'abord revenir en arrière et réinstaller la version que l'on souhaite conserver.  
Pour cela rechercher la formule en question sur le dépôt [GitHub](https://github.com/Homebrew/homebrew-core) (astuce : utiliser la fonctionnalité de recherche de GitHub pour trouver la formule d'après son nom :wink:). Une fois la formule trouvée parcourez son historique afin de noter le commit de la version à garder. Par exemple dans mon cas je souhaite conserver MongoDB en version 3.2 :

    https://github.com/Homebrew/homebrew-core/blob/6ffdf5e32d799184a6dbc67da8b0d7edfc837f99/Formula/mongodb.rb

Désinstallez la version actuelle puis réinstallez le paquet en utilisant l'url `raw` du commit précédent :

    brew install https://github.com/Homebrew/homebrew-core/blob/6ffdf5e32d799184a6dbc67da8b0d7edfc837f99/Formula/mongodb.rb

Il ne reste plus qu'à épingler la formule afin d'empêcher Homebrew de mettre à jour le paquet la prochaine fois :

    brew pin mongodb # remplacez mongodb par le nom de la formule à épingler

La commande `brew unpin <formule>` permettra par la suite de mettre à nouveau à jour le paquet.

À noter qu'il existe également la commande `brew switch <formule> <version>` qui permet de passer d'une version à une autre. Néanmoins dans ce cas il faut que le format des données reste compatible entre les différentes versions.
