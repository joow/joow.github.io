---
layout: post
title: À la découverte des clés YubiKey, partie 1
---

[Yubico](https://www.yubico.com/) est une société suédoise proposant des clés physiques de sécurité.

Ces clés de sécurité offrent plusieurs possibilités, la plus connue étant la possibilité de les utiliser comme second 
facteur d'authentification avec de nombreux sites (Google, Facebook, Twitter...).  
L'avantage principal de l'utilisation d'une clé de sécurité par rapport à un code OTP généré par une application (comme 
Google Authenticator qui est la plus connue) est l'impossibilité de fournir le code par erreur suite à un hammeçonnage, 
la clé de sécurité "authentifiant" le serveur demandant le second facteur.

La société propose différents modèles de clés, chaque modèle offrant différents connecteurs et différentes applications.

Pour le grand public ce sont actuellement les [YubiKey 5](https://www.yubico.com/products/) qui sont proposées.  
Suivant le modèle elles offrent une connexion USB-A, USB-C, NFC ou Lightning.  
Malheureusement certaines combinaisons ne sont pas proposées (par exemple USB-C et NFC), il faut donc parfois acheter 
plusieurs modèles pour couvrir tous ses besoins (smartphone avec NFC ou Lightning, ordinateur avec USB-A, un autre 
ordinateur avec USB-C...).

_Note_ : il est de toute façon recommandé de posséder au moins deux clés afin d'avoir une sauvegarde.

Cette série de billets va nous permettre de découvrir les différentes fonctionnalités offertes par les clés.

De mon côté j'utiliserai deux clés :

1. [YubiKey 5 NFC](https://www.yubico.com/product/yubikey-5-nfc) qui offre une connexion USB-A et NFC
2. [YubiKey 5Ci](https://www.yubico.com/product/yubikey-5ci) qui offre une connexion USB-C et une connexion Lightning

## YubiKey Manager

Les clés Yubikey 5 offrent différentes fonctionnalités, appelées également applications, chaque application offrant une 
fonction de sécurité.

Les clés ne sont pas open source, par contre les applications permettant de les configurer le sont.

Afin de commencer à manipuler et configurer les clés il faut installer l'application 
[YubiKey Manager](https://www.yubico.com/products/services-software/download/yubikey-manager/), qui offre une interface 
graphique et un CLI.

Pour le reste du billet j'utiliserai plutôt le CLI, mais toutes les actions sont également faisables avec l'interface
graphique.

## Obtenir des informations sur la clé

Connectez votre clé et utilisez la commande `ykman list` pour afficher la liste des clés connectées.

La commande `ykman info` va vous permettre d'obtenir plus d'informations.
En particulier vous aurez la version du firmware, les interfaces USB et les applications activées (nous reviendrons sur 
ces notions dans les prochains billets).

_Note_ : le firmware ne peut pas être mis à jour.

Si vous avez plusieurs clés connectées simultanément vous pouvez appliquer les commandes à une seule des clés.

Commencez par récupérez les numéros de série des clés :

    ykman list

Puis ajoutez l'option `--device SERIAL` à la commande `ykman` :

    ykman --device <SERIAL> info

Nous commencerons à explorer dans le prochain billet les applications proposées par les clés YubiKey 5.
