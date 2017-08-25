---
layout: post
title: Chiffrer un fichier avec OpenSSL
---

[OpenSSL](https://www.openssl.org/) est un outil pour les protocoles [TLS](https://fr.wikipedia.org/wiki/Transport_Layer_Security) et SSL. C'est également une boîte à outils très complète permettant un grand nombre d'opération cryptographiques.

Par exemple si vous souhaitez chiffrer un fichier, vous pouvez utiliser la commande suivante :
    
    openssl aes256 (ou aes-256-cbc) -in <dec> -out <enc>

Pour déchiffrer le fichier :
    
    openssl aes256 -d -in <enc> -out <dec>

Vous pouvez consulter la liste des algorithmes disponibles en utilisant cette commande :

    openssl list -cipher-algorithms
