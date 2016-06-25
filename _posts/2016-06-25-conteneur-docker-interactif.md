---
layout: post
title: Utiliser un conteneur Docker en mode interactif
---
Lors de la création d'une image Docker il peut être utile d'accéder au conteneur et d'interagir avec par le biais du shell.  
Il faut exécuter le conteneur en exécutant une commande permettant de lancer le shell, comme `sh` ou `bash` 
et utiliser les options `-i` (permet de garder ouverte l'entrée standard même si elle n'est pas attachée) et `-t` (alloue un pseudo TTY).  

Par exemple la commande suivante va permettre d'exécuter le conteneur [ruby](https://hub.docker.com/_/ruby/) en mode interactif :

    docker run --rm -it ruby:2.3-alpine sh

Cela permet ainsi de vérifier l'exécution des commandes à ajouter au fichier ‘Dockerfile‘ 
afin de ne pas avoir à reconstruire le conteneur à chaque essai :smile:.