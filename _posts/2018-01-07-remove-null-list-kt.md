---
layout: post
title: Supprimer les valeurs nulles des collections en Kotlin
---

Lors de la manipulation d'une collection il arrive fréquemment que l'on transforme les valeurs contenues dans celle-ci.  
Si la transformation d'une valeur peut potentiellement renvoyer une valeur nulle et que l'on ne souhaite pas conserver les valeurs nulles il est possible simplement de supprimer les valeurs nulles en utilisant la fonction [`filterNotNull`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not-null.html) :

{% highlight kotlin %}
// la fonction divideBy2 renvoie la valeur divisée par 2 si celle-ci est paire, nulle sinon.
fun divideBy2(x: Int) = if (x % 2 == 0) x / 2 else null

fun main(args: Array<String>) {
    val xs = listOf(0, 1, 2, 3, 4, 5, 6, 7, 8, 9).map { divideBy2(it) }.filterNotNull()
    println(xs) // [0, 1, 2, 3, 4]
}
{% endhighlight %}

Il est même possible de combiner la fonction `map` et `filterNotNull` en utilisant la fonction [`mapNotNull`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-not-null.html) qui ne conservera que les résultats non nuls :

{% highlight kotlin %}
fun main(args: Array<String>) {
    val xs = listOf(0, 1, 2, 3, 4, 5, 6, 7, 8, 9).mapNotNull { divideBy2(it) }
    println(xs) // [0, 1, 2, 3, 4]
}
{% endhighlight %}