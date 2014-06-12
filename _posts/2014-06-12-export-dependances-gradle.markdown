---
layout: post
title: Exporter des dépendances avec Gradle
---

Si jamais vous avez besoin d'exporter les dépendances d'un projet (par exemple pour les inclure à un projet n'utilisant pas d'outil de gestion de dépendances), Gradle permet de très facilement copier les dépendances souhaitées.

Pour cela il suffit de créer une nouvelle configuration qui va être utilisée pour les dépendances à exporter, déclarer les dépendances et enfin créer une tâche de type `Copy` copiant toutes les dépendances vers le répertoire voulu :

{% highlight groovy %}
configurations {
	arquillian
}

repositories {
	mavenCentral()
}

dependencies {
	arquillian 'org.jboss.arquillian:arquillian-bom:1.1.4.Final'
	arquillian 'org.jboss.arquillian.junit:arquillian-junit-container:1.1.4.Final'
	arquillian 'org.jboss.arquillian.container:arquillian-tomcat-embedded-7:1.0.0.CR6'
}

task copyJars(type: Copy) {
	from configurations.arquillian
	into 'libs/arquillian'
}
{% endhighlight %}

En lançant la tâche `copyJars`, toutes les dépendances nécessaires à [Arquillian] seront copiées dans le répertoire `libs\arquillian`.

Vous pouvez également utiliser comme type de tâche `Sync`plutôt que `Copy`. Le type `Sync` étend le type `Copy` tout en supprimant du répertoire de destination les fichiers qui n'ont pas été copiés (*synchronisation*), ce qui est très utile lors du changement de version d'une dépendance.

[Arquillian]: http://arquillian.org
