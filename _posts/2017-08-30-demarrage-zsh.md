---
layout: post
title: Mesurer le temps de démarrage de Zsh
---

Si vous utilisez [Zsh] et un framework de configuration (comme [Oh-My-Zsh](http://ohmyz.sh/)) il peut arriver que le démarrage de votre shell soit de plus en plus long suite à l'ajout de plugins.  
Pour mesurer précisément le temps de démarrage de [Zsh], utiliser la commande suivante :

    time zsh -i -c exit

Elle démarrera le shell [Zsh] puis exécutera la commande `exit`, la commande `time` vous donnant le temps nécessaire à l'opération.

[Zsh]: http://www.zsh.org/
