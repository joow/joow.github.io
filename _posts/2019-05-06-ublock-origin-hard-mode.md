---
layout: post
title: uBlock Origin - Hard mode
---

[uBlock Origin](https://github.com/gorhill/uBlock) est une extension disponible pour les navigateurs Firefox et Chrome qui permet de bloquer certains éléments d'une page Web (images, scripts, cadres...).

Par défaut il intègre plusieurs listes bloquant certains éléments en fonction de règles prédéfinies.

Il peut également être intéressant d'aller plus loin en passant en mode "avancé" afin de régler très précisément les éléments à bloquer d'un site en particulier.

Pour cela il faut aller dans le paramétrage de l'extension et cocher la case **"Activer les fonctionnalités avancées"**.

Une fois ceci fait le cadre d'information de l'extension affiche en plus une matrice permettant de régler finement les règles globales (appliquées à tous les sites) ou locales (appliquées à un site en particulier) à un domaine.  
La colonne de gauche s'applique à tous les sites (règles globales) alors que la colonne de droite s'applique uniquement au site visité (règles locales).

Par défaut bloquez tous les éléments tierce-partie, les scripts de tierce-partie et les cadres de tierce-partie, cela aura pour effet de bloquer toutes les requêtes faites par un site à des sites externes.  
Cela aura également pour effet de _"casser"_ de nombreux sites !  

En effet lors de la première visite à un site, il est probable qu'il ne soit pas fonctionnel si il a besoin de ressources externes.  
C'est parfois le cas pour les images par exemple, hébergées sur un autre site.  
Pour autoriser le chargement d'éléments externes, ouvrez la matrice et essayez de trouver dans les sites bloqués celui ou ceux qu'il est nécessaire d'autoriser.

Par exemple si je visite le site du journal [Le Monde](https://www.lemonde.fr) les images ne sont pas affichées.

En regardant la matrice uBlock je vois que trois sites sont bloqués :

  1. aticdn.net
  2. lemde.fr
  3. ligatus.com

J'autorise le premier (_aticdn.net_), en cliquant sur le bouton gris de la colonne de droite.  
Cela permet de passer les requêtes vers aticdn.net en mode "noop", ainsi les filtres éventuels des listes d'uBlock Origin seront toujours pris en compte.  
Une fois ceci fait vous pouvez recharger la page (en cliquant sur l'icône de rechargement, les deux flèches) et constater si les images sont désormais affichées ou non.  
Ce n'est pas le cas ici.

Il est possible d'annuler les modifications, en cliquant sur la gomme (en haut à gauche de la matrice), puis en débloquant le deuxième site (_lemde.fr_) afin de voir si cela permet d'afficher les images.  
Cette fois c'est bien le cas.  
Nous pouvons donc enregistrer nos modifications en cliquant sur le cadenas (situé à côté de la gomme) afin de conserver les réglages pour notre prochaine visite.

Si le déblocage pour un site en particulier est trop fastidieux il est bien sûr possible d'outrepasser les règles globales pour le site en question en passant les lignes "Tierce-partie" et "Scripts tierce-partie", voire "Cadres de tierce-partie" en mode noop.  
Au pire il est également possible de désactiver temporaire uBlock Origin pour le site en cliquant sur l'interrupteur !

L'auteur de cette extension propose également une autre extension : [uMatrix](https://github.com/gorhill/uMatrix).

Par rapport à uBlock Origin elle est un peu plus puissante mais plus complexe à mettre en place, par défaut tout est refusé, là où uBlock Origin a le fonctionnement inverse.  
Il peut être néanmoins intéressant de l'utiliser pour aller plus loin lorsque l'on se sent à l'aise avec le mode difficile de uBlock Origin !

Vous pouvez consulter les deux liens suivants pour avoir plus d'informations :

  - [Wiki uBlock Origin](https://github.com/gorhill/uBlock/wiki/Blocking-mode:-hard-mode)
  - [The Ultimate Superuser’s Guide to uBlock Origin](https://www.maketecheasier.com/ultimate-ublock-origin-superusers-guide/)
