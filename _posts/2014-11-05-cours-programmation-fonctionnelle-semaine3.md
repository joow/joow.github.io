---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 3
---

Voici un billet rapide résumant la semaine 3 du cours d'[Introduction à la programmation fonctionnelle] proposé par [edX].  
Vous pouvez vous référer aux autres billets que j'ai déjà consacré à la [semaine 1] et à la [semaine 2].

## Compréhension de liste
Le langage [Haskell] permet de définir des listes en compréhension :
{% highlight haskell %}
-- liste des entiers pairs
[x | x <- [0..], x `mod` 2 == 0]

-- compter la longueur d'une liste
length :: [a] -> Int
length xs = sum [1 | _ <- xs]
{% endhighlight %}

Cette fonctionnalité est très intéressante et permet de définir de nombreuses fonctions opérant sur les listes de manière simple.

## Récursivité
La seconde partie de cette semaine est consacrée à la récursivité.  
Pour les personnes ayant déjà utilisé ce mécanisme cette partie vous semblera familière mais permettra sans doute de rafraîchir un peu vos connaissances. Bien que souvent moins performante, cette manière de définir un algorithme apporte souvent une solution très lisible :
{% highlight haskell %}
isMatch :: String -> String -> Bool
isMatch _ [] = True
isMatch [] _ = False
isMatch (x : xs) (y : ys)
	| x == y = isMatch xs ys
	| otherwise = isMatch xs (y : ys)

-- isMatch "abcd" "bc" -> True
-- isMatch "abcd" "db" -> False
-- isMatch "abcd" "bb" -> False
-- isMatch "abcc" "cc" -> True
{% endhighlight %}

## Exercices
Les exercices m'ont paru assez difficiles, les rendant ainsi intéressants. Encore une fois ils sont grandement inspirés du livre accompagnant le cours.  
La partie sur la récursivité proposait un lab qui offre l'avantage de pouvoir être résolu dans différents langages (Haskell, [Frege], Java ou Scala).  
Je trouve cette idée très bonne, le cours étant une introduction à la programmation fonctionnelle et non au langage [Haskell] cela permet à tout un chacun d'utiliser un autre langage pour résoudre cette partie. Afin de valider le lab un ensemble de questions est posé, rendant la réalisation complètement indépendante du langage utilisé. À priori rien n'empêchait même quelqu'un d'utiliser un autre langage que ceux listés.

## Avis
Je trouve personnellement que cette troisième semaine était plus intéressante que les deux premières. Cela est peut-être dû au fait que j'avais déjà *goûté* à la programmation fonctionnelle et que petit à petit les concepts exposés sont plus avancés et apportent plus de nouveautés.  
Le cours et les exercices permettent de valider les connaissances acquises, le professeur [Erik Meijer] est toujours excellent et [Haskell] est pour moi une excellente découverte, un langage que je trouve très propre et très lisible.  

Je continue donc avec un enthousiasme renouvelé ce cours et vous invite à [y jeter] un coup d'œil.

[Introduction à la programmation fonctionnelle]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[edX]: https://www.edx.org
[semaine 1]: {% post_url 2014-10-19-cours-programmation-fonctionnelle-semaine1 %}
[semaine 2]: {% post_url 2014-10-25-cours-programmation-fonctionnelle-semaine2 %}
[Haskell]: http://www.haskell.org/haskellwiki/Haskell
[Frege]: https://github.com/Frege/frege
[Erik Meijer]: http://www.wikiwand.com/en/Erik_Meijer_%28computer_scientist%29
[y jeter]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126