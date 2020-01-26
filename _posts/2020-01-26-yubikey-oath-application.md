---
title: L'application OATH sur les clés YubiKey
layout: post
---

Les clés YubiKey de la série 5 (et 4) proposent l'application 
[OATH](https://support.yubico.com/support/solutions/articles/15000014219-yubikey-5-series-technical-manual#OATHa7mzu6) qui 
permet de générer des jetons de type [HTOP](https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_algorithm) ou 
[HTOP](https://support.yubico.com/support/solutions/articles/15000014219-yubikey-5-series-technical-manual#OATHpmqnzs)

Ces jetons sont souvent utilisés comme second facteur d'authentification avec une application mobile, la plus connue 
étant Google Authenticator (les alternatives recommandées sont [Aegis Authenticator](https://getaegis.app/) pour Android
et [Tofu](https://www.tofuauth.com/) pour iOS).

L'avantage d'utiliser une clé YubiKey est que les paramètres permettant de générer un jeton seront stockés sur la clé et
pourront ainsi être utilisés sur n'importe quelle plateforme en utilisant la clé et l'application 
[Yubico Authenticator](https://www.yubico.com/products/services-software/download/yubico-authenticator/), disponible sur 
Windows, macOS, Linux, Android et iOS.  
Il est également possible d'utiliser la commande `ykman oath` en ligne de commande.

Afin de vérifier que l'application est bien activée sur votre clé vous pouvez utilisez la commande suivante :

    ykman oath info

Pour ajouter un nouvel identifant HOTP ou TOTP à la clé il est possible d'utiliser la commande `ykman` :

    ykman oath add --oath-type [TOTP|HOTP] --digits [6|7|8] --algorithm [SHA1|SHA256|SHA512] --issuer TEXT --period INTEGER NAME

En général les options `--oath-type`, `--digits`, `--algorithm` et `--period` ne sont pas nécessaires, les valeurs par 
défaut étant celles généralement utilisées par la plupart des sites proposant ce type de facteur d'authentification.  
Sur la trentaine de codes que j'ai ils utilisent tous sans exception ces paramètres (GitHub, GitLab, Dropbox ou encore 
Twitter).  
Le paramètre `--issuer` permet d'indiquer le service (par exemple `Dropbox` ou `GitHub`) et l'argument `NAME` permet de 
préciser le compte (par exemple `Dropbox:<EMAIL>`).

La commande vous demandera alors le secret qui sera utilisé comme paramètre de génération des jetons (il est également 
possible de préciser le secret en tant que dernier argument de la commande mais cela exposera votre secret dans 
l'historique du shell).

N'oubliez pas d'ajouter l'identifiant sur votre seconde clé YubiKey, voire sur une application mentionnée précédemment.

Il est important de noter que l'un des inconvénients d'utiliser les clés YubiKey est qu'il est impossible d'extraire par
la suite les informations. Si vous perdez l'une de vos clés vous ne pourrez pas par la suite les ajouter à une nouvelle
clé.

L'une des solutions possibles est d'ajouter également ces clés à une application permettant d'exporter les 
identifiants.  
C'est le cas de Aegis Authenticator par exemple, qui permet de faire un export chiffré de sa base de données et qui 
propose un [script](https://raw.githubusercontent.com/beemdevelopment/Aegis/master/scripts/decrypt.py) Python pour
déchiffrer ensuite cette base, ce qui vous offre également la possibilé de sauvegarder vos identifiants OATH OTP (Tofu 
sur iOS ne propose malheureusement pas encore d'export).

Vous pouvez aussi ajouter les informations à votre gestionnaire de mots de passe (1Password le propose, ou bien vous 
pouvez sauvegarder le QR code sous forme d'image).  
Personnellement je ne recommande pas cette solution, cela réduisant la sécurité de vos comptes, vos mots de passe et 
vos seconds facteurs d'authentification étant stockés au même endroit.

Une autre information importante à noter est que les clés YubiKey 5 peuvent seulement stocker 32 identifiants OATH OTP.  
C'est généralement suffisant mais il reste possible d'atteindre cette limite.

Une fois les identifiants ajoutés à votre clé vous pouvez les lister :

    ykman oath list

Et bien entendu vous pouvez générer un code :

    ykman oath code

Cela affichera les codes pour tous les identifiants stockés sur la clé.

Vous pouvez bien sûr restreindre afin de n'afficher le code que pour un service, par exemple Dropbox :

    ykman oath code dropbox

Enfin vous pouvez bien sûr supprimer un identifiant :

    ykman oath delete dropbox

Ou tout supprimer et réinitialiser la configuration de l'application OATH :

    ykman oath reset

Enfin il est également possible d'ajouter un niveau de protection en demandant la saisie d'un mot de passe pour accéder 
aux identifiants :

    ykman oath set-password (--remember)

L'option `--remember` permet de retenir le mot de passe sur la machine afin de ne pas avoir besoin de le saisir tout 
le temps.
