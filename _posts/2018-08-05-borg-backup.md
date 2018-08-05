---
title: Gérer ses sauvegardes avec Borg
layout: post
---

## Présentation

[Borg](https://borgbackup.readthedocs.io/en/stable/) est un logiciel de sauvegarde gérant la déduplication des données.  

Concrètement cela signifie qu'à chaque sauvegarde il ne sauvegarde en plus que les différences par rapport aux 
sauvegardes précédentes.  
Cela permet d'optimiser l'espace utilisé par les sauvegardes, tout en permettant de conserver 
différentes sauvegardes et ainsi revenir à différents états (même si pour un usage particulier cela représente moins 
d'intérêt il peut toujours être intéressant de revenir à un état donné dans le passé et pas forcément uniquement le 
dernier état).  
Sans rentrer dans les détails techniques Borg découpe les fichiers à sauvegarder et ne sauvegarde que les nouveaux 
morceaux, ce qui lui permet de bénéficier d'une plus grande optimisation de l'espace occupé.

De plus Borg permet de chiffrer les sauvegardes, soit par une phrase de passe, soit par une clé.  
Il est bien sûr possible d'utiliser d'autres outils pour chiffrer la sauvegarde une fois celle-ci faite mais l'intérêt 
immense d'utiliser le chiffrement intégré est que Borg gère toujours la déduplication une fois le chiffrement effectué.

Il peut aussi signer les sauvegardes afin de garantir leur authenticité.

## Création d'un dépôt

Avant de commencer une sauvegarde il est nécessaire de créer un dépôt qui contiendra ensuite les différentes archives 
correspondant aux sauvegardes effectuées :
```bash
borg init --encryption repokey /backups
```

Cette commande permet de créer un dépôt vide utilisant le chiffrement AES-CTR-256 en stockant la clé dans le dépôt.  
Cela signifie qu'une personne récupérant votre dépôt aura la clé de chiffrement, mais **PAS** la phrase de passe.  
Il est aussi possible d'utiliser comme chiffrement `keyfile` afin de ne pas stocker la clé de chiffrement dans le dépôt 
(il faudra dans ce cas la sécuriser et ne surtout jamais la perdre).  
Les options de chiffrement `repokey-blake2` et `keyfile-blake2` permettent d'utiliser l'algorithme de chiffrement 
[BLAKE2](https://en.wikipedia.org/wiki/BLAKE_(hash_function)).

## Création d'une sauvegarde

Pour créer une nouvelle sauvegarde du répertoire de votre utilisateur il suffit d'utiliser la commande suivante 
(adapter l'option `compression` en fonction de votre besoin) :
```bash
borg create --verbose --progress --stats --compression lzma,9 /backups::<NOM_SAUVEGARDE> ~/
```

Il est bien entendu possible d'exclure des fichiers de la sauvegarde.

## Nettoyage des sauvegardes

Borg est tout à fait approprié pour être exécuté tous les jours puisqu'il ne sauvegardera que les "morceaux" de 
fichiers ayant changé.

Il est néanmoins en général utile de supprimer les plus anciennes sauvegardes en utilisant la commande suivante :
```bash
borg prune --verbose --stats --keep-daily=7 --keep-monthly=3 /backups
```

Cette commande va conserver les 7 dernières sauvegardes journalières **et** les trois dernières sauvegardes mensuelles 
en plus (les 7 dernières sauvegardes journalières ne compteront pas dans les trois dernières sauvegardes mensuelles).  
Si vous avez une sauvegarde chaque jour de l'année l'option `--keep-daily=7` conservera les sauvegardes du 25, 26, 27, 
28, 29, 30 et 31 décembre et l'option `--keep-monthly=3` conservera les sauvegardes du 30 novembre, 31 octobre et 30 
septembre (la sauvegarde du 31 décembre ayant déjà été sélectionnée par l'option `--keep-daily`).

N'hésitez pas à consulter la documentation très bien faite de l'outil par le biais de la commande `man` ou du 
[site](https://borgbackup.readthedocs.io/en/stable/usage/general.html).
