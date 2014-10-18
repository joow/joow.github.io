---
title: Utiliser un agent SSH avec Cygwin
layout: post
---
Lorsque l'on travaille avec différents serveurs Unix, on utilise fréquemment SSH pour se connecter. La manière recommandée de se connecter à un serveur par le biais de SSH est d'utiliser l'authentification par clé privée. La clé privée étant protégée par une phrase de passe, il faut la saisir à chaque fois que l'on souhaite l'utiliser. Afin d'éviter cela, il est possible d'utiliser un agent gardant en mémoire les clés privées.
Pour le mettre en place sur Windows, il faut ruser un peu afin que l'agent SSH soit partagé par toutes les sessions Cygwin.

Ajouter les lignes suivantes au fichier .bash_profile ou .zshrc si vous utilisez [Zsh] :

	if [[ -f $HOME/.sshagent.conf ]]; then
		. $HOME/.sshagent.conf
	else
		SSH_AGENT_PID=99999
	fi

	if `ps -p ${SSH_AGENT_PID} >/dev/null`; then true;
	else
		ssh-agent >$HOME/.sshagent.conf
		. $HOME/.sshagent.conf
		ssh-add
	fi

Si le fichier `.sshagent.conf` existe il est sourcé (afin que les informations concernant le processus existant de l'agent soient connues). On vérifie ensuite que le processus existe bien et si ce n'est pas le cas on invoque la commande `ssh-agent` et on ajoute les clés existantes à l'aide de `ssh-add`.

Cela permet d'avoir à saisir votre phrase de passe uniquement lors de la première connexion et non à chaque fois que vous démarrez une nouvelle session.

Dans le cas où vous utilisez [Zsh] comme shell et [Oh My Zsh] il est possible d'utiliser tout simplement le plugin `ssh-agent` qui fera tout cela pour vous automatiquement :-).

[Zsh]: http://www.zsh.org/
[Oh My Zsh]: http://ohmyz.sh/
