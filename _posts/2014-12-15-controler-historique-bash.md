---
layout: post
title: Contrôler l'historique bash
---
Par défaut les shells offrent un historique des commandes saisies, très pratique afin de retrouver une commande déjà exécutée.  
Il arrive pourtant parfois que l'on souhaite ne pas ajouter une commande au shell, par exemple si elle comporte un mot de passe ou une autre information sensible, certains logiciels ne permettant pas la saisie de cette information lors de l'exécution.  
Le shell [bash] permet de contrôler l'ajout ou non d'une commande à l'historique par le biais de la variable d'environnement `HISTCONTROL`. En lui affectant la valeur `ignorespace` toute commande commençant par un espace ne sera pas ajoutée à l'historique :
{% highlight bash %}
export HISTCONTROL=ignorespace
 echo "motdepasse"
{% endhighlight %}

La valeur `ignoredups` permet quant à elle de ne pas enregistrer les doublons dans l'historique.  
Enfin les valeurs `ignoreboth` ou `ignorespace:ignoredups` (ou l'inverse :wink:) combinent les deux.  
Vous pouvez bien entendu ajouter cette option à votre fichier `~/.bashrc` afin de ne pas avoir à utiliser la commande `export` à chaque fois que vous souhaitez ignorer une commande.

Du côté de [zsh] il faudra regarder du côté de la directive `setopt` à mettre directement dans votre fichier `~/.zshrc` :
{% highlight bash %}
setopt histignorespace
setopt histignoredups
{% endhighlight %}

[bash]: https://www.gnu.org/software/bash/
[zsh]: http://www.zsh.org/