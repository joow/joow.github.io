---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 6
---

Un très court billet pour faire un retour lui aussi rapide sur la sixième semaine du [cours d'introduction à la programmation fonctionnelle] proposée par [edx].  

La première partie de cette semaine était consacrée à la déclaration de types et de classes. La seconde partie traitait quant à elle de la résolution du _problème "countdown"_, plus connu chez nous comme la partie _["Le compte est bon"]_ du jeu ["Des chiffres et des lettres"].  

## Types et classes
La déclaration de types en Haskell permet d'abstraire un type existant : le type `String` n'est par exemple qu'une abstraction du type `[Char]` (liste de caractères).  
Il est ainsi possible définir une position dans un espace à deux dimensions comme une paire d'entiers. Lui donner un alias permet de simplifier la manipulation d'un tel type :
{% highlight haskell %}
type Pos = (Int, Int)
{% endhighlight %}

Il est également possible de rendre un type paramétrable, par exemple pour déclarer une liste d'associations d'une clé d'un type `k` et d'une valeur d'un type `v`, permettant de représenter une table de hachage :
{% highlight haskell %}
type Assoc k v = [(k, v)]
{% endhighlight %}

Pour finir sur les types il est par contre impossible de déclarer un type qui serait récursif, c'est-à-dire se référant à lui-même :
{% highlight haskell %}
-- Illégal !
type Tree = (Int, [Tree])
{% endhighlight %}

Il est bien sûr possible de créer de nouveaux types en utilisant le mot-clé `data` :
{% highlight haskell %}
data Bool = False | True

data Move = Left | Right | Up | Down
{% endhighlight %}

On peut également paramétrer un _constructeur_ de classe en faisant suivre la nouvelle valeur du ou des paramètres du constructeur (concept que l'on peut retrouver en Scala avec les [case class]) :

{% highlight haskell %}
data Shape = Circle Float | Rect Float Float
{% endhighlight %}

La partie sur les classes n'a pas été développée dans les vidéos. Il est possible d'approfondir cette partie à l'aide du livre (vivement) conseillé lors du cours. Les classes en Haskell se rapproche de celles que l'on retrouve dans la plupart des langages orientés objet, en indiquant les fonctions que doit supporter un type faisant partie d'une classe. La notion de hiérarchie est également supportée pour les classes Haskell.

## Des chiffres et ... des chiffres
Un exemple complet et intéressant de programme en Haskell a ensuite été exposé dans la seconde partie la résolution du problème du [compte est bon]. Assez relevée cette partie permettait néanmoins de montrer comment résoudre un problème donné de manière fonctionnelle.

## Exercices
J'ai été un peu déçu des exercices proposés cette semaine.  
La première partie abordait rapidement les types et revenait grandement sur les [monades] _"expliquées"_ la [semaine dernière]. Si cette partie restait un peu obscure il était vivement conseiller de réviser ! La seconde partie comportait quatre questions sur des fonctions à implémenter dans le cadre du problème résolu.  

Au final cette semaine était un peu plus légère que la précédente, avant d'aborder les deux dernières semaines de ce cours qui malgré certains bons points risque certainement de laisser de nombreuses personnes sur leur faim.

[cours d'introduction à la programmation fonctionnelle]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[edx]: https://www.edx.org
["Le compte est bon"]: http://www.wikiwand.com/fr/Des_chiffres_et_des_lettres#/Le_Compte_est_bon
["Des chiffres et des lettres"]: http://www.wikiwand.com/fr/Des_chiffres_et_des_lettres
[case class]: http://www.scala-lang.org/old/node/107
[compte est bon]: http://www.wikiwand.com/fr/Des_chiffres_et_des_lettres#/Le_Compte_est_bon
[monades]: http://www.wikiwand.com/fr/Monade_%28informatique%29
[semaine dernière]: {% post_url 2014-11-24-introduction-programmation-fonctionnelle-semaine-5 %}
