---
title: Chiffrer et déchiffrer un fichier avec GnuPG
layout: post
---

[GnuPG](https://www.gnupg.org/) est une implémentation libre du standard [OpenPGP](https://www.openpgp.org/), définissant un format d'échange de messages, signatures et certificats pour des applications cryptographiques.

Il peut servir par exemple à chiffrer et déchiffrer un fichier, en utilisant un chiffrement symétrique ou asymétrique.

Pour chiffre un fichier avec un chiffrement symétrique, utiliser la commande `gpg` avec l'option `--symmetric` ou `-c` :

    gpg --symmetric <FICHIER>

Cela créera un fichier nommé <FICHIER>.gpg.

Il est également possible de changer l'algorithme de chiffrement utilisé avec l'option `--cipher-algo` et en précisant un algorithme proposé par GnuPG, liste qui peut être obtenue en utilisant la commande `gpg --version` :

    gpg -c --cipher-algo AES256 <FICHIER>

Enfin on peut préciser le fichier chiffré à écrire en utilisant l'option `--output` ou `-o` :

    gpg --symmetric --output <FICHIER_CHIFFRE> <FICHIER>

Pour déchiffrer le fichier il suffit d'utiliser la commande `gpg` avec l'option `--decrypt` ou `-d` :

    gpg --decrypt <FICHIER_CHIFFRE>

Par défaut le fichier déchiffré est écrit sur la sortie standard, utiliser l'option `--output` pour indiquer le fichier de sortie :

    gpg -d --output <FICHIER> <FICHIER_CHIFFRE>

_Note : en général GnuPG démarre un agent, ce qui fait que le déchiffrement ne demandera pas forcément le secret._
