---
layout: post
title: Ajouter un répertoire vide à Git
---

Git ne permet pas de suivre les répertoires vides, puisqu'il n'y a pas de contenu à suivre :smiley:.

Il existe deux techniques couramment utilisées pour ajouter des répertoires vides à Git.

## Fichier .gitkeep

La première technique souvent utilisée est de créer un fichier nommé `.gitkeep` dans le répertoire vide et de l'ajouter. Ainsi le répertoire n'est plus vide et peut être suivi dans Git.  
Il faut par contre penser à supprimer ce fichier lorsque d'autres fichiers ont été ajoutés au répertoire, le fichier `.gitkeep` n'étant alors plus utile.

## Fichier .gitignore

Dans le cas où vous souhaitez ajouter un répertoire vide et qu'il reste comme tel dans Git, la précédente technique n'est plus adaptée. En effet dès que vous allez ajouter un fichier dans le répertoire celui-ci sera "vu" par Git.  
Il arrive parfois que l'on souhaite garder un répertoire vide dans Git, même si des fichiers sont ajoutés localement dans ce répertoire.  
Pour cela à la place du fichier vide `.gitkeep` il suffit de créer un fichier `.gitignore` dans le répertoire avec le contenu suivant :

    *
    !.gitignore

On indique à Git d'ignorer tous les fichiers (`*`) sauf le fichier `.gitignore` (`!.gitignore`). Ainsi même si des fichiers sont ajoutés au répertoire ils seront ignorés.

## Fichier README.md

Une troisième technique, peut-être plus éclairée, est d'ajouter un fichier `README.md` indiquant la raison de ce répertoire non vide. Le fichier joue un peu le même rôle que le fichier vide `.gitkeep` tout en pouvant expliciter l'existence du répertoire "vide" dans Git.

*Note* : il serait possible d'utiliser également le fichier `.gitkeep` à cet égard mais les gens ne penseront pas forcément à consulter un fichier nommé ainsi, qu'ils ne verront pas forcément non plus vu que dans les explorateurs de fichiers il sera masqué par défaut, son nom commençant par le caractère `.`

## Conclusion

Pour permettre le suivi d'un répertoire vide avec Git, il faut le rendre non vide :smile: en ajoutant dans le répertoire :

    * un fichier vide `.gitkeep` qui lui sera suivi par Git
    * un fichier `.gitignore` ignorant tous les fichiers sauf lui-même
    * un fichier  `README.md` suivi par Git décrivant la raison d'être du répertoire