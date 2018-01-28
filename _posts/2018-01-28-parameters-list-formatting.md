---
layout: post
title: Formater une liste de paramètres
---

Le formatage du code est un sujet souvent sensible, chacun ayant ses préférences personnelles.

Pour certains langages la question peut facilement être tranchée à l'aide d'outils "imposant" les règles. Cela évite ainsi les longs débats qui ne sont pas forcément très productifs.

Par exemple `Go` propose en standard la commande [`gofmt`](https://golang.org/cmd/gofmt/) alors que pour `JavaScript` l'outil [`prettier`](https://prettier.io/) semble petit à petit s'imposer au sein de de la communauté.  
Pour d'autres langages il n'existe malheureusement pas encore d'outils similaires.

Certaines règles peuvent néanmoins être justifiées avec des arguments plus objectifs que des préfèrences personnelles.

Prenons par exemple le formatage de la liste des paramètres d'une fonction, que ce soit dans la signature de la fonction ou lors de son appel :

{% highlight kotlin %}
fun myReallyLongMethodName(myReallyLongFirstArgumentName: String, myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongMethodName(myReallyLongFirstArgumentName, myReallyLongSecondArgumentName)
{% endhighlight %}

La manière classique de formater le code précédent afin de ne pas avoir de lignes trop longue est simplement de passer à la ligne lorsque la ligne est trop longue :

{% highlight kotlin %}
fun myReallyLongMethodName(myReallyLongFirstArgumentName: String,
    myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongMethodName(myReallyLongFirstArgumentName,
    myReallyLongSecondArgumentName)
{% endhighlight %}

Le problème de ce formatage est que le second paramètre se retrouve désormais aligné verticalement avec le nom de la fonction déclarée ou appelée. Cela casse un peu la relation avec le paramètre et on perd la notion de liste de paramètres.

Une autre façon de formater le code pour éviter ce problème est d'aligner le second paramètre avec le premier :

{% highlight kotlin %}
fun myReallyLongMethodName(myReallyLongFirstArgumentName: String,
                           myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongMethodName(myReallyLongFirstArgumentName, 
                                                        myReallyLongSecondArgumentName)
{% endhighlight %}

Ce formatage permet de conserver la liste des paramètres groupés visuellement en les alignant verticalement.  
Malheureusement si l'on remanie le code en renommant la fonction, l'alignement risque d'être perdu si l'on ne pense pas à réaligner manuellement tous les appels à la fonction renommé (ce qui est plutôt fastidieux et peu intéressant !) :

{% highlight kotlin %}
fun myReallyLongRefactoredMethodName(myReallyLongFirstArgumentName: String,
                           myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongRefactoredMethodName(myReallyLongFirstArgumentName, 
                                                        myReallyLongSecondArgumentName)
{% endhighlight %}

La dernière façon de formater le code est sans doute la plus correcte :

{% highlight kotlin %}
fun myReallyLongMethodName(
        myReallyLongFirstArgumentName: String,
        myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongMethodName(
        myReallyLongFirstArgumentName,
        myReallyLongSecondArgumentName)
{% endhighlight %}

Ici les paramètres sont alignés verticalement et décalés d'une indentation par rapport au nom de la fonction afin de bien faire apparaître les deux blocs distinctement, le nom de la fonction et la liste de ses paramètres.
On aurait pu également conserver la liste des paramètres sur une seule ligne si sa longueur est acceptable :

{% highlight kotlin %}
fun myReallyLongMethodName(
        myReallyLongFirstArgumentName: String, myReallyLongSecondArgumentName: Int)

val myReallyLongNameForMyValue = myReallyLongMethodName(
        myReallyLongFirstArgumentName, myReallyLongSecondArgumentName)
{% endhighlight %}

Je vous invite à voir l'excellente [conférence de Kevlin Henney](https://youtu.be/ZsHMHukIlJY) sur ce sujet (entre autres).
