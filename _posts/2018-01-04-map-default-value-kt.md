---
layout: post
title: Créer une map avec une valeur par défaut en Kotlin
---

Kotlin propose une fonction très pratique permettant de créer une map (mutable ou non) renvoyant une valeur prédéfinie si la clé demandée n'existe pas.  
Cette fonction nommée [`withDefault`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/with-default.html) prend comme paramètre une lamba prenant comme paramètre la clé demandée et renvoyant une valeur.

L'exemple suivant renvoie simplement la clé demandée comme valeur si la clé n'existe pas :

{% highlight kotlin %}
val map = emptyMap<String, String>().withDefault { it } // it est la clé demandée

println(map.getValue("key")) // affiche key
// Attention !
println(map.get("key"))      // affiche null
println(map["key"])          // affiche null

val mutableMap = mutableMapOf<String, String>().withDefault { key -> "$key-${Clock.systemDefaultZone().millis()}" }
println(mutableMap.getValue("key")) // affiche key-<MILLIS>


{% endhighlight %}

Il est **important** de noter qu'il faut utiliser la fonction `getValue` et non `get` ou l'accès par index !
