---
layout: post
title: Réflexion en Kotlin
---

En Java lorsque l'on souhaite obtenir le type d'une classe on utilise la syntaxe suivante :
{% highlight java %}
String.class
{% endhighlight %}

L'équivalent en Kotlin est la syntaxe littérale :
{% highlight kotlin %}
String::class
{% endhighlight %}

De la même manière pour obtenir la classe d'un objet la même syntaxe peut être utilisée :
{% highlight kotlin %}
val s = "Hello, Kotlin"
println(s::class) // class kotlin.String
{% endhighlight %}

Il est important de noter que nous obtenons une classe `Kotlin` (une [`KClass`]) et non une classe `Java`.  
Lorsque nous interagissons avec du code `Java` il peut être nécessaire d'obtenir la classe `Java` plutôt.  
Pour cela la syntaxe suivante peut être utilisée :
{% highlight kotlin %}
// Classe
println(String::class.java) // class java.lang.String

// Objet
val s = "Hello, Kotlin"
println(s::class.java) // class java.lang.String
println(s.javaClass)   // idem
{% endhighlight %}

Enfin pour obtenir la [`KClass`] à partir de la classe `Java` il suffit d'utiliser la propriété `kotlin` de la classe :
{% highlight kotlin %}
String::class.java.kotlin // class kotlin.String
{% endhighlight %}

[`KClass`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect/-k-class/
