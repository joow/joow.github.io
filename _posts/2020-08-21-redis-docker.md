---
title: Partage réseau Docker pour Redis
layout: post
---

La base de données clé valeur [Redis](https://redis.io/) ne propose pas de distribution pour Windows.  
Il faut soit compiler soi-même le code soit récupérer une version compilée.

Une solution plus simple est d'utiliser l'image officielle Docker et de créer un alias :
{% highlight shell %}
alias redis-cli='docker container run -it --rm redis:5.0 redis-cli'
{% endhighlight %}

Cette solution présente l'inconvénient de ne pas permettre facilement l'interaction avec un autre conteneur Redis local.

Pour cela il faut créer un réseau et l'utiliser dans les deux conteneurs afin que le client ait accès au serveur :

{% highlight shell %}
docker network create redis
# Démarre le serveur Redis avec le réseau redis
docker container run --rm --name redis --network redis redis:5.0
# Exécute un conteneur sur le réseau redis, le serveur Redis étant accessible avec le nom d'hôte redis (nom du conteneur)
docker container run --rm --name redis-cli --network redis redis:5.0 redis-cli -h redis ping
# Exécute une session bash
docker container run --rm --name redis-cli --network redis --interactive --tty redis:5.0 bash
{% endhighlight %}


