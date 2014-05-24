---
layout: post
title: Chiffrer vos mots de passe dans Maven
---

Il est fréquemment nécessaire d'utiliser des mots de passe avec [Maven], par exemple pour se connecter à un gestionnaire d'artefacts comme [Artifactory] ou [Nexus].
[Maven] permet ainsi de stocker les mots de passe en les chiffrant afin de ne pas les stocker en clair.
Pour cela il faut commencer par créer un mot de passe maître à l'aide de la ligne de commande suivante :

`mvn --encrypt-master-password <password>` ou `mvn -emp <password>`  

Le mot de passe est celui de votre choix. Depuis la version 3.2.1 de [Maven] il n'est pas obligatoire de le saisir comme argument de la commande, il vous sera alors demandé lors de l'exécution de la commande (recommandé afin de ne pas conserver le mot de passe en clair dans l'historique des commandes).
La sortie de la commande sera la suivante :

`{6PM+hH5SInsH+xnJuzxe99vM0+/y4t6tSnGbCCTHcWA=}` (le mot de passe est entre les accolades).

Copier et coller ce mot de passe dans un fichier nommé `.m2/settings-security.xml` :  
{% highlight xml %}
<settingsSecurity>
	<master>{6PM+hH5SInsH+xnJuzxe99vM0+/y4t6tSnGbCCTHcWA=}</master>
</settingsSecurity>
{% endhighlight %}

Une fois ceci fait vous pouvez chiffrer le mot de passe que vous utilisez pour vous connecter à un serveur à l'aide la commande suivante :

`mvn --encrypt-password <password>` ou `mvn -ep <password>`  

Encore une fois l'argument `<password>` est facultatif depuis la version 3.2.1 de [Maven].
La sortie de la commande sera la suivante :

`{qH2aRFzhzxYHhyGOH+WYPfcMegoYpZlYxq5BSWXQ9eM=}`  

Vous pouvez désormais utiliser ce mot de passe chiffré dans les paramètres du serveur :
{% highlight xml %}
<settings>
	<servers>
		<server>
			<id>id</id>
			<username>username</username>
			<password>{qH2aRFzhzxYHhyGOH+WYPfcMegoYpZlYxq5BSWXQ9eM=}</password>
		</server>
	</servers>
</settings>
{% endhighlight %}

A noter que cette manière de stocker les mots de passe n'apporte qu'un niveau de sécurité faible. En effet [Maven] déchiffre le mot de passe avant l'envoi en serveur. Il est donc très facile en utilisant le mot de passe maître de déchiffrer les mots de passe des différents serveur.
Il est possible d'améliorer un peu cela en stockant le mot de passe maître sur un support externe.
Pour cela modifier le fichier `.m2/settings-security.xml` ainsi :  
{% highlight xml %}
<settingsSecurity>
	<relocation>/Volumes/Usb/secure/settings-security.xml</relocation>
</settingsSecurity>
{% endhighlight %}

[Maven]: http://maven.apache.org
[Artifactory]: http://www.jfrog.com/home/v_artifactory_opensource_overview
[Nexus]: http://www.sonatype.org/nexus/
