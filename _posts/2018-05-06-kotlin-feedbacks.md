---
title: Retours sur Kotlin
layout: post
---

Ce billet fait suite à une utilisation quotidienne du langage Kotlin pendant les 6 derniers mois.

Il présente plus particulièrement des points spécifiques sur lesquels être attentif, les articles positifs sur le langage étant déjà nombreux.  
Cela ne signifie pas que cette expérience a été négative pour moi, bien au contraire.  
Aujourd'hui j'éviterai si possible d'utiliser Java, Kotlin présente trop d'avantages par rapport à ce dernier et rien dans la feuille de route du langage Java ne m'y encourage. Quand bien même Java "rattraperais" son retard sur Kotlin, certaines fonctionnalités fondamentales resteraient plus intéressantes en Kotlin.

Ceci étant dit, voici une petite liste de mes retours sur l'utilisation de Kotlin en production.

## Éviter l'utilisation des classes `Pair` et `Triple`

Il peut parfois être tentant d'utiliser ces classes plutôt que d'introduire un nouveau type.  
Ne le faîtes pas ! La création d'une classe data prend une ligne et permet de rendre le code bien plus expressif.

## Interopérabilité avec Java

Kotlin a été pensé pour être complètement interopérable avec Java, permettant ainsi d'utiliser toutes les librairies de la plate-forme.  
En particulier cette interopérabilité passe souvent par l'utilisation des annotations adéquates.

Par exemple pour rendre une méthode statique (le concept n'existe pas en Kotlin) utiliser l'annotation `@JvmStatic`, très utile pour les méthodes à exécuter avant et après une classe de tests `JUnit` en particulier.  

De la même manière pour indiquer qu'une méthode est synchronisée il suffit d'utiliser l'annotation `@Synchronized`.

Enfin il est important de noter que si l'on souhaite cibler un champ ou son accesseur il existe les annotations `@field:<Annotation>` et `@get:<Annotation>`, l'annotation préfixée sera générée respectivement sur le champ ou l'accesseur.

## Classes data et héritage

Les classes data ne devraient pas utiliser l'héritage !  
Cela peut être tentant mais ce n'est pas prévu pour et cela rend leur écriture lourde !

## Injection de dépendances dans les tests

Afin de permettre l'injection de dépendances, en particulier dans les tests, l'utilisation de `private lateinit var dependency: DependencyType` est votre ami.  
Cela permet d'indiquer à Kotlin que vous lui promettez que le champ sera initialisé correctement lors de l'exécution avant sa première utilisation et vous évite de déclarer le champ comme nullable.

## Nommage des méthodes de test

En utilisant l'accent grave (`` ` ``) il est possible de nommer les fonctions avec des caractères spéciaux, comme l'espace par exemple.  
Usez de cette possibilité pour nommer vos tests afin de les rendre plus expressifs et plus lisibles :

{% highlight kotlin %}
// au lieu de
fun it_should_convert_a_number_to_a_string()
// ou
fun itShouldConvertANumberToAString()

// utilisez
fun `it should convert a number to a string`()
{% endhighlight %}

## Nullabilité avec Java

En Kotlin les types sont non nullables par défaut. Ce n'est pas le cas en Java.  
Afin d'assurer une interopérabilité plus simple le compilateur relâche cette vérification pour les types Java (appelés également types plateforme).  
Par contre cela signifie que vous pouvez avoir une exception de type `NullPointerException` à l'exécution quand vous manipulez des types Java.

## Alternative aux champs et méthodes statiques

Le concept de champs ou de méthodes statiques n'existe pas en Kotlin. Il est recommandé de passer plutôt par des fonctions de premier niveau (déclarée en dehors d'une classe) ou éventuellement par le biais de l'objet compagnon d'une classe :

{% highlight kotlin %}
class Counter {

  companion object {
    private val version = AtomicInteger()
  }

}
{% endhighlight %}

_Note_ : même marquée `private` la propriété `version` sera accessible dans la classe `Counter`.

## Bytecode Java

IntelliJ IDEA offre une fonctionnalité permettant de voir le code Java équivalent au code Kotlin.  
Cela permet de se rendre compte de la trop grande verbosité de Java !
