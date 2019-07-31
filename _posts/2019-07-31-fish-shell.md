---
layout: post
title: Fish Shell
---

[fish](https://fishshell.com/) est un shell facile d'utilisation.  
Son nom signifie d'ailleurs **f**riendly **i**nteractive **sh**ell.

Aujourd'hui on retrouve très fréquemment [bash](https://www.gnu.org/software/bash/), qui est installé et utilisé par 
défaut sur la plupart des distributions Linux et macOS (jusqu'à Mojave), et [zsh](http://www.zsh.org/), un peu plus 
puissant et qui sera utilisé par défaut dans macOS à partir de Catalina (https://support.apple.com/en-us/HT208050).  
Les systèmes BSD utilisent en général d'autres shells par défaut, bash étant un projet GNU.

L'avantage de fish par rapport à zsh est que la configuration par défaut est largement satisfaisante, proposant entre 
autre une complétion complète des commandes.  
Pour avoir une configuration équivalent sur zsh il est souvent nécessaire de passer un peu de temps à le configurer ou 
utiliser un système de plugins (comme [Oh My Zsh](https://ohmyz.sh/)), qui peut parfois rendre un peu lent zsh.

Il n'est pas par contre pas conseillé d'utilisé par défaut fish comme shell de connexion.  
En effet il ne cherche pas à être compatible [POSIX](https://fr.wikipedia.org/wiki/POSIX) et certains scripts écrits 
pour bash ne fonctionneront pas avec (alors qu'ils marcheront sur zsh, ce dernier étant compatible). Les scripts 
indiquant bien l'interpréteur à utiliser à l'aide du [shebang](https://fr.wikipedia.org/wiki/Shebang) n'auront pas ce 
problème mais ce n'est malheureusement pas le cas de tous les scripts.

Les autres différences notables par rapport à bash/zsh sont les suivantes.

## Export de variables

La commande `export` n'existe tout simplement pas.

À la place il existe la commande `set` :

```shell
# remplace export KEY=VALUE
set --export KEY VALUE
```

Cette commande permet également d'exporter des variables "universelles", qui seront persistantes :
```shell
set --universal KEY VALUE
```

Enfin dans les scripts cette commande est utilisée pour déclarer et initialiser des variables.

## Session privée

Comme nos navigateurs il est possible de démarrer une session privée afin de ne pas enregistrer les commandes exécutées 
dans l'historique (pour éviter d'enregistrer des mots de passe saisis sur la ligne de commande par exemple) :
```shell
fish --private
```
## Alias

fish ne possède pas la notion d'alias.

Les abbréviations permettent de remplacer avantageusement cette fonctionnalité :

```shell
abbr --add gco git checkout
```

## Fonctions

Afin de créer une nouvelle fonction il suffit d'utiliser la commande `funced` :

```shell
funced rnd
```

fish va ouvrir un éditeur par défaut, à moins que vous ayez déclaré une variable `$VISUAL` ou `$EDITOR`.

L'éditeur par défaut va enregistrer votre fonction lorsque vous appuierez sur la touche `Entrée`, il faut utiliser les 
touches `ALT` et `Entrée` pour insérer une nouvelle ligne dans votre code.

Il est intéressant de noter certaines différences importantes par rapport à bash :

  * `$argv` est utilisée pour les arguments
  * Les indices des tableaux commencent à 1. Par exemple pour accéder au premier argument on utilisera `$argv[1]`
  * une variable déclarée ainsi `set array un deux trois` est considérée comme un tableau
  * `$status` est utilisée comme code de retour (et non `$?` !)

Une fois votre fonction validée, vous pouvez la sauvegarder à l'aide de la commande `funcsave` :
```shell
funcsave rnd
```

Toutes vos fonctions créées ainsi sont disponibles dans `~/.config/fish/functions`.

## open

Comme sur macOS la commande `open` permet d'ouvrir un fichier avec son application par défaut.

_Note_ : la commande macOS correspondante n'est pas remplacée par fish si vous l'utilisez sur macOS :smile:

## Autres

On peut aussi noter que fish propose dans la complétion d'une commande l'historique (pas besoin de rechercher 
spécifiquement dans l'historique une commande).

La complétion propose aussi l'aide des options disponibles :
```shell
benoit@arch ~> ls --all 
--all                                                                           (Inclure les fichiers cachés)
--almost-all                                                      (Inclure les fichiers cachés, sauf . et ..)
--author                                                                (Afficher l’auteur de chaque fichier)
--block-size                                        (Paramétrer la taille de bloc spécifiée pour l’affichage)
--classify                                                               (Append filetype indicator (*/=>@|))
--color                                                                                   (Colorer la sortie)
--color=                                                                                  (Colorer la sortie)
--context                                              (Display security context so it fits on most displays)
--dereference                                                                  (Suivre les liens symboliques)
--dereference-command-line                                                     (Suivre les liens symboliques)
--dereference-command-line-symlink-to-dir                                                                    
--directory                                                  (Lister les noms des dossiers, pas leur contenu)
--dired                                                  (Créer une sortie adaptée au mode « dired » d’Emacs)
--escape                                                           (Octal escapes for non-graphic characters)
--file-type                                                  (Préfixer avec un indicateur de type de fichier)
--format                                                                     (Spécifier le format de listage)
--full-time                                   (Format d’affichage long, avec l’horodatage au format full-iso)
--group-directories-first                                         (Regrouper les dossiers avant les fichiers)
--help                                                                           (Afficher l’aide et quitter)
...
```

Bref, je recommande vivement d'essayer au moins fish, pour son apparente simplicité et sa puissance :smiley:
