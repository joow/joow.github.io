---
layout: post
title: Installer un MacBook Pro
---
Les [MacBook Pro](http://www.apple.com/fr/macbook-pro/) sont des machines idéales pour travailler, en particulier pour le développement.  
Même pour quelqu'un d'habitué à Linux, le passage au système d'exploitation [Mac OS](http://www.apple.com/fr/osx/) est un vrai plaisir ; 
la base est semblable (Unix), les mêmes outils existent et l'interface graphique est très intuitive.  

Comme pour tout nouveau système il est néanmoins nécessaire de le configurer au préalable et d'installer certains logiciels indispensables.  

Le tout premier logiciel à installer est [brew] qui comme son site l'indique est le gestionnaire de 
paquets manquants pour OS X.  
Avec [brew] vous pouvez installer et mettre à jour la plupart des applications nécessaires pour le 
développement et bien plus.  
Voici une liste de paquets à installer :

  * [autojump](https://github.com/wting/autojump/wiki) : une version améliorée de la commande `cd` mémorisant les répertoires 
    auxquels vous accédez fréquemment
  * [elm](http://elm-lang.org/) : un langage fonctionnel pour le développement Web
  * [git](https://git-scm.com/) : le gestionnaire de versions le plus utilisé
  * [gradle](https://gradle.org/) : un outil de build pour la JVM
  * [heroku](https://toolbelt.heroku.com/standalone) : la suite d'outils pour exploiter le PAAS [Heroku](https://www.heroku.com/)
  * [htop](http://hisham.hm/htop/) : une commande permettant de visualiser les processus en cours d'exécution
  * [httpie](https://httpie.org/) : la commande `curl` (client HTTP) pour les humains
  * [maven](http://braumeister.org/formula/maven) : un autre outil de build pour la JVM
  * [mongodb](https://www.mongodb.org/) : une base de données NoSQL orientée documents
  * [mpv](https://mpv.io/) : un lecteur vidéo très complet, fork de [MPlayer](http://www.mplayerhq.hu/)
  * [p7zip](http://p7zip.sourceforge.net/) : le port de [7-Zip](http://www.7-zip.org/) pour les systèmes POSIX
  * [sbt](http://www.scala-sbt.org/) : un outil de build pour le langage Scala
  * [scala](https://www.scala-lang.org/) : un langage multi-paradigmes, objet et fonctionnel pour la JVM
  * [stow](https://www.gnu.org/software/stow/manual/stow.html) : une commande permettant de gérer l'installation de logiciels 
    par le biais de liens symboliques, très pratique pour gérer ses [dotfiles](https://dotfiles.github.io/)
  * [task](https://taskwarrior.org/) : un gestionnaire de tâches en ligne de commande
  * [wget](https://www.gnu.org/software/wget/) : un client HTTP, plutôt utilisé pour le téléchargement de fichiers
  * [xz](http://tukaani.org/xz/) : un algorithme de compression très efficace
  * [youtube-dl](https://rg3.github.io/youtube-dl/) : un client de téléchargement pour YouTube et de nombreux autres sites de vidéos.
  * [zsh](http://www.zsh.org/) : un shell très puissant
  * [zsh-completions](https://github.com/zsh-users/zsh-completions) : permet d'ajouter la complétion de commandes

Alors que [brew] permet d'installer des applications de type ligne de commande, [brew cask](https://caskroom.github.io/) 
permet de faire la même chose pour les applications graphiques :

  * [alfred] : un lanceur d'applications
  * [cheatsheet](https://www.cheatsheetapp.com/CheatSheet/) : permet d'afficher les raccourcis clavier d'une application
  * [coconutbattery](http://www.coconut-flavour.com/coconutbattery/) : permet de suivre l'évolution de la batterie
  * [docker](https://www.docker.com/) : logiciel de gestion de containers applicatifs
  * [dropbox](https://www.dropbox.com/) : service de stockage en ligne de fichiers 
  * [flux](https://justgetflux.com/) : adapte l'affichage à l'heure de la journée
  * [gimp](http://www.gimp.org/) : un logiciel de dessin
  * [google-chrome](https://www.google.fr/chrome/browser/desktop/index.html) : le navigateur Web de Google
  * [intellij-idea](https://www.jetbrains.com/idea/) : le meilleur IDE pour Java (et d'autres langages)
  * [iterm2](https://iterm2.com/) : un terminal
  * [java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) : le JDK Java
  * [keepassx](https://www.keepassx.org/) : un gestionnaire de mots de passe
  * [keepingyouawake](https://github.com/newmarcel/KeepingYouAwake) : empêche le système de passer en veille
  * [robomongo](https://robomongo.org/) : un client graphique pour [MongoDB]
  * [slack](https://slack.com/) : un logiciel de communication moderne
  * [visual-studio-code](https://code.visualstudio.com/) : un excellent éditeur de code

Certaines applications doivent être installées manuellement, comme :

  * [Oh-My-Zsh](http://ohmyz.sh/) : configuration avancée du shell [Zsh]
  * [nvm](https://github.com/creationix/nvm) : gestionnaires de versions de [Node](https://nodejs.org/)

Enfin il peut être utile de modifier la configuration par défaut pour certains éléments :

  * afficher les extensions des fichiers dans Finder : `Finder > Préférences > Options avancées` et cocher `Afficher toutes les extensions de fichiers`
  * masquer automatiquement le Dock : `Préférences Système > Dock` et sélectionner `Masquer/afficher automatiquement le Dock`
  * désactiver Spotlight et affecter le raccourci clavier à [Alfred] : `Préférences Système > Clavier > Raccourcis > Spotlight` 
    et décocher `Afficher la recherche Spotlight`
  * faire en sorte que les touches F1 à F12 soient utilisables directement : `Préférences Système > Clavier` et cocher
    `Utiliser les touches F1, F2 et ainsi de suite, comme des touches de fonction standard`
  * installer le script d'IntelliJ IDEA afin de pouvoir utiliser la commande `idea` dans le terminal : 
    `Tools > Create Command-line Launcher...`

Voilà, avec ces logiciels et ses réglages vous devriez être armé pour utiliser pleinement votre nouvelle machine :smiley:.

[brew]: http://brew.sh/
[alfred]: https://www.alfredapp.com/