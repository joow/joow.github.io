---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 4
---

Un peu de retard pour ce nouveau billet consacré au cours d'[introduction à la programmation fonctionnelle].  

## Les fonctions d'ordre supérieure

Cette semaine était consacrée uniquement aux [fonctions d'ordre supérieure] (higher order functions). Ces fonctions ont comme particularité de prendre en paramètre une autre fonction. Elles peuvent éventuellement renvoyer une autre fonction comme résultat, même si dans ce cas on parle plutôt de fonctions [curryfiées]. L'un des intérêts principaux de ce type de fonction est de permettre d'abstraire un comportement commun. Par exemple de nombreuses fonctions sur les listes peuvent être définies à partir d'une fonction nommée `foldr` :
{% highlight haskell %}
-- la fonction sum permet d'obtenir la somme des éléments d'une liste :
sum :: Num a => [a] -> a
sum = foldr (+) 0

-- la fonction map permet d'appliquer une "transformation" à une liste :
map :: (a -> b) -> [a] -> [b]
map f = foldr (\x y -> f x : y) []

{% endhighlight %}

La fonction `foldr` prend comme paramètre une fonction et une valeur correspondant à la liste vide et applique récursivement la fonction à la liste en partant de la droite (d'où le **r** de _right_). Une fonction similaire `foldl` existe et applique elle la fonction en partant de la gauche.

## Exercices

Pour vérifier les connaissances acquises la partie "Exercices" et la partie "Lab" proposaient de nombreuses questions sur les fonctions d'ordre supérieur. La difficulté est clairement montée d'un cran et il semblerait que cela continue petit à petit. C'est une très bonne chose, les exercices de cette semaine demandant clairement plus d'investissement que les exercices des précédentes semaines. C'est une très bonne chose et permet de renouveller l'enthousiasme que j'ai pour ce cours.  
La suite dans quelques jours :-)

[introduction à la programmation fonctionnelle]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[fonctions d'ordre supérieure]: http://www.wikiwand.com/fr/Fonction_d%27ordre_sup%C3%A9rieur
[curryfiées]: http://www.wikiwand.com/fr/Curryfication