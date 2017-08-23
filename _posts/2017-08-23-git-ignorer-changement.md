---
layout: post
title: Ignorer les changements avec Git
---

Il peut arriver que l'on souhaite modifier un fichier existant dans un dépôt Git sans vouloir partager nos modifications, par exemple un fichier de configuration pour l'environnement de développement.  
Dans ce cas Git propose une commande permettant de ne plus suivre les modifications d'un fichier :

    git update-index --assume-unchanged dev.config.json

Si vous voulez à nouveau suivre les modifications du fichier, utilisez la commande suivante : 

    git update-index --no-assume-unchanged dev.config.json

Enfin pour visualiser la liste des fichiers dont les changements ne sont pas suivis vous pouvez utiliser cette commande :

    git ls-files -v | grep -E "^[a-z]"

qui renvoie :
    
    h dev.config.json
    
Cette commande affiche des informations sur la liste des fichiers de l'index et du répertoire de travail, l'argument `-v` indique d'utiliser un caractère minuscule pour le statut des fichiers dont les modifications ne sont pas suivies, d'où la commande `grep` qui permet d'afficher uniquement les fichiers dont le statut est une lettre minuscule.
