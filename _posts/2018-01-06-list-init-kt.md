---
layout: post
title: Initialiser une liste en Kotlin
---

Lors de la création d'une liste, si l'on souhaite l'initialiser directement il est nécessaire de faire l'opération à travers une boucle.  
Non seulement ce peut être assez verbeux mais en plus cela interdit la création d'une liste immuable.

Kotlin propose une fonction [`List`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list.html) prenant en paramètre une taille et une expression lambda permettant d'initialiser la liste.  
Cette expression lambda prend comme paramètre l'index de l'élément de la liste à initialiser.

Si par exemple on souhaite obtenir une liste des 100 premiers nombres, de 1 à 100 il suffit d'appeler cette fonction avec la valeur `100` (la taille de la liste) et une expression lambda incrémentant de 1 l'index :

{% highlight kotlin %}
fun main(args: Array<String>): {
    val xs = List(100) { it + 1 }
    println(xs) // [1, 2, 3, ..., 98, 99, 100]
}
{% endhighlight %}

La fonction `MutableList` permet de créer une liste muable en l'initialisant de la même manière.
