---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 2
---

Voici le second billet d'une série qui sera dédiée au [cours] d'introduction à la programmation fonctionnelle en [Haskell]. Vous retrouverez [ici] mes impressions sur la première semaine.  
Nous allons voir désormais ce que la deuxième semaine de ce cours nous a réservé.

## Types et classes
La première partie était dédiée aux types et aux classes.  
Les types permettent de regrouper un ensemble fini ou infini de valeurs. Les types de base sont entre autres `Bool` (_True_ ou _False_), `Char` (_'a'_, _'b'_, ...), les types numériques comme `Int`, `Integer`, `Float` et les types plus avancées comme les listes, notées `[a]` où a est un type quelconque, `String` (qui n'est autre qu'une liste de caractères, notée `[Char]`) ou encore les tuples qui sont un couple de 2 à _n_ éléments d'un ou de n types différents, notés (a1, ..., a _n_) où _n_ est supérieur ou égal à 2.  
Les classes permettent de contraindre un type en indiquant que le type supporte certaines opérations.  
Par exemple la classe `Num` correspond aux nombres (types `Int`, `Integer` ou `Float`), la classe `Eq` indique que le type supporte l'opération d'égalité (`==`) ou encore la classe `Ord` qui permet d'indiquer que le type peut être classé par ordre croissant ou décroissant et supporte ainsi les opérations `<`, `<=`, `>=`, `>`, `min` et `max`.  
Notez bien que les classes n'ont rien à voir avec ce que l'on peut retrouver dans les langages orientés objet, [Haskell] n'appartenant pas à cette grande famille.

## Les fonctions
La seconde partie du cours présente dans les grandes lignes la définition de fonctions.  
[Haskell] propose différentes manières de définir des fonctions. Par exemple à la place de l'utilisation d'une expression conditionnelle il est possible d'utiliser des fonctions avec des conditions de garde :
{% highlight haskell %}
-- expression conditionnelle
isEven :: Integral a => a -> Bool
isEven n = if n `mod` 2 == 0 then True else False

-- garde
isEven' :: Integral a => a -> Bool
isEven'
	| n `mod` 2 == 0 = True
	| otherwise == 1 = False
{% endhighlight %}
À noter que l'expression `otherwise` est définie par `Prelude` comme égale à `True`. On notera également que l'expression `if` contient **toujours** la branche `else` en [Haskell].

Il est également possible (et souvent conseillé) d'utiliser le [pattern matching] pour définir des fonctions conditionnelles :
{% highlight haskell %}
isEven'' :: Integral a => a -> Bool
isEven'' 0 = True
isEven'' 1 = False
isEven'' n = isEven'' (n - 2)
{% endhighlight %}

Ici on définit que _0_ est pair, _1_ est impair et que pour tout autre nombre _n_ on appelle récursivement la fonction avec la valeur de _n_ auquel on a retranché 2. On peut également utiliser le métacaractère `_` lorsque l'argument de la fonction n'a pas d'importance dans l'évaluation :
{% highlight haskell %}
and :: Bool -> Bool
True `and` b = b
False `and` _ = False
{% endhighlight %}

La partie suivante décrit les expressions lambdas et comment elles peuvent être utilisées pour définir des fonctions curryfiées. Cette partie est très intéressante, d'autant plus que les expressions lambdas _"envahissent"_ tous les langages _"modernes"_ (même s'il serait plus juste de parlés de _closure_ ou [fermeture] dans de nombreux cas).

Enfin la dernière partie explique les _sections_ sur les opérateurs mathématiques et comment les utiliser afin de définir des fonctions partielles de manière très naturelle :
{% highlight haskell %}
double = (2*)
halve = (/2)
{% endhighlight %}

## Exercices
Cette fois-ci j'ai un peu mieux réussi les exercices :-). Il faut dire que j'avais été échaudé lors de la première semaine et que j'ai donc bien pris le temps de vérifier chaque question à l'aide de l'interpréteur interactif [ghci]. Même si cela peut paraître fastidieux cela permet également d'écrire du code, ce qui reste l'une des façons les plus efficaces d'apprendre un nouveau langage.  
Une seconde série d'exercices nommée _Labs_ offrent de nombreuses autres questions (qu'on ne retrouve pas elles dans le livre Programming in Haskell).

## Avis
Mon avis sur ce cours reste dans l'ensemble mitigé.  
Je suis toujours très agréablement surpris par le langage [Haskell]. Par contre le cours en lui-même ne fait que suivre le livre dont l'achat est très vivement recommandé pour suivre le cours. La pédagogie du professeur [Erik Meijer] apparaît clairement dans certaines explications et il maîtrise très bien son sujet, c'est donc d'autant plus frustrant qu'il ne se _"contente"_ que de suivre le livre. Les exercices sont eux aussi presque lettre pour lettre les mêmes que ceux proposés par le livre, seul le _lab_ offrant des exercices originals. Je m'attendais ici à quelque chose de plus "poussé", impliquant peut-être l'écriture de code comme je l'ai déjà vu fréquemment dans d'autres cours (notamment sur [Coursera]).  

Le contenu me laisse également un peu sur ma faim même si je pense qu'un grand débutant n'ayant jamais abordé la programmation fonctionnelle préfèrera cette session à [celle] qui avait été donnée par [Martin Odersky].  

Je vais pour ma part continuer à suivre le cours car cela me permet d'approfondir et de mieux appréhender certains aspects de la programmation fonctionnelle mais je pense que certaines personnes déjà assez à l'aide avec ce type de langage seront frustrés. À mon avis ils ne sont pas du tout la cible de cours, nommmé très justement **Introduction** à la programmation fonctionnelle.

[cours]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[Haskell]: http://www.haskell.org/haskellwiki/Haskell
[ici]: {% post_url 2014-10-19-cours-programmation-fonctionnelle-semaine1 %}
[pattern matching]: http://www.wikiwand.com/en/Pattern_matching
[fermeture]: http://www.wikiwand.com/fr/Fermeture_%28informatique%29
[ghci]: http://www.haskell.org/haskellwiki/GHC/GHCi
[Erik Meijer]: http://www.wikiwand.com/en/Erik_Meijer_%28computer_scientist%29
[Coursera]: https://www.coursera.org/
[celle]: https://www.coursera.org/course/progfun
[Martin Odersky]: http://www.wikiwand.com/en/Martin_Odersky