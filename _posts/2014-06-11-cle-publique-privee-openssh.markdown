---
layout: post
title: Utiliser l'authentification par clé publique/privée avec OpenSSH
---

Lorsque vous devez vous connecter à un serveur par le biais de `ssh` la méthode la plus courante et la plus simple est d'utiliser le nom d'utilisateur et le mot de passe associé.

Cette méthode présente néanmoins quelques inconvénients :

1.   Au niveau sécurité le serveur doit accepter l'authentification par mot de passe, ce qui le rend vulnérable aux attaques par force brute.
2.   De même le mot de passe est transmis au serveur lors de la phase d'authentification, un attaquant pouvant dès lors l'intercepter.
3.   D'un point de vue pratique cela vous oblige à saisir à chaque connexion votre mot de passe.
4.   De la même manière cela rend difficile l'utilisation de scripts pour effectuer certaines opérations sur un serveur distant .

Afin de pallier à ces différents inconvénients, il est possible d'utiliser une authentification par clé publique/privée. Cette méthode permet de ne plus transmettre de mot de passe et ainsi désactiver la connexion par mot de passe.

La première chose à faire est de créer le couple clé publique/privée protégée par une phrase de passe :

	ssh-keygen -o -a 64

Cette commande va générer deux clés (`~/.ssh/id_dsa` pour la clé privée et `~/.ssh/id_dsa.pub` pour la clé publique) en utilisant le nouveau format (paramètre `-o`) introduit dans la version 6.5 de [OpenSSH]. Ce nouveau format utilise le système [Ed25519] qui a l'avantage d'être rapide et très sécurisé (beaucoup plus que les formats utilisés généralement, [RSA], [ECDSA] ou [DSA]) bien que le nombre de tours utilisés pour la dérivation de la clé (paramètre `-a X`) influe énormément sur le temps nécessaire au chiffrage/déchiffrage de la clé privée (par défaut [OpenSSH] utilise 16 tours, dans l'exemple j'utilise 64 tours, ce qui prend environ une seconde, une valeur de 4096 prenant envirant une minute).

Dans le cas où vous ne voulez pas (ou ne pouvez pas) utiliser ce nouveau format vous pouvez utiliser l'un des deux commandes suivantes ([recommandations] de l'[ANNSI]) :

	ssh-keygen -t rsa -b 2048
	ssh-keygen -t ecdsa -b 256

Une fois les clés générées il faut copier la clé publique sur le serveur ssh sur lequel on souhaite se connecter. Pour cela la commande `ssh-copy-id` est recommandée :

	ssh-copy-id <utilisateur>@<host>

Vous pouvez également préciser la clé publique à copier à l'aide du paramètre `-i <fichier>`. Par défaut la commande va copier le fichier le plus récent correspondant au motif `~/.ssh/id*.pub`.
A partir de là vous pouvez désactiver la connexion par mot de passe (**pas avant car la copie de la clé publique nécessite de se connecter par mot de passe !**) et mettre en place une solution permettant de mettre en cache la saisie de la phrase de passe (par exemple ssh-agent et ssh-add, gnome-keyring ou encore pageant sous Windows).

Vous retrouverez également sur ce [site] plus d'informations quant au nouveau format utilisé par [OpenSSH].

[OpenSSH]: http://www.openssh.com/
[Ed25519]: http://ed25519.cr.yp.to/
[RSA]: http://fr.wikipedia.org/wiki/Chiffrement_RSA
[ECDSA]: http://fr.wikipedia.org/wiki/ECDSA
[DSA]: http://fr.wikipedia.org/wiki/Digital_Signature_Algorithm
[recommandations]: http://www.ssi.gouv.fr/fr/guides-et-bonnes-pratiques/recommandations-et-guides/securite-des-reseaux/recommandations-pour-un-usage-securise-d-open-ssh.html
[ANNSI]: http://www.ssi.gouv.fr/
[site]: http://www.tedunangst.com/flak/post/new-openssh-key-format-and-bcrypt-pbkdf