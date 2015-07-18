---
layout: post
title: Détecter l'origine d'un bug avec Git
---
Le gestionnaire de versions [Git] propose un excellent moyen de détecter le moment où un bug a été introduit dans votre code source.  

Le principe général est assez simple à comprendre :

- Git parcourt un ensemble limité de révisions (qui correspond à l'ensemble de l'historique par défaut).
- Pour chaque révision vous testez et indiquez à Git si la révision contient le bug ou non.
- Par dichotomie Git sélectionne la prochaine révision à tester.
- Finalement il ne doit en rester qu'une :smiley: (révision !).

## Mise en pratique

On commence par indiquer à Git que l'on souhaite démarrer une session de chasse au bug :

	git bisect start

Il est possible d'ajouter une révision que l'on sait mauvaise (par exemple _HEAD_) et une que l'on sait bonne, cela permet de réduire le nombre d'étapes à exécuter pour débusquer le bug :

	git bisect start HEAD <revision>

Git vous indique alors le nombre de révisions à tester et le nombre d'étapes que cela représente :
	
	Bisecting: 17 revisions left to test after this (roughly 4 steps)

La prochaine révision à tester est automatiquement récupérée et vous n'avez plus qu'à la tester afin d'indiquer à Git si elle contient ou non le bug :

	git bisect good|bad

La prochaine révision à tester est récupérée, et ce jusqu'à ce que vous ayez tester toutes les révisions nécessaires.  
Git vous donnera alors la révision coupable :

	d209088c516c60f1a079ca7433c7542c9dbcb9c1 is the first bad commit

Une fois le coupable identifié vous pouvez arrêter la session afin de remettre votre code source dans l'état précédent :

	git bisect reset

## Astuce

En cas d'erreur lors du marquage d'une révision il est toujours possible après coup d'indiquer à Git si la révision est bonne ou mauvaise :

	git bisect good|bad <revision>

[Git]: http://www.git-scm.com/