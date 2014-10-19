---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 1
---

La programmation fonctionnelle est un sujet très à la mode depuis quelques années déjà et la récente introduction des lambdas dans Java 8 confirme cette tendance de fond.  
Les partisans de ce type de programmation arguent qu'il est le plus à même de relever les défis présents et futurs qu'amènent les nouvelles architectures (augmentation du nombre de cœurs au sein d'un processeur plutôt que de la puissance brute d'un cœur).  
La programmation fonctionnelle présenterait également de nombreux autres avantages, comme par exemple la concision des algorithmes écrits (tout en restant expressif) ou la réduction du nombre d'erreurs (dû entre autres à l'immutabilité comme "état" par défaut).

Le but de ce billet (et des autres qui suivront) n'est pas de discuter ou non de la validité de ces arguments (sachant que je pars avec un esprit ouvert et positif vis-à-vis de ce type de programmation) mais plutôt de décrire le déroulement d'un cours en ligne dédié à ce paradigme.

## Présentation
Le cours **[FP101x - Introduction to Functional Programming]** proposé par la plate-forme [edX] a pour but de présenter les bases de la programmation fonctionnelle et comment les appliquer au monde réel (entendre des projets "métiers").  
Ayant déjà eu l'occasion de suivre deux cours en ligne sur la programmation fonctionnelle (utilisant Scala) et ayant découvert par la même occasion le très bon professeur Erik Meijer (qui dirige ce cours) j'ai été immédiatement intéressé par ce cours. Il est prévu pour durer 6 semaines (de mi-octobre à début décembre) et se base sur le langage [Haskell] (dont Scala semble être fortement inspiré).

Voici mon compte-rendu rapide pour cette première semaine.

## Forme
J'ai tout de suite été un peu déçu par la plate-forme utilisée par [edX] pour délivrer ses cours en ligne. Je l'avais déjà découverte avec un cours en ligne suivi sur [FUN] (la plate-forme ce cours en ligne des universités et des écoles françaises) et je l'avais trouvée bien moins ergonomique que la plate-forme utilisée par [Coursera]. Après tout il s'agit de [Open edX], une plate-forme libre développée par [edX], il est donc normal qu'ils se basent dessus !  
Je lui reproche principalement une difficulté plus grande pour voir notre progression dans le cours (pas d'indicateur de suivi clair) ce qui me semble primordial pour les cours en ligne où l'étudiant est le seul à savoir où il en est, la plate-forme devrait donc l'aider grandement dans cette tâche.  
De même les forums sont moins accessibles et il me semble plus difficile de trouver facilement une information.  
Au niveau des bons points la lecture de vidéos ne pose aucun problème (contrairement à [Coursera] où parfois la vidéo ne se lance pas du tout).  
Pour finir sur la forme (qui reste le point le moins important au final !) les énoncés des questions des exercices sont parfois peu lisibles, toute comme l'avancement dans l'exercice et le résultat final qui n'est pas immédiatement visible.  
Bref la plate-forme a beaucoup de chemin à parcourir pour devenir aussi accessible et ergonomique que celle proposée par [Coursera] mais heureusement la plate-forme étant libre tout le monde peut contribuer.

## Fond
Passons au contenu de cette première semaine.  

La première partie du cours offre un historique bref mais intéressant sur les langages fonctionnels. Il est surtout *amusant* de voir que les langages fonctionnels sont bien antérieurs aux langages objets.  

La seconde partie présente brièvement le langage [Haskell] et l'interpréteur que nous allons utiliser par la suite, [GHCi], l'environnement interactif de GHC (pour Glasgow Haskell Compiler ou Glorious Haskell Compiler, le compilateur [Haskell] le plus utilisé).
Quelques exercices sont proposés pour terminer cette première semaine. Et là deuxième déception, il est très vivement recommandé de se procurer le livre [Programming in Haskell] avant de passer à ces exercices. C'est la première fois que je suis un cours en ligne (j'en avais suivi six avant celui-ci) que l'achat de matériel supplémentaire est demandé. Bien entendu il doit être possible de s'en passer mais tout indique que c'est grandement préférable ! Je trouve personnellement cela dommage, ce n'est effectivement pas un problème pour moi d'acheter ce livre mais pour certaines personnes cela peut l'être (sans parler uniquement de l'aspect financier, peut-on se le procurer partout ? et dans quel délai ? et quelqu'un ne possédant pas de moyen de paiement, comme un adolescent ?). Bref un frein alors que l'intérêt des cours en ligne est de permettre un accès plus facile aux ressources pédagogiques.  
De plus une fois ce livre obtenu je me suis rendu compte que les deux parties de cette première semaine sont très largement inspirées des deux premiers chapitres du livre.  
Pour revenir sur les exercices il faut être très attentif car une seule réponse est possible ! J'ai donc obtenu un score moyen (60%, le minimum pour être certifié) alors que mes erreurs sont plutôt des erreurs d'inattention et non des erreurs dûes à une mauvaise compréhension. J'ai finalement plus appris en faisant les exercices proposés dans le livre !  
Enfin un atelier est proposé, il permet de découvrir différents aspects du langage et l'interpréteur [GHCi]. Cet atelier est également proposé dans plusieurs autres langages ([Groovy], [F#] ou [Frege], un *port* de [Haskell] sur la machine virtuelle Java).

## Conclusion
Mon impression sur cette première semaine est mitigée.  
Les défauts de la plate-forme sont une chose mineure mais je regrette que le cours soit pour le moment uniquement une transposition du livre dont l'achat est très vivement recommandé. Pour le moment j'ai plus appris grâce à ce livre (excellent au passage) que grâce au cours. J'espère que les prochaines semaines seront plus qu'une "lecture" du cours.   Le langage [Haskell] est par contre une très belle découverte. Il apporte des points très intéressants comme une inférence de type très poussée et une concision très agréable.  
Écrire avec ce langage donne l'impression d'écrire du code non typé avec un côté [Python] pour l'utilisation de l'indendation comme délimiteur pour le compilateur. Par exemple la fonction suivante ne comporte aucune information de type mais le compilateur est capable de déterminer que l'argument de la fonction doit être de type numérique (`double 2` renverra bien `4` alors que `double "Hello"` renverra une erreur).
{% highlight haskell %}
double x = x + x
{% endhighlight %}

J'attends avec impatience la semaine suivante qui me permettra de me faire une idée plus définitive de ce cours.


[FP101x - Introduction to Functional Programming]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[edX]: https://www.edx.org/
[Haskell]: http://www.haskell.org/haskellwiki/Haskell
[FUN]: http://www.france-universite-numerique.fr/
[Coursera]: https://www.coursera.org/
[Open edX]: http://code.edx.org/
[Programming in Haskell]: http://www.cs.nott.ac.uk/~gmh/book.html
[Python]: https://www.python.org/
[Groovy]: http://groovy.codehaus.org/
[F#]: http://fsharp.org/
[Frege]: https://github.com/Frege/frege
[GHCi]: http://www.haskell.org/haskellwiki/GHC/GHCi