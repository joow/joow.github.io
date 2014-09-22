---
layout: post
title: For vs While
---
Il existe principalement trois types de boucle en Java (ainsi que dans d'autres langages) : ``for``, ``while`` et ``do...while``.
Le dernier type de boucle est rarement utilisé, cet article va se pencher brièvement sur les différences (ou non) entre les boucles ``for`` et les boucles ``while``.

## For ou While ?
Suite à une discussion récente entre collègues, nous avons été amenés à nous interroger sur les différences entre ces deux boucles.  
Même si chacun peut avoir son propre point de vue, il semble communément admis que la principale différence est sémantique :

* ``for`` permet d'indiquer le parcours d'une collection donnée en connaissant exactement le nombre d'itérations. Cela sera fréquemment le parcours complet de la collection.
* ``while`` à l'opposée est plutôt utilisé lorsque la condition d'arrêt de la boucle n'est pas connue à l'avance. Cela peut concerner le parcours éventuellement partiel d'une collection (recherche d'un élément donné par exemple) ou alors l'attente d'en évènement particulier (fin d'un autre fil d'exécution par exemple).

Le cas qui nous intéressait concernait le parcours d'une collection avec la recherche d'un élément voulu. Il semble plus logique et plus juste (sémantiquement parlant) d'utiliser une boucle ``while`` dans ce cas.
Les "opposants" de cette pratique arguent que la boucle ``for`` est dans tous les cas plus intéressantes :

- elle offre au lecteur une seule ligne plus concise (initialisation, condition(s) d'arrêt et instruction(s) à chaque boucle).
- la portée des variables est strictement limitée à la boucle elle-même.

Le premier point est discutable et est affaire de goût. On peut en effet répliquer que le lecteur ne s'attend pas à avoir à "décoder" une boucle ``for``, puisqu'il s'attend à ce qu'elle se contente d'effectuer le parcours complet de la collection utilisée.
Le second point peut être aussi facilement appliqué à une boucle ``while`` :

* en créant une méthode propre à la boucle ``while``.
* si nécessaire en créant un bloc propre à la boucle ``while`` (qui finalement rejoint le point ci-dessus, en étant moins parlant voire même susceptible de dérouter certains développeurs) :
{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2, 3, 4, 5, 6, 7, 8, 9);
Integer six = null;

{
    final Iterator<Integer> it = integers.iterator();
    while (it.hasNext() && six == null) {
        final Integer current = it.next();
        if (current.equals(Integer.valueOf(6))) {
            six = current;
        }
    }
}

System.out.println(six);
{% endhighlight %}

Enfin pour finir il est intéressant de noter que l'extrait de code suivant pour la boucle ``for`` une fois compilé produit exactement le même bytecode que l'extrait précédent (obtenu à l'aide de la commande ``javap -c <Class.class>``), signe que le débat porte uniquement sur des préférences personnelles :
{% highlight java %}
List<Integer> integers = Arrays.asList(0, 1, 2, 3, 4, 5, 6, 7, 8, 9);
Integer six = null;

for (Iterator it = integers.iterator(); it.hasNext() && six == null; ) {
	final Integer current = it.next();
    if (current.equals(Integer.valueOf(6))) {
        six = current;
    }
}
{% endhighlight %}


## Java 8 met tout le monde d'accord

Heureusement pour nous avec l'arrivée de Java 8 ce genre de débat (qui touche au débat philosophique !) n'aura plus lieu d'être, les lambdas permettant enfin au développeur Java d'accéder et de goûter aux joies de la programmation fonctionnelle.
Ainsi la recherche précédente d'un élément donné dans une liste pourra se faire de l'une des manières suivantes :

{% highlight java %}
final List<Integer> integers = Arrays.asList(0, 1, 2, 3, 4, 5, 6, 7, 8, 9);

/* La fonction reduce prend comme arguments le dernier élément
 * renvoyé par cette même méthode (en commençant par le premier argument)
 * et l'élément courant.
 * Ici 0 et 6 (seul élément de la collection filtrée) et renvoie 6.
 */
final Integer six = integers.stream().filter(n -> n == 6).reduce(0, (n, m) -> m); 

// Cette méthode est très certainement préférable.
final Integer seven = integers.stream().filter(n -> n== 7).findFirst().orElse(-1);
{% endhighlight %}

À moins que tout ceci ne déclenche de nouveaux débats !
