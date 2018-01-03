---
layout: post
title: La fonction associateBy en Kotlin
---

Il arrive parfois que l'on souhaite indexer une liste de valeurs en fonction d'une clé.  
Kotlin propose pour cela la fonction [`associateBy`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/associate-by.html) qui permet de renvoyer une map contenant la liste des valeurs.  
La clé utilisée pour la map est une lambda prenant comme paramètre la valeur et renvoyant la clé à prendre :

{% highlight kotlin %}
// nous avons une liste d'utilisateurs avec pour chacun leur nom et leur email.
val users = listOf(
        Pair("Alice", "alice@email.com"),
        Pair("Bob", "bob@email.com"),
        Pair("Carol", "carol@email.com"))

// nous souhaitons les indexer par adresse email :
val usersByEmails = users.associateBy { it.second } // la lambda renvoie l'email comme clé

prinln(usersByEmails)
// {alice@email.com=(Alice, alice@email.com), bob@email.com=(Bob, bob@email.com), carol@email.com=(Carol, carol@email.com)}
{% endhighlight %}

Vous vous demandez peut-être ce qu'il se passe si plusieurs valeurs renvoient la même clé (ici si deux utilisateurs ont le même email) ?  
Dans ce cas c'est la dernière valeur indexée qui est ajoutée à la map :

{% highlight kotlin %}
// nous rajoutons Dave qui a la même adresse email qu'Alice.
val users = listOf(
        Pair("Alice", "alice@email.com"),
        Pair("Bob", "bob@email.com"),
        Pair("Carol", "carol@email.com"),
        Pair("Dave", "alice@email.com"))

val usersByEmails = users.associateBy { it.second }

// cette fois c'est Dave et non plus Alice qui va être ajoutée à la map.
prinln(usersByEmails)
// {alice@email.com=(Dave, alice@email.com), ...}
{% endhighlight %}
