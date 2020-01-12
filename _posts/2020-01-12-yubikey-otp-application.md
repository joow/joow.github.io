---
title: L'application OTP sur les clés YubiKey
layout: post
---

Les clés YubiKey 5 proposent six applications différentes.

Dans ce billet nous allons voir l'application OTP (ce 
[guide](https://support.yubico.com/support/solutions/articles/15000014219-yubikey-5-series-technical-manual#OTPlr9ax) 
présente en détail les fonctionnalités offertes par les clés YubiKey 5).

L'application OTP fournit deux emplacements, chacun pouvant renfermer un type d'identifiant parmi ceux proposés.

Pour utiliser le premier emplacement il suffit d'appuyer rapidement sur le bouton de la clé alors que pour le second 
emplacement il faut effectuer une pression de 3 secondes sur ce même bouton.

Par défaut le premier emplacement contient un identifiant Yubico OTP, programmé lors de la fabrication.

Pour gérer cette application il suffit d'utiliser la commande `ykman otp`.

Une commande souvent utilisée est la commande `ykman otp swap`.  
Elle permet de basculer l'identifiant Yubico OTP sur le second emplacement et ainsi ne pas le saisir par erreur en 
appuyant par mégarde sur le bouton de la clé.

## Yubico OTP

Un identifiant Yubico OTP permet d'utiliser le [service en ligne](https://developers.yubico.com/OTP/) proposé par 
Yubico et permettant d'utiliser la clé comme second facteur d'authentification.

## Mot de passe statique

Il est également possible de programmer un mot de passe statique dans l'un des deux emplacements de la clé (ou les 
deux).

Par exemple pour générer un mot de passe de 32 caractères et l'ajouter au second emplacement :

    ykman otp static --generate --length 32 2

Notez que par défaut seuls les caractères suivants sont utilisés : `cbdefghijklnrtuv`.  
Cela permet de ne pas avoir de problème avec la disposition du clavier utilisée par le système sur lequel vous branchez 
la clé.

## Défi-réponse HMAC-SHA1

Ce type d'identifiant permet à un logiciel d'envoyer un défi à la clé YubiKey qui devra renvoyée la réponse attendue.

Par exemple le gestionnaire de mots de passe [KeyPassXC](https://keepassxc.org) peut utiliser la réponse envoyée par la 
clé YubiKey pour chiffrer la base de données contenant les mots de passe.

## OATH-HOTP

Le dernier type d'identifiant géré par l'application OTP est un identifiant de type OATH-HOTP 
([Open AuTHentication](https://openauthentication.org/)) qui permet de renvoyer un code numérique de 6 ou 8 chiffres 
comme second facteur d'authentification.

Les clés YubiKey 5 proposent une autre application permettant de gérer un plus grand nombre de codes HOTP (ainsi que 
TOTP).

## Conclusion

L'application OTP n'est pas l'application la plus utilisée sur une clé YubiKey mais la possibilité de programmer un mot 
de passe statique peut être très intéressante, par exemple pour sécuriser le déchiffrement de son disque dur au 
démarrage.  
De la même manière le défi-réponse HMAC-SHA1 peut être utilisé afin de renforcer la sécurité de certains logiciels 
le supportant, comme l'excellent gestionnaire de mots de passe KeyPassXC.

Nous verrons dans les prochains billets les autres applications disponibles sur les clés YubiKey 5.
