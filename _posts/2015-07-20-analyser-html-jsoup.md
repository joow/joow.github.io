---
layout: post
title: Analyser du HTML en Java avec jsoup
---
Si vous avez besoin d'analyser une page HTML je vous recommande vivement la librairie [jsoup].  
Cette librairie permet d'extraire les informations voulues d'une page HTML en utilisant une syntaxe semblable à [jQuery]. Couplé à l'utilisation des [expressions lambda] apportés par la version 8 de Java il devient très simple d'écrire du code élégant pour manipuler du HTML :
{% highlight java %}
Jsoup.connect("http://cdimage.debian.org/debian-cd/8.1.0/amd64/iso-cd/").get().select("a").stream()
    	.map(e -> e.attr("href"))
        .filter(s -> s.endsWith(".iso"))
        .map(s -> "http://cdimage.debian.org/debian-cd/8.1.0/amd64/iso-cd/" + s)
        .forEach(System.out::println);
{% endhighlight %}

La ligne suivante permet d'obtenir tous les liens de la page (sélecteur [jQuery] _a_), de récupérer uniquement leur attribut _href_, de ne conserver que ceux terminant par _.iso_, d'ajouter l'adresse à cet attribut et enfin de lister les résultats.  
Je vous laisse faire la même chose en utilisant Java 7 :wink: !

[jsoup]: http://jsoup.org/
[jQuery]: http://jquery.com/
[expressions lambda]: https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html