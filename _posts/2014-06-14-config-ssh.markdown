---
layout: post
title: Configuration SSH
---

## Définir des alias SSH
Lorsque que l'on est amené à travailler avec plusieurs serveurs distants, [SSH] est un moyen fréquent utilisé pour se connecter à ces différents serveurs. Afin de simplifier les connexions aux différentes machines il peut être tentant de créer des alias (à l'aide de la commande éponyme `alias`).

Pourtant le client ssh fourni avec [OpenSSH] possède un moyen bien plus puissant pour cela à l'aide du fichier de configuration `~/.ssh/config`. En effet ce fichier permet de définir des alias, l'utilisateur à utiliser pour se connecter, le port, des redirections éventuelles de port, etc ... De plus il est également utilisé par la commande `scp` pour la copie de fichiers.

Sa syntaxe générale est la suivante :
{% highlight bash %}
Host <host>
	HostName <hostname>
	User <user>
	Port <port>
	...
{% endhighlight %}

`Host` permet d'indiquer l'alias du serveur. Il est possible d'utiliser le caractère `*` afin de prendre en compte différentes variantes de l'alias (ou le caractère `?` pour accepter un seul caractère variant). Il est également possible de fournir une liste d'alias (en utilisant le caratère `,` comme séparateur) ou encore d'exclure certains alias en les préfixant par le caractère `!`

`HostName` indique le nom réel de l'hôte à utiliser pour l'alias. Dans le cas où vous avez utilisé un motif pour l'alias, il est possible d'utiliser la variable `%n` qui sera remplacée par l'alias utilisée pour exécuter la commande `ssh`.

Enfin `User` et `Port` peuvent être utilisés pour indiquer l'utilisateur et le port à utiliser pour la connexion [SSH].

Par exemple supposions que vous ayez une liste de serveurs dont le nom de machine est `app<N>.domain.com` où N est un nombre quelconque. Pour la connexion vous utilisez systématiquement l'utilisateur `app`. La configuration suivante vous offre la possibilité de saisir la commande `ssh app<N>` pour vous connecter au serveur `<N>` correspondant :
{% highlight bash %}
Host app*
	HostName %n.domain.com
	User app
{% endhighlight %}

## Redirection de ports
En ajoutant la commande `LocalForward <port> <host>:<hostport>` vous pouvez rediriger le port distant voulu sur l'un de vos ports locals. Par exemple si vous souhaitez rediriger le port 8080 distant sur votre port 8080 local il suffit d'ajouter cette ligne à votre fichier de configuration :

	LocalForward 8080 %h:8080

`%h` est une variable qui sera remplacée par le nom de machine. Il est bien entendu possibles de rediriger plusieurs ports à l'aide de plusieurs commandes `LocalForward`.

## Connexion à une machine derrière d'autres machines.
Pour des raisons de sécurité il est parfois nécessaire de passer par une ou plusieurs machines avant de pouvoir se connecter à la machine qui nous intéresse. Dans ce cas il faut se connecter à une première machine, puis de cette machine se connecter à une deuxième machine et ainsi de suite ... Cela peut être très vite un peu pénible, d'autant plus si vous devez copier des fichiers sur la machine finale car dans ce cas vous devez la copier sur chaque machine !

Heureusement là encore le fichier de configuration peut vous simplifier grandement la vie.

La directive `ProxyCommand` permet de spécifier la commande à utiliser pour se connecter à un serveur donné. Imaginions que vous devions passer par trois machines (que nous appelerons `jump1`, `jump2` et `jump3`) avant de pouvoir se connecter aux machines `app<N>`. Voici un fichier de configuration permettant de se connecter directement aux serveurs `app<N>` en une seule commande (`ssh app<N>`) :
{% highlight bash %}
Host jump*
	HostName %n.domain.com
	User jump

Host jump2
	ProxyCommand ssh jump1

Host jump3
	ProxyCommand ssh jump2

Host app*
	HostName %n.domain.com
	User app
	ProxyCommand ssh jump3
	LocalForward 8080 %h:8080
{% endhighlight %}

-  La configuration indique déjà les éléments communs à tous les serveurs de type *jump* (nom de machine et utilisateur pour la connexion [SSH]).
-  Pour les serveurs `jump2` et `jump3` on indique qu'il faut utiliser une connexion [SSH] au serveur `jump1`/`jump2`.
-  Enfin la connexion aux serveurs `app<N>` utilisent une connexion [SSH] au serveur `jump3` et le port 8080 distant est redirigé vers le port local (la redirection fonctionne parfaitement même en passant par plusieurs machines).

Lorsque la commande `ssh app<N>` est utilisée, [SSH] se connecte d'abord à `jump3`, qui lui-même nécessite de se connecter à `jump2`, qui à son tour a besoin de se connecter à `jump1`, la boucle est bouclée !

A noter que même si cela vous permet en une seule commande de passer de votre machine à un serveur par le biais de trois autres machines il faut toujours saisir les mots de passe pour chaque serveur, à moins que vous ayez eu l'excellente idée de mettre en place l'authentification par clé publique/privée et un agent ssh sur chaque machine.

Le fichier de configuration permet de nombreuses autres choses (partage de connexions pour plusieurs sessions, envoi de variables d'environnements à la connexion, ...), je vous invite à consulter la page de manuelle de celui-ci (`man 5 ssh_config`).

[SSH]: http://fr.wikipedia.org/wiki/Ssh
[OpenSSH]: http://fr.wikipedia.org/wiki/OpenSSH