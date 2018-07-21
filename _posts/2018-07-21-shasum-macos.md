---
title: Calculer ou vérifier les sommes SHA sur macOS
layout: post
---

Par défaut les commandes `sha1sum`, `sha256sum` ou `sha512sum` ne sont pas disponibles sur macOS.  
Il existe par contre la commande `shasum` qui peut prendre une option indiquant l'algorithme à utiliser (par défaut c'est l'algorithme 1 qui est utilisé, ce qui correspond à SHA-1) :

    shasum [FICHIER]        => calcule le SHA-1
    shasum -a 1 [FICHIER]   => idem
    shasum -a 256 [FICHIER] => calcule le SHA-256
    shasum -a 512           => calcule le SHA-512

Il est également possible avec l'option `-c` de préciser le fichier contenant les sommes à contrôler :

    shasum -a 256 -c [FICHIER] => vérifie les sommes SHA-256 du fichier

Afin de se simplifier la vie il est possible de rajouter trois alias au fichier `~/.bashrc` ou `~/.zshrc` pour rendre les commandes `sha1sum`, `sha256sum` et `sha512sum` disponibles :

    alias sha1sum="shasum -a 1"
    alias sha256sum="shasum -a 256"
    alias sha512sum="shasum -a 512"
