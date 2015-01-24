---
layout: post
title: La notation grand O
---

_Note : ce billet est librement inspiré du [billet suivant]_

La notation grand O est une façon d'indiquer de manière abstraire le temps nécessaire d'exécution à un algorithme et ainsi être en mesure de comparer ce temps à ceux d'autres algorithmes.  

Les grands principes utilisées afin de noter un algorithme sont les suivants :

  1. Comme il est impossible de calculer un temps d'exécution exact qui peut dépendre du matériel, voire du langage utilisé pour l'implémentation cette notation est abstraite et indique la manière dont le temps d'exécution augmente.
  2. Le temps d'exécution est relatif à la taille de l'entrée et permet d'indiquer comment le temps d'exécution augmente, par exemple en fonction de la taille de l'entrée.
  3. Seules les parties d'exécution grandissant relativement à l'entrée sont prises en compte dans la notation, certaines parties de l'algorithme pouvant _grandir_ moins rapidement comparativement à d'autres. C'est pour cela que l'on parle parfois de [comparaison asymptotique].

Voyons maintenant différents exemples de cette notation.

## Temps constant - O(1)

	void printFirstItem(list xs):
		echo xs[1]

Dans cet exemple, quelque soit la taille de la liste en entrée seul le premier élément est affiché. Le temps d'exécution est donc _constant_ et l'algorithme est noté `O(1)`, indiquant que la taille de l'entrée n'influe pas sur le temps d'exécution.

Si l'algorithme contenait d'autres instructions (par exemple affichage du deuxième élément d'une liste) la notation serait toujours `O(1)`, et ce même si c'est en réalité `O(2)` qui serait plus juste, cela n'apporte aucune information (et pourrait compliquer inutilement la comparaison de deux algorithmes dont le temps d'exécution est finalement proche).

## Temps linéraire - O(n)

	void printItems(list xs):
		for x in xs
			echo x

Si la liste en entrée comporte 10 éléments, l'algorithme exécutera 10 fois l'instruction d'affichage, si elle en comporte 1000 il faudra exécuter 1000 fois l'instruction.  
Le temps d'exécution de tels algorithmes est dit linéaire car il augmente de la même manière qu'augmente la taille de l'entrée.

## Temps quadratique - O(n²)

	void printAllPairs(list xs):
		for x in xs
			for y in xs
				print x, y

Cet algorithme nécessite de parcourir deux fois la liste en entrée, pour 10 éléments l'instruction d'affichage sera 10*10 fois, tandis qu'elle sera exécutée 1000\*1000 fois si la liste comporte 1000 éléments.  

À noter que filtrer certains éléments (par exemple afficher uniquement les pairs où chaque élément est différent) ne changera pas le temps d'exécution théorique et l'algorithme sera toujours noté `O(n²)` :

	void printAllPairs(list xs):
		for x in xs
			for y in xs
				if x != y print x, y


Bien entendu le temps d'exécution réel pourrait être moindre (quoique le test étant effectué à chaque itération il est possible qu'il soit plus élevé pour cette seconde version !). 

## Quelques règles

Pour finir ce billet nous allons énumérer rapidement quelques _règles_ applicables à la notation grand O :

  1. Le temps dépend de l'algorithme et non des paramètres en entrée.  
  Par exemple `void printItems(n): for i in 1..n echo "echo"` est noté **O(n)** tout comme l'exemple précédent `printItems`.

  2. Les nombres constants ne sont pas conservés dans la notation :  
  `void echo2Times(xs): for x in xs echo x; for x in xs echo x` pourrait être noté **O(2n)** mais la constante est supprimée et l'algorithme est donc noté simplement **O(n)**

  3. De la même manière le terme le moins significatif est supprimé : **O(n² + n)** deviendra **O(n²)**

  4. Le _"pire"_ cas est généralement conservé :  
  `Boolean contains(xs): for xs in x if x == "this" return True else return False` pourrait être noté **O(1)** dans le meilleur cas (le premier élément de la liste est "this") et dans le pire cas **O(n)** (le dernier élément de la liste est "this"). L'algorithme sera donc noté **O(n)** et non **O(1)**. 

  5. Il est aussi possible d'utiliser la notation grand O pour parler de complexité d'espace (taille allouée à l'exécution de l'algorithme).  
  `Array createArray(n): array = new Array[n]; for i in 1..n array[i] = "hi"` prend une place notée **O(n)**. La taille des paramètres en entrée n'est elle pas prise en compte.

Voilà une présentation rapide de la notation grand O qui permet de rapidement comparer entre eux différents algorithmes.  
Attention tout de même à ne pas se fier aveuglément à cette information. Deux implémentations ayant la même notation peuvent tout de même avoir des temps d'exécution très différents. Par exemple en Java l'utilisation ou non de la classe [StringBuilder] pour concaténer des chaînes de caractères :

{% highlight java %}
public String concat1(Collection<String> strings) {
	String concat;

	for(String s : strings) {
		concat += s;
	}

	return concat;
}

public String concat2(Collection<String> strings) {
	StringBuilder concat = new StringBuilder();

	for (String s : strings) {
		concat.append(s);
	}

	return concat.toString();
}
{% endhighlight %}

Les deux algorithmes sont notés **O(n)** mais `concat2` sera bien plus rapide à l'exécution que `concat1` grâce à l'utilisation d'un [StringBuilder].


[billet suivant]: https://www.interviewcake.com/big-o-notation-time-and-space-complexity
[comparaison asymptotique]: http://www.wikiwand.com/fr/Comparaison_asymptotique
[StringBuilder]: https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html