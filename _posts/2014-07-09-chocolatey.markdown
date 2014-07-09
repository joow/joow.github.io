---
layout: post
title: Chocolatey, apt-get pour Windows
---

[Chocolatey] est un gestionnaire de logiciels pour Windows. L'un des manques flagrants de Windows par rapport aux distributions Linux/BSD est l'absence d'un gestionnaire de paquets. Vous devez gérer manuellement le téléchargement, l'installation et la mise à jour des différents logiciels installés sur votre ordinateur.
[Chocolatey] propose de gérer tout cela pour vous de manière simple.

## Installer Chocolatey
L'installation est très simple, il suffit d'ouvrir l'invite de commandes en mode "Administrateur" et de saisir la ligne de commandes fournie sur le site :
	
	@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin

## Rechercher un logiciel
Les commandes `choco list <paquet>` ou `choco search <paquet>` ou `clist <paquet>` permettent de rechercher un paquet.  
Les commandes `clist -localonly` ou `clist -lo` permettent quant à elles d'afficher la liste des paquets installés localement.

## Installer un logiciel
Pour installer un logiciel, il suffit d'utiliser la commande `cinst <paquet>`.  
A noter que l'installation d'un logiciel par le biais de [Chocolatey] effectue une installation silencieuse, elle est donc équivalente à une installation manuelle.

## Mettre à jour un logiciel
La commande `cup <paquet>` effectue la mise à jour du logiciel voulu.  
La commande `cup all` permet quant à elle de mettre à jour tous les logiciels installés par le biais de [Chocolatey].

## Désinstaller un logiciel
Enfin pour désinstaller un logiciel, il suffit d'utiliser la commande `cuninst <paquet>`.

## Un peu d'aide
Si vous souhaitez obtenir un peu d'aide sur l'utilisation de [Chocolatey], la commande `chocolatey /?` vous permettra d'afficher une aide dans la console.

[Chocolatey] est un excellent logiciel à recommander pour tous ceux utilisant Windows (par choix ou non !) et à qui un gestionnaire de paquets manque cruellement. Le seul reproche que l'on peut lui faire est l'absence de certains logiciels, mais rien ne vous empêche de contribuer.

[Chocolatey]: http://chocolatey.org/
