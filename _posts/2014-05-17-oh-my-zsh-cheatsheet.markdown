---
layout: post
title: Oh My Zsh cheat sheet
---

[Oh My Zsh] est un framework permettant de gérer votre configuration [Zsh]. [Zsh] est quant à lui un shell, équivalent à [Bash] mais possédant des fonctionnalités plus avancées.
[Oh My Zsh] possède de nombreuses fonctions, plugins et thèmes vous simplifiant l'utilisation du terminal. Par exemple lorsque vous vous trouvez dans le répertoire d'un dépôt git, l'état et la branche courante sont affichés dans l'invite de commande.
Voici quelques raccourcis de base, certains nécessite l'activation du plugin correspondant dans le fichier `.zshrc` en l'ajoutant à la ligne `plugins=(...)`

## Alias de base
`l` - `ls -lah`  
`la` - `ls -lAh`  
`ll` - `ls -lh'`  
`..` - `cd ..`  
`...` - `cd ...`  
`cd/` - `cd /`  
`d` - affiche les 10 derniers répertoires les plus utilisés  
`N` - permet d'aller au répertoire N (chiffre entre 0 et 9), listé par la commande ci-dessus  
`rd` - `rmdir`  
`take <répertoire>` - crée le répertoire voulu (avec les sous-répertoires si nécessaires) et change le répertoire courant vers ce dernier  
`x`/`extract <archive>` - décompresse l'archive <archive>  
`_`/`please` - `sudo`  
`upgrade_oh_my_zsh` - met à jour [Oh My Zsh]   

## Alias git (plugin git activé par défaut)
`g` - `git`  
`gst` - `git status`  
`gl` - `git pull`  
`gp` - `git push`  
`gd` - `git diff`  
`gc` - `git commit`  
`gco` - `git checkout`  
`gb` - `git branch`  
`gcl` - `git config --list`  
`ga` - `git add`  

## Alias maven (plugin mvn)
`mvnc` - `maven clean`  
`mvnci` - `maven clean install`  

## Alias pacman (plugin archlinux)
`upgrade`/`pacupg` - `sudo pacman -Syu` (met à jour le système)  
`pacreps` - `pacman -Ss` (recherche un paquet)  
`pacrep` - `pacman -Si` (affiche les informations sur un paquet)  
`pacin` - `sudo pacman -S` (installe un paquet)  
`pacins` - `sudo pacman -U` (installe un paquet local)  
`pacre` - `sudo pacman -R` (supprime un paquet)  
`pacrem` - `sudo pacman -Rns` (supprime un paquet et ses dépendances sans sauvegarder les fichiers de configuration)  
`paclsorphans` - `sudo pacman -Qdt` (affiche les paquets orphelins)  
`pacrmorphans` - `sudo pacman -Rs $(pacman -Qtdq)` (supprime les paquets orphelins)  
`pacmir` - `sudo pacman -Syy` (force la mise à jour des dépôts)  

Il y a de nombreux autres plugins, de raccourcis correspondants, de fonctions et de thèmes à découvrir. N'hésitez pas à lire les sources des plugins afin de découvrir toutes les fonctionnalités offertes.

[Oh My Zsh]: http://ohmyz.sh/
[Zsh]: http://www.zsh.org/
[Bash]: http://tiswww.case.edu/php/chet/bash/bashtop.html