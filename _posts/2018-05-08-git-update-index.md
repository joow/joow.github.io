---
title: La commande git update-index
layout: post
---

La commande [git update-index](https://git-scm.com/docs/git-update-index) permet de manipuler l'index d'un dépôt Git.  
Elle est particulièrement utile pour supprimer des fichiers de l'index afin qu'ils ne soient plus suivis par Git.

Elle possède deux options intéressantes dont la différence peut sembler subtile au premier abord.

## --skip-worktree

Cette option permet de demander à Git de prétendre que la version du fichier dans l'espace de travail est à jour afin de lire à la place le contenu de la version de l'index.  
C'est généralement cette option que l'on souhaite utiliser, lorsque par exemple on modifie manuellement un fichier de configuration par défaut et que l'on ne souhaite pas suivre ces modifications.  
L'option `--no-skip-worktree` permet de suivre à nouveau les modifications.

## --assume-unchanged

Cette option paraît se rapprocher de la précédente. En effet une fois activée sur un fichier les modifications sur ce dernier ne seront plus suivis par Git.  
En réalité la finalité de cette option est différente. Elle indique à Git que le développeur _promet_ que le fichier ne sera pas modifié et que Git n'a pas besoin de vérifier son contenu. Cela est utile sur les très gros projets où le système de fichiers peut être lent afin d'éviter des opérations de lecture sur le disque.  
Là aussi l'option inverse `--no-assume-unchanged` permet d'indiquer à Git de suivre à nouveau les modifications sur le fichier.

Pour conclure l'option que l'on souhaite généralement utilisée est `git update-index --skip-worktree <chemin>` afin d'ignorer les futures modifications d'un fichier.
