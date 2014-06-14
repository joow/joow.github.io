---
layout: post
title: L'annotation @tailrec en Scala
---

Certains algorithmes peuvent être très élégamment écrits en utilisation la récursivité, c'est-à-dire que la même fonction va s'appeler elle-même jusqu'à arriver à un cas final permettant de résoudre le problème.
Par exemple pour compter le nombre d'occurences d'une lettre dans une chaîne de caractères il est possible de :

- vérifier si la première lettre de la chaîne est égale à la lettre
- appeler la fonction en passant comme chaîne de caractères la chaîne restante (la première lettre est retirée).

Les solutions récursives sont très souvent élégantes mais peuvent parfois nécessiter un trop grand nombre d'appels récursifs, entraînant alors une erreur de mémoire. Heureusement le compilateur Scala est capable d'optimiser les fonctions récursives dites *tail call* ou *terminale*. Ces fonctions ont la propriété de se terminer par un appel récursif à la fonction, cela doit être impérativement le dernier appel, il ne doit pas y avoir d'autres calculs entrant en jeu une fois l'appel effectué. L'avantage de telles fonctions est que le compilateur Scala est capable d'optimiser le code afin d'utiliser des boucles `while` et ainsi ne pas nécessiter de nouvel appel, créant à chaque fois une nouvelle pile d'appel et pouvant entraîner une erreur de mémoire après de trop nombreux appels. Il est possible, et même vivement conseillé d'utiliser l'annotation `@annotation.tailrec` pour indiquer qu'une fonction est récursive terminale, le compilateur Scala produisant une erreur si ce n'est pas le cas :
{% highlight scala %}
import annotation.tailrec

def occurrence(s: String, c: Char): Int = {
	@tailrec
	def occurrence0(s: String, acc: Int): Int = {
		if (s.isEmpty) acc else
		if (s.head == c) occurrence0(s.tail, acc + 1) else occurrence0(s.tail, acc)
	}
	occurrence0(s, 0)
}
{% endhighlight %}

*A noter que le paramètre `c` représentant le caractère recherché n'est pas fourni à la sous-fonction car elle y a accès de manière implicite.*

Par contre la méthode similaire n'est elle pas récursive terminale :
{% highlight scala %}
def occurrence(s: String, c: Char): Int = {
	if (s.isEmpty) 0 else
	if (s.head == c) 1 + occurrence(s.tail, c) else occurrence(s.tail, c)
}
{% endhighlight %}

Si vous essayez de mettre l'annotation `@tailrec`, le compilateur Scala vous dira très clairement que c'est une erreur :

	error: could not optimize @tailrec annotated method occurrence: it contains a recursive call not in tail position
       	if (s.head == c) 1 + occurrence(s.tail, c) else occurrence(s.tail, c)
                                       ^
                                       
En effet dans ce cas lorsque le premier caractère de la chaîne est égale au caractère recherché, on ajoute 1 à l'appel récursif de la fonction, il faut donc appeler la fonction puis ajouter 1 à son résultat. Bien que cette dernière solution soit plus élégante encore, on préfèrera la première solution car elle permettra au compilateur Scala d'optimiser le code généré. Très souvent la transformation d'une fonction récursive à une fonction récursive terminale nécessite la création d'une sous-fonction utilisant un (ou plusieurs) accumulateurs permettant de propager le calcul.