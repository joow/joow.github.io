---
layout: post
title: Construire un projet Java avec Gradle
---

Dans ce nouveau post consacré à [Gradle], nous allons voir comment construire un projet Java à l'aide de cet outil de construction.
Nous avons vu [précédemment]({% post_url 2014-04-01-gradle %}) comment débuter avec [Gradle] et en particulier comment utiliser le wrapper (le moyen recommandé d'exécuter [Gradle]).
Passons maintenant à une utilisation pratique de [Gradle] afin de construire un projet Java SE.

## Initialiser le projet

Par défaut si vous créez un fichier `build.gradle` vide et invoquez la commande `gradle tasks` vous pourrez voir la liste des tâches que vous pouvez exécuter :

	------------------------------------------------------------
	All tasks runnable from root project
	------------------------------------------------------------

	Build Setup tasks
	-----------------
	init - Initializes a new Gradle build. [incubating]
	wrapper - Generates Gradle wrapper files. [incubating]

	Help tasks
	----------
	dependencies - Displays all dependencies declared in root project 'JavaSE'.
	dependencyInsight - Displays the insight into a specific dependency in root project 'JavaSE'.
	help - Displays a help message
	projects - Displays the sub-projects of root project 'JavaSE'.
	properties - Displays the properties of root project 'JavaSE'.
	tasks - Displays the tasks runnable from root project 'JavaSE'.

La première chose à faire est de créer une tâche de type `Wrapper` afin de spécifier la version de Gradle à utiliser pour le projet :
{% highlight groovy %}
task wrapper(type: Wrapper) {
	gradleVersion = '2.0-rc-2'
}
{% endhighlight %}

Dans ce cas nous allons utiliser la dernière candidate pour la prochaine version de [Gradle]. En production il est conseillé de rester avec la dernière version stable (1.12).

## Appliquer le plugin Java
Tout comme [Maven], [Gradle] utilise des plugins afin d'offrir des fonctionnalités supplémentaires. Certains plugins font partie intégrantes de la distribution standard alors que de nombreux autres existent. L'une des grosses nouveautés à venir est d'ailleurs la centralisation de tous ces plugins externes afin de pouvoir leur donner une bien meilleure visibilité.

Le plugin `java` permet d'ajouter à votre projet des fonctionnalités liées bien entendu à Java. Pour l'utiliser il suffit d'ajouter la ligne suivante au début de votre fichier de construction :
{% highlight groovy %}
apply plugin: 'java'
{% endhighlight %}

Si à nouveau vous exécutez la commande `./gradlew tasks` vous verrez que de nombreuses tâches ont été ajoutées afin de compiler votre projet, exécuter les tests unitaires, produire une archive, etc ...

Tout comme [Maven] [Gradle] utilise par défaut des conventions. Par exemple le plugin `java` définit deux jeux pour les sources :
1. Les sources du projet situées dans `src/main/java` et les ressources situées dans `src/main/resources`.
2. Les sources de test du projet situées dans `src/test/java` et les ressources de test dans `src/test/resources`.

Il est bien entendu possible de modifier les emplacements par défaut :
{% highlight groovy %}
sourceSets {
	main {
		java {
			srcDir 'src/java'
		}
		resources {
			srcDir 'src/resources'
		}
	}
	test {
		java {
			srcDir 'test/java'
		}
		resources {
			srcDir 'test/resources'
		}
	}
}
{% endhighlight %}

Créer une classe dans le répertoire `src/java` et lancer la tâche `compileJava` : votre projet est désormais compilé.

Créer une classe de test [JUnit] dans le répertoire `test/java` et lancer la tâche `compileTestJava` : une erreur survient.
En effet la classe de test nécessite comme dépendance [JUnit], pour cela il faut d'une part indiquer à [Gradle] où télécharger les dépendances et les dépendances à utiliser :
{% highlight groovy %}
repositories {
	mavenCentral()
}

dependencies {
	testCompile 'junit:junit:4.11'
}
{% endhighlight %}

La méthode `mavenCentral()` permet de définir la configuration vers [Maven Central] et le bloc `dependencies` permet de définir toutes les dépendances. Vous pouvez utiliser :

- `compile` pour les dépendances de compilation des sources Java.  
- `runtime` pour les dépendances nécessaires uniquement à l'exécution.  
- `testCompile` pour les dépendances nécessaires à la compilation des classes de test.  
- `testRuntime` pour les dépendances utilisées pour l'exécution des tests.  

## Spécifier la version de Java à utiliser
Afin de rendre la construction plus pérenne il est conseillé de déifinir la version du JDK à utiliser pour la compilation, [Gradle] utilise par défaut celle du système :
{% highlight groovy %}
sourceCompatibility = '1.7'
{% endhighlight %} 

Cela permet de définir la version à utiliser pour compiler les sources. Par la même occasion la propriété `targetCompatibility` prend par défaut la même valeur, il n'est donc pas nécessaire de la rédéfinir.
A noter que la définition de cette propriété se fait au niveau du projet. En effet le plugin Java introduit plusieurs tâches de type `JavaCompile` : `compileJava` et `compileTestJava`. Ces tâches utilisent cette propriété, il serait également possible de définir seulement ces propriétés pour l'une de ces tâches :
{% highlight groovy %}
compileJava {
	sourceCompatibility = '1.6'
}

task displaySourceCompatibility << {
	println compileJava.sourceCompatibility // égal à 1.6
	println compileTestJava.sourceCompatibility // égal à 1.7
}
{% endhighlight %}

Si dans ce cas cela a peu d'intérêt, il est très intéressant de noter qu'il est très facile de définir différentes tâches de compilation utilisant différentes versions du JDK :
{% highlight groovy %}
apply plugin: 'java'

sourceCompatibility = '1.7' // compilation par défaut en 1.7

task compileJava6(type: JavaCompile) {
	sourceCompatibility = '1.6'
}
{% endhighlight %}

Dans cette exemple invoquer la tâche `compileJava6` permettra d'utiliser la version 1.6 pour compiler les sources.

## Définir un nouveau jeu de sources
Il est également très simple de définir un nouveau jeu de sources (permet de facilement éviter la création d'un nouveau projet spécifique). Par exemple si nous souhaitons créer des tests d'intégration il suffit de définir un nouveau jeu de sources ainsi :
{% highlight groovy %}
sourceSets {
	...

	itTest {
		java {
			srcDir 'it/java'
		}
		resources {
			srcDir 'it/resources'
		}
	}
}
{% endhighlight %}

Vos tests d'intégration ayant certainement besoin des classes de votre application, il faut ajouter la ligne suivante aux dépendances :

	itTestCompile sourceSets.main.output


Nous verrons dans un prochain post d'autres aspects du plugin `java` mais ce premier post devrait vous permettre d'utiliser assez rapidement [Gradle] avec un projet Java.

[Gradle]: http://www.gradle.org
[Maven]: http://maven.apache.org/
[JUnit]: http://junit.org/
[Maven Central]: http://search.maven.org/
